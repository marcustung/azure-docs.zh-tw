---
title: 分析使用 Azure 資料的時間序列資料
description: 了解如何分析時間序列資料，在雲端中使用 Azure 資料總管。
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/07/2019
ms.openlocfilehash: 8492f736e64366802b3601f9b5fc8bd1d9b6ea79
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59273068"
---
# <a name="time-series-analysis-in-azure-data-explorer"></a>Azure 資料總管中的時間序列分析

Azure 資料總管 (ADX) 會執行從雲端服務或 IoT 裝置持續收集遙測資料的作業。 您可以針對各種見解分析此資料，例如，監視服務健康情況、實際的生產環境流程，以及使用方式趨勢。 您可以對所選計量的時間序列進行分析，以便在模式中找出相較於其一般基準模式的偏差。
ADX 包含對於建立、操作及分析多個時間序列的原生支援。 在本主題中，了解如何使用 ADX 來建立和分析**數千個時間序列 (以秒為單位)**，從而啟用近乎即時的監視解決方案和工作流程。

## <a name="time-series-creation"></a>建立時間序列

在本節中，我們將使用 `make-series` 運算子，以簡單且直覺的方式建立大量的標準時間序列，並視需要填入遺漏的值。
時間序列分析中的第一個步驟是分割原始遙測資料表並轉換為一組時間序列。 此資料表通常會包含一個時間戳記資料行、內容相關的維度，以及選擇性計量。 維度可用來分割資料。 目標是依規律的時間間隔，為每個分割區建立數千個時間序列。

輸入資料表 *demo_make_series1* 會包含 60 萬筆任意 Web 服務流量的記錄。 請使用下列命令來取樣 10 筆記錄：

```kusto
demo_make_series1 | take 10 
```

結果資料表會包含一個時間戳記資料行、三個內容相關的維度資料行，但沒有任何計量：

|   |   |   |   |   |
| --- | --- | --- | --- | --- |
|   | TimeStamp | BrowserVer | OsVer | 國家 (地區) |
|   | 2016-08-25 09:12:35.4020000 | Chrome 51.0 | Windows 7 | 英國 |
|   | 2016-08-25 09:12:41.1120000 | Chrome 52.0 | Windows 10 |   |
|   | 2016-08-25 09:12:46.2300000 | Chrome 52.0 | Windows 7 | 英國 |
|   | 2016-08-25 09:12:46.5100000 | Chrome 52.0 | Windows 10 | 英國 |
|   | 2016-08-25 09:12:46.5570000 | Chrome 52.0 | Windows 10 | 立陶宛共和國 |
|   | 2016-08-25 09:12:47.0470000 | Chrome 52.0 | Windows 8.1 | 印度 |
|   | 2016-08-25 09:12:51.3600000 | Chrome 52.0 | Windows 10 | 英國 |
|   | 2016-08-25 09:12:51.6930000 | Chrome 52.0 | Windows 7 | 荷蘭 |
|   | 2016-08-25 09:12:56.4240000 | Chrome 52.0 | Windows 10 | 英國 |
|   | 2016-08-25 09:13:08.7230000 | Chrome 52.0 | Windows 10 | 印度 |

由於沒有計量，因此，我們僅能建置一組代表流量計數本身的時間序列，其會使用下列查詢依 OS 進行分割：

```kusto
let min_t = toscalar(demo_make_series1 | summarize min(TimeStamp));
let max_t = toscalar(demo_make_series1 | summarize max(TimeStamp));
demo_make_series1
| make-series num=count() default=0 on TimeStamp in range(min_t, max_t, 1h) by OsVer
| render timechart 
```

- 使用 [`make-series`](/azure/kusto/query/make-seriesoperator) 運算子來建立含三個時間序列的集合，其中：
    - `num=count()`： 時間序列的流量
    - `range(min_t, max_t, 1h)`： 在 1 小時量化中的時間範圍 （最舊和最新時間戳記的資料表記錄） 中建立時間序列
    - `default=0`： 指定遺漏的分類收納組來建立規則的時間序列的填滿方法。 或者，使用 [`series_fill_const()`](/azure/kusto/query/series-fill-constfunction)、[`series_fill_forward()`](/azure/kusto/query/series-fill-forwardfunction)、[`series_fill_backward()`](/azure/kusto/query/series-fill-backwardfunction) 及 [`series_fill_linear()`](/azure/kusto/query/series-fill-linearfunction) 進行變更
    - `byOsVer`： 作業系統磁碟分割
- 實際的時間序列資料結構是每個時間間隔中彙總值的數值陣列。 我們使用 `render timechart` 來產生視覺效果。

