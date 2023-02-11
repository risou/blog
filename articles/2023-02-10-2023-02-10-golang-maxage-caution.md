title: Golang で Cookie を扱うときは Max-Age の指定方法に注意が必要
date: 2023-02-10
datetime: 2023-02-10T13:30:36.212320Z
---

近年では Chrome が Cookie の有効期限の上限を定めるなど、 Cookie を扱う上で有効期限は軽率にしてはいけないものという認識が広がってきているように思う。  
Cookie の有効期限の指定については歴史的経緯で `Expires` を指定する方法と `Max-Age` を指定する方法の2つがあり、モダンブラウザと呼ばれるものであれば大体は両方指定されている場合は `Max-Age` が優先されるようになっている。

`Max-Age` には **「その Cookie がセットされてから何秒後まで有効か」** を整数で渡すことができるようになっており、よく見るものだと「24時間有効とする `60 * 60 * 24` 」や「1年有効とする `60 * 60 * 24 * 365` 」という値がコード上ではセットされている。  
当然、実際に通信される際の値は計算済みのものだし、ブラウザでは受け取った日時から指定された秒数を足した絶対時間で Cookie を保持している。

`Max-Age` および `Expires` が指定されていない場合、その Cookie はセッション終了時に消えるようになっており、ブラウザが閉じられたタイミングで消える。ただしブラウザがセッションを復元する機能を持っている場合、再度ブラウザが開かれたタイミングで Cookie も復元されることもある。

また `Max-Age` には 0 や負数を設定することもでき、この場合ブラウザ側は有効期限として表現可能な最も古い日時をセットする。当然この表現可能な最も古い日時は過去日時であるため、その Cookie は直ちに破棄される。

さて、ようやく本題に入るが、 Golang で Cookie を扱う際にはおそらく一般的には net/http パッケージの Cookie 構造体を利用すると思う。  
そしてこの Cookie 構造体を用いて Cookie の設定をする際には `Max-Age` の値のセットの仕方に注意が必要だ。

具体的に注意が必要になるのは 0 である。  
先に書いたように Cookie の仕様としての `Max-Age` は以下のようになっている。

- `Max-Age` に正の整数が指定されていれば、 Cookie を受け取った時間から `Max-Age` 秒後にその Cookie は有効ではなくなる
- `Max-Age` に 0 もしくは負の整数が指定されていれば、その Cookie は直ちに破棄される

しかし net/http パッケージの Cookie 構造体が受け取る `MaxAge` フィールドの挙動は若干異なる。  
https://pkg.go.dev/net/http#Cookie には `MaxAge` フィールドについて以下のようにコメントが記載されている。

```golang
	// MaxAge=0 means no 'Max-Age' attribute specified.
	// MaxAge<0 means delete cookie now, equivalently 'Max-Age: 0'
	// MaxAge>0 means Max-Age attribute present and given in seconds
```

つまりこうだ。

- `MaxAge` に正の整数が指定されていれば、 Cookie を受け取った時間から `MaxAge` 秒後にその Cookie は有効ではなくなる（ `Max-Age` にそのまま値が渡される）
- `MaxAge` に負の整数が指定されていれば、その Cookie は直ちに破棄される（ `Max-Age` には 0 が設定される）
- `MaxAge` に 0 が指定されていれば、そのCookieはセッションが閉じられるまで有効である（ `Max-Age` を **指定しない** ）

net/http の Cookie 構造体経由で Cookie を扱うのであれば Cookie 構造体の `MaxAge` とリクエスト／レスポンスで扱われる `Max-Age` の違いを理解しておく必要がある。

| `MaxAge` (Golang) | `Max-Age` (HTTP) | 有効期限 |
| :--: | :--: | :-- |
| N > 0 | N | N 秒後まで |
| N < 0 | 0 | 期限切れ |
| 0 | - | セッションが閉じられるまで |