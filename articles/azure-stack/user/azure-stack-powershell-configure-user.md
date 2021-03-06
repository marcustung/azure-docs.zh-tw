---
title: 以使用者的身分使用 PowerShell 連線到 Azure Stack | Microsoft Docs
description: 連線到使用者 Azure Stack 執行個體的步驟。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2019
ms.author: mabrigg
ms.reviewer: thoroet
ms.lastreviewed: 01/24/2019
ms.openlocfilehash: b8f2e3ebfa7187b6695fbd291c7baf0a9ba3b712
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2019
ms.locfileid: "58485776"
---
# <a name="connect-to-azure-stack-with-powershell-as-a-user"></a>以使用者的身分使用 PowerShell 連線到 Azure Stack

*適用於：Azure Stack 整合式系統和 Azure Stack 開發套件*

本文提供連線到您 Azure Stack 執行個體的步驟。 您必須連線，才能使用 PowerShell 來管理 Azure Stack 資源。 例如，您可以使用 PowerShell 來訂閱供應項目、建立虛擬機器及部署 Azure Resource Manager 範本。 以執行 PowerShell Cmdlet。

進行設定：
  - 確定您具有必要項目。
  - 與 Azure Active Directory (Azure AD) 或「Active Directory 同盟服務」(AD FS) 連線。 
  - 註冊資源提供者。
  - 測試連線能力。

## <a name="prerequisites"></a>必要條件

從[開發套件](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop)，或從 Windows 型外部用戶端 (如果您[透過 VPN 連線](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn))，設定下列先決條件：

* 安裝 [Azure Stack 相容的 Azure PowerShell 模組](azure-stack-powershell-install.md)。
* 下載[處理 Azure Stack 所需的工具](azure-stack-powershell-download.md)。

務必使用 Azure Stack 組態中的值取代下列指令碼變數：

- **Azure AD 租用戶名稱**  
  用來管理 Azure Stack 的 Azure AD 租用戶名稱，例如 yourdirectory.onmicrosoft.com。
- **Azure Resource Manager 端點**  
  針對 Azure Stack 開發套件，此值會設定為 https://management.local.azurestack.external。 若要取得 Azure Stack 整合式系統的這個值，請與您的服務提供者連絡。

## <a name="connect-with-azure-ad"></a>與 Azure AD 連線

```powershell  
    Add-AzureRMEnvironment -Name "AzureStackUser" -ArmEndpoint "https://management.local.azurestack.external"
    # Set your tenant name
    $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackUser").ActiveDirectoryAuthority.TrimEnd('/')
    $AADTenantName = "<myDirectoryTenantName>.onmicrosoft.com"
    $TenantId = (invoke-restmethod "$($AuthEndpoint)/$($AADTenantName)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

    # After signing in to your environment, Azure Stack cmdlets
    # can be easily targeted at your Azure Stack instance.
    Add-AzureRmAccount -EnvironmentName "AzureStackUser" -TenantId $TenantId
```

## <a name="connect-with-ad-fs"></a>與 AD FS 連線

  ```powershell  
  # Register an Azure Resource Manager environment that targets your Azure Stack instance
  Add-AzureRMEnvironment -Name "AzureStackUser" -ArmEndpoint "https://management.local.azurestack.external"

  # Sign in to your environment
  Login-AzureRmAccount -EnvironmentName "AzureStackUser"
  ```

## <a name="register-resource-providers"></a>註冊資源提供者

沒有透過入口網站部署任何資源的新使用者訂用帳戶，並不會自動註冊資源提供者。 執行下列指令碼，即可明確註冊資源提供者：

```powershell  
foreach($s in (Get-AzureRmSubscription)) {
        Select-AzureRmSubscription -SubscriptionId $s.SubscriptionId | Out-Null
        Write-Progress $($s.SubscriptionId + " : " + $s.SubscriptionName)
Get-AzureRmResourceProvider -ListAvailable | Register-AzureRmResourceProvider -Force
    }
```

## <a name="test-the-connectivity"></a>測試連線

當您將一切設定妥當時，使用 PowerShell 在 Azure Stack 中建立資源，以測試連線能力。 在測試時，建立應用程式的資源群組並新增虛擬機器。 若要建立名為 "MyResourceGroup" 的資源群組，請執行下列命令：

```powershell  
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>後續步驟

- [開發適用於 Azure Stack 的範本](azure-stack-develop-templates.md)
- [使用 PowerShell 部署範本](azure-stack-deploy-template-powershell.md)
- [Azure Stack PowerShell 模組參考](https://docs.microsoft.com/en-us/powershell/azure/azure-stack/overview)
- 如果想要針對雲端操作員環境設定 PowerShell，請參閱[設定 Azure Stack 操作員的 PowerShell 環境](../azure-stack-powershell-configure-admin.md)一文。
