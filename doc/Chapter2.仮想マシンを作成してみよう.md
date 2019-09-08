# Chapter2.仮想マシンを作成してみよう

## 参考サイト

- [Azure IaaS 上に作成した Windows Server 2016 仮想マシンの UI を日本語化する](https://kogelog.com/2016/10/18/20161018-01/)

- [Azure で一般化された VM の管理対象イメージを作成する](https://docs.microsoft.com/ja-jp/azure/virtual-machines/windows/capture-image-resource)

- [Virtual Machines シリーズ](https://azure.microsoft.com/ja-jp/pricing/details/virtual-machines/series/)

- [Azure の Windows 仮想マシンのサイズ](https://docs.microsoft.com/ja-jp/azure/virtual-machines/windows/sizes)

## コマンド集

#### WindowsServer2016を日本語化
```powershell
### Localizing Windows Server 2016 into Japanese

# Variables
$LpUrl = "http://download.windowsupdate.com/c/msdownload/update/software/updt/2016/09/"
$LpFile = "lp_9a666295ebc1052c4c5ffbfa18368dfddebcd69a.cab"
$LpTemp = "C:\LpTemp.cab"

# Download Japanese language pack
Set-WinUserLanguageList -LanguageList ja-JP,en-US -Force
Start-BitsTransfer -Source $LpUrl$LpFile -Destination $LpTemp -Priority High
Add-WindowsPackage -PackagePath $LpTemp -Online
Set-WinDefaultInputMethodOverride -InputTip "0411:00000411"
Set-WinLanguageBarOption -UseLegacySwitchMode -UseLegacyLanguageBar
Remove-Item $LpTemp -Force
Restart-Computer

# Set display language to Japanese
Set-WinUILanguageOverride -Language ja-JP
Set-WinCultureFromLanguageListOptOut -OptOut $False
Set-WinHomeLocation -GeoId 0x7A
Set-WinSystemLocale -SystemLocale ja-JP
Set-TimeZone -Id "Tokyo Standard Time"
Restart-Computer
```

#### Sysprepコマンド
```powershell
# Windowsの一般化
C:\Windows\System32\sysprep\sysprep.exe /generalize /oobe /mode:vm /shutdown
```