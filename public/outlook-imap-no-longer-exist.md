---
title: 「Some of the requested messages no longer exist」エラーの対処法
tags:
  - mail
  - error
  - imap
  - thunderbird
  - Outlook
private: false
updated_at: "2024-03-19T00:04:35+09:00"
id: 6c832e000bc15befe66c
organization_url_name: null
slide: false
ignorePublish: false
---

Outlook.comなどでIMAP通信すると「Some of the requested messages no longer exist」エラー発生することがあります。その対処方法です。

:::note warn

※ これは2016-10-29に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## 原因

![Mozilla Thunderbird の エラー 「アカウントのメールサーバーからの応答:Some of the requested messages no longer exist」](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/0c093f61-ed89-f52d-0735-4ff3ad231596.png)

メーラーによる同時アクセス数が多すぎことによるoutlook.comから接続規制によって発生します。

## 対処方法

- 「アカウント設定」→「サーバー設定」→「詳細...」→「サーバへの最大同時接続数」を1にする(それぞれのアカウントで行う必要があります)。
  ![サーバー設定→詳細→サーバーへの最大同時接続数を「1」にする](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/317b504e-9f19-1137-d8bb-ee8163870b5b.png)
- ゴミ箱移動、アーカイブなどの処理をボタン連打で1メールずつやるのではなく、シフトキーやコントロールキーを押しながら複数選択した後にまとめて処理を行うようにする。
