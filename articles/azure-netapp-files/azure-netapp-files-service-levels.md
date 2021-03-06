---
title: Azure NetApp Files 的服務層級 | Microsoft Docs
description: 描述 Azure NetApp Files 之服務層級的輸送量效能。
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/14/2019
ms.author: b-juche
ms.openlocfilehash: c2086eb0c5529d8e570a545e35fc716f70c7541f
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/22/2019
ms.locfileid: "56668339"
---
# <a name="service-levels-for-azure-netapp-files"></a>Azure NetApp Files 的服務等級
Azure NetApp Files 支援兩個服務層級：「進階」和「標準」。 

## <a name="Premium"></a>進階儲存體

「進階」儲存體能提供最多每 TiB 64 MiB/秒的輸送量。 輸送量效能是針對磁碟區配額編製索引。 例如，來自進階儲存體並具有 2 TiB 已佈建配額 (無論實際耗用量為何) 的磁碟區，將具有 128 MiB/秒的輸送量。

## <a name="Standard"></a>標準儲存體

「標準」儲存體能提供最多每 TiB 16 MiB/秒的輸送量。 輸送量效能是針對磁碟區配額編製索引。 例如，來自標準儲存體並具有 2 TiB 已佈建配額 (無論實際耗用量為何) 的磁碟區，將具有 32 MiB/秒的輸送量。

## <a name="next-steps"></a>後續步驟

- 如需不同服務層級的價格，請參閱 [Azure NetApp Files 定價頁面](https://azure.microsoft.com/pricing/details/storage/netapp/)
- [設定容量集區](azure-netapp-files-set-up-capacity-pool.md)
