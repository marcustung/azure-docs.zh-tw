---
title: 部署適用於 VM 的 Azure 監視器 (預覽) | Microsoft Docs
description: 本文說明您如何部署並設定適用於 VM 的 Azure 監視器，讓您可以開始了解分散式應用程式的執行方式，以及已識別出哪些健康情況問題。
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/13/2019
ms.author: magoedte
ms.openlocfilehash: 1a4bfae22477e345176971bd40b0afa91c8867fb
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885820"
---
# <a name="deploy-azure-monitor-for-vms-preview"></a>部署適用於 VM 的 Azure 監視器 (預覽)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

本文說明如何設定適用於 VM 的 Azure 監視器。 此服務會監視 Azure 虛擬機器 (VM) 和虛擬機器擴展集的作業系統健康情況，以及您環境中的虛擬機器。 此監控包括探索及對應可能會在其上裝載的應用程式相依性。 

您可以使用下列其中一種方法，啟用適用於 VM 的 Azure 監視器：

* 直接從 VM 選取 [Insights (預覽)]，以啟用單一 Azure 虛擬機器。
* 使用 Azure 原則啟用兩個或多個 Azure VM。 透過這個方法可安裝並正確設定現有 VM 和新 VM 所需的相依性。 系統會報告不符合規範的 VM，因此您可以決定是否要啟用這些 VM，以及如何進行修復。
* 使用 PowerShell，跨指定的訂用帳戶或資源群組啟用兩個或多個 Azure VM 或虛擬機器擴展集。

本文稍後會提供有關每個方法的其他資訊。

## <a name="prerequisites"></a>必要條件
開始之前，請確定您了解下列各節中的資訊。

### <a name="log-analytics"></a>Log Analytics

適用於 Vm 的 azure 監視器支援 Log Analytics 工作區，在下列區域：

- 美國中西部
- 美國東部
- 加拿大中部<sup>1</sup>
- 英國南部<sup>1</sup>
- 西歐
- 東南亞<sup>1</sup>

<sup>1</sup> 此區域目前不支援適用於 VM 的 Azure 監視器的健康情況功能。

>[!NOTE]
>Azure 虛擬機器可以從任何區域部署，不限於 Log Analytics 工作區支援的區域。
>

