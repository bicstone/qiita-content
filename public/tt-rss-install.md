---
title: "レンタルサーバーにTiny Tiny RSSをインストール"
tags:
  - "PHP"
  - "Programming"
  - "Tiny Tiny RSS"
  - "MySQL"
  - "cron"
private: false
updated_at: ""
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

レンタルサーバーにTiny Tiny RSSを簡単にインストールする方法を紹介します。今回はさくらのレンタルサーバでの例を紹介しますが、PHPとMySQLが動作する一般的なレンタルサーバーであれば、概ねインストールできます。

:::note warn

※ これは2017-05-28に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## Tiny Tiny RSS とは

Tiny Tiny RSS (略称tt-rss)とはサーバーサイドのオープンソースRSSクライアントです。インストールしたURLにアクセスするだけで見られるのでスマホなどからどこでも見られる・高頻度でRSSを取得しに行くので更新頻度が高いサイトでも逃さずキャッチしやすい・登録したレンタルサーバー業者以外に登録しているRSSの情報が知られないなどの特徴があります。一方、定期的にtt-rssをアップデートするなどの保守が必要になります。

インストール不要で登録制のサービスとしてfeedlyがありますが、RSSの登録数に制限がある、Googleアラートの登録が不可、更新間隔が長い、RSS登録情報がfeedlyに知られるといった欠点があります。

## データベースの作成

まず、事前準備として、さくらのレンタルサーバのコントロールパネルより、MySQLのデータベースを作成します。

[さくらのレンタルサーバーのコントロールパネル](https://secure.sakura.ad.jp/rscontrol/)を開きます。

データベースの設定よりデータベースを新規作成します。

![データベースの新規作成を選択](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/0258ecf8-d167-6590-32e0-2f5384eb2973.png)

適当なデータベース名を設定し、作成します。

![適当なMySQLのデータベースを作成](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/5eb4a290-b99e-0eba-a6d5-4156c3dec11e.png)

## tt-rss をダウンロード

次に、tt-rssをダウンロードしてFTPでアップロードします。または、WebサーバーにSSHで入って同じコマンドを叩きます。

```sh
git clone https://git.tt-rss.org/fox/tt-rss.git
```

https://git.tt-rss.org/fox/tt-rss.git

## 設定

`「https://[インストールしたURL]/install/」` を開くとインストール画面が出てきます。

- Database typeは `MySQL`
- Usernameはユーザー名(初期アカウント名)
- Passwordは設定しているパスワード
- Database nameは先程設定したデータベース名
- Host nameはコントロールパネルに記載の `mysql[数字].db.sakura.ne.jp`
- Portは `3306`
- Tiny Tiny RSS URLは現在アクセスしているURLから/install/を消したもの

![Tiny Tiny RSS installer のスクリーンショット](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/907e9295-64b3-9fbd-427d-0305610a6956.png)

Test configurationをクリック、その後Save configurationをクリックすると無事インストール完了です。

## ログイン

「`https://[インストールしたURL]`」からログインします。初期はログイン:「`admin`」、パスワード:「`password`」になっています。

![Tiny Tiny RSS のログイン画面のスクリーンショット](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/f6e72b60-c93b-e07a-c5fd-a6da0d5b25f9.png)

ログイン後、右上の設定より、パスワードを変更します。他のユーザーアカウントが必要なければ、アカウントを追加せずにadminを引き続き使用してよさそうです。

![Tiny Tiny RSS の設定画面のスクリーンショット](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/d25c6b28-80f7-efd1-7d22-0b6e37fe666d.png)

## CRON を設定する

tt-rssのRSSを巡回させるには、サーバー内部から「update.php --feeds --quiet」を定期的に実行させる必要があります。そのためにCRONを設定します。

[さくらのレンタルサーバーのコントロールパネル](https://secure.sakura.ad.jp/rscontrol/)より、「CRONの設定」を開きます。

次のCRONを設定します。この場合、30分ごとの取得となります。間隔が短すぎると取得先のサーバーから規制される場合があります(IPアドレスで規制されてしまった場合、IPアドレスを変更しなければ一生取得できなくなります…)。

- 実行コマンドは `/usr/local/bin/php /home/[ユーザー名]/www/[設置パス]/update.php --feeds --quiet`
- 月は `*`
- 日は `*`
- 時は `*`
- 分は `*/30`
- 曜日は `日月火水木金土`
- コメントは任意

以上で設定は完了です。

## アップデート

tt-rssは定期的にアップデートする必要があります(アップデートしないとセキュリティの懸念が発生する場合もあります)。

FTPでアップロードしている場合、 `git pull` してからFTPで上書きします。

SSHでWebサーバーにcloneしている場合、SSHで入って `git pull` します。

ただし、config.phpの書式が更新されている可能性があるので、注意します。また、更新中にCRONによってupdate.phpが呼ばれると不具合が生じることもあるので、update.phpは一時的に削除しておくことをおすすめします。
