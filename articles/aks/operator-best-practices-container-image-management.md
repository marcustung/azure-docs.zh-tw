---
title: 操作員最佳做法 - Azure Kubernetes Services (AKS) 中的容器映像管理
description: 了解叢集操作員在 Azure Kubernetes Service (AKS) 中管理和保護容器映像的最佳做法
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: iainfou
ms.openlocfilehash: 1cc91f55d3895f06176875cb9ae620685dc09a26
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2018
ms.locfileid: "53605546"
---
# <a name="best-practices-for-container-image-management-and-security-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Services (AKS) 中的容器映像管理與安全性最佳做法

當您在 Azure Kubernetes Service (AKS) 中開發和執行應用程式時，容器與容器映像的安全性是主要考量。 包含過期基底映像或未修補的應用程式執行階段的容器，易招致安全性風險和可能的攻擊媒介。 若要讓這些風險降至最低，您應該整合可在建置時和在執行階段掃描和修復容器中問題的工具。 越早發現流程中的弱點或過期的基底映像，叢集就越安全。 在本文中，「容器」表示容器登錄中儲存的容器映像與執行中的容器。

本文著重在如何保護 AKS 中的容器。 您會了解如何：

> [!div class="checklist"]
> * 掃描和修復映像弱點
> * 使用包含經過數位簽署容器映像之信任的登錄
> * 更新基底映像時，自動觸發和重新部署容器映像

您也可以閱讀適用於[叢集安全性][best-practices-cluster-security]與 [Pod 安全性][best-practices-pod-security]的最佳做法。

## <a name="secure-the-images-and-run-time"></a>保護映像與執行階段

**最佳做法指導方針** - 掃描容器映像的弱點，並僅部署通過驗證的映像。 定期更新基底映像與應用程式執行階段，然後重新部署 AKS 叢集中的工作負載。

採用容器型工作負載的其中一項考量，就是驗證用來建置您自己應用程式的映像與執行階段安全性。 如何確保您的部署不會招致安全性弱點？ 您的部署工作流程應包含使用 [Twistlock][twistlock] 或 [Aqua][aqua] 等工具掃描容器映像，然後僅允許部署經驗證映像的程序。

![掃描和修復容器映像、驗證及部署](media/operator-best-practices-container-security/scan-container-images-simplified.png)

在真實世界範例中，您可以使用持續整合與持續部署 (CI/CD) 管線，將映像掃描、驗證及部署自動化。 Azure Container Registry 包含這些弱點掃描功能。

## <a name="use-a-trusted-registry"></a>使用信任的登錄

**最佳做法指導方針** - 限制 Pod 與部署可以使用的映像登錄。 只允許信任的登錄，且您可驗證和控制從其中取得的映像。

為了增加安全性，您也能以數位方式簽署容器映像，就像是以數位方式簽署應用程式程式碼。 接著，只允許 AKS 部署已簽署的映像。 此程序可提供一層額外的安全性，限制 AKS 只能提取經過數位簽署並受您信任的映像，而不只是通過弱點檢查的映像。 您也要確定容器映像未遭到竄改並以名稱完全相同的映像取代。

提供經過數位簽署容器映像的信任登錄，可為您的環境提供複雜度，但可能需要符合特定原則或法規。 Azure Container Registry 支援使用信任的登錄與經簽署的映像。

如需經過數位簽署映像的詳細資訊，請參閱 [Azure Container Registry 中的內容信任][acr-content-trust]。

## <a name="automatically-build-new-images-on-base-image-update"></a>在基底映像更新時，自動建置新的映像

**最佳做法指導方針** - 當您使用基底映像作為應用程式映像時，在更新基底映像時，使用自動化功能建置新的映像。 這些基底映像通常包含安全性修正程式，請一併更新任何下游應用程式的容器映像。

每次更新基底映像時，也應該更新任何下游的容器映像。 此建置流程應與 [Azure Pipelines][azure-pipelines] 或 Jenkins 等的驗證與部署管線整合。 這些管線可確保應用程式會持續在更新的基底映像上執行。 驗證應用程式容器映像之後，可以更新 AKS 部署以執行最新且安全的映像。

Azure Container Registry 工作也可在更新基底映像時自動更新容器映像。 這項功能可讓您少量建置基底映像，並以 Bug 與安全性修正程式定期更新。

如需基底映像更新的詳細資訊，請參閱[使用 Azure Container Registry 工作在基底映像更新時自動執行映像建置][acr-base-image-update]。

## <a name="next-steps"></a>後續步驟

本文著重在如何保護您的容器。 若要實作這些部分的一些內容，請參閱下列文章：

* [使用 Azure Container Registry 工作在基底映像更新時自動執行映像建置][acr-base-image-update]
* [Azure Container Registry 中的內容信任][acr-content-trust]

<!-- EXTERNAL LINKS -->
[azure-pipelines]: /azure/devops/pipelines/?view=vsts
[twistlock]: https://www.twistlock.com/
[aqua]: https://www.aquasec.com/

<!-- INTERNAL LINKS -->
[best-practices-cluster-security]: operator-best-practices-cluster-security.md
[best-practices-pod-security]: developer-best-practices-pod-security.md
[acr-content-trust]: ../container-registry/container-registry-content-trust.md
[acr-base-image-update]: ../container-registry/container-registry-tutorial-base-image-update.md
