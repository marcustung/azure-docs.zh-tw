---
title: 驗證 Azure Stack 的 Azure 身分識別 | Microsoft Docs
description: 使用 Azure Stack 整備檢查程式來驗證 Azure 身分識別。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 03/23/2019
ms.openlocfilehash: b6a859f1104be8650b95ac789f3da606fb1b70f6
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2019
ms.locfileid: "58486677"
---
# <a name="validate-azure-identity"></a>驗證 Azure 身分識別

使用 Azure Stack 整備檢查程式工具 (**AzsReadinessChecker**) 來驗證 Azure Active Directory (Azure AD) 是否已準備好與 Azure Stack 搭配使用。 開始部署 Azure Stack 之前，請先驗證 Azure 身分識別解決方案。  

整備檢查程式會驗證：

- Azure Active Directory (Azure AD) 為 Azure Stack 的識別提供者。
- 您打算使用的 Azure AD 帳戶可以透過 Azure Active Directory 的全域管理員身分登入。

驗證可確保環境已準備好讓 Azure Stack 在 Azure AD 中儲存 Azure Stack 使用者、應用程式、群組和服務主體的相關資訊。

## <a name="get-the-readiness-checker-tool"></a>取得整備檢查程式工具

從 [PowerShell 資源庫](https://aka.ms/AzsReadinessChecker) \(英文\) 下載最新版的「Azure Stack 整備檢查程式」工具 (AzsReadinessChecker)。  

## <a name="prerequisites"></a>必要條件

需要下列必要條件：

**執行這個工具的電腦：**

- Windows 10 或 Windows Server 2016，具有網際網路連線能力。
- PowerShell 5.1 或更新版本。 若要檢查版本，請執行下列 PowerShell 命令，然後再檢閱**主要**版本和**次要**版本：  

  ```powershell
  $PSVersionTable.PSVersion
  ```

- [針對 Azure Stack 設定的 PowerShell](azure-stack-powershell-install.md)。
- 最新版的 [Microsoft Azure Stack 整備檢查程式](https://aka.ms/AzsReadinessChecker)工具。

**Azure Active Directory 環境：**

- 識別將用於 Azure Stack 的 Azure AD 帳戶，並確保它是 Azure Active Directory 全域管理員。
- 識別 Azure AD 租用戶名稱。 租用戶名稱必須是 Azure Active Directory 的主要網域名稱，例如 **contoso.onmicrosoft.com**。
- 識別您將使用的 Azure 環境。 支援的環境名稱參數值為 **AzureCloud**、**AzureChinaCloud** 或 **AzureUSGovernment**，視您使用哪一個 Azure 訂用帳戶而定。

## <a name="steps-to-validate-azure-identity"></a>驗證 Azure 身分識別的步驟

1. 在符合必要條件的電腦上，開啟提高權限的 PowerShell 命令提示字元，然後執行下列命令以安裝 **AzsReadinessChecker**：  

   ```powershell
   Install-Module Microsoft.AzureStack.ReadinessChecker -Force
   ```

2. 從 PowerShell 提示字元中，執行下列命令以將 $serviceAdminCredential 設定為 Azure AD 租用戶的服務管理員。  用您的帳戶和租用戶名稱取代 **serviceadmin\@contoso.onmicrosoft.com**：

   ```powershell
   $serviceAdminCredential = Get-Credential serviceadmin@contoso.onmicrosoft.com -Message "Enter credentials for service administrator of Azure Active Directory tenant"
   ```

3. 從 PowerShell 提示字元中，執行下列命令以開始驗證您的 Azure AD：

   - 指定 **AzureEnvironment** 的環境名稱值。 支援的環境名稱參數值為 **AzureCloud**、**AzureChinaCloud** 或 **AzureUSGovernment**，視您使用哪一個 Azure 訂用帳戶而定。
   - 用您的 Azure Active Directory 租用戶名稱取代 **contoso.onmicrosoft.com**。

   ```powershell
   Invoke-AzsAzureIdentityValidation -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment <environment name> -AADDirectoryTenantName contoso.onmicrosoft.com
   ```

4. 在此工具執行之後，請檢閱輸出。 確認安裝需求的狀態是**正常**。 驗證成功時會出現類似下圖的輸出：

   ```powershell
   Invoke-AzsAzureIdentityValidation v1.1809.1005.1 started.
   Starting Azure Identity Validation

   Checking Installation Requirements: OK

   Finished Azure Identity Validation

   Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
   Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
   Invoke-AzsAzureIdentityValidation Completed
   ```

## <a name="report-and-log-file"></a>報告與記錄檔

每當驗證執行時，它會將結果記錄到 **AzsReadinessChecker.log** 和 **AzsReadinessCheckerReport.json**。 PowerShell 中的驗證結果會一同顯示這些檔案的位置。

這些檔案可協助您在部署 Azure Stack 或調查驗證問題之前共用驗證狀態。 這兩個檔案會保存每個後續驗證檢查的結果。 此報告會向部署團隊提供身分識別設定的確認。 記錄檔可協助部署或支援小組調查驗證問題。

根據預設，這兩個檔案都會寫入到 C:\Users\<使用者名稱>\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json。  

- 使用執行命令列結尾的 **-OutputPath** ***&lt;path&gt;*** 參數來指定不同的報告位置。
- 使用執行命令結尾的 **-CleanReport** 參數，從 **AzsReadinessCheckerReport.json** 清除先前執行工具時留下的資訊。

如需詳細資訊，請參閱 [Azure Stack 驗證報告](azure-stack-validation-report.md)。

## <a name="validation-failures"></a>驗證失敗

如果驗證檢查失敗，則會在 PowerShell 視窗中顯示有關失敗的詳細資料。 此工具也會將相關資訊記錄至 AzsReadinessChecker.log 檔案中。

下列範例會提供關於一般驗證失敗的指引。

### <a name="expired-or-temporary-password"></a>過期或暫時的密碼

```powershell
Invoke-AzsAzureIdentityValidation v1.1809.1005.1 started.
Starting Azure Identity Validation

Checking Installation Requirements: Fail
Error Details for Service Administrator Account admin@contoso.onmicrosoft.com
The password for account  has expired or is a temporary password that needs to be reset before continuing. Run Login-AzureRMAccount, login with  credentials and follow the prompts to reset.
Additional help URL https://aka.ms/AzsRemediateAzureIdentity

Finished Azure Identity Validation

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsAzureIdentityValidation Completed
```

**原因** - 帳戶無法登入，因為密碼已過期，或屬於暫時密碼。

**解決方式** - 在 PowerShell 中執行下列命令，然後遵循提示來重設密碼：

```powershell
Login-AzureRMAccount
```

或者，以該帳戶的擁有者身分登入 [Azure 入口網站](https://portal.azure.com)，然後系統將會強制使用者變更密碼。

### <a name="unknown-user-type"></a>不明的使用者類型 
 
```powershell
Invoke-AzsAzureIdentityValidation v1.1809.1005.1 started.
Starting Azure Identity Validation

Checking Installation Requirements: Fail
Error Details for Service Administrator Account admin@contoso.onmicrosoft.com
Unknown user type detected. Check the account  is valid for AzureChinaCloud
Additional help URL https://aka.ms/AzsRemediateAzureIdentity

Finished Azure Identity Validation

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsAzureIdentityValidation Completed
```

**原因** - 帳戶無法登入指定的 Azure Active Directory (**AADDirectoryTenantName**)。 在此範例中，AzureChinaCloud 會指定為 AzureEnvironment。

**解決方式** - 確認此帳戶對指定的 Azure 環境是有效的。 在 PowerShell 中，執行下列命令以確認此帳戶是可在特定環境中使用的有效帳戶：

```powershell
Login-AzureRmAccount –EnvironmentName AzureChinaCloud
```

### <a name="account-is-not-an-administrator"></a>帳戶不是系統管理員

```powershell
Invoke-AzsAzureIdentityValidation v1.1809.1005.1 started.
Starting Azure Identity Validation

Checking Installation Requirements: Fail
Error Details for Service Administrator Account admin@contoso.onmicrosoft.com
The Service Admin account you entered 'admin@contoso.onmicrosoft.com' is not an administrator of the Azure Active Directory tenant 'contoso.onmicrosoft.com'.
Additional help URL https://aka.ms/AzsRemediateAzureIdentity

Finished Azure Identity Validation

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsAzureIdentityValidation Completed
```

**原因** - 雖然帳戶可以成功登入，但該帳戶不是 Azure Active Directory (**AADDirectoryTenantName**) 的系統管理員。  

**解決方式** - 以帳戶擁有者身分登入 [Azure 入口網站](https://portal.azure.com)，並移至 [Azure Active Directory]、[使用者]、[選取使用者] 及 [目錄角色]，然後確定使用者是**全域管理員**。 如果帳戶是**使用者**，請移至 [Azure Active Directory] > [自訂網域] 名稱，並確認您提供給 **AADDirectoryTenantName** 的名稱已標示為這個目錄的主要網域名稱。 在此範例中，這就是 contoso.onmicrosoft.com。

Azure Stack 會要求網域名稱必須是主要網域名稱。

## <a name="next-steps"></a>後續步驟

[驗證 Azure 註冊](azure-stack-validate-registration.md)  
[檢視整備報告](azure-stack-validation-report.md)  
[一般 Azure Stack 整合考量](azure-stack-datacenter-integration.md)  
