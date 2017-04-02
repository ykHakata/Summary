# NAME

man - UNIX オンラインマニュアルのまとめ

# DESCRIPTION

## オンラインマニュアルの見方

    1990年に初版が発行されている「たのしいUNIX」という本にはこう書いてあります。

    「... わからないことがあったとき、周りの人や先輩に訊くのも１つの方法ですが、
    まずはオンライン・マニュアルを読んでください。きっと、回答が得られるはずです。」

    1990年とは今から27年ほど前の話になりますが、今でもこの考え方は有効で、
    スマートフォンからインターネットへの常時接続が当たり前になり、
    ほとんどの情報が google 検索で得られるようになった今でも、
    オンライン・マニュアルで得られる情報は確実性が高いです。

    コマンドの使い方がわからない時、いたずらにネットで検索して、どこかのまとめ記事に
    書いてあるように操作してみるが、なぜか微妙に挙動がことなり、
    結局オンライン・マニュアルを読んで調べたということはよくある話です。

    本質的な話をすると一番信頼性の高い説明書は公式ドキュメントで、
    公式ドキュメントで調べるのが間違いないということです。

    とはいえ、プログラミングの世界の公式ドキュメントは大抵英語で記述されているおり
    その英語の内容もプログラミングをやっている人ならではの言い回しが存在するので、
    プログラミングの素養がない人にとっては敷居が高いものになっています。

    プログラミングの初級者と中級者をどの基準で分けるのかといえば、
    公式ドキュメントについてこれるかで分けるのが一つの基準としては良いでしょう。

    まずは最低限の man ページの操作と読み方を覚えて、次は、簡単なスクリプトを
    こしらえた後、しっかりとしたドキュメントを自分で作ってみるとオンライン・マニュアルも
    自然と読めるようになるでしょう。

### ターミナルにて

```bash
# man コマンドの man をみてみる
$ man man
# man(1)
#
# NAME
#        man - format and display the on-line manual pages
#
# SYNOPSIS
# ... 省略

# 表示されている状態で h を入力

#                   SUMMARY OF LESS COMMANDS
#
#      Commands marked with * may be preceded by a number, N.
#      Notes in parentheses indicate the behavior if N is given.
#      A key preceded by a caret indicates the Ctrl key; thus ^K is ctrl-K.
# ... 省略

# 画面の操作の仕方の説明があらわれる
# 説明によれば
# j - 次の行
# k - 前の行
# f - 次の画面
# b - 前の画面
# j - 次の行
# q - 終了
# この操作性は vi にそっくり

# いったん man を q を押して終了して次は -f オプションをつけて実行
$ man -f man
# mysqlman(1)              - default man page for mysql
# IO::Socket::SSL::Intercept(3pm) - -- SSL interception (man in the middle)
# Pod::Man(3pm)            - Convert POD data to formatted *roff input
# Pod::Perldoc::ToMan(3pm) - let Perldoc render Pod as man pages
# groff_man(7)             - groff `man' macros to support generation of man pages
# groffer(1)               - display groff files and man~pages on X and tty
# man(1)                   - format and display the on-line manual pages
# man.conf(5)              - configuration data for man
# zshall(1)                - the Z shell meta-man page

# 私の端末ではこれだけ出力された、これは登録されている man コマンド名と利用目的を表示
# man(1) の数字はどういうことか？
$ man 1 man

# man man と同じ内容が出力される、次は
$ man 5 man.conf

# このあたりの入力方法も man man で説明されているが
# これ以上詳細な説明は man の日本語訳になってしまうので、
# man の大事な部分を次に解説、詳細は自身で調べるのが一番理解が早いと思われる。
```

### 項目の説明

NAME (ネーム)

    直訳 - 名前
    意味 - コマンドの名前とその使用目的

SYNOPSIS (シノプシス)

    直訳 - 概要
    意味 - コマンドの入力形式

    例: cat [-benstuv] [file ...]

    ファイル名、オプション、文字列の引数などが列挙
    [] で囲まれている部分は省略が可能
    ... と表記がある部分は、その直前で記述されている引数を繰り返し指定できる

    $ cat file1
    こういう記述や

    $ cat file1 file2 > file3
    こういう記述ができるということになる

DESCRIPTION

    直訳 - 説明
    意味 - コマンドに関する詳細な説明

    例: cat の場合

    The cat utility reads files sequentially, ...
    (このように英語で説明文や)

    The options are as follows:

    -b      Number the non-blank output lines, starting at 1.
    ...
    (オプションについての説明などが記述されている)

FILES

    直訳 - ファイル
    意味 - コマンドに関連したファイル名が絶対パスで記されている



SEE ALSO

BUGS

