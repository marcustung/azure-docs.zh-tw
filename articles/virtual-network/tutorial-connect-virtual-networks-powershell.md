---
title: 使用虛擬網路對等互連連線虛擬網路 - PowerShell | Microsoft Docs
description: 在本文中，您將了解如何使用 Azure PowerShell，透過虛擬網路對等互連來連線虛擬網路。
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to connect two virtual networks so that virtual machines in one virtual network can communicate with virtual machines in the other virtual network.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 49a6c91587905a8f7086b46b275a5078197939eb
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/21/2019
ms.locfileid: "56649946"
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-powershell"></a>使用 PowerShell 以虛擬網路對等互連連線虛擬網路

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

您可以使用虛擬網路對等互連，讓虛擬網路彼此連線。 一旦虛擬網路對等互連，兩個虛擬網路中的資源就可以彼此通訊，且通訊時會有相同的延遲和頻寬，彷彿這些資源是位於相同的虛擬網路中。 在本文中，您將了解：

* 建立兩個虛擬網路
* 使用虛擬網路對等互連連線兩個虛擬網路
* 將虛擬機器 (VM) 部署到每個虛擬網路
* 虛擬機器之間的通訊

如果您沒有 Azure 訂用帳戶，請在開始前建立 [免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) 。

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

如果您選擇要安裝在本機使用 PowerShell，本文需要 Azure PowerShell 模組版本 1.0.0 或更新版本。 執行 `Get-Module -ListAvailable Az` 來了解安裝的版本。 如果您需要升級，請參閱[安裝 Azure PowerShell 模組](/powershell/azure/install-az-ps)。 如果您在本機執行 PowerShell，則也需要執行 `Connect-AzAccount` 以建立與 Azure 的連線。

## <a name="create-virtual-networks"></a>建立虛擬網路

建立虛擬網路之前，您必須為虛擬網路以及在本文中建立的所有其他資源，建立資源群組。 使用 [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) 來建立資源群組。 下列範例會在 eastus 位置建立名為 myResourceGroup 的資源群組。

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

使用 [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) 建立虛擬網路。 下列範例會建立名為 myVirtualNetwork1 的虛擬網路，其位址首碼為 10.0.0.0/16。

```azurepowershell-interactive
$virtualNetwork1 = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork1 `
  -AddressPrefix 10.0.0.0/16
```

建立的子網路設定[新增 AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig)。 下列範例會建立具有 10.0.0.0/24 位址首碼的子網路組態：

```azurepowershell-interactive
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork1
```

子網路組態寫入虛擬網路[組 AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork)，這會建立子網路：

```azurepowershell-interactive
$virtualNetwork1 | Set-AzVirtualNetwork
```

建立具有 10.1.0.0/16 位址首碼和一個子網路的虛擬網路：

```azurepowershell-interactive
# Create the virtual network.
$virtualNetwork2 = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork2 `
  -AddressPrefix 10.1.0.0/16

# Create the subnet configuration.
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.1.0.0/24 `
  -VirtualNetwork $virtualNetwork2

# Write the subnet configuration to the virtual network.
$virtualNetwork2 | Set-AzVirtualNetwork
```

## <a name="peer-virtual-networks"></a>對等互連虛擬網路

建立具有對等互連[新增 AzVirtualNetworkPeering](/powershell/module/az.network/add-azvirtualnetworkpeering)。 下列範例會將 myVirtualNetwork1 對等互連至 myVirtualNetwork2。

```azurepowershell-interactive
Add-AzVirtualNetworkPeering `
  -Name myVirtualNetwork1-myVirtualNetwork2 `
  -VirtualNetwork $virtualNetwork1 `
  -RemoteVirtualNetworkId $virtualNetwork2.Id
```

在執行上一個命令之後所傳回的輸出中，您會看到 **PeeringState** 是「已啟動」。 對等互連會持續處於「已啟動」狀態，直到您建立從 myVirtualNetwork2 到 myVirtualNetwork1 的對等互連為止。 建立從 myVirtualNetwork2 到 myVirtualNetwork1 的對等互連。

```azurepowershell-interactive
Add-AzVirtualNetworkPeering `
  -Name myVirtualNetwork2-myVirtualNetwork1 `
  -VirtualNetwork $virtualNetwork2 `
  -RemoteVirtualNetworkId $virtualNetwork1.Id
