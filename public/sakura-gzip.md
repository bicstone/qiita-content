---
title: さくらのレンタルサーバにてGZIP圧縮が出来ないときの対処法
tags:
  - Apache
  - プログラミング
  - gzip
  - htaccess
  - さくらのレンタルサーバ
private: false
updated_at: "2024-03-09T15:55:24+09:00"
id: 521a2a9b8ae17463ee58
organization_url_name: null
slide: false
ignorePublish: false
---

さくらのレンタルサーバにてmod_deflateを用いたGZIP圧縮が出来ないときの対処法です。

※ これは2016-09-28に[個人ブログ](https://bicstone.me)で公開した記事を移植したものです。

## 対処方法

.htaccessで「`SetOutputFilter DEFLATE`」を記載してもgzip圧縮は出来ない場合があります。原因は「Webアプリケーションファイアウォールの設定」です。

さくらのレンタルサーバのコントロールパネルから「Webアプリケーションファイアウォールの設定」を開いてみてください([直リンク](https://secure.sakura.ad.jp/rscontrol/rs/waf))。対象のドメインにおいて「利用する」となっている場合は無効に設定してください。

強制的にGZIPを使用するようにする「`SetEnv force-gzip`」を指定するのは避けましょう。

## 原因

Webアプリケーションファイアウォールはアクセス時に一部の情報を切り取ってしまいます。その情報の中には、ブラウザがGZIP圧縮に対応しているという情報(`HTTP_ACCEPT_ENCODING`)が含まれています。詳細はこちらの記事に掲載しています。

https://bicstone.me/siteguard-environment-variable/
