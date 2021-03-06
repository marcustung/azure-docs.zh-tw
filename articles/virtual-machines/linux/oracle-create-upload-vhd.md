---
title: 建立及上傳 Oracle Linux VHD | Microsoft Docs
description: 了解如何建立及上傳包含 Oracle Linux 作業系統的 Azure 虛擬硬碟 (VHD)。
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: jeconnoc
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: dd96f771-26eb-4391-9a89-8c8b6d691822
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: szark
ms.openlocfilehash: ecd30d30434d91893102ce6ec0df21daa84b677c
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59276829"
---
# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>为 Azure 准备 Oracle Linux 虚拟机
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>必要條件
本文假設您已將 Oracle Linux 作業系統安裝到虛擬硬碟。 有多個工具可用來建立 .vhd 檔案，例如，像是 Hyper-V 的虛擬化解決方案。 如需指示，請參閱 [安裝 Hyper-V 角色及設定虛擬機器](https://technet.microsoft.com/library/hh846766.aspx)。

### <a name="oracle-linux-installation-notes"></a>Oracle Linux 安裝注意事項
* 如需有關準備 Azure 之 Linux 的更多秘訣，另請參閱 [一般 Linux 安裝注意事項](create-upload-generic.md#general-linux-installation-notes) 。
* Hyper-V 和 Azure 同时支持 Oracle 的 Red Hat 兼容内核及其 UEK3（坚不可摧企业内核）。 若要獲得最佳結果，請在準備執行 Oracle Linux VHD 的同時，確實更新到最新核心。
* Hyper-V 和 Azure 不支援 Oracle 的 UEK2，因為它不包含必要的驅動程式。
* Azure 不支援 VHDX 格式，只支援 **固定 VHD**。  您可以使用 Hyper-V 管理員或 convert-vhd Cmdlet，將磁碟轉換為 VHD 格式。
* 安裝 Linux 系統時，建議您使用標準磁碟分割而不是 LVM (常是許多安裝的預設設定)。 這可避免 LVM 與複製之虛擬機器的名稱衝突，特別是為了疑難排解而需要將作業系統磁碟連接至其他虛擬機器時。 如果願意，您可以在資料磁碟上使用 [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 或 [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。
* 由於 2.6.37 以下的 Linux 核心版本有錯誤，因此較大的 VM 不支援 NUMA。 這個問題主要會影響使用上游 Red Hat 2.6.32 kernel 的散發套件。 手动安装的 Azure Linux 代理 (waagent) 将自动在 Linux 内核的 GRUB 配置中禁用 NUMA。 您可以在以下步驟中找到與此有關的詳細資訊。
* 請勿在作業系統磁碟上設定交換磁碟分割。 您可以設定 Linux 代理程式在暫存資源磁碟上建立交換檔。  您可以在以下步驟中找到與此有關的詳細資訊。
* Azure 上的所有 VHD 必須具有與 1 MB 對應的虛擬大小。 從未經處理的磁碟轉換成 VHD 時，您必須確定未經處理的磁碟大小在轉換前是 1 MB 的倍數。 如需詳細資訊，請參閱 [Linux 安裝注意事項](create-upload-generic.md#general-linux-installation-notes)。
* 確定已啟用 `Addons` 儲存機制。 編輯檔案`/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) 或`/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux 7)，並變更這一行`enabled=0`要`enabled=1`之下 **[ol6_addons]** 或 **[ol7_addons]** 此檔案中。

## <a name="oracle-linux-64"></a>Oracle Linux 6.4+
你必须在操作系统中完成特定的配置步骤才能使虚拟机在 Azure 中运行。

1. 在 Hyper-V 管理員的中間窗格中，選取虛擬機器。
2. 按一下 **[連接]** ，以開啟虛擬機器的視窗。
3. 執行下列命令以解除安裝 NetworkManager：
   
        # sudo rpm -e --nodeps NetworkManager
   
    **附註：** 如果尚未安裝封裝，此命令將會失敗，並出現錯誤訊息。 這是預期行為。
4. 在 `/etc/sysconfig/` 目錄中，建立名為 **network** 且包含下列文字的檔案：
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
5. 在包含以下文本的 `/etc/sysconfig/network-scripts/` 目录中创建一个名为 **ifcfg-eth0** 的文件：
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
6. 修改 udev 角色可防止產生乙太網路介面的靜態規則。 在 Microsoft Azure 或 Hyper-V 中複製虛擬機器時，這些規則可能會造成問題：
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
7. 要確保開機時會啟動網路服務，可執行以下命令：
   
        # chkconfig network on
8. 通过运行以下命令安装 python-pyasn1：
   
        # sudo yum install python-pyasn1
9. 在 grub 配置中修改内核引导行，以使其包含 Azure 的其他内核参数。 作法是，在文字編輯器中開啟 "/boot/grub/menu.lst"，並確定預設核心包含以下參數：
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off
   
   這也將確保所有主控台訊息都會傳送給第一個序列埠，有助於 Azure 支援團隊進行問題偵錯程序。 由于 Oracle 的 Red Hat 兼容内核中的一个 bug，这将禁用 NUMA。
   
   除了上述以外，我們還建議您 *移除* 下列參數：
   
        rhgb quiet crashkernel=auto
   
   在雲端環境中，我們會將所有記錄傳送到序列埠，因此不適合使用圖形化和無訊息啟動。
   
   如有需要，您可以保留 `crashkernel` 選項的設定，但請注意，此參數將會減少 VM 中約 128MB 或以上的可用記憶體數量，這在較小的 VM 中可能會是個問題。
10. 確定您已安裝 SSH 伺服器，並已設定為在開機時啟動。  這通常是預設值。
11. 執行以下命令來安裝 Azure Linux 代理程式。 最新版為 2.0.15。
    
        # sudo yum install WALinuxAgent
    
    請注意，如果 NetworkManager 和 NetworkManager-gnome 套件沒有如步驟 2 所述遭到移除，則在安裝 WALinuxAgent 套件時會將這兩個套件移除。
12. 請勿在作業系統磁碟上建立交換空間。
    
    Azure Linux 代理可使用在 Azure 上设置后附加到虚拟机的本地资源磁盘自动配置交换空间。 請注意，資源磁碟是 *暫存* 磁碟，可能會在 VM 取消佈建時清空。 安裝 Azure Linux 代理程式 (請參閱上一個步驟) 後，請在 /etc/waagent.conf 中適當修改下列參數：
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
13. 執行下列命令，以取消佈建虛擬機器，並準備將它佈建於 Azure 上：
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
14. 在 Hyper-V 管理器中单击“操作”->“关闭”。 您現在可以將 Linux VHD 上傳至 Azure。

- - -
## <a name="oracle-linux-70"></a>Oracle Linux 7.0+
**Oracle Linux 7 中的變更**

準備適用於 Azure 的 Oracle Linux 7 虛擬機器會與 Oracle Linux 6 極為類似，不過，其中有幾個重要差異值得注意：

* Azure 支援 Red Hat 相容核心和 Oracle 的 UEK3。  建議使用 UEK3 核心。
* NetworkManager 封裝不會再與 Azure Linux 代理程式發生衝突。 依預設會安裝此封裝，建議您不要將它移除。
* GRUB2 现在用作默认引导加载程序，因此编辑内核参数的过程已更改（见下文）。
* XFS 現為預設的檔案系統。 如有需要，您仍可使用 ext4 檔案系統。

**組態步驟**

1. 在 Hyper-V 管理員中，選取虛擬機器。
2. 按一下 [連接] ，以開啟虛擬機器的主控台視窗。
3. 在包含以下文本的 `/etc/sysconfig/` 目录中创建一个名为 **network** 的文件：
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
4. 在包含以下文本的 `/etc/sysconfig/network-scripts/` 目录中创建一个名为 **ifcfg-eth0** 的文件：
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
5. 修改 udev 角色可防止產生乙太網路介面的靜態規則。 在 Microsoft Azure 或 Hyper-V 中複製虛擬機器時，這些規則可能會造成問題：
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
6. 要確保開機時會啟動網路服務，可執行以下命令：
   
        # sudo chkconfig network on
7. 執行下列命令以安裝 python-pyasn1 封裝：
   
        # sudo yum install python-pyasn1
8. 執行下列命令，以清除目前的 yum 中繼資料並安裝任何更新：
   
        # sudo yum clean all
        # sudo yum -y update
9. 修改 grub 組態中的核心開機那一行，使其額外包含用於 Azure 的核心參數。 若要執行這個動作，請在文字編輯器中開啟 "/etc/default/grub" 並編輯 `GRUB_CMDLINE_LINUX` 參數，例如：
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   這也將確保所有主控台訊息都會傳送給第一個序列埠，有助於 Azure 支援團隊進行問題偵錯程序。 也會關閉新的 OEL 7 對 NIC 的命名慣例。 除此之外，建议*删除*以下参数：
   
       rhgb quiet crashkernel=auto
   
   在雲端環境中，我們會將所有記錄傳送到序列埠，因此不適合使用圖形化和無訊息啟動。
   
   如有需要，您可以保留 `crashkernel` 選項的設定，但請注意，此參數將會減少 VM 中約 128MB 或以上的可用記憶體數量，這在較小的 VM 中可能會是個問題。
10. 在您參照上述完成編輯 "/etc/default/grub" 之後，請執行下列命令以重建 grub 組態：
    
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
11. 確定您已安裝 SSH 伺服器，並已設定為在開機時啟動。  這通常是預設值。
12. 執行以下命令來安裝 Azure Linux 代理程式：
    
        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent
13. 請勿在作業系統磁碟上建立交換空間。
    
    Azure Linux 代理可使用在 Azure 上设置后附加到虚拟机的本地资源磁盘自动配置交换空间。 請注意，資源磁碟是 *暫存* 磁碟，可能會在 VM 取消佈建時清空。 在安装 Azure Linux 代理（请参见前一步骤）后，相应地在 /etc/waagent.conf 中修改以下参数：
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.
14. 執行下列命令，以取消佈建虛擬機器，並準備將它佈建於 Azure 上：
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
15. 在 Hyper-V 管理器中单击“操作”->“关闭”。 Linux VHD 现已准备好上传到 Azure。

## <a name="next-steps"></a>後續步驟
您現在可以開始使用您的 Oracle Linux .vhd 在 Azure 中建立新的虛擬機器。 如果您是第一次將 .vhd 檔案上傳至 Azure，請參閱[從自訂磁碟建立 Linux VM](upload-vhd.md#option-1-upload-a-vhd)。

