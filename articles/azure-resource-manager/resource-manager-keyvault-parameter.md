---
title: 金鑰保存庫密碼與 Azure Resource Manager 範本 | Microsoft Docs
description: 示範如何在部署期間從金鑰保存庫中傳遞密碼做為參數。
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: 93b92a8a3b8aacd1f665725643314858fe92ad3c
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/13/2019
ms.locfileid: "56233763"
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>在部署期間使用 Azure Key Vault 以傳遞安全的參數值

您可以在部署期間從 [Azure Key Vault](../key-vault/key-vault-whatis.md) 擷取值，而不是將安全的值 (例如密碼) 直接放入參數檔案中。 您可以藉由參考金鑰保存庫和參數檔案中的密碼來擷取值。 您只參考其金鑰保存庫識別碼，因此該值絕不會公開。 金鑰保存庫可以存在於與您部署的資源群組的不同訂用帳戶中。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="deploy-key-vaults-and-secrets"></a>部署金鑰保存庫和祕密

若要建立 Key Vault 並新增祕密，請參閱：

- [使用 CLI 設定和擷取祕密](../key-vault/quick-create-cli.md)
- [使用 PowerShell 設定和擷取祕密](../key-vault/quick-create-powershell.md)
- [使用入口網站設定和擷取祕密](../key-vault/quick-create-portal.md)
- [使用 .NET 設定和擷取祕密](../key-vault/quick-create-net.md)
- [使用 Node.js 設定和擷取祕密](../key-vault/quick-create-node.md)

整合 Key Vault 與 Resource Manager 範本部署時，有一些其他考量和需求：

- `enabledForTemplateDeployment` 是金鑰保存庫屬性。 若要從 Resource Manager 部署中存取此 Key Vault 內的秘密，`enabledForTemplateDeployment` 必須是 `true`。 
- 如果您不是金鑰保存庫的擁有者，則擁有者必須更新金鑰保存庫安全性原則設定，以便您新增祕密。

下列 Azure CLI 和 Azure PowerShell 範例示範如何完成此工作：

```azurecli
# Create a Key Vault
az keyvault create \
  --name $keyVaultName \
  --resource-group $resourceGroupName \
  --location $location \
  --enabled-for-template-deployment true
az keyvault set-policy --upn $userPrincipalName --name $keyVaultName --secret-permissions set delete get list
```

```azurepowershell
New-AzKeyVault `
  -VaultName $keyVaultName `
  -resourceGroupName $resourceGroupName `
  -Location $location `
  -EnabledForTemplateDeployment
Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $userPrincipalName -PermissionsToSecrets set,delete,get,list
```

## <a name="grant-access-to-the-secrets"></a>授與祕密的存取權

