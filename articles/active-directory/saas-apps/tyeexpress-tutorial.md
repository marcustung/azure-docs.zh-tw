---
title: 教學課程：Azure Active Directory 與 T&E Express 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 T&E Express 之間的單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: B42374E5-2559-4309-8EF2-820BEE7EBB0C
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/07/2019
ms.author: jeedes
ms.openlocfilehash: d3459f7bcfc0e2e61cb55b38a05b7b6a21ea3e9e
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59283506"
---
# <a name="tutorial-azure-active-directory-integration-with-te-express"></a>教學課程：Azure Active Directory 與 T&E Express 整合

在本教學課程中，您將了解如何整合 T&E Express 與 Azure Active Directory (Azure AD)。
T&E Express 與 Azure AD 整合提供下列優點：

* 您可以在 Azure AD 中控制可存取 T&E Express 的人員。
* 您可以讓使用者使用其 Azure AD 帳戶自動登入 T&E Express (單一登入)。
* 您可以在 Azure 入口網站中集中管理您的帳戶。

若您想了解 SaaS app 與 Azure AD 整合的更多詳細資訊，請參閱 [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。
如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>必要條件

若要設定 Azure AD 與 T&E Express 整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure AD 環境，您可以在[這裡](https://azure.microsoft.com/pricing/free-trial/)取得一個月的試用帳戶
* 已啟用 T&E Express 單一登入的訂用帳戶

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入。

* T&E Express 支援由 **IDP** 起始的 SSO

## <a name="adding-te-express-from-the-gallery"></a>從資源庫新增 T&E Express

若要設定將 T&E Express 整合到 Azure AD 中，您需要從資源庫將 T&amp;amp;E Express 新增到受控 SaaS 應用程式清單。

**如要從資源庫新增 T&E Express，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，按一下 [Azure Active Directory] 圖示。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 瀏覽至 [企業應用程式]，然後選取 [所有應用程式] 選項。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請按一下對話方塊頂端的 [新增應用程式] 按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **T&E Express**，從結果面板中選取 [T&E Express]，然後按一下 [新增] 按鈕以新增應用程式。

    ![結果清單中的 T&E Express](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 **Britta Simon** 的測試使用者為基礎，設定及測試與 T&E Express 搭配運作的 Azure AD 單一登入。
若要讓單一登入能夠運作，必須建立 Azure AD 使用者與 T&E Express 中相關使用者之間的連結關聯性。

若要使用 T&E Express 來設定並測試 Azure AD 單一登入，您需要完成下列建置組塊：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** - 讓您的使用者能夠使用此功能。
2. **[設定 T&E Express 單一登入](#configure-te-express-single-sign-on)** - 在應用程式端設定單一登入設定。
3. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 Britta Simon 測試 Azure AD 單一登入。
4. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 Britta Simon 能夠使用 Azure AD 單一登入。
5. **[建立 T&E Express 測試使用者](#create-te-express-test-user)** - 使 T&E Express 中對應的 Britta Simon 連結到使用者在 Azure AD 中的代表項目。
6. **[測試單一登入](#test-single-sign-on)**，驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要設定與 T&E Express 搭配運作的 Azure AD 單一登入，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [T&E Express] 應用程式整合頁面上，選取 [單一登入]。

    ![設定單一登入連結](common/select-sso.png)

2. 在 [選取單一登入方法] 對話方塊中，選取 [SAML/WS-Fed] 模式以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

3. 在 [以 SAML 設定單一登入] 頁面上，按一下 [編輯] 圖示以開啟 [基本 SAML 設定] 對話方塊。

    ![編輯基本 SAML 組態](common/edit-urls.png)

4. 在 [以 SAML 設定單一登入] 頁面上，執行下列步驟：

    ![T&E Express 網域與 URL 單一登入資訊](common/idp-intiated.png)

    a. 在 [識別碼] 文字方塊中，使用下列模式來輸入作為 URL 的值： `https://<domain>.tyeexpress.com`

    b. 在 [回覆 URL] 文字方塊中，使用下列模式來輸入 URL： `https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`

    > [!NOTE]
    > 這些都不是真正的值。 請使用實際的識別碼和回覆 URL 更新這些值。 在此建議您在 [識別碼] 中使用唯一的字串值。 請連絡 [T&E Express 用戶端支援小組](http://www.tyeexpress.com/contacto.aspx)以取得這些值。 您也可以參考 Azure 入口網站中**基本 SAML 組態**區段所示的模式。

5. 在 [以 SAML 設定單一登入] 頁面的 [SAML 簽署憑證] 區段中按一下 [下載]，以依據您的需求從指定選項下載**同盟中繼資料 XML**，並儲存在您的電腦上。

    ![憑證下載連結](common/metadataxml.png)

6. 在 [設定 T&E Express] 區段上，依據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

    a. 登入 URL

    b. Azure AD 識別碼

    c. 登出 URL

### <a name="configure-te-express-single-sign-on"></a>設定 T&E Express 單一登入

1. 若要在 **T&E Express** 端設定單一登入，請使用系統管理員認證而不要使用 SAML 單一登入來登入 T&E Express 應用程式。

1. 在 [系統管理員] 索引標籤底下，按一下 [SAML 網域] 以開啟 [SAML 設定] 頁面。

    ![設定單一登入](./media/tyeexpress-tutorial/tye-SAML.png)

1. 將 [Activar (啟動)] 選項從 [No (否)] 改為選取 [SI (是)]。 在 [識別提供者中繼資料] 文字方塊中，貼上您從 Azure 入口網站下載的中繼資料 XML。

    ![設定單一登入](./media/tyeexpress-tutorial/tyeAdmin.png)

1. 按一下 [Guardar (儲存)] 按鈕以儲存設定。

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者

本節的目標是要在 Azure 入口網站中建立一個名為 Britta Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]、[使用者] 和 [所有使用者]。

    ![[使用者和群組] 與 [所有使用者] 連結](common/users.png)

2. 在畫面頂端選取 [新增使用者]。

    ![[新增使用者] 按鈕](common/new-user.png)

3. 在 [使用者] 屬性中，執行下列步驟。

    ![[使用者] 對話方塊](common/user-properties.png)

    a. 在 [名稱] 欄位中，輸入 **BrittaSimon**。
  
    b. 在 [使用者名稱] 欄位中，輸入 **brittasimon@yourcompanydomain.extension**  
    例如， BrittaSimon@contoso.com

    c. 選取 [顯示密碼] 核取方塊，然後記下 [密碼] 方塊中顯示的值。

    d. 按一下頁面底部的 [新增] 。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 T&E Express 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，依序選取 [企業應用程式]、[所有應用程式] 及 [T&E Express]。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [T&E Express]。

    ![應用程式清單中的 T&E Express 連結](common/all-applications.png)

3. 在左側功能表中，選取 [使用者和群組]。

    ![[使用者和群組] 連結](common/users-groups-blade.png)

4. 按一下 [新增使用者] 按鈕，然後在 [新增指派] 對話方塊中，選取 [使用者和群組]。

    ![[新增指派] 窗格](common/add-assign-user.png)

5. 在 [使用者和群組] 對話方塊的 [使用者] 清單中，選取 [Britta Simon]，然後按一下畫面底部的 [選取] 按鈕。

6. 如果您預期使用 SAML 判斷提示中的任何角色值，請在 [選取角色] 對話方塊的清單中選取適當使用者角色，然後按一下畫面底部的 [選取] 按鈕。

7. 在 [新增指派] 對話方塊中，按一下 [指派] 按鈕。

### <a name="create-te-express-test-user"></a>建立 T&E Express 測試使用者

若要讓 Azure AD 使用者可以登入 T&E Express，必須將他們佈建到 T&E Express。 T&E Express 需以手動方式佈建。

**若要佈建使用者帳戶，請執行下列步驟：**

1. 以系統管理員身分登入您的 T&E Express 公司網站。

1. 在 [系統管理員] 索引標籤底下，按一下 [使用者] 以開啟 [使用者] 主版頁面。

    ![新增員工](./media/tyeexpress-tutorial/tye-adminusers.png)

1. 在首頁上按一下 **+** 以新增使用者。

    ![新增員工](./media/tyeexpress-tutorial/tye-usershome.png)

1. 在表單中輸入所要求的所有必要詳細資料，然後按一下 [儲存] 按鈕來儲存詳細資料。

    ![新增員工](./media/tyeexpress-tutorial/tye-usersadd.png)

    ![新增員工](./media/tyeexpress-tutorial/tye-userssave.png)

### <a name="test-single-sign-on"></a>測試單一登入

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 T&E Express 圖格時，應該會自動登入您已設定 SSO 的 T&E Express。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

