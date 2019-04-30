# Chapter5.ロードバランサを使って負荷分散してみよう

## 参考サイト

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