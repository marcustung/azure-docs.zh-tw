---
title: 教學課程：Azure Active Directory 與 BlueJeans 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 BlueJeans 之間的單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/31/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b22a7a86862f02c3320b7c14cc1ed878a57bd23d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "57901945"
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>教學課程：Azure Active Directory 與 BlueJeans 整合

在本教學課程中，您會了解如何將 BlueJeans 與 Azure Active Directory (Azure AD) 整合。
將 BlueJeans 與 Azure AD 整合可提供下列優點：

* 您可以在 Azure AD 中控制可存取 BlueJeans 的人員。
* 您可以讓使用者使用其 Azure AD 帳戶自動登入 BlueJeans (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>必要條件

若要設定 Azure AD 與 BlueJeans 的整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以在[這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月的試用帳戶
* 已啟用 BlueJeans 單一登入的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* BlueJeans 支援由 **SP** 起始的 SSO

* BlueJeans 支援[**自動**使用者佈建](bluejeans-provisioning-tutorial.md)

## <a name="adding-bluejeans-from-the-gallery"></a>從資源庫新增 BlueJeans

若要設定將 BlueJeans 整合到 Azure AD 中，您需要從資源庫將 BlueJeans 新增到受控 SaaS 應用程式清單。

**若要從資源庫新增 BlueJeans，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory] 圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]，然後選取 [所有應用程式] 選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式] 按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **BlueJeans**，從結果面板中選取 [BlueJeans]，然後按一下 [新增] 按鈕以新增應用程式。

     ![結果清單中的 BlueJeans](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 **Britta Simon** 的測試使用者為基礎，設定及測試與 BlueJeans 搭配運作的 Azure AD 單一登入。
若要讓單一登入能夠運作，必須建立 Azure AD 使用者與 BlueJeans 中相關使用者之間的連結關聯性。

若要設定及測試與 BlueJeans 搭配運作的 Azure AD 單一登入，您需要完成下列構成要素：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 BlueJeans 單一登入](#configure-bluejeans-single-sign-on)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 BlueJeans 測試使用者](#create-bluejeans-test-user)** - 使 BlueJeans 中對應的 Britta Simon 連結到該使用者在 Azure AD 中的代表項目。
6. **[測試單一登入](#test-single-sign-on)**，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要設定與 BlueJeans 搭配運作的 Azure AD 單一登入，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/) 的 [BlueJeans] 應用程式整合頁面上，選取 [單一登入]。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法] 對話方塊中，選取 [SAML/WS-Fed] 模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [基本 SAML 組態] 區段上，執行下列步驟：

    ![BlueJeans 網域及 URL 單一登入資訊](common/sp-signonurl.png)

    在 [登入 URL] 文字方塊中，以下列模式輸入 URL︰`https://<companyname>.BlueJeans.com`

    > [!NOTE]
    > 這不是真正的值。 請使用實際的「登入 URL」來更新此值。 請連絡 [BlueJeans 用戶端支援小組](https://support.bluejeans.com/contact)以取得此值。 您也可以參考 Azure 入口網站中**基本 SAML 組態**區段所示的模式。

4. 在 [以 SAML 設定單一登入] 頁面的 [SAML 簽署憑證] 區段中，按一下 [下載]，以依據您的需求從指定選項下載 [憑證 (Base64)]，並儲存在您的電腦上。

    ![憑證下載連結](common/certificatebase64.png)

6. 在 [設定 BlueJeans] 區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

    a. 登入 URL

    b. Azure AD 識別碼

    c. 登出 URL

### <a name="configure-bluejeans-single-sign-on"></a>設定 BlueJeans 單一登入

1. 在不同的網頁瀏覽器視窗中，以系統管理員身分登入您的 **BlueJeans** 公司網站。

2. 移至 [管理] \> [群組設定] \> [安全性]。

    ![管理](./media/bluejeans-tutorial/IC785868.png "管理")

3. 在 [安全性] 區段中，執行下列步驟：

    ![SAML 單一登入](./media/bluejeans-tutorial/IC785869.png "SAML 單一登入")

    a. 選取 [SAML 單一登入] 。

    b. 選取 [啟用自動佈建] 。

4. 繼續執行下列步驟：

    ![憑證路徑](./media/bluejeans-tutorial/IC785870.png "憑證路徑")

    a. 按一下 [選擇檔案]，以上傳您從 Azure 入口網站下載的 base-64 編碼憑證。

    b. 在 [登入 URL] 文字方塊中，貼上您從 Azure 入口網站複製的 [登入 URL] 值。

    c. 在 [密碼變更 URL] 文字方塊中，貼上您從 Azure 入口網站複製的 [變更密碼 URL] 值。

    d. 在 [登出 URL] 文字方塊中，貼上您從 Azure 入口網站複製的 [登出 URL] 值。

5. 繼續執行下列步驟：

    ![儲存變更](./media/bluejeans-tutorial/IC785874.png "儲存變更")

    a. 在 [使用者識別碼] 文字方塊中，輸入 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`。

    b. 在 [電子郵件] 文字方塊中，輸入 `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`。

    c. 按一下 [儲存變更]。

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

在本節中，您會將 BlueJeans 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]、[所有應用程式] 及 [BlueJeans]。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [BlueJeans]。

    ![應用程式清單中的 [BlueJeans] 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者] 按鈕，然後在 [新增指派] 對話方塊中，選取 [使用者和群組]。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組] 對話方塊的 [使用者] 清單中，選取 [Britta Simon]，然後按一下畫面底部的 [選取] 按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色] 對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取] 按鈕。

7. 在 [新增指派] 對話方塊中，按一下 [指派] 按鈕。

### <a name="create-bluejeans-test-user"></a>建立 BlueJeans 測試使用者

本節的目標是要在 BlueJeans 中建立一個名為 Britta Simon 的使用者。 BlueJeans 支援自動使用者佈建，該功能預設為啟用。 您可以在[這裡](bluejeans-provisioning-tutorial.md)找到關於如何設定自動使用者佈建的更多詳細資料。

**如果您需要手動建立使用者，請執行下列步驟：**

1. 以系統管理員身分登入您的 **BlueJeans** 公司網站。

2. 移至 [管理] \> [管理使用者] \> [新增使用者]。

    ![管理](./media/bluejeans-tutorial/IC785877.png "管理")

    >[!IMPORTANT]
    >只有在未核取 [安全性] 索引標籤中的 [啟用自動佈建] 時，才能使用 [新增使用者] 索引標籤。 

3. 在 [新增使用者] 區段中，執行下列步驟：

    ![新增使用者](./media/bluejeans-tutorial/IC785886.png "新增使用者")

    a. 在 [名字] 文字方塊中，輸入使用者的名字，例如 **Britta**。

    b. 在 [姓氏] 文字方塊中，輸入使用者的姓氏，例如 **Simon**。

    c. 在 [挑選 BlueJeans 使用者名稱] 文字方塊中，輸入使用者的名稱，例如 **Brittasimon**。

    d. 在 [建立密碼] 文字方塊中，輸入您的密碼。

    e. 在 [公司] 文字中，輸入您的公司。

    f. 在 [Email Address] \(電子郵件地址\) 文字方塊中，輸入使用者的電子郵件地址，例如 **brittasimon\@contoso.com**。

    g. 在 [建立 BlueJeans 會議識別碼] 文字方塊中，輸入您的會議識別碼。

    h. 在 [挑選審查工具密碼] 文字方塊中，輸入您的密碼。

    i. 按一下 [繼續]。

    ![新增使用者](./media/bluejeans-tutorial/IC785887.png "新增使用者")

    J. 按一下 [新增使用者]。

> [!NOTE]
> 您可以使用任何其他的 BlueJeans 使用者帳戶建立工具或 BlueJeans 提供的 API 來佈建 Azure AD 使用者帳戶。

### <a name="test-single-sign-on"></a>測試單一登入

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [BlueJeans] 圖格時，應該會自動登入您已設定 SSO 的 BlueJeans。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

