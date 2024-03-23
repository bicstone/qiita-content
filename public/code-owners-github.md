---
title: "Code Owners を設定して GitHub プルリクストのレビュアーを自動アサイン"
tags:
  - GitHub
  - CodeOwners
  - チーム開発
  - 開発体験
  - DeveloperExperience
private: false
updated_at: ""
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

GitHubで `CODEOWNERS` を作成し、プルリクエストのレビュアーアサイン自動化や責務の明確化などを行うことができます。

:::note warn

※ これは2022-12-14に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## 背景

モノレポのリポジトリ運用が始まった際にプルリクエストの運用で次の課題を感じていました。

- ディレクトリごとにメインの担当者が異なり、レビュアーに抜けが発生する
- スキーマの変更のルリクエストを確認してほしいレビュアーがとても多く、プルリクエスト作成時に毎回手動指定するのが大きな手間

最初はWorking Agreement (チーム内のルール) でカバーしていたのですが、GitHubの機能であるCode Ownersで仕組み化することにしました。

## Code Owners とは

> CODEOWNERS ファイルを使い、リポジトリ中のコードに対して責任を負う個人あるいは Team を指定できます。

https://docs.github.com/ja/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners

この機能を使うことで、ファイル / ディレクトリごとにコードの責任者を指定できます。

## CODEOWNERS ファイルの作成方法

`.github/CODEOWNERS` を作成し、.gitignoreと同じ構文で設定できます。意外と知られていないのですがメールアドレスでもOKです。チームも設定できます。

```plain
/project-a/ @bicstone
/project-b/ t.oishi@example.com
/project-c/ @octo-org/octocats
.graphqls @bicstone t.oishi@example.com @octo-org/octocats
```

ちなみにCODEOWNERS自体をGitHubで開くと構文チェックの結果が表示されるので、リポジトリに対するユーザーの権限が無くなった場合などに検知できます。

![CODEOWNERS を GitHub で開いたスクリーンショット。 "This CODEOWNERS file is valid." と表示されている。](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/35759808-eb8a-4553-1f30-ad39bb03f0cf.jpeg)

## 効果

### レビュアーが自動でアサインされる

該当のファイルに対するPRを作成すると、レビュアーが自動でアサインされます。チームにおいてはシャッフラーなどの機能も使用されます。

![GitHub のプルリクエストのスクリーンショット。 Reviewers に bicstone が Code owner として自動で追加されている。](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/c3da29ef-f632-efed-e38a-dcca644919b7.jpeg)

また、Branch protection ruleに "Require review from Code Owners" というルールがあり、これを設定するとマージにはCode Ownersの承認を必須にできます。

https://docs.github.com/ja/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/managing-a-branch-protection-rule#creating-a-branch-protection-rule

### コードに Code Owners が表示される

GitHub上でファイルを開くと、該当のCode Ownersが表示されます。開発した人 (Contributors) だけでなく、PRを開かずも承認したチーム (Code Owners) がわかるのは大きく、後々役立つことが多いです。

![GitHub 上でファイルを開いた画面のスクリーンショット。このファイルの Code Owners が表示されている。](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/cca8847b-fc62-ba04-8bac-2d3c671ec4df.jpeg)

## まとめ

GitHubのCode Owners機能を活用することで、モノレポ環境においての開発体験 (Developer Experience) の向上を実現できました。
