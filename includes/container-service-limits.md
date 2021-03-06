---
title: 包含檔案
description: 包含檔案
services: container-service
author: dlepow
ms.service: container-service
ms.topic: include
ms.date: 10/11/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 62eb75ef18d3ac81be65783e57c21c0aefd7a429
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554777"
---
| 資源 | 預設限制 |
| --- | :--- |
| 最大的叢集，每個訂用帳戶 | 100 |
| 每個叢集的節點上限 | 100 |
| 每個節點的最大 pod:採用 Kubenet 的[基本網路功能][basic-networking] | 110 |
| 每個節點的最大 pod:[進階網路功能][ advanced-networking]與 Azure 容器網路介面 | Azure CLI 部署：30<sup>1</sup><br />Azure Resource Manager 範本：30<sup>1</sup><br />入口網站部署：30 |

<sup>1</sup>當您部署使用 Azure CLI 或 Resource Manager 範本在 Azure Kubernetes Service (AKS) 叢集時，這個值是可設定到 110 的 pod，每個節點。 您已經部署 AKS 叢集，或如果您使用 Azure 入口網站部署叢集，您無法設定每個節點的最大 pod。<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
