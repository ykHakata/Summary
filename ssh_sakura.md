# NAME

```
ssh_sakura - さくらのレンタルサーバー (スタンダード) での ssh 利用
```

# SYNOPSIS

## 読み方

- __local__ -> ローカル(Mac)の画面 bash
- __sakura__ -> さくらのレンタルサーバー内の画面 csh

## ローカル側(mac)とレンタルサーバー

```
・ローカル側(mac)で鍵のペアを生成 -> (秘密鍵、公開鍵)
・公開鍵 -> レンタルサーバー側に転送
・秘密鍵 -> ローカル側 (mac) 保存
```

## Github 側とレンタルサーバー

```
・レンタルサーバー側で鍵のペアを生成 -> (秘密鍵、公開鍵)
・公開鍵 -> Github 側に転送
・秘密鍵 -> レンタルサーバー側 保存
```

## Github との接続の確認

__local__

```
$ ssh -T git@github.com
```

### sakura レンタルサーバーで git clone によるリボジトリの生成

__sakura__

```
(例: リポジトリ git@github.com:ykHakata/yonabe.git)
% git clone git@github.com:ykHakata/yonabe.git
```

### Github より pull - sakura レンタルサーバーへ

__sakura__

```
(git リポジトリのある場所へ)
% pwd
/home/yonabemt/www/yonabe

% git pull origin master
```

# DESCRIPTION

## SETTING - 環境構築

### 注意事項

__記事の内容は下記のアカウント情報でのやり方レンタルサーバー契約時に異なるので読み換えること__

```
アカウント:  yonabemt
初期ドメイン: yonabemt.sakura.ne.jp
```

### パスワードでの ssh 接続

__local__

```
$ ssh yonabemt@yonabemt.sakura.ne.jp
(パスワード入力)
```

### レンタルサーバー内に公開鍵の設置

__sakura__

```
% pwd
/home/yonabemt
(今いる場所)

% ls -a
.       .login      .shrc       db      tmp
..      .login_conf .spamassassin   ports       www
.cshrc      .my.version .ssh        sakura_pocket
.history    .profile    MailBox     sblo_files
(現在のディレクトリ状況確認)

% ls -al
...
drwx------    2 yonabemt  users   512 Mar 13  2009 .ssh
...
(パーミッションは 700 になってる、ファイルの所有者、読み書き実行可)

% ls -a .ssh/
.   ..
(なにも存在しない)
```

### 鍵の準備

__ローカル側で鍵のペアを作る前に、すでに作られているかを確認__

```
id_rsa -> (秘密鍵)
id_rsa.pub -> (公開鍵)
```

__local__

```
(ローカル側(mac)に移動)
$ ls -a ~/.ssh/
.       config      id_rsa.pub
..      id_rsa      known_hosts

(すでに存在するので今回はそのまま流用)

$ chmod 600 ~/.ssh/id_rsa.pub
(念の為にパーミッションを変更)

$ scp ~/.ssh/id_rsa.pub yonabemt@yonabemt.sakura.ne.jp:~/.ssh/authorized_keys
(ファイルをレンタルサーバー側に転送 scp コマンドを使い、同時にファイル名も変更)

yonabemt@yonabemt.sakura.ne.jp's password:
(パスワード入力)

id_rsa.pub                                              100%  401     0.4KB/s   00:00
(転送完了)
```

### 鍵認証でログイン

__local__

```
$ ssh -i ~/.ssh/id_rsa yonabemt@yonabemt.sakura.ne.jp
(ローカル(mac)から鍵をつかってログイン)
```

__sakura__

```
Welcome to FreeBSD!

%

% exit
(ログアウト)

$ ssh yonabemt@yonabemt.sakura.ne.jp
(ローカル(mac)から鍵をつかってログイン -> 省略して入力)
(デフォルトでは ~/.ssh/id_rsa を見るようになっている)

Welcome to FreeBSD!

%

% exit
(ログアウト)
```

### レンタルサーバー内で鍵の生成

__local__

```
$ ssh yonabemt@yonabemt.sakura.ne.jp
```

__sakura__

```
% ls -a ~/.ssh/
.  ..  authorized_keys

(ローカル(mac)とレンタルサーバーで通信するための公開鍵 authorized_keys が存在)

% ssh-keygen -t rsa -C 'yonabemt'
(レンタルサーバー側で鍵の生成、いろいろ聞かれるがすべてリターン)

% ls -a ~/.ssh/
.  ..  authorized_keys  id_rsa  id_rsa.pub

(id_rsa -> 秘密鍵 id_rsa.pub -> 公開鍵)
```

### Github に公開鍵を登録

```
Github サイトの右上アイコン、プルダウン -> Settings
Personal settings -> SSH and GPG keys
New SSH key クリック

Title -> sakura_standard_yonabemt
key -> 公開鍵の内容をコピペする
```

__sakura__

```
% cat ~/.ssh/id_rsa.pub
(cat コマンドで標準出力し内容をコピペ)
```

```
貼り付ける範囲は
ssh-rsa ... から
== '任意のコメント' 改行
まで

パスワードを聞かれるので github のパスワード入力
```

### 接続の確認

__sakura__

```
% ssh -T git@github.com
The authenticity of host 'github.com (192.30.252.129)' ...
... connecting (yes/no)? yes

(yes と入力)

Warning: Permanently added 'github.com' (RSA) to the list of known hosts.

(known_hosts に履歴が追加)

Hi ykHakata! You've successfully authenticated, but GitHub does not provide shell access.

(Github と接続ができたメッセージ)
```

### Github にリポジトリを新規作成

```
右上 + プルダウン -> New repository

Repository name: yonabe
Description (optional): よなべ Perl 勉強会用ランディングページ
Initialize this repository with a README -> チェック

Create repository -> クリック
```

### ローカル環境でリポジトリにコンテンツを追加

__local__

```
$ cd ~/Github/
$ git clone git@github.com:ykHakata/yonabe.git

...

$ cd ~/Github/yonabe/
(ここへコンテンツを一通り移動)

$ git add -A
$ git commit -m '新規作成'
$ git push origin master
```

### レンタルサーバーで git clone によるリボジトリの生成

__sakura__

```
% which git
/usr/local/bin/git

% git --version
git version 2.7.0

% pwd
/home/yonabemt/www

% git clone git@github.com:ykHakata/yonabe.git

% cd ~/www/yonabe/
% git status
On branch master
Your branch is up-to-date with 'origin/master'.

nothing to commit, working directory clean
```

### ローカル環境でコンテンツ修正、Github へ push

__local__

```
(コンテンツ修正後)

$ git add index.html
$ git commit -m 'タイトル変更'
$ git push origin master
```

### レンタルサーバーへ、Github より pull

__sakura__

```
% pwd
/home/yonabemt/www/yonabe
(居場所確認)

% git fetch
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 3 (delta 2), reused 3 (delta 2), pack-reused 0
Unpacking objects: 100% (3/3), done.
From github.com:ykHakata/yonabe
   b5f7e0c..c1354db  master     -> origin/master
(リポジトリを最新の状態に)

% git status
On branch master
Your branch is behind 'origin/master' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working directory clean
(git pull ができる状態)

% git pull origin master
From github.com:ykHakata/yonabe
 * branch            master     -> FETCH_HEAD
Updating b5f7e0c..c1354db
Fast-forward
 index.html | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)
(ローカルと同じ状態)
```

# SEE ALSO
