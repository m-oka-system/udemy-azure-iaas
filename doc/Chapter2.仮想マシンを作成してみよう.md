# Chapter2.仮想マシンを作成してみよう

## 参考サイト

- [Azure IaaS 上に作成した Windows Server 2016 仮想マシンの UI を日本語化する](https://kogelog.com/2016/10/18/20161018-01/)

- [Azure で一般化された VM の管理対象イメージを作成する](https://docs.microsoft.com/ja-jp/azure/virtual-machines/windows/capture-image-resource)

## コマンド集
#### Sysprepコマンド
```powershell
# Windowsの一般化
C:\Windows\System32\sysprep\sysprep.exe /generalize /oobe /mode:vm /shutdown
```