---
title: Vue で空文字が Prop で渡ってきてバグってしまった話
tags:
  - JavaScript
  - プログラミング
  - Vue.js
  - vue2
private: false
updated_at: "2024-02-25T16:28:20+09:00"
id: 0bca7652fa65c5f5e85d
organization_url_name: null
slide: false
ignorePublish: false
---

数年前に作られていたvue 2 (TypeScriptなし) のプロダクトで空文字がPropで渡ってきてバグってしまった話と、注意点を備忘録として残します。

※ これは2022/12/05に[個人ブログ](https://bicstone.me)で公開した記事を移植したものです。

## 背景

数年前に作られていたvue 2 (TypeScriptなし) のプロダクトで次のようなコンポーネントがありました。

```js
export default {
  props: {
    sample: Boolean,
    default: false,
  },
};
```

このコンポーネントはこのように呼び出されています。

```vue
<template>
  <Sample :sample="sample2" />
</template>
```

このsample2、実は文字列が入っていました！

設計者の意図としては、JSの暗黙的な型変換と同様に空文字列であれば `false` 、 1文字以上であれば `true` という実装にしたかったのだと考えられます。

## 空文字列 `""` が `true` !?

ある日、 sample2に空文字列 `""` が入ってきました。しかし、 Sampleが `true` の場合の挙動をします。

なんと、 空文字 `""` が入ってきたときにコンポーネント内で `true` に変換されていたのです。

空文字 `""` はJavaScriptの真偽判定においてFalsyになるのでしばらく原因を理解するのに時間かかりました。

https://developer.mozilla.org/ja/docs/Glossary/Falsy

原因は、 `v-bind` において、 falsyな値は `null`, `undefined`, `false` だけという仕様があるためです。

https://v2.vuejs.org/v2/guide/migration.html#Truthiness-Falsiness-with-v-bind-changed

## なぜ

HTML Attributeと同じ挙動になるように揃えたみたいです。個人的にはReact (JSX) と同じ挙動なのでわかりやすく感じます。 v-ifなどは引き続きJavaScriptの真偽判定と同様の挙動をするようです。

## まとめ

- そもそもbooleanなpropに文字列を渡さないようにしよう
- このようなプログラムを書かれないように規約を設けるという意味でもTypeScriptは必須ですね
