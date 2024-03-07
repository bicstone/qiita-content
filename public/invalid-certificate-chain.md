---
title: 「Invalid Certificate Chain」エラーが発生した場合の対処法
tags:
  - SSL
  - TLS
  - さくらのレンタルサーバ
  - 中間証明書
  - 独自SSL
private: false
updated_at: "2024-03-07T23:28:52+09:00"
id: d1b463f8c4792abc9808
organization_url_name: null
slide: false
ignorePublish: false
---

Qualys SSL LabsのSSL Server Testで「Invalid Certificate Chain」エラーが発生した場合の対処法です。 ブラウザでは正常に表示が出来るので戸惑ってしまいました。

https://www.ssllabs.com/ssltest/

※ これは2016-08-26に個人ブログで公開した記事を移植したものです。

## 原因

中間証明書がサーバから送信されていない。

ブラウザではPC自身に中間証明書がインストールされているためエラーが発生しない。

## 対処方法

中間証明書を送信するように設定する。さくらのレンタルサーバの場合、独自SSLの設定から、中間証明書のインストールを必ず行いましょう。

![さくらレンタルサーバーの独自SSLの設定から「中間証明書のインストール」を選択する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/1ea12631-bf8f-0d87-fe64-e79cf897de8e.png)
