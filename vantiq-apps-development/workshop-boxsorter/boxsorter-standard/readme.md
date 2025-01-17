# Vantiq ワークショップ 概要

## 荷物仕分け (Box Sorter) アプリケーション 

読み取った送り先コードで荷物を仕分けするアプリケーションの実装を通じて Vantiq の基本機能や MQTT について学びます。  

### Beginner コースとの違い

Beginner コースとは下記の点が異なります。

#### MQTT

オーバーヘッドの多い **HTTP** プロトコルに変わり、通信量がより少ない **MQTT** プロトコルを利用してデータの送受信を行います。  
また、通信プロトコルの変更に伴い **Topic** ではなく **Soruce** を利用してデータの送受信を行います。

#### Source

Vantiq では、外部システムとの接続ポイントとして、 Source というリソースが用意されています。  
Source を利用することで、様々な通信プロトコルを用いたデータの送受信が可能となります。

#### CachedEnrich

Type へ毎回 アクセスを行う **Enrich** ではなく、 Type のデータをメモリ上にキャッシュすることでより高速な処理が可能になる **CachedEnrich** を用いるように変更します。

#### SplitByGroup

CachedEnrich を用いる上で必要になる **SplitByGroup** を用います。  

Vantiq では複数の処理ノードにイベントが分散されて処理されています。  
事前に **SplitByGroup** を用いることで、任意のキー単位でイベントをグルーピングし、処理されるノードを固定できるようになります。



## 荷物仕分けシステムの全体のイメージ

<img src="./imgs/overview.png" width="800">

1. バーコードリーダーで荷物のバーコードを読み取る。
1. 読み取った結果を MQTTブローカーに送信する。
1. Vantiq は MQTTブローカーから読み取り結果を取得する。
1. Vantiq はその結果を元に仕分け処理を行う。
1. Vantiq は仕分け指示を MQTTブローカーに送信する。
1. 制御システムは仕分け指示を MQTTブローカーから取得する。
1. 制御システムは仕分け指示に従ってソーターを制御する。

[実物のイメージはこちら](https://www.youtube.com/watch?v=1LvaiA3N0E8&t=282s)

ワークショップではVantiqの担当部分である No.3〜5 を実装します。
> No.1〜2 は、 Google Colaboratory を利用し、 MQTTブローカーに読み取り結果のサンプル情報を送信することで代用します。  
> Google Colaboratory の詳細は [こちら](/vantiq-google-colab\docs\jp\colab_basic_knowledge.md) で解説しています。

### Vantiqで実装する荷物仕分け (Box Sorter) アプリケーション 概要

<img src="./imgs/vantiq-app.png" width="600">

このアプリケーションを実装していきます。  
詳細は次のステップで説明しますが、 `MQTTブローカーから情報を取得` 、`仕分け` 、`仕分け指示を MQTTブローカーに送信` という処理を行います。

## 各自で準備するVantiq以外の要素(事前にご準備ください)

- MQTTブローカー
  - Vantiq から仕分け結果を送信する先として使用します。
  - お好きなブローカーをご利用ください。  
    AmazonMQ などマネージドなものを使っても、 ActiveMQ や Mosquitto をご自身でインストールして準備しても構いません。
  - [The Free Public MQTT Broker by HiveMQ](https://www.hivemq.com/public-mqtt-broker/) のように無料で使用できるブローカーもございます。
  - Vantiq やご自身のクライアントからアクセスできる必要がありますのでインターネット接続できる必要があります。
- Google アカウント
  - Google Colaboratory を利用するために使用します。
- MQTTクライアント（Google Colaboratory を利用しない場合）
  - ご自身の環境から MQTTブローカーに接続し、メッセージをパブリッシュしたりサブスクライブするのに使用します。
  - お好きなクライアントをご利用ください（[MQTT X](https://mqttx.app/) など）。

## ドキュメント

- [手順](./instruction.md)