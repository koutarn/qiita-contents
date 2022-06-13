---
title: vimiumの微妙に嫌な所を改善する。
tags: Vim vimium アドオン ポエム
author: koutarn
slide: false
---
#Vimiumって凄い。
vimの操作に微妙に慣れてきたので、vimっぽく動かせるchromeのアドオンを探していたのですが
Vimiumっていうすんごいアドオンに出会ってしまいました。
fキーで選択出来るわ、検索用のモーダル出せるわ、何やこいつ状態です。
しかし、微妙に使いづらい所があったんで、そこら辺の修正方法を書いておこうと思います。

## googleの検索結果もコマンドで次ページへ移動出来るようにする
初期状態では、gooleの検索結果を[[や]]で改ページ出来ません。これは検索するうえで不便だと思います。

![タイトルなし.png](https://qiita-image-store.s3.amazonaws.com/0/253308/a0cc36e7-b983-22db-7eb4-ce283c9a51cb.png)

設定のPrevious pattrnsとNext patternsに"前へ"と"次へ"を追加します。
これで改ページを行えるようになります。

## 新規タブを検索画面にする。
僕のchromeの設定のせいかもしれませんが、初期状態の新規タブだとアドレスバーにフォーカスした状態になってしまいます。そのうえ、[javascriptでフォーカスを抜ける技](https://github.com/philc/vimium/issues/840)も使えませんでした。これは新規タブの画面を通常の検索画面にすることで改善出来ました。

![タイトル2し.png](https://qiita-image-store.s3.amazonaws.com/0/253308/0b7a3c34-2fa9-7434-463a-b30610ebeaf4.png)

設定のNew tab URLを検索画面にしてください。

## 検索すると勝手にテキストボックスにフォーカスされている
oを押してカスタムエンジンを使うと、勝手にテキストボックスにフォーカスされていて一々Escで抜けるのが面倒でした。

![タイトルなし3.png](https://qiita-image-store.s3.amazonaws.com/0/253308/7407cacc-3623-b3ab-a361-74354238aa0d.png)

これも設定のDon't let pages steal the focus on load にチェックボックスを入れるだけで改善出来ます。
