---
title: gdbについてまとめてみました
tags: デバッグ gdb
author: koutarn
slide: false
---
業務でgdbを使う機会が最近あったので書いてみます。

##コマンドは略称で覚える
コマンドは、breakpointだったりcontinueと打つと効率が悪いので略称で覚えたほうが効率が良いです。
breakpointはbで打てますし、continueはcで打てます。

##TUIモードがなんだかんだ最強
gdbはデフォルトだと見づらいので、TUIモードで使うと見やすいです。
wh　というコマンドを打つと呼び出せます。ただこのモードは頻繁に画面が崩れるのでCntr + Lで画面を再描画しないといけません。pythonであれば、[GDB Dashboard](https://github.com/cyrus-and/gdb-dashboard)を入れると最高だと思います。

##gdbinitで設定を弄るべき
gdbには、起動時に読み込むgdbinitファイルがあるのでコマンドをdefineでまとめたり設定をここに書きましょう。githubに[僕のdotfile](https://github.com/koutakun/dotfile)をあげているので見てください。



