---
title: 使用 Azure 流量管理員設定加權循環配置資源流量路由方法 | Microsoft Docs
description: 此文章說明如何在流量管理員中使用循環配置資源方法對流量進行負載平衡
services: traffic-manager
documentationcenter: ''
author: kumudd
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: kumud
ms.openlocfilehash: ef39c09d4fc411937fdd6f4b5b5aec491efd0c5f
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2019
ms.locfileid: "57994556"
---
# <a name="configure-the-weighted-traffic-routing-method-in-traffic-manager"></a>在流量管理員中設定加權流量路由方法

常見的流量路由方法提供一組完全相同的端點 (包括雲端服務和網站)，並以均等的方式將流量傳送到每一個端點。 下列步驟概述如何設定這種類型的流量路由方法。

> [!NOTE]
> Azure Web 應用程式已為 Azure 區域 (可能包含多個資料中心) 內的網站提供循環配置資源負載平衡功能。 流量管理員可讓您將流量分散到位於不同資料中心的網站。

## <a name="to-configure-the-weighted-traffic-routing-method"></a>設定加權流量路由方法

1. 從瀏覽器登入 [Azure 入口網站](https://portal.azure.com)。 如果您沒有帳戶，您可以註冊[免費試用一個月](https://azure.microsoft.com/free/)。 
2. 在入口網站的搜尋列中，搜尋「流量管理員設定檔」，然後按一下您想要設定路由方法的設定檔名稱。
3. 在 [流量管理員設定檔] 刀鋒視窗中，確認您想要納入組態的雲端服務和網站皆存在。
4. 在 [設定] 區段中，按一下 [組態]，並在 [組態] 刀鋒視窗中，完成下列設定：
    1. 在 [流量路由方法設定] 中，確認流量路由方法為 [加權]。 如果不是，按一下下拉式清單中的 [加權]。
    2. 將此設定檔中的所有端點都設定為如下所示的相同「端點監視設定」：
        1. 選取適當的 [通訊協定]，並指定 [連接埠] 號碼。 
        2. 針對 [路徑]，輸入正斜線 */*。 若要監視端點，您必須指定路徑和檔案名稱。 正斜杠“/”是有效的相对路径条目，表示文件位于根目录（默认位置）中。
        3. 按一下頁面頂端的 [儲存]。
5. 測試組態中的變更，如下所示：
    1.  在入口網站的搜尋列中，搜尋流量管理員設定檔名稱，並按一下於結果中顯示的流量管理員設定檔。
    2.  在 [流量管理員設定檔] 刀鋒視窗中，按一下 [概觀]。
    3.  “流量管理器配置文件”边栏选项卡会显示新建的流量管理器配置文件的 DNS 名称。 這可由任何用戶端使用 (例如使用網頁瀏覽器進行瀏覽)，以路由傳送到由路由類型所決定的正確端點。 在此情況下，所有要求都會以循環配置資源方式路由到每個端點。
6. 在流量管理員設定檔運作後，您可以編輯授權 DNS 伺服器上的 DNS 記錄，以將您的公司網域名稱指向流量管理員網域名稱。

![使用流量管理員設定加權流量路由方法][1]

## <a name="next-steps"></a>後續步驟

- 深入了解[優先順序流量路由方法](traffic-manager-configure-priority-routing-method.md)。
- 深入了解[效能流量路由方法](traffic-manager-configure-performance-routing-method.md)。
- 深入了解[地理路由方法](traffic-manager-configure-geographic-routing-method.md)。
- 深入了解如何[測試流量管理員設定](traffic-manager-testing-settings.md)。

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png
