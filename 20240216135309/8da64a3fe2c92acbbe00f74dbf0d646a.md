+++
title = "Surface Go 3にGentooインストール"
date = 2024-02-16T13:53:09+09:00
tags = ["gentoo"]
+++

手持のモバイルPCがSurface Go 3だけなのと、
もくもく会でLinux環境で作業をしたかったのでGentooインストールしました。

![](7fc953ee0bba99d8f00ec2665c9b7be0.jpg)

Windows環境は残しておきたいので、デュアルブートにします。

## インストールメディアを起動するまで

Gentooのインストールメディアを起動するまでの手順はこちらを参考にしました。
[https://remoteroom.jp/diary/2023-09-03/](https://remoteroom.jp/diary/2023-09-03/)

### USBブートとUEFI画面

USBブート: 音量Downを長押ししたまま電源ON
UEFI画面: 音量Upを長押ししたまま電源ON

### Windows11のリカバリイメージを作成

[https://support.microsoft.com/ja-jp/surface-recovery-image](https://support.microsoft.com/ja-jp/surface-recovery-image)
ここからリカバリイメージをダウンロードできるのでUSBに書き出しておく。
その際Surface端末のシリアルコードが必要になるので、Surface実機のSurfaceアプリで確認する。

### リカバリイメージからWindows11をクリーンインストールと設定

リカバリイメージに問題ないことも確認したいので、
作成したイメージからWindowsもクリーンインストール。
その後、下記の設定をOFFにしていく。

- 高速スタートアップ
- デバイスの暗号化
- セキュアブート(UEFI画面で設定)

最後にGentoo用のストレージ確保のため、
ディスクマネージャーでWindowsが入っているパーティションを縮小する。

## Gentoo Install Battle

Gentooインストールメディアを起動する準備ができたので、
USBブートかUEFI画面のブートメディア選択からGentooインストールメディアを選択して起動。

### Battle

カーネルインストールまでは通常のGentooインストールと同じでした。
Wifiもキーボードも認識するのでここまで作業に問題なかったです。

### カーネルインストール

[https://www.youtube.com/watch?v=f9OQxdOk-TM](https://www.youtube.com/watch?v=f9OQxdOk-TM)
最初はこちら動画を参考にしたのですが、カーネル起動せず...
動画の方もなぜかブート画面見せてくれないしもしかして起動しない...？

[https://github.com/parinzee/linux-surface-overlay/issues/8](https://github.com/parinzee/linux-surface-overlay/issues/8)
こちらのissueを見ると、要は[linux-surface](https://github.com/linux-surface/linux-surface)のパッチを当ててビルドすれば良いということなので、
Gentoo側のカーネルとパッチのバージョンをできるだけ合わせてパッチを当てつつ、
カーネル設定はReleasesのdebパッケージから取って来た物を使用してビルドしました。

### initramfsの作成

参考にした動画と同じようにgenkernelで作成すると起動せず、
dracutで作成すると起動に成功しました。