如果您沒有工作區，可以使用下列其中一種方法建立一個工作區：
* [Azure CLI](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [PowerShell](../../azure-monitor/learn/quick-create-workspace-posh.md)
* [Azure 入口網站](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)

如果您要在 Azure 入口網站中啟用單一 Azure VM 的監視，可以在此程序期間建立一個工作區。

若要啟用大規模案例的解決方案，請先在您的 Log Analytics 工作區中設定下列項目：

* 安裝 ServiceMap 和 InfrastructureInsights 解決方案。 您僅能使用本文中提供的 Azure Resource Manager 範本，完成此安裝。
* 設定 Log Analytics 工作區，以收集效能計數器。

若要針對大規模案例設定您的工作區，請參閱＜針對大規模部署設定 Log Analytics 工作區＞。

### <a name="supported-operating-systems"></a>受支援的作業系統

下表列出適用於 VM 的 Azure 監視器支援的 Windows 和 Linux 作業系統。 本節稍後會提供詳列主要和次要 Linux OS 版本及支援的核心版本的完整清單。

|作業系統版本 |效能 |地圖 |健康情況 |
|-----------|------------|-----|-------|
|Windows Server 2019 | X | X | |
|Windows Server 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |
|Windows Server 2012 R2 | X | X | |
|Windows Server 2012 | X | X | |
|Windows Server 2008 R2 | X | X| |
|Red Hat Enterprise Linux (RHEL) 6 7| X | X| X |
|Ubuntu 14.04、 16.04、 18.04 | X | X | X |
|CentOS Linux 6、 7 | X | X | X |
|SUSE Linux Enterprise Server (SLES) 11、 12 | X | X | X |
|Debian 8、 9.4 | X<sup>1</sup> | | X |

<sup>1</sup> 適用於 VM 的 Azure 監視器的效能功能僅能從 Azure 監視器使用。 當您直接從 Azure VM 的左窗格存取時，則無法使用。

>[!NOTE]
>下列資訊適用於 Linux 作業系統的支援：
> - 只支援預設版本和 SMP Linux 核心版本。
> - 所有 Linux 散發套件皆不支援非標準的核心版本，例如實體位址擴充 (PAE) 和 Xen。 例如，不支援版本字串為 *2.6.16.21-0.8-xen* 的系統。
> - 不支援自訂核心，包括重新編譯的標準核心。
> - 支援 CentOSPlus 核心。

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| 作業系統版本 | 核心版本 |
|:--|:--|
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |
| 7.6 | 3.10.0-957 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| 作業系統版本 | 核心版本 |
|:--|:--|
| 6.9 | 2.6.32-696 |
| 6.10 | 2.6.32-754 |

### <a name="centosplus"></a>CentOSPlus
| 作業系統版本 | 核心版本 |
|:--|:--|
| 6.9 | 2.6.32-696.18.7<br>2.6.32-696.30.1 |
| 6.10 | 2.6.32-696.30.1<br>2.6.32-754.3.5 |

#### <a name="ubuntu-server"></a>Ubuntu Server

| 作業系統版本 | 核心版本 |
|:--|:--|
| Ubuntu 18.04 | 核心 4.15.* |
| Ubuntu 16.04.3 | 核心 4.15.* |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

#### <a name="suse-linux-11-enterprise-server"></a>SUSE Linux 11 Enterprise Server

| 作業系統版本 | 核心版本
|:--|:--|
|11 SP4 | 3.0.* |

#### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 Enterprise Server

| 作業系統版本 | 核心版本
|:--|:--|
|12 SP2 | 4.4.* |
|12 SP3 | 4.4.* |

### <a name="the-microsoft-dependency-agent"></a>Microsoft Dependency Agent
適用於 VM 的 Azure 監視器對應功能會從 Microsoft Dependency Agent 取得其資料。 Dependency Agent 須憑藉 Log Analytics 代理程式才能連線至 Log Analytics。 因此，系統必須安裝 Log Analytics 代理程式並使用 Dependency Agent 加以設定。

無論您為單一 Azure VM 啟用適用於 VM 的 Azure 監視器，還是使用大規模部署方法，您都需要使用 Azure VM Dependency Agent 擴充功能，將代理程式安裝為該體驗的一部分。

在混合式環境中，您可以透過下列其中一種方式，下載並安裝 Dependency Agent：手動或使用裝載於 Azure 外部之虛擬機器的自動化部署方法。

下表說明對應功能在混合式環境中支援的連線來源。

| 連線的來源 | 支援 | 描述 |
|:--|:--|:--|
| Windows 代理程式 | 是 | 除了[適用於 Windows 的 Log Analytics 代理程式](../../azure-monitor/platform/log-analytics-agent.md)以外，Windows 代理程式還需要 Microsoft Dependency Agent。 如需作業系統版本的完整清單，請參閱[支援的作業系統](#supported-operating-systems)。 |
| Linux 代理程式 | 是 | 除了[適用於 Linux 的 Log Analytics 代理程式](../../azure-monitor/platform/log-analytics-agent.md)以外，Linux 代理程式還需要 Microsoft Dependency Agent。 如需作業系統版本的完整清單，請參閱[支援的作業系統](#supported-operating-systems)。 |
| System Center Operations Manager 管理群組 | 否 | |

您可以從下列位置下載 Dependency Agent：

| 檔案 | 作業系統 | 版本 | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) |  Windows | 9.7.4 | A111B92AB6CF28EB68B696C60FE51F980BFDFF78C36A900575E17083972989E0 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) |  Linux | 9.7.4 | AB58F3DB8B1C3DEE7512690E5A65F1DFC41B43831543B5C040FCCE8390F2282C |

## <a name="role-based-access-control"></a>角色型存取控制
若要啟用並存取適用於 VM 的 Azure 監視器中的功能，您需要獲指派下列存取角色：

- 若要啟用解決方案，您必須具備 *Log Analytics 參與者*角色。

- 若要檢視效能、健康情況並對應資料，您必須擁有 Azure VM 的*監視讀者*角色。 您必須針對適用於 VM 的 Azure 監視器設定 Log Analytics 工作區。

如需有關如何控制 Log Analytics 工作區存取的詳細資訊，請參閱[管理工作區](../../azure-monitor/platform/manage-access.md)。

## <a name="enable-monitoring-in-the-azure-portal"></a>在 Azure 入口網站中啟用監視
若要在 Azure 入口網站中啟用 Azure VM 的監視，請執行下列動作：

1. 登入 [Azure 入口網站](https://portal.azure.com)。

1. 選取 [虛擬機器]。

1. 從清單中選取 VM。

1. 在 VM 頁面的 [監視] 區段中，選取 [Insights (預覽)]。

1. 在 [Insights (預覽)] 頁面上，選取 [立即試用]。

    ![為 VM 啟用適用於 VM 的 Azure 監視器](./media/vminsights-onboard/enable-vminsights-vm-portal-01.png)
1. 在 [將 Azure 監視器 Insights 上線] 頁面上，如果您目前在同一個訂用帳戶中有 Log Analytics 工作區，請在下拉式清單中選取它。  
    此清單會預先選取已在訂用帳戶中部署虛擬機器的預設工作區和位置。 

    >[!NOTE]
    >如果您想要建立新的 Log Analytics 工作區以儲存來自 VM 的監視資料，請遵循在先前所列的其中一個支援區域中[建立 Log Analytics 工作區](../../azure-monitor/learn/quick-create-workspace.md)中的指示。

啟用監視之後，可能需要約 10 分鐘的時間才能檢視虛擬機器的健康情況計量。

![啟用適用於 VM 的 Azure 監視器來監視部署處理](./media/vminsights-onboard/onboard-vminsights-vm-portal-status.png)


## <a name="deploy-at-scale"></a>大規模部署
在本節中，您要使用 Azure 原則或 Azure PowerShell，大規模部署適用於 VM 的 Azure 監視器。

部署 VM 之前，請執行下列操作，預先設定您的 Log Analytics 工作區：

1. 如果您還沒有工作區，請建立一個可以支援適用於 VM 的 Azure 監視器的工作區。  
    在繼續之前，請參閱[管理工作區](../../log-analytics/log-analytics-manage-access.md?toc=/azure/azure-monitor/toc.json)以了解成本、管理和合規性考量。

1. 建立新工作區 (如果不存在的話)，以用來支援適用於 VM 的 Azure 監視器。 建立新工作區之前先檢閱[管理工作區](../../azure-monitor/platform/manage-access.md?toc=/azure/azure-monitor/toc.json)，以了解成本、管理和合規性考量，然後再繼續進行。

1. 在工作區中啟用效能計數器，以收集 Linux 和 Windows VM 上的資料。

1. 在工作區中安裝並啟用 ServiceMap 和 InfrastructureInsights 解決方案。

### <a name="set-up-a-log-analytics-workspace"></a>設定 Log Analytics 工作區
如果您沒有 Log Analytics 工作區，請檢閱[「必要條件」](#log-analytics)一節中所建議的方法，建立一個工作區。

#### <a name="enable-performance-counters"></a>啟用效能計數器
如果解決方案所參考的 Log Analytics 工作區還未設定為收集此解決方案所需的效能計數器，則必須啟用這些效能計數器。 您可以透過下列其中一種方式進行：
* 手動，如 [Log Analytics 中的 Windows 和 Linux 效能資料來源](../../azure-monitor/platform/data-sources-performance-counters.md)中所述
* 下載並執行可從 [Azure PowerShell 資源庫](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)取得的 PowerShell 指令碼

#### <a name="install-the-servicemap-and-infrastructureinsights-solutions"></a>安裝 ServiceMap 和 InfrastructureInsights 解決方案
此方法包含一個 JSON 範本，其會指定在 Log Analytics 工作區中啟用解決方案元件的設定。

如果您不熟悉使用範本部署資源，請參閱：
* [使用 Resource Manager 範本與 Azure PowerShell 來部署資源](../../azure-resource-manager/resource-group-template-deploy.md)
* [使用 Resource Manager 範本和 Azure CLI 來部署資源](../../azure-resource-manager/resource-group-template-deploy-cli.md)

如果您選擇使用 Azure CLI，必須先在本機安裝並使用 CLI。 您必須執行 Azure CLI 2.0.27 版或更新版本。 若要知道您使用的版本，請執行 `az --version`。 如果您需要安裝或升級 Azure CLI，請參閱[安裝 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)。

1. 複製以下 JSON 語法並貼到您的檔案中：

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "WorkspaceName": {
                "type": "string"
            },
            "WorkspaceLocation": {
                "type": "string"
            }
        },
        "resources": [
            {
                "apiVersion": "2017-03-15-preview",
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('WorkspaceName')]",
                "location": "[parameters('WorkspaceLocation')]",
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },

                        "plan": {
                            "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'ServiceMap')]",
                            "promotionCode": ""
                        }
                    },
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },
                        "plan": {
                            "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'InfrastructureInsights')]",
                            "promotionCode": ""
                        }
                    }
                ]
            }
        ]
    }
    ```

1. 在本機資料夾中將此檔案儲存為 *installsolutionsforvminsights.json*。

1. 擷取 *WorkspaceName*、*ResourceGroupName* 和 *WorkspaceLocation* 的值。 *WorkspaceName* 的值是您 Log Analytics 工作區的名稱。 *WorkspaceLocation* 的值是定義工作區所在的區域。

1. 您已準備好部署此範本。
 
    * 在包含範本的資料夾中使用下列 PowerShell 命令：

        ```powershell
        New-AzResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName <ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
        ```

        可能需要幾分鐘的時間才能完成設定變更。 完成之後，將會顯示如下的訊息並包含結果：

        ```powershell
        provisioningState       : Succeeded
        ```

    * 使用 Azure CLI 來執行下列命令：
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --name DeploySolutions --resource-group <ResourceGroupName> --template-file InstallSolutionsForVMInsights.json --parameters WorkspaceName=<workspaceName> WorkspaceLocation=<WorkspaceLocation - example: eastus>
        ```

        可能需要幾分鐘的時間才能完成設定變更。 完成之後，將會顯示如下訊息並包含結果：

        ```azurecli
        provisioningState       : Succeeded

### Enable by using Azure Policy
To enable Azure Monitor for VMs at scale in a way that helps ensure consistent compliance and the automatic enabling of the newly provisioned VMs, we recommend [Azure Policy](../../governance/policy/overview.md). These policies:

* Deploy the Log Analytics agent and the Dependency agent.
* Report on compliance results.
* Remediate for non-compliant VMs.

To enable Azure Monitor for VMs by using Azure Policy in your tenant:

- Assign the initiative to a scope: management group, subscription, or resource group
- Review and remediate compliance results

For more information about assigning Azure Policy, see [Azure Policy overview](../../governance/policy/overview.md#policy-assignment) and review the [overview of management groups](../../governance/management-groups/index.md) before you continue.

The policy definitions are listed in the following table:

|Name |Description |Type |
|-----|------------|-----|
|[Preview]: Enable Azure Monitor for VMs |Enable Azure Monitor for the Virtual Machines (VMs) in the specified scope (management group, subscription, or resource group). Takes Log Analytics workspace as a parameter. |Initiative |
|[Preview]: Audit Dependency Agent Deployment – VM Image (OS) unlisted |Reports VMs as non-compliant if the VM Image (OS) isn't defined in the list and the agent isn't installed. |Policy |
|[Preview]: Audit Log Analytics Agent Deployment – VM Image (OS) unlisted |Reports VMs as non-compliant if the VM Image (OS) isn't defined in the list and the agent isn't installed. |Policy |
|[Preview]: Deploy Dependency Agent for Linux VMs |Deploy Dependency Agent for Linux VMs if the VM Image (OS) is defined in the list and the agent isn't installed. |Policy |
|[Preview]: Deploy Dependency Agent for Windows VMs |Deploy Dependency Agent for Windows VMs if the VM Image (OS) is defined in the list and the agent isn't installed. |Policy |
|[Preview]: Deploy Log Analytics Agent for Linux VMs |Deploy Log Analytics Agent for Linux VMs if the VM Image (OS) is defined in the list and the agent isn't installed. |Policy |
|[Preview]: Deploy Log Analytics Agent for Windows VMs |Deploy Log Analytics Agent for Windows VMs if the VM Image (OS) is defined in the list and the agent isn't installed. |Policy |

Standalone policy (not included with the initiative) is described here:

|Name |Description |Type |
|-----|------------|-----|
|[Preview]: Audit Log Analytics Workspace for VM - Report Mismatch |Report VMs as non-compliant if they aren't logging to the Log Analytics workspace specified in the policy/initiative assignment. |Policy |

#### Assign the Azure Monitor initiative
With this initial release, you can create the policy assignment only in the Azure portal. To understand how to complete these steps, see [Create a policy assignment from the Azure portal](../../governance/policy/assign-policy-portal.md).

1. To launch the Azure Policy service in the Azure portal, select **All services**, and then search for and select **Policy**.

1. In the left pane of the Azure Policy page, select **Assignments**.  
    An assignment is a policy that has been assigned to take place within a specific scope.
    
1. At the top of the **Policy - Assignments** page, select **Assign Initiative**.

1. On the **Assign Initiative** page, select the **Scope** by clicking the ellipsis (...), and select a management group or subscription.  
    In our example, a scope limits the policy assignment to a grouping of virtual machines for enforcement.
    
1. At the bottom of the **Scope** page, save your changes by selecting **Select**.

1. (Optional) To remove one or more resources from the scope, select **Exclusions**.

1. Select the **Initiative definition** ellipsis (...) to display the list of available definitions, select **[Preview] Enable Azure Monitor for VMs**, and then select **Select**.  
    The **Assignment name** box is automatically populated with the initiative name you selected, but you can change it. You can also add an optional description. The **Assigned by** box is automatically populated based on who is logged in, and this value is optional.
    
1. In the **Log Analytics workspace** drop-down list for the supported region, select a workspace.

   > [!NOTE]
   > If the workspace is beyond the scope of the assignment, grant *Log Analytics Contributor* permissions to the policy assignment's Principal ID. If you don't do this, you might see a deployment failure such as: `The client '343de0fe-e724-46b8-b1fb-97090f7054ed' with object id '343de0fe-e724-46b8-b1fb-97090f7054ed' does not have authorization to perform action 'microsoft.operationalinsights/workspaces/read' over scope ...`
   > To grant access, review [how to manually configure the managed identity](../../governance/policy/how-to/remediate-resources.md#manually-configure-the-managed-identity).
   > 
   >  The **Managed Identity** check box is selected, because the initiative being assigned includes a policy with the *deployIfNotExists* effect.
    
1. In the **Manage Identity location** drop-down list, select the appropriate region.

1. Select **Assign**.

#### Review and remediate the compliance results

You can learn how to review compliance results by reading [identify non-compliance results](../../governance/policy/assign-policy-portal.md#identify-non-compliant-resources). In the left pane, select **Compliance**, and then locate the **[Preview] Enable Azure Monitor for VMs** initiative for VMs that aren't compliant according to the assignment you created.

![Policy compliance for Azure VMs](./media/vminsights-onboard/policy-view-compliance-01.png)

Based on the results of the policies included with the initiative, VMs are reported as non-compliant in the following scenarios:

* Log Analytics or the Dependency agent isn't deployed.  
    This scenario is typical for a scope with existing VMs. To mitigate it, deploy the required agents by [creating remediation tasks](../../governance/policy/how-to/remediate-resources.md) on a non-compliant policy.  
    - [Preview]: Deploy Dependency Agent for Linux VMs
    - [Preview]: Deploy Dependency Agent for Windows VMs
    - [Preview]: Deploy Log Analytics Agent for Linux VMs
    - [Preview]: Deploy Log Analytics Agent for Windows VMs

* VM Image (OS) isn't identified in the policy definition.  
    The criteria of the deployment policy include only VMs that are deployed from well-known Azure VM images. Check the documentation to see whether the VM OS is supported. If it isn't supported, duplicate the deployment policy and update or modify it to make the image compliant.  
    - [Preview]: Audit Dependency Agent Deployment – VM Image (OS) unlisted
    - [Preview]: Audit Log Analytics Agent Deployment – VM Image (OS) unlisted

* VMs aren't logging in to the specified Log Analytics workspace.  
    It's possible that some VMs in the initiative scope are logging in to a Log Analytics workspace other than the one that's specified in the policy assignment. This policy is a tool to identify which VMs are reporting to a non-compliant workspace.  
    - [Preview]: Audit Log Analytics Workspace for VM - Report Mismatch

### Enable with PowerShell
To enable Azure Monitor for VMs for multiple VMs or virtual machine scale sets, you can use the PowerShell script [Install-VMInsights.ps1](https://www.powershellgallery.com/packages/Install-VMInsights/1.0), available from the Azure PowerShell Gallery. This script iterates through every virtual machine and virtual machine scale set in your subscription, in the scoped resource group that's specified by *ResourceGroup*, or to a single VM or virtual machine scale set that's specified by *Name*. For each VM or virtual machine scale set, the script verifies whether the VM extension is already installed. If the VM extension is not installed, the script tries to reinstall it. If the VM extension is installed, the script installs the Log Analytics and Dependency agent VM extensions.

This script requires Azure PowerShell module Az version 1.0.0 or later. Run `Get-Module -ListAvailable Az` to find the version. If you need to upgrade, see [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-az-ps). If you're running PowerShell locally, you also need to run `Connect-AzAccount` to create a connection with Azure.

To get a list of the script's argument details and example usage, run `Get-Help`.

```powershell
Get-Help .\Install-VMInsights.ps1 -Detailed

