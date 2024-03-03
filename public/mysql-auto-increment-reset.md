---
title: MySQL で、 AUTO INCREMENT の値が時々リセットされる原因
tags:
  - MySQL
  - プログラミング
  - RDBMS
  - InnoDB
  - autoincrement
private: false
updated_at: "2024-03-03T20:26:26+09:00"
id: 480d7a443eebd0a856e2
organization_url_name: null
slide: false
ignorePublish: false
---

MySQL 5.6のInnoDBデータベースでAUTO INCREMENTのカウンター値が時々リセットされ、物理削除後の値が再度使用されてしまう可能性がある原因と対処法を紹介します。

## 現象

私が関わっていたプロジェクトで、AUTO INCREMENTのカラムなのに物理削除後の値が再利用されるという不具合が発生しました。

ユーザーテーブルとして、会員IDをプライマリキーかつAUTO INCREMENTで設定しており、削除したらテーブルから物理削除する仕様になっていました。

ある日、削除済みの会員IDと、登録されていた会員IDが重複するという不具合が発生しました。

## 原因

MySQL5系のInnoDBデータベースでは、再起動されるとAUTO INCREMENTカウンターがリセットされ、次のSQL結果の値に設定されると公式ドキュメントに記載されています。

```sql
SELECT MAX(ai_col) FROM t FOR UPDATE;`
```

https://dev.mysql.com/doc/refman/5.7/en/innodb-auto-increment-handling.html

MySQLが再起動されると連番の一番大きい値にカウンターがリセットされるという実装になっています。

つまり、最大値のレコードが物理削除された後に再起動されると、その値が再度使用されてしまうということです。AUTO INCREMENTカウンターをシーケンスであると完全に信頼して設計していたので、拍子抜けしましたし、実装を理解せずに使用して恥ずかしいです。

## 対処方法

### MySQL のバージョンアップ

MySQL8系ではこの実装が変更されています。

公式ドキュメントによると、カウンター値をメモリでなくディスクに保存するように変更されたので、MySQLの再起動後もカウンター値が維持されるとのことです。

バージョンアップできれば、バージョンアップで対処できます。

> In MySQL 8.0, this behavior is changed. The current maximum auto-increment counter value is written to the redo log each time it changes and is saved to an engine-private system table on each checkpoint. These changes make the current maximum auto-increment counter value persistent across server restarts.

https://dev.mysql.com/doc/refman/8.0/en/innodb-auto-increment-handling.html#innodb-auto-increment-initialization

### 論理削除にする

論理削除にすれば、レコードが削除されないのでリセットされても影響はなくなります。

### 採番テーブルを使用する

今回のプロジェクトでは、急遽古き良き採番テーブルを使用する方法を取りました。

## まとめ

特定のRDBMSの機能に依存するのは手軽ですが、仕様を確認して見極めて使っていきたいです。
