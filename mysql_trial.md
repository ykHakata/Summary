# NAME

mysql_trial - mysql 使い方簡単まとめ

# INSTALL

## Mac の場合

### homebrew を使ったインストール

今回、インストールには `homebrew` を使う

homebrew の特徴

```
homebrew は mac に初期インストールされていないものを
インストールするということと、出来るだけ安定版の新しいバージョンの
アプリをインストールすることを前提としている
homebrew の方針に従い、現時点で新しいものである mysql5.7 をインストール
```

#### ターミナル

```
(homebrew が使えることの確認)
$ which brew
/usr/local/bin/brew

(最新の状態に)
$ brew update

(基本的に homebrew は新しいバージョンのアプリをインストールすることを前提にしている)
(mysql インストール)
$ brew install mysql

(英語ばかりでしんどくなるが、大事なことが書いてあるので読んでおく)
(圧縮ファイルをダウンロード)
==> Installing dependencies for mysql: openssl
==> Installing mysql dependency: openssl
==> Downloading https://homebrew.bintray.com/bottles/openssl-1.0.2k.sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring openssl-1.0.2k.sierra.bottle.tar.gz
==> Using the sandbox

(警告 ssl 周りの注意が書いてある)
==> Caveats
A CA file has been bootstrapped using certificates from the SystemRoots
keychain. To add additional certificates (e.g. the certificates added in
the System keychain), place .pem files in
  /usr/local/etc/openssl/certs

and run
  /usr/local/opt/openssl/bin/c_rehash

This formula is keg-only, which means it was not symlinked into /usr/local.

Apple has deprecated use of OpenSSL in favor of its own TLS and crypto libraries

If you need to have this software first in your PATH run:
  echo 'export PATH="/usr/local/opt/openssl/bin:$PATH"' >> ~/.bash_profile

For compilers to find this software you may need to set:
    LDFLAGS:  -L/usr/local/opt/openssl/lib
    CPPFLAGS: -I/usr/local/opt/openssl/include

==> Summary
🍺  /usr/local/Cellar/openssl/1.0.2k: 1,696 files, 12M

(mysql インストール 5.7 がインストールされている)
==> Installing mysql
==> Downloading https://homebrew.bintray.com/bottles/mysql-5.7.17.sierra.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring mysql-5.7.17.sierra.bottle.1.tar.gz
==> /usr/local/Cellar/mysql/5.7.17/bin/mysqld --initialize-insecure --user=yk --basedir=/usr/local/Cellar/mysql/5.7.17 --datadir=/usr/local/var/mysql --tmpdi

(警告 root パスワードなしでインストールされている)
==> Caveats
We've installed your MySQL database without a root password. To secure it run:
    mysql_secure_installation

(ログインの仕方と自動起動の仕方が書いてある)
To connect run:
    mysql -uroot

To have launchd start mysql now and restart at login:
  brew services start mysql
Or, if you don't want/need a background service you can just run:
  mysql.server start
==> Summary
🍺  /usr/local/Cellar/mysql/5.7.17: 321 files, 234.4M
(終了)

(後でこれらのインストール時の内容を確認したい場合)
$ brew info mysql
```

#### インストールログに従い動作確認

```
(確認)
$ which mysql
/usr/local/bin/mysql

(mysql を自動的に起動するように)
$ brew services start mysql
==> Tapping homebrew/services
Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-services'...
remote: Counting objects: 10, done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 10 (delta 0), reused 5 (delta 0), pack-reused 0
Unpacking objects: 100% (10/10), done.
Tapped 0 formulae (37 files, 50.7K)
==> Successfully started `mysql` (label: homebrew.mxcl.mysql)

(mysql にログインする)
$ mysql -uroot
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.7.17 Homebrew

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

## CentOS6 (sakura vps 標準)

### yum を使ったインストール

基本的に yum を使った操作は管理者権限

```
$ sudo yum ... (一般ユーザーからは)

# yum ... (root でログインか su コマンドで変身)
```

CentOS6 標準状態でインストール

```
# yum -y install mysql-server

