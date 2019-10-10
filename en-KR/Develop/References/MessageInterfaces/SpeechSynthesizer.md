<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

# SpeechSynthesizer

<!-- Start of the shared content: CICAPIforAudioPlayback -->
The SpeechSynthesizer namespace provides interfaces for requesting CIC to synthesize text into text-to-speech (TTS) or to provide TTS to the client. SpeechSynthesizer provides the following event messages and directive messages.

| Message name         | Type  | Description                                   |
|------------------|-----------|---------------------------------------------|
| [`Request`](#Request)                 | Event     | Requests CIC to generate a specified text into TTS.                                               |
| [`Speak`](#Speak)                     | Directive | Instructs the client to play the synthesized TTS through the client speaker.                                        |
| [`SpeechFinished`](#SpeechFinished)   | Event     | Reports to CIC that the client has finished playing the TTS.                                 |
| [`SpeechStarted`](#SpeechStarted)     | Event     | Reports to CIC that the client has started playing the TTS.                                 |
| [`SpeechStopped`](#SpeechStopped)     | Event     | Reports to CIC that the client has stopped playing the TTS.                                 |

<!-- End of the shared content -->

## Request event {#Request}

Requests CIC to generate a specified text into TTS.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields
| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `lang`  | string | The language used for speech synthesis. <ul><li><code>"en"</code>: English</li><li><code>"ja"</code>: Japanese</li><li><code>"ko"</code>: Korean</li><li><code>"zh"</code>: Chinese</li></ul> | Required    |
| `text`  | string | The text to request for TTS generation.           | Required    |

### Message example
{% raw %}
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
      "namespace": "SpeechSynthesizer",
      "name": "Request",
      "messageId": "ab63d4cb-49f0-4a92-94fc-5ee356193551"
    },
    "payload": {
      "text": "Make me an audio file",
      "lang": "ko"
    }
  }
}
```
{% endraw %}

### See also
* [`SpeechSynthesizer.Speak`](#Speak)

## Speak directive {#Speak}
Instructs the client to play the synthesized TTS through the client speaker. CIC can return multiple `SpeechSynthesizer.Speak` directive messages as a response to a single request. As such, the client must play TTS in the same order it has received messages. TTS can be returned either in a [multipart message](/Develop/References/CIC_API.md#MultipartMessage) or an audio streaming address.

### Payload fields
| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `contentType`          | string  | The MIME type of the TTS audio file to play. This field is provided when the TTS audio file to play is in the HLS protocol, and has the `"application/vnd.apple.mpegurl"` value. | Conditional  |
| `format`               | string  | The format of the TTS audio file. The value is always `"AUDIO_MPEG"`. | Always    |
| `token`                | string  | The token value to recognize the TTS audio file.<div class="note"><p><strong>Note!</strong></p><p>The maximum length of this field is 2048 bytes.</p></div>                    | Always    |
| `ttsLang`              | string  | The language used for TTS synthesis. <ul><li><code>"en"</code>: English</li><li><code>"ja"</code>: Japanese</li><li><code>"ko"</code>: Korean</li><li><code>"zh"</code>: Chinese</li></ul> | Conditional    |
| `url`                  | string  | The URI of the TTS audio file to play.<div class="note"><p><strong>Note!</strong></p><p>The maximum length of this field is 2048 bytes.</p></div>                        | Always    |
| `x-clova-pause-before` | number  | The idle time before playing the TTS audio. The value is an integer and the unit is in milliseconds.        | Conditional    |

### Remarks

The `url` can be in either one of the formats below. Process the audio according to the specified format.

| Format | Description |
|---------|-------------------------------|
| `cid:{Content-ID}` Format | If the `url` value of the client is in a `cid:{Content-ID}` format, the synthesized voice is sent as multipart message. Play the audio data (binary type) that has the same `Content-ID` message header. Since the order of messages containing audio data are not guaranteed, output audio data based on the `Content-ID` of the directive messages received.|
| URI format | Play the audio stream in the provided `url`.  |

### Message example

{% raw %}
```
// cid:{Content-ID} format
// Play the audio data with the Content-ID: 22f2ca4e-3b08-4d33-b32a-7eb62a8c0369

--Boundary-Text
Content-Disposition: form-data; name="speakDirective1"
Content-Type: application/json; charset=utf-8

{
  "directive": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "Speak",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524",
      "dialogRequestId": "caa7862a-3566-4aef-98de-489be0973e18"
    },
    "payload": {
      "format": "AUDIO_MPEG",
      "token": "cbba5103-8ce4-4e65-869b-f94d5878f579",
      "ttsLang": "ko",
      "url": "cid:22f2ca4e-3b08-4d33-b32a-7eb62a8c0369",
      "x-clova-pause-before": 0
    }
  }
}

