---
title: AdBlockなどの広告ブロッカーを使用しているか判定する方法
tags:
  - JavaScript
  - jQuery
  - プログラミング
  - AdBlock
  - 広告ブロッカー
private: false
updated_at: "2024-03-07T23:33:04+09:00"
id: 916f1485e5d9fae997c8
organization_url_name: null
slide: false
ignorePublish: false
---

AdBlockなどの広告ブロッカーを使用しているかをJavaScriptを用いて判定する方法です。

※ これは2016-04-29に[個人ブログ](https://bicstone.me)で公開した記事を移植したものです。

## 広告ブロッカーを検知する仕組み

広告ブロッカーは指定のアドレス(例えばGoogle AdSenseのJavaScriptファイル)のアクセスを遮断する他、余分な空白を無くす目的で、例えば `"adsbygoogle"` というclass名がついたdivをまるごと消すといったこともしています。この仕組みを使用し、JavaScriptで `"adsbygoogle"` 等のclass名がついたdivボックスを小さく作成し、広告ブロッカーによって消されたことを検知することによって広告ブロッカーを検知します。

## 広告ブロッカーを検知するライブラリを導入

広告ブロッカーを検出するライブラリはたくさんありますが、シンプルな仕組みの「[BlockAdBlock](https://github.com/sitexw/BlockAdBlock)」をおすすめします。

しかし、このままでは使用できません。当然広告ブロッカーサイドにもこのライブラリが知られており、このライブラリが反応しないように修正されるなどイタチごっこが起きています。そこで `blockadblock.js` を一部書き換えます。

15行目の `baitClass: 'pub_300x250 pub_300x250m pub_728x90 text-ad textAd text_ad text_ads text-ads text-ad-links',` は、前述した小さいdivにつけられるclass名です。このclass名をさらに追加します。

例えば、次のようにします。思いつけば色々追加してみてください。増やせば増やすほど広告ブロッカーに引っかかりやすくなります。非表示にするのは広告ブロッカーのみで「誤検出」は発生しませんのでご安心ください。

`baitClass: 'pub_300x250 pub_300x250m pub_728x90 text-ad textAd text_ad text_ads text-ads text-ad-links ad- ad_ _ad_ ads- -ads- ads_ _ads_ -ads- _ads_ ad ads koukoku blog_ad blog-ad ad-blog ad_blog blogroll adsbygoogle',`

## ライブラリを動作させる

次のコードを挿入します。広告ブロッカー検知時に表示するclass名をxxxとしていますが、この名称は任意です。ただし、広告ブロッカーがブロックしそうな名称または前章で定義した名称にすると非表示になってしまいます。

```html
<script type="text/javascript" src="blockadblock.js"></script>
<script
  type="text/javascript"
  src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.4/jquery.min.js"
></script>

<div class="xxx" style="display:none;">広告ブロッカーが有効です。</div>
<script>
  function adBlockDetected() {
    /*広告ブロッカー検知時の動作*/
    jQuery(".xxx").show();
  }
  function adBlockNotDetected() {
    /*広告ブロッカー未検知時の動作*/
    jQuery(".xxx").hide();
  }
  if (typeof blockAdBlock === "undefined") {
    /*blockadblock.jsが読み込めない場合は広告ブロッカー検知扱い*/
    adBlockDetected();
  } else {
    /*広告ブロッカー検知*/
    blockAdBlock.onDetected(adBlockDetected);
    /*広告ブロッカー未検知*/
    blockAdBlock.onNotDetected(adBlockNotDetected);
  }
</script>
```

広告ブロッカーが始動していればclass名 `xxx` のdivが表示されます。広告ブロッカーが発動していなければ `xxx` のdivは非表示になります。

尚、念のためJavaScript無効ユーザーに向けてclass名 `xxx` のdivは `style="display:none;"` を追加してデフォルトでは非表示にしています。
