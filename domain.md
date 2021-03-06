# NAME

domain - web のドメイン設定について簡単まとめ

# DESCRIPTION

web の基本的な仕組みについて

```
お手元のPCから web ブラウザを立ち上げ URL を入力し web サーバーへリクエストし、
リクエスト結果が web サーバから、お手元の PC に返却されてるのが
基本的な一連のうごきであるが、その際に入力する URL は
例えば、 `https://www.yahoo.co.jp/` のような形になっている
`https://183.79.135.206` のように IP アドレスを直接入力でもアクセスできることは
できる。

現実的には数字の羅列は入力や判別が難しいので文字列で代用したものをドメインという形で活用している。

基本的には IP アドレスは世界に一つで、その IP を参照しているドメインも一つである
ということになっているが、実際には IP とドメインの組み合わせの設定方法は
様々なものが存在するので、ドメインからIPまたはその逆も、完全に特定するのは
難しかったりします。

IP とドメインの設定方法の基本的なものだけを具体的な手順を参考にまとめておく
```

ドメイン自体の取得

既存のドメイン取得サービスを活用する

ドメイン取得サービスは様々業者が存在するがやっていることは
そんなに変わらず、価格も極端に開きはないが、
「お名前.com」 を活用することにする

事前準備

登録用のメールアドレス
支払い用のクレジットカード情報
登録者の氏名、住所などの情報
ドメインが co.jp などの企業向けの場合は会社自体の情報が必要

- <https://www.onamae.com/> - お名前.com

一連のユーザー登録をすませる

画面の指示に従えば、希望のドメインを検索し、申し込みまですすめる
完了すると登録のメールアドレスに登録されたドメインに関する情報が送信されてくるので、
大事にアーカイブしておく。

公開用の web サーバーを用意する

既存のサーバーレンタルサービスを活用する

サーバーレンタルサービスも様々な業者が存在するが
「さくらインターネット」を活用する

- <https://www.sakura.ad.jp/> - さくらインターネット


事前準備

登録用のメールアドレス
支払い用のクレジットカード情報
登録者の氏名、住所などの情報

一連のユーザー登録をすませる

レンタルするサーバーを選ぶ

目安としては
いわゆるホームページ(静的なwebサイト)を作りたいなら
さくらのレンタルサーバー(スタンダード)を選ぶ
webアプリ(動的なwebサイト)を作りたいなら
さくらのVPS(512MB)を選ぶ
何かを開発するときのコツは小さく産んで大きく育てる
この原則を守っていれば大きく失敗することはない。
従って、一番安いプランから始めると良い。
さくらのレンタルサーバーについては一番安い(ライト)が存在するが、
こちらは ssh でのシェルログインができず、後々やりにくいことが起こるので、
さくらのレンタルサーバーについては(スタンダード)から始めた方がいい。

ドメインネームサーバー(DNS)の指定をする

お名前.com でドメインを申し込んだ場合、最初は、お名前.comのネームサーバーが
設定されている。
今回はネームサーバーをさくらインターネットのネームサーバーに設定する
こうすると、ドメインが参照する IP アドレスなどの細かい設定は
さくらインターネット側で操作することになる。

ネームサーバーの指定は下記の記事などを参考にする

お名前.com 側は以上で終了になる

さくらのレンタルサーバーの場合のドメインの設定

独自ドメインの設定についてはドキュメントが存在するのでそちらを
参考にするのが良いが、注意点は、ドメインの設定画面は２種類ある
レンタルサーバーの管理画面から設定する画面と
会員情報から設定する画面

レンタルサーバーで独自ドメインを活用するにはレンタルサーバーの管理画面から
設定する必要がある

細かい手順は公式ドキュメントを参考にする

# SEE ALSO

- <> - ***
- <> - ***
