---
title: ".gitattributes をもっと活用して GitHub 上での開発体験を向上させよう"
tags:
  - Git
  - GitHub
  - コードレビュー
  - 開発体験
  - .gitattributes
private: false
updated_at: ""
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

.gitattributesを活用しプルリクエストレビューの負荷を軽減して、開発体験を高めた記録です。

:::note warn

※ これは2022-12-15に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## 背景

プルリクエストの差分を確認する際、パッケージマネージャーのロックファイルや自動生成ファイル (CodeGen) が差分となって出てくることがあります。

自動生成ファイルは人が読むものではないため、レビューにおいて邪魔に感じてしまいレビュー負荷が増加してしまうことがあります。

また、実際よりも差分が大きく見えてしまうことで、レビュワーがレビューを後回しにしてしまうことで、リリースの速度の低下にも繋がっていました。

そこで、 `.gitattributes` を適切に設定してみることにしました。

## `.gitattributes` とは

もともとGitで `.gitattributes` の設定が存在します。下記ではツールを利用してバイナリ差分を見やすくする方法について解説されています。

https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%9E%E3%82%A4%E3%82%BA-Git-%E3%81%AE%E5%B1%9E%E6%80%A7

これに加え、Githubでは `linguist-generated` というオリジナルの属性が用意されています。

https://docs.github.com/ja/repositories/working-with-files/managing-files/customizing-how-changed-files-appear-on-github

## 使用例

`linguist-generated` を指定すると、該当のファイルは差分としてデフォルト折りたたまれるようになり、差分の行数としてカウントされなくなります。

```plain
yarn.lock linguist-generated=true
```

![GitHub のプルリクエストのスクリーンショット。yarn.lock の差分がデフォルトで閉じられており、「Some generated files are not rendered by default」と表示されている。](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/615b8492-a8a9-0a13-4e4e-456490fd91d5.png)

ちなみにドキュメントを見ると他にもいくつかの属性があり、GitHub上に表示される言語統計情報などをカスタマイズできます。

https://github.com/github/linguist/blob/master/docs/overrides.md

- `linguist-detectable`
- `linguist-documentation`
- `linguist-generated`
- `linguist-language`
- `linguist-vendored`

## まとめ

`.gitattributes` を活用し、ロックファイルや自動生成ファイルの差分を折りたたむように設定してみました。
