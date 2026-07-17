title: "`git history *` で作られるコミットには署名がつかない"
date: 2026-07-17
datetime: 2026-07-17T05:07:58.253804Z
---

Git 2.54 で `git history reword` , `git history split` コマンドが、 Git 2.55 で `git history fixup` コマンドが追加された。それぞれ以下の作業を置き換えられる。

- `git history reword`
	- `git rebase -i` で `reword` を指定
- `git history split`
	- `git rebase -i` で `edit` を指定
	- `git reset HEAD~` して分割したいコミットの差分を uncommitted にする
	- "`git add -p` して `git commit`" を分割後のコミットの単位で実行
	- `git rebase --continue`
- `git history fixup`
	- `git commit --fixup` して `git rebase --autosquash`

いずれも、対象のコミットハッシュさえ取れればかゆいところに手が届く便利なコマンドである。  
ただし、Git 2.55 時点では3つとも experimental である。

そして、これらのコマンドによって作られたコミットはどうも署名されないようだ。
手元（Git 2.55.0）で試したところ、コミットを再生成する際に `commit.gpgsign` が反映されず、対象コミット以降がすべて未署名になった。  
実装を見ると、コミット時の署名有無の指定をする箇所で NULL が渡されており、署名付きコミットの処理に入ることはなさそうに見える。

https://github.com/git/git/blob/e9019fcafe0040228b8631c30f97ae1adb61bcdc/builtin/history.c#L145-L147

今後のアップデートでこの問題が改善する可能性は十分にあるが、少なくとも現時点ではコミットにちゃんと署名をつけたい場合、 `git history` の利用を控える、もしくは `git history` の実行後に以下のコマンドを実行する必要がある。

```sh
git rebase --force-rebase {署名を付け直したい最も古いコミットの親コミットのハッシュ}
```

`--force-rebase` をつけておかないと no-op になって署名が外れたコミットのままになるので注意。