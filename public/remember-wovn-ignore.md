---
title: wovn-ignoreを入れ忘れない方法
tags:
  - CSS
  - プログラミング
  - I18n
  - wovn
  - wovn-ignore
private: false
updated_at: "2024-03-11T21:30:22+09:00"
id: 197350d50e79ac00bb8e
organization_url_name: null
slide: false
ignorePublish: false
---

Webサイトの多言語化・翻訳対応を一任できるサービスWOVN.ioにおいて、個人情報や動的な数字がWOVNに送信されないように翻訳抽出対象外にするwovn-ignoreを設定し忘れない方法を紹介します。

:::note warn

※ これは2021/03/29に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## WOVN.io について

Webサイトの多言語化・翻訳対応を丸投げできるSaaSです。資産は日本語のまま、JSタグを追加するだけでGoogle翻訳のように翻訳をしてくれます。Google翻訳とは異なり、辞書を登録したり、人力で翻訳をしてもらうこともできます。

ちなみに読み方は「ウォーブン」だそうです。私の周りでは「ウーヴン」や、「オーブン」など様々な呼び方がされていました。初期はみんなビビって「ダブリューオーヴィエヌ」と呼んでいました。ASUSみたいですね？

https://wovn.io/ja/

Webエンジニアとしては、システム上での多言語対応はほとんどしなくて良くなるのでかなり開発が楽になります。

しかし、サービスの特性上、ユーザーがサイトを開いたときに、表示されている文言がすべてWOVNに送信され自動登録されてしまいます。個人情報や、動的な数値などがすべて登録されてしまい、セキュリティ面や費用面でも問題があります。

## wovn-ignore 属性について

そこで、抽出対象外を設定する `wovn-ignore` 属性というものが用意されています。

https://wovn-support.zendesk.com/hc/ja/articles/360007899791

HTML属性に `wovn-ignore` を追加するだけでそのタグ内はすべて除外されます。

```html
会員名：<span wovn-ignore>おおいし</span>様
```

ちなみに似た設定に「コンテンツ除外ルール設定」や「ライブ編集」があるのですが、WOVNにログインしないと対象かわからなかったり、誤って設定を変更してしまったり、HTMLの構造変更で対象が認識されないなどの危険があります。確実な運用ができないので避けていました。

## wovn-ignore を入れ忘れる

WOVNを入れる予算が確保できるほどの大規模サイトであれば、wovn-ignore属性を設定するべき箇所は無数にあるはずです。

そんなプロジェクトで何人もの開発者が携わると絶対に発生するのが属性の「入れ忘れ」、「スコープ漏れ」、そして「スペルミス」です。しかも、入れ忘れていないか確認する方法がソースコードを確認するしかないという苦行です。

そこで、絶対に誰が開発しても「入れ忘れ」、「スコープ漏れ」と「スペルミス」を防止できる方法を発明したので、皆さんに伝授します。

```css
*[wovn-ignore] {
  background: fuchsia;
}
```

https://codesandbox.io/embed/wovn-ignore-ucer9y?theme=light&view=preview

なんということでしょう！wovn-ignore属性のただのテキストがたった3行のCSSでドギツい赤紫色に生まれ変わったではありませんか！

これなら絶対に間違えません。しかも、サイズなどに変更がまったくないのでデザインの影響もありません。 私が携わっていた案件では、あまりにもミスが多かったので開発環境のみこのCSSが設定されています。さすがに…という場合はブラウザのユーザースタイルシートや開発者ツールで設定するという方法もあります。

サイトのデザインと同化せず存在感を放つ色を選定してみてください。
