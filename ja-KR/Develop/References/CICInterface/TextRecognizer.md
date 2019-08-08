# TextRecognizer

TextRecognizerインターフェースは、ユーザーから入力されたテキストを認識するときに使用される名前空間です。

## Recognizeイベント {#Recognize}
`TextRecognizer.Recognize`イベントは、ユーザーから取得したテキストをCICに送信して、ユーザーの意図を認識するようにリクエストします。Clova内部の自然言語解析システムと対話理解システムが、入力されたテキストを解析し、ユーザーのリクエストを処理します。ユーザーの音声を取得することが困難な環境にあるか、マイクが正常に動作しない場合、[`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)イベントの代わりに使用して、ユーザーからテキストの入力を受けることができます。

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields
| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `explicit`         | boolean  | [`SpeechRecognizer.ExpectSpeech`](/Develop/References/CICInterface/SpeechRecognizer.md#ExpectSpeech)ディレクティブによって、ユーザーの音声を追加で取得する場合、`SpeechRecognizer.ExpectSpeech`ディレクティブに含まれた`explicit`フィールドの値をそのまま入力します。  | 任意  |
| `speechId`   | string   | [`SpeechRecognizer.ExpectSpeech`](/Develop/References/CICInterface/SpeechRecognizer.md#ExpectSpeech)ディレクティブによって、ユーザーの音声を追加で取得する場合、`SpeechRecognizer.ExpectSpeech`ディレクティブに含まれた`expectSpeechId`フィールドの値をそのまま入力します。  | 任意  |
| `text`        | string  | ユーザーが入力したテキストを設定します。 |      |

### Message example
{% raw %}
```json
// ユーザーがテキストを入力する場合
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
      "namespace": "TextRecognizer",
      "name": "Recognize",
      "dialogRequestId": "bc834682-6d22-4bbb-8352-4a49df2ed3d7",
      "messageId": "b120c3e0-e6b9-4a3d-96de-71539e5f6214"
    },
    "payload": {
      "text": "今の天気は?"
    }
  }
}

// SpeechRecognizer.ExpectSpeechディレクティブによって、追加でテキストを入力する場合
{
  "header": {
      "dialogRequestId": "d3f81fec-4cb9-4ce9-a046-1ea9a71018df",
      "messageId": "8526a048-4141-4c30-98a4-c61e223afece",
      "namespace": "TextRecognizer",
      "name": "Recognize"
  },
  "payload": {
      "text": "明日は?",
      "speechId": "1a4cd9ac-8fd8-4929-9c30-3a592dd2c298",
      "explicit": false
  }
}
```
{% endraw %}

### 次の項目も参照してください。
* [`SpeechRecognizer.ExpectSpeech`](/Develop/References/CICInterface/SpeechRecognizer.md#ExpectSpeech)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
