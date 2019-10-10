<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

# AudioPlayer

<!-- Start of the shared content: CICAPIforAudioPlayback -->
The AudioPlayer namespace provides interfaces for playing an audio stream or reporting to CIC about event messages that occur during playback. The AudioPlayer namespace provides the following directive messages and event messages.

| Message name         | Type  | Description                                   |
|------------------|-----------|---------------------------------------------|
| [`ClearQueue`](#ClearQueue)           | Directive | Instructs the client to initialize the playback queue of the audio stream.                              |
| [`ExpectReportPlaybackState`](#ExpectReportPlaybackState) | Directive | Instructs the client to report the current playback state. Upon receiving the directive message, the client must send the [`AudioPlayer.ReportPlaybackState`](#ReportPlaybackState) event message to CIC. |
| [`Play`](#Play)                       | Directive | Instructs the client to either play or add to the playback queue the specified audio stream.                         |
| [`PlaybackQueueCleared`](#PlaybackQueueCleared) | Event   | If the [`AudioPlayer.ClearQueue`](#ClearQueue) directive message is received from CIC, the client must send the `PlaybackQueueCleared` event message after initializing the playback queue.       |
| [`PlayFinished`](#PlayFinished)       | Event     | Reports to CIC that the client has finished playback with the information on the audio stream.     |
| [`PlayPaused`](#PlayPaused)           | Event     | Reports to CIC that the client has paused playback with the information on the audio stream. |
| [`PlayResumed`](#PlayResumed)         | Event     | Reports to CIC that the client has resumed playback with the information on the audio stream.         |
| [`PlayStarted`](#PlayStarted)         | Event     | Reports to CIC that the client has started playback with the information on the audio stream.    |
| [`PlayStopped`](#PlayStopped)         | Event     | Reports to CIC that the client has stopped playback with the information on the audio stream.    |
| [`ProgressReportDelayPassed`](#ProgressReportPositionPassed) | Event | Reports to CIC the current playback state ([`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)) after playback has started and the specified delay has passed. The delay time for each audio stream is specified in the [`AudioPlayer.Play`](#Play) directive message to the client. |
| [`ProgressReportIntervalPassed`](#ProgressReportPositionPassed)| Event | Reports to CIC the current playback state ([`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)), by the specified interval, after playback has started. The report interval for each audio stream is specified in the [`AudioPlayer.Play`](#Play) directive message to the client.|
| [`ProgressReportPositionPassed`](#ProgressReportPositionPassed) | Event | Reports to CIC the current playback state ([`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)) at the specified time, which is measured from the start of the audio stream. The reporting time is specified in the [`AudioPlayer.Play`](#Play) directive message.|
| [`ReportPlaybackState`](#ReportPlaybackState)           | Event  | Reports to CIC the current playback state of the client. If the [`AudioPlayer.ExpectReportPlaybackState`](#ExpectReportPlaybackState) directive message is received from CIC, the client must send the `AudioPlayer.ReportPlaybackState` event messages to CIC.  |
{% if book.DocMeta.TargetReaderType == "Internal" -%}
| [`RequestPlaybackState`](#RequestPlaybackState)         | Event  | Requests CIC for the current playback state of the client. Upon receiving the `AudioPlayer.RequestPlaybackState` event message, CIC will send the [`ExpectReportPlaybackState`](#ExpectReportPlaybackState) directive message to all or specific clients registered to the user account.  |
| [`StreamDeliver`](#StreamDeliver)     | Directive | Receives the audio stream information that can be played as a response to the [`AudioPlayer.StreamRequested`](#StreamRequested) event message. |
{% endif -%}
| [`StreamRequested`](#StreamRequested) | Event     | Requests CIC for additional information needed for audio stream playback such as a streaming URL.               |
{% if book.DocMeta.TargetReaderType == "Internal" -%}
| [`SynchronizePlaybackState`](#SynchronizePlaybackState) | Directive | Instructs the client to synchronize the audio playback state. The client that had sent the `AudioPlayer.RequestPlaybackState` event message will receive the `AudioPlayer.SynchronizePlaybackState` directive message. |
{% endif -%}

<!-- End of the shared content -->

## ClearQueue directive {#ClearQueue}
Instructs the client to initialize the playback queue of the audio stream. The `clearBehavior` field value of this directive message distinguishes the reset action and the client determines whether or not to stop the currently playing audio stream by resetting the playback queue.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `clearBehavior`           | string | The type of initialization.<ul><li><code>"CLEAR_ALL"</code>: Clear the playback queue and stop playing the audio stream.</li><li><code>"CLEAR_ENQUEUED"</code>: Clear the playback queue, but continue playing the audio stream.</li></ul> | Always |

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "ClearQueue",
      "dialogRequestId": "8b81296d-218e-4a08-897a-bee51daad907",
      "messageId": "823a703d-9447-438a-bad5-21fa7a62b623"
    },
    "payload": {
      "clearBehavior": "CLEAR_ALL"
    }
  }
}
```

{% endraw %}

### See also
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlaybackQueueCleared`](#PlaybackQueueCleared)
* [`AudioPlayer.PlayStarted`](#PlayStarted)
* [`AudioPlayer.PlayStopped`](#PlayStopped)

## ExpectReportPlaybackState directive {#ExpectReportPlaybackState}

Instructs the client to report the current playback state. Upon receiving the directive message, the client must send the [`AudioPlayer.ReportPlaybackState`](#ReportPlaybackState) event message to CIC.

### Payload fields

None

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "ExpectReportPlaybackState",
      "dialogRequestId": "9d7dc3ca-17ff-4df9-9800-91736ba2a3b6",
      "messageId": "46ccdf6c-609a-43a5-91c4-7a43b961f0c0"
    },
    "payload": {}
  }
}
```

{% endraw %}

### See also
* [`AudioPlayer.ReportPlaybackState`](#ReportPlaybackState)
* [Sharing audio playback state](/Develop/Guides/Handle_Audio_Playback.md#ShareAudioPlaybackState)

<!-- Start of the shared content: AudioPlayer.Play -->

## Play directive {#Play}
Instructs the client to either play or add to the playback queue the specified audio stream.

### Payload fields
| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `audioItem`               | object | The metadata of an audio stream to play and audio stream information required for playback.                     | Always |
| `audioItem.artImageUrl`   | string | The URI for the image on the audio (e.g. album image).                                                  | Conditional  |
| `audioItem.audioItemId`   | string | ID of an audio stream. Use this ID to remove redundant Play directive messages. | Always |
| `audioItem.headerText`    | string | The text field used mainly to indicate the title of current play list.                                                | Conditional  |
| `audioItem.stream`        | [AudioStreamInfoObject](#AudioStreamInfoObject) | The audio stream information required for playback.        | Always |
| `audioItem.titleSubText1` | string | The text field used mainly to indicate the name of the artist.                                                          | Always |
| `audioItem.titleSubText2` | string | The text field used mainly to indicate the name of the album.                                                      | Conditional |
| `audioItem.titleText`     | string | The text field used mainly to indicate the title of the currently playing music.                                                         | Always  |
| `audioItem.type`          | string | **(Deprecated)** The name of the music service. This name can be the name of the streaming service provider or the service name. Use this value to determine which fields of the audioItem object are used for different service providers and select an appropriate parser to analyze them. | Always |
| `playBehavior`            | string | Indicates when to play the audio stream received from the directive message. <ul><li><code>"REPLACE_ALL"</code>: Clear the playback queue and play the audio stream immediately.</li><li><code>"ENQUEUE"</code>: Clear the playback queue and add the audio stream.</li></ul> | Always |
| `source`                  | object | The information of the audio streaming service.                                                    | Always |
| `source.logoUrl`          | string | The URI for the logo of the audio streaming service. If this field is unavailable or undefined, or if the logo cannot be displayed, you must at least display the name of the music service provider specified in the `source.name` field.  | Conditional |
| `source.name`             | string | The text field containing the name of the audio streaming service.                                                        | Always |

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>Using `source.logoUrl` over `source.name` is recommended for a more intuitive user experience.</p>
</div>

### Remarks
Based on the policy of music service providers, certain information required for playback (e.g. streaming URI) may have to be acquired right before playback. Whether additional information will be requested is specified in the `audioItem.stream.urlPlayable` field, as shown below:
* `urlPlayable` is `true`: No additional information is required. You can play the audio stream only with the URI specified in the `audioItem.stream.url` field.
* `urlPlayable` is `false`: Additional information is required for you to play the given audio stream with the URI specified in the `audioItem.stream.url` field. Make a request for additional information with the [`AudioPlayer.StreamRequested`](#StreamRequested) event message.

### Message example
{% raw %}

```json
// Example: The audio stream that can be played only with the stream URI
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "Play",
      "dialogRequestId": "34abac3-cb46-611c-5111-47eab87b7",
      "messageId": "ad13f0d6-bb11-ca23-99aa-312a0b213805"
    },
    "payload": {
      "audioItem": {
        "audioItemId": "90b77646-93ab-444f-acd9-60f9f278ca38",
        "episodeId": 22346122,
        "stream": {
          "beginAtInMilliseconds": 419704,
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": 60000,
            "progressReportPositionInMilliseconds": null
          },
          "token": "eyJ1cmwiOiJodHRwczovL2FwaS1leC5wb2RiYmFuZy5jb20vY2xvdmEvZmlsZS8xMjU0OC8yMjYxODcwMSIsInBsYXlUeXBlIjoiTk9ORSIsInBvZGNhc3RJZCI6MTI1NDgsImVwaXNvZGVJZCI6MjI2MTg3MDF9",
          "url": "https://streaming.example.com/clova/file/12548/22618701",
          "urlPlayable": true
        },
        "type": "podcast"
      },
      "source": {
        "name": "Potbbang",
        "logoUrl": "https://img.musicservice.example.net/logo_180125.png"
      },
      "playBehavior": "REPLACE_ALL"
    }
  }
}

// An example of an audio stream that cannot be played only with the stream URI
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "Play",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "audioItem": {
        "audioItemId": "9CPWU-8362fe7c-f75c-42c6-806b-6f3e00aba8f1-c1862201",
        "album": {
          "albumId": "2000240",
          "genres": [
            "Classic"
          ],
          "title": "Wonderland - Edvard Grieg : Piano Concerto, Lyric Pieces"
        },
        ...
        "stream": {
          "beginAtInMilliseconds": 0,
          "durationInMilliseconds": 60000,
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": null,
            "progressReportPositionInMilliseconds": 60000
          },
          "token": "TR-NM-17716562",
          "url": "clova:TR-NM-17716562",
          "urlPlayable": false
        },
        "title": "Symphony No.4 In A Op.90 'Italian' - III. Con Moto Moderato",
        "type": "SampleMusicProvider"
      },
      "source": {
        "name": "Sample Music Provider",
        "logoUrl": "https://img.musicservice.example.net/logo_180125.png"
      },
      "playBehavior": "REPLACE_ALL"
    }
  }
}
```

{% endraw %}

### See also
* [`AudioPlayer.PlayPaused`](#PlayPaused)
* [`AudioPlayer.PlayResumed`](#PlayResumed)
* [`AudioPlayer.PlayStarted`](#PlayStarted)
* [`AudioPlayer.PlayStopped`](#PlayStopped)
* [`AudioPlayer.ProgressReportDelayPassed`](#ProgressReportDelayPassed)
* [`AudioPlayer.ProgressReportIntervalPassed`](#ProgressReportIntervalPassed)
* [`AudioPlayer.ProgressReportPositionPassed`](#ProgressReportPositionPassed)
* [`AudioPlayer.StreamRequested`](#StreamRequested)
* [Playing audio stream](/Develop/Guides/Handle_Audio_Playback.md#PlayAudioStream)

<!-- End of the shared content -->

## PlaybackQueueCleared event {#PlaybackQueueCleared}
If the [`AudioPlayer.ClearQueue`](#ClearQueue) directive message is received from CIC, the client must send the `PlaybackQueueCleared` event message after initializing the playback queue.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `clearBehavior` | string | The `clearBehavior` field value of the [`AudioPlayer.ClearQueue`](#ClearQueue) directive message. The field value determines the initialization action, and must be filled out and sent once the corresponding action is processed.<ul><li><code>"CLEAR_ALL"</code>: Initialize the playback queue and stop playing audio stream.</li><li><code>"CLEAR_ENQUEUED"</code>: Initialize the playback queue only and continue playing the audio stream.</li></ul> | Required |


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
      "namespace": "AudioPlayer",
      "name": "PlaybackQueueCleared",
      "messageId": "1e1d8d52-9b51-454c-9fac-7597dc5f5246"
    },
    "payload": {
        "clearBehavior": "CLEAR_ALL"
    }
}
}
```

{% endraw %}

### See also
* [`AudioPlayer.ClearQueue`](#ClearQueue)

<!-- Start of the shared content: AudioPlayer.PlayFinished -->

## PlayFinished event {#PlayFinished}
Reports to CIC that the client has finished playback with the information on the audio stream.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | The current-time indicator of the audio playing (in milliseconds).                         | Required  |
| `token`                | string | The `audioItem.stream.token` field value of the [`AudioPlayer.Play`](#Play) directive message. | Required |

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
      "namespace": "AudioPlayer",
      "name": "PlayFinished",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 183000
    }
  }
}
```

{% endraw %}

### See also
* [`AudioPlayer.Play`](#Play)
* [Reporting audio playback progress](/Develop/Guides/Handle_Audio_Playback.md#ReportAudioPlaybackProgress)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlayPaused -->

## PlayPaused event {#PlayPaused}
Reports to CIC that the client has paused playback with the information on the audio stream. Send this event message in the following scenario:

1. The client sends CIC a user voice request ([`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)) to pause playback of an audio stream.
2. CIC sends the playback pause request to the client with the [`PlaybackController.Pause`](/Develop/References/MessageInterfaces/PlaybackController.md#Pause) directive message, as the result of voice recognition by the Clova platform.
3. The client pauses playback of the audio stream and sends the PlayPaused event message to CIC.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | The current-time indicator of the audio playing (in milliseconds).                         | Required  |
| `token`                | string | The `audioItem.stream.token` field value of the [`AudioPlayer.Play`](#Play) directive message. | Required |

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
      "namespace": "AudioPlayer",
      "name": "PlayPaused",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 83100
    }
  }
}
```

{% endraw %}

### See also
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayResumed`](#PlayResumed)
* [`PlaybackController.Pause`](/Develop/References/MessageInterfaces/PlaybackController.md#Pause)
* [Controlling audio playback](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlayResumed -->

## PlayResumed event {#PlayResumed}

Reports to CIC that the client has resumed playback with the information on the audio stream. Send this event message in the following scenario:

1. The client sends CIC a user voice request ([`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)) to resume the paused audio stream.
2. CIC sends the resume request to the client with the [`PlaybackController.Resume`](/Develop/References/MessageInterfaces/PlaybackController.md#Resume) directive message, as the result of voice recognition by the Clova platform.
3. The client resumes playback of the audio stream and sends the PlayResumed event message to CIC.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | The current-time indicator of the audio playing (in milliseconds).                         | Required  |
| `token`                | string | The `audioItem.stream.token` field value of the [`AudioPlayer.Play`](#Play) directive message. | Required |

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
      "namespace": "AudioPlayer",
      "name": "PlayResumed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 83100
    }
  }
}
```

{% endraw %}

### See also
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayPaused`](#PlayPaused)
* [`PlaybackController.Resume`](/Develop/References/MessageInterfaces/PlaybackController.md#Resume)
* [Controlling audio playback](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlayStarted -->

## PlayStarted event {#PlayStarted}
Reports to CIC that the client has started playback with the information on the audio stream.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | The current-time indicator of the audio playing (in milliseconds).                         | Required  |
| `token`                | string | The `audioItem.stream.token` field value of the [`AudioPlayer.Play`](#Play) directive message. | Required |

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
      "namespace": "AudioPlayer",
      "name": "PlayStarted",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 0
    }
  }
}
```

{% endraw %}

### See also
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayStopped`](#PlayStopped)
* [Playing audio stream](/Develop/Guides/Handle_Audio_Playback.md#PlayAudioStream)
* [Controlling audio playback](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlayStopped -->

## PlayStopped event {#PlayStopped}
Reports to CIC that the client has stopped playback with the information on the audio stream. Send this event message in the following scenario:

1. The client sends CIC a user voice request with the [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize) event message to stop playback of an audio stream.
2. CIC sends the stop request to the client with the [`PlaybackController.Stop`](/Develop/References/MessageInterfaces/PlaybackController.md#Stop) directive message, as the result of voice recognition by the Clova platform.
3. The client stops playback of the audio stream and sends the PlayStopped event message to CIC.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | The current-time indicator of the audio playing (in milliseconds).                         | Required  |
| `token`                | string | The `audioItem.stream.token` field value of the [`AudioPlayer.Play`](#Play) directive message. | Required |

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
      "namespace": "AudioPlayer",
      "name": "PlayStopped",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 83100
    }
  }
}
```

{% endraw %}

### See also
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayStarted`](#PlayStarted)
* [`PlaybackController.Stop`](/Develop/References/MessageInterfaces/PlaybackController.md#Stop)
* [Controlling audio playback](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.ProgressReportDelayPassed -->

## ProgressReportDelayPassed event {#ProgressReportDelayPassed}
Reports to CIC the current playback state ([`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)) after playback has started and the specified period of delay time has passed. The delay time for each audio stream is specified in the [`AudioPlayer.Play`](#Play) directive message to the client.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | The current-time indicator of the audio playing (in milliseconds).                         | Required  |
| `token`                | string | The `audioItem.stream.token` field value of the [`AudioPlayer.Play`](#Play) directive message. | Required |

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
      "namespace": "AudioPlayer",
      "name": "ProgressReportDelayPassed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 60000
    }
  }
}
```

{% endraw %}

### See also
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.ProgressReportIntervalPassed`](#ProgressReportIntervalPassed)
* [`AudioPlayer.ProgressReportPositionPassed`](#ProgressReportPositionPassed)
* [Reporting audio playback progress](/Develop/Guides/Handle_Audio_Playback.md#ReportAudioPlaybackProgress)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.ProgressReportIntervalPassed -->

## ProgressReportIntervalPassed event {#ProgressReportIntervalPassed}
Reports to CIC the current playback state ([`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)), by the specified interval, after playback has started. The report interval for each audio stream is specified in the [`AudioPlayer.Play`](#Play) directive message to the client.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | The current-time indicator of the audio playing (in milliseconds).                         | Required  |
| `token`                | string | The `audioItem.stream.token` field value of the [`AudioPlayer.Play`](#Play) directive message. | Required |

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
      "namespace": "AudioPlayer",
      "name": "ProgressReportIntervalPassed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 120000
    }
  }
}
```

{% endraw %}

### See also
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.ProgressReportDelayPassed`](#ProgressReportDelayPassed)
* [`AudioPlayer.ProgressReportPositionPassed`](#ProgressReportPositionPassed)
* [Reporting audio playback progress](/Develop/Guides/Handle_Audio_Playback.md#ReportAudioPlaybackProgress)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.ProgressReportPositionPassed -->

## ProgressReportPositionPassed event {#ProgressReportPositionPassed}
Reports to CIC the current playback state ([`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)) at the specified time, which is measured from the start of the audio stream. The reporting time is specified in the [`AudioPlayer.Play`](#Play) directive message.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | The current-time indicator of the audio playing (in milliseconds).                         | Required  |
| `token`                | string | The `audioItem.stream.token` field value of the [`AudioPlayer.Play`](#Play) directive message. | Required |

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
      "namespace": "AudioPlayer",
      "name": "ProgressReportPositionPassed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 150000
    }
  }
}
```

{% endraw %}

### See also
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.ProgressReportDelayPassed`](#ProgressReportDelayPassed)
* [`AudioPlayer.ProgressReportIntervalPassed`](#ProgressReportIntervalPassed)
* [Reporting audio playback progress](/Develop/Guides/Handle_Audio_Playback.md#ReportAudioPlaybackProgress)

<!-- End of the shared content -->

## ReportPlaybackState event {#ReportPlaybackState}

Reports to CIC the current playback state of the client. If the [`AudioPlayer.ExpectReportPlaybackState`](#ExpectReportPlaybackState) directive message is received from CIC, the client must send the `AudioPlayer.ReportPlaybackState` event messages to CIC.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields
| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds`   | number  | The current-time indicator of the audio playing (in milliseconds).  | Optional  |
| `playerActivity`         | string  | The current state of audio player.<ul><li><code>"IDLE"</code>: Not in use</li><li><code>"PLAYING"</code>: Playing</li><li><code>"PAUSED"</code>: Paused</li><li><code>"STOPPED"</code>: Stopped</li></ul>  | Required  |
| `repeatMode`             | string  | The repeat mode.<ul><li><code>"NONE"</code>: None</li><li><code>"REPEAT_ONE"</code>: Repeat one song</li></ul>  | Required  |
| `stream`                 | [AudioStreamInfoObject](#AudioStreamInfoObject) | The `audioItem.stream` of the Play directive message.                                     | Optional |
| `token`                  | string  | The `audioItem.stream.token` field value of the [`AudioPlayer.Play`](#Play) directive message.                                          | Optional |
| `totalInMilliseconds`    | number | The total duration of the recently played media. If the audio information ([AudioStreamInfoObject](/Develop/References/MessageInterfaces/AudioPlayer.md#AudioStreamInfoObject)) provided by the [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) directive message has a `durationInMilliseconds` field value, enter it as the value of this field. The unit is in milliseconds. This field is omissible if the `playerActivity` field is set to `"IDLE"`. | Optional |

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
      "namespace": "AudioPlayer",
      "name": "ReportPlaybackState",
      "messageId": "3e57f11a-a02d-4b6f-b183-fdfb6105c650"
    },
    "payload": {
      "playerActivity": "IDLE",
      "repeatMode": "NONE"
    }
  }
}
```

{% endraw %}

### See also
* [`AudioPlayer.ExpectReportPlaybackState`](#ExpectReportPlaybackState)
* [`AudioPlayer.Play`](#Play)
* [Sharing audio playback state](/Develop/Guides/Handle_Audio_Playback.md#ShareAudioPlaybackState)

{% if book.DocMeta.TargetReaderType == "Internal" %}
## RequestPlaybackState event {#RequestPlaybackState}

Requests CIC for the current playback state of the client. Upon receiving the `AudioPlayer.RequestPlaybackState` event message, CIC will send the [`AudioPlayer.ExpectReportPlaybackState`](#ExpectReportPlaybackState) directive message to all or specific clients registered to the user account.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields
| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | ID of the target client. The opaque ID in a random format. If this field is omitted, the request is broadcasted to all clients registered in the user account. | Optional |

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
      "namespace": "AudioPlayer",
      "name": "RequestPlaybackState",
      "messageId": "15323cb4-859f-4d5e-b7f4-a7b60ff41866"
    },
    "payload": {
      "deviceId": "5a849a66-c182-4c1b-8a97-b63cac2d396c"
    }
  }
}
```

{% endraw %}

### See also
* [`AudioPlayer.ExpectReportPlaybackState`](#ExpectReportPlaybackState)
* [`AudioPlayer.Play`](#Play)
* [Sharing audio playback state](/Develop/Guides/Handle_Audio_Playback.md#ShareAudioPlaybackState)
{% endif %}

<!-- Start of the shared content: AudioPlayer.StreamDeliver -->

## StreamDeliver directive {#StreamDeliver}
Receives the audio stream information that can be played as a response to the [`AudioPlayer.StreamRequested`](#StreamRequested) event message. The directive message contains the URI for audio streaming as mandatory information so the client can play the audio.

### Payload fields
| Field name | Data type | Description | Included |
|---------|------|--------|:---------:|
| `audioItemId` | string | ID of the audio stream. Use this ID to remove redundant Play directive messages. | Always |
| `audioStream` | [AudioStreamInfoObject](#AudioStreamInfoObject) | The audio stream information required for playback.       | Always |

### Remarks
Some contents of the `AudioStreamInfoObject` object provided by the `StreamDeliver` directive message may be omitted to avoid overlapping of information with the `AudioStreamInfoObject` object provided by the [`AudioPlayer.Play`](#Play) directive message. Therefore, you must combine the `payload.audioStream` information of the `StreamDeliver` directive message and the received [`Play`](#Play) directive message when playing the audio.

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "StreamDeliver",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "audioItemId": "9CPWU-8362fe7c-f75c-42c6-806b-6f3e00aba8f1-c1862201",
      "audioStream": {
          "format": "audio/mpeg",
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": null,
            "progressReportPositionInMilliseconds": 60000
          },
          "token": "TR-NM-17716562",
          "url": "https://musicservice.example.net/b767313e.mp3",
          "urlPlayable": true
      }
    }
  }
}
```

{% endraw %}

### See also
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.StreamRequested`](#StreamRequested)
* [Playing audio stream](/Develop/Guides/Handle_Audio_Playback.md#PlayAudioStream)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.StreamRequested -->

## StreamRequested event {#StreamRequested}
Requests CIC for additional information needed for audio stream playback such as a streaming URI.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields
| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `audioItemId`   | string  | The `audioItem.audioItemId` field value of the [`AudioPlayer.Play`](#Play) directive message.       | Required |
| `audioStream`   | [AudioStreamInfoObject](#AudioStreamInfoObject) | The `audioItem.stream` of the Play directive message. | Required |

### Remarks
Based on the policy of music service providers, certain information required for playback (e.g. streaming URL) may not be shared to clients until right before playback. This event message is an API designed for such situations where you cannot get stream information in advance. Do not send this event message any earlier than right before playing an audio stream.

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
      "namespace": "AudioPlayer",
      "name": "StreamRequested",
      "messageId": "198cf12-4020-b98a-b73b-1234ab312806"
    },
    "payload": {
      "audioItemId": "9CPWU-8362fe7c-f75c-42c6-806b-6f3e00aba8f1-c1862201",
      "audioStream": {
        "beginAtInMilliseconds": 0,
        "durationInMilliseconds": 60000,
        "progressReport": {
          "progressReportDelayInMilliseconds": null,
          "progressReportIntervalInMilliseconds": null,
          "progressReportPositionInMilliseconds": 60000
        },
        "token": "TR-NM-17716562",
        "url": "clova:TR-NM-17716562",
        "urlPlayable": false
      }
    }
  }
}
```

{% endraw %}

### See also
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.StreamDeliver`](#StreamDeliver)
* [Playing audio stream](/Develop/Guides/Handle_Audio_Playback.md#PlayAudioStream)

<!-- End of the shared content -->

{% if book.DocMeta.TargetReaderType == "Internal" %}
## SynchronizePlaybackState directive {#SynchronizePlaybackState}

Instructs the client to synchronize the audio playback state. The client that had sent the `AudioPlayer.RequestPlaybackState` event message will receive the `AudioPlayer.SynchronizePlaybackState` directive message.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | ID of the target client. The opaque ID in a random format.  | Always  |
| `event`       | string  | The event occurred in the client.<ul><li><code>PlayFinished</code></li><li><code>PlayPaused</code></li><li><code>PlayResumed</code></li><li><code>PlayStarted</code></li><li><code>PlayStopped</code></li></ul>  | Conditional  |
| `playbackState`          | object  | The playback state of the client.             | Conditional  |
| `playbackState.offsetInMilliseconds`   | number  | The current-time indicator of the audio playing The unit is milliseconds. This field is not included if the `playerActivity` value is `"IDLE"`.  | Conditional  |
| `playbackState.playerActivity`         | string  | The current state of audio player.<ul><li><code>"IDLE"</code>: Not in use</li><li><code>"PLAYING"</code>: Playing</li><li><code>"PAUSED"</code>: Paused</li><li><code>"STOPPED"</code>: Stopped</li></ul>  | Always  |
| `playbackState.repeatMode`             | string  | The repeat mode.<ul><li><code>"NONE"</code>: None</li><li><code>"REPEAT_ONE"</code>: Repeat one song</li></ul>  | Always  |
| `playbackState.stream`                 | [AudioStreamInfoObject](#AudioStreamInfoObject) | The information on the audio stream. This field is not included if the `playerActivity` value is `"IDLE"`.  | Conditional |
| `playbackState.token`                  | string  | The token to identify the audio playback. This field is not included if the `playerActivity` value is `"IDLE"`.                                | Conditional |
| `playbackState.totalInMilliseconds`    | number | The total duration of the recently played media. The unit is milliseconds. This field is not included if the `playerActivity` value is `"IDLE"`.                  | Conditional |

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "SynchronizePlaybackState",
      "dialogRequestId": "9d7dc3ca-17ff-4df9-9800-91736ba2a3b6",
      "messageId": "46ccdf6c-609a-43a5-91c4-7a43b961f0c0"
    },
    "payload": {
      "deviceId": "2fe8f3e8-85be-4a45-863e-beec7314ba2e",
      "playbackState": {
        "playerActivity": "IDLE",
        "repeatMode": "NONE"
      }
    }
  }
}
```

{% endraw %}

### See also
* [`AudioPlayer.ReportPlaybackState`](#ReportPlaybackState)
* [Sharing audio playback state](/Develop/Guides/Handle_Audio_Playback.md#ShareAudioPlaybackState)
{% endif %}

## Shared objects
Common objects are included in the message body (`payload`) of event messages or directive messages when they are sent with the AudioPlayer API.

| Object name            | Description                                            |
|--------------------|---------------------------------------------------|
| [AudioStreamInfoObject](#AudioStreamInfoObject) | The playback details of an audio stream such as a streaming address and credentials. |

### AudioStreamInfoObject {#AudioStreamInfoObject}
The object containing streaming details of an audio stream. This object is used when CIC returns streaming details to a client or when the client sends the details of the currently playing audio stream to CIC.

#### Object fields
| Field name       | Data type    | Description                     | Required/Included |
|---------------|---------|-----------------------------|:-------------:|
| `beginAtInMilliseconds`  | number | The playback start point. The unit is in milliseconds. If this field is specified, play the audio stream from the specified point of the stream. If set to 0, play the audio stream from the beginning.          | Required/Always |
| `customData`             | string | The metadata on the current audio in a random format. Any streaming information that cannot be classified into a specific category or defined must be included or entered in this field. Additionally required custom values can be added arbitrarily by the streaming service providers to the context of audio stream playback.<div class="warning"><p><strong>Warning!</strong></p><p>Clients must not arbitrarily use the value of this field, as it can cause problems. Also, make sure to enter this field value in the <code>stream</code> field of the <a href="/Develop/References/Context_Objects.md#PlaybackState">PlaybackState context</a> without any alterations, when reporting the playback state to CIC.</p></div> | Optional/Conditional  |
| `durationInMilliseconds` | number | The length of the audio stream. The client can play and navigate audio from the playback time specified in the `beginAtInMilliseconds` field by the amount of time defined in this field. For example, if the value of `beginAtInMilliseconds` field is `10000` and the value of this field is `60000`, you can play and navigate within 10-70 seconds of the audio stream. The unit is in milliseconds.   | Optional/Conditional  |
| `format`                 | string  | The media format (MIME type). This field can be used to identify whether the content uses the HTTP Live Streaming (HLS) protocol. Available values are: The default value is `"audio/mpeg"`.<ul><li><code>"audio/mpeg"</code></li><li><code>"audio/mpegurl"</code></li><li><code>"audio/aac"</code></li><li><code>"application/vnd.apple.mpegurl"</code></li></ul> <div class="note"><p><strong>Note!</strong></p><p>If you want to develop an extension that provides content using the HLS protocol, email <a href="mailto:{{ book.ServiceEnv.ExtensionAdminEmail }}">{{ book.ServiceEnv.ExtensionAdminEmail }}</a>.</p></div>   | Optional/Conditional  |
| `progressReport`         | object  | The time specified to receive the playback state after the audio starts.                                                  | Optional/Conditional |
| `progressReport.progressReportDelayInMilliseconds`    | number | The duration of time specified to receive the playback state after the audio starts. The unit is in milliseconds. This field can have a null value.  | Optional/Conditional |
| `progressReport.progressReportIntervalInMilliseconds` | number | The interval specified to receive the playback state while audio is playing. The unit is in milliseconds. This field can have a null value.        | Optional/Conditional |
| `progressReport.progressReportPositionInMilliseconds` | number | The point of time specified to receive the playback state to while audio is playing. The unit is in milliseconds. This field can have a null value.    | Optional/Conditional |
| `token`                  | string  | The token of the audio stream. <div class="note"><p><strong>Note!</strong></p><p>The maximum length of this field is 2048 bytes.</p></div>                          | Required/Always |
| `url`                    | string  | The audio stream URI.<div class="note"><p><strong>Note!</strong></p><p>The provided audio content must be in an <a href="/Design/Audio.md#SupportedAudioFormat">audio compression format supported by the platform</a>.</p></div><div class="note"><p><strong>Note!</strong></p><p>The maximum length of this field is 2048 bytes.</p></div>  | Required/Always |
| `urlPlayable`            | boolean | Indicates whether the audio stream URI in the `url` field can be played immediately. <ul><li><code>true</code>: The client can play the audio without additional information.</li><li><code>false</code>: The client requires additional information. To obtain additional information on audio streaming, send the <a href="#StreamRequested"><code>AudioPlayer.StreamRequested</code></a> event message to CIC.</li></ul>        | Required/Always |

#### Remarks
* Once the audio section designated in `beginAtInMilliseconds` and `durationInMilliseconds` fields finishes playing, the client must send the [`AudioPlayer.PlayFinished`](#PlayFinished) event message to CIC.
* The UI of a client must prevent users from navigating through the audio in times other than the section defined in `beginAtInMilliseconds` and `durationInMilliseconds` fields.
* When reporting the current playback state to CIC, set the `totalInMilliseconds` field value of [`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState) as the combined value in `beginAtInMilliseconds` and `durationInMilliseconds` fields.

#### Object Example
{% raw %}

```json
// An example of an audio stream that can be played only with the URI
{
  "beginAtInMilliseconds": 0,
  "progressReport": {
    "progressReportDelayInMilliseconds": null,
    "progressReportIntervalInMilliseconds": 60000,
    "progressReportPositionInMilliseconds": null
  },
  "url": "https://api-ex.example.com/file/12548/22346122",
  "urlPlayable": true
}

// An example of an audio stream that cannot be played only with the stream URI
{
  "beginAtInMilliseconds": 0,
  "progressReport": {
      "progressReportDelayInMilliseconds": null,
      "progressReportIntervalInMilliseconds": null,
      "progressReportPositionInMilliseconds": 60000
  },
  "token": "TR-NM-4435786",
  "urlPlayable": false,
  "url": "clova:TR-NM-4435786"
}
```

{% endraw %}

#### See also
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayFinished`](#PlayFinished)
* [`AudioPlayer.ProgressReportDelayPassed`](#ProgressReportDelayPassed)
* [`AudioPlayer.ProgressReportIntervalPassed`](#ProgressReportIntervalPassed)
* [`AudioPlayer.ProgressReportPositionPassed`](#ProgressReportPositionPassed)
* [`AudioPlayer.StreamRequested`](#StreamRequested)
