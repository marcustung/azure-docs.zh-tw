---
title: 使用自訂角色的 PIM-Azure Active Directory 中的 Azure 資源 |Microsoft Docs
description: 了解如何在 Azure AD Privileged Identity Management (PIM) 中使用適用於 Azure 資源的自訂角色。
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 13aef9b180a671a9b42bbc6319c487be36652093
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2019
ms.locfileid: "58574870"
---
# <a name="use-custom-roles-for-azure-resources-in-pim"></a>在 PIM 中使用適用於 Azure 資源的自訂角色

您可能需要某些角色的成員，以套用嚴格的 Azure Active Directory (Azure AD) Privileged Identity Management (PIM) 設定，同時為其他人提供較大的自主性。 假設您的組織雇用數名約聘夥伴，協助開發將在 Azure 訂用帳戶中執行的應用程式。

身為資源管理員的您，會希望員工能夠存取而不需要經過核准。 不過，當約聘夥伴要求存取組織資源時，全部都必須經過核准。

請依照下節概述的步驟操作，為 Azure 資源角色設置指定的 PIM 設定。

## <a name="create-the-custom-role"></a>建立自訂角色

若要建立資源適用的自訂角色，請依照[建立 Azure 角色型存取控制適用的自訂角色](../role-based-access-control-custom-roles.md)所述步驟操作。

建立自訂角色時請包含描述性名稱，如此方可輕易記住您打算複製的內建角色。

> [!NOTE]
> 確保自訂角色是您想複製的內建角色重複項，且其範圍符合內建角色。

## <a name="apply-pim-settings"></a>套用 PIM 設定

在 Azure 入口網站的租用戶中建立角色之後，請前往 [Privileged Identity Management - Azure 資源] 窗格。 選取角色適用的資源。

![「Privileged Identity Management - Azure 資源」窗格](media/azure-pim-resource-rbac/aadpim_manage_azure_resource_some_there.png)

[進行 PIM 角色設定](pim-resource-roles-configure-role-settings.md)，這些 PIM 角色設定應套用到該角色的成員。

最後，[將角色指派](pim-resource-roles-assign-roles.md)給您想要使用這些設定來鎖定的不同成員群組。

## <a name="next-steps"></a>後續步驟

- [在 PIM 中設定 Azure 資源角色設定](pim-resource-roles-configure-role-settings.md)
- [Azure 中的自訂角色](../../role-based-access-control/custom-roles.md)
