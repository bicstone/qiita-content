---
title: VBでシリアルポート通信した時にフリーズしてしまう対処方法
tags:
  - Windows
  - プログラミング
  - serialport
  - VB
  - バッファオーバーフロー
private: false
updated_at: "2024-03-09T15:55:24+09:00"
id: e5e7bcc1add4bb86ce36
organization_url_name: null
slide: false
ignorePublish: false
---

VBでシリアルポート通信した時にフリーズしてしまう場合の対処方法の1つを紹介します。

※ これは2016-10-29に[個人ブログ](https://bicstone.me)で公開した記事を移植したものです。

## 現象

VB(.net)で作ったプログラムにおいてシリアル通信しようとしたのですが、不定期なタイミングでフリーズ、しかもデバッグツールまでフリーズするという現象が起こりました。

## 対処方法

バッファオーバーフローが原因なので、シリアル通信のプロパティより、バッファを大きくします。「ReadBufferSize」、「WriteBufferSize」です。10MBに設定しました。

![sSerialPort の ReadBufferSize と WriteBufferSize の値を大きくする( `10000000` など)](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/e7561cf8-ad1d-8e8f-9665-fe008d1ea982.png)
