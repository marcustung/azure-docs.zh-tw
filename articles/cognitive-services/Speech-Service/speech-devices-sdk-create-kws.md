---
title: 建立自訂的喚醒字 - 語音服務
titleSuffix: Azure Cognitive Services
description: 您的裝置隨時聆聽等候喚醒字 (或片語)。 當使用者說出喚醒字時，裝置便會將所有後續的音訊傳送到雲端，直到使用者停止說話。 自訂您的喚醒字是區隔裝置並強化品牌的有效方法。
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: b5ace2e741f900dd4ab7ba6518d0956284af35f6
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2019
ms.locfileid: "58498226"
---
# <a name="create-a-custom-wake-word-by-using-the-speech-service"></a>使用語音服務來建立自訂的喚醒字

您的裝置隨時聆聽等候喚醒字 (或片語)。 例如，「嗨 Cortana」是 Cortana 助理的喚醒字。 當使用者說出喚醒字時，裝置便會將所有後續的音訊傳送到雲端，直到使用者停止說話。 自訂您的喚醒字是區隔裝置並強化品牌的有效方法。

在本文中，您將了解如何為裝置建立自訂的喚醒字。

## <a name="choose-an-effective-wake-word"></a>選擇有效的喚醒字

在選擇喚醒字時，請考慮以下指導方針：

* 您的喚醒字應是英文單字或片語。 說出喚醒字所需的時間不應超過兩秒。

* 由 4 到 7 個音節所構成的單字，是效果最佳的喚醒字。 例如 "Hey, Computer" 是不錯的喚醒字。 只有 "Hey" 則不好。

* 喚醒字應遵循一般英文發音規則。

* 遵循一般英文發音規則的獨特或甚至虛構的單字可減少誤判的情況。 例如，"computerama" 會是好的喚醒字。

* 請勿選擇一般單字。 例如，"eat" 和 "go" 是一般對話中常說的單字。 它們可能會不小心觸發您的裝置。

* 避免使用可能有其他發音的喚醒字。 使用者將必須知道讓裝置回應的「正確」發音。 例如，"509" 可能會讀為 "five zero nine"、"five oh nine" 或 "five hundred and nine"。 "R.E.I." 可能會讀為 "r-e-i" 或 "ray"。 "Live" 可能會讀為"/līv/" 或 "/liv/"。

* 請勿使用特殊字元、符號或數字。 例如，"Go#" 和 "20 + cats" 不是好的喚醒字。 不過，"go sharp" 或 "twenty plus cats" 是可用的喚醒字。 您仍然可以使用品牌中的符號，並使用行銷管道和文件來強調適當的發音。

> [!NOTE]
> 如果您使用註冊商標單字作為喚醒字，請務必確認您擁有該商標，或者已從該商標擁有者取得使用該字組的權限。 Microsoft 無須對您選擇喚醒字而產生的任何法律問題負擔任何責任。

## <a name="create-your-wake-word"></a>建立您的喚醒字

您必須先使用「Microsoft 自訂喚醒字產生」服務來建立喚醒字，才能將自訂喚醒字和您的裝置搭配使用。 之後您提供網路喚醒 word，服務會產生，您將部署到您的開發套件，以啟用網路喚醒 word，您的裝置上的檔案。

1. 移至[客製化語音服務入口網站](https://cris.ai/)。

    ![自訂語音服務入口網站](media/speech-devices-sdk/wake-word-4.png)

1. 使用接收到 Azure Active Directory 的邀請電子郵件地址登入。

1. [Custom Wake Word] \(自訂喚醒字\) 頁面並未提供給大眾使用，因此沒有可直接連至該頁面的連結。 自訂語音功能需要 Azure 訂用帳戶，但自訂喚醒 Word 功能不會。 如果出現「找不到任何訂用帳戶」 錯誤頁面，請直接將 **"Subscriptions?errorMessage=No%20Subscriptions%20found"** 取代為 URL 中的 "**customkws**"，然後點按 ENTER。 URL 應該是以下其中一個： https://westus.cris.ai/customkws、 https://eastasia.cris.ai/customkws 或 https://northeurope.cris.ai/customkws，這取決於您的地區在哪。

1. 輸入您選擇的喚醒字，然後選擇 [提交字組]。

    ![輸入您的喚醒字](media/speech-devices-sdk/wake-word-5.png)

1. 產生檔案可能需要數分鐘。 您應該會在瀏覽器視窗中看到旋轉的圓圈。 隨後，會出現資訊列要求您下載 .zip 檔案。

1. 將 .zip 檔案儲存到您的電腦。 您需要此檔案來將自訂喚醒字部署到開發套件。 若要部署自訂喚醒字，請遵循[開始使用 Speech Devices SDK](speech-devices-sdk-qsg.md) 中的指示。

1. 選擇 [登出]。

## <a name="next-steps"></a>後續步驟

若要開始，請取得[免費的 Azure 帳戶](https://azure.microsoft.com/free/)並註冊 Speech Devices SDK。

> [!div class="nextstepaction"]
> [註冊 Speech Devices SDK](get-speech-devices-sdk.md)
