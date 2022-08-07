# Chapter4.ロードバランサを使って負荷分散してみよう

## 参考サイト

- [Azure Load Balancer の概要](https://azure.microsoft.com/ja-jp/documentation/articles/virtual-machines-load-balance/)

- [Load Balancer の価格](https://azure.microsoft.com/ja-jp/pricing/details/load-balancer/)

- [Azure Load Balancer の分散モードを構成する](https://docs.microsoft.com/ja-jp/azure/load-balancer/load-balancer-distribution-mode)

- [Load Balancer と Application Gateway の通信の違い](https://blogs.technet.microsoft.com/jpaztech/2018/11/26/lb_appgw_traffic_different/)

## コマンド集

#### IIS停止と開始
```powershell
# IISの停止
Stop-Service W3SVC

# IISの状態確認
Get-Service W3SVC

# IISの開始
Start-Service W3SVC
```

# ロードバランサ作成手順(2022年8月時点)
Azureポータルの画面が変わっているため、動画講座の補足資料として画面ショット付きで説明を記載します。画面や仕様や日々変更される可能性があるため、ここに記載の画面や説明も最新の状態とは限らない事をあらかじめご了承ください。

本手順ではStandardのロードバランサを作成します。1か月使い続けると月額で約2600円程度かかります。仮想マシンのように停止して課金を止めることはできません。Chapter6の最初に削除しますので、コストを節約したい場合はなるべくまとめて学習されることをオススメします。

## 1.ロードバランサの作成
左側メニューのロードバランサーを押して、作成ボタンから新規作成します。

![ロードバランサの作成](https://user-images.githubusercontent.com/22112831/183281672-3a091a5e-1dd9-468f-823d-1faf165fb450.png)


**[基本]**

以下を入力して次（フロントエンドIP構成）へ。SKUは **「Standard」** を選択してください。
- 名前：w-iaas-lb-standard（任意）
- 地域：Japan West
- SKU：Standard
- 種類：パブリック
- レベル：地域

![ロードバランサの作成（基本）](https://user-images.githubusercontent.com/22112831/183275863-8f937608-054a-43af-bbe1-35f94c312838.png)


**[フロントエンドIP構成]**

「フロントエンドIP構成の追加」をクリック
- 名前：frontend（任意）
- IPバージョン：IPv4
- IPの種類：IPアドレス
- パブリックIPアドレス：新規作成をクリック
    - 名前：w-iaas-lb-standard-ip（任意）
    - SKU以下：すべてデフォルトのままで「OK」をクリック

「追加」をクリック


![ロードバランサの作成（フロントエンドIP構成）](https://user-images.githubusercontent.com/22112831/183275869-4f8e4593-4201-48d9-adf6-7e111e077139.png)

フロントエンドIP構成が追加されたことを確認し「確認および作成」へ進み作成を完了させてください。（バックエンドプール以降のタブの入力はあとで設定するので不要です）

![ロードバランサの作成（確認および作成）](https://user-images.githubusercontent.com/22112831/183275866-9c8bbfb0-9240-4971-9fad-5bd6f18ce180.png)

ロードバランサが作成できました。繰り返しになりますが、SKUが「Standard」になっていることを確認しておいてください。

![ロードバランサの作成（作成直後）](https://user-images.githubusercontent.com/22112831/183276133-0aa25f6d-2b42-4a19-9c69-5f7915c4f649.png)

次に、フロントエンドIPのパブリックIPアドレスにドメインを割り当てます。

この作業は必須ではありませんが、ドメインを割り当てることでIPアドレスが変わっても同じ名前でアクセスできるようになります。（あとの動作確認で使います）

パブリックIPアドレスの一覧から作成したフロントエンドIP用のパブリックIPアドレス（w-iaas-lb-standard）を開いて、構成メニューをクリックします（パブリックIPアドレスがお気に入りにない場合は「すべてのサービス」から検索してお気に入りに追加します）
- DNS名ラベル：w-iaas-lb-standard-ip（リージョンでユニーク）

ここではパブリックIPアドレスと同じ名前を入力しています。DNS名はリージョンごとでユニークである必要があり重複できないため、皆さんは他の名前を入力して進めてください。入力できたら「保存」をクリックしましょう。

![フロントエンドIPにドメインを割り当て](https://user-images.githubusercontent.com/22112831/183277827-74f3b324-35c8-4238-bfbf-def4f33adb15.png)

## 2.バックエンドプールの作成
続いてバックエンドプールを作成します。

ロードバランサのメニューの中にある「バックエンドプール」をクリックし、「追加」を押して新規作成しましょう。

![バックエンドプールの追加1](https://user-images.githubusercontent.com/22112831/183276441-9fbd175d-5a65-48d6-8aff-b0e9adfb3283.png)

以下を入力して「追加」をクリック
- 名前：backend（任意）
- 仮想ネットワーク：w-iaas-vnet（仮想マシンが存在する仮想ネットワーク）
- バックエンドプールの構成：NIC

![バックエンドプールの追加2](https://user-images.githubusercontent.com/22112831/183276442-57123760-2c03-404b-b8b8-bc46f4b72438.png)

Chapter3で作成済みの仮想マシン2台にチェックを入れて「追加」をクリック

![バックエンドプールの追加3](https://user-images.githubusercontent.com/22112831/183276443-48de2137-58e5-4f89-9d4d-988fa80f78f3.png)

元の画面に戻り、仮想2台が追加できていることを確認したら「保存」をクリック

![バックエンドプールの追加4](https://user-images.githubusercontent.com/22112831/183276446-31838c67-388a-4729-9de8-a51be90b2b79.png)


バックエンドプールが作成できたことを確認

![バックエンドプールの追加5](https://user-images.githubusercontent.com/22112831/183276447-8498b0b4-767e-4716-88c8-fc263d4d94fa.png)


## 3.正常性プローブの作成
続いて、正常性プローブを作成します。

ロードバランサのメニューの中にある「正常性プローブ」をクリックし、「追加」を押して新規作成しましょう。

![正常性プローブ1](https://user-images.githubusercontent.com/22112831/183276897-d9ec0322-5048-4150-8af4-da872e5afb73.png)

以下を入力して「追加」をクリック
- 名前：probe（任意）
- プロトコル：TCP
- ポート：80
- 間隔：5

プロトコルにTCPを選択した場合は、特定のポートにアクセスできるかチェックがされます。

HTTP、HTTPSを選択した場合は、ステータスコード200が返ってくれば正常とみなされます。

ここではデフォルトの「TCP」で進めます。

![正常性プローブ2](https://user-images.githubusercontent.com/22112831/183276898-8a50cc93-0905-40b7-a2c9-6ec7edbc172c.png)

正常性プローブが作成できたことを確認

![正常性プローブ3](https://user-images.githubusercontent.com/22112831/183276899-a9a36a1b-a99a-443c-a4ed-a495c35d9771.png)


## 4.負荷分散規則の作成
続いて、負荷分散規則を作成します。

ロードバランサのメニューの中にある「負荷分散規則」をクリックし、「追加」を押して新規作成しましょう。

![負荷分散規則1](https://user-images.githubusercontent.com/22112831/183277123-fb7ea116-7b5e-4b7b-937b-40a7f5674a92.png)

以下を入力して「追加」をクリック
- 名前：lb-rule（任意）
- IPバージョン：IPv4
- フロントエンドIPアドレス：frontend（作成済みのもの）
- バックエンドプール：backend（作成済みのもの）
- プロトコル：TCP
- ポート：80
- バックエンドポート：80
- 正常性プローブ：probe（作成済みのもの）
- セッション永続化以下：すべてデフォルトのまま

![負荷分散規則2](https://user-images.githubusercontent.com/22112831/183277188-38acd333-a904-40df-a889-2518fb2680d6.png)

負荷分散規則が作成できたことを確認

![負荷分散規則3](https://user-images.githubusercontent.com/22112831/183277125-89b1b4d0-2d34-4d68-85a4-8d2635187e00.png)

## 5.負荷分散の動作確認

「33. 負荷分散の動作確認」の動画の手順を参考にしてください。

## 6.インバウンドNAT規則の追加

インバウンドNAT規則の概要（仕組み）については「34. インバウンドNAT規則の追加」の動画を参考にしてください。


ロードバランサのメニューの中にある「インバウンドNAT規則」をクリックし、「追加」を押して新規作成しましょう。仮想マシン2台分のルールを作成します。

![インバウンドNAT規則の追加1](https://user-images.githubusercontent.com/22112831/183279399-8f61d4db-080a-4076-96cd-e0a253c10f74.png)

以下を入力して「追加」をクリック
- 名前：vm-01-rdp / vm-02-rdp（任意）
- 種類：Azure仮想マシン
- ターゲット仮想マシン：w-iaas-vm-01 / w-iaas-vm-02
- ネットワークIP構成：ipconfig1
- フロントエンドIPアドレス：frontend（作成済みのもの）
- フロントエンドポート：50000 / 50001
- サービスタグ：Custom
- バックエンドポート：3389
- プロトコル：TCP
- TCPリセットを有効にする以下：すべてデフォルトのまま

ポート番号は50000でなくても構いませんが、一般的に使われていないポートでわかりやすい番号にするのが良いです。vm01とvm02で重複しないポート番号にしましょう。

vm01のルールの入力例

![インバウンドNAT規則の追加2](https://user-images.githubusercontent.com/22112831/183279400-1387f639-ebf9-41c6-a577-e29c3535b39f.png)

同じ要領でvm02のルールも作成

![インバウンドNAT規則の追加3](https://user-images.githubusercontent.com/22112831/183279401-beacf404-958e-4409-a9df-437c0566a125.png)

インバウンドNAT規則が作成できたことを確認

![インバウンドNAT規則の追加4](https://user-images.githubusercontent.com/22112831/183279402-9ca0a49e-96a1-44f4-af0d-94cbc8f65a88.png)

続いて、RDP接続の動作確認をしてみましょう。

フロントエンドIPのパブリックIPアドレスの概要メニューを開いてDNS名をコピーします。

![インバウンドNAT規則の動作確認1](https://user-images.githubusercontent.com/22112831/183279403-98b042b7-3703-46d0-95f6-ca83cdcc08c9.png)

リモートデスクトップを起動してDNS名を貼り付けたら末尾にポート番号「:50000」または「:50001」を付け加えて接続します。

![インバウンドNAT規則の動作確認2](https://user-images.githubusercontent.com/22112831/183279404-6a81ed71-a2b9-4193-948b-d2b33667211e.png)

リモートデスクトップ接続時の証明書の警告画面からホスト名は判別できますが、

![インバウンドNAT規則の動作確認3](https://user-images.githubusercontent.com/22112831/183279405-921d58d4-2c15-4cdd-83cb-7e16200b2fd9.png)


RDP接続してからサーバマネージャやhostnameコマンドなどで確認してもOKです。
:50000でvm01に、:50001でvm02に接続できることを確認しましょう。

![インバウンドNAT規則の動作確認4](https://user-images.githubusercontent.com/22112831/183279406-eb524946-1915-4280-9cc1-f7372c23cb8f.png)

動作確認が終わったら、PowerShellの画面は閉じてリモートデスクトップは切断してください。

- Chapter5でロードバランサ経由で仮想マシンにRDP接続する手順があります。
- Chapter5で使うまでは仮想マシンを停止して頂いても構いません。
