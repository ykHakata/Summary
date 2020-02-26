# NAME

perl5_install - perl5 ローカル環境での設定手順

## DESCRIPTION

perl5 の環境構築についての説明

お手元の PC が Mac もしくは Linux 系の OS ならば間違いなく Perl はインストールされている

## Mac の場合

Finder > 移動 > ユーティリティー > ターミナル

```text
Mac にはターミナルというコマンドラインで操作をするアプリケーションが付属している
perl5 はコマンドラインによるアプリケーションの種類に属するので
ターミナルの基本的な使い方に理解がない方はインストールは難しい
```

ターミナル起動後

```console
# /usr/bin/perl 大抵は bin の配下に実行スクリプトが置かれている
$ which perl
/usr/bin/perl

# perl のバージョン情報
$ perl -v

This is perl 5, version 18, subversion 2 (v5.18.2) built ...
...
Internet, point your browser at http://www.perl.org/, the Perl Home Page.

(すぐにでも perl5 は使える状況になっている)
```

インストールされているのになぜわざわざインストール作業をするのか？

```text
大抵の UNIX 系のシステムはシステム自体が Perl5 を使っていることが多く Mac も例外ではない
本格的なプログラミングを Perl5 で行う場合は system perl は使わない方が良いというのが
昨今の Perl プログラマーの共通の認識である

MacOS がバージョンアップされた場合、自動的に perl5 もバージョンが変わったり
拡張モジュールのインストールが面倒だったり、
system perl に付属している標準モジュールもすこし違ったりすることが稀にあり
問題が発生した場合の対応が大変になる可能性がある

もし、書き捨ての簡単なスクリプトや拡張モジュールを使わず、間違いなく存在するであろう
標準モジュールのみの使用を前提としているなら、 system perl でも良いかもしれない

system perl を使うつもりならいろんなことを割り切って
使ってゆかなくてはいけなくなることを知っておくと良い

-----
system perl - 初期時にインストール済みの Perl5 を system perl と表現している
```

## Linux 系の場合

```text
Mac の場合の手順を読めば理解はしてもらえる
不安を感じる方やコマンドラインの操作が苦手な方は Mac にした方がよい
```

## Windows 系の場合

Windows に Perl5 はインストールされておらず、インストールが必要である

[ActivePerl](http://www.activestate.com/activeperl) もしくは [StrawberryPerl](http://strawberryperl.com/) が無難だ

他にも環境構築は様々な方法があるが、本格的に開発をしようとなると途端に難易度が高くなる

事情があって Windows にこだわるのなら仕方ないが Mac か Linux 系を使うのをお勧めする

## INSTALL

具体的なインストールの手順 - MacOS の場合

## インストールを開始するまえに

必ず最新の状態にアップデートしておく

```text
Mac の場合は必ず Xcode と xcode コマンドツールをインストールし最新の状態に
アップデートしておく、特に Perl モジュールの場合は Perl だけで
完結しているわけでなく、他にも依存要素がある場合がある
過去に EL Capitan になったときに SSL 周りが変更になり
Email::Sender::Transport::SMTP::TLS がうまくインストールできなかったり、
Sierra になった時に DBD::mysql がうまくインストールできなかったことがある
しばらくしてアップデートされ、 `xcode-select --install` を最新にすれば解消された。
モジュールがうまくインストールできない場合は
まずは最新の状態にアップデートするとよい。
```

## モジュールのインストールにつまづく場合

### MacOS の場合

```text
上記で書いたようにアップデートが基本だが OS のメジャーなアップデートの場合は
リリース数ヶ月後にアップデートしてからが無難
Xcode と xcode コマンドラインツールは同時にインストールされているわけでないことに注意
最新の状態でもインストールに失敗の場合はエラーログを調査しかないが、
インストールしようとしているモジュールは本当に必要なものなのか？
cpan のサイトでモジュールのメンテナンスがあまり行われていないものは
利用するのは注意した方がいい
```

### CentOS6 (sakura vps 標準)

```console
(ssl 周りのモジュールで失敗の場合)
$ sudo yum install openssl-devel

(DBD::mysql が失敗の場合、 mysql をインストール済みが前提で)
$ sudo yum install mysql-devel
```

## plenv

perl5 バージョン管理システム

```console
(plenv がインストールされているか確認)
$ plenv --version
$ plenv 2.1.1-9-g26dbef7
```

plenv がインストールされていなければインストール

```console
(詳しい手順は https://github.com/tokuhirom/plenv 参照)
$ git clone git://github.com/tokuhirom/plenv.git ~/.plenv
$ echo 'export PATH="$HOME/.plenv/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(plenv init -)"' >> ~/.bash_profile
$ exec $SHELL -l
$ git clone git://github.com/tokuhirom/Perl-Build.git ~/.plenv/plugins/perl-build/
```

## Perl5

汎用スクリプト言語

```console
(今回は 5.20.2 をインストール)
$ plenv install 5.20.2

(仕上げ)
$ plenv rehash

(システム Perl から plenv でインストールした Perl に変更)
$ plenv global 5.20.2

(確認)
$ plenv versions
  system
* 5.20.2 (set by /Users/HOME/.plenv/version)

(... Users/HOME/.plenv/ ... の HOME はお使いの環境によって異なります)
```

## cpanm

Perl5 拡張モジュールインストールコマンド

```console
$ plenv install-cpanm

(確認)
$ which cpanm
/Users/HOME/.plenv/shims/cpanm
```

## Carton

拡張モジュール管理システム

```console
(Carton インストール)
$ cpanm Carton

(確認)
$ carton -v
carton v1.0.12
```

## Perl::Tidy

Perl5 のソースコードを整形する拡張モジュール

```console
(Perl::Tidy インストール)
$ cpanm Perl::Tidy

(確認)
$ perltidy -v
This is perltidy, v20140711

Copyright 2000-2014, Steve Hancock

(... 省略)
```

## SEE ALSO

関連項目

- <http://www.perl.org/> Perl Programming Language
- <https://github.com/tokuhirom/plenv> plenv - perl binary manager
- <https://metacpan.org/pod/Carton> Carton - Perl module dependency manager
- <https://metacpan.org/pod/Perl::Tidy> Perl::Tidy - Parses and beautifies perl source
- <http://www.activestate.com/activeperl> ActivePerl - ActiveState
- <http://strawberryperl.com/> Strawberry Perl for Windows
- <http://perldoc.jp/docs/articles/github.com/tokuhirom/plenv/blob/master/README.md> - plenv README の日本語訳
