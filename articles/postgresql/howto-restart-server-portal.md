---
title: 使用 Azure 入口網站重新啟動「適用於 PostgreSQL 的 Azure 資料庫」伺服器
description: 本文說明如何重新啟動 Azure Database for PostgreSQL 伺服器使用 Azure 入口網站。
author: ajlam
ms.author: andrela
ms.service: postgresql
ms.topic: conceptual
ms.date: 3/18/2019
ms.openlocfilehash: bf73120e462b740de5d2245f8a647896ac61f2c8
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2019
ms.locfileid: "58621817"
---
# <a name="restart-azure-database-for-postgresql-server-using-the-azure-portal"></a>重新啟動適用於 PostgreSQL 伺服器使用 Azure 入口網站的 Azure 資料庫
本主題說明如何重新啟動適用於 PostgreSQL 的 Azure 資料庫伺服器。 您可能會為了進行維護而需要重新啟動伺服器，進而在伺服器執行作業時導致短暫中斷。

如果服務忙碌中，系統會阻止伺服器重新啟動。 例如，該服務可能正在處理先前要求的作業，例如調整虛擬核心。
 
完成重新啟動所需的時間取決於 PostgreSQL 復原程序。 若要減少重新啟動時間，建議您先盡量減少伺服器上發生的活動數量，再進行重新啟動。

## <a name="prerequisites"></a>必要條件
若要完成本操作說明指南，您需要：
- [適用於 PostgreSQL 的 Azure 資料庫伺服器](quickstart-create-server-database-portal.md)

## <a name="perform-server-restart"></a>執行伺服器重新啟動

下列步驟會重新啟動 PostgreSQL 伺服器：

1. 在  [Azure 入口網站](https://portal.azure.com/)，選取適用於 postgresql 的 Azure 資料庫。

2. 在伺服器 [概觀] 頁面的工具列中，按一下 [重新啟動]。

   ![適用於 PostgreSQL 的 Azure 資料庫 - 概觀 - 重新啟動按鈕](./media/howto-restart-server-portal/2-server.png)

3. 按一下 [是] 以確認要重新啟動伺服器。

   ![適用於 PostgreSQL 的 Azure 資料庫 -重新啟動確認](./media/howto-restart-server-portal/3-restart-confirm.png)

4. 您會發現伺服器的狀態變更為「正在重新啟動」。

   ![適用於 PostgreSQL 的 Azure 資料庫 -重新啟動狀態](./media/howto-restart-server-portal/4-restarting-status.png)

5. 確認伺服器重新啟動已成功。

   ![適用於 PostgreSQL 的 Azure 資料庫 -重新啟動成功](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>後續步驟

深入了解[如何設定 Azure 資料庫中的參數，適用於 PostgreSQL](howto-configure-server-parameters-using-portal.md)