在上述資料表中，我們有三個分割區。 我們可以針對每個 OS 版本建立個別的時間序列：Windows 10 (紅色)、7 (藍色) 和 8.1 (綠色)，如下圖所示：

![時間序列分割區](media/time-series-analysis/time-series-partition.png)

## <a name="time-series-analysis-functions"></a>時間序列分析函式

在本節中，我們將執行一般序列處理函式。
建立一組時間序列之後，ADX 就能支援持續增長的函式清單來處理和分析它們，您可以在[時間序列文件](/azure/kusto/query/machine-learning-and-tsa) \(英文\) 中找到這些函式。 我們將說明數個可用來處理和分析時間序列的代表性函式。

### <a name="filtering"></a>篩選

篩選是單一處理中的常見做法，非常適合用於時間序列處理工作 (例如，緩和吵雜的訊號、變更偵測)。
- 有兩個泛型篩選函式：
    - [`series_fir()`](/azure/kusto/query/series-firfunction)：套用 FIR 篩選。 用於移動時間序列平均和差異的簡單計算，以進行變更偵測。
    - [`series_iir()`](/azure/kusto/query/series-iirfunction)：套用 IIR 篩選。 用於指數平滑和累計總和。
- `Extend` 時間序列加入移動平均一系列新的設定調整大小的 5 個分類收納 (名為*ma_num*) 查詢：

```kusto
let min_t = toscalar(demo_make_series1 | summarize min(TimeStamp));
let max_t = toscalar(demo_make_series1 | summarize max(TimeStamp));
demo_make_series1
| make-series num=count() default=0 on TimeStamp in range(min_t, max_t, 1h) by OsVer
| extend ma_num=series_fir(num, repeat(1, 5), true, true)
| render timechart
```

![篩選時間序列](media/time-series-analysis/time-series-filtering.png)

### <a name="regression-analysis"></a>迴歸分析

ADX 支援使用分段線性迴歸分析來評估時間序列的趨勢。
- 使用 [series_fit_line()](/azure/kusto/query/series-fit-linefunction) \(英文\)，以使最佳線條擬合至某個時間序列以進行一般趨勢偵測。
- 使用 [series_fit_2lines](/azure/kusto/query/series-fit-2linesfunction) \(英文\) 來偵測趨勢變更 (相對於基準線)，這類變更對於監視案例非常實用。

時間序列查詢中的 `series_fit_line()` 和 `series_fit_2lines()` 函式範例：

```kusto
demo_series2
| extend series_fit_2lines(y), series_fit_line(y)
| render linechart with(xcolumn=x)
```

![時間序列迴歸](media/time-series-analysis/time-series-regression.png)

- 藍色：原始時間序列
- 綠色：擬合線
- 紅色：兩條擬合線

> [!NOTE]
> 此函式已準確地偵測到跳轉 (層級變更) 點。

### <a name="seasonality-detection"></a>季節性偵測

許多計量都會遵循季節性 (週期) 模式。 雲端服務的使用者流量通常會包含每日和每週模式，這類模式在工作日中間最高，在夜晚和週末則最低。 IoT 感應器會定期測量。 實際度量 (例如溫度、壓力或溼度) 可能也會顯示季節性行為。

下列範例會在 Web 服務的一個月流量 (以 2 小時為間隔) 上套用季節性偵測：

```kusto
demo_series3
| render timechart 
```

![季節性時間序列](media/time-series-analysis/time-series-seasonality.png)

- 使用 [series_periods_detect()](/azure/kusto/query/series-periods-detectfunction) \(英文\) 自動偵測時間序列中的期間。 
- 如果我們知道計量應具備特定的相異期間，而且想要確認它們存在，請使用 [series_periods_validate()](/azure/kusto/query/series-periods-validatefunction) \(英文\)。

> [!NOTE]
> 如果特定的相異期間不存在，則為異常狀況

```kusto
demo_series3
| project (periods, scores) = series_periods_detect(num, 0., 14d/2h, 2) //to detect the periods in the time series
| mv-expand periods, scores
| extend days=2h*todouble(periods)/1d
```

|   |   |   |   |
| --- | --- | --- | --- |
|   | 週期 | 分數 | days |
|   | 84 | 0.820622786055595 | 7 |
|   | 12 | 0.764601405803502 | 1 |

此函式會偵測每日和每週的季節性。 每日分數會低於每週分數，因為週末與工作日不同。

### <a name="element-wise-functions"></a>元素型函式

您可以針對時間序列進行算術和邏輯運算。 我們可以使用 [series_subtract()](/azure/kusto/query/series-subtractfunction) \(英文\) 來計算剩餘的時間序列，即原始未經處理的計量和經過平滑處理之時間序列間的差異，並尋找剩餘訊號中的異常狀況：

