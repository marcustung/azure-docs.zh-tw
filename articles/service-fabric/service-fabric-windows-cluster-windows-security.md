---
title: 使用 Windows 安全性保護在 Windows 上執行的叢集 | Microsoft Docs
description: 了解如何使用 Windows 安全性在 Windows 上執行的獨立叢集上設定節點對節點和用戶端對節點安全性。
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: ce3bf686-ffc4-452f-b15a-3c812aa9e672
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: 394ba3b3b8189bbe96137e920745f7b8cdd1cd95
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666669"
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a>使用 Windows 安全性保護 Windows 上的獨立叢集
為避免有人未經授權存取 Service Fabric 叢集，您必須保護叢集。 當叢集執行生產工作負載時，安全性尤其重要。 本文說明如何在 *ClusterConfig.JSON* 檔案中使用 Windows 安全性，設定節點對節點和用戶端對節點的安全性。  此程序會對應[建立在 Windows 上執行的獨立叢集](service-fabric-cluster-creation-for-windows-server.md)的設定安全性步驟。 如需有關 Service Fabric 如何使用 Windows 安全性的詳細資訊，請參閱[叢集安全性案例](service-fabric-cluster-security.md)。

> [!NOTE]
> 請仔細考慮選擇節點對節點安全性，因為您無法選擇將叢集從某種安全性升級到另一種安全性。 若要更改安全性选项，必须重建整个群集。
>
>

## <a name="configure-windows-security-using-gmsa"></a>使用 gMSA 設定 Windows 安全性  
此範例*Microsoft.azure.servicefabric.windowsserver*組態檔，以下載[可供。\<版本 >.zip](https://go.microsoft.com/fwlink/?LinkId=730690)獨立叢集封裝包含供設定 Windows 安全性使用的範本[群組受控服務帳戶 (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):  

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {  
        "ClustergMSAIdentity": "[gMSA Identity]",
        "ClusterSPN": "[Registered SPN for the gMSA account]",
        "ClientIdentities": [
            {
                "Identity": "domain\\username",
                "IsAdmin": true
            }
        ]
    }
}
```

| **組態設定** | **說明** |
| --- | --- |
| ClusterCredentialType |設定為 *Windows* 可為節點對節點通訊啟用 Windows 安全性。  | 
| ServerCredentialType |設定為 [Windows] 可為用戶端對節點通訊啟用 Windows 安全性。 |
| WindowsIdentities |包含叢集和用戶端身分識別。 |
| ClustergMSAIdentity |設定節點對節點安全性。 群組受控服務帳戶。 |
| ClusterSPN |gMSA 帳戶的已註冊 SPN|
| ClientIdentities |設定用戶端對節點安全性。 用戶端使用者帳戶的陣列。 |
| 身分識別 |新增網域使用者 (domain\username) 以做為用戶端身分識別。 |
| IsAdmin |設定為 true 可指定網域使用者具有系統管理員用戶端存取權，設定為 false 則具有使用者用戶端存取權。 |

> [!NOTE]
> ClustergMSAIdentity 值不能包含網域名稱，並只能是群組受管理的服務帳戶名稱。 I.E. "mysfgmsa"正確無誤，和"mydomain / / mysfgmsa"或"mysfgmsa@mydomain"是無效; 做為網域所默許的主機電腦。

需要在 gMSA 下執行 Service Fabric 時，可透過 **ClustergMSAIdentity** 來設定[節點對節點安全性](service-fabric-cluster-security.md#node-to-node-security)。 為了建置節點之間的信任關係，它們必須注意彼此。 这可以通过两种不同的方法实现：指定包含群集中所有节点的组托管服务帐户，或者指定包含群集中所有节点的域计算机组。 強烈建議使用 [群組受控服務帳戶 (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) 方法，特別適合於較大型的叢集 (超過 10 個節點) 或可能會擴大或縮小的叢集。  
此方法不需要建立叢集系統管理員已獲得存取權限的網域群組，即可加入和移除成員。 進行自動密碼管理時，這些帳戶也很有用。 如需詳細資訊，請參閱 [開始使用群組受控服務帳戶](https://technet.microsoft.com/library/jj128431.aspx)。  
 
[ClientIdentities](service-fabric-cluster-security.md#client-to-node-security) 可設定 **ClientIdentities**的設定安全性步驟。 若要建立用戶端與叢集之間的信任，您必須設定叢集才能知道用戶端可以信任的身分識別。 這有兩種不同的方式可以完成：指定可以连接的域组用户，或者指定可以连接的域节点用户。 Service Fabric 針對連線到 Service Fabric 叢集的用戶端，支援兩種不同的存取控制類型：系統管理員和使用者。 存取控制可讓叢集系統管理員針對不同的使用者群組限制特定類型的叢集作業的存取權，讓叢集更加安全。  系統管理員可以完整存取管理功能 (包括讀取/寫入功能)。 使用者預設只具有管理功能的讀取存取權 (例如查詢功能)，以及解析應用程式和服務的能力。 如需存取控制的詳細資訊，請參閱[角色型存取控制 (適用於 Service Fabric 用戶端)](service-fabric-cluster-security-roles.md)。  
 
下列範例 **security** 區段使用 gMSA 來設定 Windows 安全性，並指定 *ServiceFabric.clusterA.contoso.com* gMSA 中的電腦隸屬於叢集，且 *CONTOSO\usera* 具有系統管理員用戶端存取權︰  
  
```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClustergMSAIdentity" : "ServiceFabric.clusterA.contoso.com",
        "ClusterSPN" : "http/servicefabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
}
```
  
## <a name="configure-windows-security-using-a-machine-group"></a>使用電腦群組設定 Windows 安全性  
此模型已被淘汰。 建議使用 gMSA，如上所述。 此範例*ClusterConfig.Windows.MultiMachine.JSON*組態檔，以下載[可供。\<版本 >.zip](https://go.microsoft.com/fwlink/?LinkId=730690)獨立叢集封裝包含的範本設定 Windows 安全性。  Windows 安全性於 **Properties** 區段中設定︰ 

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "[domain\machinegroup]",
        "ClientIdentities": [{
            "Identity": "[domain\username]",
            "IsAdmin": true
        }]
    }
}
```

