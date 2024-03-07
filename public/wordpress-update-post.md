---
title: WordPressの記事をPHPから更新だけ行う方法
tags:
  - PHP
  - WordPress
  - プログラミング
private: false
updated_at: "2024-03-07T23:28:52+09:00"
id: a05d609910a650825e60
organization_url_name: null
slide: false
ignorePublish: false
---

WordPressの記事や固定ページをPHPから更新だけ行う方法です。

※ これは2016-10-25に[個人ブログ](https://bicstone.me)で公開した記事を移植したものです。

## 需要

キャッシュプラグインを使用している時に、一定時間で自動更新される外部ページを読み込む仕組みがあるWordPressの記事を更新したい場合に利用できます。

## 方法

次のようなPHPを実行します。場合によっては読み込みに数秒かかる場合があるのでタイムアウト設定などにご注意ください。

```php
<?php
require( '/[WordPressディレクトリ]/wp-load.php' );
wp_update_post(array('ID'=> [記事ID]));
?>
```

## 参考

https://wpdocs.osdn.jp/%E9%96%A2%E6%95%B0%E3%83%AA%E3%83%95%E3%82%A1%E3%83%AC%E3%83%B3%E3%82%B9/wp_update_post

post_titleとpost_contentを指定しなければ、更新日時だけが更新されます。

## 追記

デフォルトのままだとping打ちまくる結果となってしまうので[WordPress ping Optimizer](https://ja.wordpress.org/plugins/wordpress-ping-optimizer/)プラグインの導入を強くおすすめします。
