# remotello
Remote Tello control method using redis

# remotelloとは？

Ryze Tech. のTelloシリーズを、インターネット越しに操縦するためのシステムです。
2022年の会津大で行われるロボカップジャパンオープン　フライングロボットチャレンジで利用します。

# どんな仕組み？

Telloを直接インターネットに接続することは出来ないので、
TelloにつながったPCがインターネット上のサーバに画像を転送し、
クライアントPCはサーバから画像を受け取ります。

逆に、クライアントPCはTelloを操縦するコマンドをサーバに書き込み、
TelloにつながったPCはサーバーからコマンドを受け取ってTelloに送信します。

具体的には以下の図のようになります。

![image](https://user-images.githubusercontent.com/55542434/141723502-7d7a9e55-e558-4879-ab23-d1db6b38d028.png)

# サーバーは？

使用しているのは Redis データベースサーバーです。
メモリ(RAM)を使うデータベースなので、読み書きが高速です。

## データベースって難しそう。。。

Redisは一般的なSQL文を使用するデータベースサーバではありません。

```
r.set("名前",data)       # データの書き込み(rはサーバーのハンドルクラス)
data = r.get("名前")     # データの読み込み
```
このように、"名前"とデータを紐づけてset/getするだけなので、利用は簡単です。

MQTTのtopic名とpayloadデータみたいな気分で使えます。

SQLと比べて簡単ですが、"名前"の中に入っているデータがどんな形式なのかは、送受する人間が予め知っておかなければいけません。

## セキュリティは？

現在はredisサーバにセキュリティをかけていません。。。今後の課題です。

なので、悪意のある人間がTelloの映像を取ったり、飛行中に別のコマンドを送ったりする可能性があります。

ssl/tlsのオレオレ認証ぐらいはかけたほうが良いですかね。。。


# 開発環境

- Telloに接続するPC：　無線LAN(Telloへの接続用)、有線または無線LAN(インターネット接続用）
- クライアントPC：　有線または無線LAN(インターネット接続用）

- 使用言語：　Python3
- 主な使用ライブラリ：　OpenCV, NumPy, Redis, SSHTunnel

**動作確認したバージョン**

- Python3 3.8.10, 3.8.7
- opencv-pythonおよびopencv-contrib-python 4.5.3.56 , 4.6.0.66
- numpy 1.21.2, 1.22.4
- redis 3.5.3, 
- sshtunnel 0.4.0

# 依存ライブラリのインストール

```
$ pip3 install opencv-python opencv-contrib-python
$ pip3 install redis
$ pip3 install sshtunnel
```

# このgitの構成

- client: クライアントPCで利用するプログラム
- server: Telloに接続するPCで利用するプログラム

一応、Tello側のプログラムも掲載していますが、基本的にはクライアントPC側のプログラムだけで十分です。

