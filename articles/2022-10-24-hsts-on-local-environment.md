title: Strict-Transport-Security がローカルの開発環境にどう影響するか
date: 2022-10-24
datetime: 2022-10-24T12:35:18.616981Z
---

## Strict Transport Security とは

[RFC 6797: HTTP Strict Transport Security (HSTS)](https://www.rfc-editor.org/rfc/rfc6797)

Strict Transport Security とは Web サイトへの接続の安全性を高めるための手法のひとつとして策定されている仕組みである。

Strict Transport Security が設定されている Web サイトへの接続は、 たとえ http でのアクセスであったとしても https を用いて行われるようになっている。

## Strict Transport Security の仕組み

当然、勝手にそのようになってくれるわけではなく、これを実現するための前提となる仕組みが存在する。

サーバ側は HTTPS でリクエストしてもらうために、 Strict Transport Security の対象であることを明示する必要がある。  
具体的にはレスポンスヘッダに Strict-Transport-Security フィールドを付与し、適切な値を添えて返却することで「次回以降、このドメインには HTTPS でアクセスしてください」という表明を行う。

これを受けてクライアント側（主にブラウザ）はレスポンスヘッダに Strict-Transport-Security フィールドを見つけると、その内容に応じて次回以降のアクセスを HTTPS に切り替えてくれる。

大まかな流れとしては以上だが、 Strict Transport Security を利用するにあたっては、もう少し詳細に知っておくべきことがいくつかある。

## Strict Transport Security を深く知る

### 初回アクセスから HTTPS でのリクエストを求める

Strict Transport Security が実現したいのは Web サイトへの HTTPS でのアクセスだ。  
しかしサーバ側がこれを指定しているかどうかは一度アクセスしないとわからない。  
つまりどう頑張っても初回のリクエストが HTTP で行われた場合にはその恩恵を受けられない。

この問題を解消する方法として HSTS preload という仕組みがある。  

[HSTS Preload List Submission](https://hstspreload.org/)

ここで管理されている HSTS preload list に入っているドメインについては、ブラウザがこのリストを参照することで初回アクセスから HTTPS を強制してくれる。

### HTTP リクエストのレスポンスに付与された Strict-Transport-Security フィールドは参照されない

初回のリクエストが HTTP で行われる問題については HSTS preload によって解消されるが、もし HSTS preload list に含まれていないドメインに対して HTTP でリクエストが行われた場合、そのドメインへのアクセスは次から HTTPS に切り替えられるか。

実は、答えは No である。

なぜなら、ブラウザは HTTP でリクエストして得られたレスポンスに付与されている Strict-Transport-Security フィールドを無視するからだ。  
これは「そもそもその HTTP リクエスト、ひいてはそのレスポンスが安全であることが保証できない」ためである。  
要するに HTTPS でリクエストしない限り、「次回以降 HTTPS でリクエストする」設定にはならないようにできている。  
だからこそ HSTS preload の設定が重要であるし、サーバ側は HTTP でリクエストを受けた場合にリダイレクトして HTTPS でのアクセスに切り替えるように設定しておく必要がある。

## Strict Transport Security の設定ができているサイトをローカルで動かす場合

開発している Web サービスにおいて Strict Transport Security を設定したい場合、ローカルでの開発環境についても HTTPS を扱えるように証明書等を準備する必要がある、とは限らない。  
というのも先に述べたとおり Strict Transport Security が有効になるためには HTTPS でアクセスされる必要があるからだ。

たとえば手元で `http://localhost/` などにアクセスしてサービスの挙動を確認できるようにしているのであれば、サービス上のどこかで `https://localhost/` へのアクセスに切り替わる導線でもなければ Strict Transport Security が有効になることはない。

したがって、環境ごとに Strict-Transport-Security フィールドをセットするかどうかを分岐させる必要はない。

## ローカルで Strict Transport Security が機能して HTTP リクエストできない場合

人によっては `http://localhost/` にアクセスしたら `https://localhost/` にリダイレクトされる、という人もいるかもしれない。  
そうなっている場合、疑うべきは「ブラウザがキャッシュしている HSTS のリストに `localhost` が含まれている」可能性だ。

複数のサービスを開発していれば、ローカルでも証明書を使って HTTPS 化していることもある。  
そのサービスが Strict Transport Security を設定していれば、ブラウザは `localhost` を HSTS のリストに入れているかもしれない。

こういった場合の対処法としては `/etc/hosts` などで 127.0.0.1 に紐づけるドメインを増やしておくなどがある。  
当然ブラウザのキャッシュを削除することで HSTS のリストから外すという選択肢もあるが、そもそも HSTS を使っているサービスを手元で開発し続けるのであれば現実的ではない。
