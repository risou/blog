title: PuTTY形式の秘密鍵をOpenSSH形式に変換する
date: 2014-01-09
datetime: 2014-01-09 22:11:13.99865 JST
alias: 18

---
Windowsで使えるPuTTYというターミナルエミュレータがある。PuTTYはputtygenという鍵ジェネレータを持っていて、これを用いて普通に鍵を生成するとPuTTY形式の鍵ができあがる。拡張子がppkになっている秘密鍵などがそれだ。



Macからこの鍵を用いてSSH接続するためには、OpenSSH形式に変換する必要がある。Windows環境があればPuTTYをダウンロードしてくればputtygenで秘密鍵をOpenSSH形式に変換することができる。



Macで鍵をPuTTY形式の鍵を変換したい場合、おそらく最も簡単な方法が homebrew で puttygen をインストールすることだ。 puttygen さえ手に入れば、OpenSSH形式に変換できる。



    $ brew install putty

    $ puttygen id_rsa.ppk -O private-openssh -o id_rsa



これで、 `id_rsa` という名前のOpenSSH形式の秘密鍵が手に入る……はずである。



が、実際に試してみたところ、以下のようなエラーがでた。



    Assertion failed: (random_active), function random_byte, file ./../sshrand.c, line 313.



詳細な原因はわからないが、どうやらインストールした puttygen に問題があるようだ。仕方がないので他にMacで使えるように変更された puttygen を探してきた。



[eldridgegreg/puttygen-osx](https://github.com/eldridgegreg/puttygen-osx)



これをダウンロードしてきて、そのまま Xcode で開いて引数を指定して実行(幸いにも .xcodeproj も含まれている)。問題なく `id_rsa` が生成された。
