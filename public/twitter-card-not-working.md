---
title: "TwitterのOGPカードが表示されないときの対処法"
tags:
  - x
  - Twitter
  - OGP
  - SEO
  - SNS
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

Twitterカード用のタグを追加しているのにTwitterでTwitterカードが表示されないときの対処法を紹介します。

:::note warn

※ これは2017-07-14に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## Twitter カードが表示されない

Twitterカードが挿入されるWordPressサイトのリンクをツイートしたのですが、何故か特定のページに限りTwitterカードが表示されません。

![Tweetの例、カードが表示されていない](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/eadd836c-657f-1cc0-69db-eb9a52ec8477.png)

同サイトの他のページへのリンクではTwitterカードが表示されました。タグの記載ミスも考えにくいです。

![同じサイトの別のページではカードが表示されている](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/07a84062-52a6-b4f7-8492-05ccf1cb2631.png)

## 原因を調べる

[Twitter の公式ドキュメント](https://developer.twitter.com/en/docs/tweets/optimize-with-cards/guides/troubleshooting-cards)を参照してます。その記載によると「[Card validator](https://cards-dev.twitter.com/validator)」なる検証ツールがあり、Twitterによるアクセスログが見られるようです。

https://cards-dev.twitter.com/validator

先程の問題のURLを開いてみます。

![`Unable to render Card preview` のエラーとLogが表示される](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/51a27651-12be-c3bb-2cb2-3ee1a670c92b.png)

> ERROR: Fetching the page failed because the response is too large.

というエラーログが確認できました。つまり問題のページはページサイズが大きすぎて取得できなかった。ということになります。

「Card validator」にてURLを入力してみることでTwitterカードが表示されない原因を特定できます。

また、Twitterカードに古い情報が表示されている場合も、「Card validator」にて入力して送信するとキャッシュを頼らずに再取得してくれます(他のツイートのカードも更新されます)。
