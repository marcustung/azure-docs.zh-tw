---
title: 為集區選取 VM 大小 - Azure Batch | Microsoft Docs
description: 如何為 Azure Batch 集區中的計算節點選取可用的 VM 大小
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 43094839c9da9b00c97d1dffd53f98a3acd119d5
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2019
ms.locfileid: "56417137"
---
# <a name="choose-a-vm-size-for-compute-nodes-in-an-azure-batch-pool"></a>為 Azure Batch 集區中的運算節點選擇 VM 大小

當您為 Azure Batch 集區選取節點大小時，可以從 Azure 中幾乎所有可用的 VM 大小進行選擇。 針對不同的工作負載，Azure 提供許多適用於 Linux 和 Windows VM 的大小。

選擇 VM 大小時有幾個例外狀況和限制：

* Batch 中不支援某些 VM 系列或 VM 大小。 
* 有些 VM 大小會有限制，而且必須特別啟用後才能配置。

## <a name="supported-vm-families-and-sizes"></a>支援的 VM 系列和大小

### <a name="pools-in-virtual-machine-configuration"></a>虛擬機器組態中的集區

虛擬機器組態中的 Batch 集區支援所有 VM 大小 ([Linux](../virtual-machines/linux/sizes.md)、[Windows](../virtual-machines/windows/sizes.md))，「除了」下列幾個：

| 系列  | 不支援的大小  |
|---------|---------|
| 基本 A 系列 | Basic_A0 (A0) |
| A 系列 | Standard_A0 |
| B 系列 | 全部 |
| DC 系列 | 全部 |
| 最高等級的記憶體最佳化 | 全部 |
| Hb 系列<sup>1、2</sup> | 全部 |
| Hc 系列<sup>1、2</sup> | 全部 |
| Lsv2 系列 | 全部 |
| NDv2 系列<sup>1、2</sup> | 全部 |
| NVv2 系列<sup>1</sup> | 全部 |
| SAP HANA | 全部 |


<sup>1</sup> 已針對支援進行規劃。  
<sup>2</sup> 可由處於使用者訂用帳戶模式的 Batch 帳戶使用；使用者訂用帳戶模式 Batch 帳戶需要設定核心配額。 如需更多資訊，請參閱[使用者訂用帳戶模式的組態](batch-account-create-portal.md#additional-configuration-for-user-subscription-mode)。

只有低優先順序節點才支援下列 VM 大小：

| 系列  | 支援的大小  |
|---------|---------|
| M 系列 | Standard_M64ms |
| M 系列 | Standard_M128s |

目前不支援 M 系列家族中的其他 VM 大小。

### <a name="pools-in-cloud-service-configuration"></a>雲端服務組態中的集區

雲端服務組態中的 Batch 集區支援所有[適用於雲端服務的 VM 大小](../cloud-services/cloud-services-sizes-specs.md)，「除了」下列幾個：

| 系列  | 不支援的大小  |
|---------|---------|
| A 系列 | 特小型 |
| Av2 系列 | Standard_A1_v2、Standard_A2_v2、Standard_A2m_v2 |

## <a name="restricted-vm-families"></a>受限制的 VM 系列

下列 VM 系列可在 Batch 集區中配置，但您必須要求特定的配額增加 (請參閱[本文](batch-quota-limit.md#increase-a-quota))：

* NCv2 系列
* NCv3 系列
* ND 系列

這些大小只可在虛擬機器組態的集區中使用。

## <a name="size-considerations"></a>大小考量

* **應用程式需求** - 請考量將於節點上執行之應用程式的特性和需求。 應用程式是否為多執行緒以及需要使用多少記憶體之類的層面，有助於決定最適合且具成本效益的節點大小。 針對多重執行個體的 [MPI 工作負載](batch-mpi.md)或 CUDA 應用程式，請考慮分別使用特殊 [HPC](../virtual-machines/linux/sizes-hpc.md) 或[已啟用 GPU](../virtual-machines/linux/sizes-gpu.md) 的 VM 大小。 (請參閱[在 Batch 集區中使用具備 RDMA 功能或已啟用 GPU 功能的執行個體](batch-pool-compute-intensive-sizes.md)。)

* **每個節點的工作** - 在選取節點大小時，通常會假設每次在節點上執行一項工作。 不過，在作業執行期間有多項工作 (因而有多個應用程式執行個體) 在計算節點上[以平行方式執行](batch-parallel-node-tasks.md)，也可能有好處。 在此情況下，通常會選擇多核心節點大小，以因應增加的平行工作執行需求。

* **不同工作的負載層級** - 集區中的所有節點都是相同大小。 如果您打算執行具有不同系統需求和/或負載層級的應用程式，建議使用不同的集區。 

* **區域可用性** - 在您用來建立 Batch 帳戶的區域中，可能不會提供某個 VM 系列或大小。 若要確認是否有提供某個大小，請參閱[依區域提供的產品](https://azure.microsoft.com/regions/services/)。

* **配額** - Batch 帳戶中的[核心配額](batch-quota-limit.md#resource-quotas)可能會限制您可以新增至 Batch 集區的指定大小節點數目。 若要要求增加配額，請參閱[本文](batch-quota-limit.md#increase-a-quota)。 

* **集區組態** - 一般來說，如果您在虛擬機器組態中建立集區，您可選擇的 VM 大小會比雲端服務組態多。

## <a name="next-steps"></a>後續步驟

* 如需 Batch 的深入概觀，請參閱[使用 Batch 開發大規模的平行計算解決方案](batch-api-basics.md)。
* 如需使用計算密集型 VM 大小的相關資訊，請參閱[在 Batch 集區中使用具備 RDMA 功能或已啟用 GPU 功能的執行個體](batch-pool-compute-intensive-sizes.md)。
