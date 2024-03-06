---
title: CSSで勝手に英語大文字になってしまう原因と対処法
tags:
  - PHP
  - HTML
  - CSS
  - WordPress
  - プログラミング
private: false
updated_at: "2024-03-06T14:42:00+09:00"
id: 6c4f86383b4b275209f0
organization_url_name: null
slide: false
ignorePublish: false
---

WordPressでタイトルが勝手に英語大文字になってしまう原因と対処方法を紹介します。

※ これは2016-03-10に個人ブログで公開した記事を移植したものです。

## 現象

とあるWordPressテーマを適用すると、タイトルの英語部分が勝手に大文字になってしまいました(画像はイメージです)。

![記事のタイトルが管理画面に入力したとおり表示されている](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/f1e40f31-31cb-98c3-587b-0969a7d418b7.png)

↓

![記事のタイトルの英字がすべて大文字になっている](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/f48caebb-2187-a0e7-eb40-1025ccc0d6f4.png)

## 対処方法

CSSによって大文字に変更されています。よって、テーマのCSS上に存在する、下記2つのスタイルをすべて削除することで改善します。

```css
text-transform: uppercase;
```

```css
text-transform: capitalize;
```

https://developer.mozilla.org/ja/docs/Web/CSS/text-transform

「uppercase」はすべて大文字、「capitalize」は1文字目のみ大文字にするように指定したものです。

親テーマのCSSを変更することに問題がある場合は子テーマのCSSへ対象のセレクタに次のスタイルを指定します。

```css
text-transform: none;
```
