# NAME

text editor - テキストエディッタ設定

# Vintage モード

- <http://www.sublimetext.com/docs/3/vintage.html> - Vintage Mode (公式ドキュメント)

ESC - コマンドモード

移動

j - 下
k - 上
l - 右
h - 左

カーソルの移動(画面スクロールせず)
H - 画面上
M - 画面中央
L - 画面下

カーソルの移動(画面スクロールする)
gg - 一番上
G - 一番下

w - 単語単位で移動(. や / など区切り文字)
W - 単語単位で移動(スペースや改行文字)
b - 単語単位で逆に移動(. や / など区切り文字)
B - 単語単位で逆に移動(スペースや改行文字)


r - 1文字だけ変更(例: fttp -> f のところで rh で http に変更)
x - 1文字だけ削除

yy - 1行コピー

p - 貼り付け

dd - 1行削除
D - カーソル位置から行末まで削除

u - やり直し(1回前にもどる)

J - 行の連結


a - 入力モード(追加)
i - 入力モード(挿入)



v - ビジュアルモード

# EDITOR - SublimeText3

## ショートカットキー

ショートカットキーに関するドキュメント

    http://docs.sublimetext.info/en/latest/reference/keyboard_shortcuts_osx.html

キー

    control         ⌃
    shift           ⇧
    option / alt    ⌥
    command         ⌘

編集

    貼り付けとインデント　　　　⇧⌘V
    履歴から貼り付け　　　　　　⌥⌘V
    インデント　　　　　　　　　⌘[
    逆インデント　　　　　　　　⌘]
    前行と入れ替え　　　　　　　⌃⌘↑
    次行と入れ替え　　　　　　　⌃⌘↓
    行の複製挿入　　　　　　　　⇧⌘D
    行を削除　　　　　　　　　　⌃⇧K
    行の結合　　　　　　　　　　⌘J
    行コメントをトグル　　　　　⌘/
    行末まで削除　　　　　　　　⌃K
    大文字　　　　　　　　　　　⌘K, ⌘U
    小文字　　　　　　　　　　　⌘K, ⌘L

選択

    選択範囲を行単位分割　　　　⇧⌘L
    選択範囲拡張「行単位」　　　⌘L
    選択範囲拡張「単語単位」　　⌘D

検索

    選択条件として使用　　　　　⌘E
    次を検索　　　　　　　　　　⌘G
    前を検索　　　　　　　　　　⇧⌘G

表示

    サイドバー表示非／表示　　　⌘K, ⌘B
    画面分割解除　　　　　　　　⌥⌘1
    画面分割２列　　　　　　　　⌥⌘2

