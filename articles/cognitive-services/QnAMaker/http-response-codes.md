---
title: API HTTP 回應碼 - QnA Maker
titleSuffix: Azure Cognitive Services
description: 了解 QnA Maker API 會傳回哪些 HTTP 回應碼。 這將協助您解決任何錯誤。
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 02/20/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 319623487f37308b5b8fe3fd107b01733825184d
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/20/2019
ms.locfileid: "56454713"
---
# <a name="qna-maker-api-http-response-codes"></a>QnA Maker API HTTP 回應碼
[管理](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)和預測 API 傳回 HTTP 回應碼。 當回應訊息包含專屬於要求的資訊時，HTTP 回應狀態碼為一般。 

### <a name="management"></a>管理性

|代碼|說明|
|:--|--|
|2xx|成功|
|400|要求的參數不正確，表示必要參數遺失、格式不正確或太大|
|400|要求的本文不正確，表示 JSON 遺失、格式不正確或太大|
|401|金鑰無效|
|403|禁止 - 您沒有正確的權限|
|404|KB 不存在|
|410|此 API 已被淘汰且不再提供|
