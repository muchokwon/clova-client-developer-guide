## Speaker.VolumeState {#VolumeState}
`Speaker.VolumeState` is a format for reporting to CIC about the client speaker state at the time when the user speaks, including mute status and volume level.

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

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `muted`         | boolean | Indicates whether the speaker is muted or not.                    | Required     |
| `volume`        | number  | The current speaker volume level (0-10).     | Required     |

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

### See also
* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
