title: iOS の AdBlock アプリ Crystal は jQuery の colorbox を抑止する
date: 2016-01-29
datetime: 2016-01-29 12:16:44.125647 JST
alias: 32

---
iOS 9 で搭載されたコンテンツブロック機能によって、 Safari に AdBlock アプリを連携させられるようになった。 AdBlock アプリといってもいろいろあって、ブロックするコンテンツの判断基準が異なったりする。



今回問題になったのは Crystal という AdBlock アプリを有効にした Safari で、 colorbox によるモーダル表示ができないことだ。モーダル表示といえば、画像を拡大して表示したり、確認ダイアログを出したりと、様々な用途で用いられている。



Crystal を導入されている方は colorbox のデモページで試してみると良い。



[colorbox example](http://www.jacklmoore.com/colorbox/example1/)



回避するには、同様のことができる他のプラグインを使うのが現実的だろう。 Magnific Popup は Crystal に阻害されずちゃんと動くことを確認した。



[Magnific Popup](http://dimsemenov.com/plugins/magnific-popup/)



AdBlock アプリは、どのような条件でブロック対象とみなしているかがブラックボックスになっていて、広告ではない（ブロックされてほしくない）情報がブロックされることがある。たとえば Crystal が有効だと Google Analytics による計測が正常に行えないのは周知の事実だ。とはいえ、ユーザの快適性のためにコンテンツブロックしているのであれば、今後判定条件は変わっていく（精度をあげていく）と思うし、ここに書いたような現象もいずれ解消される可能性はある。


