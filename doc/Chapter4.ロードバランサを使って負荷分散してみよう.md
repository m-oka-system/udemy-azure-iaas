# Chapter4.ロードバランサを使って負荷分散してみよう

## 参考サイト

- [Azure Load Balancer の概要](https://azure.microsoft.com/ja-jp/documentation/articles/virtual-machines-load-balance/)

- [Load Balancer の価格](https://azure.microsoft.com/ja-jp/pricing/details/load-balancer/)

- [Azure Load Balancer の分散モードを構成する](https://docs.microsoft.com/ja-jp/azure/load-balancer/load-balancer-distribution-mode)

- [Load Balancer と Application Gateway の通信の違い](https://blogs.technet.microsoft.com/jpaztech/2018/11/26/lb_appgw_traffic_different/)

## コマンド集

#### IIS停止と開始
```powershell
# IISの停止
Stop-Service W3SVC

# IISの状態確認
Get-Service W3SVC

# IISの開始
Start-Service W3SVC
```