---
title: "allow_url_fopenが0のサーバーで外部ファイルを取得する方法"
tags:
  - "Programming"
  - "PHP"
  - "cURL"
  - "file_get_contents"
  - "allow_url_fopen"
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

allow_url_fopenに0が設定されたレンタルサーバーではPHPのfile_get_contentsなどを用いて外部のファイルが取得できません。これを回避して取得する方法を紹介します。

:::note warn

※ これは2017-07-19に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## レンタルサーバーのセキュリティが厳しい

(今は亡き)ウェブクロウというPHPが動作する無料サーバーにてPHPのfile_get_contentsを用いて外部ファイルを取得しようとしました。しかし、php.iniにてallow_url_fopen=0に設定されており、取得ができませんでした。

![取得エラーが発生したスクリーンショット](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/e1b3f4be-5aa7-44b8-301a-06679225df63.png)

次のようなエラーが発生します。

> Warning: file_get_contents(): http:// wrapper is disabled in the server configuration by allow_url_fopen=0 Warning: file_get_contents(http://www.example.com/): failed to open stream: no suitable wrapper could be found

## 別の関数を使用する

外部ファイルを取得する方法として他にcURLライブラリを用いる方法があります。このライブラリ経由であれば、allow_url_fopenに0が設定されたレンタルサーバーでも外部ファイルを取得できます。ただし、cURLライブラリがインストールされていないサーバーでは使用できません。

下記ではexample.comの内容がそのまま取得できています。

![example.comの内容がそのまま取得できているスクリーンショット](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/8bb29d01-86d5-7653-82cc-f1f0c7c8f07b.png)

下記はプログラムの例です。ユーザーエージェントやタイムアウトなどオプションが豊富なので[公式マニュアル](https://www.php.net/manual/ja/function.curl-setopt.php)を参考に色々試してみてください。

```php
<?php
$url="http://www.example.com/";
$cp = curl_init();
/*オプション:リダイレクトされたらリダイレクト先のページを取得する*/
curl_setopt($cp, CURLOPT_RETURNTRANSFER, 1);
/*オプション:URLを指定する*/
curl_setopt($cp, CURLOPT_URL, $url);
/*オプション:タイムアウト時間を指定する*/
curl_setopt($cp, CURLOPT_TIMEOUT, 30);
/*オプション:ユーザーエージェントを指定する*/
curl_setopt($cp, CURLOPT_USERAGENT, $_SERVER['HTTP_USER_AGENT']);
$data = curl_exec($cp);
curl_close($cp);
echo $data;
?>
```
