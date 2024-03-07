---
title: Djangoのテンプレートでデバッグモードを判定する
tags:
  - Python
  - Django
  - プログラミング
private: false
updated_at: "2024-02-26T22:19:26+09:00"
id: adf1d31d47e31fbfe215
organization_url_name: null
slide: false
ignorePublish: false
---

Djangoのテンプレートすべてのページから、`{% if DEBUG %}` を用いてデバッグモードを判定できるようにする方法を紹介します。`context_processors` を用いることで行います。

※ これは2019/7/23に[個人ブログ](https://bicstone.me)で公開した記事を移植したものです。

## ビューでデバッグモードを判定する方法

Djangoのビューでデバッグモードを判定する方法として下記があります。

```py
from django.conf import settings
DEBUG = settings.DEBUG
print(DEBUG)  # True
```

いちいちこの変数をテンプレートに渡すのは面倒なので、`context_processors` を用いて `DEBUG` 変数を定義してみます。

## `django.template.context_processors.debug` は微妙

似たような機能として、標準でも `django.template.context_processors.debug` が存在します。

https://docs.djangoproject.com/ja/2.2/ref/templates/api/#django.template.context_processors.debug

しかし、この機能は `INTERNAL_IPS` を適切に設定していないと機能しません。

https://docs.djangoproject.com/ja/2.2/ref/settings/#internal-ips

Dockerなどを用いると、ローカル開発環境でさえも様々なIPからアクセスされるのでなかなか使いにくいです。

## context_processors を用いた DEBUG 変数定義

そこで、`INTERNAL_IPS` を気にせずsettings.pyのDEBUGを問答無用でテンプレートから呼び出せるcontext_processorsを作ってみます。

app/context_processors.pyに次のプログラムを追加します。

```py
from django.conf import settings

def is_debug(request):
    return {"DEBUG": settings.DEBUG}
```

そして、setting.pyの `TEMPLATES['OPTIONS']['context_processors']` に今回追加したcontext_processorsを追加します。

```py
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',  # ←消して良い
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
                'app.context_processors.is_debug',  # 追加
            ],
        },
    },
]
```

以上で、`{% if DEBUG %}` が利用できます。
