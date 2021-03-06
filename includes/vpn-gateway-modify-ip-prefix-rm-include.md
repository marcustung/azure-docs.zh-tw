---
title: 包含檔案
description: 包含檔案
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/14/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 13089a2514229c5c5bc7b40d9447719247b23405
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "58115577"
---
### <a name="noconnection"></a>修改區域網路閘道 IP 位址首碼 - 沒有閘道連線

若要新增其他位址首碼：

1. 設定 LocalNetworkGateway 的變數。

   ```azurepowershell-interactive
   $local = Get-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1
   ```
2. 修改首碼。

   ```azurepowershell-interactive
   Set-AzLocalNetworkGateway -LocalNetworkGateway $local `
   -AddressPrefix @('10.101.0.0/24','10.101.1.0/24','10.101.2.0/24')
   ```

若要移除位址首碼：

  省略您不再需要的首碼。 此範例中不再需要首碼 10.101.2.0/24 (來自先前的範例)，因此我們將更新區域網路閘道，並排除該首碼。

1. 設定 LocalNetworkGateway 的變數。

   ```azurepowershell-interactive
   $local = Get-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1
   ```
2. 使用更新的首碼設定閘道。

   ```azurepowershell-interactive
   Set-AzLocalNetworkGateway -LocalNetworkGateway $local `
   -AddressPrefix @('10.101.0.0/24','10.101.1.0/24')
   ```

### <a name="withconnection"></a>修改區域網路閘道 IP 位址首碼 - 現有閘道連線

如果有一个网关连接并且想要添加或删除包含在本地网关中的 IP 地址前缀，则需要按顺序执行以下步骤。 這會導致您 VPN 連線的停機時間。 修改 IP 位址首碼時，您不需要刪除 VPN 閘道。 您只需要移除連線。

1. 移除連線。

   ```azurepowershell-interactive
   Remove-AzVirtualNetworkGatewayConnection -Name VNet1toSite1 -ResourceGroupName TestRG1
   ```
2. 使用已修改的位址首碼設定區域網路閘道。
   
   設定 LocalNetworkGateway 的變數。

   ```azurepowershell-interactive
   $local = Get-AzLocalNetworkGateway -Name Site1 -ResourceGroupName TestRG1
   ```
   
   修改首碼。
   
   ```azurepowershell-interactive
   Set-AzLocalNetworkGateway -LocalNetworkGateway $local `
   -AddressPrefix @('10.101.0.0/24','10.101.1.0/24')
   ```
3. 创建连接。 在此範例中，我們會設定 IPsec 連線類型。 當您重新建立連線時，請使用針對設定指定的連線類型。 對於其他連線類型，請參閱 [PowerShell Cmdlet](https://msdn.microsoft.com/library/mt603611.aspx) 頁面。
   
   設定 VirtualNetworkGateway 的變數。

   ```azurepowershell-interactive
   $gateway1 = Get-AzVirtualNetworkGateway -Name VNet1GW  -ResourceGroupName TestRG1
   ```
   
   建立連線。 此範例使用您在步驟 2 中設定的 $local 變數。

   ```azurepowershell-interactive
   New-AzVirtualNetworkGatewayConnection -Name VNet1toSite1 `
   -ResourceGroupName TestRG1 -Location 'East US' `
   -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
   -ConnectionType IPsec `
   -RoutingWeight 10 -SharedKey 'abc123'
   ```