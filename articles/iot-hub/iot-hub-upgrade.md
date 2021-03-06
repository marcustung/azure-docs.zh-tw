---
title: 升級 Azure IoT 中樞 | Microsoft Docs
description: 變更「IoT 中樞」的定價與級別層，以取得更多傳訊和裝置管理功能。
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: robinsh
ms.openlocfilehash: 96c3a7b2cfda23f173f4caeff4fb7a92b1ddc438
ms.sourcegitcommit: e89b9a75e3710559a9d2c705801c306c4e3de16c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/15/2019
ms.locfileid: "59571272"
---
# <a name="how-to-upgrade-your-iot-hub"></a>如何升級 IoT 中樞

隨著您的 IoT 解決方案日漸成長，「Azure IoT 中樞」已準備好協助您擴大規模。 「Azure IoT 中樞」提供基本 (B) 和標準 (S) 兩個層級，可配合想要使用不同功能的使用者需求。 在每個層級內都有三個大小 (1、2 及 3)，用來決定每天可傳送的訊息數。

當您有更多裝置且需要更多功能時，有三種方式可調整 IoT 中樞以符合您的需求：

* 在 IoT 中樞內增加單位。 例如，在 B1 IoT 中樞內每增加一個單位可允許每天額外 400,000 則訊息。

* 變更 IoT 中樞的大小。 例如，从 B1 层迁移到 B2 层即可增加每个单元每天能够支持的消息数。

* 升級至較高層級。 例如，从 B1 层升级到 S1 层，消息传递容量不变，但可以使用高级功能。

這些變更全都可以在不中斷現有作業的情況下進行。

若要将 IoT 中心降级，可以删除单元以及缩小 IoT 中心的大小，但无法降级到较低的层。 例如，您可以從 S2 層移至 S1 層，但無法從 S2 層移至 B1 層。 只有一種[Iot 中樞版本](https://azure.microsoft.com/pricing/details/iot-hub/)層內可以選擇每個 IoT 中樞。 例如，您建立的 IoT 中樞可以具有多個 S1 單位，但不能具有來自不同版本 (例如 S1 和 B3 或 S1 和 S2) 的混合單位。

這些範例是用來協助您了解如何隨著解決方案的變更，以調整您的 IoT 中樞。 如需每一層的功能的特定資訊，您應該一律參考[Azure IoT 中樞價格](https://azure.microsoft.com/pricing/details/iot-hub/)。

## <a name="upgrade-your-existing-iot-hub"></a>升級現有的 IoT 中樞

1. 登入 [Azure 入口網站](https://portal.azure.com/)，然後瀏覽至 IoT 中樞。

2. 選取 [定價與級別]。

   ![定價與級別](./media/iot-hub-upgrade/pricing-scale.png)

3. 若要變更中樞的層級，請選取 [定價與級別層]。 選取新的層級，然後按一下 [選取]。

   ![定價與級別層](./media/iot-hub-upgrade/select-tier.png)

4. 若要變更您中樞內的單位數，請在 [IoT 中樞單位] 底下輸入新的值。

5. 選取 [儲存] 來儲存變更。

您的 IoT 中樞現已調整完成，而設定則未變更。

基本层 IoT 中心和标准层 IoT 中心的最大分区限制为 32。 大部分的 IoT 中樞只需要 4 個分割區。 分割區限制會在建立 IoT 中樞時選擇，就是裝置到雲端訊息數對同時閱讀這些訊息的人員數比例。 當您從基本層遷移到標準層時，這個值保持不變。

## <a name="next-steps"></a>後續步驟

取得有關[如何選擇適合的 IoT 中樞層](iot-hub-scaling.md)的更多詳細資料。