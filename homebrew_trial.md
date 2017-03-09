# NAME

homebrew_trial - homebrew 使い方簡単まとめ

# DESCRIPTION

Homebrew (ホームブリュー) は、
MacのUNIXツールのインストールを
単純化するパッケージ管理システムのひとつである。
古くは Fink そして MacPorts と同様の目的と機能を備えている。

MacPortsはスーパーユーザで実行、Homebrewは一般ユーザで実行
MacPortsではソフトウェアは/opt/localにインストール、Homebrewは標準では/usr/localを利用

詳細は[公式ページ](http://brew.sh/index_ja.html)参照

# INSTALL

具体的なインストールの手順

先ほどの説明のように Mac の為のものであるため、下記の手順は Mac のみ対応

```bash
# sudo 権限になる必要なく実行可能、詳細は公式ページ参照
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

もしこのコマンドでインストールに失敗する場合は下記の記事を参考にする

<https://github.com/Homebrew/brew/blob/master/docs/Installation.md> ドキュメント インストール

こちらの記事によれば `Command Line Tools` が必要と書いてある。

具体的には homebrew は ruby で書かれており、更新は git で管理されている

brew を使ってインストールする UNIX ツールには gcc などを必要とする場合があるので Xcode の Command Line Tools 必須である

# USAGE

最低限の使い方

```bash
# 確かな情報は公式ドキュメント
$ man brew

BREW(1)  ...

NAME
       brew - The missing ...

SYNOPSIS
       brew --version
       brew command [--verbose|-v] [options] [formula] ...
# 省略 ...

# 今の brew のバージョンを確認
$ brew --version
Homebrew 0.9.5 (git revision e72bc; last commit 2016-01-13)

# 最新の状態に (基本的には常に最新のバージョンに更新しておくのが良い)
$ brew update
Error: The /usr/local directory is not writable.

# 省略 ...

You should probably change the ownership and permissions of /usr/local
back to your user account.
  sudo chown -R $(whoami):admin /usr/local

# MacOS の El Capitan から権限まわりが変更になっているので、このようになることがあるが指示通り対応
$ sudo chown -R $(whoami):admin /usr/local
Password:

# もう一度 update
$ brew update
yk-iMac:Summary yk$ brew update
Updated Homebrew from e72bcfa to 5a9e19f.
Updated 1 tap (homebrew/dupes).
# 省略 ...

# バージョンを確認
$ brew --version
Homebrew >1.1.0 ...
Homebrew/homebrew-core ...

# 潜在的な問題がないか確認
$ brew doctor

# 下記の出力された内容を翻訳すると、利用上は問題はない様子

# Please note that these warnings are just used to help the Homebrew maintainers
# with debugging if you file an issue. If everything you use Homebrew for is
# working fine: please don't worry and just ignore them. Thanks!

# Warning: Anaconda is known to frequently break Homebrew builds, including Vim and
# MacVim, due to bundling many duplicates of system and Homebrew-available
# tools.

# ...

# たくさん出力されるが省略 ...

# すでに brew でイントール済みの内容を確認
$ brew list
boost           icu4c           sdl
cairo           jpeg            sdl_image
cmake           libffi          sdl_mixer
czmq            libpng          sdl_ttf
fontconfig      libtiff         sqlite
fontforge       libtool         tidy-html5
freetype        mysql           tig
gettext         openssl         tree
git         pango           webp
glib            pixman          zeromq
gobject-introspection   pkg-config
harfbuzz        readline

# 個別に詳細に確認したい場合
brew list mysql

```


# SEE ALSO

関連項目

- <http://brew.sh/index_ja.html> Homebrew (日本語)
- [summary](README.md) - summary - まとめ記事一覧
