---
title: 仮想環境で開発中 JavaScript が途中で途切れて壊れる場合の対処法
tags:
  - Windows
  - Apache
  - プログラミング
  - Vagrant
  - webpack
private: false
updated_at: "2024-03-03T20:26:37+09:00"
id: 16ccb0350a16ab6b42aa
organization_url_name: null
slide: false
ignorePublish: false
---

Vagrant (VirtualBox) + Apache + webpackでコンパイルされたJavaScriptやCSSが途中で途切れて壊れる場合の対処法を紹介します。

※ これは2020-02-02に個人ブログで公開した記事を移植したものです。

## gulp.js の watch モードで JS が壊れる

フロントの開発時、gulp.jsのwatchモードを用いてwebpackコンパイルを自動で行うよう設定されているプロジェクトで開発していていました。

しかし、なぜか半分の割合で、生成されたJSが途中で切れてしまいます。当然ブラウザ上ではエラーで停止します。

ファイルを直接見るとまったく問題がなく、Apacheの再起動するとすぐに復活するため、Webサーバーのキャッシュの問題を疑いました。

## 原因

EnableSendfileというApacheの機能が影響していました。

https://httpd.apache.org/docs/2.4/mod/core.html#enablesendfile

公式ドキュメントには次の記載があります。

> デフォルトでは、 例えば静的なファイルの配送のように、リクエストの処理にファイルの 途中のデータのアクセスを必要としないときには、Apache は OS が サポートしていればファイルを読み込むことなく sendfile を使って ファイルの内容を送ります。

そして、その下に次の記載があります。

> しかし、プラットフォームやファイルシステムの中には運用上の問題を避けるためにこの機能を使用不可にした方が良い場合があります:
> ネットワークマウントされた DocumentRoot (例えば NFS や SMB) では、カーネルは自身のキャッシュを使ってネットワークからのファイルを 送ることができないことがあります。

ファイルのキャッシュが中途半端なタイミングで行われてしまい、途中で途切れた状態が出力されてしまうというのが原因なようです。

次の記事のようにNFSを使用するように設定している場合に発生します。VirtualBox初期設定のファイル共有では発生しません。

https://bicstone.me/vagrant-laravel-slow/

## EnableSendfile を無効にする

EnableSendfile機能を無効にします。若干の速度低下があるようですが、(VirtualBoxを用いている時点で遅いので)よっぽど大量のリクエストを行っていない場合は問題ありません。

httpd.confのEnableSendfileをOffにします。

```ini
EnableSendfile off
```

または、.htaccessに設定します。

```ini
EnableSendfile off
```
