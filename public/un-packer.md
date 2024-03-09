---
title: /packer/でBase62エンコードしたJavaScriptコードをデコードするツール
tags:
  - JavaScript
  - プログラミング
  - packer
  - 難読化
  - base62
private: false
updated_at: "2024-03-09T16:08:55+09:00"
id: aec6d731d11231d1342c
organization_url_name: null
slide: false
ignorePublish: false
---

[/packer/](http://dean.edwards.name/packer/)でBase62エンコードしたJavaScriptコードをデコード(復号)するツールです。

※ これは2016-04-19に[個人ブログ](https://bicstone.me)で公開した記事を移植したものです。

## デコードツール

`eval(function(p,a,c,k,e,r){e=function(c){return(` で始まるコードをコピペしてDecodeボタンを押してください。すべてブラウザで処理され、サーバーには送信されませんのでご安心ください。

2重、3重にエンコードされている場合もあります。その場合は数回Decodeボタンを押してください。

https://codesandbox.io/embed/frosty-mendel-x0uv0j?&theme=light&view=preview

デコードされたコードをわかりやすく見たい場合は[Online JavaScript beautifier](http://jsbeautifier.org/)のようなツールをご使用下さい。

## デコードコード

次のJavaScriptでデコードが可能です。

```js
var a; /*デコード前のjs*/
var b; /*デコード後のjs*/
eval("b.value=String" + a.value.slice(4));
```

## おまけ

/packer/はすでに開発を停止しています。現状ではエラーを引き起こすエンコードを行ってしまう場合があります。ECMAScript 5レベルまでのプログラムであれば使用できそうです。アロー関数式などは全滅のようで、事前にトランスコンパイルする必要があります。

## おまけ 2

漫画村のアドフラウド（隠し広告）を実行するためのJavaScriptコードが/packer/で難読化されていたそうです。意外と難読化のツールとして使われているのですね。
