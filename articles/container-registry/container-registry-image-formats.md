---
title: Azure Container Registry 的內容格式
description: 了解 Azure Container Registry 中支援的內容格式。
services: container-registry
author: dlepow
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 10/31/2018
ms.author: danlep
ms.openlocfilehash: e7155604339bc634078fd022e05ede5f902bc0d8
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634824"
---
# <a name="content-formats-supported-in-azure-container-registry"></a>Azure Container Registry 中支援的內容格式

使用 Azure Container Registry 中的私人存放庫，可管理其下列中一種內容格式。 

## <a name="docker-compatible-container-images"></a>與 Docker 相容的容器映像

* [Docker 映像資訊清單 V2，結構描述 1](https://docs.docker.com/registry/spec/manifest-v2-1/)

* [Docker 映像資訊清單 V2，結構描述 2](https://docs.docker.com/registry/spec/manifest-v2-2/) -包含允許登錄將多平台映像儲存在單一 "image:tag" 參考下的資訊清單

* [Open Container Initiative (OCI) 映像格式規格](https://github.com/opencontainers/image-spec/blob/master/spec.md) 


## <a name="helm-charts"></a>Helm 圖表

Azure Container Registry 也可裝載 [Helm 圖表](https://helm.sh/)的存放庫，這是一種可用來為 Kubernetes 快速管理及部署應用程式的封裝格式。 支援 [Helm 用戶端](https://docs.helm.sh/using_helm/#installing-helm) 2.11.0 版或更新版本。

## <a name="next-steps"></a>後續步驟

* 了解如何使用 Azure Container Registry 來[推送和提取](container-registry-get-started-docker-cli.md)映像。

* 使用 [ACR 工作](container-registry-tasks-overview.md)建置和測試容器映像。 

* 使用 [Moby BuildKit](https://github.com/moby/buildkit) 建置和封裝 OCI 格式的容器。

* 設定裝載在 Azure Container Registry 中的 [Helm 存放庫](container-registry-helm-repos.md)。 


