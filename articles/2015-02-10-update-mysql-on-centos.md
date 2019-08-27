title: CentOS 6.4 で MySQL を 5.1 から 5.6 に上げる
date: 2015-02-10
datetime: 2015-02-10 21:54:00.26542 JST
alias: 27

---
まずはいらないものを削除する。

```sh
$ sudo yum remove mysql*
```

次に RPM ファイルを取得する。

```sh
$ wget http://dev.mysql.com/get/Downloads/MySQL-5.6/MySQL-client-5.6.20-1.el6.x86_64.rpm
$ wget http://dev.mysql.com/get/Downloads/MySQL-5.6/MySQL-shared-compat-5.6.20-1.el6.x86_64.rpm
$ wget http://dev.mysql.com/get/Downloads/MySQL-5.6/MySQL-server-5.6.20-1.el6.x86_64.rpm
$ wget http://dev.mysql.com/get/Downloads/MySQL-5.6/MySQL-devel-5.6.20-1.el6.x86_64.rpm
$ wget http://dev.mysql.com/get/Downloads/MySQL-5.6/MySQL-shared-5.6.20-1.el6.x86_64.rpm
```

その後、これらをインストールするのだが、 MySQL-shared のみこの時点でインストールしようとすると依存性の問題でエラーが出る。そのため、他のものを先にインストールする。

```sh
$ sudo yum install MySQL-client-5.6.20-1.el6.x86_64.rpm
$ sudo yum install MySQL-shared-compat-5.6.20-1.el6.x86_64.rpm
$ sudo yum install MySQL-server-5.6.20-1.el6.x86_64.rpm
$ sudo yum install MySQL-devel-5.6.20-1.el6.x86_64.rpm
```

ちなみに僕が試したときは、 MySQL-server のインストールで以下のエラーが出た。

```
エラー: %pre(MySQL-server-5.6.20-1.el6) スクリプトの実行に失敗しました。終了ステータス 1
エラー:   install: スクリプト %pre の実行に失敗しました (2)。MySQL-server-5.6.20-1.el6 をスキップします。
```

これは古いバージョンの MySQL-server がアンインストールできていなかったことが原因だった。これはおそらく、最初の `yum remove` をする前にきちんと MySQL のサービスを終了していなかったのだろう。

上記4つが無事にインストールできたら、最後に MySQL-shared をインストールする。

```sh
$ sudo yum install MySQL-shared-5.6.20-1.el6.x86_64.rpm
```

これでインストールは完了だ。当然、バージョンが上がっていること、きちんと起動することを確認する必要がある。

```sh
$ mysql --version
$ service mysql start
```

僕が試したときはここで起動に失敗した。 `/var/log/mysqld.log` を確認したところ、以下のようなログがあった。

```
[ERROR] InnoDB: auto-extending data file ./ibdata1 is of a different size 640 pages (rounded down to MB) than specified in the .cnf file: initial 768 pages, max 0 (relevant if non-zero) pages!
[ERROR] InnoDB: Could not open or create the system tablespace. If you tried to add new data files to the system tablespace, and it failed here, you should now edit innodb_data_file_path in my.cnf back to what it was, and remove the new ibdata files InnoDB created in this failed attempt. InnoDB only wrote those files full of zeros, but did not yet use them in any way. But be careful: do not remove old data files which contain your precious data!
[ERROR] Plugin ''InnoDB'' init function returned error.
[ERROR] Plugin ''InnoDB'' registration as a STORAGE ENGINE failed.
[ERROR] /usr/sbin/mysqld: unknown variable ''default-character-set=utf8''
```

`ibdata1` のサイズが一致しない、というエラーは過去の MySQL のデータが残っていることが原因だ。対象のファイルを削除すれば解消する。

```sh
$ cd /var/lib/mysql
$ sudo rm ib*
```

`default-character-set=utf8` が unknown だ、というエラーはそのままの意味で、 `/etc/my.cnf` にその設定あるけど、それ 5.6 じゃ使えないから、ということだ。たとえば server 側であれば、 `character-set-server=utf8` に変更すれば解決する。

以上で、 MySQL 5.6 が動作するところまで確認できた。

あと、ユーザ関連(権限含む)の設定とかでエラーが出ると思うので、以下のコマンドを実行しておくと良い。

```sh
$ mysql_upgrade -u root -p
```

