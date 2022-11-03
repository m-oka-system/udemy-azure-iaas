# Chapter10.仮想マシンを監視してみよう

## 参考サイト

- [Azure Monitor の概要](https://learn.microsoft.com/ja-jp/azure/azure-monitor/overview)

- [Azure Monitor の価格](https://azure.microsoft.com/ja-jp/pricing/details/monitor/)

------
Log Analytics ヘルプのリンク

- [Log Analytics のチュートリアル](https://learn.microsoft.com/ja-jp/azure/azure-monitor/logs/log-analytics-tutorial)

- [オンラインコース（Pluralsight）](https://www.pluralsight.com/courses/kusto-query-language-kql-from-scratch)

- [Log Analytics のデモ環境](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring_Logs/DemoLogsBlade)

------

- [Azure Monitor ログ内の標準列](https://learn.microsoft.com/ja-jp/azure/azure-monitor/logs/log-standard-columns)

- [Kusto 照会言語 (KQL) の概要](https://learn.microsoft.com/ja-jp/azure/data-explorer/kusto/query/)

- [新しいアラート ルールを作成する](https://learn.microsoft.com/ja-jp/azure/azure-monitor/alerts/alerts-create-new-alert-rule?tabs=log)

- [マイクロソフト提供の負荷ツール](https://learn.microsoft.com/ja-jp/archive/blogs/vijaysk/tools-to-simulate-cpu-memory-disk-load)

- [Service Health の概要](https://learn.microsoft.com/ja-jp/azure/service-health/service-health-overview)


## ログ検索の監視クエリ
```
# 死活監視
Heartbeat
| where Computer == "w-iaas-vm01"

# CPU利用率監視
Perf
| where Computer == "w-iaas-vm01"
| where ObjectName == "Processor Information"
| where CounterName == "% Processor Time"
| sort by TimeGenerated asc

# メモリ使用率監視
Perf
| where Computer == "w-iaas-vm01"
| where ObjectName == "Memory"
| where CounterName == "% Committed Bytes In Use"
| sort by TimeGenerated asc
```

## コマンド集

