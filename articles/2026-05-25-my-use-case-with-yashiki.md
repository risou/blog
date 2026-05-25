title: yashiki で表示するウィンドウを自在に切り替える
date: 2026-05-25
datetime: 2026-05-25T15:54:23.769602Z
---

## タイル型ウィンドウマネージャ yashiki の導入

macOS を利用する際は、かなり前からタイル型ウィンドウマネージャを導入している。

最初に導入したのは Amethyst で、もうだいぶ昔のことなので詳細は覚えていないが、何かしらのアプリケーションとの相性の問題があって、あまり長くは使わなかったと記憶している。

その後は macOS でのタイル型ウィンドウマネージャを諦めていた時期もあったが、併用していた Linux ではタイル型ウィンドウマネージャを使っていたこともあり、良いものがあれば、という気持ちは常に持っていた。

ある時、新しい MacBook のセットアップをしたタイミングで yabai というタイル型ウィンドウマネージャを導入して、それ以降は概ねずっと何かしらのタイル型ウィンドウマネージャを使っている。  
期間的には yabai を最も長く使っていたが、他のものも試したいという気持ちで AeroSpace に切り替えて使っていた時期もある。  
そしてまた今年、新年を迎えたタイミングで新しいタイル型ウィンドウマネージャを試そうという機運になった。

候補は OmniWM と yashiki の2つ。

[BarutSRB/OmniWM: MacOS Niri and Hyprland inspired tiling window manager that's developer signed and notorized (safe for managed enterprise environments). Aiming for parity and extra innovation.](https://github.com/BarutSRB/OmniWM)

[typester/yashiki: macOS tiling window manager](https://github.com/typester/yashiki)

一口にタイル型ウィンドウマネージャといってもいろいろな種類があり、この2つはウィンドウマネージャとしての基本的な考え方が大きく異なっている。  
ここではタイル型ウィンドウマネージャの歴史や系譜については触れないが、今回は両方を実際にインストールして試した。  
少しの時間ではあるが実際に触ってみて、現在の自分の使い方に合っているのは yashiki だと判断し、今は yashiki を常用している。

## 自分の基本的な使い方

yashiki がどのようなものかはリポジトリを見に行ってもらうのが速いと思うが、自分の使い方を一部紹介する。

前提として、デフォルトのレイアウトは tatami を採用している。

まずは README に書かれている設定にもあるが macOS の Spaces を再現したいので、 `ctrl-1` 〜 `ctrl-9` でそれぞれ tag N を表示するようにしているが、 tag N はそれぞれ bitmask 2^(N-1) を表す。  
それぞれの tag にアプリケーションを割り当てており、上記のホットキーでスムーズに切り替えられるようにしている。

yashiki （や yashiki が参考にしている river）が面白いのは tag を bitmask として表現しているところだ。  
先にも書いたが、例えば `ctrl-3` は tag 3 に切り替えるが、これは bitmask 4(`binary: 100`)に相当する。  
逆に bitmask 3(`binary: 011`)は tag 1 と tag 2 の和集合であり、これを表示すると tag 1 に割り当てられたウィンドウと tag 2 に割り当てられたウィンドウの両方が表示される。

これを活かすため、自分はさらに `ctrl-shift-1` 〜 `ctrl-shift-9` に 2^(N-1) を tag-toggle する設定を追加している。  
yashiki tag-toggle コマンドは、指定した bitmask と現在表示している bitmask の XOR をとる。  
これによって、たとえば `ctrl-2` を押して tag 2 (= bitmask 2) を表示している状態で、 `ctrl-shift-5` を押すと bitmask 18 を表示している状態になる。  
これはつまり tag 2 (= bitmask 2) と tag 5 (= bitmask 16) を同時に表示している。  
tag-toggle コマンドは XOR をとった結果が 0 になる場合は何もしないので、表示する tag がない状態にはならない。

最初に tag N にそれぞれ1つ（ないしは2つくらいまでの）ウィンドウを割り当てて置くことで、1つのウィンドウを画面全体に表示したい場合も、2つのウィンドウを左右に分割したい場合も簡単に切り替えることができる。

また tatami は master-stack 式のレイアウトだが、 master 側（画面の左側）に置くウィンドウの数を増減できるので、 4K ディスプレイなどでウィンドウを 2x2 に配置することも容易にできる。

## yashiki を紹介するトークをしてきた

[湘.なんか #4 - connpass](https://shonanka.connpass.com/event/389138/)

先日、「なんか」を話す会に参加してきた。  
何を話そうか、というところで最近自分の中でホットな yashiki の話をしてきたという次第。

<script async class="docswell-embed" src="https://www.docswell.com/assets/libs/docswell-embed/docswell-embed.min.js" data-src="https://www.docswell.com/slide/ZR868J/embed" data-aspect="0.5625"></script><div class="docswell-link"><a href="https://www.docswell.com/s/risou/ZR868J-2026-05-25-yashiki-introduction">macOS のタイル型ウィンドウマネージャ yashiki の紹介 by @risou</a></div>

自分はそれなりにタイル型ウィンドウマネージャを使ってきているけど、がっつり使いこなしているというよりは、浅瀬で基本的な機能だけを使っているという感じなので、他の人がどう使っているかを知りたい。  
そのためにもまず、使ってくれる人が増えたらいいなあ、という思いがあったのでちょうどよい場が得られてよかった。  
