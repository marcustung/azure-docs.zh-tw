---
title: 影像分類：電腦視覺
titleSuffix: Azure Cognitive Services
description: 了解電腦視覺 API 的影像分類功能相關概念。
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 07fdaa22532f48cc39b6c524d85fdfe625f8b80c
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2019
ms.locfileid: "58337120"
---
# <a name="categorize-images"></a>將影像分類

除了標記和描述以外，電腦視覺也會傳回在影像中偵測到的分類型類別。 與標記不同，類別會在父/子階層結構中組織，並且更少 (86 個，而不是數以千計的標記)。 所有類別名稱皆採用英文。 類別可以單獨完成，也可以與較新的標記模型一起完成。

## <a name="the-86-category-concept"></a>86 類別概念

電腦視覺可以分類影像廣泛地或明確地說下, 圖中使用的 86 類別清單。 如需文字格式的完整分類，請參閱[類別分類](category-taxonomy.md)。

![類別分類法中所有類別的分組清單](./Images/analyze_categories-v2.png)

## <a name="image-categorization-examples"></a>影像分類範例

下列 JSON 回應說明根據視覺功能進行範例影像分類時，電腦視覺傳回的內容。

![公寓大樓屋頂上的女人](./Images/woman_roof.png)

```json
{
    "categories": [
        {
            "name": "people_",
            "score": 0.81640625
        }
    ],
    "requestId": "bae7f76a-1cc7-4479-8d29-48a694974705",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

下表所列的是一般的影像集和電腦視覺針對每個影像所傳回的類別。

| 映像 | 類別 |
|-------|----------|
| ![拍全家福的四個人](./Images/family_photo.png) | people_group |
| ![坐在草地上的小狗](./Images/cute_dog.png) | animal_dog |
| ![日落時站在山岩上的人](./Images/mountain_vista.png) | outdoor_mountain |
| ![桌上的一堆圓麵包](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>後續步驟

了解[標記影像](concept-tagging-images.md)和[描述影像](concept-describing-images.md)的相關概念。
