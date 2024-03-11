---
title: django_datatables_viewで1ページ100件超えが表示できない不具合の対処法
tags:
  - Python
  - Django
  - プログラミング
  - DataTables
  - django-datatables-view
private: false
updated_at: "2024-03-07T23:33:04+09:00"
id: 70e5fbaed37799484e0a
organization_url_name: null
slide: false
ignorePublish: false
---

Djangoでdatatablesのサーバー連携を使用できるdjango_datatables_viewで、1ページあたり100件を超えるレコードがすべて表示できない不具合の対処法を紹介します。とてもわかりにくい不具合になっており、目視だと気が付かない可能性があります。

:::note warn

※ これは2021/03/28に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## django_datatables_view について

django_datatables_viewとは、datatablesのサーバー連携を使用できるDjangoのクラスベースビューです。  
datatablesとは絞り込み・ソートなど多機能なテーブルをかんたんに実装できるjQueryプラグインです。

Djangoの管理画面よりもカスタマイズ豊富な上に、ほとんどプログラムを書くことなく、（デザイン, フロントエンド, バックエンドも）何も考えずにデータベース内容を一覧に表示できます。

![django-datatables-viewを使用しているテーブルのスクリーンショット](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/b9f9deba-cd7e-6fad-ef2d-86d9c3c26390.png)

今回のユースケースでは、次のプログラムをお借りしました。ありがとうございます。

https://github.com/okoppe8/django-datatables-view-sample

## 1 ページあたり 100 件超えにするとレコード数がおかしい

datatablesの `lengthMenu` を変更すると、1ページあたりの表示件数を柔軟に変更できます。

10件でも、1件でも、2^53-1件でも設定できます。

![datatables で件数が変更できる部分のスクリーンショット](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/25b47c4c-a350-9707-e374-64e5b4096a23.png)

https://datatables.net/reference/option/lengthMenu

ある案件で、最大「500件」の選択ができるように設定していました。500件にすると次のようになります。

![datatables で表示件数500件にしたスクリーンショット。しかし、1件目から100件目しか表示されていない。](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/00d7526d-35e9-27c5-e709-203e28366cb5.png)

一見すると、正しく表示できていそうに見えますが、皆さん、このスクショにある「不具合」を１点見つけてくださいませ。

いくら北海道がデカいとはいえ、地方自治体が500もあるわけがないのです。（ちなみに179あるらしいです）

試しに次のページに行ってみましょう。

![datatables の表示件数500件にして2ページ目に行ったスクリーンショット。501件目から表示される。](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/021c0e06-6053-d40e-18f2-e2a0f59d51e5.png)

群馬県に突入しています。つまり、「1 ～ 500件表示」となっていながら、500件のレコードが表示できていないのです。実際には100件しか表示されていません。

その案件ではデフォルトは100件にしており、テストもそれで通っていたので目視でしか確認しておらず発見できていませんでした。気づきにくい重大な不具合を生んでしまったのです。

## django_datatables_view の返却レコード数上限

django_datatables_viewには返却レコード数上限の設定( `max_display_length` )があります。README.mdには次の記述があります。

> set max limit of records returned, this is used to protect our site if someone tries to attack our site  
> and make it return huge amount of data max_display_length = 500

https://pypi.org/project/django-datatables-view/

リクエスト自体は単純なGETのため、やろうと思えば大量のデータを取得する攻撃ができてしまう意図から設定されている設定です。こちらのデフォルト値は `100` になっています。

## 返却レコード数上限を変更する

正常に動作させるためには返却レコード数上限を変更する必要があります。`lengthMenu` の最大値を設定しましょう。

```py
from django_datatables_view.base_datatable_view import BaseDatatableView

class ExampleList(BaseDatatableView):
  model = MyModel
  columns = ['id', 'name', 'dateAt', 'updatedAt']
  max_display_length = 500
```

再度実行してみましょう。

![表示件数を500件にしたdatatablesのスクリーンショット。1-500件目が正しく表示されている。](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/04f4d670-500c-45f9-6ccf-408296c61659.png)

無事群馬県までの500件が表示されるようになりました。

## 基本設定のクラス化

複数のページで使用している場合、設定漏れがあると不具合の原因となるので、基本設定は基底クラス化してしまいましょう。次のように作ります。

```py
from django_datatables_view.base_datatable_view import BaseDatatableView

class ExtendDatatableView(BaseDatatableView):
  # フロントdatatablesのlengthMenu最大値と揃えること
  max_display_length = 500
```

```py
from app.library.extend_datatable_view import ExtendDatatableView

class ExampleList(ExtendDatatableView):
  model = MyModel
  columns = ['id', 'name', 'dateAt', 'updatedAt']
```

## まとめ（反省）

- ライブラリのREADMEはちゃんと読もう
- 目視で不具合に気がつけるようにテストデータは分かりやすくしよう
- テストは条件変更した場合の挙動も確認しよう
