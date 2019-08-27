title: Git で merge commit を revert したあと、再度 merge したい
date: 2014-05-08
datetime: 2014-05-08 02:15:56.116753 JST
alias: 23
---
Git の merge は、「2つのブランチの共通の親を探し、そこから merge されるブランチのコミットを順に merge するブランチに適用」していくものだ。

では、次のようなときどうするか。

### ケース

![とあるコミットの歴史](http://gyazo.risouf.net/82a17686.png)

上の画像において、それぞれのコミットは以下の通りである。

- S：分岐元のコミット
- M, G：マージコミット
- R：マージコミット（M）の revert コミット
- A, B, C, D：通常のコミット

上記グラフができるまでの流れは、

1. マージを行う（Mコミットが作られる）
1. 先のマージはまだ早かったと気づき、 revert する（Rコミットが作られる）
1. しばらく経ち、マージしても良くなったので再度マージを行う（Gコミットが作られる）

という感じだ。

さて、このときGコミットには、A、B、C、Dの4つのコミットが適用されていて欲しい。  
しかし実際には、CとDの2つだけしか適用されない。

なぜなら、Gのマージコミットの共通はBコミットだからだ。  
そしてBコミットはその後Rコミットで revert されている。

もちろん、CとDの2つだけが適用されれば良い場合は何の問題もない。  
しかし、AとBも含まれてほしい場合はどうすれば良いか。

### 解決方法

Twitter でこの疑問を呟いたところ、 [@kana1](http://twitter.com/kana1)さんに解法を教えていただいた。

<blockquote class="twitter-tweet" data-conversation="none" lang="en"><p><a href="https://twitter.com/risou">@risou</a> <a href="https://t.co/Pq9nfnKbeM">https://t.co/Pq9nfnKbeM</a> まさにこれに解説されてる通りのシチュエーションですね。risouさんの図のRはgit revert -m1 Mで作られたと思います。Dをマージする時はgit revert Rしてからgit merge Dだそうですよ。</p>&mdash; Kana Natsuno (@kana1) <a href="https://twitter.com/kana1/statuses/459705026722549760">April 25, 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

どうやら2008年に同じ問題に遭遇し、解法が提供されるまでの一連のやりとりがあったようだ。

簡単に説明すると、Gのマージを行う前に **revert コミットを revert** すれば良い。  
わかってしまえば簡単なことだった。  
つくづく Git は良くできていると思わされたのだった。

\# そもそもの問題として、より良いブランチの運用方法があるような気はしているが、それについては現場で模索中である。