SYNOPSIS
    This script installs VM extensions for Log Analytics and the Dependency agent as needed for VM Insights.


SYNTAX
    .\Install-VMInsights.ps1 [-WorkspaceId] <String> [-WorkspaceKey] <String> [-SubscriptionId] <String> [[-ResourceGroup]
    <String>] [[-Name] <String>] [[-PolicyAssignmentName] <String>] [-ReInstall] [-TriggerVmssManualVMUpdate] [-Approve] [-WorkspaceRegion] <String>
    [-WhatIf] [-Confirm] [<CommonParameters>]


DESCRIPTION
    This script installs or re-configures following on VMs and VM Scale Sets:
    - Log Analytics VM Extension configured to supplied Log Analytics Workspace
    - Dependency Agent VM Extension

    Can be applied to:
    - Subscription
    - Resource Group in a Subscription
    - Specific VM/VM Scale Set
    - Compliance results of a policy for a VM or VM Extension

    Script will show you list of VMs/VM Scale Sets that will apply to and let you confirm to continue.
    Use -Approve switch to run without prompting, if all required parameters are provided.

    If the extensions are already installed will not install again.
    Use -ReInstall switch if you need to for example update the workspace.

    Use -WhatIf if you would like to see what would happen in terms of installs, what workspace configured to, and status of the extension.


