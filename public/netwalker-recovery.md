---
title: "NetWalker（PC-Z1）再インストール用イメージファイル"
tags:
  - NetWalker
  - PC-Z1
  - イメージファイル
  - Ubuntu
  - リカバリー
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

サポート終了によりNetWalker（PC-Z1）のリカバリー用SDカードが作れなくなってしまったので、イメージファイルを公開します。WindowsやMacなどでもリカバリー SDの作成ができます。

## サポート終了

2015年5月31日に、サポート終了に伴いレポジトリ(mit.sharp.co.jp)のアクセスが不可になってしまいました。

2017年3月31日にはリカバリー SDの作成もできなくなりました。

https://jp.sharp/support/mit/info/reinstall.html

リカバリー SDはシャープのレポジトリからしか配布されていないため入手不可能になり、不具合が起きるとリカバリーが一切できない状態になってしまいました。

NetWalkerは初期状態ではまともに使用できないので、OSやソフト入れ替えなどを行って一種の玩具として使っている方が多いのではないでしょうか。リカバリーができないと何かあっても復旧ができません。

## イメージファイルを配布します

自分が以前にバックアップを取っていたためこちらにて再配布します。

ダウンロードはこちらから(1.9GB)。

- [netwalker.img](https://drive.google.com/file/d/1ymZNI6HfCXweupjT4sD4DPFFl6pVXN6i/view?usp=share_link) (Google Drive)

(SHA256: `8ad967c79e5529f9a94ea0bf466442e0ae86e1a0eb2798d8f87b3c5026de92c8`)

7zip圧縮版のダウンロードはこちら(799MB)。

- [netwalker.7z](https://drive.google.com/file/d/1j7qzIHZldb0SWySf9znR-dIQ2UOERIcR/view?usp=sharing) (Google Drive)
- [netwalker.7z](https://1drv.ms/u/s!AoIFJ-kPEMIkjHz7B8bel8iOt3dr?e=Yf8eB5) (OneDrive)

(SHA256: `c9fd6642f77d96b4e6948caf4ac697c442fe4d6697b534bb59b3fb7f18187b3e`)

ソースコード及びライセンスはこちらで公開されています。

https://web.archive.org/web/20230510063702/https://jp.sharp/support/mit/source/download/

https://web.archive.org/web/20230510063936/https://jp.sharp/support/ex-data/ecos_1.000.tar.bz2

https://web.archive.org/web/20230510063938/https://jp.sharp/support/ex-data/linux-2.6.28-271-gec75a15_1.000.tar.bz2

## 使用方法

Google Driveからダウンロードする場合、リンクをクリックした後、「ダウンロード」ボタンをクリックします。

![Google Drive でダウンロードできませんというメッセージが表示されますが、ダウンロードボタンをクリックしてください](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/e714b209-4491-0f9e-a633-b476484862b7.png)

もう一度ダウンロードボタンをクリックします。

![Google ドライブではこのファイルのウイルススキャンを実行できません　と表示されますが、そのままダウンロードをクリックしてください](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/cc69d49e-0348-9090-2a39-acda237a6105.png)

ダウンロード中に、ブータブルメディアを作成できるソフトと2GB以上のmicroSDカードを準備します。配布物はimgファイルそのものです。ブータブルメディアを作成できるソフトを用いてmicroSDカードへの書き込みが必要です。ただし、NetWalkerの仕様上、SDHC(32GB)までが限界です。

Windowsの環境であればRufusをおすすめします(インストール不要で軽量な著名なフリーソフトです)。

https://rufus.ie/ja/

イメージファイルのダウンロードが完了したらRufusにてnetwalker.imgファイルを選択します。7zip版をダウンロードした場合は解凍してから選択してください。

![Rufus の画面、「ディスクまたはISOイメージ」を選択し、ダウンロードしたimgファイルを選択します](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/0424b799-9841-635a-cd71-075b083e4eb7.png)

その後、右のチェックボタンを押し、チェックサムを確認してください。画像の通りの文字列になっていない場合はダウンロードミスなので再度ダウンロードしてください。

![Rufus のハッシュを表示する画面、上記のSHA1と一致していることを確認してください](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/8dc943a7-3b17-f339-555f-0d0b13f6dea5.png)

怪しいSDカードを使用する場合は「不良ブロックを検出」にチェックをいれ、選択した「デバイス」が正しいことを確認し、他の設定は変えずにスタートをクリックしてください。

![Rufus のメイン画面で「スタート」をクリックする](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/519f113d-5574-b657-4982-d82f1f12bfbc.png)

書き込み後に、Windowsのディスクの管理でSDカードの状態を確認するとこのようになっています。Windowsでは認識できないファイルシステムのため、内容を確認できません。

![Windows のディスク管理で1.85GBのプライマリパーティションができます](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/b4dd7d48-46c7-9e35-e4a7-3f0a89b834a7.png)

書き込みが終了したらPCからSDカードを取り出し、電源を切ってから(再起動ではありません)netwalker本体にSDカードを挿入します。

ACアダプターを接続していることを確認し、左上のボタンを2つ同時押しをしながら電源ボタンを押して起動します。

![左上のボタンを2つ同時押しをしながら電源ボタンを押して起動します。](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/5b9362ed-a537-39f1-78d8-5e360bfb0e18.jpeg)

白い画面になったり、突然再起動してもしばらく押し続けます。リカバリーまでとても時間がかかる場合もあります。私の端末は突然再起動し2分程度かかりました。おそらくこの間も根気強くボタンを押し続けないと通常起動してしまいます。

![SHARP しばらくお待ちください の画面が表示されたらOKです](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/33568ef1-5c00-fbe2-7751-c894eba8ab6d.jpeg)

その後、コマンドラインが出たら手を離します。

![アスキーアートで「リカバリーをしますか？」と表示されます](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/6ee2e015-ed64-fa5d-e567-019d01fd75ff.jpeg)

Yを押したらリカバリーが始まります。

![アスキーアートで、「リカバリーをしています　ボタンにふれないでください」が表示されます](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/278a493c-4260-c318-8507-65d77d56dcba.jpeg)

以上でリカバリーが終わりました。

## リカバリーできない場合

下記を確認し、何度か試してみてください。失敗しても根気強く何回か試すと成功します。

- 電源をオフにしてからSDカードを挿入してください。再起動ではブートしません。
- ACアダプターを接続しているか確認してください。
- 起動したらリカバリー画面に入らなくても根気強くボタンを両方ともに押し続けてください。
- ボタンを押し始めるタイミングを変えてみてください。電源ボタンを押す前に押さなければならない個体と、電源ボタンを押した後に押さなければならない個体があるようです。
- SDカード容量を変えてみてください。容量は2GB以上で、できれば容量は小さい方が認識しやすいです。
- SDカードメーカーを変えてみてください。
- Rufusでチェックサムを確認してください。
- Rufusの「不良ブロックの検出」をオンにしてみてください。
