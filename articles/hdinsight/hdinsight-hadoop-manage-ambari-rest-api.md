---
title: 使用 Ambari REST API 監視和管理 Hadoop - Azure HDInsight
description: 了解如何使用 Ambari 來監視和管理 Azure HDInsight 中的 Hadoop 叢集。 在本文件中，您將學習如何使用 HDInsight 叢集隨附的 Ambari REST API。
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/07/2019
ms.author: hrasheed
ms.openlocfilehash: 37d4c4dbbe623d68edf1de5454d7752a81fc2bd2
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/01/2019
ms.locfileid: "58801873"
---
# <a name="manage-hdinsight-clusters-by-using-the-apache-ambari-rest-api"></a>使用 Apache Ambari REST API 來管理 HDInsight 叢集

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

了解如何使用 Apache Ambari REST API 來管理和監視 Azure HDInsight 中的 Apache Hadoop 叢集。

## <a id="whatis"></a>什麼是 Apache Ambari
[Apache Ambari](https://ambari.apache.org)簡化的管理和監視 Hadoop 叢集，藉由提供簡單易用的 web 所支援的 UI 其[REST Api](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)。  以 Linux 為基礎的 HDInsight 叢集預設會提供 Ambari。

## <a name="prerequisites"></a>必要條件
* **HDInsight 上的 Hadoop 群集**。 请参阅 [Linux 上的 HDInsight 入门](hadoop/apache-hadoop-linux-tutorial-get-started.md)。

* **Bash on Ubuntu on Windows 10**。  這篇文章中的範例會使用 Windows 10 上的 Bash 殼層。 請參閱[Linux 安裝指南適用於 Windows 10 的 Windows 子系統](https://docs.microsoft.com/windows/wsl/install-win10)如需安裝步驟。  其他[Unix shell](https://www.gnu.org/software/bash/)正常運作。  此範例中，但有一些些微的修改，可以使用 Windows 命令提示字元。  或者，您可以使用 Windows PowerShell。

* **jq**，命令列的 JSON 處理器。  請參閱 [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/)。

* **Windows PowerShell**。  或者，您可以使用[Bash](https://www.gnu.org/software/bash/)。

## <a name="base-uri-for-ambari-rest-api"></a>Ambari REST API 的基底 URI

 在 HDInsight 上 Ambari REST API 的基底 URI 是`https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME`，其中`CLUSTERNAME`是叢集的名稱。  在 Uri 中的叢集名稱**區分大小寫**。  URI (CLUSTERNAME.azurehdinsight.net) 完整網域名稱 (FQDN) 部分中的叢集名稱區分大小寫，URI 中的其他部分也區分大小寫。

## <a name="authentication"></a>Authentication

連線到 HDInsight 上的 Ambari 需要 HTTPS。 請使用您在叢集建立期間所提供的管理帳戶名稱 (預設值是 **admin**) 和密碼。

## <a name="examples"></a>範例

### <a name="setup-preserve-credentials"></a>安裝程式 （保留的認證）
會保留您的認證，以避免重新進入其每個範例。  在個別步驟中，將會保留叢集名稱。

**A.Bash**  
編輯下列指令碼來取代`PASSWORD`以您實際的密碼。  然後輸入命令。

```bash
export password='PASSWORD'
```  

**B.PowerShell**  

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
```

### <a name="identify-correctly-cased-cluster-name"></a>識別正確的大小寫的叢集名稱
實際大小寫的叢集名稱可能比您預期根據叢集的建立方式不同。  這裡的步驟會顯示實際的大小寫，並再將它儲存在變數中供後續所有範例。

編輯指令碼來取代`CLUSTERNAME`與您的叢集名稱。 然後輸入命令。 （叢集名稱，fqdn 是不區分大小寫）。

**A.Bash**  

```bash
export clusterName=$(curl -u admin:$password -sS -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')
echo $clusterName
```  

```powershell
# Identify properly cased cluster name
$resp = Invoke-WebRequest -Uri "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters" `
    -Credential $creds -UseBasicParsing
$clusterName = (ConvertFrom-Json $resp.Content).items.Clusters.cluster_name;

# Show cluster name
$clusterName
```

### <a name="parsing-json-data"></a>剖析 JSON 資料

下列範例會使用[jq](https://stedolan.github.io/jq/)或是[Convertfrom-json](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/convertfrom-json)來剖析 JSON 回應文件，並只顯示`health_report`從結果中的資訊。

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" \
| jq '.Clusters.health_report'
```  

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```   

### <a name="example-get-the-fqdn-of-cluster-nodes"></a> 取得叢集節點的 FQDN

使用 HDInsight 時，您可能需要知道叢集節點的完整網域名稱 (FQDN)。 您可以使用下列範例，輕鬆地擷取叢集中不同節點的 FQDN：

**所有節點**  

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" \
| jq '.items[].Hosts.host_name'
```  

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.Hosts.host_name
```

**前端節點**：  

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/NAMENODE" \
| jq '.host_components[].HostRoles.host_name'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/NAMENODE" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.host_components.HostRoles.host_name
```

**背景工作節點**  

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/DATANODE" \
| jq '.host_components[].HostRoles.host_name'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/DATANODE" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.host_components.HostRoles.host_name
```

**Zookeeper 節點**

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" \
| jq ".host_components[].HostRoles.host_name"
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.host_components.HostRoles.host_name
```

### <a name="example-get-the-internal-ip-address-of-cluster-nodes"></a> 取得叢集節點的內部 IP 位址

本章節中的範例所傳回的 IP 位址無法直接透過網際網路存取。 它們只能在包含 HDInsight 叢集的 Azure 虛擬網路內存取。

如需使用 HDInsight 和虛擬網路的詳細資訊，請參閱[使用自訂 Azure 虛擬網路擴充 HDInsight 功能](hdinsight-extend-hadoop-virtual-network.md)。

若要尋找 IP 位址，您必須知道叢集節點的內部完整網域名稱 (FQDN)。 一旦您擁有 FQDN，就可以取得主機的 IP 位址。 下列範例會先查詢所有主機節點之 FQDN 的 Ambari，然後查詢每部主機之 IP 位址的 Ambari。

```bash
for HOSTNAME in $(curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" | jq -r '.items[].Hosts.host_name')
do
    IP=$(curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts/$HOSTNAME" | jq -r '.Hosts.ip')
  echo "$HOSTNAME <--> $IP"
done
```  

```powershell
$uri = "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" 
$resp = Invoke-WebRequest -Uri $uri -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
foreach($item in $respObj.items) {
    $hostName = [string]$item.Hosts.host_name
    $hostInfoResp = Invoke-WebRequest -Uri "$uri/$hostName" `
        -Credential $creds -UseBasicParsing
    $hostInfoObj = ConvertFrom-Json $hostInfoResp 
    $hostIp = $hostInfoObj.Hosts.ip
    "$hostName <--> $hostIp"
}
```

### <a name="get-the-default-storage"></a>取得預設儲存體

建立 HDInsight 叢集時，您必須使用 Azure 儲存體帳戶或 Data Lake Storage 作為叢集的預設儲存體。 在建立叢集之後，您可以使用 Ambari 來擷取這項資訊。 例如，如果您想要讀取/寫入資料至 HDInsight 以外的容器。

下列範例會從叢集擷取預設儲存體組態：

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
| jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties.'fs.defaultFS'
```

> [!IMPORTANT]  
> 這些範例會傳回套用至伺服器的第一個組態 (`service_config_version=1`)，其包含這項資訊。 如果您擷取在叢集建立後已修改過的值，您可能需要列出組態版本並擷取最新的版本。

傳回值會類似下列其中一個範例︰

* `wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net` - 這個值表示叢集是使用 Azure 儲存體帳戶做為預設儲存體。 `ACCOUNTNAME` 值是儲存體帳戶的名稱。 `CONTAINER` 部分是儲存體帳戶中 blob 容器的名稱。 容器是叢集的 HDFS 相容儲存體的根目錄。

* `abfs://CONTAINER@ACCOUNTNAME.dfs.core.windows.net` - 這個值表示叢集會使用 Azure Data Lake Storage Gen2 作為預設儲存體。 `ACCOUNTNAME` 和 `CONTAINER` 值與先前所述的 Azure 儲存體具有相同的意義。

* `adl://home` - 這個值表示叢集會使用 Azure Data Lake Storage Gen1 作為預設儲存體。

    若要尋找 Data Lake Storage 帳戶名稱，請使用下列範例：

    ```bash
    curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.hostname'
    ```

    傳回值類似 `ACCOUNTNAME.azuredatalakestore.net`，其中 `ACCOUNTNAME` 是 Data Lake Storage 帳戶的名稱。

    若要在包含叢集儲存體的 Data Lake Storage 內尋找此目錄，請使用下列範例：

    ```bash
    curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'
    ```  

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.mountpoint'
    ```

    傳回值類似 `/clusters/CLUSTERNAME/`。 這個值是 Data Lake Storage 帳戶內的路徑。 這個路徑是叢集的 HDFS 相容檔案系統的根目錄。  

> [!NOTE]  
> [Get AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightcluster)所提供的 cmdlet [Azure PowerShell](/powershell/azure/overview)也會傳回叢集的儲存體資訊。


### <a name="get-all-configurations"></a> 取得所有設定

取得可供您的叢集使用的組態。

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName?fields=Clusters/desired_configs"
```

```powershell
$respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
    -Credential $creds -UseBasicParsing
$respObj.Content
```

此範例會傳回 JSON 文件，其中包含叢集上安裝之元件的目前組態 (由「tag」值識別)。 下列範例是從 Spark 叢集類型傳回之資料的摘要。
   
```json
"jupyter-site" : {
  "tag" : "INITIAL",
  "version" : 1
},
"livy2-client-conf" : {
  "tag" : "INITIAL",
  "version" : 1
},
"livy2-conf" : {
  "tag" : "INITIAL",
  "version" : 1
},
```

### <a name="get-configuration-for-specific-component"></a>取得特定元件的組態設定

取得您感興趣之元件的組態。 在下列範例中，以前一個要求傳回的標記值取代 `INITIAL`。

```bash
curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=livy2-conf&tag=INITIAL"
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=livy2-conf&tag=INITIAL" `
    -Credential $creds -UseBasicParsing
$resp.Content
```

此範例會傳回 JSON 文件，其中包含 `livy2-conf` 元件的目前組態。

### <a name="update-configuration"></a>更新設定

1. 建立 `newconfig.json`。  
   修改，然後輸入下列命令：

   * 取代`livy2-conf`與所需的元件。
   * 取代`INITIAL`取代為擷取實際值`tag`從[取得所有設定](#get-all-configurations)。

     **A.Bash**  
     ```bash
     curl -u admin:$password -sS -G "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=livy2-conf&tag=INITIAL" \
     | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json

     ```

     **B. powershell**  
     PowerShell 指令碼會使用[jq](https://stedolan.github.io/jq/)。  編輯`C:\HD\jq\jq-win64`以反映您的實際路徑和版本下面的[jq](https://stedolan.github.io/jq/)。

     ```powershell
     $epoch = Get-Date -Year 1970 -Month 1 -Day 1 -Hour 0 -Minute 0 -Second 0
     $now = Get-Date
     $unixTimeStamp = [math]::truncate($now.ToUniversalTime().Subtract($epoch).TotalMilliSeconds)
     $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=livy2-conf&tag=INITIAL" `
       -Credential $creds -UseBasicParsing
     $resp.Content | C:\HD\jq\jq-win64 --arg newtag "version$unixTimeStamp" '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
     ```

     Jq 是用來將從 HDInsight 擷取到的資料轉換至新的組態範本。 具體來說，這些範例會執行下列動作︰

   * 建立唯一的值，其中包含字串 "version" 和日期，會儲存在 `newtag`。

   * 为新的所需配置创建根文档。

   * 获取 `.items[]` 数组的内容，并将其添加在 **desired_config** 元素下。

   * 刪除 `href`、`version` 和 `Config` 元素，因為提交新組態時不需要這些元素。

   * 使用 `version#################` 的值新增 `tag` 元素。 数字部分基于当前日期。 每個組態都必須有唯一的標記。

     最後，將資料儲存至 `newconfig.json` 文件。 此文件結構應會顯示為類似下列範例：

     ```json
     {
       "Clusters": {
         "desired_config": {
           "tag": "version1552064778014",
           "type": "livy2-conf",
           "properties": {
             "livy.environment": "production",
             "livy.impersonation.enabled": "true",
             "livy.repl.enableHiveContext": "true",
             "livy.server.csrf_protection.enabled": "true",
               ....
           },
         },
       }
     }
     ```

2. 編輯`newconfig.json`。  
   開啟 `newconfig.json` 文件，並且在 `properties` 物件中修改/新增值。 下列範例會將 `"livy.server.csrf_protection.enabled"`的值從 `"true"`變更為 `"false"`。

        "livy.server.csrf_protection.enabled": "false",

    完成修改之後，請儲存檔案。

3. 提交`newconfig.json`。  
   使用以下命令將更新的組態提交至 Ambari。

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" -X PUT -d @newconfig.json "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName"
    ```

    ```powershell
    $newConfig = Get-Content .\newconfig.json
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
        -Credential $creds -UseBasicParsing `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body $newConfig
    $resp.Content
    ```  

    這些命令會將 **newconfig.json** 檔案的內容提交至叢集，做為新的所需組態。 要求會傳回 JSON 文件。 這份文件中的 **versionTag** 元素應符合您所提交的版本，**configs** 物件將會包含您所要求的組態變更。

### <a name="restart-a-service-component"></a>重新啟動服務元件

此時，如果您看一下 Ambari Web UI，Spark 服務就會指出它需要重新啟動，新組態才會生效。 使用下列步驟重新啟動服務。

1. 您可以使用下列命令啟用 Spark2 服務的維護模式：

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning on maintenance mode for SPARK2"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds -UseBasicParsing `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning on maintenance mode for SPARK2"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}'
    $resp.Content
    ```

2. 確認維護模式  

    這些命令會將 JSON 文件傳送至伺服器，該伺服器以維護模式開啟。 可以使用以下请求来验证服务当前是否处于维护模式：

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" \
    | jq .ServiceInfo.maintenance_state
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.ServiceInfo.maintenance_state
    ```

    傳回值是 `ON`。

3. 若要關閉 Spark2 服務，接下來，使用下列：

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' \
    "https://girouxSpark.azurehdinsight.net/api/v1/clusters/girouxspark/services/SPARK2"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}'
    $resp.Content
    ```

    回應如下列範例所示：

    ```json
    {
        "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
        "Requests" : {
            "id" : 29,
            "status" : "Accepted"
        }
    }
    ```

    > [!IMPORTANT]  
    > 值 `href` 值正在使用群集节点的内部 IP 地址。 若要從叢集之外使用它，請將 '10.0.0.18:8080' 部分取代為叢集的 FQDN。  

4. 請確認要求。  
    編輯下列命令，來取代`29`使用的實際值`id`傳回從先前的步驟。  下列命令會擷取要求的狀態：

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" \
    | jq .Requests.request_status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" `
        -Credential $creds -UseBasicParsing
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.Requests.request_status
    ```

    `COMPLETED` 的回應表示要求已完成。

5. 前一個要求完成後，使用下列啟動 Spark2 服務。
   
    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}' \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds -UseBasicParsing `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}'
    ```

    服務現在使用新的組態。

6. 最後，使用以下命令來關閉維護模式。

    ```bash
    curl -u admin:$password -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning off maintenance mode for SPARK2"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' \
    "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds -UseBasicParsing `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning off maintenance mode for SPARK2"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}'

    ```

## <a name="next-steps"></a>後續步驟

如需 REST API 的完整參考，請參閱 [Apache Ambari API 參考 V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md) \(英文\)。

