---
title: 署名なしのドライバーをインストールする方法 (GW-USH300N を Windows10 で動作させてみた)
tags:
  - Windows
  - Network
  - Windows10
  - ドライバー
  - ドライバー署名
private: false
updated_at: "2024-03-11T21:30:21+09:00"
id: d64592e98a0e5fe47ce8
organization_url_name: null
slide: false
ignorePublish: false
---

2010年9月に発売されたGW-USH300Nを試行錯誤の中、Windows10で動作させる事ができたので紹介します。

Windows 8やWindows 8.1でも同様の方法で動作するはずです。

追記:Windows 10 Anniversary Update及びCreators Updateした際に再インストールが必要になる可能性があるそうです。その際もこちらの方法で再インストールできます。

:::note warn

※ これは2016-03-29に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## GW-USH300N はそのままでは Windows 10 で動作しない

GW-USH300Nの動作対象はWindows 7（32/64bit）/ Vista（32/64bit）/ XP（32bit）に限られています。

GW-USH300Nのドライバーは「ドライバー署名」がないため、セキュリティが強化されたWindows 8の64ビット及びWindows 10ではドライバーのインストールが不可能なのです。

## ダウンロードする

先にGW-USH300Nのドライバーを[公式サイト](https://www.planex.co.jp/support/download/wireless/gw-ush300n.shtml)からダウンロードします。

この後の再起動後、ネットワークに繋げなくなる可能性があるためです。

## インストールする

一時的にOSのセキュリティ機能を一部無効にしてインストールします。再起動したらまたセキュリティ機能が戻りますのでご安心ください。

### シフトキーを押しながら再起動する

**シフトキーを押しながら**再起動をします。

![シフトキーを押しながら再起動を選択する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/a9d6a0a4-099b-513a-0f53-585c58987e0c.png)

### 再起動時にオプションを設定する

トラブルシューティング → 詳細オプション → スタートアップ設定 → 再起動をクリックします。

![「オプションの選択」で、「トラブルシューティング」を選択する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/efd7a570-88b2-821f-aef2-d9a8efbe1053.jpeg)

![「トラブルシューティング」で、「詳細オプション」を選択する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/02e9fabf-7df1-776c-8091-cb766a891cdf.jpeg)

![「詳細オプション」で、「スタートアップ設定」を選択する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/715de3a6-f4a6-dcfd-14eb-a5b72e787bfc.jpeg)

再起動後に次の画面が出るので「ドライバー署名の強制を無効にする」つまりキーボードの7を入力します。

![スタートアップ設定で「(7)ドライバー署名の強制を無効化する」を選択する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/acade8e8-175a-3cde-1133-afd3f9f0f923.png)

### 互換性の設定をする

インストールソフトの「Setup.exe」を右クリック → プロパティから「互換性」より「互換モードでこのプログラムを実行する」にて「Windows 7」に、「設定」にて「管理者としてこのプログラムを実行する」と設定します。

![Setup.exeのプロパティで、「管理者としてこのプログラムを実行する」を選択する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/e3cc0adb-dc26-0e76-54b5-b8cb4cc3c69f.png)

### ドライバーをインストールする

「Setup.exe」を起動してインストールします。インストール中「ドライバーソフトウェアの発行元を検証できません」と警告が出ます。「このドライバーソフトウェアをインストールします」を選択します。その後、再起動をしたらインストール終了です。

![「ハードウェアを検索し、ドライバをインストールしています PCI GW-USH300N Driver」](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/27cd9e4a-8532-4779-3f79-cac1cb57d620.png)

![Windows セキュリティより「ドライバー　ソフトウェアの発行元を検証できません」警告の表示](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/1547b857-b6d5-aa99-7698-f4aecc786286.png)

### 再起動する

「完了」をクリックして再起動すると接続できるようになります。

![「Install Shield Wizard の完了」](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/7ca24842-a708-546b-f658-37dfda3840c6.png)

![PCI GW-USH300N Utility](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/b653dc76-2850-7472-2ec1-002744e00835.png)
