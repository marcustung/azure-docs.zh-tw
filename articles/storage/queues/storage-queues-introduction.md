---
title: Azure 佇列簡介 | Microsoft Docs
description: Azure 佇列簡介
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/06/2019
ms.author: tamram
ms.subservice: queues
ms.openlocfilehash: 2ae0d3993df54e1c9e5a9bf93619e8f9faa8a917
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873592"
---
# <a name="what-are-azure-queues"></a>什麼是 Azure 佇列？

Azure 佇列儲存體是一項儲存大量訊息的服務，全球任何地方都可利用 HTTP 或 HTTPS 並透過驗證的呼叫來存取這些訊息。 單一佇列訊息的大小上限為 64 KB，而一個佇列可以包含數百萬個訊息，以儲存體帳戶的總容量為限。

## <a name="common-uses"></a>常見用途

佇列儲存體的一般用途包括：

* 建立積存的工作供非同步處理
* 將訊息從 Azure Web 角色傳遞至 Azure 背景工作角色

## <a name="queue-service-concepts"></a>佇列服務概念

佇列服務包含下列元件：

![佇列概念](./media/storage-queues-introduction/queue1.png)

* **URL 格式：** 可利用下列 URL 格式來定址佇列：   
    https://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    下列 URL 可定址圖中的佇列：  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **儲存體帳戶：** 所有對 Azure 儲存體的存取都是透過儲存體帳戶進行。 如需關於儲存體帳戶容量的詳細資訊，請參閱＜ [Azure 儲存體延展性和效能目標](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) ＞(英文)。

* **佇列：** 佇列包含一組訊息。 所有訊息都必須放在佇列中。 請注意，佇列名稱必須是小寫。 如需為佇列命名的詳細資訊，請參閱 [為佇列和中繼資料命名](https://msdn.microsoft.com/library/azure/dd179349.aspx)。

* **訊息：** 大小上限為 64 KB 的訊息 (任何格式)。 訊息可保留在佇列中的時間上限為 7 天。

## <a name="next-steps"></a>後續步驟

* [建立儲存體帳戶](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [透過 .NET 開始使用佇列](storage-dotnet-how-to-use-queues.md)
