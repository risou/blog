title: Gyazoサーバを立てた話
date: 2014-04-24
datetime: 2014-04-24 01:24:01.600789 JST
alias: 22
---
大した話ではなく、自分用に Gyazo のサーバを立てた。今使ってるサーバの1つにインストールして動かしている、というだけの話。

以下の記事を参考にしました。  
良質な記事と gyazoserver に感謝。

[さくらVPSにgyazo+sinatra+nginxでオレgyazoを構築](http://rimtty.hatenablog.com/entry/2014/03/23/113958)

上記記事に書いてある通りにすれば良いのだけれど、一箇所だけ詰まったところがあったのでメモ。

*db ディレクトリを作成すること*

それだけ。これをやっておかないと、

```
Errno::ENOENT - No such file or directory - db/id:
```

とかでてくる。
