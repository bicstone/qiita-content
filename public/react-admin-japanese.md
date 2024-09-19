---
title: React-admin の 日本語翻訳ファイルを公開しました
tags:
  - JavaScript
  - Web
  - TypeScript
  - React
  - react-admin
private: false
updated_at: "2024-09-19T13:42:13+09:00"
id: 817a19e0c5a33aff9ac1
organization_url_name: null
slide: false
ignorePublish: false
---

React-adminの日本語翻訳ファイルを公開しました。直訳せず、読みさすさとシンプルさを優先した意訳をしています。

:::note warn

※ これは2020-02-07に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[MIT](https://github.com/bicstone/ra-language-japanese/blob/v5.2.0/LICENSE)で提供しています。情報は古い可能性があります。

:::

## react-admin

React-adminは、React + Redux + Redux-Saga + React-Query + MUI + React-Router + React Hook Formなどを活用した、管理サイトのために設計されたフロントエンドフレームワークです。

https://github.com/marmelab/react-admin

デモはこちらです。

https://marmelab.com/react-admin-demo/#/

管理サイトだけでなく、データ中心の会員制のサイトなどにも使えます。

実際に仕事でちょっとした会員制のWebサイトを短納期で作れました。シンプルで保守性の高いプログラムにできます。

## ra-language-japanese

デフォルトは英語ですが、フレームワーク自体が翻訳対応しており、翻訳キーと翻訳文字列を対にしたオブジェクトを渡すことで日本語化できます。

https://marmelab.com/react-admin/Translation.html#available-locales

日本では知名度が低いのか、最新の翻訳ファイルが見つけられませんでした。とてもお世話になっているので、少しでも貢献できれば。ということで翻訳を公開しました。

先程の公式ドキュメントからもリンクしています。

https://github.com/bicstone/ra-language-japanese

## インストール

### バージョン 4 ～ 5

```shell
npm install @bicstone/ra-language-japanese@latest
# or
yarn add @bicstone/ra-language-japanese@latest
# or
pnpm add @bicstone/ra-language-japanese@latest
```

### バージョン 2 ～ 3

```shell
npm install @bicstone/ra-language-japanese@3
# or
yarn add @bicstone/ra-language-japanese@3
# or
pnpm add @bicstone/ra-language-japanese@3
```

## 使用方法

React-admin v2系では、次のように使用できます。

```js
import japaneseMessages from '@bicstone/ra-language-japanese';

const messages = {
  ja: japaneseMessages
};
const i18nProvider = locale => messages\[locale\];

<Admin locale="ja" i18nProvider={i18nProvider}>
...
</Admin>
```

v3系以降では、フックが採用されているため、`ra-i18n-polyglot` をimportする必要があります。

```js
import japaneseMessages from '@bicstone/ra-language-japanese';
import polyglotI18nProvider from 'ra-i18n-polyglot';

const messages = {
  ja: japaneseMessages
};
const i18nProvider = polyglotI18nProvider(locale => messages\[locale\]);

<Admin locale="ja" i18nProvider={i18nProvider}>
...
</Admin>
```

## まとめ

もし、便利だと思ったらスター頂ければ幸いです。実はしばらくBacklogを使用しておらず、多くの方に使用していただいているという責任感で維持している状態です。反響があると、維持するモチベーションに繋がります。

よろしくお願いいたします。

https://github.com/bicstone/ra-language-japanese
