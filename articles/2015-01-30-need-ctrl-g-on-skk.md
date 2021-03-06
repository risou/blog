title: SKK は Ctrl+G が使えないと使いものにならない
date: 2015-01-30
datetime: 2015-01-30 09:43:26.982703 JST
alias: 26
---
*タイトルには「自分にとって」が不足している。また、 SKK 側のキーバインドを変更すれば以下の問題はそもそも起きない。*

[Mac の Sublime Text 3 で SKK を快適に使う - Qiita](http://qiita.com/risou/items/e509ab9cd6ad92988021) という Tips を書いた。 Sublime Text 3 だと Ctrl+G をアプリケーション側に奪われてキャンセルできなくてつらい……という問題を解消する方法だ。

そもそも SKK でそんなに頻繁にキャンセルを使うのかというと、使う。詳しくは SKK について調べるなり試すなりしていただくのが良いかと思うが、その用途は大きく2つに別れる。1つは「変換のキャンセル」もう1つは「単語登録のキャンセル」だ。

前者は `Backspace` や `x` でも同じことができるためたいした問題ではない。問題は後者だ。 SKK では、漢字への変換を行う際にマッチするものがなければ、自動的に単語登録モードに移行する。この単語登録モードはちょっとしたタイプミスでもしばしば目にすることになる。なぜなら、 SKK では送りがなの開始位置を明確に指定するからだ。たとえば「見える」と入力したければ、 `MiEru` とタイプすることになるが `Shift` を入れるタイミングがずれて `MieRu` となった場合、該当する漢字がデフォルトの辞書にはないため、単語登録モードに移行する。そして、単語登録モードに入ると、そこから戻るには「キャンセル」しかない。ここでは `Backspace` も `x` も効かない。そのため、 `Ctrl+G` は頻繁に使用される。

ところが、この環境で Sublime Text 3 を使うと上記の問題が発生する(Sublime Text 2 でも同様の問題は起きると思う)。これを解決するのが上記リンク先に書いた設定だ。

ちなみに、 Sublime Text 3 では `Ctrl+G` には行番号指定ジャンプが割り当てられている。最初、これを `Ctrl+G -> Ctrl+G` に変更しようかと思ったが、1つ目の入力が `Ctrl+G` のため、やはり AquaSKK に `Ctrl+G` が到達しなかった。結局自分は `Shift+Ctrl+G` に行番号指定ジャンプを割り当てた。 SKK のキャンセルに比べれば使用頻度がかなり少ないのと、既に慣れているものよりこれから慣れるものの方が矯正しやすいはずなので、これで問題は起きないと思う。
