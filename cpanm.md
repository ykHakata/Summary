# cpanm まとめ

さくらのレンタルサーバーで Mojolicious を使える環境を構築するため cpanm 使い方まとめ

公式ドキュメント

```
https://metacpan.org/pod/App::cpanminus
```

単体で動かしたい場合はこのようにダウンロード

```bash
# ローカル環境でテスト、bin ディレクトリが存在する前提で
# bin ディレクトリはパスが通っている事

$ cd ~/bin
$ curl -L https://cpanmin.us/ -o cpanm
$ chmod +x cpanm

# cpnam ソースコードのページから読み込み、 cpanm というファイル名で保存
# ファイルに実行権限
# この使い方は cpanm 自体を更新することができないので、
# 最新の cpanm を使いたい場合はその都度手動でダウンロードが必要
```

cpnam 自体の使い方

```
https://metacpan.org/pod/distribution/App-cpanminus/bin/cpanm
```

実行できるようにパスが通っているかの確認

```bash
$ which cpanm
/Users/yk/bin/cpanm
```

モジュールインストール先のパスがどうなっているか確認

```bash
# plenv で perl インストールした場合モジュールのインストール先が plenv の管理下
$ perl -V:installsitelib
installsitelib='/Users/yk/.plenv/versions/5.24.0/lib/perl5/site_perl/5.24.0';
```

plenv で システム perl に変更

```bash
$ plenv global system
$ perl -V:installsitelib
installsitelib='/Library/Perl/5.18';

# この状態で cpanm を実行する場合、管理者権限 sudo が必要になる
# sudo 権限なしで cpanm を実行すると自動的にホームディレクトリ直下に
# perl5 というディレクトリを作りそこにモジュールをインストールしようとする

$ cpanm Readonly
... (省略)
! To turn off this warning, you have to do one of the following:
!   - run me as a root or with --sudo option (to install to /Library/Perl/5.18 and /usr/local/bin)
!   - Configure local::lib in your existing shell to set PERL_MM_OPT etc.
!   - Install local::lib by running the following commands
!
!         cpanm --local-lib=~/perl5 local::lib && eval $(perl -I ~/perl5/lib/perl5/ -Mlocal::lib)
!
--> Working on Readonly
# ... (インストールしているメッセージが沢山出力)
3 distributions installed

# このようなディレクトリ構造がつくられている
$ ls -al ~/perl5/
total 0
drwxr-xr-x   5 yk  staff   170  5 29 10:55 .
drwxr-xr-x+ 58 yk  staff  1972  5 29 11:06 ..
drwxr-xr-x   3 yk  staff   102  5 29 10:55 bin
drwxr-xr-x   3 yk  staff   102  5 29 10:55 lib
drwxr-xr-x   4 yk  staff   136  5 29 10:55 man

# 任意のディレクトリを作成してインストールしたい場合
# 指定したディレクトリが存在しな場合は自動的に作成される

$ cpanm -l ~/test/ Readonly
--> Working on Readonly
Fetching http://www.cpan.org/authors/id/S/SA/SANKO/Readonly-2.04.tar.gz ... OK
# ... (今度は警告なくインスール完了)
3 distributions installed

# このようなディレクトリ構造がつくられている
$ ls -al ~/test/
total 0
drwxr-xr-x   5 yk  staff   170  5 29 11:07 .
drwxr-xr-x+ 58 yk  staff  1972  5 29 11:06 ..
drwxr-xr-x   3 yk  staff   102  5 29 11:07 bin
drwxr-xr-x   3 yk  staff   102  5 29 11:07 lib
drwxr-xr-x   4 yk  staff   136  5 29 11:07 man
```

このやりかたでインストールしたモジュールを使い perl スクリプトを実行

```perl
use strict;
use warnings;
use FindBin;
use lib "$FindBin::Bin/../test/lib/perl5";
use Readonly;

Readonly my $sca => 'Readonly test';


print $sca, "\n";
# このスクリプトは ~/sandbox/test_readonly.pl での動きを想定
```

まとめ

```
cpanm を使えば任意のディレクトリにモジュールの配置が可能
利用する場合はソースコードに
use lib;
を利用するか
perl -Mlib= ... 実行ファイル
するかして、パスの指定に注意する事
```
