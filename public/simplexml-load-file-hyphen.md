---
title: simplexml_load_fileで要素名にハイフンがついていると読み込めない
tags:
  - PHP
  - XML
  - プログラミング
  - simplexml_load_file
private: false
updated_at: "2024-03-09T16:08:55+09:00"
id: ece07d9b83efce1dd1c1
organization_url_name: null
slide: false
ignorePublish: false
---

simplexml_load_fileで要素名にハイフンがついていると読み込めず、「`Parse error: syntax error, unexpected ‘[‘`」エラーが発生する場合の対処方法です。

※ これは2016-10-25に[個人ブログ](https://bicstone.me)で公開した記事を移植したものです。

## 現象

simplexml_load_fileで取得できなかったUser-Agents.orgのxmlは次のとおりになっていました(抜粋)。

```xml
<user-agents>
  <user-agent>
    <String></String>
  </user-agent>
</user-agents>
```

次のPHPコードを実行するとエラーが発生しました。

```php
<?php
$xml=simplexml_load_file("http://www.user-agents.org/allagents.xml");
print($xml->user-agent[0]->String);
?>
```

```plain
Parse error: syntax error, unexpected ‘[‘ in test.php on line 3
```

## 原因

メンバ変数名に特殊文字が入っているため発生します。

https://xtech.nikkei.com/it/article/COLUMN/20080619/308711/

## 対処方法

次のように変更しました。

```php
$xml->user-agent[0]->String;
```

↓

```php
$xml->{'user-agent'}[0]->String;
```
