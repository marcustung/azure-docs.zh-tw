---
title: 教學課程：Azure Active Directory 與 ClickUp Productivity Platform 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 ClickUp Productivity Platform 之間的單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 06853edd-e8da-4ad2-a4e6-5555d592cee5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: jeedes
ms.openlocfilehash: 3244140999dc61560549db077d4c402b3986956b
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59282639"
---
# <a name="tutorial-azure-active-directory-integration-with-clickup-productivity-platform"></a>教學課程：Azure Active Directory 與 ClickUp Productivity Platform 整合

在本教學課程中，您會了解如何將 ClickUp Productivity Platform 與 Azure Active Directory (Azure AD) 整合。
將 ClickUp Productivity Platform 與 Azure AD 整合可提供下列優點：

* 您可以在 Azure AD 中控制可存取 ClickUp Productivity Platform 的人員。
* 您可以讓使用者使用其 Azure AD 帳戶自動登入 ClickUp Productivity Platform (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>必要條件

若要設定 Azure AD 與 ClickUp Productivity Platform 的整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以在[這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月的試用帳戶
* 已啟用 ClickUp Productivity Platform 單一登入功能的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* ClickUp Productivity Platform 支援 **SP** 起始的 SSO

## <a name="adding-clickup-productivity-platform-from-the-gallery"></a>從資源庫新增 ClickUp Productivity Platform

若要設定將 ClickUp Productivity Platform 整合到 Azure AD 中，您需要從資源庫將 ClickUp Productivity Platform 新增到受控 SaaS 應用程式清單。

**若要從資源庫新增 ClickUp Productivity Platform，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory] 圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]，然後選取 [所有應用程式] 選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式] 按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **ClickUp Productivity Platform**，從結果面板中選取 [ClickUp Productivity Platform]，然後按一下 [新增] 按鈕以新增應用程式。

     ![結果清單中的 ClickUp Productivity Platform](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 **Britta Simon** 的測試使用者為基礎，設定及測試與 ClickUp Productivity Platform 搭配運作的 Azure AD 單一登入。
若要讓單一登入能夠運作，必須建立 Azure AD 使用者與 ClickUp Productivity Platform 中相關使用者之間的連結關聯性。

若要設定及測試與 ClickUp Productivity Platform 搭配運作的 Azure AD 單一登入，您需要完成下列構成要素：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 ClickUp Productivity Platform 單一登入](#configure-clickup-productivity-platform-single-sign-on)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 ClickUp Productivity Platform 測試使用者](#create-clickup-productivity-platform-test-user)** - 使 ClickUp Productivity Platform 中 Britta Simon 的對應使用者連結到該使用者在 Azure AD 中的代表身分。
6. **[測試單一登入](#test-single-sign-on)**，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要設定與 ClickUp Productivity Platform 搭配運作的 Azure AD 單一登入，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [ClickUp Productivity Platform] 應用程式整合頁面上，選取 [單一登入]。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法] 對話方塊中，選取 [SAML/WS-Fed] 模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [基本 SAML 組態] 區段上，執行下列步驟：

    ![ClickUp Productivity Platform 網域及 URL 單一登入資訊](common/sp-identifier.png)

    a. 在 [登入 URL] 文字方塊中，輸入 URL： `https://app.clickup.com/login/sso`

    b. 在 [識別碼 (實體識別碼)] 文字方塊中，使用下列模式輸入 URL： `https://api.clickup.com/v1/team/<team_id>/microsoft`

    > [!NOTE]
    > 識別碼值不是實際值。 使用實際的識別碼來更新此值，稍後會在本教學課程中說明。

5. 在 [以 SAML 設定單一登入] 頁面的 [SAML 簽署憑證] 區段中，按一下 [複製] 按鈕以複製 [應用程式同盟中繼資料 URL]，並將其儲存在您的電腦上。

    ![憑證下載連結](common/copy-metadataurl.png)

### <a name="configure-clickup-productivity-platform-single-sign-on"></a>設定 ClickUp Productivity Platform 單一登入

1. 在不同的網頁瀏覽器視窗中，以系統管理員身分登入您的 ClickUp Productivity Platform 租用戶。

2. 按一下 [使用者設定檔]，然後選取 [設定]。

    ![ClickUp Productivity 設定](./media/clickup-productivity-platform-tutorial/configure1.png)

3. 在 [單一登入 (SSO) 提供者] 下方選取 [Microsoft]。

    ![ClickUp Productivity 設定](./media/clickup-productivity-platform-tutorial/configure2.png)

4. 在 [設定 Microsoft 單一登入] 頁面上，執行下列步驟：

    ![ClickUp Productivity 設定](./media/clickup-productivity-platform-tutorial/configure3.png)

    a. 按一下 [複製] 以複製實體識別碼值，並將其貼至 Azure 入口網站中 [基本 SAML 組態] 區段上的 [識別碼 (實體識別碼)] 文字方塊中。
    
    b. 在 [Azure 同盟中繼資料 URL] 文字方塊中，貼上您從 Azure 入口網站複製的應用程式同盟中繼資料 URL 值，然後按一下 [儲存]。

5. 若要完成設定，請按一下 [向 Microsoft 驗證以完成設定]，然後使用 Microsoft 帳戶進行驗證。

    ![ClickUp Productivity 設定](./media/clickup-productivity-platform-tutorial/configure4.png)

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

在本節中，您會將 ClickUp Productivity Platform 的存取權授與 Britta Simon，使其能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]、[所有應用程式] 及 [ClickUp Productivity Platform]。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [ClickUp Productivity Platform]。

    ![應用程式清單中的 ClickUp Productivity Platform 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者] 按鈕，然後在 [新增指派] 對話方塊中，選取 [使用者和群組]。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組] 對話方塊的 [使用者] 清單中，選取 [Britta Simon]，然後按一下畫面底部的 [選取] 按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色] 對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取] 按鈕。

7. 在 [新增指派] 對話方塊中，按一下 [指派] 按鈕。

### <a name="create-clickup-productivity-platform-test-user"></a>建立 ClickUp Productivity Platform 測試使用者

1. 在不同的網頁瀏覽器視窗中，以系統管理員身分登入您的 ClickUp Productivity Platform 租用戶。

2. 按一下 [使用者設定檔]，然後選取 [使用者]。

    ![ClickUp Productivity 設定](./media/clickup-productivity-platform-tutorial/user1.png)

3. 在文字方塊中輸入使用者的電子郵件地址，然後按一下 [邀請]。

    ![ClickUp Productivity 設定](./media/clickup-productivity-platform-tutorial/user2.png)

    > [!NOTE]
    > 使用者會收到通知，而且必須接受邀請才能啟用帳戶。

### <a name="test-single-sign-on"></a>測試單一登入

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [ClickUp Productivity Platform] 圖格時，應該會自動登入您設定 SSO 的 ClickUp Productivity Platform。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

