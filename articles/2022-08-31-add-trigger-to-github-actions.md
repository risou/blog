title: Lithograph を GitHub で使いやすいようにトリガーを追加した
date: 2022-08-31
datetime: 2022-08-31T03:10:57.975102Z+09:00
---

このブログのシステムである Lithograph は元々ローカルでの利用を想定して、新しい記事を作成するコマンドなどを実装していた。

しかし昨今では想定していた主なホスティング先である GitHub でできることが増えており、現在では [直接 Visual Studio Code を起動してブラウザ上で記事を書く](/entry/2021-09-07-switch-github-actions.html) 使い方に自分自身が慣れてしまった。

そこで GitHub Actions の手動実行機能を用いて、記事ファイル作成コマンドを実行するように変更した。  
使い方は簡単で、ブログのリポジトリで Actions のページに行って、対象の Workflow を選択してボタンを押すだけ。  
すると、ファイル名の入力が求められるのでそれだけ入力すれば新しい記事ファイルを作成してコミットを積んでくれる。  
その後は Visual Studio Code に移って当該ファイルを編集すれば良い。

これで Lithograph 自体の機能を活用しつつブラウザ上でブログを書くことを完結できるようになった。

しかし1点気になることがある。  
それは GitHub Actions でコマンド実行した際のタイムゾーンが UTC であることだ。  
Lithograph の記事作成コマンドは実行された日時をファイル内に出力するし、ファイル名にも日付が使われる。  
これが UTC であること自体は問題ないのだが、これによって記事の作成日時が表記上はずれてしまう可能性がある。  
簡単に言えば、日が変わってから朝9時までに記事作成コマンドを実行すると、前日の記事として生成されてしまう。  
（もちろん、この記事も手動で2022年8月31日の記事という形に修正したが、記事ファイルが生成された時点では2022年8月30日の記事ということになっていた）

これについては、そもそも Lithograph がタイムゾーンを考慮していないことが問題なので、後日修正したい。