---
title: 【2024年版】Raspberry Pi 最適化設定
tags:
  - RaspberryPi
private: false
updated_at: '2024-11-24T22:41:02+09:00'
id: 51852c78b0c054516d58
organization_url_name: null
slide: false
ignorePublish: false
---
## はじめに
押入れの中に埋もれていたRaspberry Pi 3を、Web技術学習用サーバーとして活用しようとふと思い立ちました。
当記事は、セットアップ時に行った設定内容のメモ書きです。

## 目標
- 自宅で無線WiFi接続でWebサーバーとして使う想定
- 日本語化する
- 安全かつ簡単にSSH接続出来るようにする
- 消費電力を出来るだけ抑える
- microSDの寿命を延ばす設定にする

## 前提条件
- LPIC Level1レベルのLinux知識(vimが使える、基本的なコマンドを知ってる)

## 目次
- [はじめに](#はじめに)
- [目標](#目標)
- [前提条件](#前提条件)
- [目次](#目次)
- [ハードウェア情報](#ハードウェア情報)
- [操作OS](#操作os)
- [手順](#手順)
  - [1. インストール](#1-インストール)
    - [1.1 OS選択](#11-os選択)
    - [1.2 設定を編集する](#12-設定を編集する)
      - [一般設定](#一般設定)
      - [サービス設定](#サービス設定)
    - [1.3 実行](#13-実行)
  - [2. Raspberry Pi Connect 設定](#2-raspberry-pi-connect-設定)
  - [3. 初期設定内容確認](#3-初期設定内容確認)
  - [4. OS最新化](#4-os最新化)
  - [5. 必要なライブラリインストール](#5-必要なライブラリインストール)
  - [6. IPアドレスの固定化](#6-ipアドレスの固定化)
  - [7. SSH接続設定](#7-ssh接続設定)
  - [8. ファイアウォール設定](#8-ファイアウォール設定)
  - [9. 最適化設定](#9-最適化設定)
    - [9.1 不要な機能の無効化](#91-不要な機能の無効化)
    - [9.2 不要なライブラリ削除](#92-不要なライブラリ削除)
  - [10. GUI無効化](#10-gui無効化)
  - [11. SSH接続確認\&設定内容確認](#11-ssh接続確認設定内容確認)
  - [12.消費電力を抑える](#12消費電力を抑える)
  - [Advanced: 監視ツール導入](#advanced-監視ツール導入)
- [参考ページ](#参考ページ)

## ハードウェア情報
- Raspberry Pi 3 Model B Rev 1.2
- SAMSUNG EVO plus 64GB

## 操作OS
Windows 11

## 手順
### 1. インストール
[Raspberry Pi Imager](https://www.raspberrypi.com/software/) でOSを焼きます。

#### 1.1 OS選択
- [ ] Raspberry Piデバイス：RASPBERRY PI 3
- [ ] OS:RASPBERRY PI 0S(64-BIT)
![スクリーンショット 2024-11-24 132059.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2610123/40a3714d-f38b-2741-b73b-fad9027c9cc6.png)

#### 1.2 設定を編集する
##### 一般設定
- [ ] ホスト名を設定
- [ ] ユーザー名とパスワードを設定する
- [ ] Wi-Fiを設定する
  - [ ] Wifiを使う国：JP
- [ ] ロケール設定をする
  - [ ] タイムゾーン：Asia/Tokyo
  - [ ] キーボードレイアウト：jp

![スクリーンショット 2024-11-24 132208.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2610123/51b076b6-6457-b017-110c-1190b3221b7d.png)

##### サービス設定
- [ ] PowerShellで下のコマンドを実行し、SSH鍵作成
  ```bash
  ssh-keygen -t ed25519
  ```
  ```bash
  PS C:> ssh-keygen -t ed25519
  Generating public/private ed25519 key pair.
  Enter file in which to save the key (C:\Users\escap/.ssh/id_ed25519):
  Enter passphrase (empty for no passphrase):
  Enter same passphrase again:
  Your identification has been saved in C:\Users\escap/.ssh/id_ed25519
  Your public key has been saved in C:\Users\escap/.ssh/id_ed25519.pub
  The key fingerprint is:
  SHA256:5qNpj6HLZQs2wOP2ZTLrZy7B3plb2WnOa4a2kk0hUX0 escap@ruggah
  The key's randomart image is:
  +--[ED25519 256]--+
  |       ...       |
  |      .   . E    |
  |       .   .     |
  | .    . .        |
  |  +.   .S.       |
  | . oo  o.o .     |
  | o.*o**=.+      |
  | . +o%XB+=o      |
  |   .BBB=o++.     |
  +----[SHA256]-----+
  ```
- [ ] SSHを有効化する
  - [ ] 公開鍵認証のみを許可する
![スクリーンショット 2024-11-24 133320.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2610123/29f592ea-d312-2a38-4a73-11fe07829861.png)

#### 1.3 実行
- [ ] イメージをmicroSDに焼く
![スクリーンショット 2024-11-24 134020.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2610123/e2c314aa-0957-d206-8108-df2a103a5a8e.png)


### 2. Raspberry Pi Connect 設定
- [ ] 下の記事を参考に接続設定
https://zenn.dev/kmiura55/articles/raspberrypi-connect-fist-touch
![スクリーンショット 2024-11-24 152222.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2610123/86e4d07c-84e8-8384-5981-4f63adf54928.png)

- [ ] 下のサイトからRaspberry Pi を認識していることを確認
https://connect.raspberrypi.com/devices

![スクリーンショット 2024-11-24 152000.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2610123/45d03c34-9545-f512-a217-0e612ce4f5a3.png)

### 3. 初期設定内容確認
- [ ] [Raspberry Pi Connect](https://connect.raspberrypi.com/devices)からRemote Shellを起動して実行
  ```bash
  cat /etc/os-release
  uname -a
  vcgencmd version
  cat /proc/cpuinfo
  lsb_release -a
  getconf LONG_BIT
  ```

### 4. OS最新化
- [ ] apt アップデート
  ```bash
  sudo apt update
  sudo apt -y upgrade
  ```
- [ ] OSディストリビューションアップデート
  ```bash
  sudo apt dist-upgrade -y
  ```
- [ ] ファームウェアアップデート
  ```bash
  sudo rpi-update
  ```
- [ ] 再起動
  ```bash
  sudo apt autoremove -y
  sudo apt autoclean
  sudo reboot
  ```

### 5. 必要なライブラリインストール
- [ ] vimをインストールし、nano削除
  ```bash
  sudo apt install vim-nox
  ```

### 6. IPアドレスの固定化
- [ ] IPアドレスの確認
  ```bash
  hostname -I
  ```
- [ ] デフォルトゲートウェイ確認
  ```bash
  ip route show
  ```
- [ ] 下の記事を参考に、[Raspberry Pi Connect](https://connect.raspberrypi.com/devices)経由でGUIからIPアドレスを固定
https://qiita.com/mochi_2225/items/15c1e7c1c1ae79f97501


### 7. SSH接続設定
- [ ] SSHポート番号変更&rootログイン禁止
  ```bash
  sudo vi /etc/ssh/sshd_config
  ```
  ```conf
  #Port 22
  Port 10022
  #AddressFamily any
  #ListenAddress 0.0.0.0
  #ListenAddress ::
  ```
  ```conf
  # PermitRootLogin prohibit-password
  PermitRootLogin no
  ```
- [ ] sshd再起動
  ```bash
  sudo systemctl restart sshd
  ```
- [ ] PowerShellログイン設定
  - [ ] C:\Users\[ユーザー名]\.ssh　をエクスプローラーで開く
  - [ ] configファイルをテキストエディタで開く(無ければ作成する)
  - [ ] 以下の設定を追加
    ```
    Host raspi
      HostName [ホスト名]
      User [ユーザー名]
      Port 10022
      IdentityFile [id_ed25519の絶対パス]
    ```
  - [ ] PowerShellを開き、SSH接続
    ```sh
    ssh raspi
    ```

### 8. ファイアウォール設定
- [ ] ufwのインストール
  ```bash
  sudo apt install ufw
  ```
- [ ] 接続許可設定
  ```bash
  sudo ufw allow from 192.168.10.0/24 to any port 10022
  sudo ufw allow from 192.168.10.0/24 to any port 80
  sudo ufw allow from 192.168.10.0/24 to any port 8080
  sudo ufw allow from 192.168.10.0/24 to any port 443
  ```
- [ ] ファイアウォールを有効化
  ```bash
  sudo ufw enable
  sudo ufw status
  ```


### 9. 最適化設定
#### 9.1 不要な機能の無効化
- [ ] cups無効化
  ```bash
  sudo systemctl disable cups
  sudo systemctl stop cups
  ```

#### 9.2 不要なライブラリ削除
```bash
sudo apt purge bluez -y
sudo apt purge cups -y
sudo apt purge nano -y
sudo apt autoremove -y
```

### 10. GUI無効化
- [ ] 設定画面を開く
  ```bash
  sudo raspi-config
  ```
- [ ] System Option>Boot / Auto Login
- [ ] 「B2 Console AutoLogin」を選択→「Save」して再起動
  ※外部に公開しないので、B1 Consoleにはしない
![スクリーンショット 2024-11-24 163043.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2610123/fe7f6d54-2060-2c28-0fba-530db1d7f1a1.png)
### 11. SSH接続確認&設定内容確認
- [ ] Raspberry Pi にSSH接続
- [ ] 下のコマンドを実行して設定確認
  ```bash
  raspi-config
  cat /etc/os-release
  uname -a
  vcgencmd version
  cat /proc/cpuinfo
  lsb_release -a
  getconf LONG_BIT
  hostname -I
  ip route show
  ```

### 12.消費電力を抑える
- [ ] swap を無効化
  ```bash
  sudo systemctl disable dphys-swapfile
  ```
- [ ] /tmp をramdisk にする
- [ ] /boot/firmware/config.txt
- [ ] HDMIオフ(※3B+ Bookwormの設定方法)
  ```bash
  sudo apt install ddcutil
  ddcutil setvcp d6 5
  sudo ddcutil capabilities
  ```
- [ ] （Model Bのみ）USBコントローラー（LAN951x/LAN7515）の無効化(※3B+ Bookwormの設定方法)
  ```bash
  echo '1-1' |sudo tee /sys/bus/usb/drivers/usb/unbind
  ```
- [ ] 下のファイルを編集
  ```bash
  sudo vi /boot/firmware/config.txt
  ```
- [ ] 以下に設定を編集し、BluetoothとサウンドカードをOFFにする
  ```
  # Automatically load overlays for detected DSI displays
  display_auto_detect=1
  # Enable audio (loads snd_bcm2835)
  dtparam=audio=off

  （・・省略・・）

  [cm5]
  dtparam=audio=off
  ```

### Advanced: 監視ツール導入
- [ ] monitorixのインストール
  ```bash
  sudo apt install monitorix -y
  ```
- [ ] vcgencmd のインストールパスを確認
  ```bash
  pi@pidevserver1:~ $ which vcgencmd
  /usr/bin/vcgencmd
  pi@pidevserver1:~ $
  ```
- [ ] 設定ファイルを編集
  ```bash
  sudo vi /etc/monitorix/monitorix.conf
  ```
  ```
  raspberrypi = y
  ```
  ```
  <raspberrypi>
          cmd = /usr/bin/vcgencmd # whichコマンドの実行結果
          clocks = arm, core, h264, isp, v3d, uart, emmc, pixel, hdmi
          volts = core, sdram_c, sdram_i, sdram_p
          rigid = 0, 0, 0
          limit = 100, 100, 100
  </raspberrypi>
  ```
- [ ] monitorix を起動
  ```bash
  sudo systemctl enable monitorix
  sudo systemctl restart monitorix
  ```
- [ ] http://[固定IPアドレス]:8080/index.html　にアクセス
![スクリーンショット 2024-11-24 220833.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2610123/bb1f47a0-9a7d-6d9c-3f7f-044c12208591.png)

## 参考ページ
https://qiita.com/HirohitoHigashi/items/7b57781a8a4874012d3d
https://qiita.com/k0kubun/items/7bc5cd060131d9a4d347
https://qiita.com/mashi0727/items/805448ea9c2075c05633
https://x.com/takuya_1st/status/1789481460745068754
https://x.com/jalva_dev/status/1801178878708470039
https://dullwhale-public.hateblo.jp/entry/2023/07/17/013021
https://takuya-1st.hatenablog.jp/entry/2022/02/08/165045
https://gris-et-blanc.net/raspi/885/
https://qiita.com/danieltanaka/items/d137ca530fb3e3a70e2a
https://akkiesoft.hatenablog.jp/entry/20211017/1634405422
https://qiita.com/cafe-capitola/items/91e10926bfbe9cea61e0
https://motomura-kazushi.net/2021/02/14/raspberrypi%E3%81%AE%E3%83%A1%E3%83%A2%E3%83%AA%E3%82%84cpu%E3%82%92%E7%9B%A3%E8%A6%96%E3%81%99%E3%82%8Bmonitorix/
