# Chapter10.仮想マシンを監視してみよう

## 参考サイト

- [Azure Monitor の概要](https://docs.microsoft.com/ja-jp/azure/azure-monitor/overview)

- [Azure Monitor の価格](https://azure.microsoft.com/ja-jp/pricing/details/monitor/)

------
Log Analytics 詳細情報のリンク

- [はじめに（Log Analytics のチュートリアル）](https://docs.microsoft.com/ja-jp/azure/azure-monitor/logs/log-analytics-tutorial)

- [オンラインコース（Pluralsight）](https://www.pluralsight.com/courses/kusto-query-language-kql-from-scratch)

- [Log Analytics のデモ環境](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring_Logs/DemoLogsBlade)

------

- [Azure Monitor ログ レコードの標準プロパティ](https://docs.microsoft.com/ja-jp/azure/azure-monitor/platform/log-standard-properties)

- [Azure Monitor ログ クエリの便利な演算子](https://docs.microsoft.com/ja-jp/azure/azure-monitor/log-query/useful-operators)

- [Azure Monitor を使用してログ アラートを作成、表示、管理する](https://docs.microsoft.com/ja-jp/azure/azure-monitor/platform/alerts-log)

- [マイクロソフト提供の負荷ツール](https://blogs.msdn.microsoft.com/vijaysk/2012/10/26/tools-to-simulate-cpu-memory-disk-load/)

- [Service Health の概要](https://docs.microsoft.com/ja-jp/azure/service-health/service-health-overview)


## ログ検索の監視クエリ
```
# 死活監視
Heartbeat
| where Computer == "w-iaas-vm01"

# CPU利用率監視
Perf
| where Computer == "w-iaas-vm01"
| where CounterName == "% Processor Time"
| summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 5m), Computer
| sort by TimeGenerated asc

# メモリ使用率監視
Perf
| where Computer == "w-iaas-vm01"
| where CounterName == "% Committed Bytes In Use"
| summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 5m), Computer
| sort by TimeGenerated asc

# ディスク使用率監視
Perf
| where Computer == "w-iaas-vm01"
| where CounterName == "% Free Space"
| extend UsedSpace = 100 - CounterValue
| where InstanceName matches regex "[A-Z]:"
| summarize arg_max(TimeGenerated, UsedSpace) by Computer, InstanceName
| sort by InstanceName asc
| where (InstanceName == "C:" and UsedSpace > 70)
or (InstanceName == "D:" and UsedSpace > 70)
or (InstanceName == "E:" and UsedSpace > 70)

# イベントログ監視
Event
| where Computer == "w-iaas-vm01"
| where EventLevelName == "Error"
| where not (EventID == 46 and Source == "volmgr")
| project TimeGenerated , Computer , EventLog , EventLevelName , EventID , Source , RenderedDescription
| sort by TimeGenerated asc
```

## コマンド集
```bash
# 40GBのダミーファイルを作成
fsutil file createnew dummyfile 42949672960

# イベントログを書き込み
EVENTCREATE /ID 999 /L system /SO sys_test /T Error /D "これは System の Error のテストです。"
EVENTCREATE /ID 998 /L application /SO app_test /T Error /D "これは Application の Error のテストです。"
```
