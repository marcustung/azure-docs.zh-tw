---
ms.openlocfilehash: 8ebb10f955be8f3004fdbdc595ea0fefc0d2b7ea
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632021"
---
## <a name="update-properties"></a>更新屬性

若要更新的錨點的屬性，您使用`UpdateAnchorProperties()`方法。 如果兩個或多個裝置會嘗試同時更新相同的錨點屬性，我們會使用開放式並行存取模型。 這表示第一次寫入會獲得優先權。  所有其他寫入將會取得 「 並行 」 錯誤： 屬性的重新整理就需要再試。
