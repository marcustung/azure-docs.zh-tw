---
title: 使用 SSL 保護 Web 服務
titleSuffix: Azure Machine Learning service
description: 了解如何保護部署與 Azure Machine Learning 服務藉由啟用 HTTPS 的 web 服務。 HTTPS 會保護使用傳輸層安全性 (TLS) 安全通訊端層 (SSL) 來取代用戶端所提交的資料。 它也會使用用戶端驗證的 web 服務的身分識別。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 02/05/2019
ms.custom: seodec18
ms.openlocfilehash: 1a6aa75f3d25cd88cd1edb9b2cdcfabc3b4ec8f9
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "58103888"
---
# <a name="use-ssl-to-secure-web-services-with-azure-machine-learning-service"></a>使用 SSL 來保護具有 Azure Machine Learning 服務的 Web 服務

在此文章中，您將了解如何保護使用 Azure Machine Learning services 部署的 Web 服務。 您可以限制存取 web 服務，並確保使用的用戶端所提交的資料安全[都會使用超文字傳輸通訊協定安全 (HTTPS)](https://en.wikipedia.org/wiki/HTTPS)。

使用 HTTPS 來保護用戶端與您的 web 服務之間的通訊加密兩者之間的通訊。 加密會使用處理[傳輸層安全性 (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security)。 這會還是有時為 Secure Sockets Layer (SSL)，也就是 TLS 的前身。

> [!TIP]
> Azure 機器學習服務 SDK 會使用的詞彙 'SSL' 屬性與啟用的通訊安全。 這不表示 TLS 不是由您的 web 服務，只要該 SSL 是許多讀者更容易辨識的詞彙。

TLS 和 SSL 同時依賴__數位憑證__，用來執行加密和身分識別驗證。 如需有關憑證的數位運作的詳細資訊，請參閱維基百科項目上[公開金鑰基礎結構 (PKI)](https://en.wikipedia.org/wiki/Public_key_infrastructure)。

> [!Warning]
> 如果您未進行啟用，並針對您的 web 服務使用 HTTPS，從服務傳送的資料可能會顯示給其他人在網際網路上。
>
> HTTPS 也可讓用戶端確認它連接到伺服器的真確性。 這可避免針對的用戶端[攔截](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)攻擊。

保護新的 web 服務或現有的程序如下所示：

1. 取得網域名稱。

2. 取得數位憑證。

3. 部署或更新已啟用 SSL 設定的 Web 服務。

4. 更新您的 DNS 以指向 Web 服務。

跨[部署目標](how-to-deploy-and-where.md)保護 Web 服務時有些微差異。

## <a name="get-a-domain-name"></a>取得網域名稱

如果您還未擁有網域名稱，可以向__網域名稱註冊機構__購買。 不同的註冊機構有不同的流程，而且費用也不同。 註冊機構也會為您提供管理網域名稱所使用的工具。 這些工具用來對應的完整的網域名稱 (例如 www\.contoso.com) 來裝載您的 web 服務的 IP 位址。

## <a name="get-an-ssl-certificate"></a>取得 SSL 憑證

有許多方法可取得的 SSL 憑證 （數位憑證）。 最常見的作法是向__憑證授權單位__ (CA) 購買。 不論您在哪裡取得憑證，都需要下列檔案：

* __憑證__。 憑證必須包含完整的憑證鏈結，而且必須以 PEM 編碼。
* __金鑰__。 此金鑰必須以 PEM 編碼。

要求憑證時，您必須提供打算用於 Web 服務的位址完整網域名稱 (FQDN)。 例如，www\.contoso.com。 驗證 Web 服務的身分識別時，會比較在憑證上加上戳記的位址和用戶端所使用的位址。 如果位址不相符，用戶端就會收到錯誤。

> [!TIP]
> 如果憑證授權單位無法以 PEM 編碼的檔案來提供憑證和金鑰，您可以使用 [OpenSSL](https://www.openssl.org/) 之類的公用程式來變更格式。

> [!WARNING]
> 自我簽署的憑證應只用於開發。 這些憑證不應用於實際生產環境。 自我簽署的憑證可能會造成用戶端應用程式的問題。 如需詳細資訊，請參閱用戶端應用程式中所用網路程式庫的文件。

## <a name="enable-ssl-and-deploy"></a>啟用 SSL 並部署

若要在 SSL 啟用時部署 (重新部署) 服務，請在適當的情況下，將 `ssl_enabled` 參數設定為 `True`。 將 `ssl_certificate` 參數設定為__憑證__檔案的值，並將 `ssl_key` 設定為__金鑰__檔案的值。

+ **在 Azure Kubernetes Service (AKS) 上部署**

  佈建 AKS 叢集時，提供 SSL 相關參數的值，如下列程式碼片段所示：

    ```python
    from azureml.core.compute import AksCompute

    provisioning_config = AksCompute.provisioning_configuration(ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **在 Azure 容器執行個體 (ACI) 上部署**

  部署到 ACI 時，提供 SSL 相關參數的值，如下列程式碼片段所示：

    ```python
    from azureml.core.webservice import AciWebservice

    aci_config = AciWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **在可現場程式化閘道陣列 (FPGA) 上部署**

  部署到 FPGA 時，提供 SSL 相關參數的值，如下列程式碼片段所示：

    ```python
    from azureml.contrib.brainwave import BrainwaveWebservice

    deployment_config = BrainwaveWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem")
    ```

## <a name="update-your-dns"></a>更新您的 DNS

接下來，您必須更新 DNS 以指向 Web 服務。

+ **針對 ACI**：

  使用網域名稱註冊機構所提供的工具來更新您網域名稱的 DNS 記錄。 這筆記錄必須指向此服務的 IP 位址。

  根據註冊機構以及針對網域名稱所設定的存留時間 (TTL)，可能需要等待數分鐘到數小時的時間，用戶端才能解析網域名稱。

+ **對於 AKS**：

  在 AKS 叢集之 [公用 IP 位址] 的 [設定] 索引標籤底下更新 DNS，如下圖所示。 您可以找到公用 IP 位址，作為在包含 AKS 代理程式節點和其他網路資源之資源群組下方所建立的其中一個資源類型。

  ![Azure Machine Learning 服務：使用 SSL 保護 Web 服務](./media/how-to-secure-web-service/aks-public-ip-address.png)

## <a name="next-steps"></a>後續步驟
了解如何：
+ [使用部署為 Web 服務的機器學習模型](how-to-consume-web-service.md)
+ [在 Azure 虛擬網路內安全地執行實驗與推斷](how-to-enable-virtual-network.md)
