---
title: SQL 資料倉儲中的透明資料加密 (入口網站) | Microsoft Docs
description: SQL 資料倉儲中的透明資料加密 (TDE)
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 477123ec6696ea44b1fea16689d7f1961bde4b61
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2019
ms.locfileid: "57840574"
---
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>開始使用 SQL 資料倉儲中的透明資料加密 (TDE)
> [!div class="op_single_selector"]
> * [安全性概觀](sql-data-warehouse-overview-manage-security.md)
> * [驗證](sql-data-warehouse-authentication.md)
> * [加密 (入口網站)](sql-data-warehouse-encryption-tde.md)
> * [加密 (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permissions"></a>必要權限
您必須是系統管理員或 dbmanager 角色的成員，才能啟用透明資料加密 (TDE)。

## <a name="enabling-encryption"></a>启用加密
若要啟用 SQL 資料倉儲的 TDE，請遵循下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com)
2. 在資料庫刀鋒視窗中，按一下 [設定]  按鈕
3. 选择“透明数据加密”选项 ![][1]
4. 選取 [開啟] 設定 ![][2]
5. 選取 [儲存]
   ![][3]  

## <a name="disabling-encryption"></a>停用加密
若要停用 SQL 資料倉儲的 TDE，請遵循下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com)
2. 在資料庫刀鋒視窗中，按一下 [設定]  按鈕
3. 選取 [透明資料加密]  選項 ![][1]
4. 選取 [關閉] 設定 ![][4]
5. 選取 [儲存]
   ![][5]  

## <a name="encryption-dmvs"></a>加密 DMV
可以使用下列 DMW 來確認加密：

* [sys.databases]
* [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.databases]: https://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
