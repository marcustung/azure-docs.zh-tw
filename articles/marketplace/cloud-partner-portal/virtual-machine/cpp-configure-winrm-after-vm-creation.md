---
title: 在建立 Azure 虛擬機器之後設定 WinRM | Microsoft Docs
description: 說明如何在建立 Azure 託管的虛擬機器之後，設定 Windows 遠端管理 (WinRM)。
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: pbutlerm
ms.openlocfilehash: 6350d5bde737c46cf14a9aef75a7ec57260a6afa
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2018
ms.locfileid: "53195684"
---
# <a name="configure-winrm-after-virtual-machine-creation"></a>在建立虛擬機器之後設定 WinRM

本文說明如何設定現有的 Azure 託管虛擬機器 (VM) 來啟用 WinRM over HTTPS。  此設定僅適用於 Windows 型 VM，而且需要下列雙步驟程序：

1. 針對 WinRM over HTTPS 通訊協定啟用連接埠流量。  您將在 Azure 入口網站中為 VM 設定此設定。
2. 設定 VM，藉由執行提供的 PowerShell 指令碼來啟用 WinRM。


## <a name="enabling-port-traffic"></a>啟用連接埠流量

WinRM over HTTPS 通訊協定會使用連接埠 5896，預設不會在 Azure Marketplace 上提供的預先設定 Windows VM 上啟用。 若要啟用此通訊協定，請使用下列步驟，透過 [Azure 入口網站](https://portal.azure.com)來將新規則新增至網路安全性群組 (NSG)。  如需有關 NSG 的詳細資訊，請參閱[安全性群組](https://docs.microsoft.com/azure/virtual-network/security-overview)。

1.  瀏覽至刀鋒視窗 [虛擬機器] > [  <*vm-name*>  ] > [設定/網路]。
2.  按一下 NSG 名稱 (在此範例中為 **testvm11002**) 以顯示其屬性：

    ![網路安全性群組屬性](./media/nsg-properties.png)
 
3. 在 [設定] 底下，選取 [輸入安全性規則] 來顯示此刀鋒視窗。
4. 按一下 [+ 新增]，以建立適用於 TCP 連接埠 5986 的新規則 (稱為 `WinRM_HTTPS`)。

    ![新增輸入網路安全性規則](./media/nsg-new-rule.png)

5. 當您完成提供值時，按一下 [確定]。  輸入安全性規則的清單應該包含下列新項目。

    ![輸入網路安全性群組規則的清單](./media/nsg-new-inbound-listing.png)


## <a name="configure-vm-to-enable-winrm"></a>設定 VM 以啟用 WinRM 

使用下列步驟，在您的 Windows VM 上啟用並設定 Windows 遠端管理功能。   

1. 對您 Azure 託管的 VM 建立遠端桌面連線。  如需詳細資訊，請參閱[如何連接和登入執行 Windows 的 Azure 虛擬機器](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon)。  其餘的步驟將會在您的 VM 上執行。
2. 下載下列檔案，並將它們儲存到您 VM 上的資料夾中：
    - [ConfigureWinRM.ps1](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/ConfigureWinRM.ps1)
    - [makecert.exe](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/makecert.exe)
    - [winrmconf.cmd](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-winrm-windows/winrmconf.cmd)
3. 使用較高的權限 (**以系統管理員身分執行**) 來開啟 [PowerShell 主控台]。 
4. 執行下列命令並提供必要的參數：您 VM 的完整網域名稱 (FQDN)： <br/>
   `ConfigureWinRM.ps1 <vm-domain-name>`

    ![PowerShell 設定指令碼 1](./media/powershell-file1.png)

    此指令碼取決於相同資料夾中的另外兩個檔案。


## <a name="next-steps"></a>後續步驟

一旦設定 WinRM 之後，您就準備好[從其組成的 VHD 部署 VM](./cpp-deploy-vm-vhd.md)。
