---
title: PythonでアーカイブしたZIPファイル名をShift-JISにする
tags:
  - Python
  - Windows
  - プログラミング
  - zip
  - Shift-JIS
private: false
updated_at: "2024-03-03T20:24:24+09:00"
id: 14ef11e80cf8d36004c2
organization_url_name: null
slide: false
ignorePublish: false
---

Pythonのzipfileライブラリはファイル名をUTF-8でエンコードするためWindowsのレガシーな解凍ソフトで解凍するとファイル名が文字化けします。ファイル名をShift-JISでエンコードする方法を紹介します。

※ これは2019-07-18に個人ブログで公開した記事を移植したものです。

## 世の中には UTF-8 で文字化けする解凍ソフトが残っている

2007年9月にZIPの仕様が追加され、エンコードをUTF-8に統一するための仕組みが完成してから、ZIPファイル名はUTF-8にエンコードすることがスタンダードになりつつあります。

https://support.pkware.com/home/pkzip/developer-tools/appnote

しかし、日本国内では、UTF-8に未対応の解凍ソフト(+Lhaca Lhaplusなど)を使っているユーザーが多くいます。(WindowsのエクスプローラーだけでUTF-8のZIP解凍できるのに)

## ファイル名を Shift-JIS でエンコードできるのか

全ユーザーを説得して各UTF-8未対応の解凍ソフトをアンインストールして回るわけにもいかないので、ファイル名をShift-JISでエンコードする方法を考えます。

まず、CPythonのzipfileライブラリのソースコードを見てみます。

https://github.com/python/cpython/blob/3.7/Lib/zipfile.py#L454

CPython3.7では、454行目にファイル名のエンコードの関数があります。抜粋すると次の通りです。

```py
    def _encodeFilenameFlags(self):
        try:
            return self.filename.encode('ascii'), self.flag_bits
        except UnicodeEncodeError:
            return self.filename.encode('utf-8'), self.flag_bits | 0x800
```

まずASCIIでのエンコードを施行して、 `UnicodeEncodeError` ならば(=ASCIIでなければ)「UTF-8」でエンコードするようにハードコードされていました…。

## 標準ライブラリをコピーして Shift-JIS に変更する

あまりしたくないですが…。標準ライブラリをコピーしてShift-JISに対応します。 454行目の `'utf-8'` を `'cp932'` に変更します。また、フラグ `0x800` を削除します。

ちなみに、`0x800` は、ZIPの規格で定められた「general purpose bit flag」のBit 11を `1` にしています(16進数 `800`=2進数 `100000000000`)。これは、前述のエンコードをUTF-8に統一するための仕組みで、規格では「Language encoding flag (EFS). If this bit is set, the filename and comment fields for this file MUST be encoded using UTF-8.」と定められており、UTF-8でなければ立ててはいけないフラグなので取り除いています。

https://pkware.cachefly.net/webdocs/casestudies/APPNOTE.TXT

zipfile.py

https://github.com/python/cpython/blob/3.7/Lib/zipfile.py#L454

```diff
454-: return self.filename.encode('utf-8'), self.flag_bits | 0x800
454+: return self.filename.encode('cp932'), self.flag_bits
```

`import` 先を今回作ったzipfile.pyに変更します。

```py
from .zipfile import ZipFile
# ...
with ZipFile(zb, mode='w') as zf:
    # ...
```

以上の対策で、Windowsのレガシーな解凍ソフトで解凍できるようになります。逆にLinuxなどではUTF-8しか対応していない場合文字化けする可能性があります。
