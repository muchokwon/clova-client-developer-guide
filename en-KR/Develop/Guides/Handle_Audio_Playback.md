# Handling audio playback

Based on user request, Clova plays audio or sends directive messages related to playback to the client. The client must handle the messages related to audio playback sent from Clova and return the audio playback state of the client to CIC using event messages. It is particularly important to have Clova understand the playback state of the client by sending event messages to CIC, and this reporting process must be done using event messages that are suitable to the situation or conditions. This section explains the following:

* [Playing audio stream](#PlayAudioStream)
* [Reporting audio playback progress](#ReportAudioPlaybackProgress)
* [Controlling audio playback](#ControlAudioPlayback)
* [Sharing audio playback state](#ShareAudioPlaybackState)
* [Managing audio information](#ManageStreamInfo)

## Playing audio stream {#PlayAudioStream}
When a user requests to play music, Clova directs the client to play the audio requested by the user via CIC. The action flow of playing the audio stream is shown below.

![](/Develop/Assets/Images/CIC_Audio_Play_Work_Flow.svg)

When a user requests to play music, CIC sends the [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) directive message to the client as the first step. This directive message contains information required to play the audio stream. This information must be used to find the audio data or display the audio information on the audio player. An example of a [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) directive message that can be received is as follows:

```json
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
```

A simple description of the main fields of `payload` is shown below.

* `audioItem`: The object containing the metadata of the audio to be played.
* `audioItem.stream`: The object containing the audio stream information required for playback.
* `source`: The source of the audio streaming service.

The client plays audio using the information contained in the `audioItem.stream` field. The information contained in the `audioItem` and `source` fields can be displayed on the UI of an audio player or can be used as reference information.

If the `audioItem.stream.urlPlayable` field value of the [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) directive message is set to `false`, the audio cannot be played instantly using the `audioItem.stream` field information. This occurs in situations where the actual audio information must be looked up once more before providing the audio to the user due to issues such as for service billing issue or security issue. An example of an [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) directive message that cannot be played instantly is shown below.

```json
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

Therefore, if the `audioItem.stream.urlPlayable` field value is set to `false`, the [`AudioPlayer.StreamRequested`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamRequested) event message must be sent once more as shown below just before playing the audio using the `audioItem.stream` field value received in the received [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) directive message.

```json
{
  "context": [
    ...
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

When the [`AudioPlayer.StreamRequested`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamRequested) event message as shown above is sent, the [`AudioPlayer.StreamDeliver`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamDeliver) directive message that contains actual playable information is received from the CIC as shown below. The client can then play audio using the newly received information.

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

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Audio playback information must always be maintained as the latest information and must be managed continuously. For more information, see <a href="#ManageStreamInfo">Managing audio information</a>.</p>
</div>


## Reporting audio playback progress {#ReportAudioPlaybackProgress}

For Clova to understand the current situation of the user in terms of audio playback, the client must start the audio playback and report the playback progress to CIC as follows:

![](/Develop/Assets/Images/CIC_Audio_Play_Progress_Reporting.svg)

As shown in the action flow above, whether or not some playback progress report is carried out or not depends on the values set in the `audioItem.stream.progressReport` field of the [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) directive message or the `audioStream.progressReport` field of the [`AudioPlayer.StreamDeliver`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamDeliver) directive message. The table below indicates the situations and the conditions where a report on the playback progress has to be made and the event messages that must be used.

| Point of playback                                 | Condition                         | Event message for reporting |
|-----------------------------------------|----------------------------|----------------------|
| Right after starting audio playback                         | Always                                                                      | [`AudioPlayer.PlayStarted`](/Develop/References/MessageInterfaces/AudioPlayer.md#PlayStarted) event message  |
| After starting audio playback and passing a specific duration          | When the `progressReport.progressReportDelayInMilliseconds` field is not `null`   | [`AudioPlayer.ProgressReportDelayPassed`](/Develop/References/MessageInterfaces/AudioPlayer.md#ProgressReportDelayPassed) event message  |
| After starting audio playback and whenever passing a specified interval  | When the `progressReport.progressReportIntervalInMilliseconds` field is not `null`  | [`AudioPlayer.ProgressReportIntervalPassed`](/Develop/References/MessageInterfaces/AudioPlayer.md#ProgressReportIntervalPassed) event message  |
| After starting audio playback and passing a specific point in playback  | When the `progressReport.progressReportPositionInMilliseconds` field is not `null`  | [`AudioPlayer.ProgressReportPositionPassed`](/Develop/References/MessageInterfaces/AudioPlayer.md#ProgressReportPositionPassed) event message  |
| Right after completing audio playback                         | Always                                                                      | [`AudioPlayer.PlayFinished`](/Develop/References/MessageInterfaces/AudioPlayer.md#PlayFinished) event message  |

<div class="note">
  <p><strong>Note!</strong></p>
  <p>If the <code>audioItem.stream.urlPlayable</code> field is <code>false</code> when the <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#Play"><code>AudioPlayer.Play</code></a> directive message is received, new streaming information can be sent as the <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#StreamDeliver"><code>AudioPlayer.StreamDeliver</code></a> directive messages in the future. If the values of <code>progressReport.progressReportDelayInMilliseconds</code>, <code>progressReport.progressReportIntervalInMilliseconds</code> and <code>progressReport.progressReportPositionInMilliseconds</code> fields are changed in the <code>AudioPlayer.Play</code> directive received after the <code>AudioPlayer.StreamDeliver</code> directive, the changed values must be applied to report the progress of audio playback. For more information, see <a href="#ManageStreamInfo">Managing audio information</a>.</p>
</div>

Using the received event messages, Clova identifies the audio currently being listened to by the user and the playback point of this audio. Below is an example of the [`AudioPlayer.ProgressReportIntervalPassed`](/Develop/References/MessageInterfaces/AudioPlayer.md#ProgressReportIntervalPassed) event message. At this time, [context information related to audio playback must be built at the same time](#BuildPlaybackStateContext) to be sent.

```json
{
  "context": [
    ...
    {
      "header": {
        "namespace": "AudioPlayer",
        "name": "PlaybackState"
      },
      "payload": {
        "offsetInMilliseconds": 10000,
        "totalInMilliseconds": 300000,
        "playerActivity": "PLAYING",
        "repeatMode": "NONE",
        "stream": {
          "beginAtInMilliseconds": 0,
          "format": "audio/mpeg",
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": 120000,
            "progressReportPositionInMilliseconds": 600000
          },
          "token": "TR-NM-4435786",
          "url": "https://musicservice.example.net/b767313e.mp3",
          "urlPlayable": true
        }
      }
    }
    ...
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



## Controlling audio playback {#ControlAudioPlayback}

Users can request Clova to pause, resume, stop, or replay the audio playback on the client. This audio playback control can be requested using the three methods below.

* User attempts playback control using speech (general method)
* User attempts playback control using buttons on client device
* User attempts playback control of a specific client remotely from the Clova app

However, the playback control on the audio is not conducted directly on the client because Clova must first identify the audio playback status of the user. All such playback controls are performed via Clova and the audio playback control must be mainly implemented via the [`PlaybackController`](/Develop/References/MessageInterfaces/PlaybackController.md) interface. Also, the result must be reported through the event messages of the [`AudioPlayer`](/Develop/References/MessageInterfaces/AudioPlayer.md) interface.

The diagram below shows the action flow when pausing audio playback.

![](/Develop/Assets/Images/CIC_Audio_Playback_Control_Flow.svg)

Generally, Clova analyzes the audio control via user utterance and the corresponding playback control directive message ([`PlaybackController.Pause`](/Develop/References/MessageInterfaces/PlaybackController.md#Pause)) is sent to the client. However, if the user has requested to pause the audio by pressing a button from the client, the state of the user pressing the pause button must be reported to Clova using the [`PlaybackController.PauseCommandIssued`](/Develop/References/MessageInterfaces/PlaybackController.md#PauseCommandIssued) event message shown below.

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "PauseCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}
```

If the user has requested playback control of the client remotely using a Clova app, Clova sends a directive message of a [`PlaybackController.ExpectPauseCommand`](/Develop/References/MessageInterfaces/PlaybackController.md#ExpectPauseCommand) format to this client. This is a message which directs the client to act as if a user has remotely pressed the button. Once the message below is received, the client can send a [`PlaybackController.PauseCommandIssued`](/Develop/References/MessageInterfaces/PlaybackController.md#PauseCommandIssued) event message like above to Cova.

```json
// Request to operate as if a user has pressed the pause button
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectPauseCommand",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

Clova sends the playback control requested by the user as the directive message ([`PlaybackController.Pause`](/Develop/References/MessageInterfaces/PlaybackController.md#Pause)) below, and the client can simply control audio playback according to the directive message.

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Pause",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

As the last step, the client can complete the playback control and report the action change in the audio player to CIC. Below shows the [AudioPlayer.PlayPaused](/Develop/References/MessageInterfaces/AudioPlayer.md#PlayPaused) event message sent to CIC after pausing the audio playback. At this time,  [context information must be built](#BuildPlaybackStateContext) according to the state of audio player.

```json
{
  "context": [
    ...
    {
      "header": {
        "namespace": "AudioPlayer",
        "name": "PlaybackState"
      },
      "payload": {
        "offsetInMilliseconds": 83100,
        "totalInMilliseconds": 300000,
        "playerActivity": "PAUSED",
        "repeatMode": "NONE",
        "stream": {
          "beginAtInMilliseconds": 0,
          "format": "audio/mpeg",
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": null,
            "progressReportPositionInMilliseconds": 60000
          },
          "token": "TR-NM-4435786",
          "url": "https://musicservice.example.net/b767313e.mp3",
          "urlPlayable": true
        }
      }
    }
    ...
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

## Managing audio information {#ManageStreamInfo}

When [playing the audio](#PlayAudioStream), the client receives information related to the audio from CIC. From the moment of starting to receive the [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) directive to CIC, the client must manage the audio information contained in the `audioItem.stream` field of the directive carefully. This section explains the following topics:

* [Updating audio information](#UpdateStreamInfo)
* [Building audio context information](#BuildPlaybackStateContext)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>If the details explained here are not observed, it might not be possible to play the audio properly, or Clova might not be able to respond to the user request to play the music again.</p>
</div>

### Updating audio information {#UpdateStreamInfo}

If the `AudioPlayer.Play` directive is received, save the values  below the `audioItem.stream` field separately. Particularly, the client must store the information below the `audioItem.stream` field carefully to regain the audio that can be actually played if the `audioItem.stream.urlPlayable` field is `false`.

```json
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "Play",
      ...
    },
    "payload": {
      "audioItem": {
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
        ...
      },
      ...
    }
  }
}
```

<div class="note">
  <p><strong>Note!</strong></p>
  <p>If the <code>audioItem.stream.urlPlayable</code> field is <code>false</code> when the <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#Play"><code>AudioPlayer.Play</code></a> directive is received, <strong>this must be combined with the audio information sent to the <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#StreamDeliver"><code>AudioPlayer.StreamDeliver</code></a> directive in the future.<strong></p>
</div>

Use the [`AudioPlayer.StreamRequested`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamRequested) event afterwards to request the actual information required for audio playback. At this time, the `audioItem.stream` field value stored earlier must be sent at the same time.

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "StreamRequested",
      ...
    },
    "payload": {
      ...
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

Then, the [`AudioPlayer.StreamDeliver`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamDeliver) directive message that contains actual playable information is received from the CIC as shown below. Details of the `audioStream` field contained here  **must be combined.** Overwrite the changed details of the field, and do not change or delete the parts without any changes in the newly received details.

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

The audio information managed this way must be [built as context](#BuildPlaybackStateContext) and reported.

### Building audio context information {#BuildPlaybackStateContext}

Audio playback state must be reported through the context each time the client sends an event to CIC. If the client audio player is in an inactive state, you can send by entering the `playerActivity` field value as `"IDLE"` as shown below.

```json
{
  "context": [
    ...
    {
      "header": {
        "namespace": "AudioPlayer",
        "name": "PlaybackState"
      },
      "payload": {
        "playerActivity": "IDLE",
        "repeatMode": "NONE"
      }
    }
    ...
  ],
  "event": {
    ...
  }
}
```

If the audio players is in an active state, select the `playerActivity` field as one of the following values depending on the state of the audio player:

* `"PLAYING"`
* `"PAUSE"`
* `"STOPPED"`

At this time, after receiving through the [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) or the [`AudioPlayer.StreamDeliver`](/Develop/References/MessageInterfaces/AudioPlayer.md#StreamDeliver) directive message, the [managed streaming info](#UpdateStreamInfo) must be included in the context at the same time.

```json
{
  "context": [
    ...
    {
      "header": {
        "namespace": "AudioPlayer",
        "name": "PlaybackState"
      },
      "payload": {
        "offsetInMilliseconds": 10000,
        "totalInMilliseconds": 300000,
        "playerActivity": "STOPPED",
        "repeatMode": "NONE",
        "stream": {
          "beginAtInMilliseconds": 0,
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
    ...
  ],
  "event": {
    ...
  }
}
```

Even if the state of the audio player enters the `"PAUSE"` mode because the user has requested to stop the audio playback, the audio information being stored must not be discarded freely. If the audio information was discarded, CIC cannot identify information on the audio which the client was playing from the context when a user has requested to play the music again. For this reason, Clova might not be able to provide the features like resuming playback to the user. Therefore, the client must continue to maintain the audio information except for special situations like rebooting. If it is necessary to remove the audio information, Clova will send a directive such as the [AudioPlayer.ClearQueue](/Develop/References/MessageInterfaces/AudioPlayer.md#ClearQueue) directive in order to remove the audio information of the client.


## Sharing audio playback state {#ShareAudioPlaybackState}

The client can receive a shared sound source playback state from all other or specific clients registered in the user account. The process to receive the shared audio playback state is as follows:

![](/Develop/Assets/Images/CIC_Playback_State_Sync_Work_Flow.svg)

1. Clova app requests the audio playback state of all or specific clients registered in the user account to CIC {{ "using the [`AudioPlayer.RequestPlaybackState`](/Develop/References/MessageInterfaces/AudioPlayer.md#RequestPlaybackState) event message " if book.DocMeta.TargetReaderType == "Internal" }}.
2. CIC instructs all or specific clients registered in the user account to report the current audio playback state using the [`AudioPlayer.ExpectReportPlaybackState`](/Develop/References/MessageInterfaces/AudioPlayer.md#ExpectReportPlaybackState) directive message.
3. After receiving the report request, the client reports the current audio playback state using [`AudioPlayer.ReportPlaybackState`](/Develop/References/MessageInterfaces/AudioPlayer.md#ReportPlaybackState) an event message.
4. CIC instructs the client that made the request to synchronize information by sending the states of the information {{ "using [`AudioPlayer.SynchronizePlaybackState`](/Develop/References/MessageInterfaces/AudioPlayer.md#SynchronizePlaybackState)" if book.DocMeta.TargetReaderType == "Internal" }}.

Therefore, the client must send the [`AudioPlayer.ReportPlaybackState`](/Develop/References/MessageInterfaces/AudioPlayer.md#ReportPlaybackState) event message if the [`AudioPlayer.ExpectReportPlaybackState`](/Develop/References/MessageInterfaces/AudioPlayer.md#ExpectReportPlaybackState) directive message is received, and the information below must be included in the `payload` field.

* Audio play state
* Repeat state
* Current-time indicator of the audio playing (when audio is playing)
* [`AudioStreamInfoObject`](/Develop/References/MessageInterfaces/AudioPlayer.md#AudioStreamInfoObject) object received from the [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) directive message (when audio is playing or paused)
* `audioItem.stream.token` field value of the [`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play) directive message (when audio is playing or paused)
* Total length of the playing media (optional)

```json
// When audio is not playing
{
  "context": [
    ...
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

// When audio is playing or paused
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "ReportPlaybackState",
      "messageId": "aa1b20b1-7562-4236-a7a6-65a235571bac"
    },
    "payload": {
      "playerActivity": "PLAYING",
      "repeatMode": "NONE",
      "offsetInMilliseconds": 13000,
      "stream": {
        "beginAtInMilliseconds": 0,
        "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": null,
            "progressReportPositionInMilliseconds": 30000
        },
        "token": "TR-NM-4435786",
        "urlPlayable": false,
        "url": "clova:TR-NM-4435786"
      },
      "token": "TR-NM-4435786"
    }
  }
}
```
