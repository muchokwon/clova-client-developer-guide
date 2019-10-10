<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

# オーディオ

クライアントデバイスでオーディオコンテンツ、効果音などを出力する際に順守するべき内容を説明します。

* [オーディオ再生の基本ルール](#AudioInterruptionRule)
* [ユーザー発話時のオーディオ再生ルール](#AudioInterruptionRuleForUserUtterance)
* [効果音](#SoundEffect)
* [プラットフォームでサポートされる音声ファイルフォーマット](#SupportedAudioFormat)

## オーディオ再生の基本ルール {#AudioInterruptionRule}

クライアントは、オーディオコンテンツの再生中に、別のオーディオコンテンツの再生を要求される場合があります。そのとき、クライアントは、オーディオ再生ルールに従ってオーディオコンテンツを再生する必要があります。オーディオ再生ルールは、オーディオコンテンツのタイプを基準に作成されています。なので、再生ルールの前に、まずオーディオコンテンツのタイプについて知る必要があります。オーディオコンテンツのタイプには、以下のようなものがあります。クライアントは、オーディオコンテンツのタイプに関連するCIC APIの名前空間に基づいて、Clovaから受信するオーディオコンテンツを区別する必要があります。

| オーディオコンテンツのタイプ | 説明                                                  | 関連するCIC APIの名前空間             |
|---------------|-------------------------------------------------------|----------------------------------|
| Alert         | アラーム音、タイマー音、リマインダー音、リマインダー発話、緊急の警報音などのオーディオコンテンツ             | [`Alerts`](/Develop/References/MessageInterfaces/Alerts.md) |
| Content       | ユーザーのリクエストに対する音楽、動画、ニュース、ポッドキャストなどのオーディオコンテンツ                           | [`AudioPlayer`](/Develop/References/MessageInterfaces/AudioPlayer.md) |
| Dialogue      | ユーザーのリクエストに対するTTSオーディオコンテンツ                                                  | [`SpeechRecognizer`](/Develop/References/MessageInterfaces/SpeechRecognizer.md), [`SpeechSynthesizer`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md) |
| Feedback      | リセット音、着信音（ring tone）、呼び出し音（ringback tone）                              | なし（クライアントで判断する） |
| Notification  | 発信音、システム状態の発話（バッテリー不足の通知、Bluetooth接続解除の通知など）、通知音、通知発話         | [`Notifier`](/Develop/References/MessageInterfaces/Notifier.md) |

<div class="note">
  <p><strong>メモ</strong></p>
  <p>Alertとnotificationタイプは、効果音と発話をまとめて1つのオーディオコンテンツとして扱う必要があります。例えばリマインダーの場合、リマインダー音とリマインダー発話を1つのアラートオーディオコンテンツとして扱う必要があります。同じくバッテリー不足の通知の場合、発信音と「バッテリーが不足しています」のようなシステム状態の発話を、1つのnotificationオーディオコンテンとして扱う必要があります。</p>
</div>

以下のオーディオ再生ルールがあります。

* 物理ボタンの効果音はすぐに再生される必要があります。そのために、ミキシング方式で効果音を出力します。
* オーディオコンテンツは、すぐに再生される必要があります。再生中のオーディオコンテンツがある場合、それをバックグラウンドに処理し、新規のオーディオコンテンツを再生する必要があります。
* ただし、再生中のオーディオコンテンツと新規のオーディオ[コンテンツのタイプ](#AudioInterruptionRule)が同じ場合には、次のように処理します。
  - **Alert、Content、Dialogue、Feedbackタイプ**：再生中のオーディオコンテンツを中断（cancel）し、新規のオーディオコンテンツを再生します。
  - **Notificationタイプ**：現在再生中のオーディオコンテンツを引き続き再生し、新規のオーディオコンテンツを再生キューに追加します。再生中のオーディオコンテンツの再生を完了してから、再生キューに入っている順序通りにオーディオコンテンツを再生します。
* オーディオ再生を停止するときは、現在再生中のオーディオコンテンツから中断する必要があります。

以下は、上記のルールに基づいて、オーディオコンテンツのタイプによって再生中のオーディオコンテンツを処理する方法を示します。

<table style="text-align:center">
  <thead>
    <tr>
      <th rowspan="2">再生中のタイプ</th><th colspan="5">新規に再生するタイプ</th><th rowspan="2">物理ボタンの効果音</th>
    </tr>
    <tr>
      <th>Alert</th><th>Content</th><th>Dialogue</th><th>Feedback</th><th>Notification</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alert</th>
      <td><span class="audioInterruptionRule cancelPlayback">再生中断</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
      <td rowspan="5"><span class="audioInterruptionRule">ミキシング処理</span></td>
    </tr>
    <tr>
      <th>Content</th>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
      <td><span class="audioInterruptionRule cancelPlayback">再生中断</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
    </tr>
    <tr>
      <th>Dialogue</th>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
      <td><span class="audioInterruptionRule cancelPlayback">再生中断</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
    </tr>
    <tr>
      <th>Feedback</th>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
      <td><span class="audioInterruptionRule cancelPlayback">再生中断</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
    </tr>
    <tr>
      <th>Notification</th>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">バックグラウンド処理</span></td>
      <td><span class="audioInterruptionRule continuePlayback">引き続き再生（キューに追加）</span></td>
    </tr>
  </tbody>
</table>

## ユーザー発話時のオーディオ再生ルール {#AudioInterruptionRuleForUserUtterance}

クライアントがオーディオコンテンツを再生している途中に、ユーザーが音声入力をしようとする場合、以下のルールに従う必要があります。

* 再生中のオーディオコンテンツがある場合、attending状態からprocessing & reporting状態まで、そのオーディオをバックグラウンドで処理します。
* 音声入力の待機時間を超過したり、ユーザーのリクエストを正常に処理できなかった場合、バックグラウンド処理したオーディオコンテンツを元通りに再生します。
* リクエストの処理結果によって、別のオーディオコンテンツを再生する必要がある場合、[オーディオ再生の基本ルール](#AudioInterruptionRule)に従ってオーディオコンテンツを再生します。
* マルチターンの対話を行う際、追加的にlistening、processing & reportingとなる状態でも上記のルールに従います。

ユーザーの音声入力を聞き取るattending、listening状態で、新規のオーディオコンテンツ再生のリクエストが入ると、次のように処理します。

* **Alert/dialogue/content**タイプのオーディオコンテンツを再生する場合、ユーザーの音声入力の聞き取りをキャンセルして、そのオーディオコンテンツを再生します。
* **Notification**タイプや**Feedback**タイプのオーディオコンテンツを再生する場合、そのオーディオコンテンツをバックグラウンドで再生します。

## 効果音 {#SoundEffect}

クライアントは、デバイスの状態やユーザーのリクエストに対するフィードバックなどを表現するために、[ランプ](#Light)と一緒に効果音を提供する必要があります。クライアントがユーザーに提供すべき効果音の種類と状況を説明します。

* [効果音の種類](#SoundEffects)
* [効果音のガイドライン](#SoundEffectGuideline)

### 効果音の種類 {#SoundEffects}

クライアントの[状態およびイベント](/Design/Client_State_And_Event.md)を表現するために、以下のような効果音を提供する必要があります。

| 状態またはイベント              | 効果音のサンプル                     | 必須/任意 |
|---------------------------|------------------------------|:---------:|
| Attending状態になる         | <audio title="Attending" controls><source src="./Assets/Sounds/Clova-Client-Soundeffect-Attending.wav" type="audio/wav" /></audio> | 任意     |
| Error状態になる             | <audio title="Error" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Error.wav" type="audio/wav" /></audio>         | 必須     |
| Mute on状態になる           | <audio title="Mute on" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Mute_On.wav" type="audio/wav" /></audio>     | 必須     |
| Mute on状態の解除           | <audio title="Mute off" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Mute_Off.wav" type="audio/wav" /></audio>   | 必須     |
| アラーム（イベントが発生したときに、効果音をリピート再生する） | <audio title="Alarm" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Alarm.wav" type="audio/wav" /></audio>         | 必須     |
| リマインダー（イベントが発生したときに、効果音とリマインダー内容のTTSの順序にリピート再生する） | <audio title="Reminder" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Reminder.wav" type="audio/wav" /></audio>         | 必須     |
| タイマー（イベントが発生したときに、効果音をリピート再生する）      | <audio title="Timer" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Timer.wav" type="audio/wav" /></audio>         | 必須     |

### 効果音のガイドライン {#SoundEffectGuideline}

効果音を提供する際に、以下の内容を順守する必要があります。

* 提供されている効果音をそのまま使用することを推奨します。
* 提供されている効果音以外にも、状況またはメーカーのUXポリシーに合わせて、効果音を追加することができます。
* ユーザーが音だけで状況を認知できるようにする必要があります。
* ランプの動きや画面の状況に合わせて、一貫した効果音を提供する必要があります。
* ボタンのフィードバックに対する効果音を提供する場合、ボタンの物性と感触にマッチする効果音を提供する必要があります。

## プラットフォームでサポートされる音声ファイルフォーマット {#SupportedAudioFormat}

クライアントはClovaから送信されるストリームを再生するので、Clovaでサポートされている音声ファイルフォーマットを再生できる必要があります。

<!-- Start of the shared content: SupportedAudioFormat -->

Clovaでは、以下の音声ファイルフォーマットがサポートされています。

| 音声ファイルフォーマット                     | ライセンス費用 |
|----------------------------------|---------|
| MPEG-1 or MPEG-2 Audio Layer III | 無料       |

Clovaでは、以下のオーディオコンテナフォーマットがサポートされています。

| コンテナフォーマット   | MIMEタイプ                      | 備考                           |
|-------------|-------------------------------|-------------------------------|
| mp3         | audio/mpeg                    | <!-- -->                      |
| m3u8        | application/vnd.apple.mpegurl | HTTP Live Streamingを使用する       |


<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>Clovaでサポートされる音声ファイルフォーマットは、さらに増える可能性があります。</p>
</div>

<!-- End of the shared content -->