(古いバージョンだが 5.1 がインストール)
$ mysql --version
mysql  Ver 14.14 Distrib 5.1.73, for redhat-linux-gnu (x86_64) using readline 5.1

(サービス起動確認)
$ chkconfig --list mysqld

(自動起動に)
$ sudo chkconfig mysqld on
$ chkconfig --list mysqld
mysqld          0:off   1:off   2:on    3:on    4:on    5:on    6:off

(起動)
$ sudo /etc/rc.d/init.d/mysqld start
```

### 文字コードまわり

5.1 は初期状態では latin1 になっている

```
$ mysql -uroot

mysql> SELECT VERSION(), CURRENT_DATE;
+-----------+--------------+
| VERSION() | CURRENT_DATE |
+-----------+--------------+
| 5.1.73    | 2017-11-04   |
+-----------+--------------+
1 row in set (0.00 sec)

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.1.73, for redhat-linux-gnu (x86_64) using readline 5.1

Connection id:      2
Current database:
Current user:       root@localhost
SSL:            Not in use
Current pager:      stdout
Using outfile:      ''
Using delimiter:    ;
Server version:     5.1.73 Source distribution
Protocol version:   10
Connection:     Localhost via UNIX socket
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    latin1
Conn.  characterset:    latin1
UNIX socket:        /var/lib/mysql/mysql.sock
Uptime:         5 min 45 sec

Threads: 1  Questions: 7  Slow queries: 0  Opens: 15  Flush tables: 1  Open tables: 8  Queries per second avg: 0.20
--------------

(一旦 mysql ログアウトして設定ファイルを編集)

(設定ファイルは基本的にオリジナルは保存した上で編集)
$ sudo vi /etc/my.cnf

(下記を参考に変更)

-----
[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
user=mysql
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

# character-set
default-character-set=utf8
skip-character-set-client-handshake

[mysqld_safe]
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid

[client]
default-character-set=utf8

[mysql]
default-character-set=utf8

[mysqldump]
default-character-set=utf8
-----
```

もう一度上記を参考に mysql の status を確認

```
しかし、全てが latin1 から utf8 に変わるわけではない
ただし、スキーマーを構築する時に DEFAULT CHARSET=utf8;
をすれば、DB 側は utf8 になるようなのでこれ以上は深く追求しない
5.1 はすでに古いバージョンなので、とりあえず動かしたい場合でなければ
最近のバージョンを入れた方がいいだろう
```

### Perl から接続するためのモジュール

Perl からの接続には DBD::mysql が必要になるがおそらくインストールに失敗する

```
(追加で yum を使ってインストール)
$ sudo yum install mysql-devel
```

こちらでもインストールに失敗するようならさらにエラーログを調査

# USAGE

## 最低限の使い方

### mysql サーバへの接続

```
(homebrew の初期インストール設定は root パスワードなし)
$ mysql -uroot
Welcome to the MySQL ... (省略)

(終了は exit)
mysql> exit
Bye

(丁寧な入力方法)
$ mysql --host=localhost --user=root

(ログインするマシンと、mysql サーバーが同じ場合は省略)
$ mysql --user=root

(--user と -u は同じ意味)
$ mysql -u root
```

### mysql サーバへの接続中の入力

```
(入力の最後はセミコロン ; でしめる)
mysql> SELECT VERSION(), CURRENT_DATE;
+-----------+--------------+
| VERSION() | CURRENT_DATE |
+-----------+--------------+
| 5.7.17    | 2017-02-08   |
+-----------+--------------+
1 row in set (0.00 sec)

(小文字の入力でも可能だが、慣例的に SQL文は大文字を使うことが多い)
mysql> select version(), current_date;

(データベースを表示)
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

### mysql 使い方の基本手順

1. ユーザー作成
1. データベース作成
1. テーブル作成
1. レコード入力
1. レコード検索

#### 事前に用意しておくファイル

入力する SQL 文はあらかじめテキストファイルを作っておくと操作がラクになる

ユーザー名、パスワードは読替えてください

```
$ pwd
/Users/yk/tmp/sql
$ ls -l
total 24
-rw-r--r--@ 1 yk  staff    62  8  9 14:31 database.sql
-rw-r--r--@ 1 yk  staff  1978  8  9 14:27 schema.sql
-rw-r--r--@ 1 yk  staff   269  8  9 14:32 user.sql
```

```sql
-- ファイル名: database.sql
-- データーベース名: hackers
CREATE DATABASE hackers;
```

```sql
-- ファイル名: schema.sql
DROP TABLE if exists users;
CREATE TABLE users (
    `id` INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
    `user_id` VARCHAR(50) BINARY NOT NULL,
    `username` VARCHAR(50) BINARY NOT NULL,
    `password` VARCHAR(128) BINARY NOT NULL,
    `created_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    INDEX `user_id_and_passwrd` (user_id, password)
);

