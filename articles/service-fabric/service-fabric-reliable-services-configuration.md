---
title: 設定 Azure Service Fabric Reliable Services | Microsoft Docs
description: 深入了解在 Azure Service Fabric 中設定具狀態的 Reliable Services。
services: Service-Fabric
documentationcenter: .net
author: sumukhs
manager: chackdan
editor: vturecek
ms.assetid: 9f72373d-31dd-41e3-8504-6e0320a11f0e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/02/2017
ms.author: sumukhs
ms.openlocfilehash: 97cf4fafb53156eec654bcc67cd8dccf3d973c32
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2019
ms.locfileid: "58660692"
---
# <a name="configure-stateful-reliable-services"></a>設定具狀態可靠服務
有兩組組態設定可供 Reliable Services 使用。 一組是適用於叢集中的所有 Reliable Services，而另一組專屬於特定的 Reliable Services。

## <a name="global-configuration"></a>全域組態
在 KtlLogger 區段下，叢集的叢集資訊清單中所指定的全域 Reliable Service 組態。 它可配置共享日志位置和大小，以及记录器所使用的全局内存限制。 叢集資訊清單是單一 XML 檔案，可保留套用至叢集中所有節點和服務態的設定與組態。 此檔案通常稱為 ClusterManifest.xml。 您可以查看叢集的叢集資訊清單使用 Get-ServiceFabricClusterManifest powershell 命令。

### <a name="configuration-names"></a>組態名稱
| 名稱 | 單位 | 預設值 | 備註 |
| --- | --- | --- | --- |
| WriteBufferMemoryPoolMinimumInKB |KB |8388608 |以核心模式配置給記錄器寫入緩衝區記憶體集區的最小 KB 數。 此記憶體集區用於在寫入至磁碟之前快取狀態資訊。 |
| WriteBufferMemoryPoolMaximumInKB |KB |沒有限制 |記錄器寫入緩衝區記憶體集區可以成長的的大小上限。 |
| SharedLogId |GUID |"" |指定用來識別預設共用記錄檔的唯一 GUID，用於叢集中所有節點上的所有 Reliable Services (不會在其服務特定組態中指定 SharedLogId)。 如果有指定 SharedLogId，則也必須指定 SharedLogPath。 |
| SharedLogPath |完整路徑名稱 |"" |指定完整路徑，其中共用記錄檔用於叢集中所有節點上的所有 Reliable Services (不會在其服務特定組態中指定 SharedLogPath)。 不過，如果有指定 SharedLogPath，則也必須指定 SharedLogId。 |
| SharedLogSizeInMB |MB |8192 |指定以靜態方式配置給共用記錄檔的磁碟空間 MB 數。 此值必須是 2048 或更大。 |

在 Azure ARM 或本地 JSON 模板中，以下示例说明如何更改为支持有状态服务的任何可靠集合而创建的共享事务日志。

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

### <a name="sample-local-developer-cluster-manifest-section"></a>範例本機開發人員叢集資訊清單區段
如果您想要在本機開發環境上變更此區段，您必須編輯本機 clustermanifest.xml 檔案。

```xml
   <Section Name="KtlLogger">
     <Parameter Name="SharedLogSizeInMB" Value="4096"/>
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
   </Section>
```

### <a name="remarks"></a>備註
記錄器有配置自未分頁核心記憶體的記憶體全域集區，可供節點上的所有 Reliable Services 使用，在寫入至與 Reliable Service 複本相關聯的專用記錄檔之前快取狀態資料。 集區大小由 WriteBufferMemoryPoolMinimumInKB 和 WriteBufferMemoryPoolMaximumInKB 設定控制。 WriteBufferMemoryPoolMinimumInKB 指定此記憶體集區的初始大小，以及記憶體集區可能會縮小的大小下限。 WriteBufferMemoryPoolMaximumInKB 是記憶體集區可能會成長的大小上限。 每個開啟的 Reliable Service 複本都可能會增加記憶體集區的大小，增加幅度從系統決定的數量到 WriteBufferMemoryPoolMaximumInKB。 如果記憶體集區的記憶體需求大於可用的記憶體，會延遲記憶體要求，直到記憶體可供使用。 因此，如果寫入緩衝區記憶體集區對特定組態而言太小，則效能可能會受到影響。

SharedLogId 和 SharedLogPath 設定永遠會一起使用，以便為叢集中的所有節點定義 GUID 和預設共用記錄檔的位置。 預設共用記錄檔可用於不在特定服務 settings.xml 中指定設定的所有 Reliable Services。 為了達到最佳效能，共用記錄檔應該放在共用記錄檔專用的磁碟上，以減少爭用情形。

SharedLogSizeInMB 會指定要預先配置給所有節點上之預設共用記錄檔的磁碟空間數量。  若要指定 SharedLogSizeInMB，不需要指定 SharedLogId 和 SharedLogPath。

