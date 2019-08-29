# SpeechRecognizer

SpeechRecognizerインターフェースは、ユーザーの音声を認識するために使用する名前空間です。ユーザーの発話は、次のように入力されます。

1. クライアントは、ユーザーの音声入力が開始すると、CICに[`SpeechRecognizer.Recognize`](#Recognize)イベントを送信します。
2. クライアントは、入力されるユーザーの音声を200ミリ秒ずつ分割し、リアルタイムでCICに送信します。
3. クライアントは、CICから[`SpeechRecognizer.StopCapture`](#StopCapture)ディレクティブを受信するまで、ステップ2を続ける必要があります。

SpeechRecognizerは、次のイベントとディレクティブを提供します。

| メッセージ         | タイプ  | 説明                                   |
|------------------|-----------|---------------------------------------------|
| [`ExpectSpeech`](#ExpectSpeech)                 | ディレクティブ | クライアントに、ユーザーの音声の取得を開始するように指示します。                  |
| [`KeepRecording`](#KeepRecording)               | ディレクティブ | クライアントに、ユーザーの音声の取得を続けるように指示します。                     |
| [`Recognize`](#Recognize)                       | イベント     | 取得されるユーザーの音声を送信して、その音声を認識するようにCICにリクエストします。          |
| [`ShowRecognizedText`](#ShowRecognizedText)     | ディレクティブ | クライアントに、認識されたユーザーの音声をリアルタイムで送信します。              |
| [`StopCapture`](#StopCapture)                   | ディレクティブ | クライアントに、ユーザーの音声の取得を停止するように指示します。           |

## ExpectSpeechディレクティブ {#ExpectSpeech}

クライアントに対して、マイクを有効にし、ユーザーの音声の取得を開始するように指示します。このディレクティブは、ユーザーのリクエストで足りない情報を、CICから追加で要求するマルチターンの対話を開始するために送信されます。取得したユーザーの音声は、[`SpeechRecognizer.Recognize`](#Recognize)イベントでCICに送信されます。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `expectContentType`        | string  | クライアントが追加でユーザーの音声を取得するとき、その音声データを送信するファイルフォーマットを指定します。次の値を持ちます。<ul><li><code>"audio/l16"</code>：音声認識のために、エコー/ノイズ除去および後処理をしていない、PCMフォーマットの音声データ</li><li><code>"application/x-clova-feat"</code>：音声認識のために、エコー/ノイズの除去および後処理をした、PCMフォーマットの音声データ</li></ul>  | 条件付き  |
| `expectSpeechId`        | string  | ユーザーの音声を追加で取得するとき、それをCICで識別するためのID。この値は後に、追加で取得したユーザーの音声を[`SpeechRecognizer.Recognize`](#Recognize)イベントでCICに送信するとき、`speechId`フィールドに入力する必要があります。    |  |
| `explicit`              | boolean | ユーザーの音声を追加で取得する必要があるかどうか。`explicit`は、主に`true`に設定され、追加の情報が必要なことを示します。<ul><li><code>true</code>：必須</li><li><code>false</code>：任意</li></ul>例えば、ユーザーから「ピザを注文して」のようにリクエストされた場合、CICは数量などの必須情報を取得するために、「何枚注文しますか?」という音声と共に、`explicit`フィールドが`true`に設定された`SpeechRecognizer.ExpectSpeech`ディレクティブを送信します。その場合、ユーザーから数に関する情報を取得できない場合、正しくピザを注文することができません。`explicit`フィールドが`true`の場合、必ずユーザーから追加の音声を取得する必要があります。 |   |
| `timeoutInMilliseconds` | number  | ユーザーの音声入力を待機する時間。整数形式の値で、ミリ秒単位です。 |     |

### 備考
* クライアントはこのディレクティブを受信すると、ユーザーの音声をCICに送信するとき、前のリクエストと同じダイアログID（`dialogRequestId`）を使用する必要があります。
* `explicit`フィールドが`false`の場合には、クライアントのポリシーや条件によって、ユーザーの音声を取得しないこともできます。

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "ExpectSpeech",
      "dialogRequestId": "bc834682-6d22-4bbb-8352-4a49df2ed3d7",
      "messageId": "b120c3e0-e6b9-4a3d-96de-71539e5f6214"
    },
    "payload": {
      "timeoutInMilliseconds": 7000,
      "explicit": false,
      "expectSpeechId": "561aeecf-2096-40fa-ba17-6612e28b339f"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。

* [`SpeechRecognizer.Recognize`](#Recognize)

## KeepRecordingディレクティブ {#KeepRecording}

クライアントに、ユーザーの音声の取得を続けるように指示します。`SpeechRecognizer.KeepRecording`ディレクティブは、クライアントで取得したユーザーの音声が認識されていることを示す中間応答です。このディレクティブを受信したクライアントは、CICからの応答に対するタイムアウトを実装するか、UXに活用することができます。

### Payload fields

なし

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "KeepRecording",
      "dialogRequestId": "bc834682-6d22-4bbb-8352-4a49df2ed3d7",
      "messageId": "b120c3e0-e6b9-4a3d-96de-71539e5f6214"
    },
    "payload": {}
  }
}
```

{% endraw %}

### 次の項目も参照してください。

* [`SpeechRecognizer.Recognize`](#Recognize)

## Recognizeイベント {#Recognize}
`SpeechRecognizer.Recognize`イベントは、取得されたユーザーの音声をCICに送信して、ユーザーの意図を認識するようにリクエストするためのものです。Clova内部の自然言語解析システムと対話理解システムが入力された音声を解析し、ユーザーのリクエストを処理します。ほとんどのCICからの[ディレクティブ](/Develop/References/CIC_API.md#Directive)は、`SpeechRecognizer.Recognize`イベントでユーザーのリクエストを確認してから送信されます。

次のフォーマットの音声データを処理できます。
* 16ビットのリニアPCM
* 16kHzのサンプリングレート
* モノラルチャンネル
* リトルエンディアン

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields
| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `explicit`                                               | boolean  | [`SpeechRecognizer.ExpectSpeech`](#ExpectSpeech)ディレクティブによって、ユーザーの音声を追加で取得する場合、`SpeechRecognizer.ExpectSpeech`ディレクティブに含まれた`explicit`フィールドの値をそのまま入力します。  | 任意  |
| `format`                                                 | string   | 音声データのフォーマット。`AUDIO_L16_RATE_16000_CHANNELS_1`に固定します。                             | 任意    |
| `initiator`                                              | object   | Clovaの呼び出し方、音声の取得方法、ウェイクワードに関する情報を持つオブジェクト<div class="tip"><p><strong>ヒント</strong></p><p>このフィールドを使用することで、音声認識の精度を上げることができます。従って、このフィールドを使用することを推奨します。</p></div>                      | 任意    |
| `initiator.inputSource`                                  | string   | ユーザーの音声を取得した経路（source）。次のいずれかを指定します。<ul><li><code>SELF</code>：<code>SpeechRecognizer.Recognize</code>イベントを送信したクライアントで直接ユーザーの音声を取得した場合、この値を指定します。</li><li><code>CUSTOM_{Source_ID}</code>：<code>SpeechRecognizer.Recognize</code>イベントを送信したクライアントではなく、リモコンなどの別のデバイスでユーザーの音声を取得した場合、そのデバイスのIDを指定します。</li></ul><div class="note"><p><strong>メモ</strong></p><p>デバイスのIDは、あらかじめ提携担当者と協議済みの値を使用してください。</p></div>  |  |
| `initiator.payload`                                      | object   | `initiator`フィールドで、詳細情報を持つオブジェクト                                                        | 任意 |
| `initiator.payload.deviceUUID`                           | string   | デバイスで任意に作成したUUID。一度作成したUUIDを一貫して使用します。また、Clovaで特定のユーザーを識別できない値である必要があります。このフィールドの値として、{{ book.ServiceEnv.TargetServiceForClientAuth }}アクセストークン、ClovaアクセストークンやクライアントID、またはこれらを組み合わせた値を使用すべきではありません。   |  |
| `initiator.payload.wakeWord`                             | object   | クライアントで認識されたウェイクワードを持つオブジェクト。ウェイクワード認識の精度を高めるために使用されます。       | 任意 |
| `initiator.payload.wakeWord.confidence`                  | number   | デバイスで、ウェイクワードの認識を確信する程度（confidence）を示します。0から1までの実数型（float）の値を入力します。現在、このフィールドは有効ではありません。今後のために確保されているフィールドです。                 | 任意 |
| `initiator.payload.wakeWord.indices`                      | object   | ユーザーの音声が含まれたオーディオストリームで、ウェイクワードに該当する区間の情報を持つオブジェクト                                           |  |
| `initiator.payload.wakeWord.indices.endIndexInSamples`    | number   | オーディオストリームで、ウェイクワードが終了する位置のインデックス情報。音声入力が16kHzのサンプリングレートを持つため、インデックスの1単位は1/16,000秒になります。もしオーディオストリームでウェイクワードに該当する区間が1秒の位置で終わる場合、このフィールドに1秒を表す`16000`を入力します。  |   |
| `initiator.payload.wakeWord.indices.startIndexInSamples`  | number   | オーディオストリームで、ウェイクワードが開始する位置のインデックス情報。音声入力が16kHzのサンプリングレートを持つため、インデックスの1単位は1/16,000秒になります。通常、ユーザーの発話はウェイクワードで開始することが多いため、その場合にはインデックスの値を0に入力します。   |  |
| `initiator.payload.wakeWord.name`                         | string   | クライアントデバイスに設定されているウェイクワード。次の値を入力できます。<ul><li><code>"clova"</code></li><li><code>"jesika"</code></li><li><code>"jjangguya"</code></li><li><code>"seliya"</code></li><li><code>"pinokio"</code></li></ul>                        | 任意  |
| `initiator.type`                                         | string   | ユーザーがClovaを呼び出すために行ったアクション。次の値を入力できます。<ul><li><code>"PRESS_AND_HOLD"</code>：音声入力取得ボタン（wake up）を押したまま音声を入力した場合</li><li><code>"TAP"</code>：音声入力取得ボタン（wake up）を押したまま音声を入力した場合</li><li><code>"WAKEWORD"</code>：ウェイクワードにより音声を入力した場合</li></ul>  |  |
| `lang`                                                   | string   | ユーザーの発話を認識する言語を指定します。<ul><li><code>"en"</code>：英語</li><li><code>"ja"</code>：日本語</li><li><code>"ko"</code>：韓国語</li></ul> |     |
| `profile`                                                | string   | 今後のために確保されているフィールド。`CLOSE_TALK`に固定します。                                     | 任意    |
| `speechId`                                               | string   | [`SpeechRecognizer.ExpectSpeech`](#ExpectSpeech)ディレクティブによって、ユーザーの音声を追加で取得する場合、`SpeechRecognizer.ExpectSpeech`ディレクティブに含まれた`expectSpeechId`フィールドの値をそのまま入力します。  | 任意  |

### Message example
{% raw %}
```json
// 通常、ユーザーの音声を取得する場合
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
      "namespace": "SpeechRecognizer",
      "name": "Recognize",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "lang": "ja",
      "profile": "CLOSE_TALK",
      "format": "AUDIO_L16_RATE_16000_CHANNELS_1",
      "initiator": {
        "type": "WAKEWORD",
        "inputSource": "SELF",
        "payload": {
          "deviceUUID": "f003af9d-14c5-424b-b1f9-f0134bd0ed86",
          "wakeWord": {
            "name": "clova",
            "confidence": 0.812312,
            "indices": {
              "startIndexInSamples": 0,
              "endIndexInSamples": 16000
            }
          }
        }
      }
    }
  }
}

// SpeechRecognizer.ExpectSpeechディレクティブによって、ユーザーの音声を追加で取得する場合
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
  "header": {
      "dialogRequestId": "4951cbfe-0064-41e2-ac3a-b0e4e1b0a570",
      "messageId": "6ab89102-668b-42eb-89d0-639253db10ba",
      "namespace": "SpeechRecognizer",
      "name": "Recognize"
  },
  "payload": {
      "profile": "CLOSE_TALK",
      "format": "AUDIO_L16_RATE_16000_CHANNELS_1",
      "speechId": "561aeecf-2096-40fa-ba17-6612e28b339f",
      "explicit": false
  }
}

```
{% endraw %}

### Audio Data
`SpeechRecognizer.Recognize`イベントを送信してから、ユーザーが音声の入力を終了するか、または[StopCapture](#StopCapture)ディレクティブを受信するまで、次の音声データを送り続けます。その際、音声データは同じHTTPリクエスト内で、マルチパートのメッセージで送信される必要があります。

```
[ Message Header ]
Content-Disposition: form-data; name="audio"
Content-Type: application/octet-stream

[ PCMオーディオ添付部 ]
```

<div class="note">
  <p><strong>メモ</strong></p>
  <p><code>initiator.type</code>の値が<code>"WAKEWORD"</code>の場合、送信する音声データに、ウェイクワードに該当する音声が必ず含まれている必要があります。その際、音声データは、ウェイクワード区間の開始点より300ミリ秒（4,800サンプル）前から開始する必要があります。</p>
</div>

### 次の項目も参照してください。
* [`SpeechRecognizer.ExpectSpeech`](#ExpectSpeech)
* [`SpeechRecognizer.StopCapture`](#StopCapture)

## ShowRecognizedTextディレクティブ {#ShowRecognizedText}

Clovaの音声認識システムは、[`SpeechRecognizer.Recognize`](#Recognize)イベントで送信されているユーザーの音声を解析して、その認識結果を提供します。CICは`SpeechRecognizer.ShowRecognizedText`ディレクティブで、ユーザーの音声認識の中間処理結果をクライアントに送信します。クライアントはそれに基づいて、処理過程をユーザーにリアルタイムで表示することができます。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `text`  | string | 取得したユーザーの音声が認識されていく結果がリアルタイムで含まれます。 |     |

### 備考

* このディレクティブは、イベントに対する応答ではなく、[Downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)で送信されます。
* 基本的に、CICは音声認識の中間結果をクライアントに送信せず、一部特殊な条件で`SpeechRecognizer.ShowRecognizedText`を送信します。

<div class="note">
  <p><strong>メモ</strong></p>
  <p>このディレクティブを使用するには、Clova事務局までお問い合わせください。</p>
</div>

### Message example

{% raw %}

```json
// 中間認識結果1
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "ShowRecognizedText",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "ceb4c638-d817-4a65-977d-03726d72cb91"
    },
    "payload": {
      "text": "今日"
    }
  }
}

