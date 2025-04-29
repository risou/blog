title: yabai の制御下でアクティブウィンドウが floating かどうかを可視化する
date: 2025-02-04
datetime: 2025-02-04T13:58:09.356878Z
---

ここ数年、ウィンドウマネージャとして yabai というツールを使っている。  
yabai が本領を発揮するためには System Integrity Protection を無効にする必要があるけれど、プライベートの環境も仕事の環境もなるべく同じに揃えておきたい気持ちがあり、会社から貸与されている Macbook の SIP を無効化しようとは全く思わないため、 SIP を無効化せずに使っている。  
ところが MacOS Sequoia にアップデートして以降、少なくとも SIP が有効な環境ではウィンドウを別のスペースに移動させることができなくなった。

これまではスペースを大量に使って、スペースごとに最大でもウィンドウは2つ、といった運用をしてきたけれど、先の問題と作業環境のディスプレイの配置を変えたことが重なって、ウィンドウの管理方法が変わってきた。

最近は同時に使うスペースの数を少し減らして、代わりに1つのスペースに3つ以上のウィンドウがある状態で yabai の zoom-fullscreen を多用している。  
また 4K 以上の解像度があると、3つ以上あるウィンドウの中から任意の2つを左右に分割したり、 2x2 の4分割にしたりという運用をすることもある。

zoom-fullscreen は yabai 上で managed (= not floating) なままで使うことができるが、2分割や4分割で使うためには floating にする必要がある。すなわち yabai において managed=off にする必要がある。  
（本来的には、そのスペースにウィンドウが2つしかない状態にすることで yabai が勝手に左右に分割してくれる、という状態が望ましいし、4分割においても概ね同様のことが言える）

で floating の ON/OFF を頻繁に切り替えると、ウィンドウのフォーカスを切り替えるときなど、ふとしたときに期待する操作が行えない、ということが発生する。これは floating なウィンドウは yabai に manage されていないことに起因しており、アクティブウィンドウが floating なときに `yabai -m window --focus east` などが使えないため、他のウィンドウの選択が少々面倒になる。

ということでアクティブウィンドウが floating かどうかをいつでもわかるようにしたい、という動機で yabai, skhd, JankyBorders の設定を色々いじってみた。

具体的には、アクティブウィンドウがどれかをわかりやすくする JankyBorders の色情報を切り替えている。 yabai managed なウィンドウであれば緑に、そうでなければ黄色になるように設定した。

そのウィンドウが floating であるかどうかを判定したいタイミングは、大きくわけて以下の2種類になる。

- アクティブウィンドウの floating ON/OFF を切り替えたとき
- ウィンドウのフォーカスを切り替えたとき

前者については、該当するショートカットを実行したときに borders コマンドを叩くようにしている。  
floating の切り替えを行う際はいつも、 toggle floating のショートカットを入力するか、上記の2分割、4分割の状態にするショートカットを入力するため、それらのコマンドに borders コマンドを組み合わせているだけだ。  
一例を示すと、以下のような感じ。

```sh
alt + shift - f : yabai -m window --toggle float; [ "$(yabai -m query --windows --window | jq -r '.["is-floating"]')" = "false" ] && { borders active_color=0xc000ff80; :; } || borders active_color=0xc0ffff00
```
https://github.com/risou/config/blob/8b2f46f7dc3b93118e95690e3e1382ced7d9ba3e/.skhdrc#L61

もう1つのケースがフォーカスを切り替えたときで、この場合は yabai の signal で borders コマンドを発火している。

```sh
yabai -m signal --add event=window_focused action='[ "$(yabai -m query --windows --window | jq -r ".[\"is-floating\"]")" = "false" ] && { borders active_color=0xc000ff80; :; } || borders active_color=0xc0ffff00'
```
https://github.com/risou/config/blob/8b2f46f7dc3b93118e95690e3e1382ced7d9ba3e/.yabairc#L95

これで、常に現在アクティブなウィンドウが floating かどうかが目に見えるようになった。

とはいえ、せっかくタイル型のウィンドウマネージャを用いているのに floating を多用するのはもったいないように感じている。

現代においては AeroSpace という非常に魅力的な選択肢もあり、時間に余裕ができたら試してみようか、という気持ちは少しある。  
思想としては AeroSpace よりも yabai の方が好みなので、可能であればこのまま yabai を使い続けたいが、さて。