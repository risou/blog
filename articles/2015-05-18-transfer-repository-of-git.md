title: Git でブランチを整理しつつリポジトリを移行する
date: 2015-05-18
datetime: 2015-05-18 15:46:07.099543 JST
alias: 28

---
使っている管理ツールが変わったりサーバを移行したりする際、Git のリポジトリもあわせて移行する必要が出てくることがある。



最近、移行作業をすることがあったので、そのログを残しておこうと思う。



### やりたいこと



メインはもちろんリポジトリを移行することだが、それに付随してやりたいことがもう1つあった。ブランチの整理である。



### 手順



手順のみを簡単に列挙する。



1. 旧リポジトリから git clone する

1. 旧リポジトリ上の全てのブランチを取得する

1. メインストリームにマージされていないブランチの一覧を取得する

1. 残したい(マージされていない)ブランチ以外を削除する

1. origin の接続先を新リポジトリに設定する

1. 残したい全てのブランチを push する



### 手順の詳細



便宜上、旧リポジトリを `git@github.com:user/old_repo.git` 、新リポジトリを `git@github.com:user/new_repo.git` とする。



#### 1. 旧リポジトリから git clone する



```sh

git clone git@github.com:user/old_repo.git

```



#### 2. 旧リポジトリ上の全てのブランチを取得する



実際に作業したリポジトリでは200ほどのブランチがあったため、ひとつひとつ checkout するのは現実的ではなかったので、先人の知恵を活用させていただいた。



[origin の全てのブランチを取得するスクリプト](https://gist.github.com/mnogu/3844699)



```sh

wget https://gist.githubusercontent.com/mnogu/3844699/raw/072a5266a7b0a3d9a1e6d5d1d02478041c802dc4/git.sh

chmod +x git.sh

./git.sh

```



#### 3. メインストリームにマージされていないブランチの一覧を取得する



`git branch` コマンドには今 checkout しているブランチにマージされているブランチの一覧を表示するオプションがある。同様にマージされていないブランチの一覧を表示するオプションもある。



```sh

git branch --merged // マージ済みのブランチ一覧

git branch --no-merged // 未マージのブランチ一覧

```



ここでは `--no-merged` を使ってマージされていないブランチ一覧を取得する。



```sh

git checkout master

git branch --no-merged > no_merged_branches

```



`master` 以外にもブランチがある場合はそれも追加する。たとえばステージング環境用のブランチ `staging` がある場合は先の処理の後に以下の処理も行うといい。



```sh

git checkout staging

git branch --no-merged >> no_merged_branches

```



この場合、 `no_merged_branches` には未マージのブランチが重複している可能性がある。ソートされていれば `uniq` という手もあるが、ここは awk で処理する。



```sh

awk ''!a[$0]++'' no_merged_branches > no_merged_branches_uniq

rm no_merged_branches

mv no_merged_barnch_uniq no_merged_branches

```



#### 4. 残したいブランチ以外を削除する



`no_merged_branches` にメインストリームのブランチを追加する。このファイルには残したいブランチの一覧が存在することになる。



ファイル内は `git branch` で取得したブランチ名一覧なので、行頭に半角スペースが2つ入っている。これは次の処理のためそのままにしておく。



このファイルを利用して、不要ブランチをローカルで一気に削除する。具体的にはファイルの中身を加工して以下のような形の文字列を作る。



`^  {$branch_1}$|^  {$branch_2}$| ... |^  {$branch_n}$`



`{$branch_x}` には残したいブランチ名を入れる。この文字列を egrep に `-v` オプションをつけてかませることで、消したいブランチの一覧が取得できる。



たとえば `master` `staging` `feature/xyz` `hotfix/007` を残したい場合は以下のような形になる(便宜上、以下の説明ではこの文字列をそのまま使う)。



`^  master$|^  staging$|^  feature/xyz$|^  hotfix/007$`



```sh

git branch | egrep -v ''^  master$|^  staging$|^  feature/xyz$|^  hotfix/007$'' | xargs git branch -D

```



これで全ての不要ブランチをローカルで削除できたことになる。確認は `git branch` で簡単にできる。



```sh

git branch

  develop

  feature/xyz

  hotfix/007

* master

```



#### 5. origin の接続先を新リポジトリに設定する



```sh

git remote set-url origin git@github.com:user/new_repo.git

```



#### 6. 残したい全てのブランチを push する



手元には残したいブランチしか残っていないから、これは簡単だ。



```sh

git push --all

```



----



上記説明には含まれていないがタグも同様にして整理して移行することができる。


