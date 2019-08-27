title: Qiita Team Viewer というアプリを作った
date: 2018-04-16
datetime: 2018-04-16 22:43:19.346896 JST
alias: 40
---
インターネットで生きていると、様々なサービスのアカウントが増えて管理が大変になったりすると思います。あるいはアカウント自体は1つだけど紐づくグループが増えるパターンもあります。

たとえば Google アカウント（G Suite アカウント）とか GitHub の organization とか Slack の team とか Qiita:Team とか、仕事してたり OSS 活動してたりするとうっかり増えちゃいますよね。

本題とは一切関係ないけど、僕はこの現象をアカウント・マルティプライズ(account multiplies)と呼んでいます。

## 複数の所属している Qiita:Team の記事を一覧で見たい

今回は Qiita:Team の話です。紆余曲折あって所属するチームが複数に増えてしまった僕は、わざわざチームごとにブラウザのタブを開いて個別に閲覧することにつらさを感じました。

この手の同一サービス複数アカウントについては、サービスの主旨によって対応が変わるところですが、個人的には Qiita:Team の記事自体は複数のチームのものが混ざって一覧表示されても困らないなと思ったので、そういう使い方ができるアプリを作ることにしました。

## Qiita Team Viewer

作ったものはここから落とせます。

[Releases · risou/qiita-team-viewer](https://github.com/risou/qiita-team-viewer/releases)

使い方は簡単で、起動して（Gatekeeper 周りで警告でるかもしれません） Qiita のアカウントでログインしてアプリの連携を許可してもらえば、アカウントに紐づく Qiita:Team の記事の一覧が画面左側に出てきます。記事を選ぶと画面右側に記事の内容およびコメントが表示されます（その他は README をご覧ください）。

Qiita Team Viewer v0.1.0 では以下のことができます。

- アカウントに紐づく Qiita:Team の記事が読める
    - ただしシングルサインオンでしかログインできない Qiita:Team の記事は読めません（API の制約によるものです）
- 閲覧している記事に絵文字リアクションを追加／削除する
- 閲覧している記事へのコメントに絵文字リアクションを追加／削除する
- 閲覧している記事のページをブラウザで開く

Qiita Team Viewer v0.1.0 ではできないけど、今後のバージョンで以下のことができるようになる予定です（実装の難易度やアプリの方向性によって変更される可能性があります）。

- Qiita:Team を横断した記事検索
- 記事の既読管理
- 記事へのコメント追加

一方で以下のことは今後もできるようにならない予定です。

- Qiita:Team への記事投稿

これは主に、複数ある Qiita:Team で投稿先のチームを間違える危険を避けるためです。

## Qiita Team Viewer の技術選定

ここからは素人がアプリを開発する上で考えたことを記録として残しておこうと思います。

今回アプリを作るにあたって最初に考えたのは、 **兼ねてより興味のあった Electron を使ってみよう** ということです。  
ネイティブアプリがよかったのと、後々 Windows, Linux にも対応したかったということでちょうど良さそうに思いました。 NW.js などの類似の選択肢もありましたが、初めて触るジャンルということもあり現状で一番情報が多そうな Electron にしました。

最初は Electron の簡単なアプリをチュートリアルなどを参考に作り、そのアプリ上で Qiita の API を実行して結果を得るだけのプログラムを書きました。そこから、実際にアプリを作っていく上で必要なことを考えました。

- Qiita の API をどう叩くか
    - こういうのは大抵 API を扱うライブラリがある
    - 公式が提供している [qiita-js](https://github.com/increments/qiita-js) を使うことにした
        - 後に判明したことだが、 qiita-js は Qiita API v2 に追随できておらず、一部の API を叩けなかった
        - qiita-js が対応していない部分だけ温かみのあるリクエストになってしまった 😢
- どの JS フレームワークを使うか
    - フレームワークを使わないのはただの苦行っぽい
    - 使ったことのない React か Vue.js にしようと思って双方を軽く調べて「この時期に始めるなら Vue.js かな」って感じで選んだ（勘です）
- 状態管理
    - Vue.js 使うなら Vuex を使えばいいのかなと安易に選択（他の選択肢がいまいちわからなかった）
- CSS フレームワーク
    - 使うかどうか、ってところからちょっと悩んだけど、自力で CSS 実装するのは素人には現実的ではないと判断
    - **Bootstrap っていうものがあるらしい** 程度の認識で軽く調べて Vue.js と相性良さそうで難しくなさそうな BULMA ってやつを選んだ

あとは「node module に electron-vue ってのがあるのかー」みたいな雑な感じで使ってみたり。

専門職の人たちからすれば最適には程遠い選択だと思いますが、素人の手遊びとしては思ったより順調に試行錯誤しつつ実装が進み、なんとか現状まで持ってこれました。

## 新しく触れた技術要素

Electron を使うことを決めた時点で避けられない道だったのですが、ひとつひとつのことを実現するのに新しい技術要素に触れていくことになって新鮮な経験でした。実際に触れた新しいものは以下のとおり。

- Electron
- Vue.js
- Vuex
- BULMA

どれもこれも全然知らないところからはじめて v0.1.0 をリリースするまで約1ヶ月。バッドプラクティスと言われるような使い方をしている箇所もあるかもしれないけど、なんとか使えるレベルまで持ってこれました。わからないこと多すぎてかなり勉強になった気がする（それをいちいち書き留めておく余裕は残念ながらなかったけれど）。

## その他感想とか

できればテストも書きたかったけれど、特に仕事を休んだりもせず空いた時間で進めるには、試行錯誤しながら動くものを作るので精一杯でした。まあ、テスト自体は後からでも追加できるのでモチベーションが続けば。

今後はドッグフーディングしつつ、検索機能とか追加していければなあと思ってます。けど、他にもやるべきことがあるので、ここから先はゆっくり進めていこうかと。

あと、今回の開発で（僕みたいな人間にとっての） CSS フレームワークの強力さを思い知ったので、ブログのデザインとかアップデートしていきたいですね。

## Qiita Team Viewer を作るにあたって参考にしたサイト

- Qiita
    - [Qiita API v2ドキュメント - Qiita:Developer](https://qiita.com/api/v2/docs)
    - [increments/qiita-js: Qiita API v2 client for browser and node](https://github.com/increments/qiita-js)
- node.js
    - [node.jsでファイルの読み込み、新規作成、上書き、追記、削除 | q-Az](https://q-az.net/node-js-file-read-write-append-unlink/)
    - [Node.jsの例外処理（承前：なぜ例外処理が必要なのか） - まさたか日記](http://mk.hatenablog.com/entry/2013/09/18/003944)
- Electron
    - [ビルド手順 (macOS) | Electron](https://electronjs.org/docs/development/build-instructions-osx)
    - [リリース | Electron](https://electronjs.org/docs/development/releasing)
    - [Introduction · Electron docs gitbook](https://imfly.gitbooks.io/electron-docs-gitbook/jp/)
    - [Electron + Travis で楽々ビルド・リリースする最強のデスクトップアプリ開発 - Qiita](https://qiita.com/oniatsu/items/98851e0463dfe83f04b0)
    - [HTTP×Electronアプリのひな形の作り方から配布まで | Refills](https://syon.github.io/refills/rid/1499505/)
    - [30分で出来る、JavaScript (Electron) でデスクトップアプリを作って配布するまで - Qiita](https://qiita.com/nyanchu/items/15d514d9b9f87e5c0a29)
    - [Electronアプリをプロダクトとして「正しく」リリースするために必要な3つのこと | ヌーラボ](https://nulab-inc.com/ja/blog/typetalk/3-points-for-releasing-production-electron-app/)
    - [Electronでスクロールバーを非表示にする | カナのLinux](https://kana-linux.info/electron/electron%E3%81%A7%E3%82%B9%E3%82%AF%E3%83%AD%E3%83%BC%E3%83%AB%E3%83%90%E3%83%BC%E3%82%92%E9%9D%9E%E8%A1%A8%E7%A4%BA%E3%81%AB%E3%81%99%E3%82%8B)
    - [SimulatedGREG/electron-vue: An Electron & Vue.js quick start boilerplate with vue-cli scaffolding, common Vue plugins, electron-packager/electron-builder, unit/e2e testing, vue-devtools, and webpack.](https://github.com/SimulatedGREG/electron-vue)
    - [導入 · electron-vue](https://simulatedgreg.gitbooks.io/electron-vue/content/ja/)
    - [YuheiNakasaka/vue-twitter-client: Twitter client created with Vue.js + Electron](https://github.com/YuheiNakasaka/vue-twitter-client)
    - [2017年度版 electron-vueで始めるVue.js - Qiita](https://qiita.com/1ntegrale9/items/6e233f03b8cbff0b3c76)
    - [Re:ゼロから始めるElectron開発生活 - Qiita](https://qiita.com/nagisio/items/259bef1748a4db8e0af6)
    - [フロントエンド開発初心者がelectron-vueでアプリをつくってみた　その２～実装編～ - Qiita](https://qiita.com/kurimeg/items/1736ab05dde5d8f8973c)
    - [manosim/gitify: GitHub Notifications on your desktop.](https://github.com/manosim/gitify)
- Vue.js
    - [はじめに — Vue.js](https://jp.vuejs.org/v2/guide/index.html)
    - [やわらかVue.js - Scrapbox](https://scrapbox.io/vue-yawaraka)
- Vuex
    - [Introduction · Vuex](https://vuex.vuejs.org/ja/)
    - [Vue.js と Vuex を使った簡単なメンバー管理フォームを作る | Cubix](http://chibinowa.net/note/vuejs/vue-14.html)
    - [Vue.js の状態管理ライブラリ Vuex のはじめかた -『Vue.js』 – webmanab.html／ウェブまなぶ](https://webmanab-html.com/tip/vue-vuex/)
    - [vuexでデータのstateの初期化するには | カナのLinux](https://kana-linux.info/vue-js/vuex%E3%81%A7%E3%83%87%E3%83%BC%E3%82%BF%E3%81%AEstate%E3%81%AE%E5%88%9D%E6%9C%9F%E5%8C%96%E3%81%99%E3%82%8B%E3%81%AB%E3%81%AF)
    - [Vuex：mapStateの書き方8パターン+11サンプルコード - Qiita](https://qiita.com/suin/items/7331905a45a8ff80d4dd)
    - [vue.js＋Vuexチュートリアル - Qiita](https://qiita.com/_P0cChi_/items/ebf8fbf035b36218a37e)
    - [Vueコンポーネント & Vuex テンプレ - Qiita](https://qiita.com/hirohero/items/771ac34b8213105e2d5f)
- BULMA
    - [Get started with Bulma | Bulma: a modern CSS framework based on Flexbox](https://bulma.io/documentation/overview/start/)