移動

    移動「汎用」　　　　　　　　⌘P
    移動「シンボル」　　　　　　⌘R
    移動「プロジェクト」　　　　⇧⌘R
    移動「行番号」　　　　　　　⌘G
    次のファイル　　　　　　　　⇧⌘] or ⌥⌘→
    前のファイル　　　　　　　　⇧⌘[ or ⌥⌘←
    選択範囲を画面中央に　　　　⌘L

## SublimeText3 インストールから設定

### 本体のインストール

    http://www.sublimetext.com/3

公式ドキュメント

    http://www.sublimetext.com/docs/3/
    http://docs.sublimetext.info/en/latest/index.html

### コマンドラインからの起動設定

参考

    http://www.sublimetext.com/docs/3/osx_command_line.html

USERNAME のところは自身のホームディレクトリのユーザー名に置き換えてください

自身のホームディレクトリに bin ディレクトリがない場合は作成

    mkdir ~/bin

シンボリックリンク作成

    ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl ~/bin/subl

コマンドがつかえるようにパスを通す

    echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bash_profile

パスが通った事の確認

    which subl
    /Users/USERNAME/bin/subl

.bash_profile については [SHELL](SHELL) ページ参照

各種ファイルの場所

    ~/Library/Application Support/Sublime Text 3

### パッケージコントロールの設定

参考

    https://packagecontrol.io

パッケージコントロールをインストール

    https://packagecontrol.io/installation

SublimeText 操作

    View > Show Console (control + shift + @)

    Consoleをオープン

    installationページのSUBLIME TEXT 3 タブのスクリプトを貼り付け実行(enter)

    sublime text3を終了、再度立ち上げ

### パッケージのインストール

japanize (メニューバーの日本語化)

参考

    https://packagecontrol.io/packages/Japanize
    http://gadget-drawer.net/2013/12/mac-sublime-text-3-japanese/

SublimeText 操作

    Tools > Command Palette (shift + command + P)
    入力フォームに install と入力
    Packege Control: Install Package をクリック
    japanize と入力
    Japanize をクリック

一部のメニューが日本語、残り全てを日本語化

ターミナルにて下記入力

    mkdir ~/Library/Application\ Support/Sublime\ Text\ 3/Packages/Default

    cd ~/Library/Application\ Support/Sublime\ Text\ 3/Packages/Japanize/

    cp *.jp ../Default/

    cp Main.sublime-menu ../User/

    cd ../Default/

    mv Main.sublime-menu.jp Main.sublime-menu

おすすめのパッケージ

Markdown関連

参考

    http://webmem.hatenablog.com/entry/sublime-text-markdown

Omni​Markup​Previewer

    https://packagecontrol.io/packages/OmniMarkupPreviewer

Monokai Extended

    https://packagecontrol.io/packages/Monokai%20Extended

Markdown Extended

    https://packagecontrol.io/packages/Markdown%20Extended

Perl関連

Modern​Perl

    https://packagecontrol.io/packages/ModernPerl

Mojolicious

    https://packagecontrol.io/packages/Mojolicious

Perl​Tidy

    https://packagecontrol.io/packages/PerlTidy

PerlTidyを有効にするには cpanm でモジュールインストール

インストール方法は [PERL](PERL) ページ参照

有効にするには設定ファイルに下記を追記 (例:

    "perltidy_cmd": ["/Users/USERNAME/.plenv/shims/perltidy"],

PerlTidyを実行するには行を選択して

    Control + Shift + T

設定ファイルを変更

設定ファイルを開く

上部メニューバー

    基本設定 > 基本設定 - ユーザー

もしくは

    Sublime Text > Preference > 基本設定 - ユーザー

もしくはコマンドライン

    subl ~/Library/Application\ Support/Sublime\ Text\ 3/Packages/User/Preferences.sublime-settings

設定例

表示

    "draw_white_space": "all",           // 空白、タブ文字を表示
    "highlight_modified_tabs": true,     // 未保存のタブタイトルを強調
    "highlight_line": true,              // 選択行を強調
    "show_encoding": true,               // 右下のステータスにエンコーディング表示
    "scroll_past_end": true,             // 最終行のスクロールを可能
    "word_wrap": false,                  // 行の自動折り返しをしない
    "rulers": [78],                      // ルーラー表示(78文字)

編集

    "ensure_newline_at_eof_on_save": true,       // 保存時に自動的に行末に改行
    "translate_tabs_to_spaces": true,            // TABキー入力をスペース入力に
    "trim_trailing_white_space_on_save": true,   // 行末の空白を削除

記入例

    {
        "color_scheme": "Packages/Monokai Extended/Monokai Extended.tmTheme",
        "draw_white_space": "all",
        "ensure_newline_at_eof_on_save": true,
        "font_face": "Ricty Diminished Regular",
        "font_size": 16,
        "highlight_line": true,
        "highlight_modified_tabs": true,
        "ignored_packages":
        [
            "Vintage"
        ],
        "perltidy_cmd":
        [
            "/Users/USERNAME/.plenv/shims/perltidy"
        ],
        "rulers":
        [
            78
        ],
        "scroll_past_end": true,
        "show_encoding": true,
        "translate_tabs_to_spaces": true,
        "trim_trailing_white_space_on_save": true,
        "word_wrap": false
    }

プロジェクトの設定方法

参考

    http://www.buildinsider.net/small/sublimetext/02
    https://www.sublimetext.com/docs/3/projects.html

例えば sandbox ディレクトリにプロジェクトファイルを作成し

プロジェクト名を sandbox とする場合

    cd ~/sandbox

    touch sandbox.sublime-project

ファイルをコマンドライン subl でオープンしてみる

    subl sandbox.sublime-project

記入例

```
{
    "folders":
    [
        {
            "follow_symlinks": true,
            "path": "."
        }
    ]
}
```
