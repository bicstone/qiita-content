---
title: WordPress ショートコードで自サイトの他の記事をそのまま挿入
tags:
  - PHP
  - WordPress
  - プログラミング
private: false
updated_at: "2024-03-06T14:42:07+09:00"
id: fc1fdcad5dcd4e150ebd
organization_url_name: null
slide: false
ignorePublish: false
---

WordPressのショートコードを用いて自サイトの他の記事をそのまま挿入する方法を紹介します。

※ これは2016-04-09に個人ブログで公開した記事を移植したものです。

## functions.php へコードを追加

まず、テーマの `functions.php` に次のコードをコピペします。`functions.php` にすでにコードが追加されている場合は `<?php` 及び `?>` が二重にならないようお気をつけください。WordPressがエラーで起動できなくなってしまう場合があるためバックアップしてから保存してください。

```php
<?php
function shortcode_insert($atts){
extract(shortcode_atts(array('id'=>0),$atts));
if(get_post($id)!=null)return wpautop(do_shortcode(get_post($id)->post_content));
}
add_shortcode('insert','shortcode_insert');
?>
```

## 使用方法

そして挿入したい場所に次のようにショートコードを記入します。

`[insert id=123]`

123の部分は記事IDです。

記事IDは編集時のURLや投稿ページでのURLなどで確認ができます。

![URLの /wp-admin/post.php?post=xxx 部がIDです。](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/5d8e386c-c43b-3d30-74a3-cd3bdb70f08d.png)

![記事一覧の編集リンク先のURLを確認することでもIDを確認できます](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/14fe9962-9220-5b61-40be-0c78ca2d4c93.png)

尚、誤ったIDを指定した場合は何も表示されません。また、非公開、下書き、パスワード保護関わらず表示ができます。挿入元が更新された際には挿入先でも更新されます。非公開記事を作成し、このツールで挿入するという使い方も可能です。
