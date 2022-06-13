---
title: VimとVSCodeVimの違いをなるべく減らす設定
tags: Vim VSCode 環境構築
author: koutarn
slide: false
---
## VSCodeなかなかええやん
サクラエディタおじさんと肩を並べて、Vimで作業をしていたのですが、
重い腰を上げてVSCodeを一から設定してみました。
改めて使ってみると中々良かったので設定を晒してみます。

```json-doc

  //visual studio codeの設定
  //vsvimが有効ならghでコマンドの説明文を確認できるので使う

    //========================================================================
    // vim ...            Editorでなら処理待ちが発生せず入力可能
    // keybinding.json    あらゆる場面で制御可能、入力をすると処理待ち発生
    // prefix Editor基本操作 (vim)                      -> space
    //        Editor none Active (keybindings.json)     -> space
    //        commandparet呼び出し (vim)                -> space
    //        基本UI操作(keybinding.json)               -> ctrl+space
    //          exprorer操作(keybinding.json)           -> none 
    //          サジェスチョン操作(keybindin.json)      ->ctrl
    //          コマンドパレット操作(keybindin.json)    ->ctrl
    //        sidebar呼び出し(keybinding.json)          -> cntr+space
    //        panel呼び出し(keybinding.json)            -> cntr+space
    //========================================================================

  //========================================================================
  // vscodeのeditorの設定
  //========================================================================
                                                                               //----------------------------------------------------------
  "editor.fontSize"               : 13.5,                                      //fontsize
  "editor.wordWrap"               : "on",                                      //1行長くならないように
  "editor.lineHeight"             : 0,                                         //1行の高さ
  "editor.renderLineHighlight"    : "all",                                     //現在の行番号含めて強調表示する
  "editor.minimap.enabled"        : false,                                     //minimap削除
  "editor.insertSpaces"           : true,                                     //tabをスペースとして扱う
  "editor.tabSize"                : 4,                                         //tabをデフォルトで4にする
  "editor.renderWhitespace"       : "boundary",                                //エディタ上での空白表示設定
  "editor.renderControlCharacters": true,                                      //制御文字の表示
  "editor.cursorBlinking"         : "smooth",                                  //カーソルの点滅をヌルヌルにする
  "editor.autoIndent"             : true,                                      //autoindentを入れる
  "editor.fontFamily"             : "Cica,Consolas, 'Courier New', monospace", //font settings
  "extensions.autoUpdate"         : true,                                      //プラグインを自動アップデート
  "editor.autoClosingBrackets": "beforeWhitespace",
  "editor.autoClosingQuotes"  : "beforeWhitespace",
  "breadcrumbs.enabled"           : false,                                      //パンクズリストはそんなに使わないし非表示
  "editor.detectIndentation": false,                                            //indent設定はファイル設定ではなく、自分の設定を優先する

  //zen mode settings
  "zenMode.fullScreen": true,
  "zenMode.centerLayout": true,
  "zenMode.hideActivityBar": true,
  "zenMode.hideLineNumbers": false,
  "zenMode.hideTabs": false,
  "zenMode.hideStatusBar": true,
  "zenMode.restore": false,

  // "editor.rulers": [80,130],                                                       //目安を表示

  //サジェスチョン
  "editor.quickSuggestionsDelay":400 ,//入力補完の検出タイミング

  // "editor.quickSuggestions":false,
  "editor.quickSuggestions": {        //入力補完を自動で表示する 
        "other"   : true,             //文字列以外
        "strings" : false,            //文字列
        "comments": false,            //コメント
  },  

  "editor.tokenColorCustomizations": {
    "comments":{
      "foreground":"#7f9ea0",       //コメントの色
      "fontStyle": "",
    }, 

    "functions":{
      "fontStyle": "underline",
    },
  },

  //encodings
  "files.autoGuessEncoding"       : true,                                      // 有効な場合、ファイルを開くときに文字セット エンコードを推測します。言語ごとに構成することも可能
  "files.autoSave"                : "off",                                     //ダーティファイルの作成を無効化
  "files.eol": "auto",                                                         //既定の改行文字をautoにする
  //----------------------------------------------------------
  // vscodeのバージョン管理系
  //----------------------------------------------------------
  // ソース管理プロバイダーのセクションを常に表示するかどうか。
  "scm.alwaysShowProviders": true,

  //----------------------------------------------------------
  // vscodeの作業環境の設定(おもにタブの設定をvimと近づけるために設定)
  //----------------------------------------------------------
  "workbench.editor.labelFormat": "short",
  "workbench.editor.revealIfOpen": true,
  "workbench.editor.showIcons": true,
  "workbench.editor.highlightModifiedTabs": true,
  "workbench.editor.tabCloseButton": "left",
  "workbench.startupEditor": "none",
  "workbench.editor.openPositioning": "last",

  "explorer.openEditors.visible": 0,

  // エクスプローラーから非表示にするファイル
  "files.exclude": { 
    "tags":true,
    "**/.svn": true,
    "**/.git": true,
    "**/.DS_Store": true
  },

  // ファイル監視から除外するファイル
  "files.watcherExclude": {
    "**/.git/objects/**": true,
    "**/.git/subtree-cache/**": true,
    "**/node_modules/*/**": true,
    "**/.svn/**":true,
  },

  // ファイルブラウザには表示するが、検索から除外する
  "search.exclude": {
    "**/node_modules": true,
    "**/bower_components": true,
    "**/tmp/cache": true,
    "tags":true,
    "**/.svn": true,
  },

  //----------------------------------------------------------
  // vscodeのdebugの設定
  //----------------------------------------------------------
  "debug.inlineValues": true,

  //----------------------------------------------------------
  // vscodeのwindowの設定
  //----------------------------------------------------------
  "window.zoomLevel"      : 0,
  "window.newWindowDimensions": "default",
  "workbench.tips.enabled": false,
  "C_Cpp.updateChannel"   : "Insiders",

  //vscode sync
  "sync.forceDownload"        : false,
  "sync.quietSync"            : false,
  "sync.askGistName"          : false,
  "sync.removeExtensions"     : true,
  "sync.syncExtensions"       : true,
  "sync.autoDownload"         : false,
  "sync.autoUpload"           : false,

  // ワークベンチ設定
  "workbench.editor.showTabs"                  : true,                  // タブを表示
  "workbench.editor.enablePreview"             : false,                 //毎回新規で開く
  "workbench.editor.enablePreviewFromQuickOpen": false,                 //毎回新規で開く
  "workbench.activityBar.visible"              : false,                 // アクティビティバー(左端)を非表示に
  "workbench.editor.tabSizing"                 : "shrink",              // タブが多い場合,文字を非表示にしてもタブ表示を優先する
  "workbench.statusBar.visible"                : true,                  // ステータスバー(下端)を表示
  "workbench.sideBar.location"                 : "left",                // サイドバーを左に
  "workbench.colorTheme"                       : "One Dark Pro",
  "workbench.colorCustomizations"           : {
    "editor.lineHighlightBackground"        : "#002255",                //現在の行の背景色
    "editor.selectionBackground"            : "#31ca4a77",              //選択業の背景
    "editor.selectionHighlightBorder"       : "#00e1ff",                //線学業の前背景
    "editorError.border"                    : "#ff0000",                //エラーの下線の色
    "tab.activeBorder"                      : "#ffffff",                //アクティブなタブの色
    "tab.inactiveForeground"                : "#999999",                //アクティブでないタブの色
    "editorSuggestWidget.selectedBackground": "#4792b4",                //選択しているサジェストの背景色
    "editorSuggestWidget.foreground"        : "#f1efef",                //選択しているサジェストの文字色
    "editorWhitespace.foreground": "#8a8888",                           //tab等の制御文字
  },

  // クラッシュレポートを送信しない
  "telemetry.enableCrashReporter": false,
  "telemetry.enableTelemetry"    : false,


  //========================================================================
  //markdown settings
  //========================================================================
  "markdown.preview.breaks": true,

  //========================================================================
  //言語毎の設定
  //========================================================================
  //C関係の設定は社内の設定に合わせる
  "[c]": {
      "editor.tabSize": 4,
      "editor.insertSpaces": false,
  },
  "[cpp]": {
      "editor.tabSize": 4,
      "editor.insertSpaces": false,
  },
  "[markdown]": {
      "editor.tabSize": 2,                //タブサイズの設定
      "editor.quickSuggestions":true,     //サジェスチョンをすべて有効にする
      "editor.quickSuggestionsDelay": 0,  //サジェスチョンのディレイ
  },

  "[json]": {
      "editor.tabSize":2,
  },

  //========================================================================
  //plugin settings
  //========================================================================

  // scrolloff
  "scrolloff.alwaysCenter": false,
  "scrolloff.scrolloff"   : 9,

  //comment-divider
  "comment-divider.mainHeaderTransform":"titlecase",  //先頭を大文字にする。
  "comment-divider.subheaderTransform":"titlecase",   //先頭を大文字にする。

  //テキスト校正君
  "japanese-proofreading.textlint.丸かっこ（）":false,                                //半角カッコを指摘する
  "japanese-proofreading.textlint.疑問符(？)":false,                                  //疑問符の後に全角スペースが入っているかを指摘する
  "japanese-proofreading.textlint.かっこ類と隣接する文字の間のスペースの有無": false, //カッコの内側と外側にスペースを入れていないかを指摘する
  "japanese-proofreading.textlint.全角文字と半角文字の間": false,                     //全角文字と半角文字の間にスペースを入れていないかを指摘する
  "japanese-proofreading.textlint.全角文字どうし": false,                             //全角文字同市の間にスペースを入れていないか指摘する

  //multiCommand
  "multiCommand.commands": [
    {
    //     //preview画面から戻す
    //   "command": "multiCommand.previewMarkdownAndUnfocus",
    //   "sequence": [
    //     "workbench.action.closeActiveEditor",               //一旦閉じる
    //     "markdown-preview-enhanced.openPreviewToTheSide",   //再度開き直す
    //   ],
    },
  ],

  //========================================================================
  //VIM
  //========================================================================
  "vim.statusBarColorControl" : false,                     //statusbarの色のコントロールをしない
  "vim.highlightedyank.enable": true,                      //yankした箇所をハイライト表示にする
  "vim.highlightedyank.color":"rgba(0, 240, 170, 0.5)",    //yankした時の色
  "vim.highlightedyank.duration":150,                      //yankした時の色の表示時間
  "vim.leader"                : "<space>",                 //Map Leaderの設定
  "vim.autoindent"            : true,                      //autoindent
  "vim.useSystemClipboard"    : true,                      //system clipboardと同期する
  "vim.hlsearch"              : true,                      //hlserch
  "vim.visualstar"            : true,                      //カーソル上にあるワードを"*"で検索
  "vim.useCtrlKeys"           : true,                      //諸々のctrlキーを使った操作が有効になる
  "vim.debug.silent": true,                                //アラートを出さない
  "vim.timeout":1200,                                      //入力のタイムアウト時間

  // vim plugin有効化
  "vim.easymotion"            : true,                      //easy motionを有効化
  "vim.surround"              : true,                      //surroundを有効にする

  //easy motion
  "vim.easymotionMarkerForegroundColorOneChar": "rgba(0,240,170,0.9)",                         //一文字目の色
  "vim.easymotionMarkerForegroundColorTwoChar": "rgba(0,240,170,0.9)",                         //二文字目の色
  "vim.easymotionMarkerBackgroundColor"       : "",                                            //背景色
  "vim.easymotionMarkerWidthPerChar"          : 19,                                            //各文字に割り当てられている幅
  "vim.easymotionMarkerHeight"                : 0,                                             //マーカーの高さ
  "vim.easymotionMarkerFontFamily"            : "Cica",                                        //フォント
  "vim.easymotionMarkerFontSize"              : "12.5",                                        //フォントサイズ
  "vim.easymotionMarkerFontWeight"            : "normal",                                      //フォントの太さ
  "vim.easymotionKeys"                        : "asdfhjklwqeruioopghty;",                      //マーカーに使用される文字列
  "vim.easymotionMarkerYOffset"               : 13.5,                                          //高さのずれ修正

  //================================================================================================================
  //key map for vim
  //keybinding.jsonと違いキーの組み合わせで処理待ちが発生しない
  //keyのデフォルトキーを使い,キー操作を定義したくても
  //デフォルトのコマンドが有効になる。(例えば、sh→0にしたくてもsのコマンドが有効になっている)
  //Leaderキーを入力後のコマンドは無視されているので、極力Leaderを活用するようにする。
  //beforeは必ず定義しなければならないが、afterまたはcommandsでコマンドを呼び出せる。whenによる操作も可能っぽい
  //================================================================================================================

  //nmap
  "vim.normalModeKeyBindings": [
    // {"before": ["s"],"after"               : [""]},                                             //unmap?
  ],

  //nnoremap
  "vim.normalModeKeyBindingsNonRecursive": [

    {"before": ["J"],"after"               : ["1","0","j"]},                                                                         //移動を早める
    {"before": ["K"],"after"               : ["1","0","k"]},                                                                         //移動を早める
    {"before": ["H"],"after"               : ["0"]},                                                                                 //端に移動
    {"before": ["L"],"after"               : ["$"]},                                                                                 //端に移動
    {"before": ["<Leader>", "h"],"after"   : ["<C-w>","h"]},                                                                         //window移動
    {"before": ["<Leader>", "j"],"after"   : ["<C-w>","j"]},                                                                         //window移動
    {"before": ["<Leader>", "k"],"after"   : ["<C-w>","k"]},                                                                         //window移動
    {"before": ["<Leader>", "l"],"after"   : ["<C-w>","l"]},                                                                         //window移動
    {"before": ["]"],"commands": [{"command": "C_Cpp.PeekDeclaration"}],"when":["editorLangId == c"]},                               //宣言を見る c専用
    {"before": ["["],"commands": [{"command": "editor.action.peekDefinition"}]},                                                     //定義を見る
    {"before": ["<Leader>", "s"],"commands": [":split"]},                                                                            //水平に開く
    {"before": ["<Leader>", "v"],"commands": [":vsplit"]},                                                                           //水平にを閉じる
    {"before": [">"],"commands" : ["editor.action.indentLines"]},                                                                    //インデント調整(repeat可能)
    {"before": ["<"],"commands" : ["editor.action.outdentLines"]},                                                                   //インデント調整(repeat可能)
    {"before": ["<Leader>", "u"],"after"   : ["g","t"]},                                                                             //tab移動
    {"before": ["<Leader>", "y"],"after"   : ["g","T"]},                                                                             //tab移動
    {"before": ["<Leader>", "x"],"commands": [":q!"]},                                                                               //tabを閉じる
    {"before": ["<Leader>", "q"],"commands": [":qa!"]},                                                                              //すべてを閉じる
    {"before": ["<Leader>", "w"],"commands": [":wa"]},                                                                               //すべてを保存
    {"before": ["<Leader>","o"],"after"    : ["o","<ESC>"]},                                                                         //空の行を挿入
    {"before": ["<Leader>","O"],"after"    : ["O","<ESC>"]},                                                                         //空の行を挿入
    {"before": ["<Leader>", "c"],"commands": [{"command": "editor.action.commentLine"}]},                                            //コメント
    {"before": ["<Leader>", ":"],"commands": [{"command": "workbench.action.showCommands"}]},                                        //コマンドパレット
    {"before": ["<Leader>", ";"],"commands": [{"command": "workbench.action.quickOpen"}]},                                           //ファイル検索

    {"before": ["<CR>"],"after"    : ["G"]},                                                                             //最終行へ
    {"before": ["<BS>"],"after"    : ["g","g"]},                                                                         //先頭行へ

    // 検索結果を画面中央に
    {"before": ["n"],"after"    : ["n","z","z"]},                                                                         
    {"before": ["N"],"after"    : ["N","z","z"]},                                                                         
    {"before": ["*"],"after"    : ["*","z","z"]},                                                                         
    {"before": ["#"],"after"    : ["#","z","z"]},                                                                         

    //Surround
    {"before": ["s"],"after"               : ["y","s"]},                                                                           //surround add

    //easy motion
    {"before": ["f"],"after"               : ["<Leader>","<Leader>","2","s"]},                                                       //easymotion 2s

    //Multi-Cursor Mode
    //prefix Ctrl 
    {"before": ["<C-n>",],"after"   : ["g","b"]},                                                                                    //選択した文字に対して増やす    
    {"before": ["<C-k>",],"commands": [{"command": "editor.action.insertCursorAbove"}]},                                             //シンボルをリネーム(mulchipulcursor)   
    {"before": ["<C-j>",],"commands": [{"command": "editor.action.insertCursorBelow"}]},                                             //シンボルをリネーム(mulchipulcursor)   

    //外部プラグイン呼び出し
    {"before": ["<Leader>", "@"],"commands": [{"command": "markdown-preview-enhanced.openPreviewToTheSide"}]},                       //markdownで開く
  ],
                                                                                                                                     //insert mode
	"vim.insertModeKeyBindings":[
      {"before": ["j", "j"],"after": ["<Esc>"]},                                                                                     //jjでノーマルモードに戻る
      {"before": [";",";"],"commands": ["editor.action.triggerSuggest"]},                                                            //;;でサジェストの起動に使う
      {"before": ["v","L"],"after": ["$","h"]},                                                 //端に移動
      // {"before": ["<C-h>"],"after"    : ["0"]},                                                     //端に移動
      // {"before": ["<C-l>"],"after"    : ["$","h"]},                                                 //端に移動
  ],
                                                                                                                                     // Visual Mode
    "vim.visualModeKeyBindingsNonRecursive": [
      //vを押した直後はvのコマンドが残っているので注意
      //visualmode後にすぐ実行したいものは、二重で定義する。
      {"before": ["J"],"after"    : ["1","0","j"]},                                             //移動を早める
      {"before": ["v","J"],"after": ["1","0","j"]},                                             //移動を早める
      {"before": ["K"],"after"    : ["1","0","k"]},                                             //移動を早める
      {"before": ["v","K"],"after": ["1","0","k"]},                                             //移動を早める
      {"before": ["v"],"after"    : ["a","f"]},                                                 //拡大選択
      {"before": ["v","v"],"after": ["a","f"]},                                                 //拡大選択
      {"before": ["H"],"after"    : ["0"]},                                                     //端に移動
      {"before": ["L"],"after"    : ["$","h"]},                                                 //端に移動
      {"before": ["v","H"],"after": ["0"]},                                                     //端に移動
      {"before": ["v","L"],"after": ["$","h"]},                                                 //端に移動
      {"before": [">"],"commands" : ["editor.action.indentLines"]},                             //インデント調整(repeat可能)
      {"before": ["<"],"commands" : ["editor.action.outdentLines"]},                            //インデント調整(repeat可能)
      {"before": ["<Leader>", ":"],"commands": [{"command"  :"workbench.action.showCommands"}]},//コマンドパレット
      {"before": ["<Leader>", ";"],"commands": [{"command"  :"workbench.action.quickOpen"}]},   //ファイル検索
      {"before": ["<Leader>", "c"],"commands": [{"command":"editor.action.commentLine"}]},      //コメント

      //Multi-Cursor Mode
      {"before": ["<C-n>"],"after"   : ["g","b"]},                                                                                   //選択した文字に対して増やす 
      {"before": ["<C-l>",],"commands": [{"command": "editor.action.insertCursorAtEndOfEachLineSelected"}]},                         //行末尾にカーソルを出す

      //外部プラグイン呼び出し
      {"before": ["<Leader>", "b"],"commands": [{"command":"alignment.align"}]},                                                     //揃える
      {"before": ["<Leader>", "v"],"commands": [{"command":"extension.commentaligner"}]},                                            //コメントを揃える
    ],

                                                                                                                                     //vimではなくvscode側のキーを有効にする
  "vim.handleKeys": {
    "<C-a>": false,                                                                                                                  //全選択
    "<C-f>": false,                                                                                                                  //検索
    "<C-h>": false,                                                                                                                  //置換
  },

  //========================================================================
  //UI変更で自動で挿入されるやつ
  //========================================================================
}
```

