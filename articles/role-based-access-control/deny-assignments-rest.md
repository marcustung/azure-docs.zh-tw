---
title: 使用 REST API 列出 Azure 資源的拒絕指派 - Azure | Microsoft Docs
description: 了解如何使用適用於 Azure 資源的角色型存取控制 (RBAC) 和 REST API，來列出使用者、群組和應用程式的拒絕指派。
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 59bcf2b33d203ae216b4965b963a727a6b34ae72
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2019
ms.locfileid: "57998401"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-rest-api"></a>使用 REST API 列出 Azure 資源的拒絕指派

[拒絕指派](deny-assignments.md)會封鎖使用者執行特定的 Azure 資源動作，即使角色指派授予他們存取權也一樣。 這篇文章說明如何使用 REST API，以清單拒絕指派。

> [!NOTE]
> 在此階段中，您也可以加入自己的唯一方式會拒絕指派是使用 Azure 藍圖。 如需詳細資訊，請參閱 <<c0> [ 保護新的資源，Azure 藍圖資源鎖定](../governance/blueprints/tutorials/protect-new-resources.md)。

## <a name="prerequisites"></a>必要條件

若要取得拒絕指派的相關資訊，您必須具備：

- `Microsoft.Authorization/denyAssignments/read` 權限，隨附於多數[適用於 Azure 資源的內建角色](built-in-roles.md)。

## <a name="list-a-single-deny-assignment"></a>列出單一拒絕指派

1. 從下列要求著手：

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments/{deny-assignment-id}?api-version=2018-07-01-preview
    ```

1. 在 URI 中，將 *{scope}* 取代為您想要列出拒絕指派的範圍。

    | 影響範圍 | 類型 |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | 訂用帳戶 |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | 資源群組 |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | 資源 |

1. 將 *{deny-assignment-id}* 取代為您想要擷取的拒絕指派識別碼。

## <a name="list-multiple-deny-assignments"></a>列出多個拒絕指派

1. 從下列其中一個要求開始：

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview
    ```

    使用選擇性參數：

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. 在 URI 中，將 *{scope}* 取代為您想要列出拒絕指派的範圍。

    | 影響範圍 | 類型 |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | 訂用帳戶 |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | 資源群組 |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | 資源 |

1. 將 *{filter}* 取代為您想要套用來篩選拒絕指派清單的條件。

    | Filter | 描述 |
    | --- | --- |
    | (無篩選條件) | 列出在指定範圍內、之上或之下的所有拒絕指派。 |
    | `$filter=atScope()` | 僅列出在指定範圍內和之上的拒絕指派。 不包含子範圍內的拒絕指派。 |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | 列出具有指定名稱的拒絕指派。 |

## <a name="list-deny-assignments-at-the-root-scope-"></a>列出根範圍 (/) 內的拒絕指派

1. 以[提高 Azure Active Directory 中全域管理員的存取權](elevate-access-global-admin.md)中所述的方式來提高您的存取權。

1. 使用下列要求：

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. 將 *{filter}* 取代為您想要套用來篩選拒絕指派清單的條件。 需要篩選條件。

    | Filter | 描述 |
    | --- | --- |
    | `$filter=atScope()` | 僅列出根範圍的拒絕指派。 不包含子範圍內的拒絕指派。 |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | 列出具有指定名稱的拒絕指派。 |

1. 移除已提高的存取權。

## <a name="next-steps"></a>後續步驟

- [了解 Azure 資源的拒絕指派](deny-assignments.md)
- [提高 Azure Active Directory 中全域管理員的存取權](elevate-access-global-admin.md)
- [Azure REST API 參考](/rest/api/azure/)
