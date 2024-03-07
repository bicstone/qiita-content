---
title: TinyMCEがTableタグに「width」と「height」を勝手に設定する機能を無効にする
tags:
  - PHP
  - JavaScript
  - WordPress
  - プログラミング
  - tinyMCE
private: false
updated_at: "2024-03-07T23:28:55+09:00"
id: 255d17cde327884fe893
organization_url_name: null
slide: false
ignorePublish: false
---

WordPressがアップデートされてから、TinyMCEがTableタグに「width」と「height」を勝手に設定する機能が追加されました。レスポンシブデザインのサイトにおいて、この機能はむしろデザインを崩す原因となってしまいます。この機能を無効にする方法をご紹介します。

※ これは2016-08-28に[個人ブログ](https://bicstone.me)で公開した記事を移植したものです。

## 問題

TinyMCE 4.3.3以上(WordPress 4.5以上)においてテーブルの境界線を一度でも触ってしまうと、TinyMCEはTableタグに「width」と「height」を勝手に設定します。

![2×2のHTMLテーブルのスクリーンショット](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/081b61b8-7c33-8a6b-94aa-de99e05f201f.png)

![テーブルの枠線を触る](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/d30edd2f-e882-6c8d-67a4-7d4030673c95.png)

![テーブルの枠線をドラッグ中](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/f9966a3c-5302-fd09-4ba8-dca21d7672d8.png)

```html
<table style="width: 150px;">
  <tbody>
    <tr>
      <td style="width: 10px;">テーブル</td>
      <td style="width: 118px;">テーブル</td>
    </tr>
    <tr>
      <td style="width: 10px;">テーブル</td>
      <td style="width: 118px;">テーブル</td>
    </tr>
  </tbody>
</table>
```

## 対処方法

TinyMCEのオプション、「table_resize_bars」をFalseに変更(初期値:True)し、「object_resizing」を「img」へ変更(初期値:True)します。

### TinyMCE 単独で用いている場合

`tinymce.init()` の中に、次のコードを追加します。

```js
{
  table_resize_bars: false,
  object_resizing: img,
}
```

### WordPress で用いている場合

functions.phpなどに次のコードを追加します。

```php
<?php
function customize_tinymce_settings($mceInit) {
$mceInit['table_resize_bars'] = false;
$mceInit['object_resizing'] = "img";
return $mceInit;
}
add_filter( 'tiny_mce_before_init', 'customize_tinymce_settings', 0);
```

ちなみに、このコードを用いれば、[TinyMCE のマニュアル](https://www.tinymce.com/docs/)を参考に他の設定もカスタマイズできます。

## 結果

テーブルの境界線を触っても何も起きません。

![2×2のHTMLテーブルのスクリーンショット](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/081b61b8-7c33-8a6b-94aa-de99e05f201f.png)

![テーブルの境界を触っても、ドラッグアンドドロップできない](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/91ea5c54-61f7-ec9a-ee3a-342273afd801.png)

```html
<table>
  <tbody>
    <tr>
      <td>テーブル</td>
      <td>テーブル</td>
    </tr>
    <tr>
      <td>テーブル</td>
      <td>テーブル</td>
    </tr>
  </tbody>
</table>
```
