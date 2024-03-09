---
title: エックスサーバーの無料独自SSLをサイト移転前に設定する方法
tags:
  - PHP
  - Apache
  - プログラミング
  - htaccess
  - Xserver
private: false
updated_at: "2024-03-07T23:33:04+09:00"
id: 25f86b63880214f5c96e
organization_url_name: null
slide: false
ignorePublish: false
---

エックスサーバー・スターサーバーの無料独自SSL設定をサイト移転前に設定し、SSLで接続できなくなる問題を回避する方法を紹介します。

※スクリーンショットはスターサーバーの管理画面ですが同じ業者であるエックスサーバーでも同様に設定可能です。

※ これは2019-01-01に[個人ブログ](https://bicstone.me)で公開した記事を移植したものです。

## サイト公開しないと無料独自 SSL 設定ができない

当ブログはMixHostからスターサーバーへ移転しました。移転時に一番問題だと感じたのが、サイトをスターサーバーで公開しないと無料独自SSL設定ができないため、一定時間SSLが使用できないというダウンタイムが存在してしまうことでした。

スターサーバーにドメイン追加後、SSL設定より「無料独自SSL追加」を行ってもエラーが発生したと表示され設定ができません。

![スターサーバー管理画面の無料独自SSL追加時にエラーが発生](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/16fa4e63-498a-c278-c4af-fd46dbad002d.png)

この状態でネームサーバーの設定を変え新サーバーにアクセスが行ってしまうと、SSL検証エラーとなり、SSLを設定するまでの間ダウンタイムとなってしまいます。また、HOSTSファイルを使用して新サーバーでの動作確認も行えません。

![Google Chrome のSSLエラー 「この接続ではプライバシーが保護されません」](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/c75c1ae4-00e5-a93c-8242-be39a82ec6c1.png)

## SSL 設定時の認証

エックスサーバーやスターサーバーの「無料独自SSL」は「Let's Encrypt Authority X3」を使用しています。認証はLet's Encryptが行っています。

Let's EncryptのWeb認証では、`http://example.com/.well-known/acme-challenge/` の直下にLet's Encryptから指示された認証用一時ファイルを設置し、存在が確認されることで行われます。

エックスサーバーやスターサーバーでも「無料独自SSL追加」を行うと設定するサーバーの `/.well-known/acme-challenge/` の直下に認証用一時ファイルを設置しています。

https://letsencrypt.readthedocs.io/en/latest/using.html

## 移転前動作確認 URL でも認証ファイルにアクセスが可能

認証を通すためにはエックスサーバーやスターサーバーに設置された認証用一時ファイルへアクセスできるようにする必要があります。

エックスサーバーやスターサーバーには「移転前動作確認URL」という機能があり、この機能で作成されたURLでも `/.well-known/acme-challenge/` にアクセスが可能になっています。

https://www.xserver.ne.jp/manual/man_domain_checkproxy.php

https://www.star.ne.jp/manual/domain_checkproxy.php

## 対処方法

1. 「移転前動作確認URL」の設定から動作確認URLを入手します。

![「移転前動作確認URL」の設定から動作確認URLを入手します。](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/63884fd3-31fd-7d1c-8ee4-b561a43fef58.png)

2. 移転前のサーバーに `/.well-known/acme-challenge/` までのフォルダを作成します。

3. `/.well-known/acme-challenge/` の直下に次の `.htaccess` ファイルと `index.php` を設置します。

`index.php`

`http://masshiro-blog.check-star.jp/(アクセスされたURL)` にデータを取得しに行き、そのまま返すプログラムです。

URLは `/.well-known/acme-challenge/(英数字-_)` 以外の場合弾くようにしています。

`http://masshiro-blog.check-star.jp/` の部分は移転前動作確認URLに置き換えてください。

```php
<?php
$url = $_SERVER['REQUEST_URI'];
if(preg_match('/^/.well-known/acme-challenge/[a-zd-_]+$/i', $url)):
  $cp = curl_init();
  curl_setopt($cp, CURLOPT_URL, 'http://masshiro-blog.check-star.jp/' . $url);
  curl_setopt($cp, CURLOPT_RETURNTRANSFER, true);
  echo curl_exec($cp);
  curl_close($cp);
else:
  exit();
endif;
?>
```

`.htaccess`

WordPressでもお馴染み、直下へのすべてのアクセスを `index.php` に転送するプログラムです。

```plain
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule (.*) index.php?/$1 [L]
</IfModule>
```

4. SSL設定より「無料独自SSL追加」を行う。

![SSL設定より「無料独自SSL追加」を行う。独自SSLを追加しましたのメッセージが表示される](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/b45d2ea7-dd51-94e2-ef33-30199a117c93.png)

5. index.php及び.htaccessを削除する。