PARAMETERS
    -WorkspaceId <String>
        Log Analytics WorkspaceID (GUID) for the data to be sent to

    -WorkspaceKey <String>
        Log Analytics Workspace primary or secondary key

    -SubscriptionId <String>
        SubscriptionId for the VMs/VM Scale Sets
        If using PolicyAssignmentName parameter, subscription that VMs are in

    -ResourceGroup <String>
        <Optional> Resource Group to which the VMs or VM Scale Sets belong

    -Name <String>
        <Optional> To install to a single VM/VM Scale Set

    -PolicyAssignmentName <String>
        <Optional> Take the input VMs to operate on as the Compliance results from this Assignment
        If specified will only take from this source.

    -ReInstall [<SwitchParameter>]
        <Optional> If VM/VM Scale Set is already configured for a different workspace, set this to change to the new workspace

    -TriggerVmssManualVMUpdate [<SwitchParameter>]
        <Optional> Set this flag to trigger update of VM instances in a scale set whose upgrade policy is set to Manual

    -Approve [<SwitchParameter>]
        <Optional> Gives the approval for the installation to start with no confirmation prompt for the listed VMs/VM Scale Sets

    -WorkspaceRegion <String>
        Region the Log Analytics Workspace is in
        Supported values: "East US","eastus","Southeast Asia","southeastasia","West Central US","westcentralus","West Europe","westeurope"
        For Health supported is: "East US","eastus","West Central US","westcentralus"

    -WhatIf [<SwitchParameter>]
        <Optional> See what would happen in terms of installs.
        If extension is already installed will show what workspace is currently configured, and status of the VM extension

    -Confirm [<SwitchParameter>]
        <Optional> Confirm every action

    <CommonParameters>
        This cmdlet supports the common parameters: Verbose, Debug,
        ErrorAction, ErrorVariable, WarningAction, WarningVariable,
        OutBuffer, PipelineVariable, and OutVariable. For more information, see
        about_CommonParameters (https:/go.microsoft.com/fwlink/?LinkID=113216).

    -------------------------- EXAMPLE 1 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup>

    Install for all VMs in a Resource Group in a subscription

    -------------------------- EXAMPLE 2 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup> -ReInstall

    Specify to reinstall extensions even if already installed, for example to update to a different workspace

    -------------------------- EXAMPLE 3 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -PolicyAssignmentName a4f79f8ce891455198c08736 -ReInstall

    Specify to use a PolicyAssignmentName for source, and to reinstall (move to a new workspace)
