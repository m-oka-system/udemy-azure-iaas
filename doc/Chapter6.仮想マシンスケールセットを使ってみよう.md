# Chapter6.仮想マシンスケールセットを使ってみよう

## 参考サイト

- [Azure ポータルからDSCを設定する](https://learn.microsoft.com/ja-jp/azure/virtual-machines/extensions/dsc-overview#azure-portal-functionality)

- [Azure Desired State Configuration 拡張機能のバージョン履歴](https://learn.microsoft.com/ja-jp/azure/automation/automation-dsc-extension-history?view=powershell-7)

- [自動スケールのスケールルールについて](https://learn.microsoft.com/ja-jp/archive/blogs/jpcie/1315)

## コマンド集

#### PowershellDSC拡張機能の入力項目
```powershell
# Module-qualified Name of Configuration
WindowsWebServer.ps1\IISInstall

# Version
2.83
```

#### CPU100%にするコマンド

```powershell
while ($true) {}
```
