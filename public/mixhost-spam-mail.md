---
title: mixhostでスパムメールが大量に送られるときの対処法
tags:
  - mail
  - レンタルサーバー
  - スパム
  - cPanel
  - mixhost
private: false
updated_at: "2024-03-18T23:53:16+09:00"
id: 4412f2d4aa0e74fd7ceb
organization_url_name: null
slide: false
ignorePublish: false
---

MixHost初期設定ではドメインに来たすべてのメールを捕獲してしまうためスパムメールが大量に送られてくる原因となっています。存在しないユーザーへのメールアドレスでは捕獲しないように設定変更する方法を紹介します。

:::note warn

※ これは2018-01-02に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## スパムメールのほとんどがローカル部がランダム

mixhostのデフォルトメールアドレスには、登録したドメインに送られたすべてのメールが受信されてしまいます。次のスクリーンショットのように、ローカル部がランダムでありながらもすべて受信しています。

![webmaster@ や help@ など適当なユーザー部に向けてメールが送信され、すべて受信してしまう](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/09208432-5c5f-ccf6-a972-60db3753972b.png)

## 存在しないアカウントへのメールは受信しない

MixHostにてログイン →cPanelにログインより、電子メール → 規定のアドレスを開きます。

![MixHostにてログイン→cPanelにログインより、電子メール→規定のアドレスを開きます。](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/941f91db-1007-b3b7-dd11-c2c4889b251e.png)

すべてのドメインについて、「システム アカウント に転送します」から、「サーバーで処理が行われていても電子メールをSMTP時間までに破棄し、エラー メッセージを生成します。」に変更して保存します。

![「システム アカウント に転送します」から、「サーバーで処理が行われていても、電子メールを SMTP 時間までに破棄し、エラー メッセージを生成します。」に変更して保存します。](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/be27f2b9-8540-b69a-b5b9-60324d35adf4.png)

あまりにも件数が多く、送信数制限にひっかかる場合は、「廃棄」を選択します。

![件数が多く、送信数制限にひっかかる場合は、「廃棄」を選択します。](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/c0872361-677c-4c83-c421-8ff54941e1ad.png)

この作業によって、アカウントのないメールアドレスへの受信が止まります。
