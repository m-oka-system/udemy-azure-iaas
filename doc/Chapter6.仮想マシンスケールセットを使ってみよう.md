# Chapter6.仮想マシンスケールセットを使ってみよう

## 参考サイト

- [Azure ポータルからDSCを設定する](https://docs.microsoft.com/ja-jp/azure/virtual-machines/extensions/dsc-overview#azure-portal-functionality)

- [Azure Desired State Configuration 拡張機能のバージョン履歴](https://docs.microsoft.com/ja-jp/powershell/scripting/dsc/getting-started/azuredscexthistory?view=powershell-7)

- [自動スケールのスケールルールについて](https://docs.microsoft.com/ja-jp/archive/blogs/jpcie/1315)

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

# 仮想マシンスケールセット作成手順(2020年4月時点)

**[基本]**
仮想マシン作成時と同じように基本情報を入力する。
イメージはあらかじめ作成(日本語化)しているカスタムイメージを選択する。
![VMSSの作成-基本](https://user-images.githubusercontent.com/22112831/80272531-80942480-8705-11ea-95b5-44254e1975e3.png)

**[ディスク]**
Premium SSDのまま次へ。

![VMSSの作成-ディスク](https://user-images.githubusercontent.com/22112831/80272582-f5fff500-8705-11ea-8586-0836961acaff.png)

**[ネットワーク]**
仮想ネットワークは既存のものを選択する。
ネットワークインターフェイスのところに表示されている名前の右端のえんぴつアイコンをクリックする。
![VMSSの作成-ネットワーク(編集前)](https://user-images.githubusercontent.com/22112831/106752472-d7079980-666d-11eb-8644-a6103073bd21.png)
**[ネットワークインターフェイスの編集]**
ネットワークインターフェイスの名前は任意で修正する。
あとでまとめて削除する時に絞り込みしやすいように「vmss」を含めておくのがベター。
NICネットワークセキュリティグループを **「なし」に修正する。**
![VMSSの作成-ネットワーク(NIC編集)](https://user-images.githubusercontent.com/22112831/80272852-13ce5980-8708-11ea-8883-0a8f277a80b5.png)

**[ネットワーク]**
元の画面に戻って「ロードバランサーを使用する」を「はい」に変更して
Azure Load Balancerを新規作成する。
![VMSSの作成-ネットワーク(編集後)](https://user-images.githubusercontent.com/22112831/80272630-22b40c80-8706-11ea-9a1d-7fb12e818884.png)

**[拡大縮小中]**
初期インスタンス数を「1」に変更して、その他はデフォルトのままで進める。
![VMSSの作成-拡大縮小中](https://user-images.githubusercontent.com/22112831/80272883-6871d480-8708-11ea-982d-7dbae278f826.png)

**[管理]**
「ブート診断」を「オフ」に変更して、その他はデフォルトのままで進める。
![VMSSの作成-管理](https://user-images.githubusercontent.com/22112831/80272903-99520980-8708-11ea-883a-c5faef9cb460.png)

**[正常性]**
無効のまま次へ。
![VMSSの作成-正常性](https://user-images.githubusercontent.com/22112831/80272922-bb4b8c00-8708-11ea-8b21-20580e5b6478.png)

「詳細」「タグ」の項目はデフォルトのまま進めて作成する。

## その他注意点

VMSS新規作成時にパブリックIPアドレス名、ドメイン名ラベルが入力できなくなっています。
パブリックIPアドレスはロードバランサとセットで新規作成されるため、作成されたパブリックIPアドレスに対して、あとからドメイン名ラベルを設定することができます。(パブリックIPアドレスの構成メニュー)
※IPアドレスで接続する場合は必須ではありません。


Chapter4の「ロードバランサの作成」の後半に解説している手順を参考にしてみてください。
![パブリックIPアドレスのDNS名ラベル](https://user-images.githubusercontent.com/22112831/80273120-8b9d8380-870a-11ea-89a6-a4f5d455b1a8.png)


<br>

# PowershellDSCのインストール手順(2020年7月時点)
DSCのスクリプトを直接アップロードすることができなくなり、ご自身のストレージアカウントに
アップロードしてから選択する手順に変更になりましたので、その手順について補足いたします。

「Configuration Modules or Script」の参照ボタンをクリックする。
![DSCインストール1](https://user-images.githubusercontent.com/22112831/86205154-23ed3400-bba4-11ea-995d-db253307fc3c.png)

<br>

画面上部の「+ストレージアカウント」から新規作成して、作成したものを選択する。
![DSCインストール2](https://user-images.githubusercontent.com/22112831/86205252-5860f000-bba4-11ea-8830-84e2f162c696.png)

<br>

アップロード先のコンテナーを選択する。
ブート診断用に自動的に作成されているものを選択してもOKだが新しくコンテナーを作成したほうがわかりやすい。
画面上部の「+コンテナー」から新規作成することができる。画面の例では「powershelldsc」というコンテナーを選択している。
![DSCインストール3](https://user-images.githubusercontent.com/22112831/86205334-8d6d4280-bba4-11ea-8448-c1a661bc69d2.png)

<br>

アップロードをクリックする。
![DSCインストール4](https://user-images.githubusercontent.com/22112831/86205450-dae9af80-bba4-11ea-91de-e01cd8a09aea.png)

<br>

ファイルの選択アイコンをクリックして、あらかじめGitHubからダウンロードしている「WindowsWebServer.zip」を選択する。
![DSCインストール5](https://user-images.githubusercontent.com/22112831/86205466-e937cb80-bba4-11ea-90a8-eefca8845856.png)

<br>

アップロードをクリックする。
![DSCインストール6](https://user-images.githubusercontent.com/22112831/86205556-21d7a500-bba5-11ea-81ff-a51406dd0353.png)

<br>

コンテナーの中にアップロードされた「WindowsWebServer.zip」をクリックして選んだ状態にしてから選択ボタンをクリックする。
![DSCインストール7](https://user-images.githubusercontent.com/22112831/86205569-2bf9a380-bba5-11ea-85df-d8486a616cc7.png)

<br>

残りの以下の項目は動画の通り入力して進める。
Module-qualified Name of Configuration：WindowsWebServer.ps1\IISInstall
Version：2.76

![DSCインストール8](https://user-images.githubusercontent.com/22112831/86205687-78dd7a00-bba5-11ea-92b5-77bceec17077.png)

<br>

DSCのインストール完了後は動画の通りインスタンスのアップデートの操作を行って、ブラウザでアクセスするとHTMLが表示されることを確認してみてください。
