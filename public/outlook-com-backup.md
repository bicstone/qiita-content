---
title: outlook.com のメールをバックアップしてストレージを空ける方法
tags:
  - mail
  - バックアップ
  - Outlook
  - sylpheed
  - mbox
private: false
updated_at: "2024-03-19T00:04:35+09:00"
id: 91a19495be26a27930ee
organization_url_name: null
slide: false
ignorePublish: false
---

2023年2月にoutlook.comメールのストレージがOneDriveと結合される変更が行われるのに従い、メールをPCに保存してストレージを空ける方法を解説します。

:::note warn

※ これは2023-02-04に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## 背景

みなさんはoutlook.comを使用していますか？　私は初めて買ったiPhoneで唯一プッシュ通知に対応していたという経緯で、Hotmailからそのままoutlook.comを使用しています。無料プランでも、メール添付ファイル15 GBまで保存できる (メッセージは加算しない) という太っ腹なので、メールはすべて残しています。

しかし、先日、突然こんな発表がされました。

https://support.microsoft.com/ja-jp/office/microsoft-365-%E3%81%AE%E9%9B%BB%E5%AD%90%E3%83%A1%E3%83%BC%E3%83%AB%E6%A9%9F%E8%83%BD%E3%81%A8%E3%82%B9%E3%83%88%E3%83%AC%E3%83%BC%E3%82%B8%E3%81%AE%E5%A4%89%E6%9B%B4-e888d746-61e5-49e3-9bd1-94b88e9be988

> 2023 年 2 月 1 日から、 Microsoft 365 アプリとサービス全体で使用されるクラウド ストレージには、添付ファイルデータと OneDrive データ Outlook.com が含まれます。 すべてのデータは、Microsoft の包括的なセキュリティ機能セットで引き続き保護されます。
> この更新プログラムは、 Outlook.com メールボックスのストレージ量には影響しません。 ただし、これにより、 OneDrive で使用できるクラウド ストレージの量が減る可能性があります。 クラウド ストレージ クォータに達すると、 Outlook.com でメールを送受信する機能が中断されます。

わかりにくい表記なのですが、基本的にはoutlook.com + OneDriveで5 GBしか使えなくなるという改悪となります。

容量オーバーするとメールが受信できなくなってしまうため、メールをバックアップしてoutlook.comのストレージを空けることにしました。

## メールをバックアップする

### 公式の機能でバックアップする

outlook.com内の機能でメールをバックアップする機能が提供されています。ダウンロードできるようになるため数日かかるため、予め申請しておきましょう。

![outlook.comのスクリーンショット。outlook.com の設定の「プライバシーとデータ」から「メールボックスをエクスポート」を選択することで pst 形式でダウンロードできます。](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/881db115-711e-ed26-cb7a-f3aadacc338b.png)

残念ながらダウンロードできるファイルはpstファイル形式です。 pstファイルは、Officeに付帯しているMicrosoft Outlook以外では残念ながら開くことができません。バックアップしたファイルがMSの商用ソフトでしか開けないというのは…。

### IMAP でバックアップする

一応Microsoft Outlookで見られることを確認しましたが、念のため、メールクライアントを使ってIMAP経由でバックアップしてみます。

個人的に信頼しているオープンソースソフトウェアであるSylpheedを使ってバックアップしてみます。Thunderbirdでも良さそうですが、バックアップの用途で使用するのでポータブル版が公式で存在するSylpheedを使います。Windowsだけでなく、MacOSやLinuxでも使用できます。

https://sylpheed.sraoss.jp/ja/

ポータブル版をダウンロードし起動したらIMAP4 (MS365) を選択しログインします。

![Sylpheedのスクリーンショット。Sylpheed を開いたら、 IMAP4 (MS365) を選択しログインする](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/9ea57b6e-b695-35cd-dcf2-4c0b6e960c75.png)

次に、 SylpheedのFile → Ecport mail dataを選択します。

![Sylpheedのスクリーンショット。Sylpheed の File → Ecport mail data を選択する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/083f2991-28bf-5a54-308d-d3215dcf0da2.png)

そして、エクスポート元のフォルダとエクスポート先のファイルを指定し、エクスポートを開始します。

![Sylpheed でエクスポートを開始したスクリーンショット](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/0d935140-1e9f-1d9e-e924-fe6da698945d.png)

## outlook.com のメールを削除する

2系統のバックアップが取れたら、 outlook.comから削除しましょう。

設定 → 全般 → ストレージから一括で削除できます。

## バックアップしたメールボックスを参照する

### pst ファイル

pstファイルは、 Microsoft Outlookにインポートして参照、検索できます。

https://learn.microsoft.com/ja-jp/outlook/troubleshoot/data-files/how-to-manage-pst-files

### UNIX mbox

UNIX mboxファイルは、Sylpheedに限らず多くのクライアントソフトにインポートして参照、検索できます。