DROP TABLE if exists questions;
CREATE TABLE questions (
    `id` INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
    `question` TEXT BINARY NOT NULL,
    `answer` TEXT BINARY NOT NULL,
    `score` INT UNSIGNED NOT NULL,
    `level` INT UNSIGNED NOT NULL,
    `hint1` TEXT BINARY,
    `hint2` TEXT BINARY,
    `hint3` TEXT BINARY,
    `hint4` TEXT BINARY,
    `hint5` TEXT BINARY,
    `created_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

DROP TABLE if exists answers;
CREATE TABLE answers (
    `id` INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
    `question_id` INT UNSIGNED NOT NULL,
    `user_id` INT UNSIGNED NOT NULL,
    `user_answer` TEXT BINARY NOT NULL,
    `score` INT UNSIGNED NOT NULL,
    `hint1` INT UNSIGNED,
    `hint2` INT UNSIGNED,
    `hint3` INT UNSIGNED,
    `hint4` INT UNSIGNED,
    `hint5` INT UNSIGNED,
    `updated_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    INDEX `question_id_and_user_id` (question_id, user_id)
);

DROP TABLE if exists scores;
CREATE TABLE scores (
    `id` INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
    `user_id` INT UNSIGNED NOT NULL,
    `score` INT UNSIGNED NOT NULL,
    `updated_at` TIMESTAMP  NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    INDEX `user_id` (user_id)
);

