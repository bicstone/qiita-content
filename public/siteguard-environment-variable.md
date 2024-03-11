---
title: SiteGuardを導入するとHTTP_ACCEPT_ENCODINGなどのHTTPヘッダが削除されてしまう件
tags:
  - PHP
  - プログラミング
  - HTTP
  - さくらのレンタルサーバ
  - SiteGuard
private: false
updated_at: "2024-03-09T15:55:24+09:00"
id: 3d88fb943d7f08eb3335
organization_url_name: null
slide: false
ignorePublish: false
---

「SiteGuard」を導入にすると一部のHTTPヘッダーがとれなくなります。意外と気付かず条件設定などにおいてハマってしまうことがあります。

:::note warn

※ これは2016-09-28に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## 概要

さくらのレンタルサーバを使用しているサイトにおいてgzip圧縮データを返すようなPHPを作って動作確認していたのですが、いかなる環境で接続しても圧縮されたデータが返されませんでした。「`$_SERVER['HTTP_ACCEPT_ENCODING'];`」の値が常に空だったのです。

他のサーバーにあげたら利用できたのでさくらのレンタルサーバの環境が原因なのかと思っていました。他のサイトにあった記事ではさくらのレンタルサーバ固有の問題であるという書き方がなされていました。しかし、さくらのレンタルサーバで作っているテスト用ドメインでは動作したのです。

…あれ？

「`phpinfo();`」を眺めていて気が付きました。両方のサーバーでHTTPヘッダーに違いがあったのです。

## HTTPヘッダーの違い

「SiteGuard」を有効にしているサイトでは次のHTTPヘッダーが削除されていました。

```plan
HTTP_CONNECTION
HTTP_ACCEPT_ENCODING
```

一方で次のHTTPヘッダーが追加されていました。

```plain
HTTP_VIA = 1.0 ss212.guard.sakura.ne.jp:80 (JP-Secure/siteguard_http_01/520/ss212.guard.sakura.ne.jp)
```

## まとめ

「SiteGuard」によって一部のHTTPヘッダーが削除されていることがわかりました。
