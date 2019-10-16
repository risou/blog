title: Qiita:Team の記事を検索するための Alfred Workflow を実装した
date: 2019-10-16
datetime: 2019-10-16T20:04:18.130993+09:00
---

結構前から [Alfred](https://www.alfredapp.com) というアプリを使っていて、 Spotlight と使い分けたり、それぞれがバージョンを上げるごとに行ったり来たりを繰り返していたのだけれど、最近は Alfred Workflow がやっぱり強いな、ということで Alfred をメインに使っている（ファイルサーチだけなら現時点では Spotlight の方が強い気がしているので、サブで Spotlight も使っている）。

[Alfred Workflow](https://www.alfredapp.com/workflows/) とはなんぞや、というと Alfred が備えているアクションを組み合わせて複雑なオペレーションを Alfred の UI で操作できるようにするもので、特に強力なのがアクションとして **コードを実行したりプログラムを呼んだり** できること。

たとえば自分が便利に使っているところとしては、以下のようなものがある。

- [GitHub Workflow for Alfred 3](https://github.com/gharlan/alfred-github-workflow#github-workflow-for-alfred-3)
- [Lazy Link](http://www.packal.org/workflow/lazy-link)
- [Alfred Qiita Workflow](https://github.com/uetchy/alfred-qiita-workflow)
- [dropbox paper finder](http://www.packal.org/workflow/dropbox-paper-finder)
- [Div](http://www.packal.org/workflow/div)
- [alfred-esa-workflow](https://github.com/kyokomi/alfred-esa-workflow)

上記を見てもらうとわかるけど、いわゆる「記事」に相当するものを検索するのに多用しており、 Qiita や Esa といった情報共有サービスや Dropbox Paper のような閉じたコミュニティ内で使うようなドキュメント共有サービスにある記事を探すのに重宝している。

ただ、上記で紹介している Qiita の Workflow は qiita.com には対応しているけれど、 Qiita:Team には対応しておらず、 Qiita:Team 内の記事を検索したいケースでだけいちいちブラウザにスイッチするのが面倒だった。  
ので、自分で作った。

[Alfred Workflow for Qiita:Team](http://www.packal.org/workflow/qiitateam)

Qiita:Team の API は何度か触ったことがあるので、サクッとできた。  
Alfred Workflow では直接実行できる言語には限りがあるが、 bash とかを実行できるので実行可能なバイナリが生成できる言語であればだいたい使える。  
ということで今回は [Rust](https://www.rust-lang.org/) に挑戦してみた。

[risou/qiita-team-alfred-workflow: search articles in Qiita:Team by Alfred Workflow](https://github.com/risou/qiita-team-alfred-workflow)

1日の空き時間を少しずつ使って Rust を学びながら作って、だいたい1週間ほどで作ったので、実装自体も Rust 歴1週間の人が書いたものと思ってもらえると助かる。  
つまりこのコードの中にはアンチパターンとかバッドプラクティスとかあったりするかもしれない（あれば Issue を立てるなり Twitter 等で直接でも良いので指摘いただけると喜んで直します）。

Rust 自体には以前から興味があったけれど、こういう機会がなければなかなかチャレンジしないので今回は我ながら良い機会を得られたと思う。  
とはいってもまだまだわかっていないことも多く、たとえばテストなども書けていないわけで、学ぶべきことはまだまだたくさんある。  
このあたりはこのツールをアップデートしていくだけでなく、他にも Rust で実装する機会を作っていく必要があるな、と思っているので少しずつチャレンジしていきたい。