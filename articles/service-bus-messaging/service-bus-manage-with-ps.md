---
title: 使用 PowerShell 來管理 Azure 服務匯流排資源 | Microsoft Docs
description: 使用 PowerShell 模組來建立及管理服務匯流排資源
services: service-bus-messaging
documentationcenter: .NET
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2018
ms.author: aschhab
ms.openlocfilehash: 0d15aa4d7b8a922f7606b7c4d1b357a80b3cbfab
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/09/2019
ms.locfileid: "59359118"
---
# <a name="use-powershell-to-manage-service-bus-resources"></a>使用 PowerShell 來管理服務匯流排資源

Microsoft Azure PowerShell 是一个脚本编写环境，可用于控制和自动执行 Azure 服务的部署和管理。 本文說明如何使用本機 Azure PowerShell 主控台或指令碼，運用[服務匯流排 Resource Manager PowerShell 模組](/powershell/module/az.servicebus)來佈建及管理服務匯流排實體 (命名空間、佇列、主題和訂用帳戶)。

您也可以使用 Azure Resource Manager 範本來管理服務匯流排實體。 如需詳細資訊，請參閱[使用 Azure Resource Manager 範本建立服務匯流排資源](service-bus-resource-manager-overview.md)文章。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>必要條件

開始之前，您需要下列必要條件：

* Azure 訂用帳戶。 有关获取订阅的详细信息，请参阅[购买选项][purchase options]、[会员套餐][member offers]或[免费帐户][free account]。
* 具備 Azure PowerShell 的電腦。 如需指示，請參閱[開始使用 Azure PowerShell Cmdlet](/powershell/azure/get-started-azureps)。
* 大致了解 PowerShell 指令碼、NuGet 封裝和 .NET Framework。

## <a name="get-started"></a>開始使用

第一個步驟是使用 PowerShell 來登入 Azure 帳戶和 Azure 訂用帳戶。 遵循[開始使用 Azure PowerShell Cmdlet](/powershell/azure/get-started-azureps) 中的指示登入您的 Azure 帳戶，並擷取及存取 Azure 訂用帳戶中的資源。

## <a name="provision-a-service-bus-namespace"></a>佈建服務匯流排命名空間

當使用服務匯流排命名空間，您可以使用[Get AzServiceBusNamespace](/powershell/module/az.servicebus/get-azservicebusnamespace)，[新增 AzServiceBusNamespace](/powershell/module/az.servicebus/new-azservicebusnamespace)，[移除 AzServiceBusNamespace](/powershell/module/az.servicebus/remove-azservicebusnamespace)，和[設定 AzServiceBusNamespace](/powershell/module/az.servicebus/set-azservicebusnamespace) cmdlet。

這個範例會在指令碼中建立幾個區域變數：`$Namespace` 和 `$Location`。

* `$Namespace` 是我們想要使用的服務匯流排命名空間名稱。
* `$Location` 識別資料中心，我們佈建命名空間。
* `$CurrentNamespace` 儲存我們擷取 （或建立） 的參考命名空間。

在實際的指令碼中，`$Namespace` 和 `$Location` 可以參數的方式傳遞。

這部分的指令碼會執行下列作業：

1. 嘗試擷取具有指定名稱的服務匯流排命名空間。
2. 如果找到命名空間，它會回報找到的項目。
3. 如果找不到命名空間，它會建立命名空間，然後擷取新建立的命名空間。
   
    ``` powershell
    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
   
    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Host "The namespace $Namespace already exists in the $Location region:"
        # Report what was found
        Get-AzServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
    }
    else
    {
        Write-Host "The $Namespace namespace does not exist."
        Write-Host "Creating the $Namespace namespace in the $Location region..."
        New-AzServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace -Location $Location
        $CurrentNamespace = Get-AzServiceBusNamespace -ResourceGroup $ResGrpName -NamespaceName $Namespace
        Write-Host "The $Namespace namespace in Resource Group $ResGrpName in the $Location region has been successfully created."
                
    }
    ```

### <a name="create-a-namespace-authorization-rule"></a>建立命名空間授權規則

下列範例示範如何管理使用的命名空間授權規則[新增 AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/new-azservicebusauthorizationrule)， [Get AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/get-azservicebusauthorizationrule)， [設定 AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/set-azservicebusauthorizationrule)，並[移除 AzServiceBusAuthorizationRule](/powershell/module/az.servicebus/remove-azservicebusauthorizationrule) cmdlet。

