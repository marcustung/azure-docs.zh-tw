---
title: 在訂用時建立資源群組與資源 - Azure Resource Manager 範本
description: 描述如何在 Azure Resource Manager 範本中建立資源群組。 此外也會說明如何將資源部署到 Azure 訂用帳戶範圍。
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: bdba294e1ee776d90b93f715e930ec26765abb7f
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/18/2019
ms.locfileid: "56343029"
---
# <a name="create-resource-groups-and-resources-at-the-subscription-level"></a>在訂用帳戶層級建立資源群組和資源

您通常會將 Azure 資源部署到 Azure 訂用帳戶中的資源群組。 不過，您也可以建立 Azure 資源群組，並在訂用帳戶層級建立 Azure 資源。 若要在訂用帳戶層級部署範本，您可以使用 Azure CLI 和 Azure PowerShell。 Azure 入口網站不支援在訂用帳戶層級進行部署。

若要在 Azure Resource Manager 範本中建立資源群組，請搭配資源群組的名稱和位置定義 [**Microsoft.Resources/resourceGroups**](/azure/templates/microsoft.resources/allversions) 資源。 您可以建立資源群組，並將資源部署至相同範本中的該資源群組。 您可以在訂用帳戶層級部署的資源包括：[原則](../governance/policy/overview.md)和[角色型存取控制](../role-based-access-control/overview.md)。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="deployment-considerations"></a>部署考量

訂用帳戶層級的部署與資源群組部署的不同之處在於下列幾點：

### <a name="schema-and-commands"></a>結構描述和命令

您用在訂用帳戶層級部署上的結構描述與命令，和用於資源群組部署的不同。 

對於結構描述，請使用 `https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#` (英文)。

對於 Azure CLI 部署命令，請使用 [az deployment create](/cli/azure/deployment?view=azure-cli-latest#az-deployment-create)。 例如，下列 CLI 命令會部署範本來建立資源群組：

```azurecli
az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json \
  --parameters rgName=demoResourceGroup rgLocation=centralus
```

對於 PowerShell 部署命令，請使用 [New-AzDeployment](/powershell/module/az.resources/new-azdeployment)。 例如，下列 PowerShell 命令會部署範本來建立資源群組：

```azurepowershell
New-AzDeployment `
  -Name demoDeployment `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json `
  -rgName demoResourceGroup `
  -rgLocation centralus
```

### <a name="deployment-name-and-location"></a>部署名稱和位置

部署到訂用帳戶時，您必須提供部署的位置。 您也可以提供部署的名稱。 如果您未指定部署的名稱，會使用範本名稱做為部署名稱。 例如，部署名為 **azuredeploy.json** 的範本會建立預設的部署名稱 **azuredeploy**。

訂用帳戶層級部署的位置不可變。 您無法在某一位置建立部署，若相同名稱的現有部署已存在於其他位置。 如果您收到錯誤代碼 `InvalidDeploymentLocation`，請使用不同的名稱或與先前該名稱部署相同的位置。

### <a name="use-template-functions"></a>使用範本函式

針對訂用帳戶層級部署，使用範本函式時有一些重要考量：

* **不**支援 [resourceGroup()](resource-group-template-functions-resource.md#resourcegroup) 函式。
* 支援 [resourceId()](resource-group-template-functions-resource.md#resourceid) 函式。 您可以使用它針對用於訂用帳戶層級部署的資源取得資源識別碼。 比方說，針對具有 `resourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))` 的原則定義取得資源識別碼
* 支援 [reference()](resource-group-template-functions-resource.md#reference) 和 [list()](resource-group-template-functions-resource.md#list) 函式。

## <a name="create-resource-groups"></a>建立資源群組

下列範本會建立空的資源群組。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[parameters('rgName')]",
            "properties": {}
        }
    ],
    "outputs": {}
}
```

您可於[此處](/azure/templates/microsoft.resources/allversions)找到範本結構描述。 您可於 [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments) 找到類似範本。

## <a name="create-multiple-resource-groups"></a>建立多個資源群組

搭配資源群組使用 [copy 元素](resource-group-create-multiple.md)來建立一個以上的資源群組。 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgNamePrefix": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        },
        "instanceCount": {
            "type": "int"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[concat(parameters('rgNamePrefix'), copyIndex())]",
            "copy": {
                "name": "rgCopy",
                "count": "[parameters('instanceCount')]"
            },
            "properties": {}
        }
    ],
    "outputs": {}
}
```

如需資源反覆項目的相關資訊，請參閱[在 Azure Resource Manager 範本中部署資源或屬性的多個執行個體](./resource-group-create-multiple.md)和[教學課程：使用 Resource Manager 範本建立多個資源執行個體](./resource-manager-tutorial-create-multiple-instances.md)。

## <a name="create-resource-group-and-deploy-resources"></a>建立資源群組並部署資源

若要建立資源群組並對它部署資源，請使用巢狀範本。 巢狀範本能定義要部署至該資源群組的資源。 將巢狀範本設定為資源群組的相依項目，以確保該資源群組在部署資源之前確實存在。

下列範例會建立資源群組，並將儲存體帳戶部署至該資源群組。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        },
        "storagePrefix": {
            "type": "string",
            "maxLength": 11
        }
    },
    "variables": {
        "storageName": "[concat(parameters('storagePrefix'), uniqueString(subscription().id, parameters('rgName')))]"
    },
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[parameters('rgName')]",
            "properties": {}
        },
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2018-05-01",
            "name": "storageDeployment",
            "resourceGroup": "[parameters('rgName')]",
            "dependsOn": [
                "[resourceId('Microsoft.Resources/resourceGroups/', parameters('rgName'))]"
            ],
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "apiVersion": "2017-10-01",
                            "name": "[variables('storageName')]",
                            "location": "[parameters('rgLocation')]",
                            "kind": "StorageV2",
                            "sku": {
                                "name": "Standard_LRS"
                            }
                        }
                    ],
                    "outputs": {}
                }
            }
        }
    ],
    "outputs": {}
}
```

## <a name="create-policies"></a>建立原則

### <a name="assign-policy"></a>指派原則

下列範例會將現有原則定義指派給訂用帳戶。 如果此原則採用參數，請以物件形式提供參數。 如果此原則不採用參數，請使用預設空白物件。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "policyDefinitionID": {
            "type": "string"
        },
        "policyName": {
            "type": "string"
        },
        "policyParameters": {
            "type": "object",
            "defaultValue": {}
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "[parameters('policyName')]",
            "apiVersion": "2018-03-01",
            "properties": {
                "scope": "[subscription().id]",
                "policyDefinitionId": "[parameters('policyDefinitionID')]",
                "parameters": "[parameters('policyParameters')]"
            }
        }
    ]
}
```

