---
title: 在 Azure 儲存體中設定和擷取物件屬性與中繼資料 | Microsoft Docs
description: 將物件的自訂中繼資料儲存在Azure 儲存體中，並設定和擷取系統屬性。
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/03/2019
ms.author: tamram
ms.openlocfilehash: 86bb7e736754cbc6a93bba5fff5d8d1877b1e3b4
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916575"
---
# <a name="set-and-retrieve-properties-and-metadata"></a>設定並擷取屬性和中繼資料

除了所包含之資料外，在 Azure 儲存體支援系統屬性和使用者定義中繼資料中的物件。 本文討論如何使用[適用於 .NET 的 Azure 儲存體用戶端程式庫](https://www.nuget.org/packages/WindowsAzure.Storage/)來管理系統屬性和使用者定義的中繼資料。

* **系統屬性**: 系統屬性存在於每個儲存體資源上。 其中有些系統屬性可以讀取或設定，有些則是唯讀的。 實際上，有些系統屬性會對應至特定的標準 HTTP 標頭。 Azure 儲存體用戶端程式庫會為您維護這些屬性。

* **使用者定義的中繼資料**:使用者定義的中繼資料是由您指定 Azure 儲存體資源的一或多個名稱 / 值組所組成。 您可以使用中繼資料來儲存資源的額外值。 中繼資料值僅供您自己使用，並不會影響資源的運作方式。

擷取儲存體資源的屬性和中繼資料值是一個兩步驟程序。 您必須先呼叫 **FetchAttributes** 或 **FetchAttributesAsync** 方法明確地擷取這些值，才能開始讀取這些值。 例外情況是您在資源上呼叫 **Exists** 或 **ExistsAsync** 方法。 當您呼叫其中一個方法時，Azure 儲存體會呼叫適當的 **FetchAttributes** 方法作為呼叫 **Exists** 方法的一部分。

> [!IMPORTANT]
> 如果您發現尚未擴展儲存體資源的屬性或中繼資料值，請檢查您的程式碼是否呼叫 **FetchAttributes** 或 **FetchAttributesAsync** 方法。
>
> 中繼資料名稱/值組是有效的 HTTP 標頭，因此應該遵守控管 HTTP 標頭的所有限制。 中繼資料名稱必須是有效的 HTTP 標頭名稱，可能會包含唯一的 ASCII 字元，並應該視為區分大小寫。 包含非 ASCII 字元的中繼資料值應為 Base64 編碼或 URL 編碼。

## <a name="setting-and-retrieving-properties"></a>設定與擷取屬性
若要擷取屬性值，請呼叫 Blob 或容器上的 **FetchAttributesAsync** 方法，以填入屬性，然後讀取這些值。

若要設定物件的屬性，請指定屬性值，然後呼叫 **SetProperties** 方法。

下列程式碼範例會建立一個容器，然後將其部分屬性值寫入主控台視窗。

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

//Create the service client object for credentialed access to the Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create the container if it does not already exist.
container.CreateIfNotExists();

// Fetch container properties and write out their values.
await container.FetchAttributesAsync();
Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
Console.WriteLine("ETag: {0}", container.Properties.ETag);
Console.WriteLine();
```

## <a name="setting-and-retrieving-metadata"></a>設定與擷取中繼資料
您可以針對 Blob 或容器資源，將中繼資料指定為一個或多個名稱/值組。 若要設定**中繼資料**，將名稱/值組加入至資源的中繼資料集合，然後呼叫 **SetMetadata** 或 **SetMetadataAsync** 方法，將這些值儲存至服務。

> [!NOTE]
> 您的中繼資料名稱必須符合 C# 識別碼的命名慣例。
>
>

下列程式碼範例會在容器上設定中繼資料。 其中一個值是使用集合的 **Add** 方法設定。 其他值是使用隱含的索引鍵/值語法來設定。 兩者都有效。

```csharp
public static async Task AddContainerMetadataAsync(CloudBlobContainer container)
{
    // Add some metadata to the container.
    container.Metadata.Add("docType", "textDocuments");
    container.Metadata["category"] = "guidance";

    // Set the container's metadata.
    await container.SetMetadataAsync();
}
```

若要擷取 Blob 或容器的中繼資料，請呼叫 **FetchAttributes** 或 **FetchAttributesAsync** 方法以填入**中繼資料**集合，然後讀取這些值，如以下範例所示。

```csharp
public static async Task ListContainerMetadataAsync(CloudBlobContainer container)
{
    // Fetch container attributes in order to populate the container's properties and metadata.
    await container.FetchAttributesAsync();

    // Enumerate the container's metadata.
    Console.WriteLine("Container metadata:");
    foreach (var metadataItem in container.Metadata)
    {
        Console.WriteLine("\tKey: {0}", metadataItem.Key);
        Console.WriteLine("\tValue: {0}", metadataItem.Value);
    }
}
```

## <a name="next-steps"></a>後續步驟
* [Azure Storage Client Library for.NET 參考資料](/dotnet/api/?term=Microsoft.WindowsAzure.Storage)
* [適用於.NET NuGet 套件的 azure 儲存體用戶端程式庫](https://www.nuget.org/packages/WindowsAzure.Storage/)
