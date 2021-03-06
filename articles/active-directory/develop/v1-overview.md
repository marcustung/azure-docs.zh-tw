---
title: 關於 v1.0 | Azure
description: 本文提供使用 Azure Active Directory v1.0 端點及平台登入 Microsoft 工作和學校帳戶的概觀。
services: active-directory
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: dd39ee1dc22fb6143a8d098276172c453388170b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/13/2019
ms.locfileid: "56163983"
---
# <a name="about-v10"></a>關於 v1.0

Azure Active Directory (Azure AD) 是一項雲端識別服務，可讓開發人員使用 Microsoft 的公司或學校帳戶建置讓使用者安全登入的應用程式。 Azure AD 支援開發人員建置單一租用戶的企業營運 (LOB) 應用程式及開發多租用戶應用程式。 除了基本登入外，Azure AD 也可讓應用程式呼叫 [Microsoft Graph](https://docs.microsoft.com/graph/overview) 這類 Microsoft API，以及 Azure AD 平台上建置的自訂 API。 此文件說明如何使用業界標準通訊協定 (如 OAuth2.0 和 OpenID Connect)，將 Azure AD 支援新增至您的應用程式。

> [!NOTE]
> 此頁面上的內容大部分著重於 v1.0 端點及平台，此端點只支援 Microsoft 的公司或學校帳戶。 如果您想要登入消費者或個人的 Microsoft 帳戶，請參閱 [v2.0 端點及平台](v2-overview.md)上的資訊。 v2.0 端點會針對想要登入所有 Microsoft 身分識別的應用程式，提供統一的開發人員體驗。

| | |
| --- | --- |
|[驗證基本概念](authentication-scenarios.md) | 使用 Azure AD 進行驗證的簡介。 |
|[應用程式類型](app-types.md) | Azure AD 所支援的驗證案例概觀。 |
| | |

## <a name="get-started"></a>開始使用

v1.0 快速入門和教學課程會逐步引導您使用 Azure AD 驗證程式庫 (ADAL) SDK，在慣用平台上建置應用程式。 請參閱 [Microsoft 身分識別平台 (適用於開發人員的 Azure Active Directory)](index.yml) 中的 **v1.0 快速入門**和 **v1.0 教學課程**來開始著手。

## <a name="how-to-guides"></a>使用說明指南

請參閱 **v1.0 操作指南**，以取得 Azure AD 中最常見工作的詳細資訊和逐步解說。

## <a name="reference-topics"></a>參考主題

下列文章提供 Azure AD 中所用 API、通訊協定訊息和詞彙的詳細資訊。

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [驗證程式庫 (ADAL)](active-directory-authentication-libraries.md)   | Azure AD 所提供的程式庫和 SDK 概觀。 |
| [程式碼範例](sample-v1-code.md)                                  | 所有 Azure AD 程式碼範例的清單。 |
| [詞彙](developer-glossary.md)                                      | 本文件中使用的術語和文字定義。 |
|  |  |


[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