```

在執行上一個命令之後所傳回的輸出中，您會看到 **PeeringState** 是「已連線」。 Azure 也會將 myVirtualNetwork1-myVirtualNetwork2 對等互連的對等互連狀態變更為「已連線」。 確認已選取的對等互連狀態*myVirtualNetwork1-myVirtualNetwork2*對等互連變更為*Connected*具有[Get AzVirtualNetworkPeering](/powershell/module/az.network/get-azvirtualnetworkpeering)。

```azurepowershell-interactive
Get-AzVirtualNetworkPeering `
  -ResourceGroupName myResourceGroup `
  -VirtualNetworkName myVirtualNetwork1 `
  | Select PeeringState
```

兩個虛擬網路之對等互連的 **PeeringState** 都是「已連線」之前，其中一個虛擬網路中的資源無法與另一個虛擬網路中的資源通訊。

## <a name="create-virtual-machines"></a>建立虛擬機器

在每個虛擬網路中建立虛擬機器，以便您可以在稍後的步驟中於彼此之間通訊。

### <a name="create-the-first-vm"></a>建立第一個 VM

使用 [New-AzVM](/powershell/module/az.compute/new-azvm) 建立 VM。 下列範例會在 myVirtualNetwork1 虛擬網路中建立名為 myVm1 的虛擬機器。 `-AsJob` 選項會在背景建立虛擬機器，以便您繼續進行下一步。 出現提示時，輸入您想要用來登入虛擬機器的使用者名稱和密碼。

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork1" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm1" `
  -AsJob
```

### <a name="create-the-second-vm"></a>建立第二個 VM

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork2" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm2"
```

建立 VM 需要幾分鐘的時間。 在 Azure 建立虛擬機器並將輸出傳回至 PowerShell 之前，請勿繼續進行後續步驟。

## <a name="communicate-between-vms"></a>虛擬機器之間的通訊

您可以從網際網路連線至 VM 的公用 IP 位址。 請使用 [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) 來傳回 VM 的公用 IP 位址。 以下範例會傳回 *myVm1* VM 的公用 IP 位址：

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

請從您的本機電腦使用下列命令，來建立與 myVm1 虛擬機器的遠端桌面工作階段。 以上一個命令傳回的 IP 位址取代 `<publicIpAddress>`。

```
mstsc /v:<publicIpAddress>
```

將會建立一個「遠端桌面通訊協定」(.rdp) 檔案、下載至您的電腦並開啟。 輸入使用者名稱和密碼 (您可能需要選取 [更多選擇]，然後選取 [使用不同的帳戶] 以指定您在建立虛擬機器時輸入的認證)，然後按一下 [確定]。 您可能會在登入過程中收到憑證警告。 按一下 [是] 或 [繼續] 以繼續進行連線。

在 myVm1 虛擬機器上，讓網際網路控制訊息通訊協定 (ICMP) 通過 Windows 防火牆，您就可以在稍後的步驟中使用 PowerShell 從 myVm2 針對此虛擬機器進行 Ping 操作：

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

雖然本文使用 Ping 在虛擬機器之間進行通訊，但不建議在生產環境部署中允許 ICMP 通過 Windows 防火牆。

若要連線至 myVm2 虛擬機器，請在 myVm1 虛擬機器上從命令提示字元輸入下列命令：

```
mstsc /v:10.1.0.4
```

由於您在 myVm1 上面啟用 Ping，您現在可以在 myVm2 虛擬機器上從命令提示字元依據 IP 位址來 Ping 它：

```
ping 10.0.0.4
```

您會收到四個回覆。 同時中斷與 myVm1 和 myVm2 的 RDP 工作階段。

## <a name="clean-up-resources"></a>清除資源

若不再需要使用[移除 AzResourcegroup](/powershell/module/az.resources/remove-azresourcegroup)來移除資源群組和所有其包含之資源。

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>後續步驟

在本文中，您已了解如何使用虛擬網路對等互連來連線相同 Azure 區域中的兩個網路。 您也可以針對不同[支援區域](virtual-network-manage-peering.md#cross-region)和[不同 Azure 訂用帳戶](create-peering-different-subscriptions.md#powershell)中的虛擬網路進行對等互連，以及使用對等互連來建立[中樞和輪輻網路設計](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering)。 若要深入了解虛擬網路對等互連，請參閱[虛擬網路對等互連概觀](virtual-network-peering-overview.md)和[管理虛擬網路對等互連](virtual-network-manage-peering.md)。

您可以透過 VPN [將自己的電腦連線到虛擬網路](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json)，並且與虛擬網路中或已對等互連虛擬網路中的資源進行互動。 如需可重複使用的指令碼來完成虛擬網路文章中涵蓋的許多工作，請參閱[指令碼範例](powershell-samples.md)。