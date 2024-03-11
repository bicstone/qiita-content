---
title: Vagrant(VirtualBox)でディスクアクセスが遅い問題の対処法
tags:
  - PHP
  - Windows
  - プログラミング
  - Vagrant
  - Laravel
private: false
updated_at: "2024-03-07T23:33:05+09:00"
id: 5e07bd7b29c8c95721f4
organization_url_name: null
slide: false
ignorePublish: false
---

Windows + Vagrant + VirtualBoxでLaravelを動かすと、ローディングにかなりの時間がかかって仕事が進まなかったという問題の対処方法を紹介します。NFSを使用する方法です。

:::note warn

※ これは2020-02-01に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## Virtual Box の共有フォルダ機能は遅い

ホスト側とゲスト側でプログラムファイルなどを共有するためにVirtualBoxには共有フォルダ機能が存在しますが、デフォルトの共有方法を用いるとかなり遅くなります。

仕事でLaravelを使ったWebサイトをVirtualBoxで構築していたところ、Windows環境やMac環境でも1ページ読み込むのに5秒ほどかかり、まともに仕事ができない状態になってしまいました。

![Laravelのデバッグツールのスクリーンショット、Bootingに1.15s、Appに1.03sかかっている](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/ac106095-9e40-a975-fd51-e0da5d775443.png)

デフォルトのファイル共有機能は、比較的安定性は高く、多くのOSで使用できるのですが、メモリキャッシュなどの機能を使用していないため、ディスクアクセスは遅い欠点があります。

一方、Laravelは大量の依存ファイルを読み込むのでディスクアクセスが大量に発生します。そのため表示が遅くなります。

## 対処方法

Dockerなどの仮想化技術が使用できるツールに切り替えができるのであれば、切り替えるのをおすすめします(ちなみにDocker ToolBoxはVirtualBoxを使用するので意味ありません...)。それができない場合は、NFSでマウントする機能を使用します。

仕組みは公式ドキュメントをご確認ください。

https://www.vagrantup.com/docs/synced-folders/nfs

## Windows の場合はプラグインが必要

公式ドキュメントには次の記載があり、Windowsホストでは使えないと書いてあります。

> Windows users: NFS folders do not work on Windows hosts. Vagrant will ignore your request for NFS synced folders on Windows.

ところが、なんとWindowsでNFS機能を使用できるプラグイン(`vagrant-winnfsd`)がコミュニティに存在します！素晴らしすぎて感謝してもしきれません。

https://github.com/winnfsd/vagrant-winnfsd

ホスト側がWindowsの場合は下記コマンドでプラグインを先にインストールしておいてください。

```bash
vagrant plugin install vagrant-winnfsd
```

## NFS に変更する方法

NFSに変更するにはVagrantFileの共有ディレクトリの設定にあたる部分を変更する必要があります。

先ほどの公式ドキュメントに記載されていた通り、`type: "nfs"` を追加するだけで使用できます。

例えば次のように設定します。

```rb
config.vm.synced_folder ".", "/var/www/src", type: "nfs"
```

尚、private_networkが必要です。IPは例ですが、静的IPアドレスだとトラブルがすくないです(.1はホスト側で予約されていますので.2以上にする必要があります)。

```rb
config.vm.network :private_network, ip: "192.168.2.2"
```

(とくにMac)ファイアウォールに引っかかりやすいので、うまく動作しない場合はファイアウォールの設定をご確認ください。

## CSS や JavaScript が壊れてしまう場合

環境の組み合わせによっては、sendfileの影響でCSSやJavaScriptが壊れてしまう場合があります。その場合の対処方法はこちらをご確認ください。

https://bicstone.me/enablesendfile-js-broken/

## シンボリックリンクが動作しない場合

Laravelでは、シンボリックリンクを貼らないといけない機能(ファイルストレージ)があります。

https://readouble.com/laravel/6.x/ja/filesystem.html

ホストがWindowsの環境では、なぜかシンボリックリンクが壊れてしまい利用できませんでした。

```bash
$ ls -l storage
lrwxrwxrwx 1 root root 0 Dec 24 02:12 storage -> ../../storage/app/public
```

```bash
$ cd storage
cd: no such file or directory: storage
```

vagrant-winnfsdプラグインの影響なのか、Windowsの影響なのかわかってはいません。

私は、Publicディレクトリのみ、標準の共有ファイルに設定しました。次のようなイメージです。

```rb
config.vm.synced\_folder ".", "/var/www/src", type: "nfs", rsync\_\_exclude: "./public"
config.vm.synced\_folder "./public", "/var/www/src/public"
```

これで、高速化しつつ、ファイルストレージも使用できる環境になりました。それでもLaravelは遅いですけどね…。

![Larabelのデバッグツールスクリーンショット　Bootingが304ms、Appが195ms](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/f8b436be-bb5d-cbce-abf6-d180c29942d0.png)