## <a name="service-specific-configuration"></a>服務特定組態
您可以使用組態封裝 (組態)，或服務實作 (程式碼) 修改具狀態的 Reliable Services 的預設組態。

* **組態** - 您可以藉由變更在 Microsoft Visual Studio 封裝根的 Config 資料夾底下，為應用程式中每個服務產生的 Settings.xml 檔案，來透過組態封裝完成組態。
* **程式碼** - 您可以使用 ReliableStateManagerConfiguration 物件搭配適當的選項設定來建立 ReliableStateManager，透過程式碼完成設定。

Azure Service Fabric 執行階段預設會在建立基礎執行階段元件時，在 Settings.xml 檔案中尋找預先定義的區段名稱，並使用組態值。

> [!NOTE]
> 除非您打算透過程式碼設定服務，否則請 **不要** 刪除在 Visual Studio 方案中產生之 Settings.xml 檔案中的下列組態區段名稱。
> 設定 ReliableStateManager 時，重新命名組態封裝或區段名稱將需要變更程式碼。
> 
> 

### <a name="replicator-security-configuration"></a>复制器安全配置
复制器安全配置用于保护在复制过程中使用的通信通道的安全。 這表示服務將無法看到彼此的複寫流量，並且也會確保高度可用資料的安全。 依預設，空白的安全性組態區段會妨礙複寫安全性。

> [!IMPORTANT]
> 在 Linux 節點上，憑證必須是 PEM 格式。 若要深入了解如何尋找和設定適用於 Linux 的憑證，請參閱[在 Linux 上設定憑證](./service-fabric-configure-certificates-linux.md)。 
> 
> 

### <a name="default-section-name"></a>預設區段名稱
ReplicatorSecurityConfig

> [!NOTE]
> 若要變更此區段名稱，請在建立此服務的 ReliableStateManager 時，將 replicatorSecuritySectionName 參數覆寫至 ReliableStateManagerConfiguration 建構函式。
> 
> 

### <a name="replicator-configuration"></a>复制器配置
複寫器組態用來設定負責將狀態複寫和保存至本機，讓具狀態的 Reliable Services 狀態變得高度可靠的複寫器。
默认配置由 Visual Studio 模板生成，并应已足够。 本節說明可用於微調複寫器的其他組態。

### <a name="default-section-name"></a>默认节名称
ReplicatorConfig

> [!NOTE]
> 若要變更此區段名稱，請在建立此服務的 ReliableStateManager 時，將 replicatorSettingsSectionName 參數覆寫至 ReliableStateManagerConfiguration 建構函式。
> 
> 

### <a name="configuration-names"></a>組態名稱
| 名稱 | 單位 | 預設值 | 備註 |
| --- | --- | --- | --- |
| BatchAcknowledgementInterval |秒 |0.015 |次要複寫器收到作業後，將通知傳回給主要複寫器前所等待的時間間隔。 任何要在此間隔內傳送給作業處理的其他通知，會集中以一個回應傳送。 |
| ReplicatorEndpoint |N/A |無預設值--必要的參數 |主要/次要複寫器將用於與複本集中其他複寫器通訊的 IP 位址與連接埠。 這應該參考服務資訊清單中的 TCP 資源端點。 請參閱 [服務資訊清單資源](service-fabric-service-manifest-resources.md) ，深入了解如何在服務資訊清單中定義端點資源。 |
| MaxPrimaryReplicationQueueSize |操作的数量 |8192 |主要佇列中作業數目上限。 主要複寫器收到所有次要複寫器的通知後，系統便會釋放作業。 此值必須大於 64 且為 2 的乘冪。 |
| MaxSecondaryReplicationQueueSize |作業數目 |16384 |次要佇列中作業數目上限。 透過持續性讓狀態成為高可用性後，系統便會釋放作業。 此值必須大於 64 且為 2 的乘冪。 |
| CheckpointThresholdInMB |MB |50 |狀態完成檢查點作業後的記錄檔空間量。 |
| MaxRecordSizeInKB |KB |1024 |複寫器可以寫入記錄檔中的最大記錄大小。 此值必須是 4 的倍數且大於 16。 |
| MinLogSizeInMB |MB |0 (系統判定) |交易記錄檔大小下限。 系統將不會允許把記錄檔截斷為低於此設定的大小。 0 表示複寫器將會判斷記錄檔大小下限。 提高這個值會提高執行部分複本和增量備份的可能性，因為這會降低將相關記錄檔記錄截斷的可能性。 |
| TruncationThresholdFactor |因子 |2 |判斷將會觸發截斷的記錄檔大小。 截斷臨界值是以 MinLogSizeInMB 乘以 TruncationThresholdFactor 來決定。 TruncationThresholdFactor 必須大於 1。 MinLogSizeInMB 乘以 TruncationThresholdFactor 必須小於 MaxStreamSizeInMB。 |
| ThrottlingThresholdFactor |因素 |4 |判斷複本將會開始節流的記錄檔大小。 節流閾值 (MB) 是以 Max((MinLogSizeInMB * ThrottlingThresholdFactor),(CheckpointThresholdInMB * ThrottlingThresholdFactor)) 來決定。 限制阈值（以 MB 为单位）必须大于截断阈值（以 MB 为单位）。 截斷臨界值 (MB) 必須小於 MaxStreamSizeInMB。 |
| MaxAccumulatedBackupLogSizeInMB |MB |800 |给定备份日志链中备份日志的最大累积大小（以 MB 为单位）。 如果增量備份會產生導致累積備份記錄的備份記錄，增量備份要求將會失敗，因為相關完整備份會大於此大小。 在这种情况下，用户需要执行完整备份。 |
| SharedLogId |GUID |"" |指定用於識別此複本共用記錄檔的唯一 GUID。 通常情况下，服务不应使用此设置。 不過，如果有指定 SharedLogId，則也必須指定 SharedLogPath。 |
| SharedLogPath |完整路徑名稱 |"" |指定建立此複本共用記錄檔的完整路徑。 服務通常不應使用此設定。 不過，如果有指定 SharedLogPath，則也必須指定 SharedLogId。 |
| SlowApiMonitoringDuration |秒 |300 |設定受控 API 呼叫的監視間隔。 範例︰使用者提供的備份回呼函式。 經過這段間隔後，警告健全狀況報告會傳送到健全狀況管理員。 |
| LogTruncationIntervalSeconds |秒 |0 |會在每個複本中起始記錄截斷的可設定間隔。 它可用來確保記錄也會根據時間而不只是記錄大小進行截斷。 這項設定也會強制在可靠的字典中清除已刪除的項目。 因此，它可用來確保適時將已刪除的項目清除。 |

