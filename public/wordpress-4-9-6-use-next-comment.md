---
title: 【WordPress】「次回のコメントで使用するため…」を削除する方法
tags:
  - PHP
  - WordPress
  - プログラミング
  - gdpr
private: false
updated_at: "2024-03-11T21:36:06+09:00"
id: 19fa23e68a03cad425dd
organization_url_name: null
slide: false
ignorePublish: false
---

WordPress 4.9.6からコメント投稿フォームに追加された「次回のコメントで使用するためブラウザーに自分の名前、メールアドレス、サイトを保存する。」を削除・非表示にする方法を紹介します。

:::note warn

※ これは2018-05-23に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## コメント投稿フォームにチェックマークが増えた

2018年5月19日に更新されたWordPress 4.9.6にアップデートすると、[`comment_form`](https://developer.wordpress.org/reference/functions/comment_form/)コマンドが用いられているテーマでコメント投稿コメント欄の送信ボタンの真上に次のスクリーンショットのようなチェックボックスとテキスト「次回のコメントで使用するためブラウザーに自分の名前、メールアドレス、サイトを保存する。」が追加されてしまいます。

![「次回のコメントで使用するためブラウザーに自分の名前、メールアドレス、サイトを保存する。」がコメント投稿欄に表示される](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/26d7120d-0f23-65d6-56b6-4a10e4d86d38.png)

## 原因

2018年5月25日に発効される「欧州連合（EU）の一般データ保護規則（GDPR）」に則るための新機能が原因です。Cookieの保存の可否を問う新機能を搭載したと公式ブログにも記載があります。

https://ja.wordpress.org/2018/05/19/wordpress-4-9-6-privacy-and-maintenance-release/

## 対処方法

テーマの `functions.php` に次のコードをコピペします。`functions.php` にすでにコードが追加されている場合は `<?php` 及び `?>` が二重にならないようお気をつけください。WordPressがエラーで起動できなくなってしまう場合があるためバックアップしてから保存してください。

```php
<?php
add_filter('comment_form_default_fields', 'comment_remove_cookiescheck');
function comment_remove_cookiescheck($arg) {
 $arg['cookies'] = '';
 return $arg;
}
?>
```

## 対処結果

WordPress 4.9.5までの従来の表示に戻りました。動作的には常に保存しない設定となります。

![「次回のコメントで使用するためブラウザーに自分の名前、メールアドレス、サイトを保存する。」がコメント投稿欄に表示されなくなった](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/ac4043c9-afb1-00de-eb6d-cd11f97467e6.png)