// 中間認識結果2
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "ShowRecognizedText",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "fab4c638-d8df7-4adf-977d-adf72cb91"
    },
    "payload": {
      "text": "今日の天気"
    }
  }
}

// 中間認識結果3
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "ShowRecognizedText",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "efac233-8df7d-14df-d37d-72f72cb91"
    },
    "payload": {
      "text": "今日の天気を教えて"
    }
  }
}

```
{% endraw %}

### 次の項目も参照してください。

* [`SpeechRecognizer.Recognize`](#Recognize)
* [`SpeechRecognizer.StopCapture`](#StopCapture)

## StopCaptureディレクティブ {#StopCapture}
CICが[`SpeechRecognizer.Recognize`](#Recognize)イベントを受信し、これ以上キャプチャされたオーディオ（PCM）を受信する必要がないと判断した場合、`SpeechRecognizer.StopCapture`ディレクティブをクライアントに送信します。クライアントはこのメッセージを受信したら、すぐにユーザーの音声のキャプチャを終了します。CICがこのメッセージを送信してからもユーザーの音声を受信することがありますが、その音声は処理されません。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `recognizedText` | string | 取得したユーザーの音声がどのように認識されたかという結果が含まれます。このフィールドは、基本的に`SpeechRecognizer.StopCapture`ディレクティブには含まれず、一部特殊な条件でのみ含まれます。<div class="note"><p><strong>メモ</strong></p><p>このディレクティブを使用するには、Clova事務局までお問い合わせください。</p></div> | 条件付き |

### 備考
このディレクティブはイベントに対する応答ではなく、[Downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)で送信されます。

### Message example

```json
//サンプル1：recognizedTextフィールドがあるサンプル
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "StopCapture",
      "dialogRequestId": "eaa19eaa-07bc-447a-9e3f-c3b4a7d994e8",
      "messageId": "cc9f2a05-34c8-4edd-b810-2c040ac3d672"
    },
    "payload": {
      "recognizedText": "今日の天気を教えて"
    }
  }
}
```

//サンプル2：recognizedTextフィールドがないサンプル
```json
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "StopCapture",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}
```

### 次の項目も参照してください。

* [`SpeechRecognizer.Recognize`](#Recognize)
* [`SpeechRecognizer.ShowRecognizedText`](#ShowRecognizedText)
