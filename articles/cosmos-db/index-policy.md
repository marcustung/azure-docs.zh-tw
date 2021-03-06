---
title: Azure Cosmos DB 索引編製原則
description: 了解 Azure Cosmos DB 中編製索引的運作方式。 了解如何設定編製索引原則，以自動編製索引並追求更高的效能。
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: rimman
ms.openlocfilehash: 6998db1679e67f8ac4bf7c81ea9373c66a9618ee
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59278559"
---
# <a name="index-policy-in-azure-cosmos-db"></a>Azure Cosmos DB 中的索引原則

您可以藉由設定下列參數，在 Azure Cosmos 容器上覆寫預設的編製索引原則：

* **在索引中包含或排除項目和路徑**：您可以排除或包含在索引中，特定的項目，當您插入或取代容器內的項目。 您也可以包含或排除要在容器之間編製索引的特定路徑/屬性。 路徑可能包含萬用字元模式，例如 *。

* **設定索引類型**：此外若要範圍索引的路徑，您可以新增其他類型的索引，這類空間。

* **設定索引模式**：藉由在容器上使用編製索引原則，您就能設定不同的編製索引模式，例如「一致」或「無」。

## <a name="indexing-modes"></a>編製索引模式

Azure Cosmos DB 支援兩個您可以透過編製索引原則的 Azure Cosmos 容器設定的編製索引模式：

* **一致**：如果 Azure Cosmos 容器的原則設定為*一致*，對特定容器的查詢會依照與針對讀數所指定相同的一致性層級 (例如強式、 限定過期、 工作階段或最終)。 

  當您更新項目時，即會同步更新索引。 例如，項目上的插入、取代、更新和刪除作業將導致索引更新。 一致的編製索引支援一致的查詢，但代價是會影響寫入輸送量。 減少寫入輸送量取決於 「 路徑包含在索引中 」 和 「 一致性層級 」。 一致的編製索引模式的設計在於，索引保留在最新的 所有更新，以及立即處理查詢。

* **無**：含有「無」索引模式的容器沒有任何與其相關聯的索引。 如果使用 Azure Cosmos 資料庫作為索引鍵值儲存體，且只能依據項目的識別碼屬性來存取它們，通常會使用此模式。

  > [!NOTE]
  > 設定索引編製模式，作為*無*會有副作用的卸除任何現有的索引。 如果您的存取模式只需要識別碼或自我連結，則應使用此選項。

查詢一致性層級的維護方式類似於一般讀取作業。 Azure Cosmos 資料庫會傳回錯誤，如果查詢具有的容器*無*編製索引模式。 您可以執行查詢，為透過明確的掃描**x-ms-documentdb-啟用-掃描**REST API 中的標頭或**EnableScanInQuery**要求使用.NET SDK 的選項。 **EnableScanInQuery** 目前不支援某些查詢功能 (例如 ORDER BY)，因為它們會授權對應的索引。

## <a name="modifying-the-indexing-policy"></a>修改編製索引原則

在 Azure Cosmos DB 中，您隨時都可更新容器的編製索引原則。 在 Azure Cosmos 容器上的編製索引原則中的變更可能會導致索引圖形變更。 此變更會影響可編製索引的路徑、其精確度，以及索引本身的一致性模型。 變更編製索引原則時，需要將舊的索引有效率地轉換成新的索引。

### <a name="index-transformations"></a>索引轉換

所有的索引轉換都會在線上執行。 每個新原則有效率地轉換每個舊原則編製索引的項目不會影響寫入可用性或容器上佈建的輸送量。 讀取和寫入作業由使用 REST API，Sdk，或使用預存程序和觸發程序不會影響在索引轉換期間的一致性。

變更索引編製原則是非同步作業，並完成作業的時間取決於項目、 佈建的輸送量和項目大小的數目。 重新編製索引正在進行時，您的查詢可能不會傳回所有符合的結果，如果查詢使用被修改，索引和查詢不會傳回任何錯誤/失敗。 重新編製索引正在進行時，查詢最終是一致不論編製索引模式組態。 在索引轉換完成後，您將繼續看見一致的結果。 這適用於透過 REST API、SDK 之類的介面，或預存程序和觸發程序所發出的查詢。 索引轉換是在背景中，使用適用於特定複本的備用資源複本上以非同步方式執行。

所有的索引轉換均會就地執行。 Azure Cosmos DB 不會維護兩份索引。 因此，進行索引轉換時，您的容器中不需要也不會取用任何額外的磁碟空間。

當您變更編製索引原則時，變更會套用到從舊索引移至新的索引，而主要會依據編制索引模式組態。 相較於包含/排除的路徑、索引種類及精確度之類的其他屬性，編製索引模式設定扮演了主要角色。

如果舊的和新的編製索引原則都使用**一致**的編製索引，Azure Cosmos 資料庫就會線上執行索引轉換。 您無法在轉換進行時，使用一致的編製索引模式套用另一個編製索引原則變更。 當您移至「無」編製索引模式時，就會立即卸除索引。 當您想要取消進行中的轉換，並重新開始其他編製索引原則時，移至「無」相當實用。

## <a name="modifying-the-indexing-policy---examples"></a>修改編製索引原則 - 範例

以下是當您想要更新的索引編製原則的最常見的使用案例：

* 如果您想要的一般作業期間，會有一致的結果，而是改為使用**無**大量資料匯入期間的索引編製模式。

* 如果您想要在目前的 Azure Cosmos 容器上開始使用編製索引功能。 例如，您可以使用地理空間查詢或 ORDER BY/字串範圍查詢，這兩種查詢分別需要空間索引種類及字串範圍索引種類。

* 如果您想要以手動方式選取要編製索引的屬性，並隨著時間變更它們來適應您的工作負載。

* 如果您想要調整編製索引精確度，以改善查詢效能或減少所取用的儲存體。

## <a name="next-steps"></a>後續步驟

在下列文章中深入了解編製索引：

* [編製索引的概觀](index-overview.md)
* [索引類型](index-types.md)
* [索引路徑](index-paths.md)
* [如何管理編製索引原則](how-to-manage-indexing-policy.md)
