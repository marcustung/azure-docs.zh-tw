---
title: 讓雲端服務提供者能夠管理您的 Azure Stack 訂用帳戶 | Microsoft Docs
description: 讓服務提供者能夠存取 Azure Stack 中的訂用帳戶。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2019
ms.author: sethm
ms.reviewer: alfredop
ms.lastreviewed: 01/19/2019
ms.openlocfilehash: 5d7398853e20702aef450e2532784f0dca7aac57
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2019
ms.locfileid: "55246597"
---
# <a name="enable-a-cloud-service-provider-to-manage-your-azure-stack-subscription"></a>讓雲端服務提供者能夠管理您的 Azure Stack 訂用帳戶

*適用於：Azure Stack 整合式系統*

如果您透過雲端服務提供者 (CSP) 使用 Azure Stack，您可以選擇管理您自己的訂用帳戶，以存取 Azure 和 Azure Stack 中的資源。 您也可以讓提供者為您管理訂用帳戶。 本文將說明如何：

* 讓服務提供者可存取您的訂用帳戶。
* 確定服務提供者可管理您的服務。

> [!NOTE]
> 如果 CSP 並未管理您的帳戶，而且您略過下列步驟，則 CSP 無法為您管理 Azure Stack 訂用帳戶。

## <a name="manage-your-subscription-with-a-cloud-service-provider"></a>透過雲端服務提供者管理您的訂用帳戶

將 CSP 新增為您訂用帳戶的**使用者**。

1. 在您的租用戶目錄中將 CSP 新增為具有使用者角色的來賓使用者。 如需新增使用者的步驟，請參閱[將新的使用者新增至 Azure Active Directory](../../active-directory/add-users-azure-active-directory.md)
2. CSP 會為您建立本機 Azure Stack 訂用帳戶。 您已準備好開始使用 Azure Stack。
3. 您的 CSP 應該會在您的訂用帳戶中建立資源，以確認它們也可以管理您的資源。 例如，他們可以[使用 Azure Stack 入口網站建立 Windows 虛擬機器](azure-stack-quick-windows-portal.md)。

## <a name="enable-the-cloud-service-provider-to-manage-your-subscription-using-rbac-rights"></a>讓雲端服務提供者能夠使用 RBAC 權限來管理您的訂用帳戶

將 CSP 新增為您訂用帳戶的**擁有者**。

1. 在您的租用戶目錄中將 CSP 新增為來賓使用者。 如需新增使用者的步驟，請參閱[將新的使用者新增至 Azure Active Directory](../../active-directory/add-users-azure-active-directory.md)
2. 將**擁有者**角色新增至 CSP 來賓使用者。 如需將 CSP 使用者新增至訂用帳戶的步驟，請參閱[使用角色型存取控制來管理 Azure 訂用帳戶資源的存取](../../role-based-access-control/role-assignments-portal.md)。 CSP 會為您建立本機 Azure Stack 訂用帳戶。 您已準備好開始使用 Azure Stack。
3. 您的 CSP 應該會在您的訂用帳戶中建立資源，以確認它們可以管理您的資源。

## <a name="next-steps"></a>後續步驟

* 若要深入了解如何取出 Azure Stack 的資源使用量資訊，請參閱 [Azure Stack 中的使用量與帳單](../azure-stack-billing-and-chargeback.md)。
