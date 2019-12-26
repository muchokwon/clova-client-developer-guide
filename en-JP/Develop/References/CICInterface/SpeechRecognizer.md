# SpeechRecognizer

The SpeechRecognizer namespace provides interfaces for processing recognition of user voice request. To have a voice request processed:

1. Send CIC the [`SpeechRecognizer.Recognize`](#Recognize) event when the user makes a voice request.
2. Continue to send the speech input to CIC in every 200ms.
3. Repeat step 2 until the [`SpeechRecognizer.StopCapture`](#StopCapture) directive from CIC is received.

The SpeechRecognizer namespace provides the following events and directives.

| Message name         | Type  | Description                                   |
|------------------|-----------|---------------------------------------------|
| [`ExpectSpeech`](#ExpectSpeech)                 | Directive | Instructs the client to receive user voice input.                  |
| [`KeepRecording`](#KeepRecording)               | Directive | Instructs the client to continually receive voice input.                     |
| [`Recognize`](#Recognize)                       | Event     | Requests CIC for speech recognition on the user audio request.          |
{% if book.DocMeta.TargetReaderType == "Internal" or book.DocMeta.TargetReaderType == "Uplus" %}| [`ShowRecognizedText`](#ShowRecognizedText)     | Directive | Returns the recognition result of a user voice request in real-time.              |
| [`StopCapture`](#StopCapture)                   | Directive | Instructs the client to stop receiving voice input from the user.           |{% else %}| [`StopCapture`](#StopCapture)                   | Directive | Instructs the client to stop receiving voice input from the user.           |{% endif %}

## ExpectSpeech directive {#ExpectSpeech}

Instructs the client to enable its microphone and receive user voice input. CIC sends this directive to engage a multi-turn dialogue to request additional information, because the information provided in the original user request was inadequate. To send the user voice request to CIC, use the [`SpeechRecognizer.Recognize`](#Recognize) event.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `expectContentType`        | string  | Data type of the additional voice input to be sent to CIC from the client. Available values are: <ul><li><code>"audio/l16"</code>: Audio data in PCM format with no post-processing for speech recognition such as removing echo or noise.</li><li><code>"application/x-clova-feat"</code>: Audio data in PCM format with post-processing for speech recognition such as removing echo or noise </li></ul>  | Conditional  |
| `expectSpeechId`        | string  | ID of additional voice input for CIC to identify. Use this value in the `speechId` field when delivering the user voice request to CIC with the [`SpeechRecognizer.Recognize`](#Recognize) event.    | Always |
| `explicit`              | boolean | Indicates whether user voice input is required or not.`explicit` is usually set as `true` and is used to obtain additional needed information from the user.<ul><li><code>true</code>: Required</li><li><code>false</code>: Optional</li></ul>Suppose a user requests, "Order me a pizza." To acquire necessary information such as quantity, CIC will ask the user, "How many boxes of pizza do you want to order?" With this question, CIC may send the `SpeechRecognizer.ExpectSpeech` directive with the `explicit` field is set to `true`. If the quantity is not specified by the user at this time, the order cannot be processed successfully. Therefore, if the `explicit` field is `true`, the client must acquire the user voice input. | Always  |
| `timeoutInMilliseconds` | number  | The standby time until the user speaks to Clova. The value is an integer and the unit is in milliseconds. | Always    |

### Remarks
* Upon receiving this directive, send the user voice recording to CIC with the same dialogue ID (`dialogRequestId`) used in the previous request message.
* If the `explicit` field is `false`, user voice input may not be required depending on the client policy or the conditions.

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

### See also

* [`SpeechRecognizer.Recognize`](#Recognize)

## KeepRecording directive {#KeepRecording}

Instructs the client to continue to receive voice input from the user. The `SpeechRecognizer.KeepRecording` directive is an interim response indicating that CIC is in the process of recognizing the user voice input received by the client. The client has an option to implement timeout for CIC response and apply the timeout in UX.

### Payload fields

None

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

### See also

* [`SpeechRecognizer.Recognize`](#Recognize)

## Recognize event {#Recognize}
`SpeechRecognizer.Recognize` event sends a user voice request to CIC that recognizes what the user wants. The natural-language processing and dialogue systems in Clova interpret user voice input and processes user requests accordingly. Most [directives](/Develop/References/CIC_API.md#Directive) from CIC are sent after CIC has confirmed the user requests sent through the `SpeechRecognizer.Recognize` events.

CIC can process the following audio formats:
* 16-bit Linear PCM
* 16 kHz sample rate
* Mono
* Little endian

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields
| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `explicit`                                               | boolean  | To acquire additional user input due to the [`SpeechRecognizer.ExpectSpeech`](#ExpectSpeech) directive, use the value of the `explicit` field specified in the `SpeechRecognizer.ExpectSpeech` directive.  | Optional  |
| `format`                                                 | string   | Audio data format of the user request. The value is always `AUDIO_L16_RATE_16000_CHANNELS_1`.                             | Optional    |
| `initiator`                                              | object   | The information on the Clova call method used, voice input source, and wake word.<div class="note"><p><strong>Note!</strong></p><p>The use of this field will improve the performance of speech recognition. Therefore, we recommend that you use this field.</p></div>                      | Optional    |
| `initiator.inputSource`                                  | string   | The source of the voice input from the user. You must enter the following values:<ul><li><code>SELF</code>: Specify this value if the client device that has sent the <code>SpeechRecognizer.Recognize</code> event receives the voice input from the user directly.</li><li><code>CUSTOM_{Source_ID}</code>: Specify the ID of the device if another device, such as a remote control that is not the client device, that has sent the <code>SpeechRecognizer.Recognize</code> event, receives the voice input.</li></ul><div class="note"><p><strong>Note!</strong></p><p>You must use the value arranged with the Clova partnership team in advance for the device ID.</p></div>  | Required |
| `initiator.payload`                                      | object   | The detailed information of the `initiator` field.                                                        | Optional |
| `initiator.payload.deviceUUID`                           | string   | The UUID randomly created from the device. Once created, the UUID must not be changed and it must not be a value that reveals information about a specific user from Clova. In other words, you must not use the {{ book.ServiceEnv.TargetServiceForClientAuth }} access token value, Clova access token value, client ID, or a combination of these values.   | Required |
| `initiator.payload.wakeWord`                             | object   | Object containing the wake word information recognized from the client. This object is used to improve the wake word recognition.       | Optional |
| `initiator.payload.wakeWord.confidence`                  | number   | The value indicating the confidence level of wake word recognition from the device. Enter a float value between 0 and 1. This field is invalid at this time and reserved for later.                 | Optional |
| `initiator.payload.wakeWord.indices`                      | object   | The section containing the wake word from the audio stream of a user voice input.                                           | Required |
| `initiator.payload.wakeWord.indices.endIndexInSamples`    | number   | The index information of the point where the wake word ends in the audio stream. As the voice input has 16 kHz sample rate, 1 unit of index stands for 1/16,000 second. For example, if the wake word section ends at the 1 second offset in the audio stream, enter `16000` in the field which represents 1 second.  | Required  |
| `initiator.payload.wakeWord.indices.startIndexInSamples`  | number   | The index information of the point where the wake word starts in the audio stream. As the voice input has 16 kHz sample rate, 1 unit of index stands for 1/16,000 second. As the wake word is usually located at the first part of user utterance, the index value is entered as 0.   | Required |
| `initiator.payload.wakeWord.name`                         | string   | The wake word set in the client device. Available values are:<ul><li><code>"clova"</code></li><li><code>"jesika"</code></li><li><code>"jjangguya"</code></li><li><code>"seliya"</code></li><li><code>"pinokio"</code></li></ul>                        | Optional  |
| `initiator.type`                                         | string   | The invoke method used. Available values are: <ul><li><code>"PRESS_AND_HOLD"</code>: Voice inputted while pressing the wake up button</li><li><code>"TAP"</code>: Voice inputted after press-releasing the wake up button</li><li><code>"WAKEWORD"</code>: Voice inputted after saying the wake word</li></ul>  | Required |
| `lang`                                                   | string   | The language of user voice request for CIC recognition. <ul><li><code>"en"</code>: English</li><li><code>"ja"</code>: Japanese</li><li><code>"ko"</code>: Korean</li></ul> | Required    |
| `profile`                                                | string   | The field reserved for future use. The value is always `CLOSE_TALK`.                                     | Optional    |
| `speechId`                                               | string   | To acquire additional user input due to the [`SpeechRecognizer.ExpectSpeech`](#ExpectSpeech) directive, use the value of the `expectSpeechId` field specified in the `SpeechRecognizer.ExpectSpeech` directive.  | Optional  |

### Message example
{% raw %}
```json
// General user request
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Device.DeviceState}},
    {{Device.Display}},
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
      "lang": "ko",
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

// Additional voice input for the SpeechRecognizer.ExpectSpeech directive
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Device.DeviceState}},
    {{Device.Display}},
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
After sending the `SpeechRecognizer.Recognize` event, continue to send the user voice recording until the user finishes speaking or the [StopCapture](#StopCapture) directive is received. For this process, the recording must be sent as a multipart message within the same HTTP request.

```
[ Message Header ]
Content-Disposition: form-data; name="audio"
Content-Type: application/octet-stream

[ PCM Audio Attachment ]
```

<div class="note">
  <p><strong>Note!</strong></p>
  <p>If the value of <code>initiator.type</code> is <code>"WAKEWORD"</code>, make sure to include the section containing the wake word when sending audio data. For this process, the audio data must be started from 300 milliseconds (4,800 sample) before the starting point of the wake word section.</p>
</div>

### See also
* [`SpeechRecognizer.ExpectSpeech`](#ExpectSpeech)
* [`SpeechRecognizer.StopCapture`](#StopCapture)
{% if book.DocMeta.TargetReaderType == "Internal" or book.DocMeta.TargetReaderType == "Uplus" %}
## ShowRecognizedText directive {#ShowRecognizedText}

This directive returns recognition results to the client in real time. The speech recognition system of Clova analyzes and returns the user voice request in real time while receiving the voice request through the [`SpeechRecognizer.Recognize`](#Recognize) event. During the recognition process, CIC returns partial recognition result to the client with the `SpeechRecognizer.ShowRecognizedText` directive. The client has an option to display the recognition progress in real time.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `text`  | string | The real-time recognition result of the user request. | Always    |

### Remarks

* This directive is sent through a [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection), which means that this directive is not a response to an event.
* CIC does not provide partial result to the client by default. The `SpeechRecognizer.ShowRecognizedText` directive is sent only under special conditions.

### Message example

{% raw %}

```json
// Partial recognition result 1
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "ShowRecognizedText",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "ceb4c638-d817-4a65-977d-03726d72cb91"
    },
    "payload": {
      "text": "Let me"
    }
  }
}

// Partial recognition result 2
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "ShowRecognizedText",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "fab4c638-d8df7-4adf-977d-adf72cb91"
    },
    "payload": {
      "text": "Let me know"
    }
  }
}

// Partial recognition result 3
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "ShowRecognizedText",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "efac233-8df7d-14df-d37d-72f72cb91"
    },
    "payload": {
      "text": "Let me know today's weather"
    }
  }
}

```
{% endraw %}

### See also

* [`SpeechRecognizer.Recognize`](#Recognize)
* [`SpeechRecognizer.StopCapture`](#StopCapture)

{% endif %}

## StopCapture directive {#StopCapture}
This directive returns the last recognition result and instructs the client to stop recording the user voice when CIC decides no more voice recording (PCM) is required upon receiving the [`SpeechRecognizer.Recognize`](#Recognize) event. Upon receiving the directive, the client must immediately stop recoding the user request. However, a client can choose to continue to receive a user request even after CIC sends this directive, but the voice request taken after receiving this directive will not be processed by Clova.

### Payload fields

{% if book.DocMeta.TargetReaderType == "Internal" or book.DocMeta.TargetReaderType == "Uplus" %}
| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `recognizedText` | string | The last recognition result of the user voice request. This field is included only under special conditions. | Conditional |
{% else %}
None
{% endif %}

### Remarks
This directive is sent through a [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection), not as a response to an event.

### Message example

{% if book.DocMeta.TargetReaderType == "Internal" or book.DocMeta.TargetReaderType == "Uplus" %}
```json
{
  "directive": {
    "header": {
      "namespace": "SpeechRecognizer",
      "name": "StopCapture",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "recognizedText": "Let me know today's weather"
    }
  }
}
```
{% else %}
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
{% endif %}

### See also
{% if book.DocMeta.TargetReaderType == "Internal" or book.DocMeta.TargetReaderType == "Uplus" %}
* [`SpeechRecognizer.Recognize`](#Recognize)
* [`SpeechRecognizer.ShowRecognizedText`](#ShowRecognizedText)
{% else %}
* [`SpeechRecognizer.Recognize`](#Recognize)
{% endif %}
