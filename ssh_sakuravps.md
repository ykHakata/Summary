# NAME

```
ssh_sakuravps - ssh 設定 sakura vps と github
```

# SYNOPSIS

## 下記資料の読み方

__例: 以下の事例は各項目を置き換え__

```
ユーザー名: osomatu
アプリユーザー名: hackerz
IP: 160.16.231.20
```

## ログイン、ログアウト

ログイン
手元の PC -> sakuravps(自分のユーザー) -> アプリユーザーに変身

ログアウト
アプリユーザー -> sakuravps(自分のユーザー) -> 手元の PC

各自のユーザーへアクセス後にアプリケーションユーザーにアクセスの例

```
(ローカルから sakura vps へ)
$ ssh osomatu@160.16.231.20

(アプリユーザーに変身)
$ sudo su - hackerz

(アプリユーザーからログアウト)
[hackerz@tk2-257-38266 ~]$ logout

(一般ユーザーからログアウト)
[osomatu@tk2-257-38266 ~]$ logout
Connection to 160.16.231.20 closed.
```

# DESCRIPTION

## 大まかなセットアップの流れ

1. さくら VPS サーバーとローカル環境 (Mac)
    1. 鍵を準備
    1. ローカル側(mac)で鍵のペアを生成 -> (秘密鍵、公開鍵)
    1. 公開鍵 -> さくらVPS側に転送
    1. 秘密鍵 -> ローカル側 (mac) 保存
1. さくら VPS サーバーと Github
    1. さくら vps サーバー側で鍵のペアを生成 -> (秘密鍵、公開鍵)
    1. 公開鍵 -> Github 側に転送
    1. 秘密鍵 -> 開発サーバー (さくらvps側) 保存

## 事前に済ましておくこと

- sakura vps での基本的なシステムの設定
- 基本的な ssh の概要の理解
- ssh がインストール済みあることの確認
- github でのアカウント作成
- 基本的な git と github の概要と操作

## 入力値のルールを決めておく

## パスワード類の管理

# SETUP

## さくら VPS サーバーとローカル環境 (Mac)

さくら VPS 側に公開鍵の保管場所を作成

```
$ ssh osomatu@160.16.231.20
(ssh でパスワード認証で一旦アクセス)

$ pwd
/home/osomatu
(今いる場所)

$ ls -a
.  ..  .bash_history  .bash_logout  .bash_profile  .bashrc  .viminfo
(現在のディレクトリ状況確認)

$ mkdir ~/.ssh
(鍵を保管するディレクトリを作成)

$ chmod 700 ~/.ssh
(パーミッションを変更)
```

鍵の準備

```
もう一つタブを開き、ローカル側で鍵のペアを作る前にすでに作られているかを確認

ローカル側(mac)に移動
$ ls -a ~/.ssh/
.       config      id_rsa.pub
..      id_rsa      known_hosts

すでに存在するので今回はそのまま流用

id_rsa -> (秘密鍵)
id_rsa.pub -> (公開鍵)

$ chmod 600 ~/.ssh/id_rsa.pub
(念の為にパーミッションを変更)

(ファイルをさくらVPS側に転送 scp コマンドを使い、同時にファイル名も変更)
$ scp ~/.ssh/id_rsa.pub osomatu@160.16.231.20:~/.ssh/authorized_keys

(二つ目を追記したい場合はこちらのやり方)
$ cat ~/.ssh/id_rsa.pub | ssh osomatu@160.16.231.20 'cat >> ~/.ssh/authorized_keys'

osomatu@160.16.231.20's password:
(パスワード入力)

id_rsa.pub                                              100%  401    11.4KB/s   00:00
(転送完了)
```

さくら VPS でファイルが転送できているか確認

```
$ ls -a ~/.ssh/
.  ..  authorized_keys
(authorized_keys のファイルの中に公開鍵が収められてる)

$ exit
(ログアウト)
```

鍵認証でログイン

```
$ ssh -i ~/.ssh/id_rsa osomatu@160.16.231.20
(ローカル(mac)から鍵をつかってログイン)

[osomatu@tk2-257-38266 ~]$

$ exit
(ログアウト)

$ ssh osomatu@160.16.231.20
(ローカル(mac)から鍵をつかってログイン -> 省略して入力)
(デフォルトでは ~/.ssh/id_rsa を見るようになっている)

[osomatu@tk2-257-38266 ~]$

$ exit
(ログアウト)

本来はセキュリティーを高めるために ssh や
ファイアーウォールの設定を調整する必要があるが今回はここまで。
```

## さくら VPS サーバーと Github

開発サーバー (さくらVPS)

```
$ ssh osomatu@160.16.231.20

$ sudo su - hackerz

$ pwd
/home/hackerz
(今いる場所)

$ ls -a
.  ..  .bash_history  .bash_logout  .bash_profile  .bashrc
(現在のディレクトリ状況確認)

$ mkdir ~/.ssh
(鍵を保管するディレクトリを作成)

$ chmod 700 ~/.ssh
(パーミッションを変更)

$ ssh-keygen -t rsa -C 'hackerz'
(さくら VPS 側で鍵の生成、いろいろ聞かれるがすべてリターン)

$ ls -a ~/.ssh/
.  ..  id_rsa  id_rsa.pub

(id_rsa -> 秘密鍵 id_rsa.pub -> 公開鍵)
```

Github に公開鍵を登録

```
Github サイトの右上アイコン、プルダウン -> Settings
SSH and GPG keys -> SSH keys
New SSH key クリック

Title -> hackerz
key -> 公開鍵の内容をコピペする

$ cat ~/.ssh/id_rsa.pub
(cat コマンドで標準出力し内容をコピペ)

貼り付ける範囲は
ssh-rsa ... から
== '任意のコメント' 改行
まで

パスワードを聞かれるのでgithubのパスワード入力
```

接続の確認

```
[hackerz@tk2-257-38266 ~]$ ssh -T git@github.com
The authenticity of host 'github.com (192.30.255.113)' ...
... connecting (yes/no)? yes

(yes と入力)

Warning: Permanently added 'github.com,192.30.255.113' (RSA) to the list of known hosts.

(known_hosts に履歴が追加)

Hi ykHakata! You've successfully authenticated, but GitHub does not provide shell access.
(Github と接続ができたメッセージ)
```

# SEE ALSO

公式サイト

- SAKURA internet - <https://www.sakura.ad.jp/>

ログイン

- さくらインターネット 会員認証 - <https://secure.sakura.ad.jp/auth/login>
- vps 会員IDでログイン - <https://secure.sakura.ad.jp/vps/#/login>
- vps IPアドレスでログイン - <https://secure.sakura.ad.jp/vps/#/login?method=ip>

```
各認証に必要な ID password 情報は別途
```

マニュアル

- さくらのサポート情報 > VPS - <https://help.sakura.ad.jp/hc/ja/categories/201105252>
- 【さくらのVPS】サーバの初期設定ガイド - <https://help.sakura.ad.jp/hc/ja/articles/206208181>

```
基本的にはこちらのガイドにそって初期設定を行う、この中から手順を抜粋したものを紹介
```
