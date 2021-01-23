# memo

M5StickCを購入後、利用する為にarduino IDEかM5のUIFlowのどちらかを使うことになる。
情報の多そうなarduino IDEを使うことにする。
https://docs.m5stack.com/#/en/core/m5stickc

## arduino IDE

以下の手順に従う。
https://docs.m5stack.com/#/en/arduino/arduino_development

M5StickCはUSBドライバーのインストールをスキップできるとあるので、スキップする。

まずarduinoのサイトからIDEをダウンロードする。
https://www.arduino.cc/

手元のWindows10 Proで使ってみたいので、Windowsの手順書を読む。
https://www.arduino.cc/en/Guide/Windows

Windows10の場合は、
Microsoft storeからインストールさせようとするので、その通りにする。（たまに最新版でなかったりするのだけれども、まずは従う）
起動すると、Java(TM) Platform SE binaryが通信しようとする。Windows Difenderファイヤーウォールの許可するけれども、可能なら何が行われるかの説明は欲しい。
入ったのはarduino IDE 1.8.13。Microsoft storeからインストールしたからか言語は日本語に既に設定されている。
file > preference > setting
ファイル ＞ 環境設定　＞　設定

https://github.com/arduino/Arduino/wiki/Unofficial-list-of-3rd-party-boards-support-urls
このリストの中にある、以下のURLを利用せよとM5StickCのビデオチュートリアルが言うので設定する。
>Espressif ESP32: https://dl.espressif.com/dl/package_esp32_index.json
>Many variants of ESP32 boards

設定をOKで閉じたら
ツール　＞　ボードマネージャー
ESP32を検索して、選び、インストールする。
esp32 by Espressif Systems 1.0.4
何度か失敗するが、それで何とか終わらせる。

ESP32 PicokitがStickCのdevelopment boaardとして使うとビデオチュートリアルでは言っているが、最新のDocsをみると、M5StickCを使えということなので、そうする。

ライブラリーをインストールすると良いとのことなので、入れる。
Sketch -> Include Library -> Manage Libraries...
スケッチ -> ライブラリをインクルード -> ライブラリを管理

M5StickC 0.2.0をインストールする。
これでファイルのスケッチ例、sampleが使えるようになる。

USBで繋ぐと自動的に認識するはずだが、うまくいかないのでデバイスマネージャーで確認する。
ドライバーの更新をすると、USB Serial converterとして認識される。


### backup

最初のM5StickCに入っているイメージをバックアップする。

windows10でarduino IDEをインストールするとesptoolもインストールされる。
C:\Users\xxxxxxx\Documents\ArduinoData\packages\esp32\tools\esptool_py\2.6.1

ここのツールを、以下のコマンドで4MB分のデータを吸い出しておく。
esptool.py -p /dev/tty.usbserial-9552E83892 -b 115200 read_flash 0 0x400000 m5c_default_firmware.bin

esptool.py --chip esp32 --port COM3 --baud 115200 read_flash 0 0x400000 NodeMCU-32S_20200526.bin

esptool.py -p PORT -b 460800 read_flash 0 0x200000 flash_contents.bin

esptool --chip esp32 --port COM3 --baud 115200 read_flash 0 0x400000 m5c_default_firmware.bin
```
esptool.py v2.6
Serial port COM3
Connecting....
Chip is ESP32-PICO-D4 (revision 1)
Features: WiFi, BT, Dual Core, 240MHz, Embedded Flash, VRef calibration in efuse, Coding Scheme None
MAC: 24:a1:60:47:7c:84
Uploading stub...
Running stub...
Stub running...
4194304 (100 %)
4194304 (100 %)
Read 4194304 bytes at 0x0 in 384.5 seconds (87.3 kbit/s)...
Hard resetting via RTS pin...
```
これでバックアップが取れた。（ようにおもうが書き戻していないので不明）

### Factory test
Sampleにある、Factory testを実行してみる。
マイコンボードに書き込むを選択する
```
/*
    note: need add library FastLED from library manage
    Github: https://github.com/FastLED/FastLED

    note: Change Partition Scheme(Default -> NoOTA or MinimalSPIFFS)
*/
```
CodeにFast LEDのLibraryが必要とあるので、インストールする。ライブラリ管理から。
併せてParttition SchemeをDefaultからNoOTAにせよとある。OTA(over the air)は、WiFi経由でコードを書き込めるようにするもの。
領域が足りなくなるので、これを変更する必要がある。
ツール > Partition Schemeを初期値からNoOTAにする。

購入したものに入っていたデフォルトのファームウェアよりも、LEDの点滅が早くなっていたら成功。
これでarduino IDEからソースをコンパイルしてM5StickCボードに書き込むまでができるようになる。




