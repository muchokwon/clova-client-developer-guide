## Speaker.VolumeState {#VolumeState}
`Speaker.VolumeState`は、ユーザーが発話した時点で、クライアントのスピーカー音量やミュート情報をCICにレポートするときに使用されるメッセージ形式です。

### Object structure
{% raw %}
```json
{
  "header": {
      "namespace": "Speaker",
      "name": "VolumeState"
  },
  "payload": {
      "volume": {{number}},
      "muted": {{boolean}}
  }
}
```
{% endraw %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `muted`         | boolean | ミュートになっているかどうかを示す値。                    |      |
| `volume`        | number  | 現在のスピーカー音量（0-10）     |      |

### Object example
{% raw %}
```json
{
  "header": {
      "namespace": "Speaker",
      "name": "VolumeState"
  },
  "payload": {
      "volume": 8,
      "muted": false
  }
}
```
{% endraw %}

### 次の項目も参照してください。
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
