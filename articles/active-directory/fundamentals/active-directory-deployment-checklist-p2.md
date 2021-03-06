---
title: Azure AD 部署檢查清單
description: Azure Active Directory 功能部署檢查清單
services: active-directory
ms.service: active-directory
ms.subservice: ''
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6e9ee0d6fab96c84eee8a520d01d97faddab49f2
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2019
ms.locfileid: "58904180"
---
# <a name="azure-active-directory-feature-deployment-guide"></a>Azure Active Directory 功能部署指南

為您的組織部署 Azure Active Directory (Azure AD) 並確保其安全，看起來似乎具挑戰性。 本文指出客戶通常在 30、60、90 天或更長時間內分階段完成的一般工作，以增強其安全性狀態。 即使是已部署 Azure AD 的組織，也可以使用此指南來確保從投資中獲得最大收益。

完善規劃及執行的身分識別基礎結構，可以讓已知的使用者和裝置安全存取生產力工作負載和資料。

此外，客戶可以檢查其[身分識別安全分數](identity-secure-score.md)，以了解他們符合 Microsoft 最佳做法的程度。 在實作這些建議前後檢查您的安全分數，以了解相較於您產業中的其他人和規模與您相同的其他組織，您的表現如何。

## <a name="prerequisites"></a>必要條件

本指南中有很多建議可以透過 Azure AD Free、Basic 實作，或完全不需授權即可實作。 若需要授權，我們會陳述至少需要哪一個授權才能完成工作。

在下列頁面可以找到額外的授權資訊：

