---
title: オブジェクト URL を File または Blob オブジェクトに変換する方法
tags:
  - JavaScript
  - プログラミング
  - blob
  - XMLHttpRequest
  - fetch
private: false
updated_at: "2024-03-03T20:26:26+09:00"
id: 5796124885a3ea92adc2
organization_url_name: null
slide: false
ignorePublish: false
---

JavaScriptのオブジェクトURLをFileオブジェクトまたはBlobオブジェクトに戻す(変換する)方法を紹介します。 `URL.createObjectURL()` の逆を行います。

## オブジェクト URL とは

オブジェクトURLは、JavaScriptで次のようにメゾットを呼び出すことで生成されます。

```js
URL.createObjectURL(object);
```

https://developer.mozilla.org/ja/docs/Web/API/URL/createObjectURL

ファイルを処理したり、ユーザーからファイルを受け取るライブラリなどで、オブジェクトURLで返ってきますが、データを処理したいのでFileオブジェクトやBlobオブジェクトに戻したいケースがあります。

しかし、File APIには `URL.createObjectURL(object);` の逆が用意されていません。

例えば、 [`react-dropzone`](https://github.com/react-dropzone/react-dropzone) というReactのライブラリではオブジェクトURLで返ってきます。

## File オブジェクトに戻す方法

なんの変哲もない対処方法なのですが、URLなので、 `fetch()` または `XMLHttpRequest` を使用すればFileオブジェクトに戻すことができます。

### `fetch()` を用いた方法

ES5が動作する環境であれば `fetch()` を用いた方法がおすすめです。

[Can I Use](https://caniuse.com/#feat=fetch)によると、IE、古すぎるEdge、Opera mini以外であれば動作するようです。対象外のブラウザでも[polyfill](https://github.com/github/fetch)を導入したり、トランスコンパイルをすれば動作します。

```js
let file = await fetch(url).then((r) => r);
let blob = await fetch(url).then((r) => r.blob());
```

### `XMLHttpRequest` を用いた方法

古き良き、`XMLHttpRequest` を用いても取得できます。

[Can I Use](https://caniuse.com/#feat=mdn-api_xmlhttprequest)によると、現在の主要ブラウザはすべて動作するようです。

```js
var xhr = new XMLHttpRequest();
xhr.open("GET", url, true);
xhr.responseType = "blob";
xhr.onload = function () {
  var blob = this.response;
};
xhr.send();
```
