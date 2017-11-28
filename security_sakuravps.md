# NAME

```
security_sakuravps - 最低限のセキュリティの設定
```

# SYNOPSIS

## 下記資料の読み方

__例: 以下の事例は各項目を置き換え__

```
ユーザー名: osomatu
IP: 160.16.231.20
```

## ログイン、ログアウト

ログイン
手元の PC -> sakuravps(自分のユーザー) -> 管理者権限に変身

ログアウト
アプリユーザー -> sakuravps(自分のユーザー) -> 手元の PC

各自のユーザーへアクセス後にアプリケーションユーザーにアクセスの例

```
(ローカルから sakura vps へ)
$ ssh osomatu@160.16.231.20

(権利者権限に変身)
$ sudo su - root

(アプリユーザーからログアウト)
[root@tk2-257-38266 ~]$ logout

(一般ユーザーからログアウト)
[osomatu@tk2-257-38266 ~]$ logout
Connection to 160.16.231.20 closed.
```

# DESCRIPTION

## 事前に済ましておくこと

- sakura vps での基本的なシステムの設定
- 基本的な UNIX の知識
- 一般的なセキュリティの知識

## ファイルを編集する場合はオリジナルをバックアップ

管理者権限でしか編集できないようなファイルは必ずオリジナルを残す

例: 慣例として最後に .org や .old などの拡張子をつける

```
# mv default.conf default.conf.org
```

# SETUP

## iptables

```
基本的に利用するポート番号を解放するのが無難な設定
sakura vps などでシステムをインストールすると 22 以外は使えないようになっている
web サーバーは 80 ポートを利用するため、解放する必要がある
```

ログイン後、管理者になっておく iptables コマンド確認

```
# which iptables
/sbin/iptables

# cd /etc/sysconfig

(すでに iptables.old がつくられてる)
# ls -al ip*
-rw-------. 1 root root  560 12月  6 19:11 2016 ip6tables
-rw-------. 1 root root 1988  7月 24 11:10 2015 ip6tables-config
-rw-------. 1 root root  560 12月  6 19:11 2016 ip6tables.old
-rw-------. 1 root root  476 12月  6 19:11 2016 iptables
-rw-------. 1 root root 1974  7月 24 11:10 2015 iptables-config
-rw-------. 1 root root  476 12月  6 19:11 2016 iptables.old
(修正)
# vim iptables
```

下記の行を 22 ポート下に追記して保存する

```
    -A INPUT -p tcp --dport 80 -j ACCEPT
```

iptables を再起動

```
# service iptables restart
```

設定情報確認

```
# iptables -nL
```

# SEE ALSO

__参考記事__

- <http://knowledge.sakura.ad.jp/beginner/4048/> - さくらのナレッジ