對於包含 Key Vault 的範圍 (包括資源群組和 Key Vault 本身)，部署範本的使用者必須具備 `Microsoft.KeyVault/vaults/deploy/action` 權限。 [擁有者](../role-based-access-control/built-in-roles.md#owner)和[參與者](../role-based-access-control/built-in-roles.md#contributor)角色皆可授與此權限。 如果您建立了 Key Vault，您就是擁有者，因此就會有此權限。 如果 Key Vault 屬於不同的訂用帳戶，則必須由 Key Vault 的擁有者授與存取權。

下列程序說明如何建立具有最低權限的角色，以及如何指派使用者

1. 建立自訂角色定義 JSON 檔案：

    ```json
    {
      "Name": "Key Vault resource manager template deployment operator",
      "IsCustom": true,
      "Description": "Lets you deploy a resource manager template with the access to the secrets in the Key Vault.",
      "Actions": [
        "Microsoft.KeyVault/vaults/deploy/action"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ]
    }
    ```
    請將 "00000000-0000-0000-0000-000000000000" 取代為需要部署範本的使用者所具備的訂用帳戶識別碼。

2. 使用 JSON 檔案建立新的角色：

    ```azurepowershell
    $resourceGroupName= "<Resource Group Name>" # the resource group which contains the Key Vault
    $userPrincipalName = "<Email Address of the deployment operator>"
    New-AzRoleDefinition -InputFile "<PathToTheJSONFile>" 
    New-AzRoleAssignment -ResourceGroupName $resourceGroupName -RoleDefinitionName "Key Vault resource manager template deployment operator" -SignInName $userPrincipalName
    ```

    `New-AzRoleAssignment` 範例會在資源群組層級上將自訂角色指派給使用者。  

將金鑰保存庫與[受控應用程式](../managed-applications/overview.md)的範本搭配使用時，您必須授與**設備資源提供者**服務主體的存取權。 如需詳細資訊，請參閱[在部署 Azure 受控應用程式時存取金鑰保存庫密碼](../managed-applications/key-vault-access.md) (英文)。

## <a name="reference-secrets-with-static-id"></a>使用靜態識別碼參考祕密

透過這個方法，您可參考參數檔案中的金鑰保存庫，而非範本。 下圖顯示參數檔案如何參考祕密，並將該值傳遞至範本。

![Resource Manager 金鑰保存庫整合靜態識別碼圖表](./media/resource-manager-keyvault-parameter/statickeyvault.png)

[教學課程：在 Resource Manager 範本部署中整合 Azure Key Vault](./resource-manager-tutorial-use-key-vault.md)會使用此方法。 本教學課程會部署包含系統管理員密碼的虛擬機器。 密碼參數會設定為安全字串：

![Resource Manager 金鑰保存庫整合靜態識別碼範本檔案](./media/resource-manager-keyvault-parameter/resource-manager-key-vault-static-id-template-file.png)

現在，請為前述範本建立參數檔案。 在參數檔案中，指定符合範本中參數名稱的參數。 針對參數值，參考來自金鑰保存庫的密碼。 您可以藉由傳遞金鑰保存庫的資源識別碼和密碼的名稱來參考密碼：

![Resource Manager 金鑰保存庫整合靜態識別碼參數檔案](./media/resource-manager-keyvault-parameter/resource-manager-key-vault-static-id-parameter-file.png)

如果需要使用目前版本以外的祕密版本，請使用 `secretVersion` 屬性。

```json
"secretName": "examplesecret",
"secretVersion": "cd91b2b7e10e492ebb870a6ee0591b68"
```

部署範本並在參數檔案中傳遞：

對於 Azure CLI，請使用：

```azurecli
az group create --name $resourceGroupName --location $location
az group deployment create \
    --resource-group $resourceGroupName \
    --template-uri <The Template File URI> \
    --parameters <The Parameter File>
```

對於 PowerShell，請使用：

```powershell
New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
  -ResourceGroupName $resourceGroupName `
  -TemplateUri <The Template File URI> `
  -TemplateParameterFile <The Parameter File>
```

## <a name="reference-secrets-with-dynamic-id"></a>使用動態識別碼參考祕密

上一節已說明如何從參數傳遞金鑰保存庫秘密的靜態資源識別碼。 不過，在某些情況下，您需要參考會隨目前的部署而變化的金鑰保存庫密碼。 或者，您可能想要將參數值傳至範本，而不要在參數檔案中建立參考參數。 在任一情況下，您都可以使用連結的範本，以動態方式產生金鑰保存庫秘密的資源識別碼。

由於參數檔中不允許使用範本運算式，因此您無法在參數檔中以動態方式產生資源識別碼。 

在父代範本中，您必須新增連結的範本，並傳入包含動態產生的資源識別碼的參數。 下圖顯示連結的範本中的參數如何參考祕密。

![動態識別碼](./media/resource-manager-keyvault-parameter/dynamickeyvault.png)

[下列範本](https://github.com/Azure/azure-quickstart-templates/tree/master/201-key-vault-use-dynamic-id)會動態建立金鑰保存庫識別碼，並將它當作參數傳遞。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "The location where the resources will be deployed."
            }
        },
        "vaultName": {
            "type": "string",
            "metadata": {
                "description": "The name of the keyvault that contains the secret."
            }
        },
        "secretName": {
            "type": "string",
            "metadata": {
                "description": "The name of the secret."
            }
        },
        "vaultResourceGroupName": {
            "type": "string",
            "metadata": {
                "description": "The name of the resource group that contains the keyvault."
            }
        },
        "vaultSubscription": {
            "type": "string",
            "defaultValue": "[subscription().subscriptionId]",
            "metadata": {
                "description": "The name of the subscription that contains the keyvault."
            }
        },
        "_artifactsLocation": {
            "type": "string",
            "metadata": {
                "description": "The base URI where artifacts required by this template are located. When the template is deployed using the accompanying scripts, a private location in the subscription will be used and this value will be automatically generated."
            },
            "defaultValue": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/"
        },
        "_artifactsLocationSasToken": {
            "type": "securestring",
            "metadata": {
                "description": "The sasToken required to access _artifactsLocation.  When the template is deployed using the accompanying scripts, a sasToken will be automatically generated."
            },
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "apiVersion": "2018-05-01",
            "name": "dynamicSecret",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "contentVersion": "1.0.0.0",
                    "uri": "[uri(parameters('_artifactsLocation'), concat('./nested/sqlserver.json', parameters('_artifactsLocationSasToken')))]"
                },
                "parameters": {
                    "location": {
                        "value": "[parameters('location')]"
                    },
                    "adminLogin": {
                        "value": "ghuser"
                    },
                    "adminPassword": {
                        "reference": {
                            "keyVault": {
                                "id": "[resourceId(parameters('vaultSubscription'), parameters('vaultResourceGroupName'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
                            },
                            "secretName": "[parameters('secretName')]"
                        }
                    }
                }
            }
        }
    ],
    "outputs": {
        "sqlFQDN": {
            "type": "string",
            "value": "[reference('dynamicSecret').outputs.sqlFQDN.value]"
        }
    }
}
```

請部署上述範本，並提供參數值。 您可以使用 GitHub 中的範例範本，但您必須為您的環境提供參數值。

對於 Azure CLI，請使用：

```azurecli
az group create --name $resourceGroupName --location $location
az group deployment create \
    --resource-group $resourceGroupName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json \
    --parameters vaultName=$keyVaultName vaultResourceGroupName=examplegroup secretName=examplesecret
```

對於 PowerShell，請使用：

```powershell
New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
  -ResourceGroupName $resourceGroupName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json `
  -vaultName $keyVaultName -vaultResourceGroupName $keyVaultResourceGroupName -secretName $secretName
```

## <a name="next-steps"></a>後續步驟

- 如需有關金鑰保存庫的一般資訊，請參閱[什麼是 Azure Key Vault？](../key-vault/key-vault-overview.md)。
- 如需參考金鑰密碼的完整範例，請參閱 [金鑰保存庫範例](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples)。
