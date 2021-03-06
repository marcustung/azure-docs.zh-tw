---
title: 教學課程：Azure Active Directory 與 MOVEit Transfer - Azure AD integration 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 MOVEit Transfer - Azure AD integration 之間的單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/25/2019
ms.author: jeedes
ms.openlocfilehash: d7020299bbd52f5e7ba22809847815cb04048cb6
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59259400"
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a>教學課程：Azure Active Directory 與 MOVEit Transfer - Azure AD integration 整合

在本教學課程中，您將了解如何整合 MOVEit Transfer - Azure AD integration 與 Azure Active Directory (Azure AD)。
將 MOVEit Transfer - Azure AD integration 與 Azure AD 整合可提供下列優點：

* 您可以在 Azure AD 中控制可存取 MOVEit Transfer - Azure AD integration 的人員。
* 您可以讓使用者使用他們的 Azure AD 帳戶自動登入 MOVEit Transfer - Azure AD integration (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>必要條件

若要設定 Azure AD 與 MOVEit Transfer - Azure AD integration 整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以在[這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月的試用帳戶
* 已啟用 MOVEit Transfer - Azure AD integration 單一登入功能的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* MOVEit Transfer - Azure AD integration 支援 **SP** 起始的 SSO

## <a name="adding-moveit-transfer---azure-ad-integration-from-the-gallery"></a>從資源庫新增 MOVEit Transfer - Azure AD integration

若要設定 MOVEit Transfer - Azure AD integration 與 Azure AD 整合，您需要從資源庫將 MOVEit Transfer - Azure AD integration 新增到受控 SaaS 應用程式清單中。

**若要從資源庫新增 MOVEit Transfer - Azure AD 整合，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory] 圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]，然後選取 [所有應用程式] 選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式] 按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **MOVEit Transfer - Azure AD integration**，從結果面板中選取 [MOVEit Transfer - Azure AD integration]，然後按一下 [新增] 按鈕以新增應用程式。

     ![結果清單中的 MOVEit Transfer - Azure AD integration](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 **Britta Simon** 的測試使用者身分，設定及測試與 MOVEit Transfer - Azure AD integration 搭配運作的 Azure AD 單一登入。
為了讓單一登入正常運作，必須建立 Azure AD 使用者和 MOVEit Transfer - Azure AD integration 中相關使用者之間的連結關聯性。

若要設定及測試對 MOVEit Transfer - Azure AD integration 的 Azure AD 單一登入，您需要完成下列建構元素：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 MOVEit Transfer - Azure AD integration 單一登入](#configure-moveit-transfer---azure-ad-integration-single-sign-on)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 MOVEit Transfer - Azure AD integration 測試使用者](#create-moveit-transfer---azure-ad-integration-test-user)** - 使 MOVEit Transfer - Azure AD integration 中對應的 Britta Simon 連結到該使用者在 Azure AD 中的代表項目。
6. **[測試單一登入](#test-single-sign-on)**，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要使用 MOVEit Transfer - Azure AD integration 設定 Azure AD 單一登入功能，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [MOVEit Transfer - Azure AD integration] 應用程式整合頁面上，選取 [單一登入]。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法] 對話方塊中，選取 [SAML/WS-Fed] 模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 如果您有**服務提供者中繼資料檔案**，請在 [基本 SAML 設定] 區段上執行下列步驟：

    a. 按一下 [上傳中繼資料檔案]。

    ![上傳中繼資料檔案](common/upload-metadata.png)

    b. 按一下**資料夾圖示**以選取中繼資料檔案，然後按一下 [上傳]。

    ![選擇中繼資料檔案](common/browse-upload-metadata.png)

    c. 成功上傳中繼資料檔案後，就會在 [基本 SAML 組態] 區段中自動填入 [識別碼] 和 [回覆 URL] 值：

    ![MOVEit Transfer - Azure AD integration 網域和 URL 單一登入資訊](common/sp-identifier-reply.png)

    在 [登入 URL] 文字方塊中，以下列模式輸入 URL︰`https://contoso.com`

    > [!NOTE]
    > [登入 URL] 的值不是真正的值。 請使用實際的「登入 URL」來更新此值。 請連絡 [MOVEit Transfer - Azure AD integration 用戶端支援](https://community.ipswitch.com/s/support)小組來取得此值。 您可以從**服務提供者中繼資料 URL** 中下載**服務提供者中繼資料檔案**，稍後會在教學課程中的＜設定 MOVEit Transfer - Azure AD integration 單一登入＞一節中說明。 您也可以參考 Azure 入口網站中**基本 SAML 組態**區段所示的模式。

4. 在 [以 SAML 設定單一登入] 頁面的 [SAML 簽署憑證] 區段中按一下 [下載]，以依據您的需求從指定選項下載**同盟中繼資料 XML**，並儲存在您的電腦上。

    ![憑證下載連結](common/metadataxml.png)

6. 在 [設定 MOVEit Transfer - Azure AD integration] 區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

    a. 登入 URL

    b. Azure AD 識別碼

    c. 登出 URL

### <a name="configure-moveit-transfer---azure-ad-integration-single-sign-on"></a>設定 MOVEit Transfer - Azure AD integration 單一登入

1. 以系統管理員身分登入 MOVEit Transfer 租用戶。

2. 在左側的導覽窗格上，按一下 [設定] 。

    ![應用程式端上的設定區段](./media/moveittransfer-tutorial/tutorial_moveittransfer_000.png)

3. 按一下 [單一登入] 連結，其位於 [安全性原則] -> [使用者驗證] 下。

    ![應用程式端上的安全性原則](./media/moveittransfer-tutorial/tutorial_moveittransfer_001.png)

4. 按一下 [中繼資料 URL] 連結以下載中繼資料文件。

    ![服務提供者中繼資料 URL](./media/moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
   * 請確認 **entityID** 符合**基本 SAML 組態**區段中的**識別碼**。
   * 確認 [AssertionConsumerService] 位置 URL 符合 [基本 SAML 組態] 區段中的 **REPLY URL**。
    
     ![在應用程式端設定單一登入](./media/moveittransfer-tutorial/tutorial_moveittransfer_007.png)

5. 按一下 [新增識別提供者]  按鈕來加入新的同盟識別提供者。

    ![新增識別提供者](./media/moveittransfer-tutorial/tutorial_moveittransfer_003.png)

6. 按一下 [瀏覽...] 以選取您從 Azure 入口網站下載的中繼資料檔案，然後按一下 [新增識別提供者] 以上傳所下載的檔案。

    ![SAML 識別提供者](./media/moveittransfer-tutorial/tutorial_moveittransfer_004.png)

7. 選取 [是] 做為 [編輯同盟識別提供者設定...] 頁面中的 [啟用]，然後按一下 [儲存]。

    ![同盟識別提供者設定](./media/moveittransfer-tutorial/tutorial_moveittransfer_005.png)

8. 在 [編輯同盟識別提供者使用者設定] 頁面中，執行下列動作：
    
    ![編輯同盟識別提供者設定](./media/moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    a. 選取 [SAML NameID] 做為 [登入名稱]。
    
    b. 在 [其他] 中選取 [全名]，並在 [屬性名稱] 文字方塊中，輸入值：`http://schemas.microsoft.com/identity/claims/displayname`。
    
    c. 在 [其他] 中選取 [電子郵件]，並在 [屬性名稱] 文字方塊中，輸入值：`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`。
    
    d. 選取 [是] 做為 [在登入時自動建立帳戶]。
    
    e. 按一下 [儲存]  按鈕。

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者 

本節的目標是要在 Azure 入口網站中建立一個名為 Britta Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]、[使用者] 和 [所有使用者]。

    ![[使用者和群組] 與 [所有使用者] 連結](common/users.png)

2. 在畫面頂端選取 [新增使用者]。

    ![[新增使用者] 按鈕](common/new-user.png)

3. 在 [使用者] 屬性中，執行下列步驟。

    ![[使用者] 對話方塊](common/user-properties.png)

    a. 在 [名稱] 欄位中，輸入 **BrittaSimon**。
  
    b. 在 [使用者名稱] 欄位中，輸入 **brittasimon\@yourcompanydomain.extension**  
    例如， BrittaSimon@contoso.com

    c. 選取 [顯示密碼] 核取方塊，然後記下 [密碼] 方塊中顯示的值。

    d. 按一下頁面底部的 [新增] 。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 MOVEit Transfer - Azure AD integration 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]、[所有應用程式] 及 [MOVEit Transfer - Azure AD integration]。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [MOVEit Transfer - Azure AD integration]。

    ![應用程式清單中的 MOVEit Transfer - Azure AD integration 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者] 按鈕，然後在 [新增指派] 對話方塊中，選取 [使用者和群組]。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組] 對話方塊的 [使用者] 清單中，選取 [Britta Simon]，然後按一下畫面底部的 [選取] 按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色] 對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取] 按鈕。

7. 在 [新增指派] 對話方塊中，按一下 [指派] 按鈕。

### <a name="create-moveit-transfer---azure-ad-integration-test-user"></a>建立 MOVEit Transfer - Azure AD integration 測試使用者

本節的目標是要在 MOVEit Transfer - Azure AD integration 中建立一個名為 Britta Simon 的使用者。 MOVEit Transfer - Azure AD integration 支援您已啟用的 Just-In-Time 佈建。 在這一節沒有您需要進行的動作項目。 嘗試存取 MOVEit Transfer - Azure AD integration 時若尚無使用者，則會建立新使用者。

>[!NOTE]
>如果您需要手動建立使用者，則必須連絡 [MOVEit Transfer - Azure AD integration 用戶端支援小組](https://community.ipswitch.com/s/support)。

### <a name="test-single-sign-on"></a>測試單一登入 

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在「存取面板」中按一下 [MOVEit Transfer - Azure AD integration] 圖格時，應該會自動登入您設定 SSO 的 MOVEit Transfer - Azure AD integration。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

