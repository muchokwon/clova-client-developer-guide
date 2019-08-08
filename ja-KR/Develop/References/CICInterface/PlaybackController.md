<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

# PlaybackController

<!-- Start of the shared content: CICAPIforAudioPlayback -->
PlaybackControllerインターフェースは、クライアントのオーディオ再生およびスピーカー出力をコントロールする際に使用する名前空間です。PlaybackControllerでは、次のイベントとディレクティブを提供しています。

| メッセージ         | タイプ  | 説明                                   |
|------------------|-----------|---------------------------------------------|
| [`CustomCommandIssued`](#CustomCommandIssued)  | イベント     | ユーザーがクライアントデバイスのショートカットボタンのうち1つを押した場合、クライアントはこのイベントをCICに送信する必要があります。  |
| [`ExpectNextCommand`](#ExpectNextCommand)      | ディレクティブ | ユーザーがクライアント上で「次」ボタン（Next）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.NextCommandIssued`](#NextCommandIssued)イベントをCICに送信するように指示します。  |
| [`ExpectPauseCommand`](#ExpectPauseCommand)    | ディレクティブ | ユーザーがクライアント上で「一時停止」ボタン（Pause）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)イベントをCICに送信するように指示します。  |
| [`ExpectPlayCommand`](#ExpectPlayCommand)      | ディレクティブ | ユーザーがクライアント上で「再生」ボタン（Play）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)イベントをCICに送信するように指示します。  |
| [`ExpectPreviousCommand`](#ExpectPreviousCommand)  | ディレクティブ | ユーザーがクライアント上で「前」ボタン（Previous）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)イベントをCICに送信するように指示します。  |
| [`ExpectResumeCommand`](#ExpectResumeCommand)  | ディレクティブ | ユーザーがクライアント上で「再開」ボタン（Resume）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)イベントをCICに送信するように指示します。  |
| [`ExpectStopCommand`](#ExpectStopCommand)      | ディレクティブ | ユーザーがクライアント上で「停止」ボタン（Stop）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.StopCommandIssued`](#StopCommandIssued)イベントをCICに送信するように指示します。  |
| [`Mute`](#Mute)                                | ディレクティブ | クライアントに、オーディオプレーヤーをミュートにするように指示します。            |
| [`Next`](#Next)                                | ディレクティブ | クライアントに、再生キューに入っている次のオーディオストリームを再生するように指示します。   |
| [`NextCommandIssued`](#NextCommandIssued)      | イベント     | ユーザーがクライアントデバイスで「次」ボタン（Next）を押したり、CICから[`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。 |
| [`Pause`](#Pause)                              | ディレクティブ | クライアントに、再生中のオーディオストリームを一時停止するように指示します。        |
| [`PauseCommandIssued`](#PauseCommandIssued)    | イベント     | ユーザーがクライアントデバイスで「一時停止」ボタン（Pause）を押したり、CICから[`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。  |
| [`PlayCommandIssued`](#PlayCommandIssued)      | イベント     | ユーザーがクライアントデバイスで「再生」ボタン（Play）を押したり、CICから[`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。  |
| [`Previous`](#Previous)                        | ディレクティブ | クライアントに、再生キューにある前のオーディオストリームを再生するように指示します。 |
| [`PreviousCommandIssued`](#PreviousCommandIssued) | イベント | ユーザーがクライアントデバイスで「前へ」ボタン（Previous）を押したり、CICから[`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。 |
| [`Replay`](#Replay)                            | ディレクティブ | クライアントに、オーディオストリームを最初から再度再生するように指示します。         |
| [`Resume`](#Resume)                            | ディレクティブ | クライアントに、オーディオストリームの再生を再開するように指示します。                |
| [`ResumeCommandIssued`](#ResumeCommandIssued)  | ディレクティブ | ユーザーがクライアントデバイスで「再開」ボタン（Resume）を押したり、CICから[`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。  |
| [`SetRepeatMode`](#SetRepeatMode)              | ディレクティブ | クライアントに、指定されたリピート再生モードに再生状態を変更するように指示します。  |
| [`SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued) | イベント | ユーザーがクライアントデバイスで「リピート再生」ボタン（Repeat）を押した場合、クライアントはこのイベントをCICに送信する必要があります。  |
| [`Stop`](#Stop)                                | ディレクティブ | クライアントに、オーディオストリームの再生を停止するように指示します。                |
| [`StopCommandIssued`](#StopCommandIssued)      | イベント     | ユーザーがクライアントデバイスで「再開」ボタン（Resume）を押したり、CICから[`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。  |
| [`TurnOffRepeatMode`](#TurnOffRepeatMode)      | ディレクティブ | **（Deprecated）** クライアントに、1曲リピート再生モードを無効にするように指示します。                  |
| [`TurnOnRepeatMode`](#TurnOnRepeatMode)        | ディレクティブ | **（Deprecated）** クライアントに、1曲リピート再生モードを有効にするように指示します。                  |
| [`Unmute`](#Unmute)                            | ディレクティブ | クライアントに、オーディオプレーヤーのミュートを解除するように指示します。              |
| [`VolumeDown`](#VolumeDown)                    | ディレクティブ | **（Deprecated）** クライアントに、オーディオプレーヤーの音量を下げるように指示します。                      |
| [`VolumeUp`](#VolumeUp)                        | ディレクティブ | **（Deprecated）** クライアントに、オーディオプレーヤーの音量を上げるように指示します。                      |

<!-- End of the shared content -->

## CustomCommandIssuedイベント {#CustomCommandIssued}
ユーザーがクライアントデバイスのショートカットボタンのうち1つを押した場合、クライアントはこのイベントをCICに送信する必要があります。CICはこのイベントを受信すると、適切なディレクティブをクライアントに送信します。

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields
| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `button`      | string  | クライアントデバイスのショートカットボタンを区別するための値です。（例：<code>"CUSTOM_BUTTON_2"</code>） |  |

### 備考
* クライアントデバイスのボタンは、ハードウェア方式の物理ボタンの場合もあり、オーディオプレーヤーウィジェットのボタンのようなソフトウェア方式のボタンの場合もあります。

### Message example

```json
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Speaker.VolumeState}},
    {{SpeechSynthesizer.SpeechState}}
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "CustomCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "button": "CUSTOM_BUTTON_2"
    }
  }
}
```

### 次の項目も参照してください。
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

## ExpectNextCommandディレクティブ {#ExpectNextCommand}
ユーザーがクライアント上で「次」ボタン（Next）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.NextCommandIssued`](#NextCommandIssued)イベントをCICに送信するように指示します。クライアントはこのディレクティブを受信すると、関連する動作を処理して、[`PlaybackController.NextCommandIssued`](#NextCommandIssued)イベントをCICに送信する必要があります。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 制御対象の情報を持つオブジェクト。<div class="note"><p><strong>メモ</strong></p><p>このフィールドがある場合、クライアントは、<a href="#NextCommandIssued"><code>PlaybackController.NextCommandIssued</code></a>イベントを送信するとき、<code>source</code>フィールドを含める必要があります。</p></div> | 条件付き  |
| target.namespace  | string  | CIC APIの名前空間。制御対象を確認するための情報です。[`PlaybackController.NextCommandIssued`](#NextCommandIssued)イベントを送信するとき、このフィールドに`source.namespace`の値を入力する必要があります。以下の値を持ちます。<ul><li><code>"AudioPlayer"</code>：AudioPlayer</li><li><code>"MediaPlayer"</code>：MediaPlayer</li></ul>  |   |

### Message example

```json
サンプル1：対象が指定されていないサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectNextCommand",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}

サンプル2：対象が「AudioPlayer」に指定されたサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectNextCommand",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### 次の項目も参照してください。
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)

## ExpectPauseCommandディレクティブ {#ExpectPauseCommand}
ユーザーがクライアント上で「一時停止」ボタン（Pause）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)イベントをCICに送信するように指示します。クライアントはこのディレクティブを受信すると、[`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)イベントをCICに送信する必要があります。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 制御対象の情報を持つオブジェクト。<div class="note"><p><strong>メモ</strong></p><p>このフィールドがある場合、クライアントは、<a href="#PauseCommandIssued"><code>PlaybackController.PauseCommandIssued</code></a>イベントを送信するとき、<code>source</code>フィールドを含める必要があります。</p></div> | 条件付き  |
| target.namespace  | string  | CIC APIの名前空間。制御対象を確認するための情報です。[`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)イベントを送信するとき、このフィールドに`source.namespace`の値を入力する必要があります。以下の値を持ちます。<ul><li><code>"AudioPlayer"</code>：AudioPlayer</li><li><code>"MediaPlayer"</code>：MediaPlayer</li></ul>  |   |

### Message example

```json
サンプル1：対象が指定されていないサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectPauseCommand",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}

サンプル2：対象が「AudioPlayer」に指定されたサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectPauseCommand",
      "dialogRequestId": "7182403e-b5eb-4b71-b2af-179a7515edc4",
      "messageId": "bbb3a40d-ad7f-4609-9021-97f3f0aa2a9e"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### 次の項目も参照してください。
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [オーディオ再生をコントロールする](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

## ExpectPlayCommandディレクティブ {#ExpectPlayCommand}
ユーザーがクライアント上で「再生」ボタン（Play）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)イベントをCICに送信するように指示します。このディレクティブは、現在再生しているオーディオストリームを別のデバイスで再生しようとするときにも受信することがあります。クライアントはこのディレクティブを受信すると、関連する動作を処理して、[`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)イベントをCICに送信する必要があります。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `handover`            | object  | リモートでオーディオの再生を引き継ぐときに必要な情報を持つオブジェクト。オーディオの再生を引き継ぐ場合、`handover`オブジェクトがメッセージに含まれます。`handover`オブジェクトが含まれている場合、クライアントは[`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)イベントの`payload`内の`handover`オブジェクトを、このオブジェクトの内容で設定する必要があります。     | 条件付き |
| `handover.customData` | string  | オーディオの再生に必要な情報。               |  |
| `handover.deviceId`   | string  | オーディオの再生を引き渡すクライアントデバイスのID  |  |
| `token`               | string  | 再生するオーディオストリームのトークン。オーディオの再生を引き継ぐ場合、`token`フィールドがメッセージに含まれます。`token`フィールドが含まれている場合、クライアントは[`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)イベントの`token`に、このフィールドの値を入力する必要があります。  | 条件付き  |

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectPlayCommand",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### 次の項目も参照してください。
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)

## ExpectPreviousCommandディレクティブ {#ExpectPreviousCommand}
ユーザーがクライアント上で「前」ボタン（Previous）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)イベントをCICに送信するように指示します。クライアントはこのディレクティブを受信すると、関連する動作を処理して、[`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)イベントをCICに送信する必要があります。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 制御対象の情報を持つオブジェクト。<div class="note"><p><strong>メモ</strong></p><p>このフィールドがある場合、クライアントは、<a href="#PreviousCommandIssued"><code>PlaybackController.PreviousCommandIssued</code></a>イベントを送信するとき、<code>source</code>フィールドを含める必要があります。</p></div> | 条件付き  |
| target.namespace  | string  | CIC APIの名前空間。制御対象を確認するための情報です。[`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)イベントを送信するとき、このフィールドに`source.namespace`の値を入力する必要があります。以下の値を持ちます。<ul><li><code>"AudioPlayer"</code>：AudioPlayer</li><li><code>"MediaPlayer"</code>：MediaPlayer</li></ul>  |   |

### Message example

```json
サンプル1：対象が指定されていないサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectPreviousCommand",
      "dialogRequestId": "036fbb1e-4739-4453-9439-ccff68eccc63",
      "messageId": "e32f6330-e696-4c21-9e52-529e9b4cbc14"
    },
    "payload": {}
  }
}

サンプル2：対象が「AudioPlayer」に指定されたサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectPreviousCommand",
      "dialogRequestId": "036fbb1e-4739-4453-9439-ccff68eccc63",
      "messageId": "e32f6330-e696-4c21-9e52-529e9b4cbc14"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### 次の項目も参照してください。
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)

## ExpectResumeCommandディレクティブ {#ExpectResumeCommand}
ユーザーがクライアント上で「再開」ボタン（Resume）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)イベントをCICに送信するように指示します。クライアントはこのディレクティブを受信すると、[`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)イベントをCICに送信する必要があります。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 制御対象の情報を持つオブジェクト。<div class="note"><p><strong>メモ</strong></p><p>このフィールドがある場合、クライアントは、<a href="#ResumeCommandIssued"><code>PlaybackController.ResumeCommandIssued</code></a>イベントを送信するとき、<code>source</code>フィールドを含める必要があります。</p></div> | 条件付き  |
| target.namespace  | string  | CIC APIの名前空間。制御対象を確認するための情報です。[`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)イベントを送信するとき、このフィールドに`source.namespace`の値を入力する必要があります。以下の値を持ちます。<ul><li><code>"AudioPlayer"</code>：AudioPlayer</li><li><code>"MediaPlayer"</code>：MediaPlayer</li></ul>  |   |

### Message example

```json
サンプル1：対象が指定されていないサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectResumeCommand",
      "dialogRequestId": "70d75f9a-2202-4a4e-a0cb-9f804d235890",
      "messageId": "3865c844-27dd-4fda-a7d2-03ef96a4cc83"
    },
    "payload": {}
  }
}

サンプル2：対象が「AudioPlayer」に指定されたサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectResumeCommand",
      "dialogRequestId": "d7b5f918-2bb5-4b94-91c5-e1ddec0aa8c4",
      "messageId": "c75a01b4-8402-444a-9386-aa1819d12d29"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### 次の項目も参照してください。
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [オーディオ再生をコントロールする](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

## ExpectStopCommandディレクティブ {#ExpectStopCommand}
ユーザーがクライアント上で「停止」ボタン（Stop）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.StopCommandIssued`](#StopCommandIssued)イベントをCICに送信するように指示します。クライアントはこのディレクティブを受信すると、[`PlaybackController.StopCommandIssued`](#StopCommandIssued)イベントをCICに送信する必要があります。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 制御対象の情報を持つオブジェクト。<div class="note"><p><strong>メモ</strong></p><p>このフィールドがある場合、クライアントは、<a href="#StopCommandIssued"><code>PlaybackController.StopCommandIssued</code></a>イベントを送信するとき、<code>source</code>フィールドを含める必要があります。</p></div> | 条件付き  |
| target.namespace  | string  | CIC APIの名前空間。制御対象を確認するための情報です。[`PlaybackController.StopCommandIssued`](#StopCommandIssued)イベントを送信するとき、このフィールドに`source.namespace`の値を入力する必要があります。以下の値を持ちます。<ul><li><code>"AudioPlayer"</code>：AudioPlayer</li><li><code>"MediaPlayer"</code>：MediaPlayer</li></ul>  |   |

### Message example

```json
サンプル1：対象が指定されていないサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectStopCommand",
      "dialogRequestId": "e3295587-23a5-49d7-bc72-05adf02e4a08",
      "messageId": "8b15ad68-ee0c-44de-959a-0f0c3526b361"
    },
    "payload": {}
  }
}

サンプル2：対象が「AudioPlayer」に指定されたサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectStopCommand",
      "dialogRequestId": "bbfdad3a-333c-4a97-beb0-33ac9d46ac9d",
      "messageId": "1a565441-e2bc-40a5-bb3e-7846767f37be"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### 次の項目も参照してください。
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)
* [オーディオ再生をコントロールする](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

## Muteディレクティブ {#Mute}
クライアントに、オーディオプレーヤーをミュートにするように指示します。クライアントはこのディレクティブを受信すると、オーディオストリームの再生に関連するスピーカーをミュートにする必要があります。

### Payload fields
なし

### 備考

Clovaは、スピーカーの出力に関連するコントロールの場合、通知のためのオーディオファイルを[`SpeechSynthesizer.Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak)ディレクティブに添付しません。それは、ユーザーの音楽鑑賞などのUXを向上させるためで、その場合には音声案内の代わりに、クライアントデバイスのライトや、短い効果音などで音量が調節されたことをユーザーに通知する必要があります。

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Mute",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### 次の項目も参照してください。
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## Nextディレクティブ {#Next}
クライアントに、再生キューに入っている次のオーディオストリームを再生するように指示します。クライアントは、このディレクティブを受信すると、次のオーディオストリームを再生する必要があります。

### Payload fields
なし

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Next",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### 次の項目も参照してください。
* [`PlaybackController.Previous`](#Previous)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## NextCommandIssuedイベント {#NextCommandIssued}
ユーザーがクライアントデバイスで「次」ボタン（Next）を押したり、CICから[`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。CICはこのイベントを受信すると、適切なディレクティブをクライアントに送信します。

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | クライアントデバイスのID。オーディオの再生をリモートでコントロールする状況でない場合、このフィールドは省略します。 | 任意 |
| `source`      | object  | イベントのソースを持つオブジェクト。このイベントを送信した状況やコンテキストに関する情報が含まれます。<div class="tip"><p><strong>ヒント</strong></p><p>クライアントがユーザーに音声出力（TTS）とオーディオコンテンツの再生を同時に提供している場合、通常、制御対象となるのは音声出力のほうです。このフィールドを使用すると、このイベントが制御する対象を指定できます。Clovaが制御対象を指定するように委任するには、このフィールドを省略します。</p></div>  | 任意  |
| `source.namespace` | string | CIC APIの名前空間。このイベントが送信された状況やコンテキストを確認できるように、関連するCIC APIの名前空間を入力します。<ul><li><code>"AudioPlayer"</code>：AudioPlayer</li><li><code>"MediaPlayer"</code>：MediaPlayer</li></ul>  |   |

### 備考
* クライアントデバイスのボタンは、ハードウェア方式の物理ボタンの場合もあり、オーディオプレーヤーウィジェットのボタンのようなソフトウェア方式のボタンの場合もあります。

### Message example

```json
サンプル1：ソースが指定されていないサンプル
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Speaker.VolumeState}},
    {{SpeechSynthesizer.SpeechState}}
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "NextCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}

サンプル2：ソースが「AudioPlayer」に指定されたサンプル
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Speaker.VolumeState}},
    {{SpeechSynthesizer.SpeechState}}
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "NextCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "source": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### 次の項目も参照してください。
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

<!-- End of the shared content -->

<!-- Start of the shared content: PlaybackController.Pause -->

## Pauseディレクティブ {#Pause}
クライアントに、再生中のオーディオストリームを一時停止するように指示します。クライアントは、このディレクティブを受信すると、オーディオストリームの再生を一時停止する必要があります。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 制御対象の情報を持つオブジェクト。このディレクティブから制御対象を確認できます。 | 条件付き  |
| target.namespace  | string  | CIC APIの名前空間。制御対象を確認するための情報です。以下の値を持ちます。<ul><li><code>"AudioPlayer"</code>：AudioPlayer</li><li><code>"MediaPlayer"</code>：MediaPlayer</li></ul>  |   |

### Message example

```json
サンプル1：対象が指定されていないサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Pause",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}

サンプル2：対象が「AudioPlayer」に指定されたサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Pause",
      "dialogRequestId": "a85e78b4-44f7-4bea-8a40-66c181b9720f",
      "messageId": "a76c8dfd-c1b6-44f6-a58d-fc8f33c242c1"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### 次の項目も参照してください。
* [`AudioPlayer.PlayPaused`](/Develop/References/CICInterface/AudioPlayer.md#PlayPaused)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
* [オーディオ再生をコントロールする](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

## PauseCommandIssuedイベント {#PauseCommandIssued}
ユーザーがクライアントデバイスで「一時停止」ボタン（Pause）を押したり、CICから[`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。CICはこのイベントを受信すると、適切なディレクティブをクライアントに送信します。

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | クライアントデバイスのID。オーディオの再生をリモートでコントロールする状況でない場合、このフィールドは省略します。 | 任意 |
| `source`      | object  | イベントのソースを持つオブジェクト。このイベントを送信した状況やコンテキストに関する情報が含まれます。<div class="tip"><p><strong>ヒント</strong></p><p>クライアントがユーザーに音声出力（TTS）とオーディオコンテンツの再生を同時に提供している場合、通常、制御対象となるのは音声出力のほうです。このフィールドを使用すると、このイベントが制御する対象を指定できます。Clovaが制御対象を指定するように委任するには、このフィールドを省略します。</p></div>  | 任意  |
| `source.namespace` | string | CIC APIの名前空間。このイベントが送信された状況やコンテキストを確認できるように、関連するCIC APIの名前空間を入力します。現在、`"AudioPlayer"`のみ入力できます。  |   |

### 備考
* クライアントデバイスのボタンは、ハードウェア方式の物理ボタンの場合もあり、オーディオプレーヤーウィジェットのボタンのようなソフトウェア方式のボタンの場合もあります。

### Message example

```json
//サンプル1：ソースが指定されていないサンプル
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Speaker.VolumeState}},
    {{SpeechSynthesizer.SpeechState}}
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "PauseCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}

//サンプル2：ソースが「AudioPlayer」に指定されたサンプル
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Speaker.VolumeState}},
    {{SpeechSynthesizer.SpeechState}}
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "PauseCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "source": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### 次の項目も参照してください。
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectPauseCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)
* [オーディオ再生をコントロールする](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

## PlayCommandIssuedイベント {#PlayCommandIssued}
ユーザーがクライアントデバイスで特定の楽曲を再生するようにUIを操作したり、CICから[`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。CICはこのイベントを受信すると、適切なディレクティブをクライアントに送信します。CICから送信された[`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)ディレクティブの`payload`に`handover`フィールドが含まれている場合、そのまま使用してオーディオ再生の引き継ぐ必要があります。

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`            | string  | クライアントデバイスのID。リモートでオーディオの再生を引き渡す状況でない場合、`deviceId`フィールドは省略します。 | 任意 |
| `handover`            | object  | リモートでオーディオの再生を引き継ぐときに必要な情報を持つオブジェクト。オーディオ再生を引き継ぐ必要がある場合、`handover`オブジェクトの内容を、[`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)ディレクティブの`payload`の`handover`オブジェクトで設定する必要があります。     | 任意 |
| `handover.customData` | string  | オーディオの再生に必要な情報。               |  |
| `handover.deviceId`   | string  | オーディオの再生を引き渡すクライアントデバイスのID  |  |
| `token`               | string  | 再生するオーディオストリームのトークン。ユーザーがリストからトラックを選んで再生ボタンを押した場合、[`TemplateRuntime.RenderPlayerInfo`](/Develop/References/CICInterface/TemplateRuntime.md#RenderPlayerInfo)ディレクティブの`playableItems[].token`フィールドの値が適用される必要があります。[`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)ディレクティブを受信した場合、そのメッセージの`token`フィールドの値を入力する必要があることもあります。  | 任意  |

### 備考
* ユーザーがクライアント上で「再生」ボタン（Play）を押した場合には、[`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)イベントをCICに送信する必要があります。

### Message example

```json
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Speaker.VolumeState}},
    {{SpeechSynthesizer.SpeechState}}
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "PlayCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}
```

### 次の項目も参照してください。
* [`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectPlayCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

## Previousディレクティブ {#Previous}
クライアントに、再生キューにある前のオーディオストリームを再生するように指示します。クライアントは、このディレクティブを受信すると、前のオーディオストリームを再生する必要があります。

### Payload fields
なし

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Previous",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### 次の項目も参照してください。
* [`PlaybackController.Next`](#Next)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## PreviousCommandIssuedイベント {#PreviousCommandIssued}
ユーザーがクライアントデバイスで「前へ」ボタン（Previous）を押したり、CICから[`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。CICはこのイベントを受信すると、適切なディレクティブをクライアントに送信します。

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | クライアントデバイスのID。オーディオの再生をリモートでコントロールする状況でない場合、このフィールドは省略します。 | 任意 |
| `source`      | object  | イベントのソースを持つオブジェクト。このイベントを送信した状況やコンテキストに関する情報が含まれます。<div class="tip"><p><strong>ヒント</strong></p><p>クライアントがユーザーに音声出力（TTS）とオーディオコンテンツの再生を同時に提供している場合、通常、制御対象となるのは音声出力のほうです。このフィールドを使用すると、このイベントが制御する対象を指定できます。Clovaが制御対象を指定するように委任するには、このフィールドを省略します。</p></div>  | 任意  |
| `source.namespace` | string | CIC APIの名前空間。このイベントが送信された状況やコンテキストを確認できるように、関連するCIC APIの名前空間を入力します。現在、`"AudioPlayer"`のみ入力できます。  |   |

### 備考
* クライアントデバイスのボタンは、ハードウェア方式の物理ボタンの場合もあり、オーディオプレーヤーウィジェットのボタンのようなソフトウェア方式のボタンの場合もあります。

### Message example

```json
//サンプル1：ソースが指定されていないサンプル
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Speaker.VolumeState}},
    {{SpeechSynthesizer.SpeechState}}
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "PreviousCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}

//サンプル2：ソースが「AudioPlayer」に指定されたサンプル
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Speaker.VolumeState}},
    {{SpeechSynthesizer.SpeechState}}
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "PreviousCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "source": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### 次の項目も参照してください。
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

## Replayディレクティブ {#Replay}
クライアントに、オーディオストリームを最初から再度再生するように指示します。クライアントはこのディレクティブを受信すると、再生位置をオーディオストリームの最初に移動し、すぐにオーディオストリームの再生を開始する必要があります。もし、オーディオストリームの再生が一時停止していた場合には、オーディオストリームの再生を再開します。

### Payload fields
なし

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Replay",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### 次の項目も参照してください。
* [`PlaybackController.Pause`](#Pause)
* [`PlaybackController.Resume`](#Resume)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
* [オーディオ再生をコントロールする](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- Start of the shared content: PlaybackController.Resume -->

## Resumeディレクティブ {#Resume}
クライアントに、オーディオストリームの再生を再開するように指示します。クライアントは、このディレクティブを受信すると、オーディオストリームの再生を再開する必要があります。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 制御対象の情報を持つオブジェクト。このディレクティブから制御対象を確認できます。 | 条件付き  |
| target.namespace  | string  | CIC APIの名前空間。制御対象を確認するための情報です。以下の値を持ちます。<ul><li><code>"AudioPlayer"</code>：AudioPlayer</li><li><code>"MediaPlayer"</code>：MediaPlayer</li></ul>  |   |

### Message example

```json
//サンプル1：対象が指定されていないサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Resume",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}

//サンプル2：対象が「AudioPlayer」に指定されたサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Resume",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### 次の項目も参照してください。
* [`AudioPlayer.PlayResumed`](/Develop/References/CICInterface/AudioPlayer.md#PlayResumed)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
* [オーディオ再生をコントロールする](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

## ResumeCommandIssuedイベント {#ResumeCommandIssued}
ユーザーがクライアントデバイスで「再生」ボタン（Play）または「再開」ボタン（Resume）を押したり、CICから[`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。CICはこのイベントを受信すると、適切なディレクティブをクライアントに送信します。

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | クライアントデバイスのID。オーディオの再生をリモートでコントロールする状況でない場合、このフィールドは省略します。 | 任意 |
| `source`      | object  | イベントのソースを持つオブジェクト。このイベントを送信した状況やコンテキストに関する情報が含まれます。<div class="tip"><p><strong>ヒント</strong></p><p>クライアントがユーザーに音声出力（TTS）とオーディオコンテンツの再生を同時に提供している場合、通常、制御対象となるのは音声出力のほうです。このフィールドを使用すると、このイベントが制御する対象を指定できます。Clovaが制御対象を指定するように委任するには、このフィールドを省略します。</p></div>  | 任意  |
| `source.namespace` | string | CIC APIの名前空間。このイベントが送信された状況やコンテキストを確認できるように、関連するCIC APIの名前空間を入力します。現在、`"AudioPlayer"`のみ入力できます。  |   |

### 備考
* クライアントデバイスのボタンは、ハードウェア方式の物理ボタンの場合もあり、オーディオプレーヤーウィジェットのボタンのようなソフトウェア方式のボタンの場合もあります。
* **ユーザーが再生ボタンを押した場合**にも、このイベントを使用する必要があります。

### Message example

```json
//サンプル1：ソースが指定されていないサンプル
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Speaker.VolumeState}},
    {{SpeechSynthesizer.SpeechState}}
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ResumeCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}

//サンプル2：ソースが「AudioPlayer」に指定されたサンプル
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Speaker.VolumeState}},
    {{SpeechSynthesizer.SpeechState}}
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ResumeCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "source": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### 次の項目も参照してください。
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectResumeCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)
* [オーディオ再生をコントロールする](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

## SetRepeatModeディレクティブ {#SetRepeatMode}

クライアントに、指定されたリピート再生モードに再生状態を変更するように指示します。クライアントはこのディレクティブを受信すると、リピートモードの状態（[コンテキストオブジェクト](/Develop/References/Context_Objects.md)）を指定された値に変更し、[`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)イベントの`repeatMode`フィールドを指定された値に設定して送信する必要があります。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `repeatMode`  | string  | 選択されたリピートモード<ul><li><code>NONE</code>：リピート再生しない</li><code>REPEAT_ONE</code>：一曲リピート再生</li></ul>  |  |

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "SetRepeatMode",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {
      "repeatMode": "NONE"
    }
  }
}
```

### 次の項目も参照してください。
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)

## SetRepeatModeCommandIssuedイベント {#SetRepeatModeCommandIssued}

ClovaアプリまたはコンパニオンアプリからリモートでClovaデバイスの再生モードを変更するときに使用します。CICはこのイベントを受信すると、[`PlaybackController.SetRepeatMode`](#SetRepeatMode)ディレクティブを対象のクライアントに送信します。

<div class="note">
  <p><strong>メモ</strong></p>
  <p>コンパニオンアプリからリモートでClovaデバイスの再生モードを変更するとき以外は、クライアントがこのイベントを作成・処理する必要はありません。クライアントデバイスの再生モードの変化は、<a href="/Develop/References/Context_Objects.md#PlaybackState"><code>AudioPlayer.PlaybackState</code></a>コンテクストの<code>repeatMode</code>フィールドを使用してCICにレポートします。</p>
</div>

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | クライアントデバイスのID。オーディオの再生をリモートでコントロールする状況でない場合、このフィールドは省略します。                               | 任意 |
| `repeatMode`  | string  | 選択されたリピートモード<ul><li><code>NONE</code>：リピート再生しない</li><code>REPEAT_ONE</code>：一曲リピート再生</li></ul>  |  |

### 備考
* コンパニオンアプリからClovaデバイスの再生モードを変更する機能を実装しない場合、このイベントを作成したり、このメッセージに関連して処理する作業はありません。
* クライアントデバイスの再生モードの変化は、[`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)コンテクストの`repeatMode`フィールドを使用してCICにレポートします。

### Message example

```json
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Speaker.VolumeState}},
    {{SpeechSynthesizer.SpeechState}}
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "SetRepeatModeCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "repeatMode": "NONE"
    }
  }
}
```

### 次の項目も参照してください。
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.SetRepeatMode`](#SetRepeatMode)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

<!-- Start of the shared content: PlaybackController.Stop -->

## Stopディレクティブ {#Stop}
クライアントに、オーディオストリームの再生を停止するように指示します。クライアントは、このディレクティブを受信すると、オーディオストリームの再生を停止する必要があります。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | 制御対象の情報を持つオブジェクト。このディレクティブから制御対象を確認できます。 | 条件付き  |
| target.namespace  | string  | CIC APIの名前空間。制御対象を確認するための情報です。以下の値を持ちます。<ul><li><code>"AudioPlayer"</code>：AudioPlayer</li><li><code>"MediaPlayer"</code>：MediaPlayer</li></ul>  |   |

### Message example

```json
//サンプル1：対象が指定されていないサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Stop",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}

//サンプル2：対象が「AudioPlayer」に指定されたサンプル
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Stop",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### 次の項目も参照してください。
* [`AudioPlayer.PlayStopped`](/Develop/References/CICInterface/AudioPlayer.md#PlayStopped)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
* [オーディオ再生をコントロールする](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

## StopCommandIssuedイベント {#StopCommandIssued}
ユーザーがクライアントデバイスで「再開」ボタン（Resume）を押したり、CICから[`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。CICはこのイベントを受信すると、適切なディレクティブをクライアントに送信します。

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | クライアントデバイスのID。オーディオの再生をリモートでコントロールする状況でない場合、このフィールドは省略します。 | 任意 |
| `source`      | object  | イベントのソースを持つオブジェクト。このイベントを送信した状況やコンテキストに関する情報が含まれます。<div class="tip"><p><strong>ヒント</strong></p><p>クライアントがユーザーに音声出力（TTS）とオーディオコンテンツの再生を同時に提供している場合、通常、制御対象となるのは音声出力のほうです。このフィールドを使用すると、このイベントが制御する対象を指定できます。Clovaが制御対象を指定するように委任するには、このフィールドを省略します。</p></div>  | 任意  |
| `source.namespace` | string | CIC APIの名前空間。このイベントが送信された状況やコンテキストを確認できるように、関連するCIC APIの名前空間を入力します。現在、`"AudioPlayer"`のみ入力できます。  |   |

### 備考
* クライアントデバイスのボタンは、ハードウェア方式の物理ボタンの場合もあり、オーディオプレーヤーウィジェットのボタンのようなソフトウェア方式のボタンの場合もあります。

### Message example

```json
//サンプル1：ソースが指定されていないサンプル
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Speaker.VolumeState}},
    {{SpeechSynthesizer.SpeechState}}
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "StopCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}

//サンプル2：ソースが「AudioPlayer」に指定されたサンプル
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Speaker.VolumeState}},
    {{SpeechSynthesizer.SpeechState}}
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "StopCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "source": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### 次の項目も参照してください。
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectResumeCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [オーディオ再生をコントロールする](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

## TurnOffRepeatModeディレクティブ {#TurnOffRepeatMode}
**（Deprecated）** クライアントに、1曲リピート再生モードを無効にするように指示します。

### Payload fields
なし

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "TurnOffRepeatMode",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### 次の項目も参照してください。
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## TurnOnRepeatModeディレクティブ {#TurnOnRepeatMode}
**（Deprecated）** クライアントに、1曲リピート再生モードを有効にするように指示します。クライアントは、このディレクティブを受信すると、現在再生しているオーディオストリームをリピート再生する必要があります。

### Payload fields
なし

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "TurnOnRepeatMode",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### 次の項目も参照してください。
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## Unmuteディレクティブ {#Unmute}
クライアントに、オーディオプレーヤーのミュートを解除するように指示します。クライアントはこのディレクティブを受信すると、スピーカーのミュートを解除して、元の音量に戻す必要があります。

### Payload fields
なし

### 備考

Clovaは、スピーカーの出力に関連するコントロールの場合、通知のためのオーディオファイルを[`SpeechSynthesizer.Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak)ディレクティブに添付しません。それは、ユーザーの音楽鑑賞などのUXを向上させるためで、その場合には音声案内の代わりに、クライアントデバイスのライトや、短い効果音などで音量が調節されたことをユーザーに通知する必要があります。

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Unmute",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### 次の項目も参照してください。
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## VolumeDownディレクティブ {#VolumeDown}
**（Deprecated）** クライアントに、オーディオプレーヤーの音量を下げるように指示します。クライアントはこのディレクティブを受信すると、オーディオストリームの再生に関連するスピーカーの音量を下げる必要があります。下げる量は、クライアントのUX基準に従います。

<div class="note">
  <p><strong>メモ</strong></p>
  <p><code>PlaybackController.VolumeDown</code>ディレクティブは、今後サポートされない予定です。このディレクティブの代わりに、<a href="/Develop/References/CICInterface/DeviceControl.md#Decrease"><code>DiviceControl.Decrease</code></a>ディレクティブを使用する必要があります。</p>
</div>

### Payload fields
なし

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "VolumeDown",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### 次の項目も参照してください。
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## VolumeUpディレクティブ {#VolumeUp}

**（Deprecated）** クライアントに、オーディオプレーヤーの音量を上げるように指示します。クライアントはこのディレクティブを受信すると、オーディオストリームの再生に関連するスピーカーの音量を上げる必要があります。上げる量は、クライアントのUX基準に従います。

<div class="note">
  <p><strong>メモ</strong></p>
  <p><code>PlaybackController.VolumeUp</code>ディレクティブは、今後サポートされない予定です。このディレクティブの代わりに、<a href="/Develop/References/CICInterface/DeviceControl.md#Increase"><code>DiviceControl.Increase</code></a>ディレクティブを使用する必要があります。</p>
</div>

### Payload fields
なし

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "VolumeUp",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### 次の項目も参照してください。
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
