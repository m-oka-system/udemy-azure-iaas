# 概要

2023/11/6 時点の事象

仮想マシンに接続して Azure Storage Explorer をインストール後、起動時に "A JavaScript error occurred in the main process" のエラーが表示される。

![AzureStorageExplorerInstallError](https://github.com/m-oka-system/udemy-azure-iaas/assets/22112831/ed3a8a25-3042-4616-8d4e-7c21c7b0a30f)

# 環境

- 対象レクチャー：Chapter5 AzureStorageExplorer の使い方
- OS: Windows Server 2022 DataCenter Azure Edition の仮想マシン

# 対応方法

2023/11/6 時点の最新バージョン v1.32.0 で発生する事象のようなので、ひとつ前のバージョン v1.31.2 をインストールする。

1. 設定 → アプリからインストール済みの Azure Storage Explorer をアンインストールする
1. 仮想マシンを再起動する
1. GitHub の [Release](https://github.com/microsoft/AzureStorageExplorer/releases) ページから v1.31.2 の「StorageExplorer-windows-x64.exe」をダウンロードする
1. 講座の手順どおりインストールする（途中、すでにフォルダが存在するメッセージが出ても上書きして OK）

![AzureStorageExplorerGitHubReleasePage](https://github.com/m-oka-system/udemy-azure-iaas/assets/22112831/3293c6aa-1b91-45f5-82f7-959ab9e9938e)
