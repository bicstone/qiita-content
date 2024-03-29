---
title: 「発行先の共有が必要です。」エラーの対処方法
tags:
  - Windows
  - Network
  - samba
  - smb
  - 共有ドライブ
private: false
updated_at: "2024-03-11T21:30:21+09:00"
id: d44645f94592a393978d
organization_url_name: null
slide: false
ignorePublish: false
---

ネットワークの場所の追加をする際に「発行先の共有が必要です。別の場所を試してください。」というエラーが発生した時の対処方法です。

:::note warn

※ これは2016-10-25に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## 現象

「ネットワークの場所の追加」にて「発行先の共有が必要です。別の場所を試してください。」というエラーが発生してしまう。

![「発行元の共有が必要です。別の場所を試してください。」　というエラーが表示されます](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/0efad4d0-6e29-4934-de19-b760f3dfd0a8.png)

## 原因

翻訳がとてもわかりにくいのですが、接続先から認証を求められたので「発行先の共有が必要です」なのです。認証を求められたとしたら「ネットワークの場所の追加」では接続できません。

## 対処方法

「ネットワークドライブの割り当て」を使用します。「別の資格情報を使用して接続する」を選択してください。

1. エクスプローラーの「PC」→「コンピューター」→「ネットワークドライブの割り当て」を選択
   ![エクスプローラーの「PC」→「コンピューター」→「ネットワークドライブの割り当て」を選択](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/b1c55a52-464c-afe8-5f57-5274d6f8c1e4.png)
1. 「割り当てるネットワークフォルダーを選択してください」で「別の資格情報を使用して接続する」をオンにした状態で「完了」を選択
   ![「割り当てるネットワークフォルダーを選択してください」で「別の資格情報を使用して接続する」をオンにした状態で「完了」を選択](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/8f378dba-aac2-ea27-8552-e32f83e18d47.png)
1. Windowsセキュリティ」の「ネットワーク資格情報の入力」でIDとパスワードを登録する
   ![「Windowsセキュリティ」の「ネットワーク資格情報の入力」でIDとパスワードを登録する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/f2087236-4857-ef4f-4dff-5747609a9a1a.png)
1. エクスプローラーPCの「ネットワークの場所」にドライブが追加される
   ![エクスプローラーPCの「ネットワークの場所」にドライブが追加されます。](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/f30e3228-b069-7cd1-0b4d-c737c3bdc05c.png)