若要將內建原則套用到 Azure 訂用帳戶，請使用下列 Azure CLI 命令：

```azurecli
# Built-in policy that does not accept parameters
definition=$(az policy definition list --query "[?displayName=='Audit resource location matches resource group location'].id" --output tsv)

az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=auditRGLocation
```

若要使用 PowerShell 部署此範本，請使用：

```azurepowershell
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Audit resource location matches resource group location' }

New-AzDeployment `
  -Name policyassign `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName auditRGLocation
```

若要將內建原則套用到 Azure 訂用帳戶，請使用下列 Azure CLI 命令：

```azurecli
# Built-in policy that accepts parameters
definition=$(az policy definition list --query "[?displayName=='Allowed locations'].id" --output tsv)

az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=setLocation policyParameters="{'listOfAllowedLocations': {'value': ['westus']} }"
```

若要使用 PowerShell 部署此範本，請使用：

```azurepowershell
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Allowed locations' }

$locations = @("westus", "westus2")
$policyParams =@{listOfAllowedLocations = @{ value = $locations}}

New-AzDeployment `
  -Name policyassign `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName setLocation `
  -policyParameters $policyParams
```

### <a name="define-and-assign-policy"></a>定義及指派原則

您可以在相同的範本[定義](../governance/policy/concepts/definition-structure.md)和指派原則。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyDefinitions",
            "name": "locationpolicy",
            "apiVersion": "2018-05-01",
            "properties": {
                "policyType": "Custom",
                "parameters": {},
                "policyRule": {
                    "if": {
                        "field": "location",
                        "equals": "northeurope"
                    },
                    "then": {
                        "effect": "deny"
                    }
                }
            }
        },
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "location-lock",
            "apiVersion": "2018-05-01",
            "dependsOn": [
                "locationpolicy"
            ],
            "properties": {
                "scope": "[subscription().id]",
                "policyDefinitionId": "[resourceId('Microsoft.Authorization/policyDefinitions', 'locationpolicy')]"
            }
        }
    ]
}
```

