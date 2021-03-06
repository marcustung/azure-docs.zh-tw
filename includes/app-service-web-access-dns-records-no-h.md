---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: d001d76bca5b9a0837349b6e05b3b0a271ea3a73
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50134444"
---
> [!NOTE]
> 您可以使用 Azure DNS 來設定 Azure Web Apps 的自訂 DNS 名稱。 如需詳細資訊，請參閱[使用 Azure DNS 為 Azure 服務提供自訂網域設定](../articles/dns/dns-custom-domain.md#app-service-web-apps)。
>
>

登入網域提供者的網站。

尋找管理 DNS 記錄的頁面。 每個網域提供者有自己的 DNS 記錄介面，請查閱您的提供者文件。 在網站中尋找標示為 [網域名稱]、[DNS] 或 [名稱伺服器管理] 的區域。 

通常可透過檢視您的帳戶資訊，然後尋找 [我的網域] 之類的連結，來找到 DNS 記錄分頁。 移至該分頁，然後尋找名為**區域檔案**、**DNS 記錄**或**進階設定**之類的連結。

下列螢幕擷取畫面是 DNS 記錄頁面的範例：

![DNS 記錄頁面範例](./media/app-service-web-access-dns-records-no-h/example-record-ui.png)

在螢幕擷取畫面範例中，您可以選取 [新增] 以建立記錄。 某些提供者有不同的連結來新增其他記錄類型。 同樣地，請參閱提供者的文件。

> [!NOTE]
> 對於某些提供者 (例如 GoDaddy)，您必須選取另外的 [儲存變更] 連結，才會讓 DNS 記錄的變更生效。 
