# Game Controllerizer : Developers site

Game Controllerizer はディジタルゲーム拡張のためのミドルウェアです．多様な機器および情報源を既存ゲームへの入力として扱えるようにすることで，新たなエンタテインメントの創出やゲーミフィケーションの構成のための試行錯誤を容易に行うことを可能とし，もって当該分野の研究・教育活動推進に寄与することを目的としています．

ミドルウェアの開発は，[津田塾大学・栗原一貴](http://www.unryu.org/)先生を中心としたチームでおこなっています．詳しくは[論文](http://doi.org/10.20729/00207632)を参照ください

また，初めての方は [Starers site](https://sites.google.com/view/gamecontrollerizer) も併せて参照ください．

## 応用例
[![](https://img.youtube.com/vi/Fn537wPVH6Q/0.jpg)](https://www.youtube.com/watch?v=Fn537wPVH6Q)

※津田塾大学栗原研究室の研究事例紹介

## 構成概要
Game Controllerizer は，様々な既存ディジタルゲームプラットフォームへの入力をソフトェア（S/W）またはハードウェア（H/W）によりエミュレートするゲーム入力エミュレーション部，およびエンドユーザプログラマが多様な機器および情報源と通信しながら最終的にゲーム入力エミュレーション部へと操作入力を送信する手順を記述するビジュアルプログラミング部からなります．  
ビジュアルプログラミング部については
- [Node-RED](https://nodered.org/) : エミュレータを RasPi や PC といった Linux 環境から制御するため
- [Makecode](https://makecode.microbit.org/) : エミュレータを micro:bit から制御する場合

の2環境を設けており，利用目的や得意分野に合わせて選択可能です．

また，ディジタルゲームへの入力信号の時系列情報を一次元の文字列として記述する可読性の高い簡易言語である [DSL4GC](https://github.com/GameControllerizer/DSL4GC)（Domain Specific Language for Game Control）を用意しており，Node-RED環境で利用可能です．

<img src="./images/overview.png" width="800px">

## 各部説明
### [Node-RED](https://github.com/GameControllerizer/node-red-contrib-game_controllerizer)
エミュレータを RasPi や PC といった Linux 環境から制御する場合に利用可能です．ゲーム制御情報を Node-RED のカスタムノード，もしくは DSL4GC で記述・編集することができます．IoT / HTTP / WebSocket といった分野を得意にする方にお勧めします．詳しくは[こちら](https://github.com/GameControllerizer/node-red-contrib-game_controllerizer).

### [Makecode](https://github.com/GameControllerizer/pxt-gamecontrollerizer)
エミュレータを micro:bit から制御する場合に利用可能です．ゲーム制御情報を Scratch スタイルのプログラミングブロック，もしくは Javascript で記述・編集することができます．マイコン / 電子工作といった分野を得意にする方にお勧めします．詳しくは[こちら](https://github.com/GameControllerizer/pxt-gamecontrollerizer).

### S/W emulator（Deprecated）
各ゲームプラットフォームに対する入力を電子的に模擬するS/Wです．詳しくは[こちら](https://github.com/GameControllerizer/GcSwEmulator)．  
現時点での対応プラットフォームは以下です．

- PC(Mouse)
- PC(Keyboard)

### [H/W emulator](https://github.com/GameControllerizer/GcHwEmulator)
各ゲームプラットフォームに対する入力を電子的に模擬するH/Wです．対象プラットフォームに接続すると，USB HID互換ゲームパッド（DirectInput入力方式）として振舞います．詳しくは[こちら](https://github.com/GameControllerizer/GcHwEmulator)．現時点での対応プラットフォームは以下です．

- PC(Gamepad)
- Game console(Gamepad) * _USB互換ゲームパッドを接続可能な機種に限る_
- ~~PC(Mouse) / Smartphone(Mouse) / Game console(Mouse)~~ * _discontinued_
- ~~PC(Keyboard) / Smartphone(Keyboard) / Game console(Keyboard)~~ * _discontinued_

### [DSL4GC](https://github.com/GameControllerizer/DSL4GC)
ディジタルゲームへの入力信号を抽象化し,簡便に表現するための固有言語（Domain Specific Language for Game Control）です． JSON で記述できます．言語仕様については[こちら](https://github.com/GameControllerizer/DSL4GC)．

### [GcScanner](https://github.com/GameControllerizer/GcScannerJs)
一般的な USB Gamepad / Mouse / Keyboard の入力信号を DSL4GC 形式の JSON に変換するプログラムです．HTML+Javascript で書かれておりブラウザ上で動作します．詳しくは[こちら](https://github.com/GameControllerizer/GcScannerJs)．

## プログラミングインタフェース
GUI（e.g. Makecode, Node-RED）に頼らないプログラミングインタフェースを利用したり，
多様な機器・環境と接続する場合にお使いください．
将来的には拡充する予定です（e.g. python Android, iOS）．

- [GcOpsJs](https://github.com/GameControllerizer/GcOpsJs) : node.js むけ
- [GcOpsObniz](https://github.com/GameControllerizer/GcOpsObniz) : obniz むけ
- [GcOpsM5](https://github.com/GameControllerizer/GcOpsM5) : M5 stack むけ
- [GcUnityIF](https://github.com/GameControllerizer/GcUnityIF) : Unity InputSystem むけ

## 関連ソフトウェア（外部リンク）
GameControllerizer を使っての開発を進める際に便利なソフトウェアの紹介です．

- [HTML5 Gamepad Tester](https://greggman.github.io/html5-gamepad-test/)(Win/Mac/Linux) : G.C. H/W gamepad emulator や一般的なゲームパッドの入力を確認するのに適しています．Gamepad API の仕様のため十字キーが認識されなかったり，ブラウザより挙動が異なる場合があります．  
- [Xbox 360 Controller Emulator](https://www.x360ce.com/)(Win) : G.C. H/W gamepad emulator が採用するDirectInput入力方式を，XInput入力方式に変換することができます（XInput入力方式のみをサポートしている市販ゲームなどを操作対象にする場合に適しています）．
- [JoyToKey](https://joytokey.net/ja/)(Win) : G.C. H/W gamepad emulator が採用するDirectInput入力方式を，キーボード入力やマウス入力に変換することができます（キーボードやマウスのみをサポートしている市販ゲームなどを操作対象にする場合に適しています）．
- [Enjoyable](https://yukkurigames.com/enjoyable/)(Mac) : JoyToKeyと同様の働きをするMac用ソフトウェアです．

---

## [GcByProduct](https://github.com/GameControllerizer/GcByProducts)

GameControllerizerを開発・試用する過程で生まれた派生品です．詳しくは[こちら](https://github.com/GameControllerizer/GcByProducts)．