```

下列範例示範如何在資料夾中使用 PowerShell 命令來啟用適用於 VM 的 Azure 監視器，以及了解預期的輸出：

```powershell
$WorkspaceId = "<GUID>"
$WorkspaceKey = "<Key>"
$SubscriptionId = "<GUID>"
.\Install-VMInsights.ps1 -WorkspaceId $WorkspaceId -WorkspaceKey $WorkspaceKey -SubscriptionId $SubscriptionId -WorkspaceRegion eastus

Getting list of VMs or VM ScaleSets matching criteria specified

VMs or VM ScaleSets matching criteria:

db-ws-1 VM running
db-ws2012 VM running

This operation will install the Log Analytics and Dependency agent extensions on above 2 VMs or VM Scale Sets.
VMs in a non-running state will be skipped.
Extension will not be reinstalled if already installed. Use -ReInstall if desired, for example to update workspace

Confirm
Continue?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

db-ws-1 : Deploying DependencyAgentWindows with name DAExtension
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Deploying DependencyAgentWindows with name DAExtension
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Summary:

Already Onboarded: (0)

Succeeded: (4)
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Connected to different workspace: (0)

Not running - start VM to configure: (0)

Failed: (0)
```

## <a name="enable-for-a-hybrid-environment"></a>針對混合式環境啟用
本節說明如何部署裝載於您資料中心或其他雲端環境的虛擬機器或實體電腦，由適用於 VM 的 Azure 監視器進行監視。

適用於 VM 的 Azure 監視器對應 Dependency Agent 本身不會傳輸任何資料，因此不需變更防火牆或連接埠。 如果您的 IT 安全性原則不允許網路上的電腦連線到網際網路，則對應資料一律會由 Log Analytics 代理程式直接傳輸到 Azure 監視器服務，或透過 [OMS 閘道](../../azure-monitor/platform/gateway.md)傳輸。

檢閱適用於 [Log Analytics Linux 和 Windows 代理程式](../../log-analytics/log-analytics-agent-overview.md)的需求和部署方法。

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

其步驟摘要如下︰

1. 安裝適用於 Windows 或 Linux 的 Log Analytics 代理程式。

1. 針對 [Windows](https://aka.ms/dependencyagentwindows) 或 [Linux](https://aka.ms/dependencyagentlinux)，下載並安裝適用於 VM 的 Azure 監視器對應 Dependency Agent。

1. 啟用收集效能計數器。

1. 部署適用於 VM 的 Azure 監視器。

### <a name="install-the-dependency-agent-on-windows"></a>在 Windows 上安裝 Dependency Agent
您可以執行 `InstallDependencyAgent-Windows.exe` 以手動將 Dependency Agent 安裝在 Windows 電腦上。 如果您在執行此可執行檔時未使用任何選項，則會啟動以互動方式安裝代理程式的安裝精靈。

>[!NOTE]
>必須有*系統管理員*權限，才能安裝或解除安裝代理程式。

下表將強調說明命令列中適用於代理程式的安裝程式所支援的參數。

| 參數 | 描述 |
|:--|:--|
| /? | 傳回命令列選項的清單。 |
| /S | 執行無訊息安裝，而不需要與使用者互動。 |

例如，若要使用 `/?` 參數執行安裝程式，請輸入 **InstallDependencyAgent-Windows.exe /?**。

Windows Dependency Agent 的檔案預設安裝在 *C:\Program Files\Microsoft Dependency Agent* 中。 如果 Dependency Agent 在安裝完成之後無法啟動，請檢查記錄以取得詳細的錯誤資訊。 記錄檔目錄是 *%Programfiles%\Microsoft Dependency Agent\logs*。

### <a name="install-the-dependency-agent-on-linux"></a>在 Linux 上安裝 Dependency Agent
將 Dependency Agent 從 *InstallDependencyAgent-Linux64.bin* (具有自我解壓縮二進位檔的殼層指令碼) 安裝在 Linux 伺服器上。 您可以使用 `sh` 來執行檔案，或對檔案本身新增執行權限。

>[!NOTE]
> 必須有 root 權限，以便安裝或設定代理程式。
>

| 參數 | 描述 |
|:--|:--|
| -help | 取得命令列選項的清單。 |
| -s | 執行無訊息安裝，不會出現任何使用者提示。 |
| --check | 檢查權限和作業系統，但不會安裝代理程式。 |

例如，若要使用 `-help` 參數執行安裝程式，請輸入 **InstallDependencyAgent-Linux64.bin -help**。

執行下列命令，以 root 身分安裝 Linux Dependency Agent：`sh InstallDependencyAgent-Linux64.bin`。

如果 Dependency Agent 無法啟動，請檢查記錄檔以取得詳細的錯誤資訊。 在 Linux 代理程式上，記錄檔的目錄是 /var/opt/microsoft/dependency-agent/log。

Dependency Agent 的檔案位於下列目錄：

| 檔案 | 位置 |
|:--|:--|
| 核心檔案 | /opt/microsoft/dependency-agent |
| 記錄檔 | /var/opt/microsoft/dependency-agent/log |
| 組態檔 | /etc/opt/microsoft/dependency-agent/config |
| 服務可執行檔 | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| 二進位儲存體檔案 | /var/opt/microsoft/dependency-agent/storage |

### <a name="enable-performance-counters"></a>啟用效能計數器
如果解決方案所參考的 Log Analytics 工作區還未設定為收集此解決方案所需的效能計數器，則必須啟用這些效能計數器。 您可以透過下列其中一種方式進行：
* 手動，如 [Log Analytics 中的 Windows 和 Linux 效能資料來源](../../azure-monitor/platform/data-sources-performance-counters.md)中所述
* 下載並執行可從 [Azure PowerShell 資源庫](https://www.powershellgallery.com/packages/Enable-VMInsightsPerfCounters/1.1)取得的 PowerShell 指令碼

### <a name="deploy-azure-monitor-for-vms"></a>部署適用於 VM 的 Azure 監視器
此方法包含一個 JSON 範本，其會指定在 Log Analytics 工作區中啟用解決方案元件的設定。

如果您不熟悉使用範本部署資源，請參閱：
* [使用 Resource Manager 範本與 Azure PowerShell 來部署資源](../../azure-resource-manager/resource-group-template-deploy.md)
* [使用 Resource Manager 範本和 Azure CLI 來部署資源](../../azure-resource-manager/resource-group-template-deploy-cli.md)

如果您選擇使用 Azure CLI，必須先在本機安裝並使用 CLI。 您必須執行 Azure CLI 2.0.27 版或更新版本。 若要知道您使用的版本，請執行 `az --version`。 如果您需要安裝或升級 Azure CLI，請參閱[安裝 Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)。

#### <a name="create-and-execute-a-template"></a>建立並執行範本

1. 複製以下 JSON 語法並貼到您的檔案中：

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "WorkspaceName": {
                "type": "string"
            },
            "WorkspaceLocation": {
                "type": "string"
            }
        },
        "resources": [
            {
                "apiVersion": "2017-03-15-preview",
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('WorkspaceName')]",
                "location": "[parameters('WorkspaceLocation')]",
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },

                        "plan": {
                            "name": "[concat('ServiceMap', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'ServiceMap')]",
                            "promotionCode": ""
                        }
                    },
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },
                        "plan": {
                            "name": "[concat('InfrastructureInsights', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'InfrastructureInsights')]",
                            "promotionCode": ""
                        }
                    }
                ]
            }
        ]
    }
    ```