setting.jsonは通常の設定とvimの設定をしています。
VScodevimでデフォルトのキーをアンマップする方法が分からなかったので、
prefixとして文字移動はShift、window移動やそれ以外の操作はLeaderを使用する構成にしています。

VSCodeで設定する際にkeybinding.jsonとsetting.jsonで設定を
どちらに書けば良いのか分かりづらかったのですが、
基本的にエディター上での操作はVScodevimに一任するスタイルでやっていってます。

特にお気に入りなのが、オートコンプリートを出すのに;;を使うようにしたものです。
jjでコマンドモードに戻る人も多いと思いますが、
それと同じ要領で覚えることが出来ます。(;;の入力で困ったらそのうち変えそうですが)



```json-doc

// 既定値を上書きするには、このファイル内にキー バインドを挿入しますauto[]
[
 [
    //========================================================================
    // vim ...            Editorでなら処理待ちが発生せず入力可能
    // keybinding.json    あらゆる場面で制御可能、入力をすると処理待ち発生
    // prefix Editor基本操作 (vim)                      -> space
    //        Editor none Active (keybindings.json)     -> space
    //        commandparet呼び出し (vim)                -> space
    //        基本UI操作(keybinding.json)               -> ctrl+space
    //          exprorer操作(keybinding.json)           -> none 
    //          サジェスチョン操作(keybindin.json)      ->ctrl
    //          コマンドパレット操作(keybindin.json)    ->ctrl
    //        sidebar呼び出し(keybinding.json)          -> cntr+space
    //        panel呼び出し(keybinding.json)            -> cntr+space
    //========================================================================

    //======================================================================================
    // bind方法はここを参照する
    // https://vscode-doc-jp.github.io/docs/getstarted/keybindings.html
    // https://code.visualstudio.com/docs/getstarted/keybindings#_when-clause-contexts
    //======================================================================================

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
        "command":"workbench.action.focusPreviousGroup",
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
        "key": "space oem_plus",
        "command": "workbench.action.quickOpen",
        "when":"!editorTextFocus && !editorHasSelection && !editorHasMultipleSelections && !inQuickOpen && !sideBarFocus && !panelFocus"
    },

/* -------------------------------------------------------------------------- */
/*                                  sidebar                                   */
/* -------------------------------------------------------------------------- */

    //サイドバー表示(toggle)
    {
        "key": "ctrl+Space ctrl+Space",
        "command":"workbench.action.toggleSidebarVisibility",
        "when": "!explorerViewletVisible && !searchViewletVisible && !inDebugMode && vim.mode != 'SearchInProgressMode' && vim.mode != 'Insert'"
    },
    {
        "key": "ctrl+Space ctrl+Space",
        "command": "workbench.action.toggleSidebarVisibility",
        "when": "explorerViewletVisible && !searchViewletVisible && !inDebugMode && vim.mode != 'SearchInProgressMode' && vim.mode != 'Insert'"
    },

    // サイドバーからの移動 
    {
        "key": "space l",
        "command":"workbench.action.focusFirstEditorGroup",
        "when": "explorerViewletVisible && explorerViewletFocus && !editorFocus && !inQuickOpen"
    },

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
        "key": "j",
        "command": "list.focusDown",
        "when": "explorerViewletFocus && explorerViewletVisible && !inputFocus"
    },
    {
        "key": "k",
        "command": "list.focusUp",
        "when": "explorerViewletFocus && explorerViewletVisible && !inputFocus"
    },

    //新規ファイル作成
    {
        "key": "n",
        "command": "explorer.newFile",
        "when": "explorerViewletFocus && explorerViewletVisible && !inputFocus"
    },
    //新規フォルダ作成
    {
        "key": "f",
        "command": "explorer.newFolder",
        "when": "explorerViewletFocus && explorerViewletVisible && !inputFocus"
    },
    //エクスプローラーでフォルダを開く
    {
        "key": "e",
        "command": "revealFileInOS",
        "when": "explorerViewletFocus && explorerViewletVisible && !inputFocus"
    },
    //リネーム
    {
        "key": "r",
        "command": "renameFile",
        "when": "explorerViewletFocus && explorerViewletVisible && !inputFocus"
    },
    //削除
    {
        "key": "d",
        "command": "deleteFile",
        "when": "explorerViewletFocus && explorerViewletVisible && !inputFocus"
    },
    // //svn操作
    // {
    //     "key": "s",
    //     "command": "tortoise-svn ...",
    //     "when": "explorerViewletFocus && explorerViewletVisible && !inputFocus",
    //     "args":"explorerPath",
    // },

/* ----------------------------- grep検索(sidebar) ---------------------------- */
    {
        "key": "ctrl+space ctrl+f",
        "command": "workbench.action.findInFiles",
        "when": "!searchInputBoxFocus"
    },
    {
        "key": "ctrl+space f",
        "command": "workbench.action.findInFiles",
        "when": "!searchInputBoxFocus"
    },

/* ---------------------------- debug(sidebar) ---------------------------- */

    {
        "key": "ctrl+space ctrl+d",
        "command": "workbench.view.debug"
    },
    {
        "key": "ctrl+space d",
        "command": "workbench.view.debug"
    },


/* -------------------------- version管理(sidebar) -------------------------- */

    {
        "key": "ctrl+space ctrl+s",
        "command": "workbench.view.scm"
    },
    {
        "key": "ctrl+space s",
        "command": "workbench.view.scm"
    },

/* --------------------------- plugin管理(sidebar) -------------------------- */

    {
        "key": "ctrl+space ctrl+z",
        "command": "workbench.view.extensions"
    },
    {
        "key": "ctrl+space z",
        "command": "workbench.view.extensions"
    },

/* -------------------------------------------------------------------------- */
/*                                    Panel                                   */
/* -------------------------------------------------------------------------- */

    //パネルから戻る
    {
        "key": "space k",
        "command": "workbench.action.focusLastEditorGroup",
        "when":"panelFocus && !editorTextFocus && !editorHasSelection && !editorHasMultipleSelections && !inQuickOpen"
    },
    //パネルを出す
    {
        "key": "ctrl+space ctrl+p",
        "command": "workbench.action.togglePanel",
        "when": "vim.mode != 'SearchInProgressMode' && vim.mode != 'Insert'"
    },

/* ---------------------------- terminal(panel) --------------------------- */

    //現状使わないので消しておく
    // {
    //     "key": "ctrl+space ctrl+t",
    //     "command": "workbench.action.terminal.toggleTerminal" ,
    //     "when": "vim.mode != 'SearchInProgressMode' && vim.mode != 'Insert'",
    // },

/* ---------------------------- error画面(panel) ---------------------------- */

    {
        "key": "ctrl+space ctrl+q",
        "command": "workbench.actions.view.problems" 
    },
    {
        "key": "ctrl+space q",
        "command": "workbench.actions.view.problems" 
    },
]
```

