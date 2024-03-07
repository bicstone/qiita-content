---
title: DockerがESETの影響でファイルマウントできない場合の対処法
tags:
  - Windows
  - プログラミング
  - Docker
  - ESET
private: false
updated_at: "2024-03-02T15:27:57+09:00"
id: 10fa085291e5b0d3426f
organization_url_name: null
slide: false
ignorePublish: false
---

DockerがESET Endpoint Protectionのファイアウォールの影響で「Firewall Detected」エラーが発生し、ファイル共有が一切できなくなる問題の対処方法を掲載しています。

※ これは2019-07-15に[個人ブログ](https://bicstone.me)で公開した記事を移植したものです。

## Docker でファイルマウントができない

Dockerでファイルのマウントが一切できない状態になりました。

Dockerを右クリック →「Settings」

![Dockerを右クリック→「Settings」](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/bfdb947a-9097-9f9e-311e-88d20f9bf492.png)

「Shared Drives」→「Shared」にチェック →「Apply」

![「Shared Drives」→「Shared」にチェック→「Apply」](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/cfc45a70-6ba1-9d57-6ba6-4128936a85d3.png)

「Docker needs to access your computer's filesystem」にファイルシステムへアクセスができるユーザー情報を入力。

![「Docker needs to access your computer's filesystem」にファイルシステムへアクセスができるユーザー情報を入力](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/ba76d1f2-1df4-8fe3-3eb6-587eb7a10fc3.png)

「Firewall detected」エラーが発生。

![「Firewall detected」エラーが発生](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/e2fa131d-f1f9-ec63-777c-f1c68b60f968.png)

## ファイアウォールの解除方法

PCに入っていた「ESET Endpoint Protection」のファイアウォール機能が原因でした。

「ESET Endpoint Protection」を起動し、「設定」→「ネットワーク」をクリックします。

![ESET Endpoint Protection のメイン画面](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/ded8bae3-f1e7-41e4-cb3f-e07d148413b1.png)

再度Dockerを操作して「Firewall detected」エラーを発生させた後、「トラブルシューティングウィザード」をクリックします。

![トラブルシューティングウィザード　を選択する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/a4b20cae-e662-cb67-1554-1fdf47115d1a.png)

「ネットワーク保護トラブルシューティング」のIPアドレス「10.0.75.2」を探し出し、「ブロック解除」をクリックします。あまりにも数が多く見つからない場合は「最後の5分」をします。

![ネットワーク保護トラブルシューティング を開き、フィルターを「最後の5分」と選択し、10.0.75.2をブロック解除する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/59ca6aed-b4c4-3a0c-1f05-ecac7c0d6aeb.png)

「以前にブロックされた通信と類似した通信は許可されます」と表示されるので「完了」をクリックします。

![以前にブロックされた通信と類似した通信は許可されます　ダイアログが表示されるので「完了」をクリックする](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/919f5f7f-3c30-79e0-4f0a-360097972e4b.png)

再度「Shared」にチェックして、「Apply」をクリックするとマウントが可能になります。できない場合は「Reset Credentials」をクリックするか、再起動してみてください。

![Docker の Shared Drives でオフ→オンする](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/7b0afa3f-8ca6-59b9-70f4-bf8e362cb337.png)
