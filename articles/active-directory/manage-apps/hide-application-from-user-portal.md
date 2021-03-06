---
title: 從 Azure Active Directory 的使用者體驗中隱藏應用程式 | Microsoft Docs
description: 如何從 Azure Active Directory 存取面板或 Office 365 啟動器的使用者體驗中隱藏應用程式。
services: active-directory
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: celested
ms.reviewer: kasimpso
ms.collection: M365-identity-device-management
ms.openlocfilehash: 59c4fa0149b4a8f59dc4fa1e30936b35c1a5ac90
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/13/2019
ms.locfileid: "56191753"
---
# <a name="hide-applications-from-end-users-in-azure-active-directory"></a>對 Azure Active Directory 中的使用者隱藏應用程式

如何在使用者的 MyApps 面板或 Office 365 啟動器中隱藏應用程式的指示。 隱藏應用程式時，使用者仍有應用程式的權限。 

## <a name="prerequisites"></a>必要條件

需具備應用程式管理員權限，才能對 MyApps 面板和 Office 365 啟動器隱藏應用程式。

需具備全域管理員權限，才能隱藏所有的 Office 365 應用程式。


## <a name="hide-an-application-from-the-end-user"></a>對使用者隱藏應用程式
使用下列步驟，在 MyApps 存取面板和 Office 365 應用程式啟動器中隱藏應用程式。

1.  以目錄的全域管理員身分登入 [Azure 入口網站](https://portal.azure.com)。
2.  選取 **Azure Active Directory**。
3.  選取 [企業應用程式]。 [企業應用程式 - 所有應用程式] 刀鋒視窗隨即開啟。
4.  在 [應用程式類型] 下，選取 [企業應用程式]\(如果尚未選取)。
5.  搜尋您想要隱藏的庫應用程式，然後按一下該應用程式。  應用程式的概觀隨即開啟。
6.  按一下 [內容] 。 
7.  針對 [是否要向使用者顯示] 問題，按一下 [否]。
8.  按一下 [檔案] 。


## <a name="hide-office-365-applications-from-the-myapps-panel"></a>在使用者存取面板中隱藏 Office 365 應用程式

您可以使用下列步驟在 MyApps 面板中隱藏所有 Office 365 應用程式。 這些應用程式仍會顯示在 Office 365 入口網站中。

1.  以目錄的全域管理員身分登入 [Azure 入口網站](https://portal.azure.com)。
2.  選取 **Azure Active Directory**。
3.  選取 [使用者設定]。
4.  在 [企業應用程式] 之下，按一下 [管理終端使用者如何啟動及檢視其應用程式]。
5.  針對 [使用者只能在 Office 365 入口網站看到 Office 365 應用程式]，按一下 [是]。
6.  按一下 [檔案] 。


## <a name="next-steps"></a>後續步驟
* [查看我的所有群組](../fundamentals/active-directory-groups-view-azure-portal.md)
* [將使用者或群組指派給企業應用程式](assign-user-or-group-access-portal.md)
* [從企業應用程式中移除使用者或群組指派](remove-user-or-group-access-portal.md)
* [變更企業應用程式的名稱或標誌](change-name-or-logo-portal.md)

