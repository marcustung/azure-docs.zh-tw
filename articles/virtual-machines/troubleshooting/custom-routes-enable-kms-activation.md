---
title: 使用 Azure 自訂路由透過強制通道啟用 KMS 啟用 | Microsoft Docs
description: 示範當在 Azure 中使用強制通道時，如何使用 Azure 自訂路由啟用 KMS 啟用。
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 12/20/2018
ms.author: genli
ms.openlocfilehash: b121996530ea0618fc757f1ae12dfafde10ed7bb
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/09/2019
ms.locfileid: "55979372"
---
# <a name="windows-activation-fails-in-forced-tunneling-scenario"></a>強制通道案例中的 Windows 啟用失敗

本文說明如何解決在站對站 VPN 連線或 ExpressRoute 案例中啟用強制通道時可能遇到的 KMS 啟用問題。

## <a name="symptom"></a>徵狀

您在 Azure 虛擬網路子網路上啟用[強制通道](../../vpn-gateway/vpn-gateway-forced-tunneling-rm.md)，以將所有網際網路繫結流量導向回到您的內部部署網路。 在此案例中，執行 Windows Server 2012 R2 (或更新版本的 Windows) 的 Azure 虛擬機器 (VM) 可以成功啟動 Windows。 不過，執行舊版 Windows 的 VM 無法啟動 Windows。

## <a name="cause"></a>原因

Azure Windows VM 需要連接到 Azure KMS 伺服器以進行 Windows 啟用。 若要啟用，啟用要求須來自 Azure 的公用 IP 位址。 在強制通道的案例中，啟用會失敗，因為啟用要求來自內部部署網路，而不是 Azure 的公用 IP 位址。

## <a name="solution"></a>解決方法

若要解決此問題，請使用 Azure 自訂路由將啟用流量路由到 Azure KMS 伺服器。

Azure Global 雲端其 KMS 伺服器的 IP 位址是 23.102.135.246。 其 DNS 名稱是 kms.core.windows.net。 如果您使用其他 Azure 平台 (例如 Azure Germany)，則必須使用對應 KMS 伺服器的 IP 位址。 如需詳細資訊，請參閱下列表格：

|平台| KMS DNS|KMS IP|
|------|-------|-------|
|Azure 全域|kms.core.windows.net|23.102.135.246|
|Azure Germany|kms.core.cloudapi.de|51.4.143.248|
|Azure US Government|kms.core.usgovcloudapi.net|23.97.0.13|
|Azure China 21Vianet|kms.core.chinacloudapi.cn|42.159.7.249|


若要新增自訂的路由，請遵循下列步驟：

### <a name="for-resource-manager-vms"></a>針對 Resource Manager VM

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

1. 開啟 Azure PowerShell，然後[登入您的 Azure 訂用帳戶](https://docs.microsoft.com/powershell/azure/authenticate-azureps)。
2. 執行下列命令：

    ```powershell
    # First, get the virtual network that hosts the VMs that have activation problems. In this case, we get virtual network ArmVNet-DM in Resource Group ArmVNet-DM:

    $vnet = Get-AzVirtualNetwork -ResourceGroupName "ArmVNet-DM" -Name "ArmVNet-DM"

    # Next, create a route table and specify that traffic bound to the KMS IP (23.102.135.246) will go directly out:

    $RouteTable = New-AzRouteTable -Name "ArmVNet-DM-KmsDirectRoute" -ResourceGroupName "ArmVNet-DM" -Location "centralus"

    Add-AzRouteConfig -Name "DirectRouteToKMS" -AddressPrefix 23.102.135.246/32 -NextHopType Internet -RouteTable $RouteTable

    Set-AzRouteTable -RouteTable $RouteTable
    ```
3. 移至有啟用問題的 VM。 使用 [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) 測試是否能與 KMS 伺服器連線：

        psping kms.core.windows.net:1688

4. 嘗試啟動 Windows 並查看問題是否已解決。

### <a name="for-classic-vms"></a>適用於傳統 VM

1. 開啟 Azure PowerShell，然後[登入您的 Azure 訂用帳戶](https://docs.microsoft.com/powershell/azure/authenticate-azureps)。
2. 執行下列命令：

    ```powershell
    # First, create a new route table:
    New-AzureRouteTable -Name "VNet-DM-KmsRouteGroup" -Label "Route table for KMS" -Location "Central US"

    # Next, get the route table that was created:
    $rt = Get-AzureRouteTable -Name "VNet-DM-KmsRouteTable"

    # Next, create a route:
    Set-AzureRoute -RouteTable $rt -RouteName "AzureKMS" -AddressPrefix "23.102.135.246/32" -NextHopType Internet

    # Apply the KMS route table to the subnet that hosts the problem VMs (in this case, we apply it to the subnet that's named Subnet-1):
    Set-AzureSubnetRouteTable -VirtualNetworkName "VNet-DM" -SubnetName "Subnet-1" 
    -RouteTableName "VNet-DM-KmsRouteTable"
    ```

3. 移至有啟用問題的 VM。 使用 [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) 測試是否能與 KMS 伺服器連線：

        psping kms.core.windows.net:1688

4. 嘗試啟動 Windows 並查看問題是否已解決。

## <a name="next-steps"></a>後續步驟

- [KMS 用戶端安裝識別碼](https://docs.microsoft.com/windows-server/get-started/kmsclientkeys
)
- [檢閱並選取啟用方法](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134256(v=ws.11)
)
