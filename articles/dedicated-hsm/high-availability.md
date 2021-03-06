---
title: 高可用性 - Azure 專用 HSM | Microsoft Docs
description: 「Azure 專用 HSM」高可用性範例和基本考量
services: dedicated-hsm
author: barclayn
manager: barbkess
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: barclayn
ms.openlocfilehash: 8a3698232c90d0705cb97677781ad512fffab803
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2019
ms.locfileid: "58539236"
---
# <a name="azure-dedicated-hsm-high-availability"></a>Azure 專用 HSM 高可用性

Azure 專用 HSM 的基礎是由 Microsoft 高可用性資料中心所構成。 但是，任何高可用性的資料中心都很容易受到當地故障的影響，而且在極端情況下，可能會出現區域層級的故障。 Microsoft 會將 HSM 裝置部署到同個區域內的不同資料中心，以確保不會因為佈建了多個裝置，而導致裝置共用到單一機架。 可藉由跨資料中心使用 Gemalto HA 群組功能區域中配對這些 Hsm 設定了額外的層級的高可用性。 也可以跨區域配對裝置，以便在需要災害復原的情況下，解決區域容錯移轉問題。 使用這種多層高可用性設定，即可自動應對任何裝置故障情形，保持應用程式正常運作。 所有資料中心在現場也會備有備用裝置和元件，因此可以及時更換任何故障的裝置。

## <a name="high-availability-example"></a>高可用性範例

如需進一步瞭解如何在軟體層級設定 HSM 裝置以實現高可用性，請參閱《Gemalto Luna 網路 HSM 管理指南》(英文)， 可至 [Gemalto 客戶支援入口網站](https://supportportal.gemalto.com/csm/)下載。

下圖顯示的是一個高可用性的架構。 此架構在同個區域中使用了多台裝置，且在獨立區域中有多台配對裝置。 此架構最少使用了四個 HSM 裝置和虛擬網路元件。

![高可用性圖表](media/high-availability/high-availability.png)

## <a name="next-steps"></a>後續步驟

建議在裝置佈建和應用程式設計或部署之前，先充分了解服務的所有重要概念，例如高可用性和安全性。
更多概念層級的主題：

* [部署架構](deployment-architecture.md) (英文)
* [實體安全性](physical-security.md)
* [網路功能](networking.md)
* [支援能力](supportability.md)
* [監視](monitoring.md)

如需 HSM 裝置高可用性設定方面的詳細資訓，請參閱《系統管理員指南》的〈Gemalto 客戶支援入口網站〉第 6 節。