* [Azure AD 授權](https://azure.microsoft.com/pricing/details/active-directory/)
* [Microsoft 365 Enterprise](https://www.microsoft.com/en-us/licensing/product-licensing/microsoft-365-enterprise)
* [Enterprise Mobility + Security](https://www.microsoft.com/en-us/licensing/product-licensing/enterprise-mobility-security)
* [Azure AD B2B 授權指導方針](../b2b/licensing-guidance.md)

## <a name="phase-1-build-a-foundation-of-security"></a>第 1 階段：建置安全基礎

在這個階段，系統管理員會啟用基準安全性功能，以在我們匯入或建立一般使用者帳戶之前，在 Azure AD 中建立更安全且容易使用的基礎。 此基本階段可確保您一開始就處於更安全的狀態，而且只需要向一般使用者介紹新概念一次。

| Task | 詳細資料 | 必要授權 |
| ---- | ------ | ---------------- |
| [指定一個以上的全域系統管理員](../users-groups-roles/directory-emergency-access.md) | 在發生緊急狀況時，請指定至少兩個僅限雲端的永久全域管理員帳戶。 這些帳戶並非每天使用，而且應該有複雜的長密碼。 | Azure AD Free |
| [請盡可能使用非全域系統管理角色](../users-groups-roles/directory-assign-admin-roles.md) | 僅賦予您的系統管理員存取其所需存取區域的權限。 並非所有系統管理員都必須是全域管理員。 | Azure AD Free |
| [追蹤系統管理員角色，請使用啟用 Privileged Identity Management](../privileged-identity-management/pim-getting-started.md) | 啟用 Privileged Identity Management 以開始追蹤系統管理角色使用方式。 | Azure AD Premium P2 |
| [推出自助式密碼重設](../authentication/howto-sspr-deployment.md) | 允許員工使用您設為管理員控制項的原則來重設自己的密碼，以減少針對密碼重設的技術支援電話。 | Azure AD Basic |
| [建立組織特定自訂的禁用的密碼清單](../authentication/howto-password-ban-bad-configure.md) | 防止使用者從您的組織或區域建立包含常見字組或片語的密碼。 | Azure AD Basic |
| [啟用與 Azure AD 密碼保護的內部部署整合](../authentication/concept-password-ban-bad-on-premises.md) | 將禁用密碼清單擴充至您的內部部署目錄，以確保在內部部署環境設定的密碼也會符合全域和租用戶專屬禁用密碼清單的規定。 | Azure AD Premium P1 |
| [啟用 Microsoft 密碼指引](https://www.microsoft.com/research/publication/password-guidance/) | 停止要求使用者依照設定的排程變更其密碼，停用複雜性需求，而您的使用者會更容易記住其密碼並維護其安全性。 | Azure AD Free |
| [停用定期的密碼重設雲端使用者帳戶](../authentication/concept-sspr-policy.md#set-a-password-to-never-expire) | 定期密碼重設會鼓勵使用者遞增其現有的密碼。 使用 Microsoft 的密碼指引文件中的指導方針，將您的內部部署原則反映至僅限雲端的使用者。 | Azure AD Free |
| [自訂 Azure Active Directory 的智慧鎖定](../authentication/howto-password-smart-lockout.md) | 讓來自雲端式使用者的鎖定停止複寫至內部部署 Active Directory 使用者 | Azure AD Basic |
| [適用於 AD FS 中啟用外部網路的智慧鎖定](/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-smart-lockout-protection) | AD FS 外部網路鎖定可防禦暴力密碼猜測破解攻擊，同時讓有效的 AD FS 使用者繼續使用其帳戶。 | |
| [部署使用條件式存取原則的 Azure AD 多重要素驗證](../authentication/howto-mfa-getstarted.md) | 要求使用者在使用條件式存取原則來存取敏感性應用程式時，執行雙步驟驗證。 | Azure AD Premium P1 |
| [啟用 Azure Active Directory Identity Protection](../identity-protection/enable.md) | 能夠追蹤有風險的登入以及貴組織中使用者遭入侵的認證。 | Azure AD Premium P2 |
| [使用風險事件來觸發 multi-factor authentication 和密碼變更](../authentication/tutorial-risk-based-sspr-mfa.md) | 啟用可觸發以下事件的自動化功能：例如多重要素驗證、密碼重設，以及根據風險封鎖登入。 | Azure AD Premium P2 |
| [啟用聚合式的註冊自助式密碼重設和 Azure AD Multi-factor Authentication （預覽）](../authentication/concept-registration-mfa-sspr-converged.md) | 可讓使用者在單一常見體驗中註冊 Azure Multi-Factor Authentication 和自助密碼重設。 | Azure AD Premium P1 |

## <a name="phase-2-import-users-enable-synchronization-and-manage-devices"></a>第 2 階段：匯入使用者、啟用同步處理，以及管理裝置

接著，我們會匯入使用者，以及啟用同步處理、規劃來賓存取及準備支援額外功能，藉此擴建增階段 1 所設置的基礎。

| Task | 詳細資料 | 必要授權 |
| ---- | ------ | ---------------- |
| [安裝 Azure AD Connect。](../connect/active-directory-aadconnect-select-installation.md) | 準備讓使用者從現有的內部部署目錄同步至雲端。 | Azure AD Free |
| [實作密碼雜湊同步處理](../connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md) | 同步密碼雜湊，以允許複寫變更密碼、偵測和補救不正確的密碼，以及回報外洩的認證。 | Azure AD Premium P1 |
| [實作密碼回寫](../authentication/howto-sspr-writeback.md) | 可讓雲端中的密碼變更回寫至內部部署 Windows Server Active Directory 環境。 | Azure AD Premium P1 |
| [實作 Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md) | 能夠監視 Azure AD Connect 伺服器、AD FS 伺服器和網域控制站的主要健康情況統計資料。 | Azure AD Premium P1 |
| [依據 Azure Active Directory 中的群組成員資格將授權指派給使用者](../users-groups-roles/licensing-groups-assign.md) | 建立可依照群組啟用或停用功能 (而非依據每位使用者設定) 的授權群組，以節省時間和精力。 | |
| [建立來賓使用者存取權的計劃](../b2b/what-is-b2b.md) | 讓來賓使用者使用自己的公司、學校或社交身分識別登入您的應用程式與服務，藉此與他們共同作業。 | [Azure AD B2B 授權指導方針](../b2b/licensing-guidance.md) |
| [決定裝置管理策略](../devices/overview.md) | 決定貴組織允許的裝置相關事項。 註冊與加入，攜帶您自己的裝置與公司提供。 | |
| [Windows hello 企業版在組織中部署](/windows/security/identity-protection/hello-for-business/hello-manage-in-organization) | 準備使用 Windows Hello 的無密碼驗證 | |

## <a name="phase-3-manage-applications"></a>階段 3：管理應用程式

當我們繼續以先前的階段為根基，我們會找出可供移轉以及與 Azure AD 整合的候選應用程式，並完成這些應用程式的設定。

| Task | 詳細資料 | 必要授權 |
| ---- | ------ | ---------------- |
| 識別您的應用程式 | 識別貴組織中使用的應用程式：內部部署應用程式、雲端中的 SaaS 應用程式，以及其他企業營運應用程式。 判斷這些應用程式是否可以且應該透過 Azure AD 管理。 | 不需要授權 |
| [將資源庫中支援的 SaaS 應用程式整合](../manage-apps/add-application-portal.md) | Azure AD 有一個資源庫，其中包含數千個預先整合的應用程式。 您組織使用的某些應用程式可能就在可從 Azure 入口網站直接存取的資源庫中。 | Azure AD Free |
| [若要整合內部部署應用程式使用應用程式 Proxy](../manage-apps/application-proxy-add-on-premises-application.md) | 應用程式 Proxy 可讓使用者使用其 Azure AD 帳戶登入來存取內部部署應用程式。 | Azure AD Basic |

## <a name="phase-4-audit-privileged-identities-complete-an-access-review-and-manage-user-lifecycle"></a>階段 4：稽核特殊權限的身分識別、完成存取權檢閱，以及管理使用者生命週期

階段 4 可看見系統管理員強制執行系統管理的最低權限準則、完成其第一次存取權檢閱，以及讓一般使用者生命週期工作自動化。

| Task | 詳細資料 | 必要授權 |
| ---- | ------ | ---------------- |
| [強制使用 Privileged Identity Management](../privileged-identity-management/pim-security-wizard.md) | 從一般日常使用者帳戶中移除系統管理角色。 在成功執行多重要素驗證檢查、提供業務理由，或是向指定的核准者要求核准之後，讓系統管理使用者有資格使用其角色。 | Azure AD Premium P2 |
| [在 PIM 中完成 Azure AD 目錄角色的存取權檢閱](../privileged-identity-management/pim-how-to-start-security-review.md) | 與您的安全性和領導小組合作，一起建立存取權檢閱原則，以根據貴組織原則來檢閱系統管理存取權。 | Azure AD Premium P2 |
| [實作動態群組成員資格的原則](../users-groups-roles/groups-dynamic-membership.md) | 使用動態群組，根據 HR (或您的真實來源) 的屬性自動將使用者指派至群組，例如部門、標題、區域和其他屬性。 |  |
| [實作以群組為基礎的應用程式佈建](../manage-apps/what-is-access-management.md) | 使用群組型存取管理佈建，為 SaaS 應用程式自動佈建使用者。 |  |
| [自動化使用者佈建和解除佈建](../manage-apps/user-provisioning.md) | 從您員工帳戶的生命週期移除手動步驟，以防止未經授權的存取。 將真實來源 (HR系統) 中的身分識別同步至 Azure AD。 |  |

## <a name="next-steps"></a>後續步驟

[Azure AD 授權和定價詳細資料](https://azure.microsoft.com/pricing/details/active-directory/)

[身分識別與裝置存取設定](https://docs.microsoft.com/microsoft-365/enterprise/microsoft-365-policies-configurations)

[一般建議的身分識別和裝置存取原則](https://docs.microsoft.com/microsoft-365/enterprise/identity-access-policies)
