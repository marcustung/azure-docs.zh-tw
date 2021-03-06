---
title: 檢視與修改主機名稱 | Microsoft Docs
description: 如何檢視和變更 Azure 虛擬機器的主機名稱、Web 和背景工作角色以進行名稱解析
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
ms.assetid: c668cd8e-4e43-4d05-acc3-db64fa78d828
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 3fdb0f566789382a1606b19e4fac179f9ecf40cd
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "57887454"
---
# <a name="viewing-and-modifying-hostnames"></a>檢視與修改主機名稱
若要允许通过主机名引用角色实例，必须在服务配置文件中为每个角色设置主机名的值。 您可以將需要的主機名稱新增到 **Role** 項目的 **vmName** 屬性。 **vmName** 屬性的值會做為各個角色執行個體之主機名稱的基底。 例如，如果 **vmName** 是 *webrole*，并且该角色有三个实例，则这些实例的主机名将为 *webrole0*、*webrole1* 和 *webrole2*。 您不需要指定組態檔中虛擬機器的主機名稱，因為虛擬機器的主機名稱會根據虛擬機器名稱填入。 如需設定 Microsoft Azure 服務的詳細資訊，請參閱 [Azure 服務組態結構描述 (.cscfg 檔)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>檢視主機名稱
您可以使用下列任何工具，在雲端服務中檢視虛擬機器和角色執行個體的主機名稱。

### <a name="service-configuration-file"></a>服務組態檔
您可以從 Azure 入口網站中服務的 [設定] 刀鋒視窗，下載已部署服務的服務組態檔。 然後您可以尋找 **Role name** 項目的**vmName** 屬性，查看主機名稱。 請記住，這個主機名稱是做為各個角色執行個體之主機名稱的基底。 例如，如果 **vmName** 是 webrole 且有三個該角色的執行個體，執行個體的主機名稱將是 webrole0、webrole1 以及 webrole2。

### <a name="remote-desktop"></a>遠端桌面
啟用您虛擬機器或角色執行個體的遠端桌面 (Windows)、Windows PowerShell 遠端執行功能 (Windows) 或 SSH (Linux 和 Windows) 連線之後，您可以利用多種方式檢視使用中遠端桌面連線的主機名稱：

* 在命令提示字元或 SSH 終端機中輸入主機名稱。
* 在命令提示字元中輸入 ipconfig /all (僅限 Windows)。
* 在系統設定中檢視電腦名稱 (僅限 Windows)。

### <a name="azure-service-management-rest-api"></a>Azure 服務管理 REST API
從 REST 用戶端，請遵循下列指示：

1. 確定您有連線到 Azure 入口網站的用戶端憑證 若要获取客户端证书，请执行[如何：下载和导入发布设置和订阅信息](https://msdn.microsoft.com/library/dn385850.aspx)中提供的步骤。 
2. 設定名稱為 x-ms-version，值為 2013-11-01 的標頭項目。
3. 傳送要求以下列格式： https:\//management.core.windows.net/\<subscrition-i d\>/services/hostedservices/\<服務名稱\>？ 內嵌詳細資料 = true
4. 尋找各個 **RoleInstance** 項目的 **HostName** 項目。

> [!WARNING]
> 您也可以透過檢查 **InternalDnsSuffix** 項目、從遠端桌面工作階段中的命令提示字元執行 ipconfig /all (Windows)，或從 SSH 終端機執行 cat /etc/resolv.conf (Linux)，檢視 REST 呼叫回應中您雲端服務的內部網域尾碼。
> 
> 

## <a name="modifying-a-hostname"></a>修改主機名稱
您可以透過上傳已修改的服務組態檔，或是從遠端桌面工作階段重新命名電腦，來修改任何虛擬機器或角色執行個體的主機名稱。

## <a name="next-steps"></a>後續步驟
[名稱解析 (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Azure 服務組態結構描述 (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Azure 虛擬網路組態結構描述](https://go.microsoft.com/fwlink/?LinkId=248093)

[使用網路組態檔指定 DNS 設定](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)

