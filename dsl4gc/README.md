# DSL4GC（Domain Specific Language for Game Control）

[GameControllerizer](../README.md) においてディジタルゲームへの入力信号を抽象化し,簡便に表現するための固有言語（Domain Specific Language for Game Control）であり， JSON で記述されます．そのため，可読性にすぐれ，エディットにも特別な環境を必要としません．

これら記述を Node-RED を通してS/WエミュレーターまたはH/Wエミュレータへと送ることで，エミュレータは「あたかも
人間がこの操作をしているGamepad/Keyboard/Mouse」であるかのように振る舞います．

## 例：Street Fighter 2 の波動拳コマンド
一連の制御入力を直感的に記述することができます．
```
[
  {"dpad":[2], "btn":[], "ang":[0,0,0,0], "dur":2}
  {"dpad":[3], "btn":[], "ang":[0,0,0,0], "dur":2}  
  {"dpad":[6], "btn":[], "ang":[0,0,0,0], "dur":2}
  {"dpad":[5], "btn":[0], "ang":[0,0,0,0], "dur":2}
]
```

# 言語仕様

DSL4GC で対象とするデバイスは Gamepad/Mouse/Keyboard とし，この制御情報を次に示すフォーマットで記載します．

```
gc_sentence = Array[gc_word]

gc_word = gc_gamepad_word | gc_mouse_word | gc_keyboard_word

gc_gamepad_word = {“dpad”:Int, “btn”:Array[Int], ”ang“:Array[Int], “dur”:Int}
gc_mouse_word =  {“btn”:Array[Int], “mov”:Array[Int], “dur”:Int}
gc_keyboard_word = {“key”:Array[String], “mod”:Array[Int],  “dur”:Int}
```

## gc_gamepad_word
`{“dpad”:Int, “btn”:Array[Int], ”ang“:Array[Int], “dur”:Int}`

下記要素をもつゲームパッドとして抽象化しています．

- 十字キー：1個
- ボタン：n個
- アナログスティック：2個

### dpad : `Int`

1～9の数値でゲームパッドの方向キー（十字キー）入力を表現します．方向と番号の対応は以下．

<img src="./img/dpad.png" width="160px">

### btn : `Array[Int]`
ボタンの押し込み状態を表現します．押しているボタンのIDが配列に格納されます．

### ang : `Array[Int]`
アナログスティックの倒しこみ状態を表現しています．インデックス順に [Axis0-h, Axis0-v, Axis1-h, Axis1-v]，値域は-128～127としています．

### dur : `Int`
上記制御を適用する期間で，単位はframeとしました（1frame = 16.666msに相当）．値が表現する動作は以下です．

- 0～127 : 指定された制御（dpad, btn, ang）を指定frame間適用した後，default値にもどる
- 0 < : 指定された制御（dpad, btn, ang）を次の入力がある適用し続ける

### 例
0番目のボタンを2frameの間押し，離す（実動作では，スタートボタンや決定ボタンを押すことに相当）
```
{“dpad”:5, “btn”:[0], “ang”:[0,0,0,0], “dur”:2}
```

方向キーの下ボタンを押し続ける．
```
{“dpad”:2, “btn”:[], “ang”:[0,0,0,0], “dur”:-1}
```

### ヒント
なお，制御期間は gc_sentence で指定することもできますが，実時間として扱うこともできます．例えば，ビジュアルプログラミング部に入力/出力される gc_sentence において，以下2つの gc_sentence は等価であり交換可能です．

#### 表現1
```
 [{“dpad”:5, “btn”:[0], “ang”:[0,0,0,0], “dur”:2}]
```

#### 表現2
```
[{“dpad”:5, “btn”:[0], “ang”:[0,0,0,0], “dur”:-1}]
  2frame（=0.033 msec）待ち
[{“dpad”:5, “btn”:[], “ang”:[0,0,0,0], “dur”:-1}]
```
 
## gc_mouse_word

`{“btn”:Array[Int], “mov”:Array[Int], “dur”:Int}`

下記要素をもつマウスとして抽象化しました．
- ボタン：3個
- 移動軸：2軸

### btn : `Array[Int]`
ボタンの押し込み状態を表現します．左ボタン：０，右ボタン：1，中央ボタン：2．

### mov : `Array[Int]`
マウスの相対移動量を指し示します．インデックス順に[dX, dY]．画面に対して左上を原点とします．

### dur : `Int`
Gamepadの場合に同じです．

### Example
左クリック.
```
{“btn”:[0], “mov”:[0, 0], “dur”:2}
```

## gc_keyboard_word
`{“key”:Array[String], “mod”:Array[Int],  “dur”:Int}`

下記要素をもつキーボードとして抽象化しました.
- Alphanumeric keys（”a”～”z”,”0”～”9”）
- Arrow pad keys（”ArrowUp”, “ArrowDown”, “ArrowRight”, “ArrowLeft”）
- Function keys（”F1”～”F12”, “Escape”）
- Functional keys （”Space”, “Tab”, “Enter”, “Backspace”）
- Modifierkey（”Ctrl”, “Shift”, “Alt”）

### key : `Array[String]`
Modifierkeyを除くキーの押し込み状態を文字列配列で表現します．同時に押されているキーがある場合，それぞれのキーに対応する文字列をすべて配列に格納するものとします（順不同．

### mod : `Array[Int]`
Modifierkeyの押し込み状態を表現します．Ctrl：０，Shift：1，Alt：2．

### dur : `Int`
\* Gamepadの場合に同じです．

現実のkeyboardには上記以外にも多数のキーが存在します．しかしこれらを含めて統一的に扱う標準仕様は存在しておらず，OS，ロケーション，機器，入力を受け取るアプリケーション（例：ウェブブラウザ）によりまちまちです．  
そのため，現状の GameControllerizer では抽象化の範囲を一般的にゲームコントロールに使われるであろうキーのみに限定しています．なお，DSL4GC自体は押し込まれたキーを文字列で表現する形式をとっているため，これ以外のキーを判定に加えた場合にも言語仕様そのものを変更する必要はありません．

### 例

”a”をタイプ
```
{“key”:[“a”], “mod”:[], “dur”:2}
```

“A”をタイプ（”Shift”+”a”の組み合わせ）．”mod”:[1] はMofdifierkeyの1番（=”Shift”）を表します．
```
{“key”:[“a”], “mod”:[1], “dur”:2}
```