| **組態設定** | **說明** |
| --- | --- |
| ClusterCredentialType |設定為 *Windows* 可為節點對節點通訊啟用 Windows 安全性。  |
| ServerCredentialType |設定為 [Windows] 可為用戶端對節點通訊啟用 Windows 安全性。 |
| WindowsIdentities |包含叢集和用戶端身分識別。 |
| ClusterIdentity |使用電腦群組名稱 (domain\machinegroup) 來設定節點對節點安全性。 |
| ClientIdentities |設定用戶端對節點安全性。 用戶端使用者帳戶的陣列。 |  
| 身分識別 |新增網域使用者 (domain\username) 以做為用戶端身分識別。 |  
| IsAdmin |設定為 true 可指定網域使用者具有系統管理員用戶端存取權，設定為 false 則具有使用者用戶端存取權。 |  

如果希望在 Active Directory 域内使用计算机组，可通过设置“ClusterIdentity”来配置[节点到节点安全性](service-fabric-cluster-security.md#node-to-node-security)。 如需詳細資訊，請參閱[在 Active Directory 中建立電腦群組 (Create a Machine Group in Active Directory)](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx)。

使用 ClientIdentities 配置[客户端到节点安全性](service-fabric-cluster-security.md#client-to-node-security)。 若要在客户端与群集之间建立信任关系，必须对群集进行配置，使群集知道可以信任哪些客户端标识。 您可以透過兩種不同方式建立信任︰

- 指定可以連線的網域群組使用者。
- 指定可以連線的網域節點使用者。

Service Fabric 针对连接到 Service Fabric 群集的客户端支持两种不同的访问控制类型：管理员和用户。 存取控制可讓叢集系統管理員針對不同的使用者群組限制特定類型的叢集作業的存取權，讓叢集更加安全。  管理员拥有对管理功能（包括读取/写入功能）的完全访问权限。 默认情况下，用户只有管理功能的读取访问权限（例如查询功能），以及解析应用程序和服务的能力。  

下列範例 **security** 區段可設定 Windows 安全性，指定 ServiceFabric/clusterA.contoso.com 中的電腦隸屬於叢集，並指定 CONTOSO\usera 具有系統管理員用戶端存取權︰

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
},
```

> [!NOTE]
> 不应在域控制器上部署 Service Fabric。 請確定在使用電腦群組或群組受控服務帳戶 (gMSA) 時，ClusterConfig.json 不會包含網域控制站的 IP 位址。
>
>

## <a name="next-steps"></a>後續步驟
在 ClusterConfig.JSON 文件中配置 Windows 安全性之后，请继续执行[创建 Windows 上运行的独立群集](service-fabric-cluster-creation-for-windows-server.md)中的群集创建过程。

如需有關節點對節點安全性、用戶端對節點安全性和角色型存取控制的詳細資訊，請參閱[叢集安全性案例](service-fabric-cluster-security.md)。

如需使用 PowerShell 或 FabricClient 來連線的範例，請參閱[連線到安全的叢集](service-fabric-connect-to-secure-cluster.md) 。
