---
title: 使用 REST API 和範本部署資源 | Microsoft Docs
description: 使用 Azure Resource Manager 和 Resource Manager REST API 將資源部署至 Azure。 資源會定義在 Resource Manager 範本中。
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 1d8fbd4c-78b0-425b-ba76-f2b7fd260b45
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2019
ms.author: tomfitz
ms.openlocfilehash: 15e4a7058dc1e74c726644e86c58381003eee937
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2019
ms.locfileid: "58649746"
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>使用 Resource Manager 範本和 Resource Manager REST API 部署資源

這篇文章說明如何使用 Resource Manager REST API 與 Resource Manager 範本來將您的資源部署至 Azure。  

您可以在要求本文中納入範本，也可以連結至檔案。 使用檔案時，該檔案可以是本機檔案，也可以是透過 URI 所取得的外部檔案。 當範本位於儲存體帳戶中時，您可以限制範本的存取權，並在部署期間提供共用存取簽章 (SAS) 權杖。

## <a name="deployment-scope"></a>部署範圍

您可以針對您的 Azure 訂用帳戶或訂用帳戶內的資源群組的部署。 在大部分情況下，您會針對部署至資源群組。 使用訂用帳戶部署來套用原則和角色指派，在訂用帳戶。 您也可以使用訂用帳戶部署來建立資源群組，並將資源部署到它。 根據部署的範圍，您可以使用不同的命令。

若要部署到**資源群組**，使用[Deployments-建立](/rest/api/resources/deployments/createorupdate)。

若要部署到**訂用帳戶**，使用[Deployments-建立在訂用帳戶範圍](/rest/api/resources/deployments/createorupdateatsubscriptionscope)。

這篇文章中的範例會使用資源群組部署。 如需有關訂用帳戶部署的詳細資訊，請參閱[建立資源群組和資源的訂用帳戶層級](deploy-to-subscription.md)。

## <a name="deploy-with-the-rest-api"></a>使用 REST API 部署

1. 設定[一般參數和標頭](/rest/api/azure/) (包括驗證權杖)。

1. 如果您沒有現有資源群組，請建立新的資源群組。 提供您的訂用帳戶識別碼、新資源群組的名稱，以及需要解決方案的位置。 如需詳細資訊，請參閱[建立資源群組](/rest/api/resources/resourcegroups/createorupdate)。

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2018-05-01
   ```

   使用如下的要求本文：

   ```json
   {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
   }
   ```

1. 請先執行[驗證範本部署作業](/rest/api/resources/deployments/validate)來驗證部署，然後再執行部署。 测试部署时，请提供与执行部署时所提供的完全相同的参数（如下一步中所示）。

1. 建立部署。 提供您的訂用帳戶識別碼、資源群組的名稱、部署的名稱，以及範本的連結。 如需範本檔案的相關資訊，請參閱[參數檔案](#parameter-file)。 如需以 REST API 建立資源群組的相關詳細資訊，請參閱[建立範本部署](/rest/api/resources/deployments/createorupdate)。 請注意，**mode** 是設為 **Incremental**。 若要執行完整部署，請將 **mode** 設為 **Complete**。 使用完整模式時請務必謹慎，因為您可能會不小心刪除不在範本中的資源。

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
   ```

   使用如下的要求本文：

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      }
    }
   }
   ```

    如果您想要記錄回應內容及/或要求內容，可在要求中包括 **debugSetting** 。

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "debugSetting": {
        "detailLevel": "requestContent, responseContent"
      }
    }
   }
   ```

    您可以設定儲存體帳戶以使用共用存取簽章 (SAS) Token。 如需詳細資訊，請參閱[使用共用存取簽章委派存取](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature)。

1. 不要連結至含有範本和參數的檔案，而是將它們納入要求本文中。

   ```json
   {
      "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
              "Standard_LRS",
              "Standard_GRS",
              "Standard_ZRS",
              "Premium_LRS"
            ],
            "metadata": {
              "description": "Storage Account type"
            }
          },
          "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
              "description": "Location for all resources."
            }
          }
        },
        "variables": {
          "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            "apiVersion": "2018-02-01",
            "location": "[parameters('location')]",
            "sku": {
              "name": "[parameters('storageAccountType')]"
            },
            "kind": "StorageV2",
            "properties": {}
          }
        ],
        "outputs": {
          "storageAccountName": {
            "type": "string",
            "value": "[variables('storageAccountName')]"
          }
        }
      },
      "parameters": {
        "location": {
          "value": "eastus2"
        }
      }
    }
   }
   ```

