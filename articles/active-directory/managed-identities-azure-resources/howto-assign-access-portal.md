---
title: 如何使用 Azure 入口網站為受控識別指派對 Azure 資源的存取權
description: 以下逐步指示說明如何使用 Azure 入口網站為一項資源的受控識別指派對另一項資源的存取權。
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 958b3d72a3a8df4a3b67f62e7db788d7142ca667
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2019
ms.locfileid: "58445477"
---
# <a name="assign-a-managed-identity-access-to-a-resource-by-using-the-azure-portal"></a>使用 Azure 入口網站為受控識別指派對資源的存取權

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

以受控識別設定 Azure 資源後，您可以為此受控識別提供對另一項資源的存取權，就像任何安全性主體一樣。 本文說明如何使用 Azure 入口網站為 Azure 虛擬機器或虛擬機器擴展集的受控識別提供對 Azure 儲存體帳戶的存取權。

## <a name="prerequisites"></a>必要條件

- 如果您不熟悉 Azure 資源的受控識別，請參閱[概觀一節](overview.md)。 **請務必檢閱[系統指派和使用者指派受控識別之間的差異](overview.md#how-does-it-work)**。
- 如果您還沒有 Azure 帳戶，請先[註冊免費帳戶](https://azure.microsoft.com/free/)，再繼續進行。

## <a name="use-rbac-to-assign-a-managed-identity-access-to-another-resource"></a>使用 RBAC 為受控識別指派對其他資源的存取權

您在 Azure 資源 ([例如 Azure VM](qs-configure-portal-windows-vm.md) 或 [Azure VMSS](qs-configure-portal-windows-vmss.md)) 上啟用受控識別之後：

1. 使用與已設定受控識別的 Azure 訂用帳戶相關聯的帳戶，登入 [Azure 入口網站](https://portal.azure.com)。

2. 瀏覽至您要修改其存取控制的資源。 在此範例中，我們會為 Azure 虛擬機器提供對儲存體帳戶的存取權，因此我們瀏覽至儲存體帳戶。

3. 選取資源的 [存取控制 (IAM)] 頁面，然後選取 [+ 新增角色指派]。 接著，指定 [角色]、[存取權指派對象]，並指定對應的 [訂用帳戶]。 在搜尋條件區域中，您應該會看到資源。 選取資源，然後選取 [儲存]。 

   ![存取控制 (IAM) 螢幕擷取畫面](./media/msi-howto-assign-access-portal/assign-access-control-iam-blade-before.png)  
     
## <a name="next-steps"></a>後續步驟

- [適用於 Azure 資源的受控識別概觀](overview.md)
- 若要在 Azure 虛擬機器上啟用受控識別，請參閱[使用 Azure 入口網站在 VM 上設定 Azure 資源的受控識別](qs-configure-portal-windows-vm.md)。
- 若要在 Azure 虛擬機器擴展集上啟用受控識別，請參閱[使用 Azure 入口網站在虛擬機器擴展集上設定 Azure 資源的受控識別](qs-configure-portal-windows-vmss.md)。


