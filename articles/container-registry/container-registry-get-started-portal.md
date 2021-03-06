---
title: 快速入門 - 在 Azure 中建立私人 Docker 登錄 - Azure 入口網站
description: 快速了解如何使用 Azure 入口網站建立私用的 Docker 容器登錄。
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: quickstart
ms.date: 01/22/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 93c22475a4043d1cbf5cb0ad7f9b134e8ac717cc
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2019
ms.locfileid: "55298410"
---
# <a name="quickstart-create-a-private-container-registry-using-the-azure-portal"></a>快速入門：使用 Azure 入口網站建立私人容器登錄

Azure Container Registry 是 Azure 中的私人 Docker 登錄，您可以在其中儲存並管理私人 Docker 容器映像。 在本快速入門中，您會使用 Azure 入口網站建立容器登錄。 然後，使用 Docker 命令將容器映像推送到登錄中，最後從您的登錄中提取映像並加以執行。

若要登入登錄以使用容器映像，您在進行此快速入門時必須執行 Azure CLI (建議使用 2.0.55 版或更新版本)。 執行 `az --version` 以尋找版本。 如果您需要安裝或升級，請參閱[安裝 Azure CLI][azure-cli]。

您也必須在本機上安裝 Docker。 Docker 提供可輕鬆在 [Mac][docker-mac]、[Windows][docker-windows] 或 [Linux][docker-linux] 系統上設定 Docker 的套件。

## <a name="sign-in-to-azure"></a>登入 Azure

在 https://portal.azure.com 登入 Azure 入口網站。

## <a name="create-a-container-registry"></a>建立容器登錄庫

選取 [建立資源] > [容器] > [容器登錄]。

![在 Azure 入口網站中建立容器登錄][qs-portal-01]

輸入 [登錄名稱] 和 [資源群組] 的值。 登錄名稱在 Azure 內必須是唯一的，且包含 5-50 個英數字元。 在此快速入門中，請在 `West US` 位置中建立名為 `myResourceGroup` 的新資源群組，並選取 [基本] 作為 [SKU]。 選取 [建立] 以部署 ACR 執行個體。

![在 Azure 入口網站中建立容器登錄][qs-portal-03]

您在本快速入門中會建立「基本」登錄，這是正在學習 Azure Container Registry 的開發人員所適用的成本最佳化選項。 如需可用服務層的詳細資訊，請參閱[容器登錄 SKU][container-registry-skus]。

當 [部署成功] 訊息出現時，請在入口網站中選取容器登錄。 

![Azure 入口網站中的容器登錄概觀][qs-portal-05]

記下 [登入伺服器] 的值。 您在後續的步驟中透過 Azure CLI 和 Docker 使用登錄時，將會用到此值。

## <a name="log-in-to-registry"></a>登入登錄

發送和提取容器映像之前，您必須登入 ACR 執行個體。 在您的作業系統中開啟命令殼層，並在 Azure CLI 中使用 [az acr login][az-acr-login] 命令。

```azurecli
az acr login --name <acrName>
```

完成後，此命令會傳回 `Login Succeeded`。 

[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

## <a name="list-container-images"></a>列出容器映像

若要列出您登錄中的映像，請瀏覽至入口網站中的登錄，然後選取 [存放庫]，以及選取您使用 `docker push` 建立的存放庫。

在此範例中，我們選取 **busybox** 存放庫，且可以在 **TAGS** 下方看到加上 `v1` 標記的映像。

![列出 Azure 入口網站中的容器映像][qs-portal-09]

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>清除資源

若要清除資源，請在入口網站中瀏覽至 **myResourceGroup** 資源群組。 載入資源群組後，按一下 [刪除資源群組] 以移除資源群組、容器登錄，以及儲存於該處的容器映像。

![在 Azure 入口網站中刪除資源群組][qs-portal-08]

## <a name="next-steps"></a>後續步驟

在本快速入門中，您已使用 Azure 入口網站建立 Azure Container Registry、推送容器映像，以及從登錄中提取映像並加以執行。 請繼續進行 Azure 容器登錄教學課程，以深入了解 ACR。

> [!div class="nextstepaction"]
> [Azure Container Registry 教學課程][container-registry-tutorial-quick-task]

<!-- IMAGES -->
[qs-portal-01]: ./media/container-registry-get-started-portal/qs-portal-01.png
[qs-portal-02]: ./media/container-registry-get-started-portal/qs-portal-02.png
[qs-portal-03]: ./media/container-registry-get-started-portal/qs-portal-03.png
[qs-portal-05]: ./media/container-registry-get-started-portal/qs-portal-05.png
[qs-portal-08]: ./media/container-registry-get-started-portal/qs-portal-08.png
[qs-portal-09]: ./media/container-registry-get-started-portal/qs-portal-09.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-login]: /cli/azure/acr#az-acr-login
