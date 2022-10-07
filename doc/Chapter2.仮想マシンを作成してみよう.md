# Chapter2.仮想マシンを作成してみよう

## 参考サイト

- [Azure で一般化された VM の管理対象イメージを作成する](https://docs.microsoft.com/ja-jp/azure/virtual-machines/windows/capture-image-resource)

- [Virtual Machines シリーズ](https://azure.microsoft.com/ja-jp/pricing/details/virtual-machines/series/)

- [Azure の Windows 仮想マシンのサイズ](https://docs.microsoft.com/ja-jp/azure/virtual-machines/windows/sizes)

- [Windows Virtual Machines の料金](https://azure.microsoft.com/ja-jp/pricing/details/virtual-machines/windows/)

## コマンド集

#### Sysprepコマンド
```powershell
# Windowsの一般化
C:\Windows\System32\sysprep\sysprep.exe /generalize /oobe /mode:vm /shutdown
```