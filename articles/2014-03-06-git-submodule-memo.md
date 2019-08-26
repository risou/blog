title: git submodule覚え書き
date: 2014-03-06
datetime: 2014-03-06 12:50:52.252598 JST
alias: 20

---
`git submodule`を使っていると、「あれ、これどうしたらいいんだ……？」と悩むシーンが結構あるので、自分のために記録する。



`git version 1.8.5.2`で確認した。



*もし、この記事をご覧になるような奇特な方がいて、内容の間違い等に気づかれたら、 [@risou](http://twitter.com/risou) に教えてくれると幸いです。*



### `git submodule` が使われているリポジトリを clone したけど中身がない



`git clone` しただけでは、submodule の中身はとれていないので、中身を取得する。



```

git submodule update --init

```



### submodule の中で submodule が使われている



リポジトリBを`git submodule add`しているリポジトリAを clone 。  

ちゃんと `git submodule update --init` したけど、上手く動かない。  

よく見たらリポジトリBがリポジトリCを`git submodule add`してた。



リポジトリBのルートディレクトリに移動して`git submodule update --init`でも良いけれど、  

リポジトリAからまとめてとりたい場合は以下のようにする。



```

git submodule update --init --recursive

```



### submodule を削除したい



submodule の削除は以前から面倒で、最近のバージョンでは少し改善されているものの、やはりまだ面倒。



submodule に関する情報は4つある。



- .git/config の submodule セクション

- .gitmodules の submodule セクション

- submodule のディレクトリ

- .git/modules にある submodule のディレクトリ



このうち、`git submodule deinit`で消せるのは1つ目だけのようだ。  

2つ目は`git config -f --remove-section`で消せる。  

残念ながら後は直接消すしかない。



まとめると以下のようになる。  

`<submodule>` は submodule のパスを示すものとする。



```

git submodule deinit <submodule>

git config -f .gitmodules --remove-section submodule.<submodule>

git rm --cached <submodule>

rm -r <submodule>

rm -r .git/modules/<submodule>

```



3行目と4行目は、まとめて`git rm -f <submodule>`でも良い。



### submodule したリポジトリの中身を最新にしたい



submodule もリポジトリなので、対象のルートディレクトリに移動して、  

`git pull`すれば最新を取得できる。



が、複数のリポジトリに対してまとめてやりたい場合は以下のようにすると楽。



```

git submodule foreach git pull

```



### submodule したリポジトリの中身を間違えて更新してしまった



試しに使ってる [Atom](http://atom.io/) でファイル内で replace All しようとして、  

間違えてリポジトリの全てのファイルに replace All してしまった。



この時、 submodule の中身も replace されてしまっているので戻す必要がある。  

ちなみに Atom でのこの手の undo のやり方誰か教えてください。  

（普通に undo するとアクティブなファイルでの最後の変更だけが戻る）



これも上記と同様にすれば早い。



```

git submodule foreach git reset --hard

```
