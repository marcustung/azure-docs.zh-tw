---
title: Stream 壓縮音訊與 Speech SDK-語音服務
titleSuffix: Azure Cognitive Services
description: 了解如何將串流處理至 Azure 的語音服務與 Speech SDK 壓縮的音訊。 適用於C++， C#，和適用於 Linux 的 Java。
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: amishu
ms.openlocfilehash: 2066dc3e20ab9fc92b23fd071728ea6a920d3324
ms.sourcegitcommit: e43ea344c52b3a99235660960c1e747b9d6c990e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/04/2019
ms.locfileid: "59012499"
---
# <a name="stream-compressed-audio-with-the-speech-sdk"></a>Stream 壓縮音訊與 Speech SDK

語音 SDK**壓縮音訊輸入 Stream** API 可用來串流處理壓縮的音訊到語音服務使用 PullStream 或 PushStream。

> [!IMPORTANT]
> 壓縮的音訊資料流處理，才支援C++， C#，和在 Linux （Ubuntu 16.04 或 Ubuntu 18.04） 上的 Java。
> 支援僅限於 MP3 和這種門外漢只/OGG 項目。

## <a name="prerequisites"></a>必要條件

您必須安裝這些相依性，以壓縮的音訊輸入搭配適用於 Linux 的語音 SDK:

```sh
sudo apt install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly
```

## <a name="streaming-compressed-audio"></a>資料流的壓縮的音訊

若要串流處理以壓縮的音訊格式的語音服務，建立`PullAudioInputStream`或`PushAudioInputStream`。 接著，建立`AudioConfig`從您的資料流類別的執行個體，指定資料流的壓縮格式。

假設您有呼叫的輸入資料流類別`myPushStream`，而使用這種門外漢只/OGG。 這是程式碼可能如下： 

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;

var speechConfig = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

// Create an audio config specifying the compressed audio format and the instance of your input stream class.
var audioFormat = AudioStreamFormat.GetCompressedFormat(AudioStreamContainerFormat.OGG_OPUS);
var audioConfig = AudioConfig.FromStreamInput(myPushStream, audioFormat);

var recognizer = new SpeechRecognizer(speechConfig, audioConfig);

var result = await recognizer.RecognizeOnceAsync();

var text = result.GetText();
```

## <a name="next-steps"></a>後續步驟

* [取得語音試用版訂用帳戶](https://azure.microsoft.com/try/cognitive-services/)
* [了解如何辨識中的語音C#](quickstart-csharp-dotnet-windows.md)
