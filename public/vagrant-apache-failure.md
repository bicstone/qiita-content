---
title: "Vagrant で Apache が自動起動しない場合の対処法"
tags:
  - "Programming"
  - "Vagrant"
  - "Laravel"
  - "VirtualBox"
  - "Apache"
private: false
updated_at: ""
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

Vagrant + VirtualBoxでApacheを動かすと、 `DocumentRoot is not a directory, or is not readable` で自動起動せずに強制終了してしまい、手動でのサービス再起動が必要になってしまいます。Apacheを強制終了しないようにする方法を紹介します。

※ これは2020-02-11に個人ブログで公開した記事を移植したものです。

## 強制終了してしまう

仕事でLaravelを使ったWebサイトをVirtualBoxで構築(指定されたので...)していたところ、 `vagrant up` してもApacheが自動起動せずに強制終了してしまう現象に遭遇しました。

ログには次のように残っています。

```sh
Main PID: 1021 (code=exited, status=1/FAILURE)
systemd[1]: Starting The Apache HTTP Server...
httpd[1021]: AH00526: Syntax error on line 5 of /etc/httpd/conf/httpd.conf:
httpd[1021]: DocumentRoot '/var/www/public' is not a directory, or is not readable
systemd[1]: httpd.service: main process exited, code=exited, status=1/FAILURE
kill[1455]: kill: cannot find process ""
systemd[1]: httpd.service: control process exited, code=exited status=1
systemd[1]: Failed to start The Apache HTTP Server.
systemd[1]: Unit httpd.service entered failed state.
systemd[1]: httpd.service failed.
```

Apacheから、DocumentRootが読めないというエラーになっています。

`DocumentRoot '/var/www/public' is not a directory, or is not readable`

しかし、この後にサービスの再起動をすると強制終了せずに動作します。

## 原因

原因は、Vagrantによるマウントが完了する前にApacheが起動してしまい、DocumentRootのシンボリックリンクが読めないため発生しています。

## 対処方法

かなり対処療法ですが、VagrantFileによるマウントの記述の後にApacheを再起動することで対処します。

CentOS 7系の場合はマウントの記述の後に下記を追加します。

```rb
config.vm.provision :shell, run: "always", :inline => <<-EOT
  sudo systemctl restart httpd.service
EOT
```

その他は下記を追加します。

```rb
config.vm.provision :shell, run: "always", :inline => <<-EOT
  sudo service httpd restart
EOT
```

以上で、マウント後にApacheが自動再起動されます。

ちなみに、`vagrant up` 時にApacheの再起動をした際のログが次のように残ります。

```sh
==> default: Running provisioner: shell...
    default: Running: inline script
```
