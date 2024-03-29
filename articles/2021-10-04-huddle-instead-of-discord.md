title: Discord の代替にするために Slack Huddle Meeting に求めたいもの
date: 2021-10-04
datetime: 2021-10-04T20:31:37.123123+09:00
---

実は以前から仕事で Discord を使っている。  
主な用途としてはチームのデイリーミーティングと業務中のカジュアルなコミュニケーションのための常駐だ。

もう結構前になるが Slack が Huddle Meeting という機能を提供すると聞いた時、もしかしたら仕事で Discord に頼っていた部分を Slack に置き換えられるかもしれないと思った。  
管理対象の外部サービスは少ないに越したことはない。そういった事情もあり、既に社内で使われている Slack に統一できるのであれば、それは僕にとっては魅力的な話だった。

しかし実際には、今のところそうなってはいない。  
主な理由は2つ。1つは CPU 占有率の高さ、もう1つは常駐に適さないことだ。

CPU 占有率の高さについては、原因はよくわかっていない。  
普通にミーティングをするのに使う分にはそんなに問題にもなっていない。  
ただ、30分以上会話をするなら、音声のみでも結構重くなり、発熱する。音声のみで Google Meet 　とかと変わらないくらい（あるいはもっと）重くなるし、 Huddle Meeting しながらテキストチャットするともっさりしていると感じることがある。

常駐に適さない点については、より顕著な問題がある。  
長時間接続しっぱなしにしていると、モニタかマシンのスリープのタイミングで切断される。  
Discord の場合はそもそも音声接続中にマシンがスリープしたりはしない。

そもそも Discord に常駐して何をしているのかというと、オフラインで近くで働いていたときにあったようなカジュアルな雑談だったり相談だったりを再現しようとしている。  
一応弁解しておくと、 Discord への常駐はメンバーには強制されていない。  
接続している人も休憩中や話すことがないときは自由にミュートにしているし、作業に集中したいときには自由に切断したりスピーカーミュートにしたりしている。

このやり方がチームで通用しているのは、所属メンバーの多くが以前から音声チャットの文化に慣れているという点が大きい。  
コロナ禍以前から慣れている人が多く、そうでない人も馴染める人は馴染んでくれている、という状況だ。

Discord は非常に軽く、長時間接続していても負荷は重くないし切断もされない。  
しかし Slack の Huddle Meeting には明らかに Discord よりも良い点もある。  
たとえば、画面共有の画質は Slack の方が良い。  
普段は音声のみでやりとりしているが、時々画面を共有したいときがある。そういったとき Discord だと有料プランに加入している人とそうでない人で配信画質が変わってしまう。 Slack でもプランによって違いがあるかもしれないが、会社として契約しているプランでは不満はない。

業務外での Discord の利用をやめたいということは全くなく、今後も利用していきたいと考えているが、業務において Discord の代替を Slack Huddle Meeting あるいは類する何かに任せられる日が来るのを楽しみにしている。
