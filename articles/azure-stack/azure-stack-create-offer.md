---
title: 在 Azure Stack 中建立供應項目 | Microsoft Docs
description: 身為雲端系統管理員，您應該了解如何在 Azure Stack 中為使用者建立供應項目。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/07/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 03/07/2019
ms.openlocfilehash: 59c69477e120facec1fbf132ce7017ca21aa8748
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2019
ms.locfileid: "58092611"
---
# <a name="create-an-offer-in-azure-stack"></a>在 Azure Stack 中建立優惠

[供應項目](azure-stack-key-features.md)是一組提供者供使用者購買或訂閱的一或多個方案。 此文說明如何建立一個包含[您所建立的方案](azure-stack-create-plan.md)的供應項目。 此供應項目可讓訂閱者設定虛擬機器 (VM)。

## <a name="create-an-offer-1902-and-later"></a>建立供應項目 (1902 和更新版本)

1. 登入 [Azure Stack 系統管理員入口網站](https://adminportal.local.azurestack.external)，然後依序選取 [+ 建立資源]、[供應項目 + 方案] 和 [供應項目]。

   ![建立供應項目](media/azure-stack-create-offer/offers.png)

2. 此時會出現索引標籤式使用者介面，讓您定義供應項目名稱，並新增現有的或建立新的基本方案和附加方案。 最重要的是，在決定要建立供應項目之前，您可以先檢閱其詳細資料。

   在 [基本] 索引標籤中的 [新增供應項目] 下，輸入 [顯示名稱] 與 [資源名稱]，然後在 [資源群組] 下選取 [新建] 或 [使用現有的]。 顯示名稱是易記的供應項目名稱。 此易記名稱是使用者在使用者入口網站中訂閱供應項目時唯一會看到的供應項目相關資訊。 請使用可協助使用者了解該供應項目附帶內容的直覺式名稱。 只有系統管理員可以看到資源名稱。 它是系統管理員用來處理其他供應項目 (以 Azure Resource Manager 資源方式) 的名稱。 在此索引標籤中，您也可以選擇要將此供應項目設為公用還是保持為私用 (這是預設值)。 您也可以稍後再[變更供應項目的公用或私用狀態](#change-the-state-of-an-offer)。

   ![新增供應項目](media/azure-stack-create-offer/new-offer.png)
  
3. 選取 [基本方案] 索引標籤。選取要包含在供應項目中的方案。

   ![選取方案](media/azure-stack-create-offer/select-plan.png)

4. 此時，您可以建立附加方案以修改基本方案，但此作業是選擇性的。 我們將在下一篇文章 ([Azure Stack 附加方案](create-add-on-plan.md)) 中建立附加方案。

5. 選取 [檢閱 + 建立] 索引標籤。檢閱供應項目摘要，以確保所有的值皆正確。 此介面可讓您在選擇的方案中逐一展開配額，以檢視方案中各項配額的詳細資料，並回頭進行任何必要的編輯。

6. 選取 [建立] 以建立供應項目。

   ![檢閱並建立](media/azure-stack-create-offer/review-offer.png)

### <a name="change-the-state-of-an-offer"></a>變更供應項目的狀態

建立供應項目之後，您可以變更其狀態。 供應項目必須設定為「公用」，使用者才能在訂閱時取得完整的檢視。 供應項目可以是：

- **公用**︰可讓使用者看見。
- **私用**：只有雲端系統管理員可以看到。 在草擬方案或供應項目時，或雲端系統管理員想要[為使用者建立訂用帳戶](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator)時，此設定會相當實用。
- **解除委任**：不開放給新的訂閱者。 雲端系統管理員可以解除委任供應項目，以避免之後的訂用帳戶使用，但不會影響目前的訂閱者。

  > [!TIP]  
  > 對供應項目所做的變更不會立即對使用者顯示。 若要查看變更，使用者可能必須先登出並再次登入使用者入口網站，才能看見新的供應項目。

有兩種方式可以變更供應項目的狀態：

1. 在 [所有資源] 中，選取供應項目的名稱。 在供應項目的 [概觀] 畫面上，選取 [變更狀態]。 選擇您想要使用的狀態 (例如，公用)。

   ![選擇狀態](media/azure-stack-create-offer/change-state.png)

2. 在 [所有資源] 中，選取供應項目的名稱。 然後，選取 [供應項目設定]。 選擇您要使用的狀態 (例如，公用)，然後選取 [儲存]。

   ![選取可存取性狀態](media/azure-stack-create-offer/offer-settings.png)

## <a name="create-an-offer-1901-and-earlier"></a>建立供應項目 (1901 和更早版本)

1. 登入 [Azure Stack 系統管理員入口網站](https://adminportal.local.azurestack.external)，然後依序選取 [+ 建立資源]、[租用戶供應項目 + 方案] 和 [供應項目]。

   ![建立供應項目](media/azure-stack-create-offer/image01.png)
  
2. 在 [新增供應項目] 下，輸入 [顯示名稱] 與 [資源名稱]，然後在 [資源群組] 下選取 [新建] 或 [使用現有的]。 顯示名稱是易記的供應項目名稱。 此易記名稱是使用者訂閱供應項目時唯一會看到的供應項目相關資訊。 請使用可協助使用者了解該供應項目附帶內容的直覺式名稱。 只有系統管理員可以看到資源名稱。 它是系統管理員用來處理其他供應項目 (以 Azure Resource Manager 資源方式) 的名稱。

   ![新增供應項目](media/azure-stack-create-offer/image01a.png)
  
3. 選取 [基本方案] 來開啟 [方案]。 選取要包含在供應項目中的方案，然後選擇 [選取]。 若要建立供應項目，請選取 [建立]。

   ![選取方案](media/azure-stack-create-offer/image02.png)
  
4. 建立供應項目之後，您可以變更其狀態。 供應項目必須設定為「公用」，使用者才能在訂閱時取得完整的檢視。 供應項目可以是：

   - **公用**︰可讓使用者看見。
   - **私用**：只有雲端系統管理員可以看到。 在草擬方案或供應項目時，或雲端系統管理員想要[為使用者建立訂用帳戶](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator)時，此設定會相當實用。
   - **解除委任**：不開放給新的訂閱者。 雲端系統管理員可以解除委任供應項目，以避免之後的訂用帳戶使用，但不會影響目前的訂閱者。

   > [!TIP]  
   > 對供應項目所做的變更不會立即對使用者顯示。 若要查看變更，使用者可能必須先登出並再次登入使用者入口網站，才能看見新的供應項目。

   在供應項目的 [概觀] 畫面上，選取 [可存取性狀態]。 選擇您想要使用的狀態 (例如，公用)，然後選取 [儲存]。

     ![選擇狀態](media/azure-stack-create-offer/change-stage-1807.png)

     或者，您可以選取 [變更狀態]，然後選擇狀態。

    ![選取可存取性狀態](media/azure-stack-create-offer/change-stage-select-1807.png)

> [!NOTE]
> 您也可以使用 PowerShell 來建立預設供應項目、方案和配額。 如需詳細資訊，請參閱 [Azure Stack PowerShell Module 1.4.0](/powershell/azure/azure-stack/overview?view=azurestackps-1.4.0)。

## <a name="next-steps"></a>後續步驟

- [建立訂用帳戶](azure-stack-subscribe-plan-provision-vm.md)
- [佈建虛擬機器](azure-stack-provision-vm.md)
