---
title: 規劃 Azure 檔案服務部署 | Microsoft Docs
description: 了解規劃 Azure 檔案服務部署時的考量事項。
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 03/25/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: d4361fc37d01b351d20a273aa39f558e9b00faa4
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "59525920"
---
# <a name="planning-for-an-azure-files-deployment"></a>規劃 Azure 檔案服務部署

[Azure 檔案服務](storage-files-introduction.md)可提供在雲端中完全受控的檔案共用，可透過業界標準 SMB 通訊協定加以存取。 因為 Azure 檔案服務受到完整管理，所以部署於生產環境案例遠易於部署及管理檔案伺服器或 NAS 裝置。 針對在組織中部署生產環境使用的 Azure 檔案共用，本文說明應考慮的主題。

## <a name="management-concepts"></a>管理概念

 下圖說明 Azure 檔案服務管理建構：

![檔案結構](./media/storage-files-introduction/files-concepts.png)

* **儲存體帳戶**：所有對 Azure 儲存體的存取都是透過儲存體帳戶進行。 如需關於儲存體帳戶容量的詳細資訊，請參閱[延展性和效能目標](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)。

* **共用**：檔案儲存體共用是 Azure 中的 SMB 檔案共用。 所有的目錄和檔案必須在上層共用中建立。 帳戶可包含無限數目的共用，而共用可儲存無限數目的檔案，最多可達檔案共用的 5 TiB 總容量。

* **目錄**：選擇性的目錄階層。

* **檔案**：共用中的檔案。 檔案的大小可高達 1 TiB。