1. 在本機資料夾中將此檔案儲存為 *installsolutionsforvminsights.json*。

1. 編輯 *WorkspaceName*、*ResourceGroupName* 和 *WorkspaceLocation* 的值。 *WorkspaceName* 的值是您 Log Analytics 工作區的完整資源識別碼，其中包含工作區名稱。 *WorkspaceLocation* 的值是定義工作區所在的區域。

1. 您已經準備好使用下列 PowerShell 命令部署此範本：

    ```powershell
    New-AzResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
    ```

    可能需要幾分鐘的時間才能完成設定變更。 完成之後，將會顯示如下的訊息並包含結果：

    ```powershell
    provisioningState       : Succeeded
    ```
   啟用監視之後，可能需要約 10 分鐘的時間才能檢視混合式電腦的健康狀態和計量。

## <a name="performance-counters-enabled"></a>已啟用效能計數器
適用於 VM 的 Azure 監視器會將 Log Analytics 工作區設定為收集解決方案所使用的效能計數器。 下表列出解決方案每隔 60 秒所收集的物件和計數器。

### <a name="windows-performance-counters"></a>Windows 效能計數器

|物件名稱 |計數器名稱 |
|------------|-------------|
|LogicalDisk |% Free Space |
|LogicalDisk |Avg.Disk sec/Read |
|LogicalDisk |Avg.Disk sec/Transfer |
|LogicalDisk |Avg.Disk sec/Write |
|LogicalDisk |Disk Bytes/sec |
|LogicalDisk |Disk Read Bytes/sec  |
|LogicalDisk |Disk Reads/sec  |
|LogicalDisk |Disk Transfers/sec |
|LogicalDisk |Disk Write Bytes/sec |
|LogicalDisk |Disk Writes/sec |
|LogicalDisk |Free Megabytes |
|記憶體 |可用的 MB |
|網路介面卡 |Bytes Received/sec |
|網路介面卡 |Bytes Sent/sec |
|處理器 |% Processor Time |