若要在訂用帳戶中建立原則定義，並將它套用至訂用帳戶，請使用下列 CLI 命令：

```azurecli
az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

若要使用 PowerShell 部署此範本，請使用：

```azurepowershell
New-AzDeployment `
  -Name definePolicy `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

## <a name="create-roles"></a>建立角色

### <a name="assign-role-at-subscription"></a>指派訂用帳戶的角色

下列範例將角色指派給訂用帳戶的使用者或群組。 在此範例中，您不會指定指派的範圍，因為範圍會自動設為訂用帳戶。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string"
        },
        "roleDefinitionId": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "name": "[guid(parameters('principalId'), deployment().name)]",
            "apiVersion": "2017-09-01",
            "properties": {
                "roleDefinitionId": "[resourceId('Microsoft.Authorization/roleDefinitions', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

若要將 Active Directory 群組指派給您訂用帳戶的角色，請使用下列 Azure CLI 命令：

```azurecli
# Get ID of the role you want to assign
role=$(az role definition list --name Contributor --query [].name --output tsv)

# Get ID of the AD group to assign the role to
principalid=$(az ad group show --group demogroup --query objectId --output tsv)

az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/roleassign.json \
  --parameters principalId=$principalid roleDefinitionId=$role
```

若要使用 PowerShell 部署此範本，請使用：

```azurepowershell
$role = Get-AzRoleDefinition -Name Contributor

$adgroup = Get-AzADGroup -DisplayName demogroup

New-AzDeployment `
  -Name demoRole `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/roleassign.json `
  -roleDefinitionId $role.Id `
  -principalId $adgroup.Id
```

### <a name="assign-role-at-scope"></a>指派範圍的角色

下列訂用帳戶層級的範本會將角色指派給訂用帳戶中某個資源群組範圍內的使用者或群組。 此範圍必須等於或低於部署層級。 您可以部署至訂用帳戶，並指定該訂用帳戶中某個資源群組範圍內的角色指派。 不過，您無法部署至資源群組，並指定訂用帳戶範圍內的角色指派。

若要指派範圍的角色，請使用巢狀部署。 請注意，資源群組名稱會指定於部署資源的屬性和角色指派的範圍屬性中。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "principalId": {
            "type": "string"
        },
        "roleDefinitionId": {
            "type": "string"
        },
        "rgName": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2018-05-01",
            "name": "assignRole",
            "resourceGroup": "[parameters('rgName')]",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Authorization/roleAssignments",
                            "name": "[guid(parameters('principalId'), deployment().name)]",
                            "apiVersion": "2017-09-01",
                            "properties": {
                                "roleDefinitionId": "[resourceId('Microsoft.Authorization/roleDefinitions', parameters('roleDefinitionId'))]",
                                "principalId": "[parameters('principalId')]",
                                "scope": "[concat(subscription().id, '/resourceGroups/', parameters('rgName'))]"
                            }
                        }
                    ],
                    "outputs": {}
                }
            }
        }
    ],
    "outputs": {}
}
```

若要將 Active Directory 群組指派給您訂用帳戶的角色，請使用下列 Azure CLI 命令：

```azurecli
# Get ID of the role you want to assign
role=$(az role definition list --name Contributor --query [].name --output tsv)

# Get ID of the AD group to assign the role to
principalid=$(az ad group show --group demogroup --query objectId --output tsv)

az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/scopedRoleAssign.json \
  --parameters principalId=$principalid roleDefinitionId=$role rgName demoRg
```

若要使用 PowerShell 部署此範本，請使用：

```azurepowershell
$role = Get-AzRoleDefinition -Name Contributor

$adgroup = Get-AzADGroup -DisplayName demogroup

New-AzDeployment `
  -Name demoRole `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/scopedRoleAssign.json `
  -roleDefinitionId $role.Id `
  -principalId $adgroup.Id `
  -rgName demoRg
```

## <a name="next-steps"></a>後續步驟

* 如需針對 Azure 資訊安全中心部署工作區設定的範例，請參閱 [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json)。
* 若要了解如何建立 Azure 資源管理員範本，請參閱 [撰寫範本](resource-group-authoring-templates.md)。 
* 如需在範本中可用函式的清單，請參閱 [範本函式](resource-group-template-functions.md)。
