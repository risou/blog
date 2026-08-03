title: Git のコミットメッセージってどれくらいの情報量を載せられるんだろう
date: 2026-08-03
datetime: 2026-08-03T15:37:50.055005Z
---

というのをちょっとだけ調べて、　[湘なんか #5 - connpass](https://shonanka.connpass.com/event/394970/) という集まりで話してきた。

<script async class="docswell-embed" src="https://www.docswell.com/assets/libs/docswell-embed/docswell-embed.min.js" data-src="https://www.docswell.com/slide/K3JLXV/embed" data-aspect="0.5625"></script><div class="docswell-link"><a href="https://www.docswell.com/s/risou/K3JLXV-2026-08-03-git-commit-message-size">git のコミットメッセージはどこまで大きくできるのか by @risou</a></div>

要約すると：
- Git はファイルサイズや文字数の制限を設けていない
- だいたい 64KB くらいにおさめると安全
	- 日本語文字なら20,000文字くらい
	- GitHub のコミット詳細に全文表示される
	- JetBrains の Git クライアントでも全文表示される
	- lazygit でも1秒かからなず表示される（当然これはスペック依存ではある）

----

それはそうと、湘なんかという集まりに参加するのは2回目なのだけど、非常に良い空気を纏っていてとても楽しめた。  
その地域に対して好意的な人たちが集まっていることもあるし、トークや LT の内容も多種多様でどれも面白い。  
（実は参加した2回とも、前にスケジュールが詰まっていて、最初から参加できてはいないのだけど……）  
こういうコミュニティが参加できる距離にあるということに幸運を感じている。  
今後も参加できるときは参加したいし、こういう場で話せるようなネタも仕入れておきたい。
