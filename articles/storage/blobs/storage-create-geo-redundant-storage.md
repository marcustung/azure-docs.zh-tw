---
title: 教學課程：建置採用 Blob 儲存體的高可用性應用程式 - Azure 儲存體
description: 使用讀取權限異地備援儲存體讓應用程式資料具有高可用性
services: storage
author: tamram
ms.service: storage
ms.topic: tutorial
ms.date: 01/03/2019
ms.author: tamram
ms.custom: mvc
ms.subservice: blobs
ms.openlocfilehash: c4e81d9be09855cde986bfd21f8f688fa7d1341e
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793697"
---
# <a name="tutorial-build-a-highly-available-application-with-blob-storage"></a>教學課程：建置採用 Blob 儲存體的高可用性應用程式

本教學課程是一個系列的第一部分。 在其中，您可了解如何讓應用程式資料在 Azure 中具有高可用性。

當您完成本教學課程時，就會擁有主控台應用程式，可從[讀取權限異地備援](../common/storage-redundancy-grs.md#read-access-geo-redundant-storage) (RA-GRS) 儲存體帳戶上傳及擷取 Blob。

RA-GRS 的運作方式是將交易從主要區域複寫到次要區域。 此複寫程序可保證次要區域中的資料是最終一致的。 該應用程式會使用[斷路器](/azure/architecture/patterns/circuit-breaker)模式來決定要連線到哪一個端點，並且在模擬失敗和復原時自動在端點間切換。

如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。

在系列的第一部分中，您將了解如何：

> [!div class="checklist"]
> * 建立儲存體帳戶
> * 設定連接字串
> * 執行主控台應用程式

## <a name="prerequisites"></a>必要條件

若要完成本教學課程：

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

* 使用下列工作負載安裝 [Visual Studio 2017](https://www.visualstudio.com/downloads/)：
  - **Azure 開發**

  ![Azure 開發 (在 [Web 和 Cloud] 之下)](media/storage-create-geo-redundant-storage/workloads.png)

# <a name="pythontabpython"></a>[Python](#tab/python)

* 安裝 [Python](https://www.python.org/downloads/)
* 下載並安裝 [Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python)

# <a name="java-v7-sdktabjava-v7"></a>[Java V7 SDK](#tab/java-v7)

* 安裝 [Maven](https://maven.apache.org/download.cgi) 並設定成從命令列運作
* 安裝並設定 [JDK](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

* 安裝 [Maven](http://maven.apache.org/download.cgi) 並設定成從命令列運作
* 安裝並設定 [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

---

## <a name="sign-in-to-the-azure-portal"></a>登入 Azure 入口網站

登入 [Azure 入口網站](https://portal.azure.com/)。

## <a name="create-a-storage-account"></a>建立儲存體帳戶

儲存體帳戶提供唯一命名空間來儲存及存取您的 Azure 儲存體資料物件。

請遵循下列步驟來建立讀取權限異地備援儲存體帳戶：

1. 選取 Azure 入口網站左上角的 [建立資源] 按鈕。
2. 從 [新增] 頁面選取 [儲存體]。
3. 在 [精選] 之下選取 [儲存體帳戶 - blob、檔案、資料表、佇列]。
4. 在儲存體帳戶表單中填寫下列資訊 (如下圖所示)，然後選取 [建立]：

   | 設定       | 建議的值 | 說明 |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **名稱** | mystorageaccount | 用於儲存體帳戶的唯一值 |
   | **部署模型** | Resource Manager  | Resource Manager 含有最新的功能。|
   | **帳戶類型** | StorageV2 | 如需帳戶類型的詳細資訊，請參閱[儲存體帳戶的類型](../common/storage-introduction.md#types-of-storage-accounts) |
   | **效能** | 標準 | 標準便足供此案例範例使用。 |
   | **複寫**| 讀取權限異地備援儲存體 (RA-GRS) | 這是讓範例起作用的必要項目。 |
   |**訂用帳戶** | 您的訂用帳戶 |如需訂用帳戶的詳細資訊，請參閱[訂用帳戶](https://account.windowsazure.com/Subscriptions)。 |
   |**ResourceGroup** | myResourceGroup |如需有效的資源群組名稱，請參閱[命名規則和限制](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions)。 |
   |**位置** | 美國東部 | 選擇位置。 |

![建立儲存體帳戶](media/storage-create-geo-redundant-storage/createragrsstracct.png)

## <a name="download-the-sample"></a>下載範例

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

[下載範例專案](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip)並擷取 (解壓縮) storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.zip 檔案。 您也可以使用 [git](https://git-scm.com/) 將應用程式的複本下載到您的開發環境。 此範例專案包含一個主控台應用程式。

```bash
git clone https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs.git
```

# <a name="pythontabpython"></a>[Python](#tab/python)

[下載範例專案](https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs/archive/master.zip)並擷取 (解壓縮) storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.zip 檔案。 您也可以使用 [git](https://git-scm.com/) 將應用程式的複本下載到您的開發環境。 此範例專案包含基本 Python 應用程式。

```bash
git clone https://github.com/Azure-Samples/storage-python-circuit-breaker-pattern-ha-apps-using-ra-grs.git
```

# <a name="java-v7-sdktabjava-v7"></a>[Java V7 SDK](#tab/java-v7)

[下載範例專案](https://github.com/Azure-Samples/storage-java-ha-ra-grs)並解壓縮 storage-java-ragrs.zip 檔案。 您也可以使用 [git](https://git-scm.com/) 將應用程式的複本下載到您的開發環境。 此範例專案包含基本 Java 應用程式。

```bash
git clone https://github.com/Azure-Samples/storage-java-ha-ra-grs.git
```

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

[下載範例專案](https://github.com/Azure-Samples/storage-java-V10-ha-ra-grs)並解壓縮 storage-java-ragrs.zip 檔案。 您也可以使用 [git](https://git-scm.com/) 將應用程式的複本下載到您的開發環境。 此範例專案包含基本 Java 應用程式。

```bash
git clone https://github.com/Azure-Samples/storage-java-V10-ha-ra-grs
```

---

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

在應用程式，您必須提供儲存體帳戶的連接字串。 建議您在執行應用程式的本機電腦上，將此連接字串儲存在環境變數內。 請根據您的作業系統，遵循以下其中一個範例來建立環境變數。

在 Azure 入口網站中，瀏覽至您的儲存體帳戶。 在儲存體帳戶中選取 [設定] 下的 [存取金鑰]。 從主要或次要金鑰複製**連接字串**。 根據您的作業系統執行下列其中一個命令，將 \<yourconnectionstring\> 取代為您的實際連接字串。 此命令會將一個環境變數儲存至本機電腦。 在 Windows 中，必須重新載入**命令提示字元**或您所使用的殼層，才能使用此環境變數。 取代下列範例中的 **\<storageConnectionString\>**：

### <a name="linux"></a> Linux

```
export storageconnectionstring=\<yourconnectionstring\>
```

### <a name="windows"></a> Windows

```PowerShell
setx storageconnectionstring "\<yourconnectionstring\>"
```

# <a name="pythontabpython"></a>[Python](#tab/python)

在應用程式，您必須提供儲存體帳戶的連接字串。 建議您在執行應用程式的本機電腦上，將此連接字串儲存在環境變數內。 請根據您的作業系統，遵循以下其中一個範例來建立環境變數。

在 Azure 入口網站中，瀏覽至您的儲存體帳戶。 在儲存體帳戶中選取 [設定] 下的 [存取金鑰]。 從主要或次要金鑰複製**連接字串**。 根據您的作業系統執行下列其中一個命令，將 \<yourconnectionstring\> 取代為您的實際連接字串。 此命令會將一個環境變數儲存至本機電腦。 在 Windows 中，必須重新載入**命令提示字元**或您所使用的殼層，才能使用此環境變數。 取代下列範例中的 **\<storageConnectionString\>**：

### <a name="linux"></a> Linux

```
export storageconnectionstring=\<yourconnectionstring\>
```

### <a name="windows"></a> Windows

```PowerShell
setx storageconnectionstring "\<yourconnectionstring\>"
```

# <a name="java-v7-sdktabjava-v7"></a>[Java V7 SDK](#tab/java-v7)

在應用程式，您必須提供儲存體帳戶的連接字串。 建議您在執行應用程式的本機電腦上，將此連接字串儲存在環境變數內。 請根據您的作業系統，遵循以下其中一個範例來建立環境變數。

在 Azure 入口網站中，瀏覽至您的儲存體帳戶。 在儲存體帳戶中選取 [設定] 下的 [存取金鑰]。 從主要或次要金鑰複製**連接字串**。 根據您的作業系統執行下列其中一個命令，將 \<yourconnectionstring\> 取代為您的實際連接字串。 此命令會將一個環境變數儲存至本機電腦。 在 Windows 中，必須重新載入**命令提示字元**或您所使用的殼層，才能使用此環境變數。 取代下列範例中的 **\<storageConnectionString\>**：

### <a name="linux"></a> Linux

```
export storageconnectionstring=\<yourconnectionstring\>
```

### <a name="windows"></a> Windows

```PowerShell
setx storageconnectionstring "\<yourconnectionstring\>"
```

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

使用此範例時，您必須安全地儲存您的儲存體帳戶名稱和金鑰。 請將其儲存在執行範例的電腦本機的環境變數中。 根據您的作業系統，使用 Linux 或 Windows 範例來建立環境變數。 在 Windows 中，必須重新載入**命令提示字元**或您所使用的殼層，才能使用此環境變數。

### <a name="linux-example"></a>Linux 範例

```
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

### <a name="windows-example"></a>Windows 範例

```
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

---

## <a name="run-the-console-application"></a>執行主控台應用程式

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

在 Visual Studio 中，按下 **F5** 或選取 [啟動]，開始對應用程式進行偵錯。 如有設定，Visual Studio 會自動還原缺少的 NuGet 套件，如需深入了解，請造訪[透過套件還原來安裝和解除安裝套件](https://docs.microsoft.com/nuget/consume-packages/package-restore#package-restore-overview)。

隨即會有主控台視窗啟動，應用程式也會開始執行。 此應用程式會將 **HelloWorld.png** 影像從解決方案上傳到儲存體帳戶。 此應用程式會進行檢查，以確保影像已複寫到次要 RA-GRS 端點。 之後，則會開始下載影像，最多會下載 999 次。 每次的讀取都會以 **P** 或 **S** 來表示。其中，**P** 代表主要端點，**S** 則代表次要端點。

![主控台應用程式執行中](media/storage-create-geo-redundant-storage/figure3.png)

在程式碼範例中，系統會使用 `Program.cs` 檔案中的 `RunCircuitBreakerAsync` 工作，透過 [DownloadToFileAsync](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_DownloadToFileAsync_System_String_System_IO_FileMode_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) 方法從儲存體帳戶下載影像。 在下載之前，系統會先定義 [OperationContext](/dotnet/api/microsoft.windowsazure.storage.operationcontext?view=azure-dotnet)。 作業內容會定義事件處理常式，當下載作業成功完成，或下載作業失敗而正在重試時，便會引發這些處理常式。

# <a name="pythontabpython"></a>[Python](#tab/python)

若要在終端機或命令提示字元上執行應用程式，請移至 **circuitbreaker.py** 目錄，然後輸入 `python circuitbreaker.py`。 此應用程式會將 **HelloWorld.png** 影像從解決方案上傳到儲存體帳戶。 此應用程式會進行檢查，以確保影像已複寫到次要 RA-GRS 端點。 之後，則會開始下載影像，最多會下載 999 次。 每次的讀取都會以 **P** 或 **S** 來表示。其中，**P** 代表主要端點，**S** 則代表次要端點。

![主控台應用程式執行中](media/storage-create-geo-redundant-storage/figure3.png)

在程式碼範例中，會使用 `circuitbreaker.py` 檔案中的 `run_circuit_breaker` 方法，透過 [get_blob_to_path](https://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html) 方法從儲存體帳戶下載影像。

儲存物件重試函式會設為線性重試原則。 重試函式會決定是否要重試要求，並指定重試要求之前所等待的秒數。 如果對主要端點的初始要求失敗時應對次要端點重試，請將 [對次要端點重試]**\_\_** 的值設為 true。 在範例應用程式中，自訂重試原則會定義於儲存體物件的 `retry_callback` 函式中。

在下載之前，會定義服務物件 [retry_callback](https://docs.microsoft.com/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) 和 [response_callback](https://docs.microsoft.com/python/api/azure.storage.common.storageclient.storageclient?view=azure-python) 函式。 這些函式會定義事件處理常式，當下載作業成功完成，或下載作業失敗而正在重試時，便會引發這些處理常式。

# <a name="java-v7-sdktabjava-v7"></a>[Java V7 SDK](#tab/java-v7)

您可以開啟將範圍設定於已下載的應用程式資料夾的終端機或命令提示字元，以執行應用程式。 在該處輸入 `mvn compile exec:java`，以執行應用程式。 接著，應用程式會將 **HelloWorld.png** 影像從目錄上傳到您的儲存體帳戶，並確認影像已複寫至次要 RA-GRS 端點。 確認完成後，應用程式會開始重複下載影像，同時回報它從中下載影像的端點。

儲存體物件重試函式會設為使用線性重試原則。 重試函式會決定是否要重試要求，並指定每次重試之間等待的秒數。 **BlobRequestOptions** 的 **LocationMode** 屬性會設為 **PRIMARY\_THEN\_SECONDARY**。 這可讓應用程式在嘗試下載 **HelloWorld.png** 期間無法連線到主要位置時，能夠自動切換至次要位置。

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

若要執行範例，請在命令列使用 Maven。

1. 開啟殼層，然後瀏覽至複製目錄內的 **storage-blobs-java-v10-quickstart**。
2. 輸入 `mvn compile exec:java` 。

此範例會在預設目錄中建立測試檔案，對於 Windows 使用者而言，此目錄為 **AppData\Local\Temp**。此範例接著會顯示您可以輸入的下列命令選項：

- 輸入 **P** 以執行放置 Blob 作業，這會將暫存檔案上傳至儲存體帳戶。
- 輸入 **L** 以執行列出 Blob 作業，這會列出目前在您容器中的 Blob。
- 輸入 **G** 以執行取得 Blob 作業，這會將檔案從儲存體帳戶下載到本機電腦。
- 輸入 **D** 以執行刪除 Blob 作業，這會從儲存體帳戶中刪除 Blob。
- 輸入 **E** 以關閉範例，這也會刪除範例建立的所有資源。

此範例顯示您在 Windows 上執行應用程式時的輸出。

```
Created quickstart container
Enter a command
(P)utBlob | (L)istBlobs | (G)etBlob | (D)eleteBlobs | (E)xitSample
# Enter a command :
P
Uploading the sample file into the container: https://<storageaccount>.blob.core.windows.net/quickstart
# Enter a command :
L
Listing blobs in the container: https://<storageaccount>.blob.core.windows.net/quickstart
Blob name: SampleBlob.txt
# Enter a command :
G
Get the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
The blob was downloaded to C:\Users\<useraccount>\AppData\Local\Temp\downloadedFile13097087873115855761.txt
# Enter a command :
D
Delete the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt

# Enter a command :
>> Blob deleted: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
E
Cleaning up the sample and exiting!
```

您可以控制範例，因此請輸入命令，使其執行程式碼。 輸入會區分大小寫。

---

## <a name="understand-the-sample-code"></a>了解範例程式碼

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

### <a name="retry-event-handler"></a>重試事件處理常式

當影像下載失敗，並設定為重試時，便會呼叫 `OperationContextRetrying` 事件處理常式。 如果達到應用程式中定義的重試次數上限，該項要求的 [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) 就會變更為 `SecondaryOnly`。 這項設定會迫使應用程式嘗試從次要端點下載影像。 因為不會無限期地重試主要端點，這個設定可減少要求該影像所花費的時間。

```csharp
private static void OperationContextRetrying(object sender, RequestEventArgs e)
{
    retryCount++;
    Console.WriteLine("Retrying event because of failure reading the primary. RetryCount = " + retryCount);

    // Check if we have had more than n retries in which case switch to secondary.
    if (retryCount >= retryThreshold)
    {

        // Check to see if we can fail over to secondary.
        if (blobClient.DefaultRequestOptions.LocationMode != LocationMode.SecondaryOnly)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.SecondaryOnly;
            retryCount = 0;
        }
        else
        {
            throw new ApplicationException("Both primary and secondary are unreachable. Check your application's network connection. ");
        }
    }
}
```

### <a name="request-completed-event-handler"></a>要求已完成的事件處理常式

當影像下載成功時，便會呼叫 `OperationContextRequestCompleted` 事件處理常式。 如果應用程式使用次要端點，則應用程式會繼續使用此端點，但最多 20 次。 20 次後，應用程式就會將 [LocationMode](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.locationmode?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_LocationMode) 重設為 `PrimaryThenSecondary`，並重試主要端點。 如果要求成功，應用程式會繼續從主要端點讀取。

```csharp
private static void OperationContextRequestCompleted(object sender, RequestEventArgs e)
{
    if (blobClient.DefaultRequestOptions.LocationMode == LocationMode.SecondaryOnly)
    {
        // You're reading the secondary. Let it read the secondary [secondaryThreshold] times,
        //    then switch back to the primary and see if it's available now.
        secondaryReadCount++;
        if (secondaryReadCount >= secondaryThreshold)
        {
            blobClient.DefaultRequestOptions.LocationMode = LocationMode.PrimaryThenSecondary;
            secondaryReadCount = 0;
        }
    }
}
```

# <a name="pythontabpython"></a>[Python](#tab/python)

### <a name="retry-event-handler"></a>重試事件處理常式

當影像下載失敗，並設定為重試時，便會呼叫 `retry_callback` 事件處理常式。 如果達到應用程式中定義的重試次數上限，該項要求的 [LocationMode](https://docs.microsoft.com/python/api/azure.storage.common.models.locationmode?view=azure-python) 就會變更為 `SECONDARY`。 這項設定會迫使應用程式嘗試從次要端點下載影像。 因為不會無限期地重試主要端點，這個設定可減少要求該影像所花費的時間。

```python
def retry_callback(retry_context):
    global retry_count
    retry_count = retry_context.count
    sys.stdout.write("\nRetrying event because of failure reading the primary. RetryCount= {0}".format(retry_count))
    sys.stdout.flush()

    # Check if we have more than n-retries in which case switch to secondary
    if retry_count >= retry_threshold:

        # Check to see if we can fail over to secondary.
        if blob_client.location_mode != LocationMode.SECONDARY:
            blob_client.location_mode = LocationMode.SECONDARY
            retry_count = 0
        else:
            raise Exception("Both primary and secondary are unreachable. "
                            "Check your application's network connection.")
```

### <a name="request-completed-event-handler"></a>要求已完成的事件處理常式

當影像下載成功時，便會呼叫 `response_callback` 事件處理常式。 如果應用程式使用次要端點，則應用程式會繼續使用此端點，但最多 20 次。 20 次後，應用程式就會將 [LocationMode](https://docs.microsoft.com/python/api/azure.storage.common.models.locationmode?view=azure-python) 重設為 `PRIMARY`，並重試主要端點。 如果要求成功，應用程式會繼續從主要端點讀取。

```python
def response_callback(response):
    global secondary_read_count
    if blob_client.location_mode == LocationMode.SECONDARY:

        # You're reading the secondary. Let it read the secondary [secondaryThreshold] times,
        # then switch back to the primary and see if it is available now.
        secondary_read_count += 1
        if secondary_read_count >= secondary_threshold:
            blob_client.location_mode = LocationMode.PRIMARY
            secondary_read_count = 0
```

# <a name="java-v7-sdktabjava-v7"></a>[Java V7 SDK](#tab/java-v7)

使用 Java 時，如果 **BlobRequestOptions** 的 **LocationMode** 屬性設為 **PRIMARY\_THEN\_SECONDARY**，則不需要定義回呼處理常式。 這可讓應用程式在嘗試下載 **HelloWorld.png** 期間無法連線到主要位置時，能夠自動切換至次要位置。

```java
    BlobRequestOptions myReqOptions = new BlobRequestOptions();
    myReqOptions.setRetryPolicyFactory(new RetryLinearRetry(deltaBackOff, maxAttempts));
    myReqOptions.setLocationMode(LocationMode.PRIMARY_THEN_SECONDARY);
    blobClient.setDefaultRequestOptions(myReqOptions);

    blob.downloadToFile(downloadedFile.getAbsolutePath(),null,blobClient.getDefaultRequestOptions(),opContext);
```

# <a name="java-v10-sdktabjava-v10"></a>[Java V10 SDK](#tab/java-v10)

使用 Java V10 SDK 範例建立的，仍不需要定義回呼處理常式，且 SDK 現在與 V7 SDK 有一些基本差異。 我們有次要**管線**，而沒有 LocationMode。 您可以透過 **RequestRetryOptions** 定義次要管線，若已定義，則允許應用程式在無法透過主要管線觸達您的資料時，自動切換到次要管線。

```java
// We create pipeline options here so that they can be easily used between different pipelines
PipelineOptions myOptions = new PipelineOptions();
myOptions.withRequestRetryOptions(new RequestRetryOptions(RetryPolicyType.EXPONENTIAL, 3, 10, 500L, 1000L, accountName + "-secondary.blob.core.windows.net"));
// We are using a default pipeline here, you can learn more about it at https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview
final ServiceURL serviceURL = new ServiceURL(new URL("https://" + accountName + ".blob.core.windows.net"), StorageURL.createPipeline(creds, myOptions));
```

---

## <a name="next-steps"></a>後續步驟

在系列的第一部分中，您已了解如何使用 RA-GRS 儲存體帳戶讓應用程式具有高可用性。

請進入系列的第二個部分，以了解如何模擬失敗狀況，並強制應用程式使用次要 RA-GRS 端點。

> [!div class="nextstepaction"]
> [模擬對主要儲存體端點的連線失敗](storage-simulate-failure-ragrs-account-app.md)
