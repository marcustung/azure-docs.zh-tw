---
title: 快速入門：使用 Bing 拼字檢查 REST API 和 Node.js 進行拼字檢查
titlesuffix: Azure Cognitive Services
description: 開始使用 Bing 拼字檢查 REST API 來檢查拼字和文法。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 02/20/2019
ms.author: aahi
ms.openlocfilehash: 8e3379a086eb09745142f4e3997ed195eb4d1de5
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2019
ms.locfileid: "56885902"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-nodejs"></a>快速入門：使用 Bing 拼字檢查 REST API 和 Node.js 進行拼字檢查

使用本快速入門，第一次呼叫 Bing 拼字檢查 REST API。 這個簡單的 Python 應用程式會將要求傳送至 API，並傳回無法辨識的字組清單，後面接著建議的更正。 雖然此應用程式是以 Python 撰寫的，但 API 是一種與大多數程式設計語言都相容的 RESTful Web 服務。 您可以在 [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingSpellCheckv7.js) 上找到此應用程式的原始程式碼。

## <a name="prerequisites"></a>必要條件

* [Node.js 6](https://nodejs.org/en/download/) 或更新版本。

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]


## <a name="create-and-initialize-a-project"></a>建立專案並將其初始化

1. 在您最愛的 IDE 或編輯器中建立新的 JavaScript 檔案。 請設定嚴謹度，並要求使用 HTTPS。 然後，為您 API 端點的主機、路徑和訂用帳戶金鑰建立變數。

    ```javascript
    'use strict';
    let https = require ('https');
    
    let host = 'api.cognitive.microsoft.com';
    let path = '/bing/v7.0/spellcheck';
    let key = 'ENTER KEY HERE';
    ```

2. 為您的市場、拼字檢查模式，以及您要檢查的文字建立變數。 接著，建立將 `?mkt=` 參數附加至市場、並將 `&mode=` 附加至模式的字串。

    ```javascript
    let mkt = "en-US";
    let mode = "proof";
    let text = "Hollo, wrld!";
    let query_string = "?mkt=" + mkt + "&mode=" + mode;
    ```

## <a name="create-the-request-parameters"></a>建立要求參數

使用 `POST` 方法建立新物件，以建立您的要求參數。 藉由附加端點路徑來新增路徑，並查詢字串。 將訂用帳戶金鑰新增至 `Ocp-Apim-Subscription-Key` 標頭。

```javascript
let request_params = {
   method : 'POST',
   hostname : host,
   path : path + query_string,
   headers : {
   'Content-Type' : 'application/x-www-form-urlencoded',
   'Content-Length' : text.length + 5,
      'Ocp-Apim-Subscription-Key' : key,
   }
};
```

## <a name="create-a-response-handler"></a>建立回應處理常式

建立名為 `response_handler` 的函式以取得來自 API 的 JSON 回應，並加以列印。 建立回應本文的變數。 收到 `data` 旗標時，使用 `response.on()` 附加回應。 在收到 `end` 旗標時，將 JSON 主體列印至主控台。

```javascript
let response_handler = function (response) {
    let body = '';
    response.on ('data', function (d) {
        body += d;
    });
    response.on ('end', function () {
        console.log (body);
    });
    response.on ('error', function (e) {
        console.log ('Error: ' + e.message);
    });
};
```

## <a name="send-the-request"></a>傳送要求

使用 `https.request()` 並搭配您的要求參數和回應處理常式，來呼叫 API。 將您的文字寫入至 API，然後結束要求。

```javascript
let req = https.request (request_params, response_handler);
req.write ("text=" + text);
req.end ();
```

## <a name="example-json-response"></a>範例 JSON 回應

如以下範例所示，成功的回應會以 JSON 格式來傳回： 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [建立單頁 Web 應用程式](../tutorials/spellcheck.md)

- [什麼是 Bing 拼字檢查 API？](../overview.md)
- [Bing 拼字檢查 API v7 參考](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)
