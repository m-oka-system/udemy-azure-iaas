# Chapter3.簡単なWebサイトを構築してみよう

#### IISインストールとHTML作成
```powershell
# IISインストール
Install-WindowsFeature web-server

# HTMLファイル作成
New-Item -ItemType file -Path C:\inetpub\wwwroot\index.html -Value "<h1>$($env:computername)</h1>"
```