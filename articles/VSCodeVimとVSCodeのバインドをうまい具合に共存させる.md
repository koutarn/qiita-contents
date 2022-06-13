---
title: VSCodeVimとVSCodeのバインドをうまい具合に共存させる
tags: VSCode VisualStudioCode Vim VsVim 設定
author: koutarn
slide: false
---
##はじめに

みなさん、こんにちは。
VSCodeを最近使い始めたのですが、一点だけ凄く困っている点がありました。
vscodevim(以下VSVim)でキーのバインドを行うと、
エディタがフォーカス状態になっていないと、キーが上手く動いてくれないのです。
例えば、pluginを表示している画面やmarkdownを表示している画面に
フォーカスが移動してしまうとエディタを移動したくても移動出来ず凄く困ります。

![スクリーンショット 2019-04-24 00.27.36.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/253308/87cf6684-6bec-1289-2010-6068572db787.png)
こいつが消せない・・  

```json:settings.json
"vim.leader":"<space>",//Map Leaderの設定
{"before": ["<Leader>", "x"],"commands": [":q!"]},		  　//tabを閉じる
{"before": ["<Leader>", "u"],"after"   : ["g","t"]},		//tab移動
{"before": ["<Leader>", "y"],"after"   : ["g","T"]},		//tab移動
```
このバインドが使えない

また、逆にVSCode上で設定するとprefixになるキーを押した状態で問答無用で
キーの入力待ちになりVSVimで動かしたいキーが上手く動いてくれませんでした。

苦肉の策として、VSVimとVSCodeで同じ動作をするキーを分けてみました。
VSCodeはCtrl+Space,VSVimはSpaceをprefixにして分けたのですが、大変使いづらい代物になってしまいました。

```json:settings.json

"vim.leader": "<space>",//Map Leaderの設定
{"before": ["<Leader>", "x"],"commands": [":q!"]},		   //tabを閉じる
{"before": ["<Leader>", "u"],"after"   : ["g","t"]},		//tab移動
{"before": ["<Leader>", "y"],"after"   : ["g","T"]},		//tab移動
```

```json:keybindings.json
//editorじゃない奴(markdownのprviewとか)はこれで消す
{
	"key": "space+ctrl ctrl+x",
	"command": "workbench.action.closeActiveEditor",
},
//editorじゃない奴(markdownのprviewとか)はこれでtab移動
{
	"key": "space+ctrl ctrl+u",
	"command": "workbench.action.nextEditor",
},
{
	"key": "space+ctrl ctrl+y",
	"command": "workbench.action.previousEditor",
},
```

##Whenを加えて、VSCodeVimとVSCodeを共存させる
そこで一捻り加えて見ました、全然使いこなせていなかった
VSCodeのwhenを入れてエディタ上ではVimコマンドを有効にし
エディタのテキストにフォーカスが行っていない時は
VSCodeのコマンドを有効にするようにしました。

```json:settings.json
    //editorじゃない奴(markdownのprviewとか)はこれで消す
    {
        "key": "space x",
        "command": "workbench.action.closeActiveEditor",
        "when":"!editorTextFocus && !editorHasSelection && !editorHasMultipleSelections"
    },
    //editorじゃない奴(markdownのprviewとか)はこれでtab移動
    {
        "key": "space u",
        "command": "workbench.action.nextEditor",
        "when":"!editorTextFocus && !editorHasSelection && !editorHasMultipleSelections"
    },
    {
        "key": "space y",
        "command": "workbench.action.previousEditor",
        "when":"!editorTextFocus && !editorHasSelection && !editorHasMultipleSelections"
    },
```

```
!editorTextFocus             //editor内のテキストにフォーカスがない 
!editorHasSelection          //editor内のテキストを選択していない   
!editorHasMultipleSelections //editorを複数選択していないとき
```
whenで設定した条件を満たした時に、VSCodeのコマンドが有効になります。

これにより、同じバインドで無事に操作する事が出来るようになりました。
これを応用して、コマンドパレットを呼びやすくしたり、
エクスプローラーでの操作も簡略化出来たので
かなり小指に優しいキーバインドを実現出来ました。
この記事が同じ問題で悩んでいる人の助けになれば幸いです。


参考
[Visual Studio Code (VS Code) Docsの日本語訳 キーバインディング #when節のコンテキスト](https://vscode-doc-jp.github.io/docs/getstarted/keybindings.html#when%E7%AF%80%E3%81%AE%E3%82%B3%E3%83%B3%E3%83%86%E3%82%AD%E3%82%B9%E3%83%88)

