title: Yesod 使ってみて参考になったサイト
date: 2013-09-10
datetime: 2013-09-10 01:08:39.849838 JST
alias: 1

---

自分用メモ的な位置づけ。

### Yesodをnginxで動かすときのnginx.conf

[http://qiita.com/suztomo/items/ecaaa402e555a416afe7](http://qiita.com/suztomo/items/ecaaa402e555a416afe7)

できれば直接80番をYesodアプリに割り当てて、nginxでそれ以外をコントロール、という形にしたかったけど、
やり方わからなかったので、別ポートを指定して proxy_pass で処理した。

### プロダクション向けのビルド

[http://d.hatena.ne.jp/thimura/20111117/1321480728](http://d.hatena.ne.jp/thimura/20111117/1321480728)

    $ yesod build

でできるのかと思いきや、それとこれとは全く別のようで。

    $ cabal configure -fproduction
    $ cabal build

が正解らしい。このあたりちゃんと押さえられていないの良くないですね。

### プロダクションモードでの実行

これはどこかのサイトを参考にしたわけではないけど、ついでに。

deploy/Procfile を読んだら末尾に書いてあったのを参考に、実行時にポートの指定をしないなら、

    $ ./dist/build/{$app}/{$app} production

で良いっぽい。
