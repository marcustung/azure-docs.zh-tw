---
author: kumudD
ms.service: load-balancer
ms.topic: include
ms.date: 11/09/2018
ms.author: kumud
ms.openlocfilehash: bf3aee30460e052db23cf9090f13e2af3b22628b
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2018
ms.locfileid: "52269304"
---
下列工作將會在此案例中完成：

* 建立負載平衡器，以便在連接埠 80 上接收網路流量，並將負載平衡的流量傳送到虛擬機器 "web1" 和 "web2"
* 建立 NAT 規則，以針對位於負載平衡器後面的虛擬機器進行遠端桌面存取/SSH
* 建立健全狀態探查

![負載平衡器案例](./media/load-balancer-get-started-internet-scenario-include/scenario-classic.png)
