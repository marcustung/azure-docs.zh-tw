---
title: 快速入門 - 將 Docker 容器部署至 Azure 容器執行個體 - 入口網站
description: 在本快速入門中，您可以使用 Azure 入口網站快速地部署在隔離式 Azure 容器執行個體中執行的容器化的 Web 應用程式
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: f4d232d4d6043ede3979db67e5cd35130d931bef
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369440"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-the-azure-portal"></a>快速入門：使用 Azure 入口網站部署容器執行個體

使用 Azure 容器執行個體在 Azure 中簡潔且快速地執行無伺服器 Docker 容器。 當您不需要像 Azure Kubernetes Service 的完整容器協調流程平台時，請視需要將應用程式部署至容器執行個體。

在本快速入門中，您會使用 Azure 入口網站部署隔離式 Docker 容器，並使用完整網域名稱 (FQDN) 讓其應用程式可供使用。 在設定一些設定並部署容器之後，您可以瀏覽至執行中的應用程式：

![在瀏覽器中檢視部署至 Azure Container Instances 的應用程式][aci-portal-07]

## <a name="sign-in-to-azure"></a>登入 Azure

在 https://portal.azure.com 登入 Azure 入口網站。

如果您沒有 Azure 訂用帳戶，請在開始前建立 [[免費帳戶]][azure-free-account]。

## <a name="create-a-container-instance"></a>建立容器執行個體

選取 [建立資源] > [容器] > [容器執行個體]。

![開始在 Azure 入口網站中建立新的容器執行個體][aci-portal-01]

在 [容器名稱]、[容器映像] 和 [資源群組] 文字方塊中輸入下列值。 將其他的值保留預設值，然後選取 [確定]。

* 容器名稱：`mycontainer`
* 容器映像：`mcr.microsoft.com/azuredocs/aci-helloworld`
* 資源群組：**新建** > `myResourceGroup`

![在 Azure 入口網站中設定新容器執行個體的基本設定][aci-portal-03]

在本快速入門中，保留 [公用] 預設設定，以部署公用 Microsoft `aci-helloworld` 映像。 此映像會封裝以 Node.js 撰寫並提供靜態 HTML 網頁的小型 Web 應用程式。

在 [組態] 下，為您的容器指定 [DNS 名稱標籤]。 這在您建立容器執行個體所在的 Azure 區域中，必須是唯一的名稱。 您的容器將可在 `<dns-name-label>.<region>.azurecontainer.io` 讓他人公開存取。 如果出現「DNS 名稱標籤無法使用」錯誤訊息，請嘗試使用不同的 DNS 名稱標籤。

將 [設定] 中的其他設定保留預設值，然後選取 [確定] 以驗證設定。

![在 Azure 入口網站中設定新的容器執行個體][aci-portal-04]

當驗證完成時，系統會向您顯示容器設定的摘要。 選取 [確定] 以提交容器部署要求。

![Azure 入口網站中新容器執行個體的設定摘要][aci-portal-05]

部署開始時會出現通知，指出部署正在進行中。 容器群組部署完成時，會顯示另一個通知。

![Azure 入口網站中新容器執行個體的建立進度][aci-portal-08]

瀏覽至 [資源群組] > [myResourceGroup] > [mycontainer]，開啟容器群組的概觀。 請記下容器執行個體的 [FQDN] (完整網域名稱)，以及其 [狀態]。

![Azure 入口網站中的容器群組概觀][aci-portal-06]

當其 [狀態] 成為 [執行中] 時，請在瀏覽器中瀏覽至容器的 FQDN。

![在瀏覽器中檢視使用 Azure Container Instances 所部署的應用程式][aci-portal-07]

恭喜！ 在進行幾項設定後，您即已部署可在 Azure 容器執行個體中公開存取的應用程式。

## <a name="view-container-logs"></a>檢視容器記錄

在對您的容器或其執行的應用程式排解問題時，檢視的容器執行個體的記錄會很有幫助。

若要檢視容器的記錄，請在 [設定] 下選取 [容器]，然後選取 [記錄]。 您應該會看到您在瀏覽器中檢視應用程式時產生的 HTTP GET 要求。

![Azure 入口網站中的容器記錄][aci-portal-11]

## <a name="clean-up-resources"></a>清除資源

完成容器的作業後，請選取 *mycontainer* 容器執行個體的 [概觀]，然後選取 [刪除]。

![刪除 Azure 入口網站中的容器執行個體][aci-portal-09]

在確認對話方塊出現時，選取 [是]。

![Azure 入口網站中容器執行個體的刪除確認][aci-portal-10]

## <a name="next-steps"></a>後續步驟

在本快速入門中，您已從公用 Microsoft 映像建立 Azure 容器執行個體。 如果您想要建置容器映像，並從私人的 Azure 容器登錄進行部署，請繼續進行 Azure Container Instances 教學課程。

> [!div class="nextstepaction"]
> [Azure 容器執行個體教學課程](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-portal-07]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png
[aci-portal-09]: ./media/container-instances-quickstart-portal/qs-portal-09.png
[aci-portal-10]: ./media/container-instances-quickstart-portal/qs-portal-10.png
[aci-portal-11]: ./media/container-instances-quickstart-portal/qs-portal-11.png

<!-- LINKS - External -->
[azure-free-account]: https://azure.microsoft.com/free/