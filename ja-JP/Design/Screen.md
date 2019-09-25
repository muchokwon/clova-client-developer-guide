# 画面

クライアントのディスプレイ装置で、画面に以下のUI項目を提供する必要があります。

* [起動画面](#BootingScreen)
* [ロゴの表示](#DisplayingLogo)
* [Green Dot VUI](#GreenDotVUI)

## 起動画面 {#BootingScreen}

デバイスをオンにして、起動が完了するまで表示される画面です。起動画面には主にロゴが表示されます。Clovaのロゴは、他のロゴと組み合わせることなく、単独で画面に表示します。また、ロゴは同じサイズと比率で表示する必要があります。

![](/Design/Assets/Images/Clova-Client-Partner_Logo_on_Loading_Screen.png) ![](/Design/Assets/Images/Clova-Client-Clova_Logo_on_Loading_Screen.png)

## ロゴの表示 {#DisplayingLogo}

画面のレイアウトによってClovaのロゴを配置する方法について説明します。

* [レイアウトA](#LayoutA)
* [レイアウトB](#LayoutB)
* [レイアウトC](#LayoutC)

### レイアウトA {#LayoutA}

画面下の一部や全体を覆うUI画面で、Clovaのロゴが左上に配置されます。

![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_A-Bottom_Overlay.png) ![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_A-Full_Screen_Overlay.png)

* Clovaのロゴは、左上に配置される必要があります。
* Clovaのロゴは、透明にしない必要があります。

### レイアウトB {#LayoutB}

[レイアウトA](#LayoutA)と似たようなレイアウトです。画面下の一部や全体を覆うUI画面で、Clovaのロゴが右上に配置されます。

![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_B-Bottom_Overlay.png) ![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_B-Full_Screen_Overlay.png)

* Clovaのロゴは、右上に配置される必要があります。
* Clovaのロゴは、透明にしない必要があります。

### レイアウトC {#LayoutC}

テキスト形式の結果を表現する画面で、Clovaのロゴが上部に配置されるレイアウトです。

![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_C.png)

* Clovaのロゴは、上部に配置される必要があります。
* コンテンツの提供元は、コンテンツの下に表示される必要があります。


## Green Dot VUI {#GreenDotVUI}

Green Dot VUIはClovaの音声検索ツールで、アイデンティティを表すUIデザインです。Green Dot VUIは、ユーザー音声の聞き取り、Clova音声出力など、Clova音声動作に関連する状態を表します。

画面があるクライアントデバイスは、Green Dot VUIを表現する必要があります。ここでは、Green Dot VUIで使用される色やクライアントの状態による表現方法を説明します。

* [Green Dot VUIの色](#GreenDotVUIColor)
* [Green Dot VUIの表現](#GreenDotVUIMotions)

### Green Dot VUIの色 {#GreenDotVUIColor}

Green Dot VUIは、3つの色のグラデーションをリングの形で表現する必要があります。

![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Color.png)

以下は、上図で使用されている3色の詳細です。

| 色の名称        | RGB値       | CMYK値     | PANTONEカラー   |
|----------------|-------------|-------------|-------------|
| Greendot Green | <span style="color:#03EB64; font-size:150%; vertical-align:middle;">&#9724;</span> 3, 235, 100(#03EB64) | 60, 0, 75, 0   | 2270C |
| Greendot Mint  | <span style="color:#14E6BE; font-size:150%; vertical-align:middle;">&#9724;</span>20, 230, 190(#14E6BE) | 50, 0, 25, 0   | 3255C |
| Greendot Blue  | <span style="color:#1EC8EB; font-size:150%; vertical-align:middle;">&#9724;</span>30, 200, 235(#1EC8EB) | 70, 5, 0, 0   |  298C |


### Green Dot VUIのアクション {#GreenDotVUIMotions}

Green Dot VUIのUI表現はアクションごとに異なります。以下の表は、Green Dot VUIの動作を説明しています。

| アクション名称        | 説明                           | UIモーション（Motion） |
|----------------|-------------------------------|---------------|
| 準備（Intro）          | ユーザーがウェイクワードを言ったり、ボタンを操作したときに表示されるモーション         | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Intro.gif)      |
| 待機（Waiting）        | ユーザーの音声が実際に入力されるまで表示されるモーション         | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Waiting.gif)    |
| 聞き取り（Listening）      | ユーザーの音声が実際に認識され、入力されているときに表示されるモーション | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Listening.gif)  |
| 解析/処理（Processing） | ユーザーのリクエストを解析し、処理しているときに表示されるモーション       | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Processing.gif) |
| 応答（Answering）      | ユーザーのリクエストに対する応答をTTSで出力しているときに表示されるモーションこのモーションには、次の3つの区間があります。<ul><li>エントリ区間：応答（TTS）が開始することを表します（フレーム1~13）。</li><li>リピート区間：TTS出力していることを表します（フレーム14~29）。応答が完了するまで、この区間を繰り返して再生する必要があります。</li><li>終了区間：応答が終了することを表します（フレーム30~41）。</li></ul>    | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Answering.gif)  |
| 完了（Complete）       | ユーザーのリクエストに対する応答を完了するときに表示されるモーション            | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Complete.gif)  |
| エラー（Error）          | エラーが発生している                                       | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Error.gif)     |

<div class="note">
  <p><strong>メモ</strong></p>
  <p>Green Dot VUIモーションのフレームレートは30fpsである必要があります。</p>
</div>

ユーザーの動作や[クライアントの状態](/Design/Client_State_And_Event.md)遷移に応じてGreen Dot VUIの動作を表現する必要があります。以下では、状態ごとにGreen Dot VUIを表現するルールを説明します。

* ユーザーがウェイクワードを言ったり、ボタンを押して**Attending**状態に入ると、**準備**モーションを再生します。
* **準備**モーションを1回再生し、マイクからユーザーの音声入力が実際に開始されるまで**待機**モーションを繰り返して再生します。
* ユーザーの音声入力が開始され、クライアントが**Listening**状態に入ると、ユーザーの音声入力が終了するまで**聞き取り**モーションをループ再生します。
* ユーザーの入力が終了し、クライアントが**Processing & reporting**状態に入ると、応答を出力するか、結果画面を表示するまで**解析/処理**モーションを繰り返して再生します。
* 応答（TTS）を出力するときは、**応答**のエントリ区間を1回再生し、応答が終わるまでリピート区間を繰り返して再生します。応答が終了すると、終了区間を1回再生します。
* 応答を出力する際、**応答**モーションを区分して表示できない場合には、リピート区間のみ繰り返して再生します。
* ユーザーのリクエストに対して必要な作業をすべて処理し、クライアントが**Idle**状態に入ると、**完了**モーションを1回再生します。

以下は、Green Dot VUIモーションのサンプルです。

![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Example.gif)

<div class="note">
  <p><strong>メモ</strong></p>
  <p>Green Dot VUIのモーション画像が必要な場合、Clova事務局までお問い合わせください。</p>
</div>


## Push to talkボタン {#PushToTalkButton}

Push to talkボタンは、[Green Dot VUI](#GreenDotVUI)をアクティブにするために使用するUIデザインです。クライアントデバイスの画面上にタッチ式のUIボタンとして表示し、押したあとにGreen Dot VUIがアクティブになるように実装します。

ここでは、Push to talkボタンで使用される色やクライアントの状態による表現方法を説明します。

* [Push to talkボタンの色](#PushToTalkButtonColor)
* [Push to talkボタンのアクション](#PushToTalkButtonMotions)

### Push to talkボタンの色 {#PushToTalkButtonColor}

デバイスのマイクが有効な場合のPush to talkボタンは、2つの色のグラデーションで表現する必要があります。

![](/Design/Assets/Images/Clova-Client-Push_To_Talk_Button_Active.png)

以下は、上図で使用されている2色の詳細です。

| 色の名称        | RGB値       | CMYK値     | PANTONEカラー   |
|----------------|-------------|-------------|-------------|
| Greendot Green | <span style="color:#03EB64; font-size:150%; vertical-align:middle;">&#9724;</span> 3, 235, 100(#03EB64) | 60, 0, 75, 0   | 2270C |
| Greendot Blue  | <span style="color:#1EC8EB; font-size:150%; vertical-align:middle;">&#9724;</span>30, 200, 235(#1EC8EB) | 70, 5, 0, 0   |  298C |

また、デバイスのマイクが無効またはミュート状態の場合は、ボタンをグレーアウトで表現する必要があります。

![](/Design/Assets/Images/Clova-Client-Push_To_Talk_Button_Mute.png)

以下は、上図で使用されている色の詳細です。

| RGB値       | 不透明度    |
|-------------|-------------|
| <span style="color:#999999; font-size:150%; vertical-align:middle;">&#9724;</span>153, 153, 153(#999999)     | 25%         |


### Push to talkボタンのアクション {#PushToTalkButtonMotions}

ユーザーの動作や[クライアントの状態](/Design/Client_State_And_Event.md)遷移に応じてPush to talkボタンの動作を表現する必要があります。以下では、状態ごとにPush to talkボタンを表現するルールを説明します。

* クライアントデバイスのマイクが有効な場合は、Idle状態のときにPush to talkボタンを表示します。
* ユーザーがPush to talkボタンを押すと、Green Dot VUIがアクティブになります。
* ユーザーの音声入力を処理してGreen Dot VUIのモーションが完了すると、Push to talkボタンの表示に戻ります。

![](/Design/Assets/Images/Clova-Client-Push_To_Talk_Button_Interaction.gif)

* クライアントデバイスのマイクが無効、またはミュート状態の場合は、Idle状態のときにグレーアウトのUIを表示します。

![](/Design/Assets/Images/Clova-Client-Push_To_Talk_Button_Mute.png)

<div class="note">
  <p><strong>メモ</strong></p>
  <p>Push to talkボタンの画像が必要な場合、Clova事務局までお問い合わせください。</p>
</div>
