# Chapter3.簡単なWebサイトを構築してみよう

## 参考サイト

- [VM のサイズ変更について （2018年10月3日 追記）](https://blogs.technet.microsoft.com/jpaztech/2016/04/15/vmresize/)

## コマンド集

#### IISインストールとHTML作成
```powershell
# IISインストール
Install-WindowsFeature web-server

# HTMLファイル作成
New-Item -ItemType file -Path C:\inetpub\wwwroot\index.html -Value "<h1>$($env:computername)</h1>"
```