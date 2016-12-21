# NAME

perl6_trial - Perl6 使い方簡単まとめ

# INSTALL

使い初めまでの準備

homebrew を使ったインストール

```bash
$ brew install rakudo-star

# インストール後 確認
$ which perl6
/usr/local/bin/perl6

# バージョン情報
$ perl6 -v
This is Rakudo version 2016.11 built on MoarVM version 2016.11

# man ページはエントリーされていないので、使い方は公式サイトから
$ man perl6
No manual entry ...
```

# USAGE

最低限の使い方

```bash
# ワンライナーの書き方 -e オプション、始まりと終わりは '' で囲む
$ perl6 -e 'say "hello perl6"';
hello perl6

# インタプリンタで実行、終了するときは exit
$ perl6
To exit type 'exit' or '^D'
> say "hello perl6";
hello perl6
> exit

# ファイルを読み込ませて実行

# テキストファイルを作る
# sandbox ディレクトリとは、何かを試しに作るとき保存しておくディレクトリ
# サフィックスは perl5 とわけたいので .p6 とつけておくことにする
$ echo 'say "hello perl6";' > ~/sandbox/hello_test.p6

# 読み込んで実行
$ perl6 ~/sandbox/hello_test.p6
hello perl6
```

# SEE ALSO

関連項目

- <https://perl6.org/> Perl 6 Programming Language
- <http://rakudo.org/> Rakudo Perl 6
- <http://ja.perl6intro.com/> Perl6入門 (初心者向けに日本語で解説)
- <https://docs.perl6.org/> Perl 6 Documentation - perl6 ドキュメント一式 (日本語翻訳はまだない)
- <https://docs.perl6.org/programs/00-running> perl6 - the Rakudo Perl 6 Compiler