DROP TABLE if exists sessions;
CREATE TABLE sessions (
    `session_id` VARCHAR(128) BINARY NOT NULL,
    `session_data` TEXT BINARY NOT NULL,
    `session_expires` DATETIME NOT NULL,
    `created_at` DATETIME NOT NULL,
    `updated_at` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

```sql
-- ファイル名: user.sql
-- ユーザー名: hackers / パスワード: password (任意の文字を入力)
CREATE USER 'hackers'@'localhost' IDENTIFIED BY 'password';

-- 権限の指定 すべての権限、スーパーユーザーに指定
GRANT ALL PRIVILEGES ON *.* TO 'hackers'@'localhost' WITH GRANT OPTION;
```

#### ユーザー作成

```
$ pwd
/Users/yk/tmp/sql

(接続 root ユーザーでデータベース mysql を指定して接続、 root のパスワードは設定していない前提)
$ mysql --user=root mysql

(ユーザーを作成する sql が書かれたファイルを読み込んで作成)
mysql> source ~/tmp/sql/user.sql
Query OK, 0 rows affected (0.04 sec)

Query OK, 0 rows affected (0.01 sec)

(アカウントに対する権限を確認)
mysql> SHOW GRANTS FOR 'hackers'@'localhost';
+------------------------------------------------------------------------+
| Grants for hackers@localhost                                           |
+------------------------------------------------------------------------+
| GRANT ALL PRIVILEGES ON *.* TO 'hackers'@'localhost' WITH GRANT OPTION |
+------------------------------------------------------------------------+
1 row in set (0.00 sec)

(一度ログアウトして、再度作成したユーザーでログイン)
mysql> exit
Bye
$ mysql -h localhost -u hackers -p
Enter password:
(パスワード入力)

Welcome to the MySQL monitor.  Commands end with ; or \g.
...(省略)

(現在の状況を確認)
mysql> status
--------------
mysql  Ver 14.14 Distrib 5.7.17, for osx10.12 (x86_64) using  EditLine wrapper

Connection id:      9
Current database:
Current user:       hackers@localhost
SSL:            Not in use
Current pager:      stdout
Using outfile:      ''
Using delimiter:    ;
Server version:     5.7.17 Homebrew
Protocol version:   10
Connection:     Localhost via UNIX socket
Server characterset:    utf8
Db     characterset:    utf8
Client characterset:    utf8
Conn.  characterset:    utf8
UNIX socket:        /tmp/mysql.sock
Uptime:         1 day 4 hours 33 min 33 sec

Threads: 1  Questions: 166  Slow queries: 0  Opens: 230  Flush tables: 1  Open tables: 223  Queries per second avg: 0.001
--------------
```

#### データベース作成

```
(データーベースの作成 sql が書かれたファイルを読み込んで作成)
mysql> source ~/tmp/sql/database.sql
Query OK, 1 row affected (0.04 sec)

(作成したデータベースに切り替える)
mysql> use hackers
Database changed

(一旦ログアウト)
mysql> exit
Bye

(データベースを指定してログイン)
$ mysql -h localhost -u hackers -p hackers
Enter password:
(パスワード入力)

Welcome to the MySQL monitor.  Commands end with ; or \g.
...(省略)

(データーベースに移動している)
mysql> status
--------------
mysql  Ver 14.14 Distrib 5.7.17, for osx10.12 (x86_64) using  EditLine wrapper

Connection id:      10
Current database:   hackers
Current user:       hackers@localhost
SSL:            Not in use
Current pager:      stdout
Using outfile:      ''
Using delimiter:    ;
Server version:     5.7.17 Homebrew
Protocol version:   10
Connection:     Localhost via UNIX socket
Server characterset:    utf8
Db     characterset:    utf8
Client characterset:    utf8
Conn.  characterset:    utf8
UNIX socket:        /tmp/mysql.sock
Uptime:         1 day 4 hours 39 min 18 sec

Threads: 1  Questions: 178  Slow queries: 0  Opens: 230  Flush tables: 1  Open tables: 223  Queries per second avg: 0.001
--------------

mysql>

```

#### テーブル作成

```
(テーブルの状況)
mysql> SHOW TABLES;
Empty set (0.00 sec)

(テーブル作成の sql が書かれたファイルを読み込んで作成)
mysql> source ~/tmp/sql/schema.sql
Query OK, 0 rows affected, 1 warning (0.03 sec)
... (テーブルの数だけ OK)

(テーブルの状況)
mysql> SHOW TABLES;
+-------------------+
| Tables_in_hackers |
+-------------------+
| answers           |
| questions         |
| scores            |
| sessions          |
| users             |
+-------------------+
5 rows in set (0.00 sec)

(個別のテーブルの詳細)
mysql> DESCRIBE answers;
+-------------+------------------+------+-----+-------------------+-----------------------------+
| Field       | Type             | Null | Key | Default           | Extra                       |
+-------------+------------------+------+-----+-------------------+-----------------------------+
| id          | int(10) unsigned | NO   | PRI | NULL              | auto_increment              |
| question_id | int(10) unsigned | NO   | MUL | NULL              |                             |
| user_id     | int(10) unsigned | NO   |     | NULL              |                             |
| user_answer | text             | NO   |     | NULL              |                             |
| score       | int(10) unsigned | NO   |     | NULL              |                             |
| hint1       | int(10) unsigned | YES  |     | NULL              |                             |
| hint2       | int(10) unsigned | YES  |     | NULL              |                             |
| hint3       | int(10) unsigned | YES  |     | NULL              |                             |
| hint4       | int(10) unsigned | YES  |     | NULL              |                             |
| hint5       | int(10) unsigned | YES  |     | NULL              |                             |
| updated_at  | timestamp        | NO   |     | CURRENT_TIMESTAMP | on update CURRENT_TIMESTAMP |
+-------------+------------------+------+-----+-------------------+-----------------------------+
11 rows in set (0.02 sec)
```

#### レコード入力

#### レコード検索

# EXAMPLE

## 削除

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

## パスワード設定

インストール初期時は root にパスワードはない

```
$ mysql -u root
mysql> SELECT User, Host, Password FROM mysql.user;
+---------+------------------+-------------------------------------------+
| User    | Host             | Password                                  |
+---------+------------------+-------------------------------------------+
| root    | localhost        |                                           |
| root    | yk-no-imac.local |                                           |
| root    | 127.0.0.1        |                                           |
| root    | ::1              |                                           |
|         | localhost        |                                           |
|         | yk-no-imac.local |                                           |
| hackers | localhost        | *8660BAF96F091F55F5D0593EF61DA65B6CA84088 |
+---------+------------------+-------------------------------------------+
7 rows in set (0.03 sec)

(**** は任意の文字列)
mysql> UPDATE mysql.user SET Password = PASSWORD('****') WHERE User = 'root';

(5.7から Password から authentication_string)
> UPDATE mysql.user SET authentication_string=PASSWORD('****') WHERE User='root';

Query OK, 4 rows affected (0.06 sec)
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.00 sec)

(登録された)
mysql> SELECT User, Host, Password FROM mysql.user;
+---------+------------------+-------------------------------------------+
| User    | Host             | Password                                  |
+---------+------------------+-------------------------------------------+
| root    | localhost        | *4FD439787F5AAF3CE2D68610AA04100CC93AA41E |
| root    | yk-no-imac.local | *4FD439787F5AAF3CE2D68610AA04100CC93AA41E |
| root    | 127.0.0.1        | *4FD439787F5AAF3CE2D68610AA04100CC93AA41E |
| root    | ::1              | *4FD439787F5AAF3CE2D68610AA04100CC93AA41E |
|         | localhost        |                                           |
|         | yk-no-imac.local |                                           |
| hackers | localhost        | *8660BAF96F091F55F5D0593EF61DA65B6CA84088 |
+---------+------------------+-------------------------------------------+
7 rows in set (0.00 sec)

(次回からはパスワードがないとログインできない)

$ mysql -u root -p
Enter password:
```

## バックアップ

```
(全てのデーターベース、テーブル、レコード)
$ mysqldump --user=hackers --password --events --all-databases > all_dump.sql
(指定のデータベース、テーブル、レコード)
$ mysqldump --user=hackers --password --databases hackers > hackers_all_dump.sql
(指定のデータベース、テーブル)
$ mysqldump --user=hackers --password --databases --no-data hackers > hackers_schema_dump.sql
(指定のデーターベースのテーブルスキーマーのみ)
$ mysqldump --user=hackers --password --no-data hackers > hackers_table_schema_dump.sql
(指定のデーターベースのテーブルレコードのみ)
$ mysqldump --user=hackers --password --no-create-info hackers > hackers_data_dump.sql
```

## 復元

```
(ユーザーができていること)
$ mysql --user=hackers --password < all_dump.sql
(データベースの作成からデータまで復元)
$ mysql --user=hackers --password < hackers_all_dump.sql
(データベースの作成からテーブルスキーマーまで復元)
$ mysql --user=hackers --password < hackers_schema_dump.sql
(指定のデーターベースのテーブルスキーマーのみ、データベースができていること)
$ mysql --user=hackers --password hackers < hackers_table_schema_dump.sql
(指定のデーターベースのテーブルレコードにデータを復元、テーブルスキーマーができてること)
$ mysql --user=hackers --password hackers < hackers_data_dump.sql
```

# SEE ALSO

関連項目

- <http://dev.mysql.com/doc/refman/5.6/ja/> MySQL 5.6 リファレンスマニュアル (日本語)
- <http://brew.sh/index_ja.html> Homebrew — macOS 用パッケージマネージャー
- <https://qiita.com/blackowl/items/c692982d86ad359e410c> - mysql 文字コード参考記事
