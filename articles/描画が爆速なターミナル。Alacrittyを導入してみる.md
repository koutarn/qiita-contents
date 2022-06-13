---
title: 描画が爆速なターミナル。Alacrittyを導入してみる
tags: ターミナル alacritty
author: koutarn
slide: false
---
# Alacrittyさいつよ
Alacrittyとは、GPUで動作させている事により
描画がとても高速なターミナルです。

描画が早いのは勿論、ファイル一つで設定を管理出来ることや
日本語対応もしている所が素晴らしい所になります。

※環境はubuntuを想定しています。

## Rustをインストール
```
sudo curl https://sh.rustup.rs -sSf | sh
```

## cargoをインストール

```
cargo install cargo-deb
```


## 依存関係のあるものを入れる

```shell
apt-get install cmake libfreetype6-dev libfontconfig1-dev xclip
```
## alacrittyを入れる  

```shell
git clone https://github.com/jwilm/alacritty.git
cd alacritty
cargo build --release
```

## コピーしてコマンドとアプリケーションで認識するようにする  

```shell
sudo cp trget/release/alacritty /usr/local/bin
sudo cp alacritty.desktop ~/.local/share/applications
```

## shellで補完が効くようにする  

```shell
--------- On Bash Shell ---------
sudo cp alacritty-completions.bash  ~/.alacritty
echo "source ~/.alacritty" >> ~/.bashrc

--------- On ZSH Shell ---------
sudo cp alacritty-completions.zsh /usr/share/zsh/functions/Completion/X/_alacritty

--------- On FISH Shell ---------
sudo cp alacritty-completions.fish /usr/share/fish/vendor_completions.d/alacritty.fish
```

## デフォルトのシェルに変更する

```shell
gsettings set org.gnome.desktop.default-applications.terminal exec /usr/local/bin/alacritty 
gsettings set org.gnome.desktop.default-applications.terminal exec-arg "-x"
```

・引用元
[Alacritty – A Fastest Terminal Emulator for Linux](https://www.tecmint.com/alacritty-fastest-terminal-emulator-for-linux/)
