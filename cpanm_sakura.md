# cpanm をさくらのレンタルサーバーで利用するやり方

さくらのレンタルサーバー公式サイト

```
http://www.sakura.ne.jp/
```

スタンダード以降のシェルログインが出来るものが対象

レンタルサーバーへ接続

```sh
# 鍵認証による接続(認証などは事前にやっておく)
$ ssh yonabemt@yonabemt.sakura.ne.jp

# さくらのレンタルサーバーは csh
% pwd
/home/yonabemt

# さくらレンタルサーバーの perl
% which perl
/usr/bin/perl

# 5.14 が稼働している、現時点ではサーバーコントロールパネルから選択可能
% perl -v

This is perl 5, version 14, subversion 4 (v5.14.4) ... (省略)

# ここには sudo 権限がないとインストールできない
% perl -V:installsitelib
installsitelib='/usr/local/lib/perl5/site_perl/5.14';

# c シェルの使い方はよく分からないが、設定ファイル確認
% vim .cshrc

# $FreeBSD: src/share/skel/dot.cshrc,v 1.14.6.1 2008/11/25 02:59:29 kensmith Exp $
#
# .cshrc - csh resource script, read at beginning of execution by each shell
#
# see also csh(1), environ(7).
#

alias h         history 25
alias j         jobs -l
alias la        ls -a
alias lf        ls -FA
alias ll        ls -lA

# A righteous umask
umask 22

set path = (/sbin /bin /usr/sbin /usr/bin /usr/local/sbin /usr/local/bin $HOME/bin)

...(省略)

# vim の画面を抜ける bin ディレクトリはパスが通っているようである
# bin ディレクトリを作成 cpanm ファイルをダウンロード

% mkdir ~/bin
% cd ~/bin
% curl -L https://cpanmin.us/ -o cpanm
% chmod +x cpanm

# ここで一旦ログアウト、もう一度シェルの設定読み直し
% exit
logout

# 再度ログイン
$ ssh yonabemt@yonabemt.sakura.ne.jp

# パスが通っている事を確認
% which cpanm
/home/yonabemt/bin/cpanm

# Mojolicious をインストール
% cpanm -l ~/perl5/ Mojolicious
--> Working on Mojolicious
... (省略)
3 distributions installed

# インストール完了 Mojolicious で作ったスクリプトを CGI でうごかす
# web サーバーで表示させるには ~/www/ にスクリプトを作成する
% cd ~/www

# mojo コマンドにパスが通っているか
% which mojo
mojo: Command not found.

# パスを通すのが面倒なので、フルパスで指定
% which ~/perl5/bin/mojo
/home/yonabemt/perl5/bin/mojo

# サンプルスクリプトを作成
% perl -Mlib=../perl5/lib/perl5 ~/perl5/bin/mojo generate lite_app myapp.pl

# さくらのレンタルサーバーで動かすために修正 ファイルをコピー
% cp ./myapp.pl ./myapp.cgi

# パーミッション変更
% chmod 755 ./myapp.cgi

# vim でファイルを開いて use の部分を変更
% vim ./myapp.cgi

#!/usr/bin/env perl
use FindBin;
use lib "$FindBin::Bin/../perl5/lib/perl5";
use Mojolicious::Lite;

# Documentation browser under "/perldoc"
plugin 'PODRenderer';
... (省略)

# :wq で保存 vim 終了

# 念のためにシンタックスエラーの確認をしておく
% perl -c myapp.cgi
myapp.cgi syntax OK
```

web ブラウザで下記にアクセスして表示確認

```
http://yonabemt.sakura.ne.jp/myapp.cgi
```

動作確認ができたら、不要なファイルは削除しておくこと

```sh
% pwd
/home/yonabemt/www
rm myapp.cgi
rm myapp.pl
% cd ../
rm -rf perl5
```

まとめ

```
さくらのレンタルサーバーでうごかすには、
~/www 配下に実行ファイルを設定、パーミション 755 拡張子 .cgi にしておく
```
