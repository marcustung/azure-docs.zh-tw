---
title: Azure 應用程式閘道的重新導向概觀
description: 了解 Azure 應用程式閘道的重新導向功能
services: application-gateway
documentationcenter: na
author: amsriva
manager: jpconnock
tags: azure-resource-manager
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/19/2018
ms.author: amsriva
ms.openlocfilehash: 8e88e0e11b3ccab7cc2c68b2617df2d588680780
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2019
ms.locfileid: "58170049"
---
# <a name="application-gateway-redirect-overview"></a>應用程式閘道重新導向概觀

您可以使用應用程式閘道重新導向流量。  它有泛型重新導向機制可用來將在另一個接聽程式或外部網站，有一個接聽程式接收的流量重新導向。 這可簡化應用程式組態、將資源使用量最佳化，並支援新的重新導向案例，包括全域和路徑式重新導向。

常見的重新導向案例，許多 web 應用程式，就是支援自動 HTTP 至 HTTPS 重新導向，以確保應用程式與使用者之間的所有通訊都都會透過加密的路徑。 在過去，客戶會使用一些技巧 (例如建立專屬後端集區)，其唯一目的是要將其在 HTTP 上接收的要求重新導向至 HTTPS。 應用程式閘道中的重新導向支援，您可以完成這只要將新的重新導向設定新增至路由的規則，並指定另一個接聽程式為目標接聽程式的 HTTPS 通訊協定。

支援下列類型的重新導向：

- 301 的永久重新導向
- 302 已找到
- 303，請參閱其他
- 307 暫時重新導向

應用程式閘道重新導向提供下列功能：

-  **全域重新導向**

   從閘道上的一個接聽程式到另一個接聽程式的重新導向。 這允許在網站上進行 HTTP 至 HTTPS 重新導向。
- **路徑式重新導向**

   這類型的重新導向只允許在特定網站區域上進行 HTTP 至 HTTPS 重新導向，例如以 /cart/* 表示的購物車區域。
- **重新導向至外部網站**

![重新導向](./media/redirect-overview/redirect.png)

有了這項變更，客戶必須建立新的重新導向組態物件，以指定重新導向所需的目標接聽程式或外部網站。 此組態元素也支援能將 URI 路徑和查詢字串附加至重新導向之 URL 的選項。 您也可以選擇重新導向的型別。 一旦建立，此重新導向組態會透過新規則附加至來源接聽程式。 若使用時基本規則，重新導向組態會與來源接聽程式相關聯並為全域重新導向。 使用路徑式規則時，就會在 URL 路徑對應上定義重新導向組態。 因此它只適用於網站的特定路徑區域。

### <a name="next-steps"></a>後續步驟

[在應用程式閘道上設定 URL 重新導向](tutorial-url-redirect-powershell.md)
