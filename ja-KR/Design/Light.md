# ランプ

クライアントデバイスは、[クライアントの状態とイベント](/Design/Client_State_And_Event.md)、ユーザーからのリクエストに対するフィードバックなどを表示するためにランプを提供する必要があります。クライアントがユーザーに提供すべきランプについて説明します。

* [ランプの色](#LightColor)
* [ランプの動き](#LightEffect)
* [ランプのガイドライン](#LightGuideline)

## ランプの色 {#LightColor}

クライアントは、次のようなランプの色を使用する必要があります。

| ランプの色     | RGB値                | 説明                                   | 必須/任意 |
|-------------|----------------------|---------------------------------------|:--------:|
| Green       | <span style="color:#05D686; font-size:150%; vertical-align:middle;">&#9724;</span> 5, 214, 134(#05D686)   | ユーザーの音声入力を聞き取っている                                  | 必須  |
| Yellow Green | <span style="color:#96FF00; font-size:150%; vertical-align:middle;">&#9724;</span> 150, 255, 0(#96FF00)    | Clova通知（Notification）                             | 必須  |
| Red         | <span style="color:#FF0000; font-size:150%; vertical-align:middle;">&#9724;</span> 255, 0, 0(#FF0000)      | マイクのミュート、ネットワーク接続エラー、バッテリー不足などのエラー状況     | 必須  |
| Warm White   | <span style="color:#F0E6E6; font-size:150%; vertical-align:middle;">&#9724;</span> 240, 230, 230(#F0E6E6)  | スピーカーによるClovaの音声出力、アラーム/リマインダー/タイマーイベントの受信                             | 必須  |

以下は、Waveのランプの例です。

<table style="width:600px;">
  <thead>
    <tr>
      <th style="width:150px;">Green</th>
      <th style="width:150px;">Yellow Green</th>
      <th style="width:150px;">Red</th>
      <th style="width:150px;">Warm White</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><img src="Assets/Images/Clova-Client-Light-Wave_Green.png" alt=""></td>
      <td><img src="Assets/Images/Clova-Client-Light-Wave_Yellow_Green.png" alt=""></td>
      <td><img src="Assets/Images/Clova-Client-Light-Wave_Red.png" alt=""></td>
      <td><img src="Assets/Images/Clova-Client-Light-Wave_Warm_White.png" alt=""></td>
    </tr>
  </tbody>
</table>

## ランプの動き {#LightEffect}

ランプの動きは、[ランプの色](#LightColor)が伝える意味に基づいて、もっと詳しい意味や状態を伝えるために使用されます。

以下の表は、クライアントデバイスを実装するときに、ランプが表すべきランプの動きと、それについての説明およびサンプルを提供します。

| ランプの動き                            | 説明                                      | サンプル                                                               |
|------------------------------------|------------------------------------------|-------------------------------------------------------------------|
| 点灯する（Lights up）                     | 特別な効果なしに、ランプを点灯した状態に切り替わります。   | ![](/Design/Assets/Images/Clova-Client-Light-Wave_Sustain.gif)              |
| ゆっくりと点滅を繰り返す（Repeat pulse）         | ランプの照度をゆっくりと上げたり下げたりすることを繰り返します。 | ![](/Design/Assets/Images/Clova-Client-Light-Wave_Repeat_Blink_Slowly.gif)  |
| ゆっくりと消灯する（Fade out）                 | ランプの照度をゆっくりと下げて、最後にランプを消します。 | ![](/Design/Assets/Images/Clova-Client-Light-Wave_Fade_Out.gif)             |
| 波の表現を繰り返す（Repeat Splash）          | ランプが左右に波を打つような効果を繰り返します。 | ![](/Design/Assets/Images/Clova-Client-Light-Wave_Splash.gif)         |

以下の表は、クライアントの[状態とイベント](/Design/Client_State_And_Event.md)をランプで表現する方法を示します。

| 状態またはイベント               | ランプの動き                | 必須/任意 |
|----------------------------|----------------------------|:---------:|
| Attending、listening状態     | Greenのランプを点灯する              | 必須     |
| End状態                    | Warm Whiteのランプをゆっくりと消灯する     | 必須     |
| Error状態                  | Redのランプの点滅をゆっくりと繰り返す       | 必須     |
| Mute on状態                | Redのランプを点灯する                | 必須     |
| Processing & reporting状態 | Warm Whiteのランプの点滅をゆっくりと繰り返す | 必須     |
| Mute on状態の解除            | Redのランプをゆっくりと消灯する           | 必須     |
| 待機時間が超過した直後           | Greenのランプをゆっくりと消灯する<div class="note"><p><strong>メモ</strong></p><p>このイベントに対するランプの動きはもう必須の実装項目ではなく、実装項目から削除される予定です。</p></div>         | 任意     |
| アラーム、リマインダー、タイマーの開始      | Warm Whiteのランプが波の表現を繰り返す  | 任意     |

## ランプについてのガイドライン {#LightGuideline}

ランプを提供する際に、以下の内容を順守する必要があります。
  - 1メートル以内の距離で、0.7の視力を持つ人が[ランプの色](#LightColor)を区別できる必要があります。
  - ランプの色に定義されている意味以外の状態や意味を適用しないことを推奨します。
  - ランプの色は、ユーザーがRGB値と同一の色であると認知できるように適用する必要があります。
  - 必ず表現すべき[ランプの動き](#LightEffect)以外にも、デバイスの起動、スピーカーの音量調節、充電状態、ボタンのフィードバックなど、状況に合わせて、またメーカーのUXポリシーに合わせて、ランプの色と効果を追加することができます。
  - 1つのランプの色や効果で、あまり多くの意味や状態を表現しないことを推奨します。
  - 画面が提供されないデバイスは、ランプの明るさなどでデバイスのスピーカー音量のレベルを表示することを推奨します。
  - 移動できるバッテリー搭載のモデルは、バッテリーの充電状態をランプで確認できるように実装することを推奨します。
