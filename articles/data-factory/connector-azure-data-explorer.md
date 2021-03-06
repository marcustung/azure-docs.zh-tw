---
title: 使用 Azure Data Factory 將資料複製到 Azure 資料總管或從該處複製資料 | Microsoft Docs
description: 了解如何使用 Azure Data Factory 管線中的複製活動，將資料複製到 Azure 資料總管或從該處複製資料。
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/06/2019
ms.author: orspod
ms.openlocfilehash: 6138fadd060051c1b4264cd844ca2a4b8c28116a
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2019
ms.locfileid: "58880027"
---
# <a name="copy-data-to-or-from-azure-data-explorer-using-azure-data-factory"></a>使用 Azure Data Factory 將資料複製到 Azure 資料總管或從該處複製資料

本文概述如何使用 Azure Data Factory 中的複製活動，將資料複製到 [Azure 資料總管](../data-explorer/data-explorer-overview.md)或從該處複製資料。 本文是根據[複製活動概觀](copy-activity-overview.md)一文，該文提供複製活動的一般概觀。

## <a name="supported-capabilities"></a>支援的功能

您可以將資料從任何支援的來源資料存放區複製到 Azure 資料總管。 您也可以將資料從 Azure 資料總管複製到任何支援的接收資料存放區。 如需複製活動所支援作為來源或接收器的資料存放區清單，請參閱[支援的資料存放區](copy-activity-overview.md)表格。

>[!NOTE]
>自版 3.14，支援使用從 Azure 資料總管，或將使用自我裝載整合執行階段的內部部署資料存放區複製資料。

Azure 資料總管連接器可讓您執行下列作業：

* 使用 Azure Active Directory (Azure AD) 應用程式權杖驗證搭配**服務主體**來複製資料。
* 作為來源時，請使用 KQL (Kusto) 查詢擷取資料。
* 作為接收時，請將資料附加至目的地資料表。

## <a name="getting-started"></a>開始使用

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

下列各節提供有關屬性的詳細資料，這些屬性會用來定義 Azure 資料總管連接器專屬的 Data Factory 實體。

## <a name="linked-service-properties"></a>連結服務屬性

Azure 資料總管連接器會使用服務主體驗證。 請遵循下列步驟來取得服務主體，並授與權限：

1. 遵循[使用 Azure AD 租用戶註冊應用程式](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant)，以在 Azure Active Directory (Azure AD) 中註冊應用程式實體。 請記下以下的值，您可以使用這些值來定義連結服務：

    - 應用程式識別碼
    - 應用程式金鑰
    - 租用戶識別碼

2. 服務主體適當權限授與 Azure 在資料總管中。 請參閱[管理 Azure 資料總管資料庫權限](../data-explorer/manage-database-permissions.md)上角色和權限，以及逐步解說中的管理權限的詳細資訊。 一般情況下，您需要

    - **做為來源**，請至少授與**資料庫檢視器**到您的資料庫角色。
    - **作為接收器**，請至少授與**資料庫擷取器**到您的資料庫角色。

>[!NOTE]
>當使用 ADF UI 來撰寫，列出連結的服務上的資料庫，或列出資料集上的資料表作業可能需要較高特殊權限的權限授與服務主體。 或者，您可以選擇手動輸入資料庫名稱和資料表名稱。 只要具有讀取/寫入資料的適當權限授與服務主體，請將複製活動執行運作。

Azure 資料總管連結服務支援以下屬性：

| 屬性 | 描述 | 必要項 |
|:--- |:--- |:--- |
| type | **type** 屬性必須設定為 **AzureDataExplorer** | 是 |
| endpoint | Azure 資料總管叢集的端點 URL，格式為 `https://<clusterName>.<regionName>.kusto.windows.net`。 | 是 |
| 資料庫 | 資料庫名稱。 | 是 |
| tenant | 指定您的應用程式所在租用戶的資訊 (網域名稱或租用戶識別碼)。 這是什麼您通常稱為 「**授權識別碼**」 中[Kusto 連接字串](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties)。 將滑鼠游標暫留在 Azure 入口網站右上角，即可擷取它。 | 是 |
| servicePrincipalId | 指定應用程式的用戶端識別碼。 這是什麼您通常稱為 「**AAD 應用程式用戶端識別碼**」 中[Kusto 連接字串](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties)。 | 是 |
| servicePrincipalKey | 指定應用程式的金鑰。 這是什麼您通常稱為 「**AAD 應用程式金鑰**」 中[Kusto 連接字串](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties)。 將此欄位標記為 **SecureString**，將它安全地儲存在 Data Factory 中，或[參考 Azure Key Vault 中儲存的祕密](store-credentials-in-key-vault.md)。 | 是 |

