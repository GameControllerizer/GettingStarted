# GameControllerizer

GameControllerizerはディジタルゲーム拡張のためのミドルウェアです．多様な機器および情報源を既存ゲームへの入力として扱えるようにすることで，新たなエンタテインメントの創出やゲーミフィケーション
の構成のための試行錯誤を容易に行うことを可能にします．

ミドルウェアの開発は，[津田塾大学・栗原先生](http://www.unryu.org/home/gc)を中心におこなっています．

Read this in other languages: [English](./README.en.md), [日本語](./README.md)

# 目次

- [概要](#概要)
- [全体構成](#全体構成)
- [各部説明](#各部説明)
    - [DSL4GC](#dsl4gc)
    - [GcScanner](#gcscanner)
    - [S/W emulator](#sw)
    - [H/W emulator](#hw)
 - [利用方法](#利用方法)

# 概要
[![Game Controllerizer PV](./img/video_link.png)](https://www.youtube.com/watch?v=WvQMhYef6eo)

# 構成図
<img src="./img/block_diagram.png" width="480px">

# 各部説明

GameControllerizerは，様々な既存ディジタルゲームプラットフォームへの入力をソフトェア（S/W）またはハードウェア（H/W）によりエミュレートするゲーム入力エミュレーション部，およびエンドユーザプログラマが多様な機器および情報源と通信しながら最終的にゲーム入力エミュレーショ ン部へと操作入力を送信する手順を記述するビジュアルプログラミング部からなります．後者は視覚的に手軽にプログラミングが行える [Node-RED](https://nodered.org/)を用いて実装されています．ディジタルゲームへの入力信号の時系列情報は，一次元の文字列として記述する可読性の高い簡易言語である DSL4GC（Domain Specific Language for Game Control）で記述されます．

## DSL4GC
ディジタルゲームへの入力信号を抽象化し,簡便に表現するための固有言語（Domain Specific Language for Game Control）です．JSON形式で記述できます．言語仕様については[こちら](./dsl4gc/README.md)．

## GcScanner

一般的な USB Gamepad/Keyboard/Mouse の入力信号を DSL4GC 形式の JSON に変換するプログラムです．HTML+Javascript で書かれており，インストール不要＆ブラウザ上で動作します．詳しくは[こちら](https://github.com/nobu-e753/GcScannerJs)．

<a id = "sw"></a>
## SW emulator

各ゲームプラットフォームに対する入力を電子的に模擬するS/Wです．詳しくは[こちら]()．  
現時点での対応プラットフォームは以下です．

- PC(Mouse)
- PC(Keyboard)

<a id = "hw"></a>
## H/W emulator

各ゲームプラットフォームに対する入力を電子的に模擬するH/Wです．  
対応予定プラットフォームは以下です．

- PC(Gamepad/Mouse/Keyboard)
- Smartphone(Mouse)
- Playstation 1/2/3(Gamepad)
- Xbox series(Gamepad)

配布用のキットを製作中です（2018年秋頃配布開始予定）．

# 利用方法

キットの頒布と合わせて公開予定です．

