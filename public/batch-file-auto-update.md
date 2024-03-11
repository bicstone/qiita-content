---
title: バッチファイル自身に自動アップデート機能を搭載する方法
tags:
  - Windows
  - プログラミング
  - batch
  - cmd
private: false
updated_at: "2024-03-07T23:33:04+09:00"
id: 54a0345880b705580cf5
organization_url_name: null
slide: false
ignorePublish: false
---

バッチファイル自身に自動アップデート機能を搭載する方法を紹介します。複数のPCの一括適用に便利です。

:::note warn

※ これは2016-09-27に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## 需要

Windows PC数十台が稼働しており、それぞれのPCは個人で自由に使えるようにしています。ある作業をするためにバッチファイルを使っています。すべてのPCからNASへ接続できるようにしてあり、バッチファイルのバージョンアップ時は、各自NASからダウンロードして上書き保存をするようにしていました。ユーザーの手間を削減できればいいなと思い自動アップデート機能を搭載しました。

## 方法

NASに最新バージョンのみを記載したテキストファイルと最新版のバッチを設置します。バッチには、作業の最後に最新版があれば更新してから終了するようにプログラムを作成しました。ファイルの末尾に記載するのがミソです。このあとに他の作業をすると不具合がおきます(gotoなど)。

```shell
SET /A CURRENTVERSION=10
SET /P NEWVERSION=<"\\NAS\[~]version"
if %CURRENTVERSION% LSS %NEWVERSION% (copy /y "\\NAS\[~]バッチ.bat" %0)
```

`CURRENTVERSION` に現在のバージョン、最新バージョンのみを記載したテキストファイルの内容を `NEWVERSION` に代入して比較し、 `NEWVERSION` の方が大きい場合は自身のバッチ(`%0`)へ上書きコピーするという仕組みです。

意外と簡単に実装できました。
