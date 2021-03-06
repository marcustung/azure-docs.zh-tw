---
title: 為 Azure Stack 整合式系統部署驗證 Azure Stack 公開金鑰基礎結構憑證 | Microsoft Docs
description: 說明如何驗證 Azure Stack 整合式系統的 Azure Stack PKI 憑證。 包括如何使用 Azure Stack 憑證檢查工具。
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
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: ppacent
ms.lastreviewed: 01/08/2019
ms.openlocfilehash: 06cb29d6d04fb314f9eefa63d7a2b628a2af846b
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2019
ms.locfileid: "58481115"
---
# <a name="validate-azure-stack-pki-certificates"></a>驗證 Azure Stack PKI 憑證

[從 PowerShell 資源庫](https://aka.ms/AzsReadinessChecker)可取得本文中所述的 Azure Stack 整備檢查工具。 您可以使用此工具來驗證[產生的 PKI 憑證](azure-stack-get-pki-certs.md)是否適用於預先部署。 保留足夠的時間來測試及重新發出憑證，以驗證憑證 (如有需要的話)。

整備檢查工具會執行下列憑證驗證：

- **讀取 PFX**  
    確認 PFX 檔案有效且密碼正確，以及公用資訊是否未受密碼保護。 
- **簽章演算法**  
    確認簽章演算法不是 SHA1。
- **私密金鑰**  
    確認私密金鑰存在，且是以本機電腦屬性匯出。 
- **信任鏈結**  
    確認信任鏈結完整包括自我簽署憑證的檢查。
- **DNS 名稱**  
    檢查 SAN 包含每個端點的相關 DNS 名稱，或支援萬用字元是否存在。
- **金鑰使用方式**  
    檢查金鑰使用方式是否包含數位簽章和金鑰加密，而增強金鑰使用方法包含伺服器驗證和用戶端驗證。
- **金鑰大小**  
    確認金鑰大小為 2048 或更大。
- **鏈結順序**  
    檢查其他鏈結的順序，確認順序正確。
- **其他憑證**  
    請確定除了相關的分葉憑證及其鏈結之外，PFX 中沒有封裝其他憑證。
- **沒有設定檔**  
    檢查新的使用者無須載入使用者設定檔即可載入 PFX 資料，在憑證服務期間模擬 gMSA 帳戶的行為。

> [!IMPORTANT]  
> PKI 憑證是 PFX 檔案，且密碼都應該視為機密資訊。

## <a name="prerequisites"></a>必要條件

在驗證 Azure Stack 部署的 PKI 憑證之前，您的系統應該符合下列必要條件：

- Microsoft Azure Stack 整備檢查工具
- 遵循[準備指示](azure-stack-prepare-pki-certs.md)匯出的 SSL 憑證
- DeploymentData.json
- Windows 10 或 Windows Server 2016

## <a name="perform-core-services-certificate-validation"></a>執行核心服務憑證驗證

使用下列步驟來進行準備，以及驗證用於部署和祕密輪替的 Azure Stack PKI 憑證：

1. 執行下列 Cmdlet，以從 PowerShell (5.1 或更新版本) 提示字元安裝 **AzsReadinessChecker**：

    ```powershell  
        Install-Module Microsoft.AzureStack.ReadinessChecker -force 
    ```

2. 建立憑證目錄結構。 在下列範例中，您可以將 `<c:\certificates>` 變更為您所選擇的新目錄路徑。
    ```powershell  
    New-Item C:\Certificates -ItemType Directory
    
    $directories = 'ACSBlob', 'ACSQueue', 'ACSTable', 'Admin Extension Host', 'Admin Portal', 'api_appservice', 'ARM Admin', 'ARM Public', 'ftp_appservice', 'KeyVault', 'KeyVaultInternal', 'Public Extension Host', 'Public Portal', 'sso_appservice', 'wildcard_dbadapter', 'wildcard_sso_appservice'
    
    $destination = 'c:\certificates'
    
    $directories | % { New-Item -Path (Join-Path $destination $PSITEM) -ItemType Directory -Force}
    ```
    
    > [!Note]  
    > 如果您使用 AD FS 作為身分識別系統，則需要 AD FS 和 Graph。 例如︰
    >
    > ```powershell  
    > $directories = 'ACSBlob', 'ACSQueue', 'ACSTable', 'ADFS', 'Admin Extension Host', 'Admin Portal', 'api_appservice', 'ARM Admin', 'ARM Public', 'ftp_appservice', 'Graph', 'KeyVault', 'KeyVaultInternal', 'Public Extension Host', 'Public Portal', 'sso_appservice', 'wildcard_dbadapter', 'wildcard_sso_appservice'
    > ```
    
     - 將您的憑證放在上一個步驟中建立的適當目錄。 例如︰  
        - `c:\certificates\ACSBlob\CustomerCertificate.pfx`
        - `c:\certificates\Admin Portal\CustomerCertificate.pfx`
        - `c:\certificates\ARM Admin\CustomerCertificate.pfx`

3. 在 PowerShell 視窗中，將 **RegionName** 和 **FQDN** 的值變更為適合 Azure Stack 環境的值，並執行下列命令：

    ```powershell  
    $pfxPassword = Read-Host -Prompt "Enter PFX Password" -AsSecureString 

    Invoke-AzsCertificateValidation -CertificatePath c:\certificates -pfxPassword $pfxPassword -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD  
    ```

4. 檢查輸出，所有憑證都通過所有測試。 例如︰

```powershell
Invoke-AzsCertificateValidation v1.1809.1005.1 started.
Testing: ARM Public\ssl.pfx
Thumbprint: 7F6B27****************************E9C35A
    PFX Encryption: OK
    Signature Algorithm: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Parse PFX: OK
    Private Key: OK
    Cert Chain: OK
    Chain Order: OK
    Other Certificates: OK
Testing: Admin Extension Host\ssl.pfx
Thumbprint: A631A5****************************35390A
    PFX Encryption: OK
    Signature Algorithm: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Parse PFX: OK
    Private Key: OK
    Cert Chain: OK
    Chain Order: OK
    Other Certificates: OK
Testing: Public Extension Host\ssl.pfx
Thumbprint: 4DBEB2****************************C5E7E6
    PFX Encryption: OK
    Signature Algorithm: OK
    DNS Names: OK
    Key Usage: OK
    Key Size: OK
    Parse PFX: OK
    Private Key: OK
    Cert Chain: OK
    Chain Order: OK
    Other Certificates: OK

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsCertificateValidation Completed
```

### <a name="known-issues"></a>已知問題

**徵兆**：測試會略過

**原因**︰如果不符合相依性，AzsReadinessChecker 會略過某些測試：

 - 如果信任鏈結失敗，則會略過其他憑證。

    ```powershell  
    Testing: ACSBlob\singlewildcard.pfx
        Read PFX: OK
        Signature Algorithm: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: Fail
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: Skipped
    Details:
    The certificate records '*.east.azurestack.contoso.com' do not contain a record that is valid for '*.blob.east.azurestack.contoso.com'. Please refer to the documentation for how to create the required certificate file.
    The Other Certificates check was skipped because Cert Chain and/or DNS Names failed. Follow the guidance to remediate those issues and recheck. 
    Detailed log can be found C:\AzsReadinessChecker\CertificateValidation\CertChecker.log

    Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
    Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
    Invoke-AzsCertificateValidation Completed
    ```

**解決方案**︰遵循每個憑證測試集合下詳細資料區段中的工具指引。

## <a name="perform-platform-as-a-service-certificate-validation"></a>執行平台即服務憑證驗證

如果規劃了 SQL/MySQL 或 App Service 部署，請使用下列步驟來準備及驗證用於平台即服務 (PaaS) 憑證的 Azure Stack PKI 憑證。

1.  執行下列 Cmdlet，以從 PowerShell (5.1 或更新版本) 提示字元安裝 **AzsReadinessChecker**：

    ```powershell  
      Install-Module Microsoft.AzureStack.ReadinessChecker -force
    ```

2.  建立巢狀雜湊表，並在其中針對每個需要驗證的 PaaS 憑證包含其路徑和密碼。 在 PowerShell 視窗中執行：

    ```powershell  
        $PaaSCertificates = @{
        'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
        }
    ```

3.  將 **RegionName** 和 **FQDN** 的值變更為符合 Azure Stack 環境，以啟動驗證。 然後，執行：

    ```powershell  
    Invoke-AzsCertificateValidation -PaaSCertificates $PaaSCertificates -RegionName east -FQDN azurestack.contoso.com 
    ```
4.  檢查輸出，並檢查所有憑證都通過所有測試。

    ```powershell
    Invoke-AzsCertificateValidation v1.0 started.
    Thumbprint: 95A50B****************************FA6DDA
        Signature Algorithm: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Thumbprint: EBB011****************************59BE9A
        Signature Algorithm: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Thumbprint: 76AEBA****************************C1265E
        Signature Algorithm: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
        Chain Order: OK
        Other Certificates: OK
    Thumbprint: 8D6CCD****************************DB6AE9
        Signature Algorithm: OK
        Parse PFX: OK
        Private Key: OK
        Cert Chain: OK
        DNS Names: OK
        Key Usage: OK
        Key Size: OK
    ```

## <a name="certificates"></a>憑證

| 目錄 | 憑證 |
| ---    | ----        |
| acsBlob | wildcard_blob_\<region>_\<externalFQDN> |
| ACSQueue  |  wildcard_queue_\<region>_\<externalFQDN> |
| ACSTable  |  wildcard_table_\<region>_\<externalFQDN> |
| 管理員延伸主機  |  wildcard_adminhosting_\<region>_\<externalFQDN> |
| 管理入口網站  |  adminportal_\<region>_\<externalFQDN> |
| SSL 管理員  |  adminmanagement_\<region>_\<externalFQDN> |
| ARM 公用  |  management_\<region>_\<externalFQDN> |
| KeyVault  |  wildcard_vault_\<region>_\<externalFQDN> |
| KeyVaultInternal  |  wildcard_adminvault_\<region>_\<externalFQDN> |
| 公用延伸主機  |  wildcard_hosting_\<region>_\<externalFQDN> |
| 公用入口網站  |  portal_\<region>_\<externalFQDN> |

## <a name="using-validated-certificates"></a>使用驗證的憑證

一旦您的憑證已通過 AzsReadinessChecker 的驗證，您就可以在 Azure Stack 部署中使用憑證，或將其用來進行 Azure Stack 秘密輪替。 

 - 針對部署，您可以安全地將憑證傳送給您的部署工程師，讓他們可以將其複製到部署主機上，如 [Azure Stack PKI 需求文件](azure-stack-pki-certs.md)中所述。
 - 針對秘密輪替，您可以遵循 [Azure Stack 秘密輪替文件](azure-stack-rotate-secrets.md)中的指示，使用憑證來更新 Azure Stack 環境中公開基礎結構端點的舊憑證。
 - 針對 PaaS 服務，您可以遵循[在 Azure Stack 中提供服務的概觀文件](azure-stack-offer-services-overview.md)，使用憑證在 Azure Stack 中安裝 SQL、MySQL 和 App Service 資源提供者。

## <a name="next-steps"></a>後續步驟

[資料中心身分識別整合](azure-stack-integrate-identity.md)
