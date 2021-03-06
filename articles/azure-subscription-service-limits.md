---
title: Azure 訂用帳戶限制與配額
description: 提供通用的 Azure 訂用帳戶和服務限制、配額和條件約束的清單。 這篇文章包含有關如何增加限制和最大值。
services: multiple
author: rothja
manager: jeffreyg
tags: billing
ms.assetid: 60d848f9-ff26-496e-a5ec-ccf92ad7d125
ms.service: billing
ms.topic: article
ms.date: 12/10/2018
ms.author: byvinyal
ms.openlocfilehash: a32ebb3fc2448b189aa5970446e2ceb29aed51e2
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2019
ms.locfileid: "57838777"
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure 訂用帳戶和服務限制、配額與限制
本文件列出一些最常見的 Microsoft Azure 限制，有時也稱為配額。 本文件目前未涵蓋所有 Azure 服務。 經過一段時間，會展開並更新以涵蓋更多服務 清單。

若要深入了解 Azure 定價，請參閱[Azure 定價概觀](https://azure.microsoft.com/pricing/)。 那里，預估成本，以及在使用[定價計算機](https://azure.microsoft.com/pricing/calculator/)。 您也可以移至特定服務的定價詳細資料頁面，例如[Windows Vm](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)。 如需協助您管理成本的祕訣，請參閱[使用 Azure 計費與成本管理避免非預期的成本](billing/billing-getting-started.md)。

> [!NOTE]
> 如果您想要限制或配額超過預設限制，會引發[開啟線上客戶支援要求，不另外加收費用](azure-resource-manager/resource-manager-quota-errors.md)。 無法將限制提升到高於下表所示的最大限制值。 如果沒有最大限制的資料行，該資源不會有可調整的限制。
>
> [免費試用版訂用帳戶](https://azure.microsoft.com/offers/ms-azr-0044p)不符合限制或配額增加的資格。 如果您有[免費試用訂用帳戶](https://azure.microsoft.com/offers/ms-azr-0044p)，則可以升級到[預付型方案](https://azure.microsoft.com/offers/ms-azr-0003p/)訂用帳戶。 如需詳細資訊，請參閱 < [Azure 免費試用訂用帳戶升級為隨用隨付訂用帳戶](billing/billing-upgrade-azure-subscription.md)並[免費試用訂用帳戶常見問題集](https://azure.microsoft.com/free/free-account-faq)。
>

## <a name="limits-and-azure-resource-manager"></a>限制和 Azure Resource Manager
您就可以將多個 Azure 資源結合成單一的 Azure 資源群組。 當您使用資源群組時，限制是，在全域變成管理區域層級使用 Azure Resource Manager。 如需 Azure 資源群組的詳細資訊，請參閱[Azure Resource Manager 概觀](azure-resource-manager/resource-group-overview.md)。

在下列清單中的限制，新的資料表會反映在限制中的任何差異，當您使用 Azure Resource Manager。 例如，沒有**訂用帳戶限制**資料表並**訂用帳戶限制-Azure Resource Manager**資料表。 時的限制會套用這兩個案例中，只會顯示第一個資料表中。 除非另有說明，限制在所有區域中全域適用。

> [!NOTE]
> 服務管理配額則是 Azure 資源群組中資源的配額是每個區域可以存取您的訂用帳戶中，而不每個訂用帳戶。 讓我們以 vCPU 配額為例。 若要要求增加配額以支援 Vcpu，您必須決定您想要在哪些區域中使用的多少 Vcpu。 然後，您要 Azure 資源群組 vCPU 配額的特定要求的數量和您想要的區域。 如果您需要使用 30 個 Vcpu 以在西歐該處執行您的應用程式，您特別要求 30 個 Vcpu 以在西歐區域。 僅西歐會有 30 個 vCPU 配額，不會在任何其他區域--增加 vCPU 配額。
> <!-- -->
> 如此一來，決定您的 Azure 資源群組配額必須是您在任何一個區域中的工作負載。 然後要求該數量，您要部署的每個區域中。 如需如何判斷您目前的配額，特定區域中的說明，請參閱[針對部署問題疑難排解](resource-manager-common-deployment-errors.md)。
>
>

## <a name="service-specific-limits"></a>特定服務的限制
* [Active Directory](#active-directory-limits)
* [API 管理](#api-management-limits)
* [App Service](#app-service-limits)
* [應用程式閘道](#application-gateway-limits)
* [Application Insights](#application-insights-limits)
* [自動化](#automation-limits)
* [Azure Cache for Redis](#azure-cache-for-redis-limits)
* [Azure 雲端服務](#azure-cloud-services-limits)
* [Azure Cosmos DB](#azure-cosmos-db-limits)
* [適用於 MySQL 的 Azure 資料庫](#azure-database-for-mysql)
* [適用於 PostgreSQL 的 Azure 資料庫](#azure-database-for-postgresql)
* [Azure DNS](#azure-dns-limits)
* [Azure 防火牆](#azure-firewall-limits)
* [Azure Kubernetes Service](#azure-kubernetes-service-limits)
* [Azure 地圖服務](#azure-maps-limits)
* [Azure 監視器](#monitor-limits)
* [Azure 原則](#azure-policy-limits)
* [Azure 搜尋服務](#azure-search-limits)
* [Azure SignalR 服務](#azure-signalr-service-limits)
* [備份](#backup-limits)
* [批次](#batch-limits)
* [BizTalk 服務](#biztalk-services-limits)
* [Container Instances](#container-instances-limits)
* [容器登錄](#container-registry-limits)
* [內容傳遞網路](#content-delivery-network-limits)
* [Data Factory](#data-factory-limits)
* [Data Lake Analytics](#data-lake-analytics-limits)
* [Data Lake Store](#data-lake-store-limits)
* [Database Migration Service](#database-migration-service-limits)
* [Event Grid](#event-grid-limits)
* [事件中樞](#event-hubs-limits)
* [前端服務](#azure-front-door-service-limits)
* [Identity Manager](#identity-manager-limits)
* [IoT 中心](#iot-hub-limits)
* [IoT 中樞裝置佈建服務](#iot-hub-device-provisioning-service-limits)
* [金鑰保存庫](#key-vault-limits)
* [Log Analytics](#log-analytics-limits)
* [媒體服務](#media-services-limits)
* [行動服務](#mobile-services-limits)
* [Multi-Factor Authentication](#multi-factor-authentication-limits)
* [網路功能](#networking-limits)
* [網路監看員](#network-watcher-limits)
* [通知中樞](#notification-hubs-limits)
* [資源群組](#resource-group-limits)
* [角色型存取控制](#role-based-access-control-limits)
* [排程器](#scheduler-limits)
* [服务总线](#service-bus-limits)
* [站台復原](#site-recovery-limits)
* [SQL Database](#sql-database-limits)
* [SQL 資料倉儲](#sql-data-warehouse-limits)
* [儲存體](#storage-limits)
* [StorSimple 系統](#storsimple-system-limits)
* [串流分析](#stream-analytics-limits)
* [訂用帳戶](#subscription-limits)
* [流量管理員](#traffic-manager-limits)
* [虛擬機器](#virtual-machines-limits)
* [虛擬機器擴展集](#virtual-machine-scale-sets-limits)

### <a name="subscription-limits"></a>訂用帳戶限制
#### <a name="subscription-limits---azure-service-management-classic-deployment-model"></a>訂用帳戶限制-Azure 服務管理 （傳統部署模型）
[!INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>訂用帳戶限制 - Azure Resource Manager
當您使用 Azure Resource Manager 和 Azure 資源群組時，就會適用下列限制。 未列尚未變更使用 Azure Resource Manager 的限制。 請參閱上的表，如需這些限制。

如需 Resource Manager API 讀取和寫入限制的相關資訊，請參閱[對 Resource Manager 要求進行節流](resource-manager-request-limits.md)。

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a>資源群組的限制
[!INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>虛擬機器限制
#### <a name="virtual-machines-limits"></a>虛擬機器限制
[!INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]

#### <a name="virtual-machines-limits---azure-resource-manager"></a>虛擬機器限制 - Azure 資源管理員
當您使用 Azure Resource Manager 和 Azure 資源群組時，就會適用下列限制。 未列尚未變更使用 Azure Resource Manager 的限制。 請參閱上的表，如需這些限制。

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>虛擬機器擴展集限制
[!INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="container-instances-limits"></a>Container Instances 限制
[!INCLUDE [container-instances-limits](../includes/container-instances-limits.md)]

### <a name="container-registry-limits"></a>Container Registry 登入
下表詳述「基本」、「標準」和「進階」[服務層](./container-registry/container-registry-skus.md)的功能和限制。

[!INCLUDE [container-registry-limits](../includes/container-registry-limits.md)]

### <a name="azure-kubernetes-service-limits"></a>Azure Kubernetes 服務限制
[!INCLUDE [container-service-limits](../includes/container-service-limits.md)]

### <a name="networking-limits"></a>網路限制
[!INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>网络限制
[!INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>應用程式閘道限制

下列表格適用於 v1、v2、「標準」及 WAF SKU (除非另外註明)。
[!INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="network-watcher-limits"></a>網路監看員限制
[!INCLUDE [network-watcher-limits](../includes/network-watcher-limits.md)]

#### <a name="traffic-manager-limits"></a>流量管理員限制
[!INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="azure-dns-limits"></a>Azure DNS 的限制
[!INCLUDE [dns-limits](../includes/dns-limits.md)]

#### <a name="azure-firewall-limits"></a>Azure 防火牆限制
[!INCLUDE [azure-firewall-limits](../includes/firewall-limits.md)]

#### <a name="azure-front-door-service-limits"></a>Azure Front Door Service 限制
[!INCLUDE [azure-front-door-service-limits](../includes/front-door-limits.md)]

### <a name="storage-limits"></a>儲存體限制
<!--like # storage accts -->
[!INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

如需有關儲存體帳戶限制的詳細資訊，請參閱 < [Azure 儲存體延展性和效能目標](storage/common/storage-scalability-targets.md)。

#### <a name="storage-resource-provider-limits"></a>儲存體資源提供者限制 

[!INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]

#### <a name="azure-blob-storage-limits"></a>Azure Blob 儲存體的限制
[!INCLUDE [storage-blob-scale-targets](../includes/storage-blob-scale-targets.md)]

#### <a name="azure-files-limits"></a>Azure 檔案的限制
如需有關 Azure 檔案限制的詳細資訊，請參閱 < [Azure 檔案延展性和效能目標](storage/files/storage-files-scale-targets.md)。

[!INCLUDE [storage-files-scale-targets](../includes/storage-files-scale-targets.md)]

#### <a name="azure-file-sync-limits"></a>Azure 檔案同步的限制
[!INCLUDE [storage-sync-files-scale-targets](../includes/storage-sync-files-scale-targets.md)]

#### <a name="azure-queue-storage-limits"></a>Azure 佇列儲存體的限制
[!INCLUDE [storage-queues-scale-targets](../includes/storage-queues-scale-targets.md)]

#### <a name="azure-table-storage-limits"></a>Azure 資料表儲存體的限制
[!INCLUDE [storage-tables-scale-targets](../includes/storage-tables-scale-targets.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
#### <a name="virtual-machine-disk-limits"></a>虛擬機器磁碟限制
[!INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

如需詳細資訊，請參閱 <<c0> [ 虛擬機器大小](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。

#### <a name="managed-virtual-machine-disks"></a>受控虛擬機器磁碟

[!INCLUDE [azure-storage-limits-vm-disks-managed](../includes/azure-storage-limits-vm-disks-managed.md)]

#### <a name="unmanaged-virtual-machine-disks"></a>非受控虛擬機器磁碟

[!INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

### <a name="azure-cloud-services-limits"></a>Azure 雲端服務限制
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]

### <a name="app-service-limits"></a>應用程式服務限制
下列 App Service 限制包含 Web 應用程式、行動應用程式和 API 應用程式的限制。

[!INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>排程器限制
[!INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Batch 限制
[!INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

### <a name="biztalk-services-limits"></a>BizTalk 服務限制
下表會顯示 Azure BizTalk 服務的限制。

[!INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]

### <a name="azure-cosmos-db-limits"></a>Azure Cosmos DB 限制
Azure Cosmos DB 是一個全域調整資料庫，可以調整輸送量和儲存體來因應您應用程式的需要。 如果您有關於 Azure Cosmos DB 調整的任何問題，請傳送電子郵件至askcosmosdb@microsoft.com。

### <a name="azure-database-for-mysql"></a>適用於 MySQL 的 Azure 資料庫
如需適用於 MySQL 的 Azure 資料庫限制詳細資訊，請參閱[適用於 MySQL 的 Azure 資料庫中的限制](mysql/concepts-limits.md)。

### <a name="azure-database-for-postgresql"></a>適用於 PostgreSQL 的 Azure 資料庫
如需適用於 PostgreSQL 的 Azure 資料庫限制詳細資訊，請參閱[適用於 PostgreSQL 的 Azure 資料庫中的限制](postgresql/concepts-limits.md)。

### <a name="azure-search-limits"></a>Azure 搜尋服務限制
定價層會決定搜尋服務的容量和限制。 層級包括：

* **免費**多租用戶的服務，與其他 Azure 訂用帳戶共用供評估與小型開發專案。
*  可針對規模較小的生產工作負載提供專用的計算資源，以及針對高可用性的查詢工作負載提供最多 3 個複本。
* **標準**、 包括 S1、 S2、 S3 及 S3 高密度是針對較大的生產工作負載。 標準層內的多個層級，以便您可以選擇最符合您的工作負載設定檔的資源組態。

**每一訂用帳戶的限制**

[!INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**每個搜尋服務的限制**

[!INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

若要深入了解更細微的限制，例如文件大小、每秒的查詢數、金鑰、要求和回應，請參閱 [Azure 搜尋服務的服務限制](search/search-limits-quotas-capacity.md)。

### <a name="media-services-limits"></a>媒體服務限制
[!INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="content-delivery-network-limits"></a>內容傳遞網路限制
[!INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>行動服務限制
[!INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitor-limits"></a>監視限制
[!INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hubs-limits"></a>通知中樞限制
[!INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>事件中樞限制
[!INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>服務匯流排限制
[!INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>IoT 中樞限制
[!INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="iot-hub-device-provisioning-service-limits"></a>IoT 中樞裝置佈建服務限制
[!INCLUDE [azure-iotdps-limits](../includes/iot-dps-limits.md)]

### <a name="data-factory-limits"></a>Data Factory 限制
[!INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Data Lake Analytics 限制
[!INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="data-lake-store-limits"></a>Data Lake Store 限制
[!INCLUDE [azure-data-lake-store-limits](../includes/azure-data-lake-store-limits.md)]

### <a name="database-migration-service-limits"></a>Database Migration Service 限制
[!INCLUDE [database-migration-service-limits](../includes/database-migration-service-limits.md)]

### <a name="stream-analytics-limits"></a>串流分析限制
[!INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Active Directory 限制
[!INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]

### <a name="event-grid-limits"></a>Event Grid 限制
[!INCLUDE [event-grid-limits](../includes/event-grid-limits.md)]

### <a name="azure-maps-limits"></a>Azure 地圖服務限制
[!INCLUDE [maps-limits](../includes/maps-limits.md)]

### <a name="azure-policy-limits"></a>Azure 原則限制
[!INCLUDE [policy-limits](../includes/azure-policy-limits.md)]

### <a name="storsimple-system-limits"></a>StorSimple 系統限制
[!INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]

### <a name="log-analytics-limits"></a>Log Analytics 限制
[!INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>備份限制
[!INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="azure-signalr-service-limits"></a>Azure SignalR 服務限制
[!INCLUDE [signalr-service-limits](../includes/signalr-service-limits.md)]

### <a name="site-recovery-limits"></a>Site Recovery 限制
[!INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Application Insights 限制
[!INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>API 管理限制
[!INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-cache-for-redis-limits"></a>Azure Cache for Redis 限制
[!INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>金鑰保存庫限制
[!INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication-limits"></a>多重要素驗證限制
[!INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>自動化限制
[!INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="identity-manager-limits"></a>Identity Manager 限制
[!INCLUDE [automation-limits](~/includes/managed-identity-limits.md)]

### <a name="role-based-access-control-limits"></a>角色型存取控制限制
[!INCLUDE [role-based-access-control-limits](../includes/role-based-access-control-limits.md)]

### <a name="sql-database-limits"></a>SQL Database 限制
如需 SQL Database 的限制，請參閱[單一資料庫的 SQL Database 資源限制](sql-database/sql-database-vcore-resource-limits-single-databases.md)，[彈性集區和集區的資料庫的 SQL Database 資源限制](sql-database/sql-database-vcore-resource-limits-elastic-pools.md)，和[SQL Database 資源限制受管理的執行個體](sql-database/sql-database-managed-instance-resource-limits.md)。

### <a name="sql-data-warehouse-limits"></a>SQL 資料倉儲限制
如需 SQL 資料倉儲限制，請參閱[SQL 資料倉儲資源限制](sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md)。

## <a name="see-also"></a>請參閱
- [了解 Azure 限制和增加](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
- [適用於 Azure 的虛擬機器和雲端服務大小](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Azure 雲端服務的大小](cloud-services/cloud-services-sizes-specs.md)
