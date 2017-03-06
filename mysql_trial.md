# NAME

mysql_trial - mysql 使い方簡単まとめ

# INSTALL

## Mac の場合

### homebrew を使ったインストール

今回、インストールには `homebrew` を使う

homebrew の特徴

    homebrew は mac に初期インストールされていないものを
    インストールするということと、出来るだけ安定版の新しいバージョンの
    アプリをインストールすることを前提としている
    homebrew の方針に従い、現時点で新しいものである mysql5.7 をインストール

#### ターミナル

```bash
# homebrew が使えることの確認
$ which brew
/usr/local/bin/brew

# 最新の状態に
$ brew update

# 基本的に homebrew は新しいバージョンのアプリをインストールすることを前提にしている
# mysql インストール
$ brew install mysql

# 英語ばかりでしんどくなるが、大事なことが書いてあるので読んでおく
# 圧縮ファイルをダウンロード
# ==> Installing dependencies for mysql: openssl
# ==> Installing mysql dependency: openssl
# ==> Downloading https://homebrew.bintray.com/bottles/openssl-1.0.2k.sierra.bottle.tar.gz
# ######################################################################## 100.0%
# ==> Pouring openssl-1.0.2k.sierra.bottle.tar.gz
# ==> Using the sandbox

# 警告 (ssl 周りの注意が書いてある)
# ==> Caveats
# A CA file has been bootstrapped using certificates from the SystemRoots
# keychain. To add additional certificates (e.g. the certificates added in
# the System keychain), place .pem files in
#   /usr/local/etc/openssl/certs

# and run
#   /usr/local/opt/openssl/bin/c_rehash

# This formula is keg-only, which means it was not symlinked into /usr/local.

# Apple has deprecated use of OpenSSL in favor of its own TLS and crypto libraries

# If you need to have this software first in your PATH run:
#   echo 'export PATH="/usr/local/opt/openssl/bin:$PATH"' >> ~/.bash_profile

# For compilers to find this software you may need to set:
#     LDFLAGS:  -L/usr/local/opt/openssl/lib
#     CPPFLAGS: -I/usr/local/opt/openssl/include

# ==> Summary
# 🍺  /usr/local/Cellar/openssl/1.0.2k: 1,696 files, 12M

# mysql インストール (5.7 がインストールされている)
# ==> Installing mysql
# ==> Downloading https://homebrew.bintray.com/bottles/mysql-5.7.17.sierra.bottle.1.tar.gz
# ######################################################################## 100.0%
# ==> Pouring mysql-5.7.17.sierra.bottle.1.tar.gz
# ==> /usr/local/Cellar/mysql/5.7.17/bin/mysqld --initialize-insecure --user=yk --basedir=/usr/local/Cellar/mysql/5.7.17 --datadir=/usr/local/var/mysql --tmpdi

# 警告 (root パスワードなしでインストールされている)
# ==> Caveats
# We've installed your MySQL database without a root password. To secure it run:
#     mysql_secure_installation

# ログインの仕方と自動起動の仕方が書いてある
# To connect run:
#     mysql -uroot

# To have launchd start mysql now and restart at login:
#   brew services start mysql
# Or, if you don't want/need a background service you can just run:
#   mysql.server start
# ==> Summary
# 🍺  /usr/local/Cellar/mysql/5.7.17: 321 files, 234.4M
# 終了

# 後でこれらのインストール時の内容を確認したい場合
$ brew info mysql
```

#### インストールログに従い動作確認

```bash
# 確認
$ which mysql
/usr/local/bin/mysql

# mysql を自動的に起動するように
$ brew services start mysql
# ==> Tapping homebrew/services
# Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-services'...
# remote: Counting objects: 10, done.
# remote: Compressing objects: 100% (7/7), done.
# remote: Total 10 (delta 0), reused 5 (delta 0), pack-reused 0
# Unpacking objects: 100% (10/10), done.
# Tapped 0 formulae (37 files, 50.7K)
# ==> Successfully started `mysql` (label: homebrew.mxcl.mysql)

# mysql にログインする
$ mysql -uroot
# Welcome to the MySQL monitor.  Commands end with ; or \g.
# Your MySQL connection id is 3
# Server version: 5.7.17 Homebrew

# Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

# Oracle is a registered trademark of Oracle Corporation and/or its
# affiliates. Other names may be trademarks of their respective
# owners.

# Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

# mysql>
```

# USAGE

最低限の使い方

mysql サーバへの接続

```bash
# homebrew の初期インストール設定は root パスワードなし
$ mysql -uroot
Welcome to the MySQL ... 省略

# 終了は exit
mysql> exit
Bye

# 丁寧な入力方法
$ mysql --host=localhost --user=root

# ログインするマシンと、mysql サーバーが同じ場合は省略
$ mysql --user=root

# --user と -u は同じ意味
$ mysql -u root
```

mysql サーバへの接続中の入力

```sql
-- 入力の最後はセミコロン ; でしめる
mysql> SELECT VERSION(), CURRENT_DATE;
+-----------+--------------+
| VERSION() | CURRENT_DATE |
+-----------+--------------+
| 5.7.17    | 2017-02-08   |
+-----------+--------------+
1 row in set (0.00 sec)

-- 小文字の入力でも可能だが、慣例的に SQL文は大文字を使うことが多い
mysql> select version(), current_date;

-- データベースを表示
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)
```

```sql
-- データーベースのカラムを削除
-- ALTER TABLE table DROP [COLUMN] column
-- 例: staff テーブルの name カラムを削除
ALTER TABLE staff DROP COLUMN name;

-- 短く
ALTER TABLE staff DROP name;

-- name, birthday 二つを削除
ALTER TABLE staff
    DROP COLUMN name,
    DROP COLUMN birthday;
```

# SEE ALSO

関連項目

- <http://dev.mysql.com/doc/refman/5.6/ja/> MySQL 5.6 リファレンスマニュアル (日本語)
- <http://brew.sh/index_ja.html> Homebrew — macOS 用パッケージマネージャー
