title: 「マヨイドーロ問題」を Perl 6 で解く（そして挫折へ）
date: 2015-12-17
datetime: 2015-12-17 11:47:12.75823 JST
alias: 31
---
### 「マヨイドーロ問題」とは

[マヨイドーロ問題](hyuki.com/codeiq/#c19)

始点が1つ、終点が2つある経路上に方向転換が可能なポイントがいくつか用意されている。問題となるのは、 **方向転換できる回数の上限** に対して、終点のうちの片方にたどり着く移動ルートは何通りあるか。

### 解き方

この記事の本題ではないため、解き方、考え方について詳細には触れない。

**方向転換できる回数の上限** を _N_ として、 _N_ に小さな値を入れて試していくと、解の規則性が見えてくる。

便宜上、終点の片方に到達する移動ルートの数（解）を _Y_、もう片方に到達する移動ルートの数を _Z_ と記す。

- _N_ を1増やしたとき、偶数→奇数への増加であれば _Y_ のみが増え、奇数→偶数への増加であれば _Z_ のみが増える
- _N_ を増やした時の _Y_ あるいは _Z_ の増加量はフィボナッチ数と関係している

この問題とフィボナッチ数の関連についての詳細は出題者の解説をご覧いただきたい。

### Perl 6 での解答算出プログラム

_N_ から _Y_ を算出する（算出する際にフィボナッチ数を用いる）だけの簡単なプログラムだ。

```
my @fib = (1, 1, *+* ... *);

for $*IN.lines {
	if ($_ == 0) {
		say 0;
	} else {
		my $i = $_ % 2 == 1 ?? $_ + 1 !! $_;
		say @fib[^$i].reduce(*+*);
	}
}
```

解を求める分にはこれで十分である。実際には、より計算量の少ない解法があったが僕は解説を見るまで気づかなかったので、提出したのは上記のコードである。

さて、上記のコードを CodeIQ の回答システムで実行するとどうなったか。

**Time Limit Exceeded** だ。

2015年現在、クリスマスにリリースを控えた Perl 6 は他の言語の実行環境と比較するとまだまだ遅い。とはいえ、自分の手元ではTLEになるほどの時間はかからなかった。そして気づく。

CodeIQ の回答システムで使われている Perl 6 の実行環境は **rakudo-2010.08** であるということに。

5年前の Rakudo ……。僕は知っている。その遅さを。

### そして諦めた

実行環境の制約はどうしようもない。僕は結局 Haskell で書きなおしたコードを提出して、問題をクリアした。

### "より計算量の少ない解法"ではどうだったか

先に書いたが、僕が最初に提出した解法より、出題者の解説に掲載されているロジックの方が計算量が少ない。

試さずにはいられなかった。

    僕「これなら、あるいは……！」
    CodeIQ(rakudo-2010.08)「……TLE」
    僕「ですよねー」

完