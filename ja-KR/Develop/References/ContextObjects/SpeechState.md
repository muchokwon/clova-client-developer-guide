## SpeechSynthesizer.SpeechState {#SpeechState}
`SpeechSynthesizer.SpeechState`は、クライアントが[`SpeechSynthesizer.Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak)ディレクティブで送信されたTTS（text-to-speech）を再生しているかをCICにレポートするときに使用されるメッセージ形式です。

### Object structure
{% raw %}
```json
{
  "header": {
      "namespace": "SpeechSynthesizer",
      "name": "SpeechState"
  },
  "payload": {
      "token": {{string}},
      "playerActivity": {{string}}
  }
}
```
{% endraw %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `playerActivity` | string | TTSの再生状態<ul><li><code>PLAYING</code>：現在のTTSの再生状態</li><li><code>STOPPED</code>：ユーザーの音声入力や他の割り込みにより、TTS再生が途中で停止されている</li><li><code>FINISHED</code>：TTS再生が完了している</li></ul>     |      |
| `token`          | string | [`SpeechSynthesizer.Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak)ディレクティブで送信されたTTSを識別するためのトークン。  |      |

### Object example
{% raw %}
```json
{
  "header": {
      "namespace": "SpeechSynthesizer",
      "name": "SpeechState"
  },
  "payload": {
      "token": "dc706e02-fe16-4337-9a6c-51f670b5adb2",
      "playerActivity": "FINISHED"
  }
}
```
{% endraw %}

### 次の項目も参照してください。
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