**連結的服務屬性範例：**

```json
{
    "name": "AzureDataExplorerLinkedService",
    "properties": {
        "type": "AzureDataExplorer",
        "typeProperties": {
            "endpoint": "https://<clusterName>.<regionName>.kusto.windows.net ",
            "database": "<database name>",
            "tenant": "<tenant name/id e.g. microsoft.onmicrosoft.com>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>資料集屬性

如需可用來定義資料集的區段和屬性完整清單，請參閱[資料集](concepts-datasets-linked-services.md)一文。 本節提供 Azure 資料總管資料集所支援的屬性清單。

若要將資料複製到 Azure 資料總管，請將資料集的 type 屬性設定為 **AzureDataExplorerTable**。

以下是支援的屬性：

| 屬性 | 描述 | 必要項 |
|:--- |:--- |:--- |
| type | **type** 屬性必須設定為 **AzureDataExplorerTable** | 是 |
| 資料表 | 連結服務所參考的資料表名稱。 | Yes (接收)：No (來源) |

**資料集屬性範例**

```json
{
   "name": "AzureDataExplorerDataset",
    "properties": {
        "type": "AzureDataExplorerTable",
        "linkedServiceName": {
            "referenceName": "<Azure Data Explorer linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "table": "<table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>複製活動屬性

如需可用來定義活動的區段和屬性完整清單，請參閱[管線](concepts-pipelines-activities.md)一文。 本節提供 Azure 資料總管來源和接收所支援的屬性清單。

### <a name="azure-data-explorer-as-source"></a>作為來源的 Azure 資料總管

若要從 Azure 資料總管複製資料，請將複製活動來源中的 **type** 屬性設定為 **AzureDataExplorerSource**。 複製活動的 **source** 區段支援下列屬性：

| 屬性 | 描述 | 必要項 |
|:--- |:--- |:--- |
| type | 複製活動來源的 **type** 屬性必須設定為：**AzureDataExplorerSource** | 是 |
| query | [KQL 格式](/azure/kusto/query/)中指定的唯讀要求。 使用自訂的 KQL 查詢作為參考。 | 是 |
| queryTimeout | 查詢要求逾時之前的等待時間。預設值是 10 分鐘 (00:10:00)；允許的最大值為 1 小時 (01:00:00)。 | 否 |

>[!NOTE]
>預設的 azure 資料總管來源有 500,000 記錄或 64 MB 的大小限制。 若要擷取而不會截斷的所有記錄，您可以指定`set notruncation;`查詢的開頭。 請參閱[查詢限制](https://docs.microsoft.com/azure/kusto/concepts/querylimits)上更多詳細資料。

**範例：**

```json
"activities":[
    {
        "name": "CopyFromAzureDataExplorer",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "AzureDataExplorerSource",
                "query": "TestTable1 | take 10",
                "queryTimeout": "00:10:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        },
        "inputs": [
            {
                "referenceName": "<Azure Data Explorer input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

### <a name="azure-data-explorer-as-sink"></a>作為接收的 Azure 資料總管

若要將資料複製到 Azure 資料總管，請將複製活動接收中的 type 屬性設定為 **AzureDataExplorerSink**。 複製活動的 **sink** 區段支援下列屬性：

| 屬性 | 描述 | 必要項 |
|:--- |:--- |:--- |
| type | 複製活動接收的 **type** 屬性必須設定為：**AzureDataExplorerSink** | 是 |
| ingestionMappingName | 預先建立的名稱**[對應](/azure/kusto/management/mappings#csv-mapping)** Kusto 資料表上。 若要將 Azure 資料總管-適用於從來源資料行**[所有支援來源存放區/格式](copy-activity-overview.md#supported-data-stores-and-formats)** 包括 CSV/JSON/Avro 格式等，您可以使用 「 複製活動[資料行對應](copy-activity-schema-and-type-mapping.md)（由名稱隱含或明確設定） 和 （或) Azure 資料總管對應。 | 否 |

**範例：**

```json
"activities":[
    {
        "name": "CopyToAzureDataExplorer",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataExplorerSink",
                "ingestionMappingName": "<optional Azure Data Explorer mapping name>"
            }
        },
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Data Explorer output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

## <a name="next-steps"></a>後續步驟
如需 Azure Data Factory 中的複製活動所支援作為來源和接收器的資料存放區清單，請參閱[支援的資料存放區](copy-activity-overview.md#supported-data-stores-and-formats)。