```kusto
let min_t = toscalar(demo_make_series1 | summarize min(TimeStamp));
let max_t = toscalar(demo_make_series1 | summarize max(TimeStamp));
demo_make_series1
| make-series num=count() default=0 on TimeStamp in range(min_t, max_t, 1h) by OsVer
| extend ma_num=series_fir(num, repeat(1, 5), true, true)
| extend residual_num=series_subtract(num, ma_num) //to calculate residual time series
| where OsVer == "Windows 10"   // filter on Win 10 to visualize a cleaner chart 
| render timechart
```

![時間序列作業](media/time-series-analysis/time-series-operations.png)

- 藍色：原始時間序列
- 紅色：平滑時間序列
- 綠色：剩餘時間序列

## <a name="time-series-workflow-at-scale"></a>大規模的時間序列工作流程

下列範例示範這些函式如何在數千個時間序列上大規模執行 (以秒為單位) 以進行異常偵測。 若要查看四天中 DB 服務讀取計數計量的一些範例遙測記錄，請執行下列查詢：

```kusto
demo_many_series1
| take 4 
```

|   |   |   |   |   |   |
| --- | --- | --- | --- | --- | --- |
|   | 時間戳記 | Loc | anonOp | DB | DataRead |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | 5117853934049630089 | 262 | 0 |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | 5117853934049630089 | 241 | 0 |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | -865998331941149874 | 262 | 279862 |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | 371921734563783410 | 255 | 0 |

以及簡單的統計資料：

```kusto
demo_many_series1
| summarize num=count(), min_t=min(TIMESTAMP), max_t=max(TIMESTAMP) 
```

|   |   |   |   |
| --- | --- | --- | --- |
|   | num | min\_t | max\_t |
|   | 2177472 | 2016-09-08 00:00:00.0000000 | 2016-09-11 23:00:00.0000000 |

以 1 小時的讀取計量間隔來建置時間序列 (總共四天 * 24 小時 = 96 點)，會產生標準模式波動：

```kusto
let min_t = toscalar(demo_many_series1 | summarize min(TIMESTAMP));  
let max_t = toscalar(demo_many_series1 | summarize max(TIMESTAMP));  
demo_many_series1
| make-series reads=avg(DataRead) on TIMESTAMP in range(min_t, max_t, 1h)
| render timechart with(ymin=0) 
```

![大規模時間序列](media/time-series-analysis/time-series-at-scale.png)

上述行為會讓人產生誤解，因為單一的一般時間序列會彙總自數千個可能具有異常模式的不同執行個體。 因此，我們會針對每個執行個體建立一個時間序列。 執行個體是由 Loc (位置)、anonOp (作業) 及 DB (特定機器) 定義的。

我們可以建立多少個時間序列？

```kusto
demo_many_series1
| summarize by Loc, Op, DB
| count
```

|   |   |
| --- | --- |
|   | 計數 |
|   | 18339 |

現在，我們將建立一組 18339 個讀取計數計量的時間序列。 我們會將 `by` 子句新增至 make-series 陳述式、套用線性迴歸，然後選取前兩個趨勢下降最明顯的時間序列：

```kusto
let min_t = toscalar(demo_many_series1 | summarize min(TIMESTAMP));  
let max_t = toscalar(demo_many_series1 | summarize max(TIMESTAMP));  
demo_many_series1
| make-series reads=avg(DataRead) on TIMESTAMP in range(min_t, max_t, 1h) by Loc, Op, DB
| extend (rsquare, slope) = series_fit_line(reads)
| top 2 by slope asc 
| render timechart with(title='Service Traffic Outage for 2 instances (out of 18339)')
```

![前兩個時間序列](media/time-series-analysis/time-series-top-2.png)

顯示執行個體：

```kusto
let min_t = toscalar(demo_many_series1 | summarize min(TIMESTAMP));  
let max_t = toscalar(demo_many_series1 | summarize max(TIMESTAMP));  
demo_many_series1
| make-series reads=avg(DataRead) on TIMESTAMP in range(min_t, max_t, 1h) by Loc, Op, DB
| extend (rsquare, slope) = series_fit_line(reads)
| top 2 by slope asc
| project Loc, Op, DB, slope 
```

|   |   |   |   |   |
| --- | --- | --- | --- | --- |
|   | Loc | Op | DB | slope |
|   | Loc 15 | 37 | 1151 | -102743.910227889 |
|   | Loc 13 | 37 | 1249 | -86303.2334644601 |

ADX 在兩分鐘內分析了接近 20,000 個時間序列，並偵測到兩個異常的時間序列，其中的讀取計數突然下降。

這些與 ADX 快速效能相結合的進階功能會提供唯一且功能強大的解決方案來進行時間序列分析。
