---
title: "「notification.dllが登録されていません」エラーの対処法"
tags:
  - "Windows"
  - "Notification.dll"
  - "Server"
  - "サービス"
  - "ドライバー"
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

Windows Vistaや7などで見られる、「Notification.dll登録されていません。プログラムは正常に動作しません」の対処方法の1つを紹介します。

※ これは2016-10-22に[個人ブログ](https://bicstone.me)で公開した記事を移植したものです。

## 原因

「server」サービスが起動できないために発生します。

## 対処方法

1. Windows+Rキーで「ファイル名を指定して実行」を起動します。
2. 「services.msc」を入力して開きます。
   ![ファイル名を指定して実行で、 `serivces.msc` を起動する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/d8f3a3ad-d112-6ea0-d496-b77a5e990307.png)
3. 「Server」を見つけ、プロパティを開きます。
   ![Serverサービスを選択し、プロパティを開きます](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/979e7d2e-b05f-7a6a-57f7-cf003f870508.png)
4. 「スタートアップの種類」を「自動」にしたあと、「開始」をクリックします。
   ![「スタートアップの種類」を「自動」にしたあと、「開始」をクリックします。](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/b897115e-1470-9212-f03b-61137e8e2f77.png)
5. PCを再起動します。

これ以外にもネットワーク関係のドライバが影響している可能性もあります。ネットワーク関係のドライバー再インストールもお試しください。
