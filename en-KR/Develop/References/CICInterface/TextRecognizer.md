# TextRecognizer

The TextRecognizer namespace provides an interface to request to CIC to recognize the user text entry.

## Recognize event {#Recognize}
The `TextRecognizer.Recognize` event sends the user text entry to CIC and requests to recognize what the user wants. The natural language analysis system and dialogue understanding system of Clova interpret the text input and processes user requests accordingly. Use this event instead of the [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize) event message if the client cannot receive voice input from a user or the client microphone is not functioning properly.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields
| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `explicit`         | boolean  | To acquire additional user input due to the [`SpeechRecognizer.ExpectSpeech`](/Develop/References/CICInterface/SpeechRecognizer.md#ExpectSpeech) directive, use the value of the `explicit` field specified in the `SpeechRecognizer.ExpectSpeech` directive.  | Optional  |
| `speechId`   | string   | To acquire additional user input due to the [`SpeechRecognizer.ExpectSpeech`](/Develop/References/CICInterface/SpeechRecognizer.md#ExpectSpeech) directive, use the value of the `expectSpeechId` field specified in the `SpeechRecognizer.ExpectSpeech` directive.  | Optional  |
| `text`        | string  | The text entered by a user. | Required     |

### Message example
{% raw %}
```json
// General user text input
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
      "text": "How is the weather now?"
    }
  }
}

// Additional text input for the SpeechRecognizer.ExpectSpeech directive
{
  "header": {
      "dialogRequestId": "d3f81fec-4cb9-4ce9-a046-1ea9a71018df",
      "messageId": "8526a048-4141-4c30-98a4-c61e223afece",
      "namespace": "TextRecognizer",
      "name": "Recognize"
  },
  "payload": {
      "text": "How about tomorrow?",
      "speechId": "1a4cd9ac-8fd8-4929-9c30-3a592dd2c298",
      "explicit": false
  }
}
```
{% endraw %}

### See also
* [`SpeechRecognizer.ExpectSpeech`](/Develop/References/CICInterface/SpeechRecognizer.md#ExpectSpeech)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
