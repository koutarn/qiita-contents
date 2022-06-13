---
title: base16を入れて最高にカッコいいターミナル環境を作る
tags: base16 Vim Terminal
author: koutarn
slide: false
---
# base16って最高
base16を知っていますか？
これを入れると、誰でも簡単にターミナルとvimの配色を
***コマンド一つで手軽に変更して利用できます。***今までのターミナルの設定から変更したり、
.vimrcにcolorschemeを書いていた煩わしい作業とこれでおさらば出来ます！

![image.png](https://qiita-image-store.s3.amazonaws.com/0/253308/abf5dc55-d6f1-b99c-ed15-2168f13b1042.png)
***solarized-dark***
青い感じが結構クール。vimで人気な配色です。目に優しそう。

![image.png](https://qiita-image-store.s3.amazonaws.com/0/253308/2361a113-c7e2-0cb2-ee0e-a6fb568f7b54.png)
***monokai***
molokaiの系譜である黒ベースの配色。僕はこいつが一番お薦めです。


## インストール

### shell

```shell
git clone https://github.com/chriskempson/base16-shell.git ~/.config/base16-shell
```
gitからcloneして.configの下に展開する。

```shell
# Base16 Shell
BASE16_SHELL="$HOME/.config/base16-shell/"
[ -n "$PS1" ] && \
[ -s "$BASE16_SHELL/profile_helper.sh" ] && \
eval "$("$BASE16_SHELL/profile_helper.sh")"
```
bashとzshのどちらかを利用しているなら、.bashrcまたは.zshrcに入力。


```shell
# Base16 Shell
if status --is-interactive
    set BASE16_SHELL "$HOME/.config/base16-shell/"
    source "$BASE16_SHELL/profile_helper.fish"
end
```
fishを使っているなら.config/fish/config.fishに入力。

### vim 

```
Plug 'chriskempson/base16-vim'
```
vim plugを利用しているなら、これを入力して:PlugInstall
そうじゃない人は[ここ](https://github.com/chriskempson/base16-vim)から探してください

```vim
if filereadable(expand("~/.vimrc_background"))
  let base16colorspace=256
  source ~/.vimrc_background
endif
```
あとはこいつを.vimrcに書き込むとシェルの変更と同期してくれます。

## 使い方

```shell
base16-solarized-dark #solarizedのクールな色味に!
base16-monokai #monokaiのリッチな色味に！
```

shellでbase16-solarized-darkやbase16-monokaiと入力するだけです。

![image.png](https://qiita-image-store.s3.amazonaws.com/0/253308/07013fb9-51e5-86e1-6d8b-ddf5ae9c7f47.png)
base16 まで入力してtabを押すと色々候補が出て幸せになれます。
色々使ってみて、ぜひクールなターミナルライフを送ってください！
