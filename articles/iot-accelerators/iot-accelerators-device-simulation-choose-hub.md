---
title: 使用現有的 IoT 中樞搭配裝置模擬解決方案 - Azure | Microsoft Docs
description: 本文說明如何設定「裝置模擬」解決方案加速器以使用現有的 IoT 中樞。
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: 38cde750ce07741a433baa1b8607a584f94ad9b1
ms.sourcegitcommit: 3dcb1a3993e51963954194ba2a5e42260d0be258
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "50753911"
---
# <a name="use-an-existing-iot-hub-with-the-device-simulation-solution-accelerator"></a>使用現有的 IoT 中樞搭配裝置模擬解決方案加速器

當您部署裝置模擬時，您也可以選擇部署 IoT 中樞，以在模擬中使用。 此選項會部署 [S2 層 IoT 中樞與單一縮放單位](../iot-hub/iot-hub-scaling.md)。 如果您部署這個選擇性的 IoT 中樞，您仍然可以選擇以另一個 IoT 中樞為目標進行模擬。

如果您選擇不部署選擇性的 IoT 中樞，則必須使用自己的中樞進行任何模擬。

如果您沒有 IoT 中樞，一律可以從 [Azure 入口網站](https://portal.azure.com)建立一個新的 IoT 中樞。

若要使用預先存在的 IoT 中樞，您需要 **iothubowner** 共用存取原則的連接字串。 您可以從 [Azure 入口網站](https://portal.azure.com)取得此連接字串：

1. 在入口網站中的中樞設定頁面上，按一下 [共用存取原則]。

1. 按一下 [iothubowner]。

1. 複製主要連接字串或次要連接字串。

[![取得連接字串](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-inline.png)](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-expanded.png#lightbox)

使用您設定模擬時所複製的連接字串：

![設定模擬](./media/iot-accelerators-device-simulation-choose-hub/configuresimulation.png)

### <a name="next-steps"></a>後續步驟

在本操作指南中，了解如何在模擬中使用現有的 IoT 中樞。 接著，您可能想要了解如何為模擬[建立進階裝置型號](iot-accelerators-device-simulation-advanced-device.md)。