```powershell
# Query to see if rule exists
$CurrentRule = Get-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

# Check if the rule already exists or needs to be created
if ($CurrentRule)
{
    Write-Host "The $AuthRule rule already exists for the namespace $Namespace."
}
else
{
    Write-Host "The $AuthRule rule does not exist."
    Write-Host "Creating the $AuthRule rule for the $Namespace namespace..."
    New-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule -Rights @("Listen","Send")
    $CurrentRule = Get-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule
    Write-Host "The $AuthRule rule for the $Namespace namespace has been successfully created."

    Write-Host "Setting rights on the namespace"
    $authRuleObj = Get-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthorizationRuleName $AuthRule

    Write-Host "Remove Send rights"
    $authRuleObj.Rights.Remove("Send")
    Set-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Add Send and Manage rights to the namespace"
    $authRuleObj.Rights.Add("Send")
    Set-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj
    $authRuleObj.Rights.Add("Manage")
    Set-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -AuthRuleObj $authRuleObj

    Write-Host "Show value of primary key"
    $CurrentKey = Get-AzServiceBusKey -ResourceGroup $ResGrpName -NamespaceName $Namespace -Name $AuthRule
        
    Write-Host "Remove this authorization rule"
    Remove-AzServiceBusAuthorizationRule -ResourceGroup $ResGrpName -NamespaceName $Namespace -Name $AuthRule
}
```

## <a name="create-a-queue"></a>建立佇列

若要建立佇列或主題，請使用上一節的指令碼來執行命名空間檢查。 然後，建立佇列：

```powershell
# Check if queue already exists
$CurrentQ = Get-AzServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName

if($CurrentQ)
{
    Write-Host "The queue $QueueName already exists in the $Location region:"
}
else
{
    Write-Host "The $QueueName queue does not exist."
    Write-Host "Creating the $QueueName queue in the $Location region..."
    New-AzServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -EnablePartitioning $True
    $CurrentQ = Get-AzServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName
    Write-Host "The $QueueName queue in Resource Group $ResGrpName in the $Location region has been successfully created."
}
```

### <a name="modify-queue-properties"></a>修改佇列屬性

之後上一節中執行指令碼，您可以使用[組 AzServiceBusQueue](/powershell/module/az.servicebus/set-azservicebusqueue)指令程式可更新的佇列，如下列範例所示的屬性：

```powershell
$CurrentQ.DeadLetteringOnMessageExpiration = $True
$CurrentQ.MaxDeliveryCount = 7
$CurrentQ.MaxSizeInMegabytes = 2048
$CurrentQ.EnableExpress = $True

Set-AzServiceBusQueue -ResourceGroup $ResGrpName -NamespaceName $Namespace -QueueName $QueueName -QueueObj $CurrentQ
```

## <a name="provisioning-other-service-bus-entities"></a>佈建其他服務匯流排實體

您可以使用[服務匯流排 PowerShell 模組](/powershell/module/az.servicebus)來佈建其他實體，例如主題和訂用帳戶。 這些 Cmdlet 在語法上類似於上一節中所示範的佇列建立 Cmdlet。

## <a name="next-steps"></a>後續步驟

- 請在[這裡](/powershell/module/az.servicebus)參閱完整的服務匯流排 Resource Manager PowerShell 模組文件。 此頁面會列出所有可用的 Cmdlet。
- 如需使用 Azure Resource Manager 範本的相關資訊，請參閱[使用 Azure Resource Manager 範本建立服務匯流排資源](service-bus-resource-manager-overview.md)文章。
- [服務匯流排 .NET 管理程式庫](service-bus-management-libraries.md)的相關資訊。

管理服務匯流排實體有一些替代方式，如這些部落格文章中所述︰

* [如何建立服務匯流排佇列、 主題和訂用帳戶使用 PowerShell 指令碼](https://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [如何建立服務匯流排命名空間和事件中樞使用的 PowerShell 指令碼](https://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)
* [服務匯流排 PowerShell 指令碼](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[purchase options]: https://azure.microsoft.com/pricing/purchase-options/
[member offers]: https://azure.microsoft.com/pricing/member-offers/
[free account]: https://azure.microsoft.com/pricing/free-trial/
