---
title: Real-Time Find and Replace で置換されないときの対処法
tags:
  - PHP
  - JavaScript
  - WordPress
  - プログラミング
private: false
updated_at: "2024-03-07T23:28:52+09:00"
id: ab8aacfa48d3c3f96a61
organization_url_name: null
slide: false
ignorePublish: false
---

Real-Time Find and Replaceプラグインでの置換が適用されないときの対象方法を紹介します。

※ これは2016-09-26に[個人ブログ](https://bicstone.me)で公開した記事を移植したものです。

## プラグインの概要

https://ja.wordpress.org/plugins/real-time-find-and-replace/

WordPressで表示される公開ページの置換、正規表現置換ができるプラグインです。

## 置換されない…

Autoptimizeプラグインで生成されたjs、cssファイルのURLを変更するために使用したのですが、置換作業がうまくいきませんでした。

## 原因

AutoptimizeプラグインやReal-Time Find and Replaceプラグインも `template_redirect` フックを利用していました。優先順位がどちらも同一であったため、名前順でAutoptimizeプラグインの出力のバッファリングが先に開始されてしまったことが原因です。

## 対処

Real-Time Find and Replaceプラグインの出力のバッファリングの優先順位をあげます。「real-time-find-and-replace.php」の最終行を次のように変更します。-1は場合によって変更してください。Autoptimizeは0でしたので、-1を設定しました。

```diff
- add_action( 'template_redirect', 'far_template_redirect');
+ add_action( 'template_redirect', 'far_template_redirect' ,-1);
```