### <a name="sample-configuration-via-code"></a>透過程式碼的範例組態
```csharp
class Program
{
    /// <summary>
    /// This is the entry point of the service host process.
    /// </summary>
    static void Main()
    {
        ServiceRuntime.RegisterServiceAsync("HelloWorldStatefulType",
            context => new HelloWorldStateful(context, 
                new ReliableStateManager(context, 
        new ReliableStateManagerConfiguration(
                        new ReliableStateManagerReplicatorSettings()
            {
                RetryInterval = TimeSpan.FromSeconds(3)
                        }
            )))).GetAwaiter().GetResult();
    }
}    
```
```csharp
class MyStatefulService : StatefulService
{
    public MyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica stateManager)
        : base(context, stateManager)
    { }
    ...
}
```


### <a name="sample-configuration-file"></a>範例組態檔
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```


### <a name="remarks"></a>備註
BatchAcknowledgementInterval 會控制複寫延遲性。 值為 '0' 時延遲可能性最低，但代價是降低輸送量 (隨著必須傳送與處理的通知訊息增加，每個訊息包含的通知會變少)。
BatchAcknowledgementInterval 的值越大，整體複寫輸送量越高，代價是作業延遲變高。 这直接转换为事务提交的延迟。

CheckpointThresholdInMB 的值控制複寫器可用來將狀態資訊儲存在複本專用記錄檔中的磁碟空間量。 若此值大於預設值，可能會在將複本新增至集合時，加速重新設定的時間。 這是因為記錄中具有更多可用的歷程記錄作業而造成部分狀態傳送的緣故。 在崩溃后，这可能会延长副本恢复时间。

MaxRecordSizeInKB 設定會定義複寫器可以寫入記錄檔的記錄大小上限。 在大部分情況下，預設的 1024 KB 記錄大小是最佳作法。 不過，如果服務造成更大的資料項目成為狀態資訊的一部分，則可能需要增加此值。 讓 MaxRecordSizeInKB 小於 1024 的好處不大，因為較小的記錄只會使用較小記錄所需的空間。 預期此值只會在極少數的情況下需要變更。

SharedLogId 和 SharedLogPath 設定永遠會一起使用，以便讓服務使用與節點的預設共用記錄檔不同的共用記錄檔。 为获得最佳效率，应让尽可能多的服务指定相同共享日志。 共用記錄檔應該放在共用記錄檔專用的磁碟上，以減少磁頭移動爭用情形。 我们预期此值只在极少数情况下需要更改。

## <a name="next-steps"></a>後續步驟
* [在 Visual Studio 中偵錯 Service Fabric 應用程式](service-fabric-debugging-your-application.md)
* [可靠的服務的開發人員參考資料](https://msdn.microsoft.com/library/azure/dn706529.aspx)

