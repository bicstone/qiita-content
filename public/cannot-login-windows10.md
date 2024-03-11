---
title: "Microsoft アカウントで Windows にログインできない場合の対処方法"
tags:
  - "Windows"
  - "Windows 10"
  - "Microsoft"
  - powershell
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

一瞬ウインドウが出ても消えてしまい、MicrosoftアカウントでWindows 10にログインできない場合の対処方法を紹介します。

:::note warn

※ これは2017-05-28に[個人ブログ](https://bicstone.me)で公開した記事を移植し、[CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.ja)で提供しています。情報は古い可能性があります。

:::

## 現象

MicrosoftアカウントでWindows 10の設定アプリにログインしたいと思い、「Microsoftアカウントでのサインインに切り替える」をクリックしましたが、一瞬ウインドウが出ても消えてしまい、ログインが出来ません。

「Microsoftアカウントでのサインインに切り替える」をクリックしても..。

![Windowsの設定から「Microsoft アカウントでのサインインに切り替える」をクリック](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/d6d42d74-bf74-9a3d-19b8-add49ce5c901.png)

一瞬ウインドウが現れ..。

![何も表示されないウインドウが表示される](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/75fc7017-1949-24c9-169d-a2f34ddb5031.png)

消えてしまいました。

![Windows の設定画面に戻る](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/7b4e4515-3e4e-6009-1026-e0d4a603e3fd.png)

## 原因

設定アプリが破損してしまっていることが原因です。

## 対処

Windowsキー + Xまたは、スタートメニューボタンを右クリックし、「コマンドプロンプト(管理者)」を開きます。

![Windows + X キーでメニューを開く](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/a83ad5fa-166e-d2a7-b7eb-baea260b4926.png)

`powershell` と入力してEnterを押します。

![コマンドプロンプトで、 `poweshell` を入力する](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/23493c7c-9d73-a9ba-3b4a-f5605b498ba2.png)

まずは、すべてのアプリを終了させたあと、次のコマンドを入力してEnterを押します。このコマンドによってMicrosoft製のアプリをすべて再インストールします。尚、副作用として今までに削除したMicrosoft製のアプリはすべて復活します。

```powershell
Get-AppXPackage -AllUsers | Foreach {Add-AppxPackage -DisableDevelopmentMode -Register "$($_.InstallLocation)\AppXManifest.xml" -Verbose}
```

![PowerShell のスクリーンショット](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/3395226d-c12d-7a73-bdf7-8ab61476dbb2.png)

再起動後、正常にログインできるか確認してください。これでも出来ない場合はさらに次のコマンドを実行します。このコマンドによってWindowsの根幹ファイルの探査・修復します。

```powershell
DISM.exe /Online /Cleanup-image /Restorehealth
```

```powershell
sfc /scannow
```

すこし時間がかかります。終了後、再起動して正常にログインできるか確認してください。これでも出来ない場合、新しくアカウントを作ってその際にMicrosoftアカウントでログインできないか試してみてください。

## 結果

無事にログインできるようになりました。

![Windows のログイン画面が表示される](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/31a08b84-b918-12a1-22c6-6dc91208aa62.png)
