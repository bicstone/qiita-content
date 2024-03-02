---
title: Djangoで削除時に外部キー制約によって例外が発生する問題の対処法
tags:
  - "Programming"
  - "Python"
  - "django"
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

Djangoで汎用View `django.views.generic.DeleteView` を用いた削除時に外部キー制約によってProtectedErrorの例外が発生するのをキャッチする方法を紹介します。

※ これは2020/02/05に個人ブログで公開した記事を移植したものです。

## PROTECT されている場合に削除しようとする場合の挙動

Djangoでは、モデルに外部キーを設定できます。モデルの第二引数では、参照先が削除等された場合の挙動が設定できます。

```py
class Comment(models.Model):
 user = models.ForeignKey(User, models.PROTECT)
```

ここで、参照先を削除できなくする `models.PROTECT` という設定ができます。この設定をした場合、参照先は削除できなくなります。

そして、公式ドキュメントにはこのような記述があります。

> Prevent deletion of the referenced object by raising ProtectedError, a subclass of django.db.IntegrityError.

https://docs.djangoproject.com/ja/3.0/ref/models/fields/#arguments

削除を試みると、 ProtectedError例外が発生するという仕様です。例外ということは、誤って削除が試みられると500エラーとなってしまいます。

## 汎用 View でシンプルに実装したい

一方で、せっかくDjangoで実装するのですから、汎用Viewでリレーション確認などの記述無しでシンプルに削除したいですよね。そこで次のように構築したとします。

```py
class UserDeleteView(DeleteView):
  model = User
  success_url = reverse_lazy('user-list')
```

ここで先程示したように他のモデルからPROTECTされていた場合、UserDeleteViewはProtectedError例外エラー(500エラー)を返してしまいます。

## 例外をキャッチする

Viewでは例外をキャッチするのみの実装とします。

https://docs.djangoproject.com/ja/3.0/misc/design-philosophies/

ほとんど汎用View使う理由がないような感じはしますが、post関数を上書きします。

```py
class UserDeleteView(DeleteView):
  model = User
  success_url = reverse_lazy('user-list')

  def post(self, request, *args, **kwargs):
    try:
      obj = self.get_object()
      obj.delete()
    except models.ProtectedError as e:
      messages.error(request, f'「{obj}」は紐付けられているため削除できません。')
      return redirect('user-list')
```

これで、ProtectedErrorが発生したら削除されず、フラッシュメッセージが表示されます。
