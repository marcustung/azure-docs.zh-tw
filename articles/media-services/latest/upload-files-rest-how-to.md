---
title: 使用 REST 將檔案上傳至 Azure 媒體服務帳戶 | Microsoft Docs
description: 了解如何建立並上傳資產，以將媒體內容移至媒體服務中。
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: a241f66adecbab1d0b1462f379d3765d6c1de252
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/11/2019
ms.locfileid: "55995385"
---
# <a name="upload-files-into-a-media-services-account-using-rest"></a>使用 REST 將檔案上傳至媒體服務帳戶

在媒體服務中，您會將數位檔案上傳到與資產相關聯的 Blob 容器。 [資產](https://docs.microsoft.com/rest/api/media/operations/asset)實體可以包含視訊、音訊、影像、縮圖集合、文字播放軌及隱藏式輔助字幕檔案 (以及這些檔案的相關中繼資料)。 一旦檔案會上傳到資產的容器，您的內容會安全地儲存在雲端，以便進行進一步的處理和串流。

本文說明如何使用 REST 上傳本機檔案。

## <a name="prerequisites"></a>必要條件

若要完成此主題中所述的步驟，您必須：

- 檢閱[資產概念](assets-concept.md)。
- [設定 Postman 以進行 Azure 媒體服務 REST API 呼叫](media-rest-apis-with-postman.md)。
    
    請務必遵循本[取得 Azure AD 權杖](media-rest-apis-with-postman.md#get-azure-ad-token)主題的最後一個步驟。 

## <a name="create-an-asset"></a>建立資產

本節說明如何建立新的資產。

1. 選取 [資產] -> [建立或更新資產]。
2. 按 [傳送]。

    ![建立資產](./media/upload-files/postman-create-asset.png)

您會看到 [回應]，包含新建資產的相關資訊。

## <a name="get-a-sas-url-with-read-write-permissions"></a>取得具有讀寫權限的 SAS URL 

本節說明如何取得針對已建立資產所產生的 SAS URL。 SAS URL 是以讀寫權限建立，可用來將數位檔案上傳到資產容器。

1. 選取 [資產] -> [列出資產 URL]。
2. 按 [傳送]。

    ![上傳檔案](./media/upload-files/postman-create-sas-locator.png)

您會看到 [回應]，包含資產的 URL 相關資訊。 複製第一個 URL 並用於上傳檔案。

## <a name="upload-a-file-to-blob-storage-using-the-upload-url"></a>使用上傳 URL 將檔案上傳至 blob 儲存體

使用 Azure 儲存體 API 或 SDK (例如 [儲存體 REST API](../../storage/common/storage-rest-api-auth.md)、[JAVA SDK](../../storage/blobs/storage-quickstart-blobs-java-v10.md) 或 [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md))。

## <a name="next-steps"></a>後續步驟

[教學課程：編碼以 URL 為基礎的遠端檔案及串流處理影片 - REST](stream-files-tutorial-with-rest.md)
