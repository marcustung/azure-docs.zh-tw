---
title: 如何在 Azure API 管理中管理使用者帳戶 | Microsoft Docs
description: 了解如何在 Azure API 管理中建立或邀請使用者
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2018
ms.author: apimpm
ms.openlocfilehash: 6a4ad827c39ec106acdc7b52a5b769ab6e7febf8
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893940"
---
# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>如何在 Azure API 管理中管理使用者帳戶

在 API 管理中，開發人員是指您使用 API 管理所公開之 API 的使用者。 本指南演示如何创建和邀请开发人员使用通过 API 管理实例向其提供的 API 和产品。 如需以程式設計方式管理使用者帳戶的相關資訊，請參閱 [API 管理 REST](/rest/api/apimanagement/) 參考中的[使用者實體](https://docs.microsoft.com/en-us/rest/api/apimanagement/user)文件。

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="prerequisites"></a>必要條件

完成此文中的任务：[建立 Azure APIM 執行個體](get-started-create-service-instance.md)。

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-developer"> </a>建立新的開發人員

若要新增使用者，請遵循本節中的步驟：

1. 選取畫面左側的 [使用者] 索引標籤。
2. 按 [+新增]。
3. 輸入使用者的適當資訊。
4. 按 [新增] 。

    ![新增使用者](./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png)

依預設，新建立的開發人員帳戶為 [作用中]，且與 [開發人員] 群組相關聯。 處於「作用中」  狀態的開發人員帳戶可用來存取擁有訂用帳戶的所有 API。 若要將新建立的開發人員與其他群組建立關聯，請參閱 [如何將群組與開發人員建立關聯][How to associate groups with developers]。

## <a name="invite-developer"> </a>邀請開發人員
若要邀請開發人員，請遵循本節中的步驟：

1. 選取畫面左側的 [使用者] 索引標籤。
2. 按 [+邀請]。

這時會顯示確認訊息，但剛獲邀的開發人員在尚未接受邀請之前，都不會出現在清單中。 

邀請開發人員時，有一封電子郵件會傳送給開發人員。 此电子邮件使用模板生成并可自定义。 如需詳細資訊，請參閱 [設定電子郵件範本][Configure email templates]。

接受邀請時，帳戶就會變成作用中。

## <a name="block-developer"> </a> 停用或重新啟用開發人員帳戶

依預設，新建立或邀請的開發人員帳戶為「作用中」 。 若要停用開發人員帳戶，請按一下 [封鎖] 。 若要重新啟用已封鎖的開發人員帳戶，請按一下 [啟用] 。 已封鎖的開發人員帳戶無法存取開發人員入口網站或呼叫任何 API。 若要刪除使用者帳戶，請按一下 [刪除] 。

若要封鎖使用者，請遵循下列步驟。

1. 選取畫面左側的 [使用者] 索引標籤。
2. 按一下您要封鎖的使用者。
3. 按 [封鎖]。

## <a name="reset-a-user-password"></a>重設使用者密碼

若要以程式設計方式使用使用者帳戶，請參閱 [API 管理 REST](/rest/api/apimanagement/) 參考中的[使用者實體](https://docs.microsoft.com/en-us/rest/api/apimanagement/user)文件。 若要將使用者帳戶密碼重設為特定值，您可以使用 [更新使用者](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-user-entity#UpdateUser) 作業並指定所要的密碼。

## <a name="next-steps"> </a>後續步驟
建立開發人員帳戶之後，您可以將它與角色建立關聯，並讓它訂閱產品和 API。 如需詳細資訊，請參閱[如何建立和使用群組][How to create and use groups]。

[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png


[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
[Configure email templates]: api-management-howto-configure-notifications.md#email-templates
