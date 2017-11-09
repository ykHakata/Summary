# NAME

```
nginx_sakuravps - sakura vps での nginx の基本設定
```

# SYNOPSIS

## 下記資料の読み方

__例: 以下の事例は各項目を置き換え__

```
ユーザー名: osomatu
アプリユーザー名: hackerz
IP: 160.16.231.20
アプリ名: HackerzLab
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

1. nginx インストール
1. 起動、停止確認
1. 設定ファイルの設置

## 事前に済ましておくこと

- sakura vps での基本的なシステムの設定
- 80 番ポートを解放しておく
- 公開したい web アプリの設置

# SETUP

## nginx インストール

CentOS release 6.9 (Final) の場合

サーバーに一般ユーザーでログイン後

```
$ sudo yum install nginx

(途中で [y/N] と聞いてくるが y でリターン)
(実行できない場合はパッケージが登録されていない場合がある、その場合は別途 yum の設定が必要)

(確認)
$ which nginx
/usr/sbin/nginx

(使い方の詳細はこちら)
$ man nginx

(今回インストールのバージョン)
$ nginx -v
nginx version: nginx/1.10.2

(サーバーの起動は root 権限でないとできない)
$ nginx
nginx: [alert] could not open error log file: open() "/var/log/nginx/error.log" failed (13: Permission denied)
...
```

## 起動、停止確認

管理者権限に変わって起動させる

```
$ sudo su - root
# nginx
# ps -ax
Warning: bad syntax, perhaps a bogus '-'? See /usr/share/doc/procps-3.2.8/FAQ
  PID TTY      STAT   TIME COMMAND
    1 ?        Ss     0:01 /sbin/init
...

 9837 ?        Ss     0:00 nginx: master process nginx
 9838 ?        S      0:00 nginx: worker process
 9839 ?        S      0:00 nginx: worker process
...

(サーバーを停止)
# nginx -s quit

(もう一度)
# nginx
```

web ブラウザで確認

```
(Mac の場合は open コマンドが使える)
$ open http://160.16.231.20:80

Welcome to nginx on EPEL!
の文字がブラウザに表示されれば無事起動
ポートの設定については事前に iptables で 80番ポートを解放する必要がある
```

うまく起動できない場合

```
nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
(こういうメッセージのときは何かが 80 を利用 80 を使っているものを調べる)

$ sudo lsof -i:80
```

## 設定ファイルの設置

nginx の設定ファイル

以下管理者権限で操作

```
# cat /etc/nginx/nginx.conf
...
    include /etc/nginx/conf.d/*.conf;
...
(最後の行で別のファイルを読み込んでいる)
```

実際に読み込まれているファイル

```
# ls -al /etc/nginx/conf.d/
-rw-r--r-- 1 root root  451 10月 31 21:37 2016 default.conf
-rw-r--r-- 1 root root  686 10月 31 21:37 2016 ssl.conf
-rw-r--r-- 1 root root  283 10月 31 21:37 2016 virtual.conf
(ssl.conf と virtual.conf は中身はコメントアウトされている)
```

nginx 用の conf を mojo の etc 配下に設置して読み込む

`/home/hackerzlab/HackerzLab/etc/nginx.conf` - nginx の設定ファイル

```
# cd /etc/nginx/conf.d/

(シンボリックリンク)
# ln -s /home/hackerzlab/HackerzLab/etc/nginx.conf hackerzlab.conf
# ls -al

...
lrwxrwxrwx 1 root root   42  6月 26 22:04 2017 hackerzlab.conf -> /home/hackerzlab/HackerzLab/etc/nginx.conf
...

(名前を変更)
# mv default.conf default.conf.org
# ls -al
合計 20
drwxr-xr-x 2 root root 4096  6月 26 22:05 2017 .
drwxr-xr-x 4 root root 4096  6月 25 20:09 2017 ..
-rw-r--r-- 1 root root  451 10月 31 21:37 2016 default.conf.org
lrwxrwxrwx 1 root root   42  6月 26 22:04 2017 hackerzlab.conf -> /home/hackerzlab/HackerzLab/etc/nginx.conf
-rw-r--r-- 1 root root  686 10月 31 21:37 2016 ssl.conf
-rw-r--r-- 1 root root  283 10月 31 21:37 2016 virtual.conf

(nginx を起動しなおし)
# nginx -s quit
# nginx
```

# SEE ALSO

nginx.conf の記入例

```nginx
# hackerzlab アプリサーバー
upstream hackerzlab-app {
  server 127.0.0.1:5000;
}

# http://hackerzlab.com (http://160.16.231.20:80)
server {
  listen 80;
  server_name hackerzlab.com;
  location / {
    proxy_pass http://hackerzlab-app;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }
}
```
