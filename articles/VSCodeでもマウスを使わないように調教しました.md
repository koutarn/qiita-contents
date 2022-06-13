---
title: VSCodeでもマウスを使わないように調教しました
tags: Vim VSCode ポエム 設定
author: koutarn
slide: false
---
# はじめに
かれこれ半年前、VimからVS Codeに移行しました。人に触らせると「マウス使えないから操作出来ねぇ」と言われるし、pluginをGitHubで漁るのに疲弊したからです。
移行してまず思ったことは、「これ、キーボードで操作完結させるのめんどくさいな～」でした。
キーボード操作しか想定していないVimに比べて、Microsoftのデフォルトのキーバインドは押しづらいし不便でした。
キーバインドを色々変えようと思ってみても、VS Codeのキーバインドはkeybindings.jsonなのにVS Code Vimのキーバインドはsettings.jsonで分かれてて意味不明でした。
半年、色々やってみて分かったことなどをここにまとめていければなと思います。

一応注意点として以下の人を想定しています。
* Vimを使ったことがあって基本の操作はわかる。
* VS Codeを使っている。
* WindowsのVS Code

あと書いている人のせいで以下の内容になっています。
* Web関係の仕事じゃないからEmmetとか使う事は考慮していません。
* 偉そうなこと書いてるけど素のVimの知識もそんなにないよ。(レジスタとか全然分かってない)
* 過去記事とネタがかなり被ってます。

