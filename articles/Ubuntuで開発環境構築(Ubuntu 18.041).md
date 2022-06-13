---
title: Ubuntuで開発環境構築(Ubuntu 18.041)
tags: Ubuntu 環境構築 Linux ubuntu18.041
author: koutarn
slide: false
---
# Ubuntuで環境構築
開発環境を改めてUbuntuで構築しなおしたので書きました。
Ubuntu便利だよ、Ubuntu。気づいた事があったら修正します。

インストールした奴:[Ubuntu 18.04 LTS 日本語 Remix](https://www.ubuntulinux.jp/News/ubuntu1804-ja-remix)

## Install

### ドライバ
Nvidiaドライバです。radeon派は調べてください

```
Nvidia
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo apt-get install nvidia-xxx
```

### アプリ
```
GoogleChrome (ブラウザ)
Dropbox(データ共有)
InkDrop(情報共有)
InkScape(ドローツール)
Draw.io(図作成)
Line(連絡用)
tweaks(設定用) sudo apt install gnome-tweaks
ulauncher(ランチャーソフト) sudo apt install uluncher
w3m(CLIウェブブラウザ) sudo apt install w3m
unrar(.rarファイルを解凍)sudo apt install unrar
```

### 開発環境
```
guake(ターミナル。) sudo apt install guake  
git(git) sudo apt install git -y
gitkraken(gitクライアント)
VSCode (エディタ) 
FileZila(ファイル転送)
Docker(仮想環境構築)
vagrant(仮想環境構築) apt install vagrant 
virtualbox (仮想環境構築) apt install virtualbox
zsh(シェル)apt-get install zsh && chsh -s /usr/local/bin/zsh
sl(ジョーク) apt install sl
fish(シェル)
sudo apt-add-repository ppa:fish-shell/release-2
sudo apt-get update
sudo apt-get install fish

fzf(曖昧検索)
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install

nkf(文字コード変換) sudo apt install nkf
ctags(vimでタグジャンプ用) sudo apt install ctags
tmux(端末多重化) sudo apt install tmux
silbersearcher(検索高速化)sudo apt install silversearcher-ag
xsel(ターミナルでコピペ出来るようにする) sudo apt install xsel
shellcheck(シェルの構文解析) apt-get install shellcheck
```

### PowerLine
[PowerLineを入れるまで箇条書きしたよ](https://qiita.com/koutasan/items/aed914f0ab5fea9ba578)

### Ruby

```
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(rbenv init -)"' >> ~/.zshrc
exec $SHELL
git clone git://github.com/sstephenson/ruby-build.git
cd ruby-build
sudo ./install.sh
sudo apt install libssl1.0-dev
rbenv install -l
rubenv install 2.5.1
rubenv global 2.5.1
rbenv rehash
gem install bundler
gem update --system
```

### Rails
```
gem query -ra -n "^rails$" 
mkdir rails_v5.2.0 # お好きな名前で
cd rails_v5.2.0
bundle init # カレントディレクトリにGemfileを作成
echo 'gem "rails", "5.2.0"' >> Gemfile # ※ 任意のバージョンを指定
bundle install --path vendor/bundle # ./vendor/bundleにRails関連のGemが入る
sudo apt install libsqlite3-dev #sqliteの依存関係を無くす
```  

### docker  
前提ファイルのインストール

```
sudo apt install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```

GDP公開鍵のインストール  

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -  
```
  
aptリポジトリの設定
  
```
udo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

docker-ceのインストール  

```
sudo apt update
sudo apt install -y docker-ce
```


## Settings

### update
```
sudo apt update
sudo apt -y upgrade
sudo apt -y dist-upgrade
``````
### 日本語訳を追加
追加しないと日本語約が完全ではないようなので追加します。

```
sudo add-apt-repository -y -n ppa:sicklylife/ppa
sudo apt update
sudo apt upgrade
```
### ディレクトリを英語表記
cdする時に日本語表記だと面倒なので変更しておきます。

```
LANG=C xdg-usesr-dirs-gtk-update
```
### mozkのアップデート
アップデートしておいたほうがいいらしい・・

```
sudo add-apt-repository -y -n ppa:sicklylife/mozc
sudo apt update
sudo apt dist-upgrade
```
### shをダブルクリックで開けるように
デスクトップのものをダブルクリックで開きたい時ってありますよね？

```
ファイル→設定→動作→どうするか確認する
```

### デフォルトのエディタを変更にする
emacs派とvim派の宗教戦争ってきのこたけのこ問題みたいですよね。

```
update-alternatives --config editor
```
### dotfilesを入れる
dotfilesとは設定ファイル群の事です。
githubからdotfileリポジトリをcloneしてください。  
[github.com koutakun/dotfile](https://github.com/koutakun/dotfile)

### アニメーションの無効化
```
gsettings set org.gnome.desktop.interface enable-animations false 
```
### キーリングの変更
Passwords and Keysにて空のパスワードを設定する。
