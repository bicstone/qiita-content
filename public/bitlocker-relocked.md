---
title: "BitLockerを再ロックする方法"
tags:
  - "Windows"
  - "BitLocker"
  - "バッチ"
  - manage-bde
  - "PowerShell"
private: false
updated_at: ""
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

BitLockerはドライブをまるごと暗号化できるWindowsの標準ツールです。ドライブを接続したままで再ロックする方法を紹介します。

:::note warn

※ これは2018-01-10に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## BitLocker を再ロックできない

BitLockerで暗号化したドライブを一度アンロックすると、利便性を高めるためか、ログアウトするかドライブを切断しない限り再ロックされません。

右クリックでの再ロックも不可能なようです。

![エクスプローラーのドライブを右クリックしてもBitLockerを再ロックする選択肢がない](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/6a1ea4c0-7ee9-df38-3e60-850c0222f9dd.png)

しかし、私の使用環境では、BitLockerをしたドライブはPC内部で接続しぱなしです。スリープし、しばらくログアウトしないことも多いため、せっかくの暗号化の意味が弱まってしまいます。

## コマンドプロンプトで再ロック可能

PowerShellでBitLockerを制御するプログラム `manage-bde` のパラメーター一覧を見てみたところ「`-lock` BitLocker暗号化データへのアクセスを禁止します。」との文字が！

![manage-bde のパラメータ説明。「 `-lock` BitLocker暗号化データへのアクセスを禁止します。」との文字がある](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/ba219e02-7b0d-75db-cef9-b1fe4ad6cf31.png)

試しに `manage-bde [ボリュームラベル]: -lock` を実行するとロックできました。

![`manage-bde H: -lock` するとボリュームHがロックされた](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/732de46b-80a3-d2cb-93f7-5a53c2bcb90d.png)

## バッチファイルにして簡単に実行できるように

`manage-bde [ボリュームラベル]: -lock` の1行を記載したバッチファイルを暗号化しているドライブに保存しておき、このバッチファイルを右クリックの管理者権限で実行するようにすると簡単にロックができます。

![lock.bat を作成する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/b73e20be-903d-94ee-b6e1-703b1eb75dcd.png)

![lock.bat を右クリックして、「管理者として実行」を選択する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/56ec8da0-6f0f-d69a-6640-969d0cc8a026.png)

## エクスプローラーの不具合

ただし、この方法ではエクスプローラーが誤作動します。

この方法でロックし、その後再びロック解除したあとにドライブをダブルクリックしても「BitLockerドライブ暗号化で保護されたドライブは既にロック解除されています。」と表示されます。

![Windows が 「BitLockerドライブ暗号化で保護されたドライブは既にロック解除されています。」と表示する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/0f0df1dc-3ee1-2e6e-fa15-b37edbd994ca.png)

その場合は、ドライブをダブルクリックせず、右クリックして「開く」をクリックしてください。

![エクスプローラーのドライブを右クリックして「開く」を選択](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/d5777946-49fa-fb47-b03f-f45d913db0dd.png)
