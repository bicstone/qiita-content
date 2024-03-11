---
title: Djangoで全ページキャッシュ禁止にする方法
tags:
  - Python
  - Django
  - プログラミング
  - cache-Control
private: false
updated_at: "2024-03-11T21:30:21+09:00"
id: 77930ded65dbfa5a1344
organization_url_name: null
slide: false
ignorePublish: false
---

WSGIサーバで動作させているDjangoのWebサイトで、全ページにHTTPヘッダーを付加し、キャッシュ禁止にする方法を紹介します。

:::note warn

※ これは2019/7/17に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## キャッシュ禁止にしたい

WSGIサーバで動作させているDjango2.2のWebサイトにおいて、全ページにキャッシュ禁止のHTTPヘッダーを付加し、キャッシュ禁止にしたいときがありました。

下記を参考に、ミドルウェア(プラグインのようなもの)を追加し、対処します。

https://docs.djangoproject.com/ja/2.2/topics/http/middleware/

## ミドルウェアの追加

モダンブラウザであれば、`Cache-Control` ヘッダーのみで十分なキャッシュ制御ができます。

https://developer.mozilla.org/ja/docs/Web/HTTP/Headers/Cache-Control

`Cache-Control` ヘッダーを追加するために、例えば `/app/middleware/nocache.py` にミドルウェアのPythonファイルを作成します。

```py
from django.conf import settings
from django.utils.deprecation import MiddlewareMixin

class noCacheMiddleware(MiddlewareMixin):
    def process_response(self, request, response):
        response['Cache-Control'] = 'private, no-store, no-cache, must-revalidate, no-transform'
        return response
```

## setting.py を編集

`setting.py` の `MIDDLEWARE` 変数に今回追加したミドルウェアを追加します。

```py
MIDDLEWARE = [
    'app.middleware.nocache.noCacheMiddleware',
]
```

## 動作の様子

Djangoレスポンスの例(developモードON)

![HTTP レスポンスヘッダに Cache-Control が追加された](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/ae5ffbcd-46bc-fd10-ee5a-4ad4748a597f.png)
