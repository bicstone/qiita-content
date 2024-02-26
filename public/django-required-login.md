---
title: Djangoでログイン必須サイトにする方法
tags:
  - Python
  - Django
  - プログラミング
  - 認証
private: false
updated_at: "2024-02-26T22:14:21+09:00"
id: b1b45ec1de34f74d8a4b
organization_url_name: null
slide: false
ignorePublish: false
---

Djangoですべてのページにおいてログインをしていなければログインページにリダイレクトする方法を紹介します。

※ これは2019/7/19に個人ブログで公開した記事を移植したものです。

## 標準ではデコレータを使用する

公式ドキュメントによれば、ビュー層のデコレーターを使用すれば実装が可能です。

https://docs.djangoproject.com/ja/2.2/topics/auth/default/#the-login-required-decorator

```py
from django.contrib.auth.decorators import login_required

@login_required
def my_view(request):
    # ...
```

しかし、これをすべてのビューに実装するのは手間ですし、1ページでも実装を忘れてしまったら大変なことになります。

## ミドルウェアの追加

下記を参考に、ミドルウェア(プラグインのようなもの)を追加し、対処します。リクエストを受けた段階でログイン済みか判定し、ログインしていなければリダイレクトするミドルウェアを追加します。

https://docs.djangoproject.com/ja/2.2/topics/http/middleware/

例えば `/app/middleware/auth.py` にミドルウェアのPythonファイルを作成します。リダイレクトループを防ぐため、ログインのURL`/login/` にアクセスされた場合はログインしていない場合もリダイレクトしません。

```py
from django.http import HttpResponseRedirect
from django.utils.deprecation import MiddlewareMixin

class authMiddleware(MiddlewareMixin):
    def process_response(self, request, response):
        if not request.user.is_authenticated and request.path != '/login/':
            return HttpResponseRedirect('/login/')
        return response
```

## setting.py を編集

`setting.py` の `MIDDLEWARE` 変数に今回追加したミドルウェアを追加します。

```py
MIDDLEWARE = [
    'app.middleware.auth.authMiddleware',
]
```

## 動作の様子

ログインしていなければ次のとおりログイン画面にリダイレクトされます。

![HTTP レスポンスヘッダーに `/login/` へのリダイレクトが含まれている](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/452d29cf-bb0e-2fd8-0c5e-9a7b083ce16e.png)
