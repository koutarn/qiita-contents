---
title: 3分でラクラク簡単。PowerLineをいれるまで
tags: powerline 初心者 dotfiles
author: koutarn
slide: false
---
## powerline
![image.png](https://qiita-image-store.s3.amazonaws.com/0/253308/2c4261b9-a93f-c332-fdb8-804a0ed8dff2.png)
こいつをいれると見栄えがよくなるぞ！

```shell

sudo apt install -y python-pip
pip3 install powerline-status
```
### フォント

powerline用にフォントを入れないと、悲しいことにズレまくります。
Cicaフォントを入れると、powerline用の設定だったりをしてくれているので、
面倒な手間がないで凄く良い感じ。入れない場合は、こっちを参考にするといい感じです。
[powerline のフォントのズレを修正する方法](http://oooooooo.hatenablog.com/entry/2013/05/01/151455)

[Cicaをダウンロード](https://tmnm.tech/2017/10/11/vim-setting-with-cica/)

```shell

sudo cp -r Cicaファイル /usr/share/fonts/truetype/Cicaファイル
fc-cache -fv
```



### tmux
みんな大好きtmuxは.tmux.confに書き込み  

```
run-shell "powerline-daemon -q"
source ".local/lib/python<自分のバージョン>/site-packages/powerline/bindings/tmux/powerline.conf"
```

## vim
純正Powerlineの設定ならこれ 

```vim
set laststatus=2
set showtabline=2
set t_Co=256
python3 from powerline.vim import setup as powerline_setup
python3 powerline_setup()
python3 del powerline_setup
```
### lightline
または、lightlineを入れる。軽いのでこっちのほうがオススメ。

vim plugでインストール

```Vim
Plug 'itchyny/lightline.vim'
```
色々設定。　　[Cica作者さんの丸コピです。](https://tmnm.tech/2017/10/11/vim-setting-with-cica/)

```Vim
let g:lightline = {
            \ 'colorscheme': 'molokai',
            \ 'active': {
            \   'left': [ [ 'mode', 'paste' ],
            \             [ 'fugitive', 'filename' ] ]
            \ },
            \ 'component_function': {
            \   'fugitive': 'LightLineFugitive',
            \   'readonly': 'LightLineReadonly',
            \   'modified': 'LightLineModified',
            \   'filename': 'LightLineFilename',
            \   'filetype': 'LightLineFiletype',
            \   'fileformat': 'LightLineFileformat',
            \ },
            \ 'separator': { 'left': '', 'right': '' },
            \ 'subseparator': { 'left': '', 'right': '' }
            \ }

function! LightLineModified()
    if &filetype == "help"
        return ""
    elseif &modified
        return "+"
    elseif &modifiable
        return ""
    else
        return ""
    endif
endfunction

function! LightLineReadonly()
    if &filetype == "help"
        return ""
    elseif &readonly
        return ""
    else
        return ""
    endif
endfunction

function! LightLineFugitive()
    if exists("*fugitive#head")
        let _ = fugitive#head()
        return strlen(_) ? ''._ : ''
    endif
    return ''
endfunction

function! LightLineFilename()
    return ('' != LightLineReadonly() ? LightLineReadonly() . ' ' : '') .
                \ ('' != expand('%:t') ? expand('%:t') : '[No Name]') .
                \ ('' != LightLineModified() ? ' ' . LightLineModified() : '')
endfunction

function! LightLineFiletype()
  return winwidth(0) > 70 ? (strlen(&filetype) ? &filetype . ' ' . WebDevIconsGetFileTypeSymbol() : 'no ft') : ''
endfunction

function! LightLineFileformat()
  return winwidth(0) > 70 ? (&fileformat . ' ' . WebDevIconsGetFileFormatSymbol()) : ''
endfunction
```

