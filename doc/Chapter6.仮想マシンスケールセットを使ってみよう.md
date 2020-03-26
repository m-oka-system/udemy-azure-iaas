# Chapter6.仮想マシンスケールセットを使ってみよう

## 参考サイト

- [Azure ポータルからDSCを設定する](https://docs.microsoft.com/ja-jp/azure/virtual-machines/extensions/dsc-overview#azure-portal-functionality)

- [Azure Desired State Configuration 拡張機能のバージョン履歴](https://docs.microsoft.com/ja-jp/powershell/scripting/dsc/getting-started/azuredscexthistory?view=powershell-7)

- [自動スケールのスケールルールについて](https://blogs.msdn.microsoft.com/jpcie/?p=1315)

## コマンド集

#### PowershellDSC拡張機能の入力項目
```powershell
# Module-qualified Name of Configuration
WindowsWebServer.ps1\IISInstall

# Version
2.76
```

#### CPU100%にするコマンド

```powershell
while ($true) {}
```