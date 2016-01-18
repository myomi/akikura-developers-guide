# 2-3. （Macユーザのみ）Homebrew
[Homebrew](http://brew.sh/index_ja.html)は、Mac用のパッケージマネージャです。これを導入すると、以降のツールインストール作業が楽になります。

## インストール
ターミナルを起動し、以下のコマンドを実行します。

```sh
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

インストールには時間がかかりますので、気長に待ちましょう。

## パッケージ情報のアップデート
以下のコマンドを実行し、Homebrewが管理しているパッケージ情報を最新化します。

```sh
$ brew doctor
$ brew update
```

brew updateコマンドは、これ以降も定期的に実行するようにしましょう。
アップデートしておかないと、インストールするツールのバージョンが古い場合があります。