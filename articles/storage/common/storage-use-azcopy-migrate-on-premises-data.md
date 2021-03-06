---
title: 教學課程：使用 AzCopy 將內部部署資料移轉至 Azure 儲存體 | Microsoft Docs
description: 在本教學課程中，您將使用 AzCopy 從 Blob、資料表以及檔案內容，來回移轉資料或複製資料。 輕鬆地將資料從您的本機儲存體移轉至 Azure 儲存體。
services: storage
author: roygara
ms.service: storage
ms.topic: tutorial
ms.date: 12/14/2017
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: 40138a69baf9cd621b2f287b2fe035225bfd9bec
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2019
ms.locfileid: "58877482"
---
#  <a name="tutorial-migrate-on-premises-data-to-cloud-storage-by-using-azcopy"></a>教學課程：使用 AzCopy 將內部部署資料移轉至雲端儲存體

AzCopy 是命令列工具，可使用簡單的命令從 Azure Blob 儲存體、Azure 檔案和 Azure 資料表儲存體來回複製資料。 這些命令的設計目的是獲得最佳效能。 您可以使用 AzCopy 在檔案系統和儲存體帳戶之間，或者儲存體帳戶之間複製資料。 AzCopy 可用來將本機 (內部) 資料複製到儲存體帳戶。

下載您的作業系統適用的 AzCopy 版本：

* [Linux 上的 AzCopy](storage-use-azcopy-linux.md) 是以 .NET Core Framework 建置。 它以 Linux 平台為目標，提供 POSIX 樣式的命令列選項。 
* [Windows 上的 AzCopy](storage-use-azcopy.md) 是以 .NET Framework 建置。 它提供了 Windows 樣式的命令列選項。 
 
在本教學課程中，您了解如何：

> [!div class="checklist"]
> * 建立儲存體帳戶。 
> * 使用 AzCopy 上傳您的所有資料。
> * 基於測試目的修改資料。
> * 建立排程工作或 cron 作業來識別要上傳的新檔案。

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。

## <a name="prerequisites"></a>必要條件

