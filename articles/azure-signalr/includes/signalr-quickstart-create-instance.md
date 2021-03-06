---
title: 包含檔案
description: 包含檔案
author: anthonychu
ms.service: signalr
ms.topic: include
ms.date: 09/14/2018
ms.author: antchu
ms.custom: include file
ms.openlocfilehash: 15eded28e38279ea01bf019566d4fda5e7ac6c3e
ms.sourcegitcommit: dd1a9f38c69954f15ff5c166e456fda37ae1cdf2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/07/2019
ms.locfileid: "57588103"
---
## <a name="create-an-azure-signalr-service-instance"></a>建立 Azure SignalR 服務執行個體

您的應用程式將會連線至 Azure 中的 SignalR 服務執行個體。

1. 選取 Azure 入口網站左上角的 [新增] 按鈕。 在 [新增] 畫面的搜尋方塊中輸入 *SignalR 服務*並按 Enter。

    ![搜尋 SignalR 服務](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-new.png)

1. 從搜尋結果中選取 [SignalR 服務]，然後選取 [建立]。

1. 輸入下列設定。

    | 設定      | 建議的值  | 說明                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **資源名稱** | 全域唯一的名稱 | 能識別您的新 SignalR 服務執行個體的名稱。 有效字元是 `a-z`、`0-9` 和 `-`。  | 
    | **訂用帳戶** | 您的訂用帳戶 | 將在其下建立這個新 SignalR 服務執行個體的訂用帳戶。 | 
    | **[資源群組](../../azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | 要在其中建立 SignalR 服務執行個體之新資源群組的名稱。 | 
    | **位置** | 美國西部 | 選擇您附近的[區域](https://azure.microsoft.com/regions/)。 |
    | **定價層** | 免費 | 免費試用 Azure SignalR 服務。 |
    | **單位計數** |  不適用 | 單位計數會指出您的 SignalR 服務執行個體可接受的連線數目。 它只能在標準層中設定。 |

    ![建立 SignalR 服務](../media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-create.png)

1. 選取 [建立] 以開始部署 SignalR 服務執行個體。

1. 執行個體部署後，請在入口網站中加以開啟，並找出其 [設定] 頁面。 將 [服務模式] 設定變更為 [無伺服器]。

    ![SignalR Service 模式](../media/signalr-concept-azure-functions/signalr-service-mode.png)