## SpeechSynthesizer.SpeechState {#SpeechState}
`SpeechSynthesizer.SpeechState` is a format for reporting to CIC about whether or not the client is playing the text-to-speech (TTS) sent as the [`SpeechSynthesizer.Speak`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak) directive.

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

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `playerActivity` | string | The TTS playback state. <ul><li><code>PLAYING</code>: Currently playing</li><li><code>STOPPED</code>: Stopped in the middle due to user voice input or other interruptions</li><li><code>FINISHED</code>: Completed playing</li></ul>     | Required     |
| `token`          | string | The token value for TTS identification received from the [`SpeechSynthesizer.Speak`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak) directive message.  | Required     |

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

### See also
* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
