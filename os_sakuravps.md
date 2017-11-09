# NAME

```
os_sakuravps - sakura vps でのシステムの基本設定
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

1. システム再インストール
1. OS の基本設定
1. ユーザー作成

## 事前に済ましておくこと

- sakura vps の契約申し込み
- ユーザー名やパスワードの値を考えておく

## 入力値のルールを決めておく

__ルールの事例__

- root のパスワード (ASCII文字, 8文字以上, 数字と組み合わせ)
- 一般ユーザー名 (ASCII文字, 小文字, 使う人の名前)
- 一般ユーザーパスワード (ASCII文字, 8文字以上, 数字と組み合わせ)
- アプリケーション用の一般ユーザー名 (ASCII文字,  小文字, アプリケーション名)
- 一般ユーザーパスワード (ASCII文字, 8文字以上, 数字と組み合わせ)

## パスワード類の管理

- 初期パスワードは各自変更する
- パスワード類の管理はキーチェーンなどのアプリを活用するか完全に記憶する

## サーバー情報確認

__下記の項目は確認しておく__

- vps: ***
- v4: ***
- v6: ***

# SETUP

## システム再インストール

vps IPアドレスでログイン - <https://secure.sakura.ad.jp/vps/#/login?method=ip>

```
IP: 160.16.231.20
password: 別途参照
```

サーバ情報の名前と説明を追加

```
各種設定 -> サーバー情報編集
名前: hackerz
説明: クイズシステムを中心とした統合された hackerz コミュニティーのためのシステム
```

OS を再インストールする

```
各種設定 -> OS インストール -> 標準 OS インストール
新しい root パスワード入力 (8文字以上、半角英数組合せ)
標準 OS の CentOS6 選択
スタートアップスクリプトは無効
稼働中の表示が出るまで待機
```

## OS の基本設定

ローカルより vps サーバーへアクセス

__アクセスできない場合__

```
ssh: connect to host 153.126.137.205 port 22: Connection refused
(osを再インストールして稼働中の緑になっても実際はインストールが終わってない時がある)
(>_コンソール (VNCコンソール) から情報を確認する)
```

__yum がうまくいかない場合__

```
# yum update
Loaded plugins: fastestmirror, security
Setting up Update Process
Loading mirror speeds from cached hostfile
 * base: ftp.iij.ad.jp
 * epel: ftp.riken.jp
 * extras: ftp.iij.ad.jp
 * updates: ftp.iij.ad.jp
Error: Cannot retrieve repository metadata (repomd.xml) for repository: extras. Please verify its path and try again

(このような場合は)
# yum clean all
# yum update
```

```
# はスーパーユーザー
$ は一般ユーザー

ターミナルでアクセス、パスワードで ssh 接続
( ~/.ssh/known_hosts に過去の接続情報がある場合は注意、履歴削除 )

$ ssh root@160.16.231.20

The authenticity of host '160.16.231.20 (160.16.231.20)' ...
...  (yes/no)?

(接続続行を尋ねてくるので yes)

Warning: Permanently added '160.16.231.20' (RSA) to the list of known hosts.

(known_hosts に履歴が追加)

root@160.16.231.20's password:

(パスワードを入力)

SAKURA Internet [Virtual Private Server SERVICE]

[root@tk2-257-38266 ~]#

(ログイン成功)
```

CentOS を最新の状態にしておく

```
[root@tk2-257-38266 ~]# yum update

(途中で何度か [y/n] と聞いてくるが y で入力)

...
Complete!
[root@tk2-257-38266 ~]#
```

CentOS バージョン確認

```
[root@tk2-257-38266 ~]# cat /etc/issue
CentOS release 6.9 (Final)
Kernel \r on an \m
```

日本語化

```
[root@tk2-257-38266 ~]# man man

(man コマンドのオンラインマニュアルが表示、英語表示になっている q で画面を抜ける)

[root@tk2-257-38266 ~]# vim /etc/sysconfig/i18n

LANG="C"
SYSFONT="latarcyrheb-sun16"

(これを...)

LANG="ja_JP.UTF-8"
SYSFONT="latarcyrheb-sun16"

(ファイル保存後、再起動)
[root@tk2-257-38266 ~]# service network restart

(日本語で表示になっている)
[root@tk2-257-38266 ~]# man man

(もしうまくいかないばいは一旦ログアウト後ログイン)
```

## ユーザー作成

一般ユーザー (アプリケーション用ユーザー) の作成

```
ユーザーを作るコマンドは管理者権限で行う

一般ユーザーを作るコマンド
useradd

マニュアル
man useradd

ユーザーID: hackerz
password:  (8文字以上、半角英数組合せ)
```

root でログイン状態

```
(ユーザー名設定)
[root@tk2-257-38266 ~]# useradd hackerz

(パスワード設定 passwd につづけてユーザー名を指定)
[root@tk2-257-38266 ~]# passwd hackerz

(パスワード入力)

(一般ユーザーでも管理者と同様のことをできるように設定 sudo)
[root@tk2-257-38266 ~]# usermod -G wheel hackerz

[root@tk2-257-38266 ~]# visudo

(検索する)
/wheel

(エイスケープ # を外して有効にする)
%wheel ALL=(ALL) ALL

保存(esc 押して : )
wq

[root@tk2-257-38266 ~]# exit
(ログアウト)
```

一般ユーザーでアクセス確認 (ローカルから vps へ)

```
$ ssh hackerz@160.16.231.20
hackerz@160.16.231.20's password:

( hackerz のパスワード入力 )

SAKURA Internet [Virtual Private Server SERVICE]

[hackerz@tk2-257-38266 ~]$

(ログイン完了)

(念のため今いる場所を確認)

$ pwd
/home/hackerz
```

一般ユーザー (作業用ユーザー) の作成

```
ユーザーの作成は管理者権限が必要なので sudo コマンドを活用
ユーザーID: osomatu
password: (8文字以上、半角英数組合せ)
```

hackerz でログイン状態

```
[hackerz@tk2-257-38266 ~]$ sudo useradd osomatu

We trust you have received ...
...
[sudo] password for hackerz:
(自分のパスワードを入力)

(以降はアプリケーションユーザー作成の時と同じ手順)
$ sudo passwd osomatu
$ sudo usermod -G wheel osomatu
(パスワード入力)
$ exit
(ログアウト)
(上記の手順で必要なだけユーザーを作成)
```

一般ユーザーでアクセスしたあと、アプリケーションユーザーに変身するやり方

```
$ ssh osomatu@160.16.231.20

SAKURA Internet [Virtual Private Server SERVICE]

[osomatu@tk2-257-38266 ~]$
(osomatu でログインしている)

[osomatu@tk2-257-38266 ~]$ sudo su - hackerz

We trust you have received ...
...
[sudo] password for osomatu:
(osomatu のパスワード入力)

[hackerz@tk2-257-38266 ~]$

(アプリケーションユーザー hackerz としてログインしている)
(アプリの更新などは hackerz ユーザーに変身してから行う)
(ログアウトも2回)

[hackerz@tk2-257-38266 ~]$ logout
[osomatu@tk2-257-38266 ~]$ logout
Connection to 160.16.231.20 closed.
```

登録ユーザーの一覧を確認するには？

```
$ getent passwd
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
