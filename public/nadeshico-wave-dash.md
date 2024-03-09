---
title: なでしこで「～」が文字化けする際の対処方法
tags:
  - なでしこ
  - プログラミング
  - 文字化け
  - Shift-JIS
  - Nadesiko
private: false
updated_at: "2024-03-09T15:55:23+09:00"
id: 925b8a6df4c2faaca10a
organization_url_name: null
slide: false
ignorePublish: false
---

なでしこで「～」が「?」へ文字化けすることがあります。その対処方法です。

※ これは2016-10-22に[個人ブログ](https://bicstone.me)で公開した記事を移植したものです。

## 波ダッシュ問題

いわゆる波ダッシュ問題によるものです。詳細はWikipediaを見てください。

https://ja.wikipedia.org/wiki/%E6%B3%A2%E3%83%80%E3%83%83%E3%82%B7%E3%83%A5#Unicode.E3.81.AB.E9.96.A2.E9.80.A3.E3.81.99.E3.82.8B.E5.95.8F.E9.A1.8C

## 対処方法

なでしこにおける「SJIS変換」や「UTF8N変換」などを用いず、「NKF変換」を用いるようにしましょう。「SJIS変換」や「UTF8N変換」などはWindows環境以外のShift-JISと判断して変換します。「MKF変換」は状況に応じて適切に変換します。

http://nadesi.com/man/index.php?NKF%E5%A4%89%E6%8F%9B

「NKF」とはLinux標準の命令です。

https://xtech.nikkei.com/it/article/COLUMN/20060227/230849/?rt=nocnt

Shift-JISに変換する場合は、`「s」でNKF変換`

UTF-8に変換する場合は、`「w」でNKF変換`

とします。
