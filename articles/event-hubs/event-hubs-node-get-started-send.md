---
title: 使用 Node.js 來傳送事件 - Azure 事件中樞 | Microsoft Docs
description: 本文將逐步解說如何建立一個會從「Azure 事件中樞」傳送事件的 Node.js 應用程式。
services: event-hubs
author: spelluru
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 02/19/2019
ms.author: spelluru
ms.openlocfilehash: ec3182d11f1b2ffa31acd05fa1f2db695f3f2cf7
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447713"
---
# <a name="send-events-to-azure-event-hubs-using-nodejs"></a>使用 Node.js 將事件傳送到 Azure 事件中樞

Azure 事件中樞是巨量資料串流平台和事件擷取服務，每秒可接收和處理數百萬個事件。 事件中樞可以處理及儲存分散式軟體和裝置所產生的事件、資料或遙測。 傳送至事件中樞的資料可以透過任何即時分析提供者或批次/儲存體配接器來轉換和儲存。 如需事件中樞的詳細概觀，請參閱[事件中樞概觀](event-hubs-about.md)和[事件中樞功能](event-hubs-features.md)。

此教學課程說明如何將事件從以 Node.js 撰寫的應用程式傳送到事件中樞。

> [!NOTE]
> 您可以從 [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client) 下載此快速入門來作為範例，並以您事件中樞的值取代 `EventHubConnectionString` 和 `EventHubName` 字串，然後執行。 或者，您可以遵循本教學課程中的步驟，來建立自己的解決方案。

## <a name="prerequisites"></a>必要條件

若要完成本教學課程，您需要下列必要條件：

- Node.js 8.x 版和更新版本。 從 [https://nodejs.org](https://nodejs.org) 下載最新的 LTS 版本。
- Visual Studio Code (建議採用) 或任何其他的 IDE

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>建立事件中樞命名空間和事件中樞
第一個步驟是使用 [Azure 入口網站](https://portal.azure.com)來建立「事件中樞」類型的命名空間，然後取得您應用程式與「事件中樞」進行通訊所需的管理認證。 若要建立命名空間和「事件中樞」，請依照[這篇文章](event-hubs-create.md)中的程序操作，然後繼續進行此教學課程中的下列步驟。

遵循以下文章中的指示，取得事件中樞命名空間的連接字串：[取得連接字串](event-hubs-get-connection-string.md#get-connection-string-from-the-portal)。 您會在本教學課程中稍後用到此連接字串。

## <a name="clone-the-sample-git-repository"></a>複製範例 Git 存放庫
在您的電腦上，從 [GitHub](https://github.com/Azure/azure-event-hubs-node) 複製範例 Git 存放庫。 

## <a name="install-nodejs-package"></a>安裝 Node.js 套件
在您的電腦上，安裝適用於 Azure 事件中樞的 Node.js 套件。 

```shell
npm install @azure/event-hubs
```

## <a name="clone-the-git-repository"></a>複製 Git 存放庫
從 GitHub 下載或複製[範例](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples)。 

## <a name="send-events"></a>傳送事件
您已複製的 SDK 包含多個範例，其會示範如何使用 Node.js 將事件傳送到事件中樞。 在此快速入門中，您會使用 **simpleSender.js** 範例。 若要觀察所接收的事件，請開啟另一個終端機，並使用[接收範例](event-hubs-node-get-started-receive.md)來接收事件。

1. 在 Visual Studio Code 上開啟專案。 
2. 在 [用戶端] 資料夾下方建立名為 **.env** 的檔案。 從根資料夾的 **sample.env**，複製並貼上範例環境變數。
3. 設定您的事件中樞連接字串、事件中樞名稱和儲存體端點。 如需有關取得事件中樞之連接字串的指示，請參閱[取得連接字串](event-hubs-create.md#create-an-event-hubs-namespace)。
4. 在 Azure CLI 上，瀏覽至 [用戶端] 資料夾路徑。 執行下列命令來安裝節點套件並建置專案：

    ```shell
    npm i
    npm run build
    ```
5. 執行下列命令以開始傳送事件： 

    ```shell
    node dist/examples/simpleSender.js
    ```


## <a name="review-the-sample-code"></a>檢閱範例程式碼 
檢閱 simpleSender.js 檔案中的範例程式碼，以傳送事件到事件中樞。

```javascript
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
const lib_1 = require("../lib");
const dotenv = require("dotenv");
dotenv.config();
const connectionString = "EVENTHUB_CONNECTION_STRING";
const entityPath = "EVENTHUB_NAME";
const str = process.env[connectionString] || "";
const path = process.env[entityPath] || "";

async function main() {
    const client = lib_1.EventHubClient.createFromConnectionString(str, path);
    const data = {
        body: "Hello World!!"
    };
    const delivery = await client.send(data);
    console.log(">>> Sent the message successfully: ", delivery.tag.toString());
    console.log(delivery);
    console.log("Calling rhea-promise sender close directly. This should result in sender getting reconnected.");
    await Object.values(client._context.senders)[0]._sender.close();
    // await client.close();
}

main().catch((err) => {
    console.log("error: ", err);
});

```

請記得先設定您的環境變數，然後再執行指令碼。 您可以在命令列中設定它們 (如下列範例所示)，或使用 [dotenv 套件](https://www.npmjs.com/package/dotenv#dotenv) \(英文\)。 

```shell
// For windows
set EVENTHUB_CONNECTION_STRING="<your-connection-string>"
set EVENTHUB_NAME="<your-event-hub-name>"

// For linux or macos
export EVENTHUB_CONNECTION_STRING="<your-connection-string>"
export EVENTHUB_NAME="<your-event-hub-name>"
```

## <a name="next-steps"></a>後續步驟
在此快速入門中，您已使用 Node.js 將訊息傳送到事件中樞。 若要了解如何使用 Node.js 從事件中樞接收事件，請參閱[從事件中樞接收事件 - Node.js](event-hubs-node-get-started-receive.md)

在 [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples/) 上查看事件中樞的其他 Node.js 範例。
