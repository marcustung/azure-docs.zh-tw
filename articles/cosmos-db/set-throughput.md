---
title: 在 Azure Cosmos 容器和資料庫上佈建輸送量
description: 了解如何設定 Azure Cosmos 容器和資料庫的佈建輸送量。
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/31/2019
ms.author: rimman
ms.openlocfilehash: 1f8bec6fbf0bce9a3ac272231058a96a5d9e84cc
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762492"
---
# <a name="provision-throughput-on-containers-and-databases"></a>在容器和資料庫中佈建輸送量

Azure Cosmos 資料庫是一組容器的管理單位。 資料庫是由一組無從驗證結構描述的容器所組成的。 Azure Cosmos 容器是適用於輸送量和儲存體的延展性單位。 容器會跨 Azure 區域內的一組機器進行水平分割，並散發到與您 Azure Cosmos 帳戶相關聯的所有 Azure 區域。

使用 Azure Cosmos DB，您可以佈建輸送量在兩個資料粒度：
 
- Azure Cosmos 容器
- Azure Cosmos 資料庫

## <a name="set-throughput-on-a-container"></a>在容器上設定輸送量  

在 Azure Cosmos 容器上佈建的輸送量是專為保留該容器。 該容器隨時都可接收佈建輸送量。 容器上的佈建輸送量在財務方面會由 SLA 所支援。 若要了解如何在容器上設定的輸送量，請參閱[在 Azure Cosmos 容器上的佈建輸送量](how-to-provision-container-throughput.md)。

設定容器上佈建的輸送量是最常使用的選項。 使用佈建任何數量的輸送量，您可以依據調整容器輸送量[要求單位 (Ru)](request-units.md)。 

Azure Cosmos 容器上佈建的輸送量會統一散發到容器的所有邏輯分割區。 您無法選擇性地指定邏輯磁碟分割的輸送量。 因為實體分割區會裝載容器的一或多個邏輯分割區；因此，實體分割區專屬於該容器並支援該容器上佈建的輸送量。 

如果邏輯磁碟分割上執行的工作負載的消耗超過已配置給該邏輯磁碟分割的輸送量，您的作業會取得速率限制。 速率限制發生時，您可以增加整個容器的佈建的輸送量或重試作業。 如需有關分割的詳細資訊，請參閱[邏輯分割區](partition-data.md)。

當您想要保證容器的效能時，我們建議您在容器資料粒度上設定輸送量。

下圖示範實體分割區如何裝載容器的一或多個邏輯分割區：

![實體分割區](./media/set-throughput/resource-partition.png)

## <a name="set-throughput-on-a-database"></a>在資料庫上設定輸送量

當您在 Azure Cosmos 資料庫上佈建輸送量時，輸送量會在資料庫中的所有容器上共用。 如果您在資料庫中的特定容器指定佈建的輸送量時，例外狀況。 共用其容器之間的資料庫層級佈建的輸送量相當於裝載電腦叢集上的資料庫。 因為資料庫共用內的所有容器都會共用機器上可用的資源，因此，您自然不會取得任何特定容器上的可預測效能。 若要了解如何設定資料庫的佈建的輸送量，請參閱[設定佈建的輸送量，Azure Cosmos 資料庫](how-to-provision-database-throughput.md)。

Azure Cosmos 資料庫上的設定輸送量保證，您會收到佈建的輸送量，該資料庫的所有時間。 因為資料庫共用內的所有容器都會共用佈建輸送量，因此，Azure Cosmos DB 不會針對該資料庫中的特定容器提供任何可預測的輸送量保證。 特定容器可接收的輸送量部分取決於：

* 容器數目。
* 對於各種容器所選擇的分割區索引鍵。
* 散發於容器中各種不同邏輯分割區的工作負載。 

當您想要在多個容器之間共用輸送量，但不想讓輸送量專屬於任何特定容器時，我們建議您在資料庫上設定輸送量。 

以下是最好在資料庫層級佈建輸送量的範例：

* 在一組容器上共用資料庫的佈建輸送量非常適用於多租用戶應用程式。 每位使用者都可透過不同的 Azure Cosmos 容器來表示。

* 當您將從 VM 叢集或從內部部署實體伺服器裝載的 NoSQL 資料庫 (例如 MongoDB 或 Cassandra) 移轉到 Azure Cosmos DB 時，在一組容器之間共用資料庫的佈建輸送量就很實用。 將 Azure Cosmos 資料庫上設定的佈建輸送量想像為 MongoDB 或 Cassandra 叢集計算容量之佈建輸送量的邏輯對等項目 (但更符合成本效益且更具彈性)。  

必須使用建立佈建輸送量在資料庫內建立的所有容器[分割區索引鍵](partition-data.md)。 在任何指定的時間點，配置給資料庫內容器的輸送量都會散發到該容器的所有邏輯分割區。 當您有容器的共用資料庫上所設定的佈建的輸送量時，您無法選擇性地將輸送量套用至特定容器或邏輯分割區。 