* **URL 格式**：若是透過檔案 REST 通訊協定以進行 Azure 檔案共用的要求，可以使用下列 URL 格式來定址檔案：

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory>/<file>
    ```

## <a name="data-access-method"></a>資料存取方法

Azure 檔案服務提供兩種方便的內建資料存取方法，您可以個別或結合使用來存取資料：

1. **雲端直接存取**：利用業界標準伺服器訊息區 (SMB) 通訊協定或透過檔案 REST API，[Windows](storage-how-to-use-files-windows.md)、[macOS](storage-how-to-use-files-mac.md) 及/或 [Linux](storage-how-to-use-files-linux.md) 可掛接任何 Azure 檔案共用。 共用上的檔案藉由 SMB 讀取和寫入時，會直接在 Azure 中的檔案共用上進行。 若要由 Azure 中的 VM 掛接，作業系統中的 SMB 用戶端必須至少支援 SMB 2.1。 若要掛接在內部 (例如使用者的工作站上)，工作站所支援的 SMB 用戶端必須至少支援 SMB 3.0 (含加密)。 除了 SMB 之外，新的應用程式或服務可以透過檔案 REST 直接存取檔案共用，為軟體開發提供了方便且可擴充的應用程式開發介面。
2. **Azure 檔案同步**：透過 Azure 檔案同步，您可以將共用複寫到內部部署或 Azure 中的 Windows Server。 您的使用者可透過 Windows Server 存取檔案共用，像是透過 SMB 或 NFS 共用。 這適用於從遠離 Azure 資料中心的位置 (例如分公司) 存取和修改資料的情況。 資料可以在多個 Windows Server 端點之間複寫，例如多個分公司之間。 最後，資料可能會分層至 Azure 檔案服務，因此透過伺服器仍然可以存取所有資料，但伺服器並沒有資料的完整複本。 資料實際上是在使用者開啟時順暢地取回。

下表說明使用者和應用程式如何存取您的 Azure 檔案共用：

| | 雲端直接存取 | Azure 檔案同步 |
|------------------------|------------|-----------------|
| 您需要使用哪些通訊協定？ | Azure 檔案服務支援 SMB 2.1、SMB 3.0 和檔案 REST API。 | 透過 Windows Server 上任何支援的通訊協定 (SMB、NFS、FTPS 等) 存取您的 Azure 檔案共用 |  
| 您會在何處執行工作負載？ | **Azure 中**：Azure 檔案服務提供對資料的直接存取。 | **低速網路的內部部署**：Windows、Linux 及 macOS 用戶端可掛接區域內部部署的 Windows 檔案共用，作為 Azure 檔案共用的快速快取。 |
| 您需要何種 ACL 層級？ | 共用和檔案層級。 | 共用、檔案和使用者層級。 |

## <a name="data-security"></a>資料安全性

Azure 檔案服務具有數個內建的選項，可用於確保資料安全性：

* 支援兩種網路通訊協定的加密：SMB 3.0 加密和經由 HTTPS 的檔案 REST。 依照預設： 
    * 支援 SMB 3.0 加密的用戶端傳送和接收資料，透過加密通道。
    * 不支援 SMB 3.0 加密的用戶端可以透過 SMB 2.1 或 SMB 3.0 通訊資料中心內部未加密。 不允許 SMB 用戶端透過 SMB 2.1 或 SMB 3.0 進行無加密的資料中心之間通訊。
    * 用戶端可以藉由 HTTP 或 HTTPS 透過檔案 REST 進行通訊。
* 待用資料加密 ([Azure 儲存體服務加密](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json))：所有儲存體帳戶都會啟用儲存體服務加密 (SSE)。 靜止資料是使用完全受控金鑰加密。 待用加密不會增加儲存成本或降低效能。 
* 加密傳輸資料的選擇性需求：選取時，Azure 檔案服務拒絕透過未加密的通道存取資料。 具體來說，只允許具有加密連線的 HTTPS 和 SMB 3.0。

    > [!Important]  
    > 要求資料安全傳輸將導致舊版 SMB 用戶端通訊失敗，因為它無法與加密的 SMB 3.0 通訊。 如需詳細資訊，請參閱[掛接在 Windows 上](storage-how-to-use-files-windows.md)、[掛接在 Linux 上](storage-how-to-use-files-linux.md)、[掛接在 macOS 上](storage-how-to-use-files-mac.md)。

為了有最高安全性，當您使用新式用戶端來存取資料時，強烈建議您一律啟用加密待用資料與加密傳輸資料。 例如，如果您需要在僅支援 SMB 2.1 的 Windows Server 2008 R2 VM 上掛接共用，因為 SMB 2.1 不支持加密，因此儲存體帳戶必須允許未加密的流量。

如果您使用 Azure 檔案同步來存取 Azure 檔案共用，則無論是否需要對待用資料進行加密，我們一律會使用加密的 HTTPS 和 SMB 3.0 將資料同步處理至 Windows Server。

## <a name="file-share-performance-tiers"></a>檔案共用效能層級

Azure 檔案服務提供兩個效能層級： 標準和進階。

* **標準檔案共用**是由轉動式硬碟機 (HDD) 支援，針對較不容易受效能變異影響的 IO 工作負載提供可靠的效能 (例如一般用途的檔案共用和開發/測試環境)。 標準檔案共用僅適用於隨用隨付計費模型。
* **進階檔案共用 (預覽)** 是由固態硬碟 (SSD) 支援，可提供持續高效能和低延遲，能在毫秒內處理多數的 IO 作業，適用於大部分 IO 密集型工作負載。 這讓它們適合各種不同的工作負載，例如資料庫、網站託管、開發環境等等。進階檔案共用僅適用於佈建計費模型。 進階檔案共用使用不同於標準檔案共用的部署模型。 如果您想要了解如何建立進階檔案共用，請參閱我們的文章：[如何建立 Azure 的進階檔案儲存體帳戶](storage-how-to-create-premium-fileshare.md)。

> [!IMPORTANT]
> 進階檔案共用仍處於預覽狀態，僅適用於 LRS、 和中才有區域子集具有所提供的 Azure 備份支援區域以選取：

|可用的區域  |Azure 備份支援  |
|---------|---------|
|美國東部 2      | 是|
|美國東部       | 是|
|美國西部       | 否 |
|美國西部 2      | 否 |
|美國中部    | 否 |
|北歐  | 否 |
|西歐   | 是|
|東南亞       | 是|
|日本東部    | 否 |
|南韓中部 | 否 |
|澳洲東部| 否 |

### <a name="provisioned-shares"></a>佈建共用

進階檔案共用 （預覽） 會根據固定的 GiB/IOPS/輸送量比佈建。 對於每個佈建的 GiB，共用將會發出一個 IOPS 和 0.1 MiB/秒輸送量，直到每個共用的上限為止。 允許佈建的最小值為 100 GiB 與最小 IOPS/輸送量。 共用大小可以隨時增加在任何時間，並降低，但可以減少一次後的最後一個增加每隔 24 小時。

在盡可能達成的基礎上，每個佈建之儲存體 Gib 的所有共用都可高載至最多三個 IOPS，並持續 60 分鐘或更久的時間 (視共用大小而定)。 新的共用一開始有以佈建容量為基礎的完整高載額度。

所有共用可以都突增到至少 100 IOPS 和目標輸送量 100 MiB/秒。 必須佈建共用，以 1 GiB 遞增。 最小大小為 100 GiB 下, 一個大小是 101 GIB 等。

> [!TIP]
> 基準 IOPS = 100 + 1 * 佈建 GiB。 （最多 100,000 IOPS 的上限）。
>
> 高載限制 = 3 * 基準 IOPS。 （最多 100,000 IOPS 的上限）。
>
> 輸出速率 = 60 MiB/秒 + $0.06 * 佈建 GiB
>
> 輸入速率 = 40 的 MiB/秒 + 0.04 * 佈建 GiB

共用大小可以隨時增加在任何時間，並降低，但可以減少一次後的最後一個增加每隔 24 小時。 大小變更後的 24 小時內，IOPS/輸送量規模變更將會有效的。

下表說明這些公式的已佈建的共用大小的一些範例：

(以表示大小 * 在有限的公開預覽)

|容量 (GiB) | 基準 IOPS | 高載限制 | 輸出 (mib) | 輸入 (mib) |
|---------|---------|---------|---------|---------|
|100         | 100     | 最多 300     | 66   | 44   |
|500         | 500     | 最多 1,500 位   | 90   | 60   |
|1,024       | 1,024   | 最多 3,072   | 122   | 81   |
|5,120       | 5,120   | 最多 15,360  | 368   | 245   |
|10,240 *     | 10,240  | 最多 30,720  | 675 | 450   |
|33,792 *     | 33,792  | 最多 100,000 部 | 2,088 | 1,392   |
|51,200 *     | 51,200  | 最多 100,000 部 | 3,132 | 2,088   |
|102,400 *    | 100,000 | 最多 100,000 部 | 6,204 | 4,136   |

最多 5 TiB 的檔案共用大小目前處於公開預覽狀態，最多 100 TiB 的大小有限公開預覽的詳細資訊，來要求權限有限的公開預覽完成時[這份問卷。](https://aka.ms/azurefilesatscalesurvey)

### <a name="bursting"></a>負載平衡

進階檔案共用可以在高載其 IOPS 最多三倍。 負載平衡自動執行，並根據信用額度系統的運作方式。 負載平衡適用於盡力 」 和 「 高載限制並不保證、 檔案共用可以在高載*達*限制。

每當您檔案共用的流量低於基準的 IOPS，信用額度會累積在高載貯體。 例如，100 GiB 共用有 100 基準 IOPS。 如果共用上的實際流量是 40 的 IOPS 在特定的 1 秒間隔內，則 60 未使用的 IOPS 是請求而應退還高載貯體。 這些信用額度將會再更新版本時使用作業會超過基準 IOPs。

> [!TIP]
> 高載限制貯體的大小 = Baseline_IOPS * 2 * 3600。

每當共用超過 IOPS 的基準，並且具有高載貯體中的信用額度，它會暴增。 共用可以繼續激增，只要剩餘信用額度，但小於 50 tiB 的共用僅會保持在最多一個小時的高載限制。 大於 50 的 TiB 的共用技術上來說可能會超過此一小時的限制，設定為兩個小時，但這根據所產生的高載點數數目。 基準 IOPS 超過每個 IO 會取用一個點數，並共用耗用所有的信用額度之後會回到基準 IOPS。

共用的信用額度會有三種狀態：

- 產生的當檔案共用使用低於基準的 IOPS。
- 拒絕，當檔案共用負載平衡。
- 當沒有任何信用額度 」 或 「 基準剩餘位於零，正在使用中的 IOPS。

其高載貯體中的信用額度的完整號碼的新檔案共用開始。

## <a name="file-share-redundancy"></a>檔案共用備援

Azure 檔案的標準共用支援三種資料備援選項： 本地備援儲存體 (LRS)、 區域備援儲存體 (ZRS) 和異地備援儲存體 (GRS)。

Azure 檔案的進階共用僅支援本地備援儲存體 (LRS)。

下列各節說明不同備援選項之間的差異：

### <a name="locally-redundant-storage"></a>本地備援儲存體

[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

### <a name="zone-redundant-storage"></a>區域備援儲存體

[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-ZRS.md)]

### <a name="geo-redundant-storage"></a>異地備援儲存體

> [!Warning]  
> 若您使用 Azure 檔案共用作為 GRS 儲存體帳戶中的雲端端點，您不應該啟動儲存體帳戶的容錯移轉。 這麼做將導致同步停止運作，且可能會在新分層的檔案中產生未預期的資料遺失。 若發生 Azure 區域遺失，Microsoft 將會觸發與 Azure 檔案同步相容的儲存體帳戶容錯移轉。

異地備援儲存體 (GRS) 設計為將您的資料複寫到與主要區域相隔數百英哩遠的次要區域，以在指定年份為物件提供至少 99.99999999999999% (16 9's) 的持久性。 如果您的儲存體帳戶已啟用 GRS，即使主要區域發生全區中斷或災害而無法復原的情況，您的資料仍會是永久性。

如果您選擇的讀取權限異地備援儲存體 (RA-GRS)，您應該知道，Azure 檔案並不支援讀取權限異地備援儲存體 (RA-GRS) 在任何區域中這一次。 RA-GRS 儲存體帳戶中的檔案共用會用在 GRS 帳戶一樣，並且可以收費的 GRS 價格。

GRS 會將您的資料複寫到次要區域中的另一個資料中心，但如果 Microsoft 起始從主要到次要區域的容錯移轉，該資料則為唯讀。

已啟用 grs 的儲存體帳戶，所有的資料會先複寫使用本地備援儲存體 (LRS)。 更新會先認可到主要位置，並使用 LRS 進行複寫。 接著會使用 GRS，以非同步的方式將更新複寫到次要區域。 當資料寫入次要位置時，也會使用 LRS 在該位置中複寫。

主要和次要區域會管理分散在儲存體縮放單位內不同容錯網域和升級網域之間的複本。 儲存體縮放單位是資料中心內的基本複寫單位。 此级别的复制由 LRS 提供；有关详细信息，请参阅[本地冗余存储 (LRS)：Azure 存储的低成本数据冗余](../common/storage-redundancy-lrs.md)。

當您決定要使用的複寫選項時，請記住下列幾點：

* 區域備援儲存體 (ZRS) 提供高可用性使用同步複寫，而且可能在某些情況下比 GRS 更好的選擇。 如需有關 ZRS 的詳細資訊，請參閱 [ZRS](../common/storage-redundancy-zrs.md)。
* 非同步複寫會涉及從將資料寫入主要區域，到將資料複寫至次要區域這段時間的延遲。 當發生區域性災害時，如果無法從主要區域復原尚未複寫到次要區域的變更，則這些變更可能會遺失。
* 使用 GRS 時，複本不提供讀取或寫入存取，除非 Microsoft 起始對次要區域的容錯移轉。 在容錯移轉的情況下，當容錯移轉完成時，您會有該資料的讀取和寫入存取權。 如需詳細資訊，請參閱[災害復原指導方針](../common/storage-disaster-recovery-guidance.md)。

## <a name="data-growth-pattern"></a>資料成長模式

現在，Azure 檔案共用的大小上限是 5 TiB （100 TiB 進階檔案共用有限的公開預覽狀態）。 由於目前的此一限制，當您在部署 Azure 檔案共用時，必須考量預期的資料成長。

使用 Azure 檔案同步可以將多個 Azure 檔案共用同步處理到單一 Windows 檔案伺服器。這可確保內部部署上較舊的大型檔案共用可以帶入 Azure 檔案同步。如需詳細資訊，請參閱[規劃 Azure 檔案同步部署](storage-files-planning.md)。

## <a name="data-transfer-method"></a>資料傳輸方法

有許多簡單的選項可從現有檔案共用 (例如內部部署檔案共用) 大量傳輸資料到 Azure 檔案服務。 部分常用方法包括 (非完整清單)：

* **Azure 檔案同步**：在 Azure 檔案共用 (雲端端點) 和 Windows 目錄命名空間 (伺服器端點) 之間的初次同步處理中，Azure 檔案同步會從現有檔案共用將所有資料複寫至 Azure 檔案服務。
* **[Azure 匯入/匯出](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)**：您可將硬碟寄送至 Azure 資料中心，透過 Azure 匯入/匯出服務，安全地將大量資料傳送至 Azure 檔案共用。 
* **[Robocopy](https://technet.microsoft.com/library/cc733145.aspx)**：Robocopy 是隨附於 Windows 和 Windows Server 的常用複製工具。 Robocopy 可在本機掛接檔案共用，然後將掛接位置作為 Robocopy 命令中的目的地使用，以將資料傳輸到 Azure 檔案服務中。
* **[AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#upload-files-to-an-azure-file-share)**：AzCopy 是一種命令列公用程式，專為使用簡單命令高效率地在 Azure 檔案服務及 Azure Blob 儲存體之間複製資料所設計。 Windows 和 Linux 都有 AzCopy 可供使用。

## <a name="next-steps"></a>後續步驟
* [規劃 Azure 檔案同步部署](storage-sync-files-planning.md)
* [部署 Azure 檔案服務](storage-files-deployment-guide.md)
* [部署 Azure 檔案同步](storage-sync-files-deployment-guide.md)
