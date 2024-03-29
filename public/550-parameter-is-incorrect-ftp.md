---
title: IISサーバへのFTPアクセスで「550 parameter is incorrect ftp」
tags:
  - Windows
  - ftp
  - IIS
private: false
updated_at: "2024-03-11T21:30:21+09:00"
id: 286f11196f3c973c56a3
organization_url_name: null
slide: false
ignorePublish: false
---

IISサーバへのFTPアクセスで `550 parameter is incorrect ftp` が発生した場合の対処方法の1つを紹介します。

:::note warn

※ これは2016-10-29に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## 現象

IISで動かしているFTPサーバへとあるFTPソフトで接続し、削除をしたところ、 `550 parameter is incorrect ftp` エラーが発生して削除コマンドが無視されてしまいました。

## 対処方法

IISマネージャー →「サイト」→FTP→「FTPディレクトリの参照」→「ディレクトリの表示スタイル」を `MS-DOS` にし、「ディレクトリの表示オプション」の「西暦を4桁で表示」をオフにする。

![IISマネージャー→「サイト」→FTP→「FTPディレクトリの参照」→「ディレクトリの表示スタイル」を「MS-DOS」にし、「ディレクトリの表示オプション」の「西暦を4桁で表示」をオフにする](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/c3a89d18-87e7-dfc0-c5ef-1f1bd6ddb278.png)

これでも対処しない場合、他のFTPソフトを用いるか、他の対処方法を検索してみてください。
