---
title: 建立 Azure 容器映像技術資產 | Microsoft Docs
description: 建立 Azure 容器的技術資產。
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: pbutlerm
ms.openlocfilehash: 5a7531be73a872d9c088a0bf02a8686f947c220a
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/05/2019
ms.locfileid: "59047359"
---
# <a name="prepare-your-container-technical-assets"></a>準備容器技術資產

本文說明設定 Azure Marketplace 容器供應項目的步驟和需求。

## <a name="before-you-begin"></a>開始之前

檢閱 [Azure 容器執行個體](https://docs.microsoft.com/azure/container-instances)文件，其中提供快速入門、教學課程和範例。

## <a name="fundamental-technical-knowledge"></a>基本技術知識

設計、建置和測試這些資產需要時間，且需要具備關於 Azure 平台與建置供應項目所用技術的技術知識。
 
除了解決方案領域外，工程小組還應具備下列 Microsoft 技術的知識：

-   對於 [Azure 服務](https://azure.microsoft.com/services/)的基本了解 
-   如何[設計和架構 Azure 應用程式](https://azure.microsoft.com/solutions/architecture/)
-   具備 [Azure 虛擬機器](https://azure.microsoft.com/services/virtual-machines/)、[Azure 儲存體](https://azure.microsoft.com/services/?filter=storage)和 [Azure 網路](https://azure.microsoft.com/services/?filter=networking)的運用知識
-   具備 [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/) 的運用知識
-   具備 [JSON](https://www.json.org/) 的運用知識

## <a name="suggested-tools"></a>建議的工具

選擇下列其中一種指令碼環境，或兩者均使用，以協助管理容器映像：

-   [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
-   [Azure CLI](https://docs.microsoft.com/cli/azure)

此外也建議您新增下列工具至開發環境：

-   [Azure 儲存體總管](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
-   [Visual Studio Code](https://code.visualstudio.com/)
    *   擴充功能：[Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) \(英文\)
    *   擴充功能：[Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify) \(英文\)
    *   擴充功能：[Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

也建議您至 [Azure 開發人員工具](https://azure.microsoft.com/tools/) 頁面查看可用的工具，若您使用 Visual Studio，也請查看 [Visual Studio Marketplace](https://marketplace.visualstudio.com/)。

## <a name="create-the-container-image"></a>建立容器映像

- 建立和設定容器虛擬機器 (VM) 的虛擬硬碟 (VHD)。 此 VHD 包含容器的作業系統 (Windows、Linux 或 Ubuntu)。 可能需要額外的資料磁碟。
- 設定 VM 作業系統、VM 大小、要開放的連接埠，以及任何附加的資料磁碟。
- 安裝供應項目所需的應用程式和其他軟體。 例如：資料庫軟體、協力廠商軟體或自訂應用程式。

## <a name="next-steps"></a>後續步驟

[建立您的容器供應項目](./cpp-create-offer.md)
