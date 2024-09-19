---
title: Backlog の課題に GitHub のプルリクを連携する GitHub Action を作りました
tags:
  - GitHub
  - Backlog
  - nulab
  - actions
  - GitHubActions
private: false
updated_at: "2024-09-19T13:42:13+09:00"
id: c72d184d7327c7b1d649
organization_url_name: null
slide: false
ignorePublish: false
---

プルリクストをBacklog課題のコメントに追加するGitHub Actionを公開しました。ぜひご利用ください。

:::note warn

※ これは2022-12-01に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[MIT](https://github.com/bicstone/backlog-notify/blob/v5.1.0/LICENSE)で提供しています。情報は古い可能性があります。

:::

## GitHub Action

GitHub Actionとは、GitHub内で完結するCI / CDツールです。個人開発や仕事で大変お世話になっています。

https://github.co.jp/features/actions

そのうち、仕事で活用できたパッケージを1つ公開し、GitHub Marketplaceに公開しています。

## Backlog Notify

プッシュされたりプルリクストが作成されたらBacklog課題のコメントにリンクコメントを送信するGitHub Actionです。キーワードによる課題の状態変更も可能です。

https://github.com/marketplace/actions/backlog-notify

## 使用方法

### プルリクエスト

![Backlog Notifyの動作をイメージした図。GitHub にプルリクエストを作成すると Backlog にプルリクエスト情報のコメントがされる](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/18a6ead9-f266-5ad4-6f6d-5d8363534948.png)

プルリクエストのタイトルの中に課題番号 (例: `PROJECT-123` ) がある場合は、その課題にプルリクエストに関するコメントを送信します。課題キーは先頭にある1つのみ認識します。

```plain
PROJECT-123 不具合修正
```

また、キーワードがある場合は、マージされたタイミングで課題ステータスを変更します。キーワードは末尾にある1つのみ認識します。

- `#fix` `#fixes` `#fixed` のどれかで処理済み
- `#close` `#closes` `#closed` のどれかで完了

```plain
PROJECT-123 不具合修正 #fix
```

### プッシュ

![Backlog Notifyの動作をイメージした図。GitHub にプッシュすると Backlog にコミット情報のコメントがされる](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/86771035-f931-b739-3794-9190c879e4b3.png)

コミットメッセージの中に課題番号 (例: `PROJECT-123` ) がある場合は、その課題にコミットログに関するコメントを送信します。課題キーは先頭にある1つのみ認識します。

```plain
PROJECT-123 不具合修正
```

また、キーワードがある場合は、プッシュされたタイミングで課題ステータスを変更します。キーワードは末尾にある1つのみ認識します。

- `#fix` `#fixes` `#fixed` のどれかで処理済み
- `#close` `#closes` `#closed` のどれかで完了

ちなみに空コミットすることで、Backlogを開かずともコメントを残したりステータスを変更できます。

```plain
PROJECT-123 不具合修正 #fix
```

## 設定方法

### BOT ユーザーの作成

1. Backlogのプロジェクトに移動します。
2. プロジェクト設定 → 参加ユーザー → 新しいユーザの追加はこちらからを選択します。
3. クラシックプランの場合は `一般ユーザ` 、新プランの場合は `ゲスト` を選択します。
4. 登録します。

![Backlog.com のスクリーンショット 「プロジェクト設定」→「参加ユーザー」→「新しいユーザーの追加はこちら」を選択する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/bfcced93-0d02-e4ec-4ae6-a9d0f9ea38d3.png)

### Backlog API キーの取得

1. 登録したBOTアカウントにログインします。
2. 個人設定 → API → 登録でAPIキーを発行します。

![Backlog.com のスクリーンショット 「ユーザー設定」→「API設定」からAPIを発行します](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/ee20f691-7b12-59eb-e2ec-549eefd589e8.png)

### API キーを GitHub に登録

1. GitHubのリポジトリページに移動します。
2. Setting → Secrets → Add a new secretを選択します。
3. Nameは `BACKLOG_API_KEY` とし、 ValueにAPIキーをそのまま貼り付けます。
4. 登録します。

![GitHubのプロジェクトスクリーンショット　リポジトリの「Settings」→「Secrets」→「Add a new secret」 を選択して、 BACKLOG_API_KEY を追加する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/dca8a148-1970-ab41-98a0-3a230ca81863.png)

### collaborator による workflow の実行を制限

プライベートリポジトリの場合は次の操作する必要はありません。\
パブリックリポジトリの場合は、collaboratorからのworkflowの実行を制限してください。

1. Setting → Actions → Fork pull request workflows from outside collaboratorsを開きます。
2. `Require approval for all outside collaborators` を選択します。

![GitHubのプロジェクトスクリーンショット　リポジトリの「Settings」→「Actions」→「Fork pull request workflows from outside collaborators」](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/5dd7b122-029e-b551-007c-d9fd7e315133.png)

### Workflow の作成

GitHub Actions workflowを作成します (例: `.github/workflows/backlog-notify.yml` )。

```yaml
name: Backlog Notify

on:
  push:
  pull_request:
    types:
      - opened
      - reopened
      - ready_for_review
      - closed

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Backlog Notify
        uses: bicstone/backlog-notify@v4
        with:
          project_key: PROJECT_KEY
          api_host: example.backlog.jp
          api_key: ${{ secrets.BACKLOG_API_KEY }}
```

## 高度な設定

[Workflow syntax for GitHub Actions - GitHub Docs](https://docs.github.com/ja/actions/using-workflows/workflow-syntax-for-github-actions#on) を参照に実行する条件を制御できます。
また、コメントのフォーマットや、メッセージを解析する際の正規表現などをカスタマイズできます。

```yaml
name: Backlog Notify
on:
  push:
    branches:
      - main
  pull_request:
    types:
      - opened
      - reopened
      - ready_for_review
      - closed
    branches:
      - releases/**
jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Backlog Notify
        uses: bicstone/backlog-notify@v4
        with:
          # 必須設定 (The following are required settings)
          project_key: PROJECT_KEY
          api_host: example.backlog.jp
          api_key: ${{ secrets.BACKLOG_API_KEY }}
          # 任意設定 (The following are optional settings)
          fix_keywords: |-
            #fix
            #fixes
            #fixed
          close_keywords: |-
            #close
            #closes
            #closed
          push_comment_template: |-
            <%= commits[0].author.name %>さんが[<%= ref.name %>](<%= ref.url %>)にプッシュしました
            <% commits.forEach(commit=>{ %>
            + [<%= commit.comment %>](<%= commit.url %>) (<% print(commit.id.slice(0, 7)) %>)<% }); %>
          pr_opened_comment_template: |-
            <%= sender.login %>さんがプルリクエストを作成しました
            + [<%= title %>](<%= pr.html_url %>) (#<%= pr.number %>)
          pr_reopened_comment_template: |-
            <%= sender.login %>さんがプルリクエストを作成しました
            + [<%= title %>](<%= pr.html_url %>) (#<%= pr.number %>)
          pr_ready_for_review_comment_template: |-
            <%= sender.login %>さんがプルリクエストを作成しました
            + [<%= title %>](<%= pr.html_url %>) (#<%= pr.number %>)
          pr_closed_comment_template: |-
            <%= sender.login %>さんがプルリクエストをクローズしました
            + [<%= title %>](<%= pr.html_url %>) (#<%= pr.number %>)
          pr_merged_comment_template: |-
            <%= sender.login %>さんがプルリクエストをマージしました
            + [<%= title %>](<%= pr.html_url %>) (#<%= pr.number %>)
          commit_message_reg_template: "\
            ^\
            (<%= projectKey %>\\-\\d+)\\s?\
            (.*?)?\\s?\
            (<% print(fixKeywords.join('|')) %>|<% print(closeKeywords.join('|')) %>)?\
            $\
            "
          pr_title_reg_template: "\
            ^\
            (<%= projectKey %>\\-\\d+)\\s?\
            (.*?)?\\s?\
            (<% print(fixKeywords.join('|')) %>|<% print(closeKeywords.join('|')) %>)?\
            $\
            "
          fix_status_id: 3
          close_status_id: 4
```

### 設定一覧

| 設定名                                 | 説明                                     |
| -------------------------------------- | ---------------------------------------- |
| `project_key`                          | Backlog プロジェクトキー (必須)          |
| `api_host`                             | Backlog のホスト (必須)                  |
| `api_key`                              | Backlog API キー (必須)                  |
| `fix_keywords`                         | 処理済みにするキーワード                 |
| `close_keywords`                       | 完了にするキーワード                     |
| `push_comment_template`                | プッシュ時のコメント雛形                 |
| `pr_opened_comment_template`           | プルリクエストオープン時のコメント雛形   |
| `pr_reopened_comment_template`         | プルリクエスト再オープン時のコメント雛形 |
| `pr_ready_for_review_comment_template` | プルリクエスト下書き解除時のコメント雛形 |
| `pr_closed_comment_template`           | プルリクエストクローズ時のコメント雛形   |
| `pr_merged_comment_template`           | プルリクエストマージ時のコメント雛形     |
| `commit_message_reg_template`          | コミットメッセージ解析の正規表現雛形     |
| `pr_title_reg_template`                | プルリクエストタイトル解析の正規表現雛形 |
| `fix_status_id`                        | 処理済みの 状態 ID                       |
| `close_status_id`                      | 完了の 状態 ID                           |

#### `push_comment_template`

プッシュ時のコメントの雛形を変更できます。\
構文については [lodash/template](https://lodash.com/docs/4.17.15#template) をご参照ください。

##### 使用可能な変数

| 変数名    | 型             |
| --------- | -------------- |
| `commits` | ParsedCommit[] |
| `ref`     | ParsedRef      |

ParsedCommit

| 変数名      | 型        |
| ----------- | --------- |
| `id`        | string    |
| `tree_id`   | string    |
| `distinct`  | boolean   |
| `message`   | string    |
| `timestamp` | string    |
| `url`       | string    |
| `author`    | Committer |
| `committer` | Committer |
| `added`     | string[]  |
| `modified`  | string[]  |
| `removed`   | string[]  |
| `issueKey`  | string    |
| `comment`   | string    |
| `keywords`  | string    |
| `isFix`     | boolean   |
| `isClose`   | boolean   |

ParsedRef

| 変数名 | 型     |
| ------ | ------ |
| `name` | string |
| `url`  | string |

Committer

| 変数名     | 型                      |
| ---------- | ----------------------- |
| `name`     | string                  |
| `email`    | string &#124; null      |
| `date`     | string &#124; undefined |
| `username` | string &#124; undefined |

#### `pr_*_comment_template`

プルリクエストイベントのコメントの雛形を変更できます。\
構文については [lodash/template](https://lodash.com/docs/4.17.15#template) をご参照ください。

<!-- markdownlint-disable no-duplicate-heading -->

##### 使用可能な変数

| 変数名     | 型                                                                      |
| ---------- | ----------------------------------------------------------------------- |
| `pr`       | PullRequest                                                             |
| `action`   | "opened" &#124; "reopened" &#124; "ready_for_review" &#124; "closed" ※1 |
| `sender`   | User                                                                    |
| `issueKey` | string                                                                  |
| `title`    | string ※2                                                               |
| `keywords` | string                                                                  |
| `isFix`    | boolean                                                                 |
| `isClose`  | boolean                                                                 |

※1: マージとクローズは共に `"closed"` となります。マージであるかを判別したい場合は `pr.merged` を参照ください。
※2: 課題キーとキーワードを除いたタイトルです。加工前のタイトルは `pr.title` を参照ください。

PullRequest

[Get a pull request - GitHub Docs](https://docs.github.com/en/rest/pulls/pulls#get-a-pull-request) のResponse schemaをご参照ください。

User

[Get the authenticated user - GitHub Docs](https://docs.github.com/en/rest/users/users#get-the-authenticated-user) のResponse schemaをご参照ください。

#### `commit_message_reg_template`

コミットメッセージ解析の正規表現雛形を変更できます。\
構文については [lodash/template](https://lodash.com/docs/4.17.15#template) をご参照ください。

{/_lint ignore no-duplicate-headings_/}

##### 使用可能な変数

| 変数名          | 型       |
| --------------- | -------- |
| `projectKey`    | string   |
| `fixKeywords`   | string[] |
| `closeKeywords` | string[] |

#### `pr_title_reg_template`

プルリクエストタイトル解析の正規表現雛形を変更できます。\
構文については [lodash/template](https://lodash.com/docs/4.17.15#template) をご参照ください。

##### 使用可能な変数

| 変数名          | 型       |
| --------------- | -------- |
| `projectKey`    | string   |
| `fixKeywords`   | string[] |
| `closeKeywords` | string[] |

## よくある質問と回答

- 何をプッシュしても実行に失敗し、ログに401エラーとある\
  APIキーが誤っている可能性があります。

- プロジェクトキーと課題キーが正しいのにも関わらず、実行に失敗し、ログに404エラーとある\
  該当APIキーのユーザーがプロジェクトに参加していない可能性があります。

## 料金について

GitHub Actionsは処理時間で課金されるためできるだけ実行時間を減らすよう設計しています。

[ncc](https://github.com/vercel/ncc) を活用し、単一のファイルにコンパイルしたNode.jsのプログラムとなっており、数秒で実行が完了します。2コミットでは3秒でした。とてもエコです。

![GitHub Actions の実行履歴スクリーンショット](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/e6dc1a9f-06e8-66cb-00e8-4d6998fd6b52.png)

## まとめ

もし、便利だと思ったらスター頂ければ幸いです。実はしばらくBacklogを使用しておらず、多くの方に使用していただいているという責任感で維持している状態です。反響があると、維持するモチベーションに繋がります。

よろしくお願いいたします。

https://github.com/marketplace/actions/backlog-notify