--Boundary-Text
Content-Disposition: form-data; name="attachment"
Content-ID: 22f2ca4e-3b08-4d33-b32a-7eb62a8c0369
Content-Type: application/octet-stream

{ Audio Attachment }
--Boundary-Text--


// URI format
--Boundary-Text
{
  "directive": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "Speak",
      "messageId": "0313b471-ad7f-4cdd-b4e1-c046ca8b4b58",
      "dialogRequestId": "efa43b14-67f4-4f00-86bc-dfa08a08ad0b"
    },
    "payload": {
      "format": "AUDIO_MPEG",
      "token": "64ffeb07-4b86-4659-9f59-07a77b363a0b",
      "url": "https://example.net/clova/clova_song/1.mp3"
    }
  }
}

// URL format (HLS audio)
--Boundary-Text
{
  "directive": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "Speak",
      "messageId": "28950dbe-a5a3-4171-9307-0b81d5530a7f",
      "dialogRequestId": "8dce05a4-83ec-42f4-ac6c-a6660c192b96"
    },
    "payload": {
      "contentType": "application/vnd.apple.mpegurl",
      "format": "AUDIO_MPEG",
      "token": "ae97dc8c-859e-4cbb-9884-d32c94029835",
      "url": "https://example.net/clova/clova_song/1.m3u8"
    }
  }
}
--Boundary-Text--
```

{% endraw %}

### See also
* [`SpeechSynthesizer.Request`](#Request)
* [`SpeechSynthesizer.SpeechFinished`](#SpeechFinished)
* [`SpeechSynthesizer.SpeechStarted`](#SpeechStarted)
* [`SpeechSynthesizer.SpeechStopped`](#SpeechStopped)

<!-- Start of the shared content: SpeechSynthesizer.SpeechFinished -->

## SpeechFinished event {#SpeechFinished}
Reports to CIC that the client has finished playing the TTS.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `token`       | string  | The token value for TTS identification received from the [`SpeechSynthesizer.Speak`](#Speak) directive message. | Always    |

### Message example
{% raw %}

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
      "namespace": "SpeechSynthesizer",
      "name": "SpeechFinished",
      "messageId": "15472673-49a0-4aa1-8cf0-6355669ea473"
    },
    "payload": {
      "token": "cd14ad7a-9611-4b55-8ff5-c9097265950a"
    }
  }
}
```

{% endraw %}

### See also
* [`SpeechSynthesizer.Speak`](#Speak)
* [`SpeechSynthesizer.SpeechStarted`](#SpeechStarted)
* [`SpeechSynthesizer.SpeechStopped`](#SpeechStopped)

<!-- End of the shared content -->

<!-- Start of the shared content: SpeechSynthesizer.SpeechStarted -->

## SpeechStarted event {#SpeechStarted}
Reports to CIC that the client has started playing the TTS.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `token`       | string  | The token value for TTS identification received from the [`SpeechSynthesizer.Speak`](#Speak) directive message. | Always    |

### Message example
{% raw %}

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
      "namespace": "SpeechSynthesizer",
      "name": "SpeechStarted",
      "messageId": "380c805c-0f19-4ed2-84e2-056f2f4016de"
    },
    "payload": {
      "token": "cd14ad7a-9611-4b55-8ff5-c9097265950a"
    }
  }
}
```

{% endraw %}

### See also
* [`SpeechSynthesizer.Speak`](#Speak)
* [`SpeechSynthesizer.SpeechFinished`](#SpeechFinished)
* [`SpeechSynthesizer.SpeechStopped`](#SpeechStopped)

<!-- End of the shared content -->

<!-- Start of the shared content: SpeechSynthesizer.SpeechStopped -->

## SpeechStopped event {#SpeechStopped}
Reports to CIC that the client has stopped playing the TTS.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `token`       | string  | The token value for TTS identification received from the [`SpeechSynthesizer.Speak`](#Speak) directive message. | Always    |

### Message example
{% raw %}

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
      "namespace": "SpeechSynthesizer",
      "name": "SpeechStopped",
      "messageId": "9a511e5c-4f20-413a-94cc-48172fc8710e"
    },
    "payload": {
      "token": "cd14ad7a-9611-4b55-8ff5-c9097265950a"
    }
  }
}
```

{% endraw %}

### See also
* [`SpeechSynthesizer.Speak`](#Speak)
* [`SpeechSynthesizer.SpeechFinished`](#SpeechFinished)
* [`SpeechSynthesizer.SpeechStarted`](#SpeechStarted)

<!-- End of the shared content -->
