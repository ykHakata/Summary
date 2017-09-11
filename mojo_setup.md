# NAME

```
mojo_setup - Mojolicious の基本的なセットアップ
```

# SYNOPSIS

## 下記資料の読み方

__例: 以下の事例は各項目を置き換え__

```
アプリケーション名: HackerzLab
```

## アプリケーションスタート

ローカル環境、ターミナルから

```
(WEBフレームワークを起動 development モード)
$ carton exec -- morbo script/hackerz_lab
```

ローカル環境、web ブラウザから

```
http://localhost:3000/
```

終了するときはターミナルで control + c を入力

# DESCRIPTION

## 大まかなセットアップの流れ

1. アプリケーション一式を設置するディレクトリ用意
1. github からリポジトリを git clone
1. Perl のバージョンを決定
1. Mojolicious のインストール
1. アプリケーションの雛形作成
1. ディレクトリ内を再配置
1. アプリ実行確認
1. git で管理しないものを決める
1. github へ push しておく

## 事前に済ましておくこと

- 基本的な git と github の概要と操作
- github でのアカウント作成
- github でアプリケーション用のリポジトリ作成
- ローカル環境での Perl5 セットアップ
    - [perl5_install](perl5_install.md) - perl5 ローカル環境での設定手順を参考

# SETUP

## アプリケーション一式を設置するディレクトリ用意

任意のディレクトリで問題ないが今回は

```
~/github/HackerzLab/
```

git clone をすると自動的にディレクト作成するので

```
~/github/
```

にいることを前提で下記をすすめる

## github からリポジトリを git clone

例: HackerzLab の場合

1. https://github.com/ykHakata/HackerzLab アクセス
    1. HackerzLab はすでにいろんなものが構築されているのであくまで例として
1. 例のサイトの明るい緑のボタンで、 [clone or download] のボタンをクリック
1. url が出現するので任意のディレクトリで実行

```
(git clone クリップボードにコピーしたURL)

(ホームディレクトリ配下の github ディレクトリが存在する前提)
$ cd ~/github/
$ git clone https://github.com/ykHakata/HackerzLab.git
```

HackerzLab というディレクトリが作成されるので、ディレクトリの中へ移動

```
$ cd HackerzLab
```

## Perl のバージョンを決定

plenv の使い方は事前に把握しておく

```
(今回は 5.26.0)
$ plenv local 5.26.0

(もしくは直接ファイルを作って書き込む)
$ echo '5.26.0' > .perl-version
```

## Mojolicious インストール

### carton を使用してインストール

carton の使い方は事前に把握しておく

cpanfileの作成

```
(cpanfile を作成し読み込み実行インストール)

$ touch cpanfile
$ cat cpanfile
requires 'Mojolicious', '== 7.45';
```

cpanfileの書き方(例)

```
requires 'Module::Name';
requires 'Module::Name', 'MINIMUM VERSION';
requires 'Module::Name', '>= MINIMUM, < MAXIMUM';
requires 'Module::Name', '== SPECIFIC VERSION';
```

carton インストール実行

```
$ carton install
```

## アプリケーションの雛形作成

carton exec をつけなければいけない事に注意

```
(アプリケーション名は慣例的にキャメルスタイル)
$ carton exec mojo generate app HackerzLab
```

HackerzLab が hackerz_lab の用に変化する事に注意

## ディレクトリ内を再配置

HackerzLab 内のファイル一式を移動する

```
$ mv hackerz_lab/* .
```

空ディレクトリを削除

```
$ rmdir hackerz_lab
```

ドキュメント保存用のディレクトリを作成

```
$ mkdir doc
```

## アプリ実行確認

ローカル環境で実行確認

```
(WEBフレームワークを起動 development モード)
$ carton exec -- morbo script/hackerz_lab
```

web ブラウザで `http://localhost:3000/` にアクセス

```
Welcome to the Mojolicious real-time web framework!
...

と表示されれば成功
```

## git で管理しないものを決める

### local ディレクトリ

環境によってインストールされる中身が異なるので git 管理しない

```
$ echo local/ >> .gitignore
```

### log ディレクトリ

環境によって作成される内容がことなるので git 管理しない

```
$ echo log/ >> .gitignore
```

## github へ push しておく

```
(以下3ステップで)
$ git add .
$ git commit -m 'add mojo'
$ git push origin master
```

githut サイトで確認

# SEE ALSO
