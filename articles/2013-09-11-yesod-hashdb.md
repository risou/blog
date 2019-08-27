title: Yesod で HashDB を用いた認証の実装
date: 2013-09-11
datetime: 2013-09-11 01:38:45.103145 JST
alias: 2
---
基本的には以下のページを見ながら実装すれば良いと思う。

[http://hackage.haskell.org/packages/archive/yesod-auth/1.2.1/doc/html/Yesod-Auth-HashDB.html](http://hackage.haskell.org/packages/archive/yesod-auth/1.2.1/doc/html/Yesod-Auth-HashDB.html)

たとえば、以下のようにテーブルを作っておく。

    User
        user Text
        password Text
        salt Text
        UniqueUser user
        deriving

salt を適用したパスワードのハッシュ値は sha1sum で得ることができるので、これを手動で DB に格納してやればよい。 sqlite3 なら、

    $ sqlite3
    sqlite> insert into user values (1, "username", "salt+password''s hash", "salt");

とする。

ログイン機能は Yesod.Auth が提供してくれているので getAuthR の実装は不要。

あとは、認可が必要な機能の最初で、 maybeAuthId や requireAuthId などを適宜呼んでやればよい。

    getSecretR = do
        req <- requireAuthId