若要完成本教學課程，請下載 [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#download-and-install-azcopy) 或 [Windows](https://aka.ms/downloadazcopy) 上最新版本的 AzCopy。

如果您使用 Windows，您將需要 [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx)，因為本教學課程會用它來排程工作。 Linux 使用者則應使用 crontab 命令。

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

## <a name="create-a-container"></a>建立容器

第一個步驟是建立容器，因為 Blob 一律須上傳至容器中。 容器會作為組織 Blob 群組的方法，就像您在電腦上使用資料夾來組織檔案一樣。 

請遵循下列步驟來建立容器：

1. 選取主頁面中的 [儲存體帳戶] 按鈕，並選取您所建立的儲存體帳戶。
2. 選取 [服務] 底下的 [Blob]，然後選取 [容器]。 

   ![建立容器](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
容器名稱必須以字母或數字開頭。 它們可以包含字母、數字和連字號字元 (-)。 如需 Blob 和容器的其他命名規則，請參閱[命名和參考容器、Blob 及中繼資料](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)。

## <a name="upload-contents-of-a-folder-to-blob-storage"></a>將資料夾的內容上傳至 Blob 儲存體

您可以在 [Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#upload-blobs-to-blob-storage) 或 [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#blob-download) 上使用 AzCopy，將資料夾中的所有檔案都上傳至 Blob 儲存體。 若要上傳資料夾中的所有 blob，請輸入下列 AzCopy 命令：

# [<a name="linux"></a>Linux](#tab/linux)

    azcopy \
        --source /mnt/myfolder \
        --destination https://myaccount.blob.core.windows.net/mycontainer \
        --dest-key <key> \
        --recursive

# [<a name="windows"></a> Windows](#tab/windows)

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:<key> /S
---

使用您的帳戶金鑰取代 `<key>` 和 `key`。 在 Azure 入口網站中，您可以擷取您的帳戶金鑰，方法是在您的儲存體帳戶中選取 [設定] 底下的 [存取金鑰]。 選取金鑰，並將它貼到 AzCopy 命令中。 如果指定的目的地容器不存在，則 AzCopy 會建立此容器並將檔案上傳至該容器中。 將來源路徑更新為您的資料目錄，並使用您的儲存體帳戶名稱取代目的地 URL 中的 **myaccount**。

若要以遞迴方式將指定目錄的內容上傳到 Blob 儲存體，請指定 `--recursive` (Linux) 或 `/S` (Windows) 選項。 當您使用其中一個選項執行 AzCopy 時，所有的子資料夾和其檔案也都會上傳。

## <a name="upload-modified-files-to-blob-storage"></a>將修改過的檔案上傳到 Blob 儲存體

您可以根據檔案的上次修改時間，使用 AzCopy 來[上傳檔案](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#other-azcopy-features)。 若要這麼做，請基於測試目的，在您的來源目錄中修改或建立新的檔案。 若只要上傳已更新或新的檔案，請將 `--exclude-older` (Linux) 或 `/XO` (Windows) 參數新增至 AzCopy 命令。

如果您只想複製目的地中不存在的來源資源，請在 AzCopy 命令中同時指定 `--exclude-older` 和 `--exclude-newer` (Linux) 或 `/XO` 和 `/XN` (Windows) 參數。 AzCopy 只會上傳已更新的資料 (根據其時間戳記)。

# [<a name="linux"></a>Linux](#tab/linux)

    azcopy \
    --source /mnt/myfolder \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive \
    --exclude-older

# [<a name="windows"></a> Windows](#tab/windows)

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:<key> /S /XO
---

## <a name="create-a-scheduled-task"></a>建立排程工作

您可以建立執行 AzCopy 命令指令碼的排程工作或 cron 作業。 該指令碼會識別新的內部部署資料，並在特定時間間隔將其上傳至雲端儲存體。

將 AzCopy 命令複製到文字編輯器。 將 AzCopy 命令的參數值更新為適當的值。 針對 AzCopy 將檔案另存為 `script.sh` (Linux) 或 `script.bat` (Windows)。

# [<a name="linux"></a> Linux](#tab/linux)

    azcopy --source /mnt/myfiles --destination https://myaccount.blob.core.windows.net/mycontainer --dest-key <key> --recursive --exclude-older --exclude-newer --verbose >> Path/to/logfolder/`date +\%Y\%m\%d\%H\%M\%S`-cron.log

# [<a name="windows"></a> Windows](#tab/windows)

    cd C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy
    AzCopy /Source: C:\myfolder  /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:<key> /V /XO /XN >C:\Path\to\logfolder\azcopy%date:~-4,4%%date:~-7,2%%date:~-10,2%%time:~-11,2%%time:~-8,2%%time:~-5,2%.log
---

AzCopy 是以詳細資訊 `--verbose` (Linux) 或 `/V` (Windows) 選項執行。 輸出會重新導向到記錄檔。

在本教學課程中，[Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) 會用來在 Windows 上建立排程工作。 [Crontab](http://crontab.org/) 命令會用來在 Linux 上建立 cron 作業。
 **Schtasks** 可讓系統管理員建立、刪除、查詢、變更、執行和結束本機或遠端電腦上的排程工作。 **Cron** 可讓 Linux 和 Unix 使用者在指定的日期和時間執行命令或指令碼，方法是使用 [cron 運算式](https://en.wikipedia.org/wiki/Cron#CRON_expression)。

# [<a name="linux"></a> Linux](#tab/linux)

若要在 Linux 上建立 cron 作業，請在終端機上輸入下列命令：

```bash
crontab -e
*/5 * * * * sh /path/to/script.sh
```

在命令中指定 cron 運算式 `*/5 * * * *` 會指出殼層指令碼 `script.sh` 應該每隔五分鐘執行一次。 您可以排程指令碼在每日、每月或每年的指定時間執行。 若要深入了解針對作業執行設定日期和時間，請參閱 [cron 運算式](https://en.wikipedia.org/wiki/Cron#CRON_expression)。

# [<a name="windows"></a> Windows](#tab/windows)

若要在 Windows 上建立排程工作，請於命令提示字元或者 PowerShell 中輸入下列命令：

```cmd
schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\Users\username\Documents\script.bat
```

該命令會使用：
- `/SC` 參數來指定以分鐘排程。
- `/MO` 參數來指定五分鐘的間隔。
- `/TN` 參數來指定工作名稱。
- `/TR` 參數來指定 `script.bat` 檔案的路徑。

若要深入了解在 Windows 上建立排程工作的相關資訊，請參閱 [Schtasks](https://technet.microsoft.com/library/cc772785(v=ws.10).aspx#BKMK_minutes)。

---

若要驗證排程工作/cron 作業能夠正確執行，請在您的 `myfolder` 目錄中建立新的檔案。 等候五分鐘以確認新檔案已上傳到您的儲存體帳戶。 請移至您的記錄目錄，以檢視排程工作或 cron 作業的輸出記錄。

## <a name="next-steps"></a>後續步驟

若要深入了解將內部部署資料移動至 Azure 儲存體 (或反向作業) 的相關方式，請參考下列連結：

> [!div class="nextstepaction"]
> [從 Azure 儲存體來回移動資料](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)。  

如需 AzCopy 的特定詳細資訊，請選擇您的作業系統適用的文章：

> [!div class="nextstepaction"]
> [使用 AzCopy on Windows 傳輸資料](storage-use-azcopy.md)
> [使用 AzCopy on Linux 傳輸資料](storage-use-azcopy-linux.md)