1. 取得範本部署的狀態。 如需詳細資訊，請參閱[取得範本部署的相關資訊](/rest/api/resources/deployments/get)。

   ```HTTP
   GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
   ```

## <a name="redeploy-when-deployment-fails"></a>部署失敗時重新部署

這項功能就是所謂*錯誤時回復*。 當部署失敗時，您可以從部署記錄自動重新部署先前成功的部署。 若要指定重新部署，請使用要求主體中的 `onErrorDeployment` 屬性。 如果您為您的基礎結構部署有已知的良好狀態，並想還原成這，這項功能很有用。 有一些注意事項和限制：

- 完全依照其先前執行相同的參數，則會執行重新部署。 您無法變更參數。
- 先前的部署會使用執行[完整模式](./deployment-modes.md#complete-mode)。 也會刪除任何不包含在先前的部署的資源，而任何資源的設定會設定為先前的狀態。 請確定您完全了解[部署模式](./deployment-modes.md)。
- 重新部署只會影響資源，不會影響任何資料變更。
- 這項功能只有在資源群組部署中，未訂用帳戶層級部署。 如需訂用帳戶層級部署的詳細資訊，請參閱[建立資源群組和資源的訂用帳戶層級](./deploy-to-subscription.md)。

若要使用這個選項，您的部署必須有唯一的名稱，以便在歷程記錄中進行識別。 如果您沒有唯一的名稱，則目前失敗的部署可能會覆寫歷程記錄中先前成功的部署。 您只可以使用此選項搭配根層級部署。 從巢狀範本部署不適用於重新部署。

如果目前的部署失敗，若要重新部署最後一個成功的部署，請使用：

```json
{
  "properties": {
    "templateLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
      "contentVersion": "1.0.0.0"
    },
    "mode": "Incremental",
    "parametersLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
      "contentVersion": "1.0.0.0"
    },
    "onErrorDeployment": {
      "type": "LastSuccessful",
    }
  }
}
```

如果目前的部署失敗，若要重新部署特定部署，請使用：

```json
{
  "properties": {
    "templateLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
      "contentVersion": "1.0.0.0"
    },
    "mode": "Incremental",
    "parametersLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
      "contentVersion": "1.0.0.0"
    },
    "onErrorDeployment": {
      "type": "SpecificDeployment",
      "deploymentName": "<deploymentname>"
    }
  }
}
```

指定的部署必須成功。

## <a name="parameter-file"></a>參數檔案

如果您在部署期間使用參數檔案傳遞參數值，您必須使用類似於下列範例的格式建立 JSON 檔案：

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "webSiteName": {
            "value": "ExampleSite"
        },
        "webSiteHostingPlanName": {
            "value": "DefaultPlan"
        },
        "webSiteLocation": {
            "value": "West US"
        },
        "adminPassword": {
            "reference": {
               "keyVault": {
                  "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
               },
               "secretName": "sqlAdminPassword"
            }
        }
   }
}
```

參數檔的大小不得超過 64 KB。

如果您需要提供參數機密的值 (例如密碼)，請將該值加入金鑰保存庫。 在部署期間擷取金鑰保存庫，如先前範例所示。 如需詳細資訊，請參閱 [在部署期間傳遞安全值](resource-manager-keyvault-parameter.md)。 

## <a name="next-steps"></a>後續步驟

- 若要指定如何處理存在於資源群組中、但尚未定義於範本中的資源，請參閱 [Azure Resource Manager 部署模式](deployment-modes.md)。
- 若要了解如何處理非同步 REST 作業，請參閱[追蹤非同步 Azure 作業 (英文)](resource-manager-async-operations.md)。
- 如需透過 .NET 用戶端程式庫部署資源的範例，請參閱 [使用 .NET 程式庫與範本部署資源](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。
- 若要在範本中定義參數，請參閱 [編寫範本](resource-group-authoring-templates.md#parameters)。
- 如需關於企業如何使用 Resource Manager 有效地管理訂用帳戶的指引，請參閱 [Azure 企業 Scaffold - 規定的訂用帳戶治理](/azure/architecture/cloud-adoption-guide/subscription-governance)。