keybinding.jsonではVScodevimの設定で対応出来ない、サイドバーの操作などを行えるようにしています。
また、prefixはCntr+Spaceで極力押しやすい配置を心がけています。
whenでの動作制限が参考にした記事のままだったり、まだまだ設定が足りていない部分も多いのですが
とりあえず使う分にはこんな感じでいいのかなと思っています。

## 純粋なVimと比べてみて
どうしようもないところなのですが、やはり純粋なVimに比べて
テキストオブジェクトやオペレータが貧弱なのは否めません。
僕はゆとりVimmerなのでブロックを[対応してくれるプラグイン](https://github.com/rhysd/vim-textobj-anyblock)を入れて
()や""はci(やyi"を使わずcibで対応していたので割と不便に感じます。
また、オペレーターは[vim-operator-replace](kana/vim-operator-replace)がないため、わざわざviwしてpを押す事に凄くストレスを感じます。
**2020/10/23追記　今更ですが、VSCodeVimにも同様の機能が追加されています。**

また、今までvimでお世話になったプラグインが使えなかったり、
一応、おまけとしてついているeasymotionやsurroundも
元々のVimで使っていたものと違うものなので微妙に挙動が違い不便に感じてしまいます。
(easymotionは[haya14busaさんがforkしてたやつ](https://github.com/easymotion/vim-easymotion)、surroundは[rhysdさんがoperatorにしてたやつ](https://github.com/rhysd/vim-operator-surround)

と、ここまでVScodevimについての不満を多く書きましたが、
それは僕がゆとりVimmerだからの話であってpluginを多用しない人や
元々の設定からそこまで多く変えてvimを使っていなかった人にとっては
違和感なく扱えるプラグインだと思います(文句言うならプルリクしろって話ですね、すいません)

逆にVim以外の面ではVimにはない設定のしやすさだったり、オートコンプリートが強力な所
わざわざctagsを使わないでもいい所だったり、快適さが全然違うように感じます。
ここも僕がゆとりVimmerなので大きなことが言えませんが、
デフォルトで強力なのって改めて凄い所なのだなと思いました。

> 設定を参考にさせていただいた記事
[割と突き詰めてやったvim→vscode移行](https://qiita.com/y-mattun/items/45776b7e1942edb2f727)
[VSCodeのオススメ拡張機能 24 選 (とTipsをいくつか)](https://qiita.com/sensuikan1973/items/74cf5383c02dbcd82234)
[VSCodeをVimmerが満足できる設定にしてみた](https://blog.mamansoft.net/2018/09/17/vscode-satisfies-vimmer/)
