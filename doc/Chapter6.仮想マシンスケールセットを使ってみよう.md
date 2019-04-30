# Chapter6.仮想マシンスケールセットを使ってみよう

## 参考サイト

- [Azure Desired State Configuration 拡張機能のバージョン履歴](https://docs.microsoft.com/ja-jp/powershell/dsc/getting-started/azureDscexthistory)
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