前に書いた記事
[VSCodeVimとVSCodeのバインドをうまい具合に共存させる](https://qiita.com/koutasan/items/2ecacef390cd608cc056)
[VimとVSCodeVimの違いをなるべく減らす設定](https://qiita.com/koutasan/items/06d34c279ca06c977884)

## 用語
あっているのか不安なのでこの記事で書いている用語について補足しておきます。
間違っていたら指摘して頂けるとありがたいです。

* エディター VS Code上でファイルを操作している部分です。
* パネル     VS Codeの下に表示されるターミナルやデバックコンソールとか出てるやつです。
* サイドバー 左側に表示されているエクスプローラーとかいろいろ選択できる奴です。
* エクスプローラー サイドバーから選べる、ファイルの一覧が表示されるやつです(VimでいうところのNERDTree)
* settings.json VS Codeの設定を書いていくところです。コマンドパレットで"基本設定:設定(JSON)を開く"を選択すると開けるファイルです。
* keybindings.json VS Codeのキーバインドを書いていくところです。 コマンドパレットで"基本設定:キーボード ショートカットを開く(JSON)"を選択すると開けるファイルです。

## 基本の考え方
まず、VS Code Vimでの操作はsettings.jsonに記述する必要があります。(プラグインの設定のため)
逆にエディターではない部分(検索窓やエクスプローラー、パネル等)の操作はkeybindings.jsonに記述する必要があります。
keybindings.jsonにもエディター内での操作をバインドする事ができるのですが、ややこしくなるだけだと思ったので
settings.jsonにすべて記入しています(良い運用方法があれば教えてください！)

```json:settings.json
  //Normal Mode
  "vim.normalModeKeyBindingsNonRecursive": [
    {"before": ["j"],"after"                   : ["g","j"]},                                    //移動(表示文字)
    {"before": ["k"],"after"                   : ["g","k"]},                                    //移動(表示文字)
    {"before": ["<Leader>", "w"],"commands"    : [":wa"]},                                      //すべてを保存
    {"before": ["<Leader>", "c"],"commands"    : [{"command"  : "editor.action.commentLine"}]}, //コメント
  ],
```

Vimのほうのキーバインドは簡単です。基本的に以下の3パターンでバインドを行います。
1. 基本のバインドです。beforeに変更前のキー。afterに対応させるキーを書きます。
2. Vimのコマンドのバインドです。afterではなく、commandsで書く事ができます。
3. VS Codeのコマンドのバインドです。commandsの中にさらにcommandを書くことで実現できます。

```json:keybindings.json
    // サジェスチョン操作
    {
        "key": "ctrl+j",
        "command": "selectNextSuggestion",
        "when": "suggestWidgetMultipleSuggestions && suggestWidgetVisible && textInputFocus"
    },
    {
        "key": "ctrl+k",
        "command": "selectPrevSuggestion",
        "when": "suggestWidgetMultipleSuggestions && suggestWidgetVisible && textInputFocus"
    },
```

次にVS Codeでのキーバインドです。
keyに設定したいキー。commandに設定したいVS Codeのコマンド。whenにどの状況でバインドを有効にするかを選びます。
このwhenが曲者で数が多い上にしっかり設定しないと意図していない状態でキーの入力待ち状態になってしまいます。

```json:keybindings.json
    //コマンドパレットを開く
    {
        "key": "space oem_1",
        "command": "workbench.action.showCommands",
        "when":"!editorTextFocus && !editorHasSelection && !editorHasMultipleSelections && !inQuickOpen && !sideBarFocus && !panelFocus"
    },

```json:settings.json
    {"before": ["<Leader>", ":"],"commands"    : [{"command"  : "workbench.action.showCommands"}]}, //コマンドパレット
```

settings.jsonとkeybindings.jsonで被るバインドは両方に記述しています。
この時にkeybindings.jsonのwhenをしっかり記述しないと、spaceを押した時点でキー入力待ちになってしまうので注意です。


## VS Code Vim 基本設定

### Leader

```json:settings.json
  "vim.leader" : "<space>", //map leaderの設定
```

leader(キーバイン度のprefixみたいな奴)の設定です。
大体の人がそうしている気もしますが、僕は一番押しやすいspaceにしています。

### yank時の色
```json:settings.json
  "vim.highlightedyank.enable"   : true,                           //yankした箇所をハイライト表示にする
  "vim.highlightedyank.color"    : "rgba(0   , 240 , 170 , 0.5)" , //yankした時の色
  "vim.highlightedyank.duration" : 150,                            //yankした時の色の表示時間
```

yank時の色の設定です。Vimだとpluginを入れないと実現できませんがVS Code Vimだとデフォルトで対応しています。

### vim plugin設定

```json:settings.json
  // vim plugin有効化
  "vim.easymotion"          : true, //easy motionを有効化
  "vim.surround"            : true, //surroundを有効にする
  "vim.replaceWithRegister" : true, //replaceWithRegisterを有効化
```

VS Code Vimで実装されているpluginを有効にします。好みによりますが、僕はこの3つを有効にしています。
easy motion,surroundなんかは定番ですがreplaceWithRegisterもtext Object Replaceのように動作するので大変重宝しています。

## settings.json(VS Code Vim KeyBind)

### Normal Mode

#### デフォルトの操作を微妙に変える

```json:settings.json
    // 動作が不安定なので修正
    {"before": ["u"],"commands"   : [{"command"  : "undo"}]},                            //デフォルトだと戻りすぎるので修正
    {"before": ["<C-r>"],"commands": [{"command": "redo"}]},                             //デフォルトだと戻りすぎるので修正
    {"before": ["<C-o>"],"commands": [{"command": "workbench.action.navigateBack"}]},    //デフォルトだと上手く動作しないので修正
    {"before": ["<C-i>"],"commands": [{"command": "workbench.action.navigateForward"}]}, //デフォルトだと上手く動作しないので修正

    {"before": ["x"],"commands"   : [{"command"  : "deleteRight"}]},                     //xで削除してもレジスタを占領しない
    {"before": ["n"],"after"      : ["n","z","z"]},                                      //検索結果を画面中央に
    {"before": ["N"],"after"      : ["N","z","z"]},                                      //検索結果を画面中央に
    {"before": ["*"],"after"      : ["*","z","z"]},                                      //検索結果を画面中央に
    {"before": ["#"],"after"      : ["#","z","z"]},                                      //検索結果を画面中央に
    {"before": [">"],"commands"   : ["editor.action.indentLines"]},                      //インデント調整(repeat可能)
    {"before": ["<"],"commands"   : ["editor.action.outdentLines"]},                     //インデント調整(repeat可能)
```

デフォルトの動作で微妙に嫌な動作を変えています。
undo,redo等のバインドはデフォルトのままだとなぜか戻りすぎたり、動作が不安定なのでVS Code標準の動作を行うようにバインドを設定しています。
xによる一文字削除はレジスタを占領しないように通常の削除コマンドのバインドを変更しています。
後はお好みですが、検索結果が中央になるように変更したり連続でインデントを調整できるように修正しています。

#### Vim pluginをより使いやすくする
```json:settings.json
    {"before": ["f"],"after" : ["<Leader>","<Leader>","2","s"]}, //easymotion 2s
    {"before": ["R"],"after" : ["g","r"]},                       //入れ替え
```

easymotionは2s(二文字入力したら選択画面になる)しか使わないのでfキーに置き換えています。
replaceWithRegisterのgrもReplace Objectを使っていた時のバインドであるRに設定しています。

#### ファイル操作
```json:settings.json
    {"before": ["<Leader>", ";"],"commands"    : [{"command"  : "workbench.action.showAllEditors"}]}, //エディタ検索
    {"before": ["<Leader>", "@"],"commands"    : [{"command"  : "workbench.action.quickOpen"}]},      //ファイル検索
    {"before": ["<Leader>", ":"],"commands"    : [{"command"  : "workbench.action.showCommands"}]},   //コマンドパレット
    {"before": ["<Leader>", "\\"],"commands"   : [{"command"  : "workbench.action.gotoSymbol"}]},     //シンボル検索
```

Quick Openやコマンドパレットの表示などはleader + 小指で押せる位置にまとめています。
こうする事により、ファイル間やエディター間の移動がかなり楽になります。

#### window間移動
```json:settings.json
    {"before": ["<Leader>", "h"],"after"       : ["<C-w>","h"]}, //window移動
    {"before": ["<Leader>", "j"],"after"       : ["<C-w>","j"]}, //window移動
    {"before": ["<Leader>", "k"],"after"       : ["<C-w>","k"]}, //window移動
    {"before": ["<Leader>", "l"],"after"       : ["<C-w>","l"]}, //window移動
```

Leader + hjklでwindow間を移動します。注意してほしいのがこの操作でexplorerやpanelに移動した場合、keybindings.jsonでもエディターに戻るバインドを設定してあげる必要があります。

### Insert Mode
```json:settings.json
      {"before": ["j", "j"],"after": ["<Esc>"]},                                                                       //jjでノーマルモードに戻る
      {"before": [";",";"],"commands": ["editor.action.triggerSuggest"]},                                              //;;でサジェストの起動に使う
```

insert Mode時にjjでノーマルモードに戻る設定をしてる人は多いと思いますが、設定していてお気に入りなのが;;でサジェストを起動するようにしている事です。
こうする事により、手の配置を崩すことなくかなり快適です。

### Visual Mode

#### Visual Modeに入った直後だとなぜかデフォルトキー扱いになる問題
原因は一向に謎なのですがvキーを押してVisual Modeに入った直後
設定したキーを押してもなぜかデフォルトのキー扱いになる問題があります。

```json:settings.json
    {"before": ["J"],"after"    : ["1","0","j"]}, //移動を早める
```

たとえばVisualModeに入った直後、上記のキーバインドでJキーを押しても
本来なら10行下にカーソルが移動してほしいのに現在の行と下の行で連結されてしまいます。

この問題を回避するためには、vを入れたバインドを別途する必要があります。

```json:settings.json
    {"before": ["J"],"after"    : ["1","0","j"]}, //移動を早める
    {"before": ["v","J"],"after": ["1","0","j"]}, //移動を早める
```

#### デフォルトの操作を微妙に変える

```json:settings.json
{"before": ["p"],"after": ["p","g","v","y"]}, //pasteした時に上書きされないようにする。
```

ペーストした時にレジスタが上書きされないようにしています。
連続でペーストする際なんかに便利です。

### 外部プラグイン呼び出し
```json:settings.json
    /* ------------------------------外部プラグイン呼び出し ----------------------------- */

    /* ------------------------------- Leader + ------------------------------- */

    {"before": ["<Leader>", "n"],"commands": [{"command":"extension.insertNumbers"}]},                                 //inert numbers
    {"before": ["<Leader>", "<Leader>"],"commands": [{"command": "textmarker.toggleHighlight"}]},                      //特定の色をハイライトする
    {"before": ["<Leader>", "<ESC>"],"commands": [{"command": "textmarker.clearAllHighlight"}]},                       //マーカー色を消す
    {"before": ["<Leader>", "t"],"commands": [{"command": "extension.toggle"}]},                                       //true⇒falseのように切り替える

    /* ----------------------------- Leader + F +  ---------------------------- */
    {"before": ["<Leader>","f","@"],"commands": [{"command": "markdown-preview-enhanced.openPreviewToTheSide"}]},      //markdownで開く
    {"before": ["<Leader>", "f",";"],"commands": [{"command"  : "extension.toggleSemicolon"}]},                        //セミコロンを挿入
    {"before": ["<Leader>", "f",":"],"commands": [{"command"  : "extension.toggleColon"}]},                            //コロンを挿入
    {"before": ["<Leader>", "f",","],"commands": [{"command"  : "extension.toggleComma"}]},                            //コンマを挿入

    /* ------------------------ m + "" (bookmark Plugin) ------------------------ */
    {"before": ["m", "m"],"commands": [{"command": "bookmarks.toggle"}]},                                              //bookmarkをtoggleする
    {"before": ["m", "i"],"commands": [{"command": "bookmarks.toggleLabeled"}]},                                       //引用付きbookmarkをtoggleする
    {"before": ["m", "w"],"commands": [{"command": "bookmarks.jumpToNext"}]},                                          //次のbookmarkに飛ぶ
    {"before": ["m", "b"],"commands": [{"command": "bookmarks.jumpToPrevious"}]},                                      //前のbookmarkに戻る
    {"before": ["m", "a"],"commands": [{"command": "bookmarks.list"}]},                                                //bookmarkのlistを表示する
```

外部プラグインの設定です。コマンド呼び出しで簡単に実装する事ができます。
余談ですが、Vimにバインドすると便利なオススメのプラグインは以下になります。

bookmarks               特定の行にブックマークを張れる。
insertNumbers           昇順or降順の数字の挿入。
toggle toggleSemicolon  セミコロン等をトグルできる。
Toggler                 設定した文字をトグルできる。
multi-command           自分で設定した複数コマンドを実行できる。

## keybindings.json

### window移動など
```json:keybindings.json
    // サジェスチョン操作
    {
        "key": "ctrl+j",
        "command": "selectNextSuggestion",
        "when": "suggestWidgetMultipleSuggestions && suggestWidgetVisible && textInputFocus"
    },
    {
        "key": "ctrl+k",
        "command": "selectPrevSuggestion",
        "when": "suggestWidgetMultipleSuggestions && suggestWidgetVisible && textInputFocus"
    },

    //コマンドパレットの移動
    {
        "key": "ctrl+j",
        "command": "workbench.action.quickOpenSelectNext",
        "when": "inQuickOpen",
    },
    {
        "key": "ctrl+k",
        "command": "workbench.action.quickOpenSelectPrevious",
        "when": "inQuickOpen ",
    },

    //editorじゃない時の操作(editorではvscodevimのコマンドが有効になる)
    //tabを閉じる
    {
        "key": "space x",
        "command": "workbench.action.closeActiveEditor",
        "when":"!editorTextFocus && !editorHasSelection && !editorHasMultipleSelections && !inQuickOpen && !sideBarFocus && !panelFocus",
    },
    //tabを進める
    {
        "key": "space u",
        "command": "workbench.action.nextEditor",
        "when":"!editorTextFocus && !editorHasSelection && !editorHasMultipleSelections && !inQuickOpen && !sideBarFocus && !panelFocus"
    },
    //tabを戻す
    {
        "key": "space y",
        "command": "workbench.action.previousEditor",
        "when":"!editorTextFocus && !editorHasSelection && !editorHasMultipleSelections && !inQuickOpen && !sideBarFocus && !panelFocus",
    },
    //markdownのpreviewから戻る
    {
        "key": "space h",
        "command":"workbench.action.focusLeftGroup",
        "when":"!editorTextFocus && !editorHasSelection && !editorHasMultipleSelections && !inQuickOpen && !sideBarFocus && !panelFocus",
    },
    //コマンドパレットを開く
    {
        "key": "space oem_1",
        "command": "workbench.action.showCommands",
        "when":"!editorTextFocus && !editorHasSelection && !editorHasMultipleSelections && !inQuickOpen && !sideBarFocus && !panelFocus"
    },
    //ファイルを開く
    {
        "key": "space oem_3",
        "command": "workbench.action.quickOpen",
        "when":"!editorTextFocus && !editorHasSelection && !editorHasMultipleSelections && !inQuickOpen && !sideBarFocus && !panelFocus"
    },
    //何もエディタを開いていなかったらwindowを閉じる
    {
        "key": "space q",
        "command": "workbench.action.closeWindow",
        "when":"!editorIsOpen"
    },
```

サジェスチョン移動、コマンドパレットの移動です。基本的に文字入力画面で選択する場合はctrl+j or kが鉄板で操作しやすいです。
editorじゃない時の操作は基本的にVS Code Vimと同じキーに割り当てています。

### 各windowの表示

```json:keybindings.json
    //サイドバー表示(toggle)
    {
        "key": "ctrl+Space ctrl+Space",
        "command":"workbench.action.toggleSidebarVisibility",
        "when": "!explorerViewletVisible && !inDebugMode && vim.mode != 'SearchInProgressMode' && vim.mode != 'Insert'"
    },
    {
        "key": "ctrl+Space ctrl+Space",
        "command": "workbench.action.toggleSidebarVisibility",
        "when": "explorerViewletVisible && !inDebugMode && vim.mode != 'SearchInProgressMode' && vim.mode != 'Insert'"
    },

    //explorer表示
    {
        "key": "ctrl+space ctrl+e",
        "command": "workbench.view.explorer"
    },
    {
        "key": "ctrl+space e",
        "command": "workbench.view.explorer"
    },

    //grep検索表示
    {
        "key": "ctrl+space ctrl+f",
        "command": "workbench.action.findInFiles",
    },
    {
        "key": "ctrl+space f",
        "command": "workbench.action.findInFiles",
    },

    //grep置換表示
    {
        "key": "ctrl+space ctrl+h",
        "command": "workbench.action.replaceInFiles",
    },
    {
        "key": "ctrl+space h",
        "command": "workbench.action.replaceInFiles",
    },

    //debug表示
    {
        "key": "ctrl+space ctrl+d",
        "command": "workbench.view.debug"
    },
    {
        "key": "ctrl+space d",
        "command": "workbench.view.debug"
    },

    //version管理表示
    {
        "key": "ctrl+space ctrl+s",
        "command": "workbench.view.scm"
    },
    {
        "key": "ctrl+space s",
        "command": "workbench.view.scm"
    },

    //パネル表示
    {
      "key": "ctrl+space ctrl+p",
      "command": "workbench.action.focusPanel",
        "when": "!panelFocus"
    },
    {
      "key": "ctrl+space p",
      "command": "workbench.action.focusPanel",
        "when": "!panelFocus"
    },

```

エディター以外の各windowの表示設定です。prefixとしてctrl+spaceを採用しています。
whenを設定していないので基本的にどのような状況からでも移動できるようにしています。

### サイドバー、パネルからの移動
```json:keybindings.json
    // サイドバーからの移動 
    {
        "key": "space l",
        "command":"workbench.action.focusFirstEditorGroup",
        "when": "explorerViewletVisible && explorerViewletFocus && !editorFocus && !inQuickOpen && !panelFocus"
    },

    // パネルからの移動 
    {
        "key": "space k",
        "command":"workbench.action.focusFirstEditorGroup",
        "when": "panelFocus && !explorerViewletFocus && !editorFocus && !inQuickOpen && !terminalFocus"
    },
```

サイドバーとパネルからの移動はVS Code Vimの設定と同じようにprefixをspaceにして設定しています。
ただし、terminal画面やテキストボックスでの入力画面などはprefixがspaceだと不都合があるので
サイドバーならエクスプローラーに一旦切り替える、パネルなら一旦閉じるようにしています(ここは修正の余地ありですね)

### エクスプローラー

```json:keybindings.json
/* -------------------------------- explorer -------------------------------- */
    //explorer表示
    {
        "key": "ctrl+space ctrl+e",
        "command": "workbench.view.explorer"
    },
    {
        "key": "ctrl+space e",
        "command": "workbench.view.explorer"
    },
    //エクスプローラー間移動
    {
        "key": "ctrl+j",
        "command": "list.focusDown",
        "when": "explorerViewletFocus && explorerViewletVisible && !inputFocus"
    },
    {
        "key": "ctrl+k",
        "command": "list.focusUp",
        "when": "explorerViewletFocus && explorerViewletVisible && !inputFocus"
    },
    {
        "key": "Shift+j",
        "command": "multiCommand.UpTempoListFocusDown",
        "when": "explorerViewletFocus && explorerViewletVisible && !inputFocus"
    },
    {
        "key": "Shift+k",
        "command": "multiCommand.UpTempoListFocusUp",
        "when": "explorerViewletFocus && explorerViewletVisible && !inputFocus"
    },

    //新規ファイル作成
    {
        "key": "ctrl+a",
        "command": "explorer.newFile",
        "when": "explorerViewletFocus && explorerViewletVisible && !inputFocus"
    },
    //新規フォルダ作成
    {
        "key": "ctrl+n",
        "command": "explorer.newFolder",
        "when": "explorerViewletFocus && explorerViewletVisible && !inputFocus"
    },
    //エクスプローラーでフォルダを開く
    {
        "key": "ctrl+e",
        "command": "revealFileInOS",
        "when": "explorerViewletFocus && explorerViewletVisible && !inputFocus"
    },
    //リネーム
    {
        "key": "ctrl+r",
        "command": "renameFile",
        "when": "explorerViewletFocus && explorerViewletVisible && !inputFocus"
        // "args": { "text": "'${file}'" }
    },
    //削除
    {
        "key": "ctrl+d",
        "command": "deleteFile",
        "when": "explorerViewletFocus && explorerViewletVisible && !inputFocus"
    },
    //フォルダーを検索
    {
      "key": "ctrl+f",
      "command": "filesExplorer.findInFolder",
      "when": "explorerResourceIsFolder && explorerViewletVisible && filesExplorerFocus && !inputFocus"
    },
    //エクスプローラー内で検索
    {
      "key": "oem_2",
      "command": "list.toggleKeyboardNavigation",
      "when": "listFocus && listSupportsKeyboardNavigation && !inputFocus"
    },

```

エクスプローラーの設定です。エクスプローラーの移動はVS Code Vimを入れるとj,kで上下移動できるのですが、
エクスプローラー検索(list.toggleKeyboardNavigation)の際も移動できるようにctrl+j,ctrl+kにも割り当てています。
他にもctrl+で使えると便利であろうコマンドを割り当てています。

```json:setings.json
"workbench.list.keyboardNavigation" : "filter",
```

エクスプローラー検索はfilterを設定すると、検索に引っかからないファイルが消えるのでお勧めです。

```json:settings.json
"multiCommand.commands": [
    //explorerを一気に移動
    {
      "command" : "multiCommand.UpTempoListFocusDown",
      "sequence": [
        "list.focusDown",
        "list.focusDown",
        "list.focusDown",
        "list.focusDown",
        "list.focusDown",
        "list.focusDown",
        "list.focusDown",
        "list.focusDown",
        "list.focusDown",
        "list.focusDown",
      ]
    },
    {
      "command" : "multiCommand.UpTempoListFocusUp",
      "sequence": [
        "list.focusUp",
        "list.focusUp",
        "list.focusUp",
        "list.focusUp",
        "list.focusUp",
        "list.focusUp",
        "list.focusUp",
        "list.focusUp",
        "list.focusUp",
        "list.focusUp",
      ]
    }
]

```

shift+j,shift+kにはmultiCommand.UpTempoListFocusDown,multiCommand.UpTempoListFocusUpというコマンドを割り当てています。
これはl[multi-command](https://marketplace.visualstudio.com/items?itemName=ryuta46.multi-command)というプラグインを使って
新規で作成したコマンドを割り当てています。multi-commandは使いこなすと色々な用途で使えるのでお勧めのプラグインです。

## 終わりに
色々弄ってキーボードのみで基本的に操作を完結する事が出来るようになったのですが、
まだまだ詰めが甘い部分も多く残っており、暇な時を見つけてはsettings.jsonを弄っています。
最初からかなり完成されているVS Codeですが設定していけばいくほどかゆいところに手が届いている気もするのでVimから移行を考えている人の参考になれば幸いです。
