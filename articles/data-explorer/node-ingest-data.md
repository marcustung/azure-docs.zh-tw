---
title: 快速入門：使用 Azure 資料總管 Node 程式庫擷取資料
description: 在本快速入門中，您將了解如何使用 Node.js 將資料擷取 (載入) 至 Azure 資料總管。
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 10/25/2018
ms.openlocfilehash: d530ed3710a3406e5cd8115d615e91d5056a5f8c
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/05/2019
ms.locfileid: "59047648"
---
# <a name="quickstart-ingest-data-using-the-azure-data-explorer-node-library"></a>快速入門：使用 Azure 資料總管 Node 程式庫擷取資料

Azure 資料總管是一項快速又可高度調整的資料探索服務，可用於處理記錄和遙測資料。 Azure 資料總管提供兩個適用於 Node 的用戶端程式庫：[擷取程式庫](https://github.com/Azure/azure-kusto-node/tree/master/azure-kusto-ingest)和[資料程式庫](https://github.com/Azure/azure-kusto-node/tree/master/azure-kusto-data)。 這些程式庫可讓您將資料內嵌 (載入) 至叢集，並從您的程式碼查詢資料。 在本快速入門中，您先在測試叢集中建立資料表和資料對應。 然後，您將叢集的擷取排入佇列並驗證結果。

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費 Azure 帳戶](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>必要條件

除了 Azure 訂用帳戶之外，您還需要下列項目，才能完成本快速入門：

* [測試叢集和資料庫](create-cluster-database-portal.md)

* 安裝在您開發電腦上的 [Node.js](https://nodejs.org/en/download/)

## <a name="install-the-data-and-ingest-libraries"></a>安裝資料並內嵌程式庫

安裝 *azure-kusto-ingest* 和 *azure-kusto-data*

```bash
npm i --save azure-kusto-ingest azure-kusto-data
```

## <a name="add-import-statements-and-constants"></a>新增 Import 陳述式和常數

從程式庫匯入類別

```javascript

const KustoConnectionStringBuilder = require("azure-kusto-data").KustoConnectionStringBuilder;
const KustoClient = require("azure-kusto-data").Client;
const KustoIngestClient = require("azure-kusto-ingest").IngestClient;
const IngestionProperties = require("azure-kusto-ingest").IngestionProperties;
const { DataFormat } = require("azure-kusto-ingest").IngestionPropertiesEnums;
const { BlobDescriptor } = require("azure-kusto-ingest").IngestionDescriptors;

```
若要驗證應用程式，Azure 資料總管會使用您的 Azure Active Directory 租用戶識別碼。 若要尋找您的租用戶識別碼，請遵循[尋找您的 Office 365 租用戶識別碼](https://docs.microsoft.com/onedrive/find-your-office-365-tenant-id)。

執行此程式碼之前，請先設定 `authorityId`、`kustoUri`、`kustoIngestUri` 和 `kustoDatabase` 的值。

```javascript
const cluster = "MyCluster";
const region = "westus";
const authorityId = "microsoft.com";
const kustoUri = `https://${cluster}.${region}.kusto.windows.net:443`;
const kustoIngestUri = `https://ingest-${cluster}.${region}.kusto.windows.net:443`;
const kustoDatabase  = "Weather";
```

現在來建構連接字串。 此範例使用服務驗證來存取叢集。 您也可以使用 Azure Active Directory 應用程式憑證、應用程式金鑰，以及 AAD 使用者和密碼。

您可以在稍後的步驟中建立目的地資料表和對應。

```javascript
const kcsbIngest = KustoConnectionStringBuilder.withAadDeviceAuthentication(kustoIngestUri, authorityId);
const kcsbData = KustoConnectionStringBuilder.withAadDeviceAuthentication(kustoUri, authorityId);
const destTable = "StormEvents";
const destTableMapping = "StormEvents_CSV_Mapping";
```

## <a name="set-source-file-information"></a>設定來源檔案資訊

匯入其他類別，並設定資料來源檔案的常數。 本範例使用裝載於 Azure Blob 儲存體的範例檔案。 **StormEvents** 範例資料集包含來自[美國國家環境資訊中心](https://www.ncdc.noaa.gov/stormevents/)的氣象相關資料。

```javascript
const container = "samplefiles";
const account = "kustosamplefiles";
const sas = "?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D";
const filePath = "StormEvents.csv";
const blobPath = `https://${account}.blob.core.windows.net/${container}/${filePath}${sas}`;
```

## <a name="create-a-table-on-your-test-cluster"></a>在測試叢集上建立資料表

建立與 `StormEvents.csv` 檔案中的資料結構描述相符的資料表。 此程式碼執行時，它會傳回類似以下的訊息：*若要登入，請使用網頁瀏覽器開啟頁面 https://microsoft.com/devicelogin，並輸入代碼 XXXXXXXXX 以進行驗證*。 請遵循下列步驟來登入，然後返回執行下一個程式碼區塊。 建立連線的後續程式碼區塊將需要您重新登入。

```javascript
const kustoClient = new KustoClient(kcsbData);
const createTableCommand = `.create table ${destTable} (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)`;

kustoClient.executeMgmt(kustoDatabase, createTableCommand, (err, results) => {
    console.log(results.primaryResults[0][0].toString());
});
```

## <a name="define-ingestion-mapping"></a>定義擷取對應

將傳入的 CSV 資料對應到建立資料表時使用的資料行名稱與資料類型。

```javascript
const createMappingCommand = `.create table ${destTable} ingestion csv mapping '${destTableMapping}' '[{"Name":"StartTime","datatype":"datetime","Ordinal":0}, {"Name":"EndTime","datatype":"datetime","Ordinal":1},{"Name":"EpisodeId","datatype":"int","Ordinal":2},{"Name":"EventId","datatype":"int","Ordinal":3},{"Name":"State","datatype":"string","Ordinal":4},{"Name":"EventType","datatype":"string","Ordinal":5},{"Name":"InjuriesDirect","datatype":"int","Ordinal":6},{"Name":"InjuriesIndirect","datatype":"int","Ordinal":7},{"Name":"DeathsDirect","datatype":"int","Ordinal":8},{"Name":"DeathsIndirect","datatype":"int","Ordinal":9},{"Name":"DamageProperty","datatype":"int","Ordinal":10},{"Name":"DamageCrops","datatype":"int","Ordinal":11},{"Name":"Source","datatype":"string","Ordinal":12},{"Name":"BeginLocation","datatype":"string","Ordinal":13},{"Name":"EndLocation","datatype":"string","Ordinal":14},{"Name":"BeginLat","datatype":"real","Ordinal":16},{"Name":"BeginLon","datatype":"real","Ordinal":17},{"Name":"EndLat","datatype":"real","Ordinal":18},{"Name":"EndLon","datatype":"real","Ordinal":19},{"Name":"EpisodeNarrative","datatype":"string","Ordinal":20},{"Name":"EventNarrative","datatype":"string","Ordinal":21},{"Name":"StormSummary","datatype":"dynamic","Ordinal":22}]'`;

kustoClient.executeMgmt(kustoDatabase, createMappingCommand, (err, results) => {
    console.log(results.primaryResults[0][0].toString());
});
```

## <a name="queue-a-message-for-ingestion"></a>將訊息排入佇列以供擷取

將訊息放入佇列，以從 Blob 儲存體提取資料，並將該資料內嵌至 Azure 資料總管。

```javascript
const defaultProps  = new IngestionProperties(kustoDatabase, destTable, DataFormat.csv, null,destTableMapping, {'ignoreFirstRecord': 'true'});
const ingestClient = new KustoIngestClient(kcsbIngest, defaultProps);
// All ingestion properties are documented here: https://docs.microsoft.com/azure/kusto/management/data-ingest#ingestion-properties

const blobDesc = new BlobDescriptor(blobPath, 10);
ingestClient.ingestFromBlob(blobDesc,null, (err) => {
    if (err) throw new Error(err);
});
```

## <a name="validate-that-table-contains-data"></a>驗證資料表包含資料

驗證已將資料擷取至資料表。 等待已排入佇列的擷取五到十分鐘，以排定將資料內嵌和載入至 Azure 資料總管。 然後執行下列程式碼，以取得 `StormEvents` 資料表中的記錄計數。

```javascript
const query = `${destTable} | count`;

kustoClient.execute(kustoDatabase, query, (err, results) => {
    if (err) throw new Error(err);  
    console.log(results.primaryResults[0][0].toString());
});
```

## <a name="run-troubleshooting-queries"></a>執行疑難排解查詢

登入 [https://dataexplorer.azure.com](https://dataexplorer.azure.com)，並連線至您的叢集。 在資料庫中執行下列命令，以查看最後四個小時是否有任何擷取失敗。 先取代資料庫名稱，再執行。
    
```Kusto
.show ingestion failures
| where FailedOn > ago(4h) and Database == "<DatabaseName>"
```

執行下列命令，以檢視最後四個小時內的所有擷取作業狀態。 先取代資料庫名稱，再執行。

```Kusto
.show operations
| where StartedOn > ago(4h) and Database == "<DatabaseName>" and Operation == "DataIngestPull"
| summarize arg_max(LastUpdatedOn, *) by OperationId
```

## <a name="clean-up-resources"></a>清除資源

如果您打算按照其他快速入門和教學課程繼續進行，請保留您建立的資源。 否則，請在資料庫中執行下列命令，來清除 `StormEvents` 資料表。

```Kusto
.drop table StormEvents
```

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [撰寫查詢](write-queries.md)