如果邏輯分割區上的工作負載所取用的輸送量超過配置給特定邏輯分割區的輸送量時，您的作業將會受到速率限制。 速率限制發生時，您可以增加整個資料庫的輸送量或重試作業。 如需有關分割的詳細資訊，請參閱[邏輯分割區](partition-data.md)。

單一實體分割區可以裝載多個邏輯分割區屬於不同的容器共用的資料庫佈建的輸送量。 容器的單一邏輯分割區的範圍一律是種實體分割區內，雖然 *"L"* 間的邏輯資料分割 *"C"* 可以是容器的共用資料庫的佈建的輸送量對應，以及裝載於 *"R"* 實體分割區。 

下圖顯示實體分割區如何裝載一或多個屬於資料庫內不同容器的邏輯分割區：

![實體分割區](./media/set-throughput/resource-partition2.png)

## <a name="set-throughput-on-a-database-and-a-container"></a>在資料庫和容器上設定輸送量

您可以結合這兩個模型， 就能同時在資料庫和容器上佈建輸送量。 下列範例說明如何在 Azure Cosmos 資料庫和容器上佈建輸送量：

* 您可以建立名為 Azure Cosmos 資料庫*Z*的佈建輸送量 *"K"* Ru。 
* 接下來，建立名為的五個容器*A*， *B*， *C*， *D*，以及*E*資料庫內。
* 您可以明確地設定 *"P"* Ru 的佈建的輸送量，名為 「 容器*B*。
* *"K"* Ru 的輸送量會在四個容器之間共用*A*， *C*， *D*，以及*E*。可用的輸送量的確切量*A*， *C*， *D*，或*E*而有所不同。 而且沒有適用於每個個別容器輸送量的 SLA。
* 命名的容器*B*保證能夠取得 *"P"* Ru 的輸送量所有時間。 並受到 SLA 支援。

## <a name="update-throughput-on-a-database-or-a-container"></a>更新資料庫或容器的輸送量

建立 Azure Cosmos 容器或資料庫之後，您可以更新佈建的輸送量。 最大佈建的輸送量，您可以設定資料庫或容器上沒有任何限制。 最小佈建的輸送量是根據下列因素而定： 

* 您曾經在容器中所儲存的最大資料大小
* 您就以往在容器佈建的輸送量最大值
* 您會在資料庫中，以共用的輸送量來建立的 Azure Cosmos 容器的數目上限。 

您可以藉由使用 Sdk 以程式設計方式擷取的容器或資料庫的最小輸送量，或在 Azure 入口網站中檢視的值。 使用.NET SDK 中，當[DocumentClient.ReplaceOfferAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replaceofferasync?view=azure-dotnet)方法可讓您向佈建的輸送量值。 使用 Java SDK 中，當[RequestOptions.setOfferThroughput](sql-api-java-samples.md#offer-examples)方法可讓您向佈建的輸送量值。 

使用.NET SDK 中，當[DocumentClient.ReadOfferAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readofferasync?view=azure-dotnet)方法可讓您擷取的容器或資料庫的最小輸送量。 

您可以隨時調整的容器或資料庫佈建的輸送量。 您可以執行為 4 小時的閒置時間之後相應減少作業。 閒置的時間會定義為時間週期時有任何供應項目對容器或資料庫 （其中包含相應增加和相應減少） 取代作業。 

## <a name="comparison-of-models"></a>模型的比較

|**參數**  |**資料庫上佈建的輸送量**  |**容器上佈建的輸送量**|
|---------|---------|---------|
|RU 數目下限 |400 (在前四個容器之後，每個額外的容器至少需要每秒 100 RU)。 |400|
|每個容器的 RU 數目下限|100|400|
|取用 1 GB 儲存體所需的 RU 數目下限|40|40|
|RU 數目上限|在資料庫上無限制。|在容器上無限制。|
|指派給或適用於特定容器的 RU|不提供保證。 指派給指定容器的 RU 視屬性而定。 屬性可以選擇是共用輸送量之容器的資料分割索引鍵、工作負載散發，以及容器的數目。 |設定於容器上的所有 RU 都是專為該容器保留的。|
|容器的儲存體上限|無限制。|無限制。|
|容器中每個邏輯分割區的輸送量上限|10K RU|10K RU|
|容器中每個邏輯分割區的儲存體上限 (資料 + 索引)|10 GB|10 GB|

## <a name="next-steps"></a>後續步驟

* 深入了解[邏輯分割區](partition-data.md)。
* 了解如何[在 Azure Cosmos 容器上佈建輸送量](how-to-provision-container-throughput.md)。
* 了解如何[在 Azure Cosmos 資料庫上佈建輸送量](how-to-provision-database-throughput.md)。

