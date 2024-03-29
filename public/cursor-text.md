---
title: ホームページでデフォルト時「テキスト編集カーソル」にしない方法
tags:
  - HTML
  - CSS
  - プログラミング
private: false
updated_at: "2024-03-11T21:30:21+09:00"
id: b2fdca68fc3ad5b025f6
organization_url_name: null
slide: false
ignorePublish: false
---

通常、何も指定しなければテキスト上にカーソルがあると「テキスト編集カーソル」になります。常に「テキスト編集カーソル」になっている事自体、コピーを促しているように感じてしまうのです(私だけではないはず)。

![カーソル](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/235ff1c9-eaef-28b1-55c8-0d98d0899009.png)

そこで、デフォルトでは「テキスト編集カーソル」にしない方法を紹介します。

:::note warn

※ これは2016-03-29に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## CSS を用いた方法

CSSに次の1行を追加するだけです。

```css
html {
  cursor: default;
}
```

## HTML を用いた方法

HTMLのどこかに次の1行を追加するだけです。

```html
<style>
  html {
    cursor: default;
  }
</style>
```

## 結果

テキストの上にマウスを置いても通常のカーソルになります。

![テキストをホバーしても、デフォルトのカーソルになっている](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/79f0235f-e70c-42e9-578e-bab3da07cd2e.png)

リンクをフォーカスしている時にちゃんと指のカーソルになります。

![リンクを選択すると、指のカーソルになりました](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/735402a5-a9a6-a65f-af59-fa709b904c9e.png)
