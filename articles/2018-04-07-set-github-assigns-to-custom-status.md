title: GitHub で assign されている Issue の件数を Slack の custom_status に表示する
date: 2018-04-07
datetime: 2018-04-07 00:13:53.002221 JST
alias: 39
---
元ネタというか、インスパイアされたのは以下の記事。

[「おまえは今までレビューしたプルリクの数をおぼえているのか？」 - pixiv inside](https://inside.pixiv.blog/kana/3486)

@kana1 さんの上記の記事では、依頼されているレビューの件数を表示することで、同僚にどれくらい忙しいか（レビュータスクが溜まっているか）を知ってもらって、レビュー依頼が集中しないようにしている。

Slack の custom_status は前々から有効活用できそうだなーと思っていたけど、このアイデアはとても良いものだと思ったので自分に合う形にカスタマイズしてみた。

僕の今の働き方だと、

- 複数の GitHub organization に所属している
- 複数の Slack Team に所属している
- レビュー依頼は Slack bot が通知してくれたり、デイリーでレビュー依頼件数を教えてくれるチャンネルがある
- GitHub の Issue に対してマネージャが assign してくれたり、自分で Issue を流し見たりして良さそうなものを self-assign する

という感じなので、レビューの数ではなく Issue の数を可視化することにした。

## どこでどうやって動かすか

@kana1 さんの用途では、 Slack のチャンネルを監視して custom_status に対して get/set を行っている。  
僕の用途ではチャンネルの監視は必要なくて、リアルタイムじゃなくて良いので定期的に Issue の件数を数えてもらえればそれで良い。

さらに、これだけのためにサーバを用意するのとかはとても面倒だったので Google Apps Script で全部やることにした。  
GAS では一定時間ごとに実行するトリガーを設定できるため、定期実行は GAS に任せることができる。

## GitHub から assigned Issue の件数を取得する

僕は複数の organization に所属していて、それぞれに対応する Slack Team に所属しているので、「ある organization のリポジトリに紐づく assigned Issues の件数を、対応する Slack Team の custom_status に設定」することが目標になる。

GitHub の API では以下の URL で目的の情報が取得できる。

```
https://api.github.com/orgs/{:org}/issues
```

特にクエリパラメータを設定しなくても、初期状態で self-assigned かつ open な Issue のリストが取れるためこれで良い。

当然 API を叩くために token が必要。 scope はそんなに広くなくてよくて、 repo だけチェックしておけばよい。

GAS で API を叩く際には UrlFetchApp.fetch を使えば良いらしい（このあたり node.js のモジュールとかそのまま使えないのかな？）。

## Slack の custom_status を設定する

assigned Issues の件数がとれたら、件数に合わせて custom_status を設定したい。

基本方針は @kana1 さんのものと同じく10件までは件数を表示して、10件を超えたら溢れている感のある絵文字を表示すれば良さそう。

Slack の API はチームごとに token が取れるので、 GitHub organization に対応するチームの token を用意しておく。  
Slack の API はクエリパラメータに token とセットしたい内容を与えてリクエストを送れば良い。  
今回のケースでは以下のような URL を叩くことになる。

```
https://slack.com/api/users.profile.set?token={:token}&profile={:data}
```

`data` の部分にはセットしたい値を URL エンコードされた JSON を与える。以下のような形になる。

```json
{
    status_emoji: emoji,
    status_text: text
}
```

## 定期的に実行する

GAS はトリガーを設定できる。僕の場合は15分ごとに実行するように設定している。

これだけで15分毎に assigned Issues の件数を Slack の custom_status に設定できる。
