---
title: CH32V003の使い方 (openocd+vscode)
date: 2025-12-21T13:12:00
tags:
  - 電子工作
draft: true
---
### はじめに

自作基板を作るときのマイコンとしてここのところWCHのCH32V003を第一候補として選択している。（いわゆる秋月の50円マイコンと界隈で呼ばれているあれ）

チップ、デバッガ(1000円弱)共に安いだけでなく、SWIOとGNDの2本だけ配線すればprintfデバッグもステップ実行もどちらもできてしまう。

デメリットとしてはメモリがファミコン並みに小さくてRTOSすら一切載せられなかったりするが、単機能を任せるには最適ということでもある。（複数乗っけても安いし・・・）
新人教育などにはArduinoよりよほど向いているのではないかと感じる。

ただ、毎回環境構築する際に調べなおしているので備忘録として使い方を残しておこうと思う。

### 環境構築

#### 買ってくるもの

### [WCH-LinkE](https://akizukidenshi.com/catalog/g/g118065/)

install

    1  ls

    2  ip addr

    3  exit

    4  ls

    5  curl -fsSL -o get-platformio.py https://raw.githubusercontent.com/platformio/platformio-core-installer/master/get-platformio.py

    6  python3 get-platformio.py

    7  sudo apt install python3.13-venv

    8  python3 get-platformio.py

    9  git clone https://github.com/cnlohr/ch32fun.git

   10  sudo apt-get install build-essential libnewlib-dev gcc-riscv64-unknown-elf libusb-1.0-0-dev libudev-dev gdb-multiarch

   11  ls

   12  cd ch32fun/

   13  ls

   14  cd minichlink/

   15  make

   16  exit

   17  ip addr

   18  ls

   19  sudo apt install openocd

   20  exit

   21  ls

   22  cd ch32fun/minichlink/

   23  sudo minichlink -t

   24  sudo ./minichlink -t

   25  ls \~

   26  history