### <a name="linux-performance-counters"></a>Linux 效能計數器

|物件名稱 |計數器名稱 |
|------------|-------------|
|Logical Disk |% Used Space |
|Logical Disk |Disk Read Bytes/sec  |
|Logical Disk |Disk Reads/sec  |
|Logical Disk |Disk Transfers/sec |
|Logical Disk |Disk Write Bytes/sec |
|Logical Disk |Disk Writes/sec |
|Logical Disk |Free Megabytes |
|Logical Disk |Logical Disk Bytes/sec |
|記憶體 |Available MBytes Memory |
|網路 |Total Bytes Received |
|網路 |Total Bytes Transmitted |
|處理器 |% Processor Time |

## <a name="diagnostic-and-usage-data"></a>診斷和使用量資料
當您使用 Azure 監視器服務時，Microsoft 會自動收集使用量和效能資料。 Microsoft 會使用此資料來提供並改進服務的品質、安全性和完整性。 為了提供正確且有效率的疑難排解功能，來自對應功能的資料會包含軟體設定的相關資訊，例如作業系統與版本、IP 位址、DNS 名稱和工作站名稱。 Microsoft 不會收集姓名、地址或其他連絡資訊。

如需有關資料收集與使用方式的詳細資訊，請參閱 [Microsoft 線上服務隱私權聲明](https://go.microsoft.com/fwlink/?LinkId=512132)。

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]
## <a name="next-steps"></a>後續步驟

既然已經為您的虛擬機器啟用監視，適用於 VM 的 Azure 監視器就能使用此資訊來進行分析。 若要了解如何使用健康情況功能，請參閱[檢視適用於 VM 的 Azure 監視器健康情況](vminsights-health.md)。 若要檢視探索到的應用程式相依性，請參閱[檢視適用於 VM 的 Azure 監視器對應](vminsights-maps.md)。
