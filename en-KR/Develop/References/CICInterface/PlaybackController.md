<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

# PlaybackController

<!-- Start of the shared content: CICAPIforAudioPlayback -->
The PlaybackController namespace provides interfaces for playing audio and controlling sound on a client. The PlaybackController namespace provides the following event messages and directive messages.

| Message name         | Type  | Description                                   |
|------------------|-----------|---------------------------------------------|
| [`CustomCommandIssued`](#CustomCommandIssued)  | Event     | Reports to CIC when the user presses one of the shortcut buttons on the client device.  |
| [`ExpectNextCommand`](#ExpectNextCommand)      | Directive | Instructs the client to send the [`PlaybackController.NextCommandIssued`](#NextCommandIssued) event message to CIC just like the effect of a user pressing the Next button from the client device.  |
| [`ExpectPauseCommand`](#ExpectPauseCommand)    | Directive | Instructs the client to send the [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued) event message to CIC just like the effect of a user pressing the Pause button from the client device.  |
| [`ExpectPlayCommand`](#ExpectPlayCommand)      | Directive | Instructs the client to send the [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued) event message to CIC just like the effect of a user pressing the Play button from the client device.  |
| [`ExpectPreviousCommand`](#ExpectPreviousCommand)  | Directive | Instructs the client to send the [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued) event message to CIC just like the effect of a user pressing the Previous button from the client device.  |
| [`ExpectResumeCommand`](#ExpectResumeCommand)  | Directive | Instructs the client to send the [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued) event message to CIC just like the effect of a user pressing the Resume button from the client device.  |
| [`ExpectStopCommand`](#ExpectStopCommand)      | Directive | Instructs the client to send the [`PlaybackController.StopCommandIssued`](#StopCommandIssued) event message to CIC just like the effect of a user pressing the Stop button from the client device.  |
| [`Mute`](#Mute)                                | Directive | Instructs the client to mute the audio player.            |
| [`Next`](#Next)                                | Directive | Instructs the client to start playing the next audio stream in the playback queue.   |
| [`NextCommandIssued`](#NextCommandIssued)      | Event     | Reports to CIC if the user presses the Next button on the client device or responds to the [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand) directive message from CIC. |
| [`Pause`](#Pause)                              | Directive | Instructs the client to pause the current audio stream.        |
| [`PauseCommandIssued`](#PauseCommandIssued)    | Event     | Reports to CIC if the user presses the Pause button on the client device or responds to the [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand) directive message from CIC.  |
| [`PlayCommandIssued`](#PlayCommandIssued)      | Event     | Reports to CIC if the user presses the Play button on the client device or responds to the [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand) directive message from CIC.  |
| [`Previous`](#Previous)                        | Directive | Instructs the client to start playing the previous audio stream in the playback queue. |
| [`PreviousCommandIssued`](#PreviousCommandIssued) | Event | Reports to CIC if the user presses the Previous button on the client device or responds to the [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand) directive message from CIC. |
| [`Replay`](#Replay)                            | Directive | Instructs the client to replay the current audio stream from the beginning.         |
| [`Resume`](#Resume)                            | Directive | Instructs the client to resume playing the audio stream.                |
| [`ResumeCommandIssued`](#ResumeCommandIssued)  | Directive | Reports to CIC if the user presses the Resume button on the client device or responds to the [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand) directive message from CIC.  |
| [`SetRepeatMode`](#SetRepeatMode)              | Directive | Instructs the client to change the playback state to a specified repeat mode.  |
| [`SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued) | Event | Reports to CIC when the user presses the Repeat button on the client device.  |
| [`Stop`](#Stop)                                | Directive | Instructs the client to stop playing an audio stream.                |
| [`StopCommandIssued`](#StopCommandIssued)      | Event     | Reports to CIC if the user presses the Resume button on the client device or responds to the [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand) directive message from CIC.  |
| [`TurnOffRepeatMode`](#TurnOffRepeatMode)      | Directive | **(Deprecated)** Instructs the client to stop the repeat function for one song.                  |
| [`TurnOnRepeatMode`](#TurnOnRepeatMode)        | Directive | **(Deprecated)** Instructs the client to start the repeat function for one song.                  |
| [`Unmute`](#Unmute)                            | Directive | Instructs the client to unmute the audio player.              |
| [`VolumeDown`](#VolumeDown)                    | Directive | **(Deprecated)** Instructs the client to turn down the audio player volume.                      |
| [`VolumeUp`](#VolumeUp)                        | Directive | **(Deprecated)** Instructs the client to turn up the audio player volume.                      |

<!-- End of the shared content -->

## CustomCommandIssued event {#CustomCommandIssued}
Reports to CIC when the user presses one of the shortcut buttons on the client device. Upon receiving the event message, CIC sends the appropriate directive message to the client.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields
| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `button`      | string  | The name of the shortcut button on the client device (e.g., <code>"CUSTOM_BUTTON_2"</code>). | Required |

### Remarks
* The button on the client device can either be a physical button or a software button like a widget button on a music player.

### Message example

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
      "namespace": "PlaybackController",
      "name": "CustomCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "button": "CUSTOM_BUTTON_2"
    }
  }
}
```

### See also
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

## ExpectNextCommand directive {#ExpectNextCommand}
Instructs the client to send the [`PlaybackController.NextCommandIssued`](#NextCommandIssued) event message to CIC just like the effect of a user pressing the Next button from the client device. Upon receiving the directive message, the client must perform the relevant action and send the [`PlaybackController.NextCommandIssued`](#NextCommandIssued) event message to CIC.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | Object containing the control target information.<div class="note"><p><strong>Note!</strong></p><p>If this field exists, the client must send the <code>source</code> field together with the <a href="#NextCommandIssued"><code>PlaybackController.NextCommandIssued</code></a> event message.</p></div> | Conditional  |
| target.namespace  | string  | CIC API namespace. This is the information to identify the control target. When you send the [`PlaybackController.NextCommandIssued`](#NextCommandIssued) event message, you must enter `source.namespace` as value of this field. Available values are:<ul><li><code>"AudioPlayer"</code>: audio player</li><li><code>"MediaPlayer"</code>: media player</li></ul>  | Always  |

### Message example

```json
Example 1: An example where target is not specified
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectNextCommand",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}

Example 2: An example where target is specified as "AudioPlayer"
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectNextCommand",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)

## ExpectPauseCommand directive {#ExpectPauseCommand}
Instructs the client to send the [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued) event message to CIC just like the effect of a user pressing the Pause button from the client device. Upon receiving the directive message, the client must send the [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued) event message to CIC.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | Object containing the control target information.<div class="note"><p><strong>Note!</strong></p><p>If this field exists, the client must send the <code>source</code> field together with the <a href="#PauseCommandIssued"><code>PlaybackController.PauseCommandIssued</code></a> event message.</p></div> | Conditional  |
| target.namespace  | string  | CIC API namespace. This is the information to identify the control target. When you send the [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued) event message, you must enter `source.namespace` as value of this field. Available values are:<ul><li><code>"AudioPlayer"</code>: audio player</li><li><code>"MediaPlayer"</code>: media player</li></ul>  | Always  |

### Message example

```json
Example 1: An example where target is not specified
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

Example 2: An example where target is specified as "AudioPlayer"
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectPauseCommand",
      "dialogRequestId": "7182403e-b5eb-4b71-b2af-179a7515edc4",
      "messageId": "bbb3a40d-ad7f-4609-9021-97f3f0aa2a9e"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [Controlling audio playback](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

## ExpectPlayCommand directive {#ExpectPlayCommand}
Instructs the client to send the [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued) event message to CIC just like the effect of a user pressing the Play button from the client device. The client can also receive this directive message when attempting to play the currently playing media stream from another device. Upon receiving the directive message, the client must perform the relevant action and send the [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued) event message to CIC.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `handover`            | object  | Object containing information required for remote takeover of media playback. The `handover` object is included in the directive message if the media playback must be taken over. So, if the `handover` object is included in the directive message, the client must use the details of this object in the `handover` object in the `payload` of the [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued) event message.     | Conditional |
| `handover.customData` | string  | The information required to play the media.               | Always |
| `handover.deviceId`   | string  | The ID of the client device handing over media playback.  | Always |
| `token`               | string  | The token of the media to play. The `token` field is included in the directive message if the media playback must be taken over. So, if a value exists in the `token` field, the client must include this value in the `token` field when sending the [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued) event message.  | Conditional  |

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectPlayCommand",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)

## ExpectPreviousCommand directive {#ExpectPreviousCommand}
Instructs the client to send the [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued) event message to CIC just like the effect of a user pressing the Previous button from the client device. Upon receiving the directive message, the client must perform the relevant action and send the [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued) event message to CIC.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | Object containing the control target information.<div class="note"><p><strong>Note!</strong></p><p>If this field exists, the client must send the <code>source</code> field together with the <a href="#PreviousCommandIssued"><code>PlaybackController.PreviousCommandIssued</code></a> event message.</p></div> | Conditional  |
| target.namespace  | string  | CIC API namespace. This is the information to identify the control target. When you send the [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued) event message, you must enter `source.namespace` as value of this field. Available values are:<ul><li><code>"AudioPlayer"</code>: audio player</li><li><code>"MediaPlayer"</code>: media player</li></ul>  | Always  |

### Message example

```json
Example 1: An example where target is not specified
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectPreviousCommand",
      "dialogRequestId": "036fbb1e-4739-4453-9439-ccff68eccc63",
      "messageId": "e32f6330-e696-4c21-9e52-529e9b4cbc14"
    },
    "payload": {}
  }
}

Example 2: An example where target is specified as "AudioPlayer"
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectPreviousCommand",
      "dialogRequestId": "036fbb1e-4739-4453-9439-ccff68eccc63",
      "messageId": "e32f6330-e696-4c21-9e52-529e9b4cbc14"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)

## ExpectResumeCommand directive {#ExpectResumeCommand}
Instructs the client to send the [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued) event message to CIC just like the effect of a user pressing the Resume button from the client device. Upon receiving the directive message, the client must send the [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued) event message to CIC.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | Object containing the control target information.<div class="note"><p><strong>Note!</strong></p><p>If this field exists, the client must send the <code>source</code> field together with the <a href="#ResumeCommandIssued"><code>PlaybackController.ResumeCommandIssued</code></a> event message.</p></div> | Conditional  |
| target.namespace  | string  | CIC API namespace. This is the information to identify the control target. When you send the [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued) event message, you must enter `source.namespace` as value of this field. Available values are:<ul><li><code>"AudioPlayer"</code>: audio player</li><li><code>"MediaPlayer"</code>: media player</li></ul>  | Always  |

### Message example

```json
Example 1: An example where target is not specified
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectResumeCommand",
      "dialogRequestId": "70d75f9a-2202-4a4e-a0cb-9f804d235890",
      "messageId": "3865c844-27dd-4fda-a7d2-03ef96a4cc83"
    },
    "payload": {}
  }
}

Example 2: An example where target is specified as "AudioPlayer"
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectResumeCommand",
      "dialogRequestId": "d7b5f918-2bb5-4b94-91c5-e1ddec0aa8c4",
      "messageId": "c75a01b4-8402-444a-9386-aa1819d12d29"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [Controlling audio playback](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

## ExpectStopCommand directive {#ExpectStopCommand}
Instructs the client to send the [`PlaybackController.StopCommandIssued`](#StopCommandIssued) event message to CIC just like the effect of a user pressing the Stop button from the client device. Upon receiving the directive message, the client must send the [`PlaybackController.StopCommandIssued`](#StopCommandIssued) event message to CIC.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | Object containing the control target information.<div class="note"><p><strong>Note!</strong></p><p>If this field exists, the client must send the <code>source</code> field together with the <a href="#StopCommandIssued"><code>PlaybackController.StopCommandIssued</code></a> event message.</p></div> | Conditional  |
| target.namespace  | string  | CIC API namespace. This is the information to identify the control target. When you send the [`PlaybackController.StopCommandIssued`](#StopCommandIssued) event message, you must enter `source.namespace` as value of this field. Available values are:<ul><li><code>"AudioPlayer"</code>: audio player</li><li><code>"MediaPlayer"</code>: media player</li></ul>  | Always  |

### Message example

```json
Example 1: An example where target is not specified
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectStopCommand",
      "dialogRequestId": "e3295587-23a5-49d7-bc72-05adf02e4a08",
      "messageId": "8b15ad68-ee0c-44de-959a-0f0c3526b361"
    },
    "payload": {}
  }
}

Example 2: An example where target is specified as "AudioPlayer"
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectStopCommand",
      "dialogRequestId": "bbfdad3a-333c-4a97-beb0-33ac9d46ac9d",
      "messageId": "1a565441-e2bc-40a5-bb3e-7846767f37be"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)
* [Controlling audio playback](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

## Mute directive {#Mute}
Instructs the client to mute the audio player. Upon receiving the directive message, the client must mute the speaker for the audio stream playback.

### Payload fields
None

### Remarks

If the control is related to speaker output, Clova does not provide a voice guide with the [`SpeechSynthesizer.Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak) directive message. This is in consideration of the UX such as for a user listening to music. For this, you must implement an action to inform the user that the volume has been changed using the lights or a simple sound effect on the client.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Mute",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## Next directive {#Next}
Instructs the client to start playing the next audio stream in the playback queue. Upon receiving the directive message, the client must play the next audio stream in queue.

### Payload fields
None

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Next",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also
* [`PlaybackController.Previous`](#Previous)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## NextCommandIssued event {#NextCommandIssued}
Reports to CIC if the user presses the Next button on the client device or responds to the [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand) directive message from CIC. Upon receiving the event message, CIC sends the appropriate directive message to the client.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | The client device ID. Skip this field if you are not controlling the media playback remotely. | Optional |
| `source`      | object  | Object containing the original information of event. Includes the information to find out the situation or the background for sending this event message. <div class="tip"><p><strong>Tip!</strong></p><p>If the client is providing speech output (TTS) and audio playback to the user at the same time, the speech output becomes the control target. Using this field, you can specify  on which target this event message is trying to control. Omit this field to delegate so that Clova specifies the control target by itself.</p></div>  | Optional  |
| `source.namespace` | string | CIC API namespace Enter the related CIC API namespace value to identify what kind of situation or context this event message is sent from.<ul><li><code>"AudioPlayer"</code>: audio player</li><li><code>"MediaPlayer"</code>: media player</li></ul>  | Required  |

### Remarks
* The button on the client device can either be a physical button or a software button like a widget button on a music player.

### Message example

```json
Example 1: An example that has not specified the original information
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
      "namespace": "PlaybackController",
      "name": "NextCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}

Example 2: An example that has specified the original information as "AudioPlayer".
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
      "namespace": "PlaybackController",
      "name": "NextCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "source": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

<!-- End of the shared content -->

<!-- Start of the shared content: PlaybackController.Pause -->

## Pause directive {#Pause}
Instructs the client to pause the current audio stream. Upon receiving the directive message, the client must pause playing the audio stream.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | The object contatining control target. You can see the target to control through this directive message. | Conditional  |
| target.namespace  | string  | CIC API namespace. This is the information to identify the control target. Available values are:<ul><li><code>"AudioPlayer"</code>: audio player</li><li><code>"MediaPlayer"</code>: media player</li></ul>  | Always  |

### Message example

```json
Example 1: An example where target is not specified
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

Example 2: An example where target is specified as "AudioPlayer"
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Pause",
      "dialogRequestId": "a85e78b4-44f7-4bea-8a40-66c181b9720f",
      "messageId": "a76c8dfd-c1b6-44f6-a58d-fc8f33c242c1"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also
* [`AudioPlayer.PlayPaused`](/Develop/References/CICInterface/AudioPlayer.md#PlayPaused)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
* [Controlling audio playback](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

## PauseCommandIssued event {#PauseCommandIssued}
Reports to CIC if the user presses the Pause button on the client device or responds to the [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand) directive message from CIC. Upon receiving the event message, CIC sends the appropriate directive message to the client.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | The client device ID. Skip this field if you are not controlling the media playback remotely. | Optional |
| `source`      | object  | Object containing the original information of event. Includes the information to find out the situation or the background for sending this event message. <div class="tip"><p><strong>Tip!</strong></p><p>If the client is providing speech output (TTS) and audio playback to the user at the same time, the speech output becomes the control target. Using this field, you can specify  on which target this event message is trying to control. Omit this field to delegate so that Clova specifies the control target by itself.</p></div>  | Optional  |
| `source.namespace` | string | CIC API namespace Enter the related CIC API namespace value to identify what kind of situation or context this event message is sent from. Only `"AudioPlayer"` value is currently available.  | Required  |

### Remarks
* The button on the client device can either be a physical button or a software button like a widget button on a music player.

### Message example

```json
// Example 1: An example that has not specified the original information
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
      "namespace": "PlaybackController",
      "name": "PauseCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}

// Example 2: An example that has specified the original information as "AudioPlayer".
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
      "namespace": "PlaybackController",
      "name": "PauseCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "source": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectPauseCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)
* [Controlling audio playback](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

## PlayCommandIssued event {#PlayCommandIssued}
Reports to CIC if the user operates the UI to play a specific song on the client device or responds to the [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand) directive message from CIC. Upon receiving the event message, CIC sends the appropriate directive message to the client. If the `handover` field exists is in the `payload` of the [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand) directive message received from CIC, the client must take over the media playback using the field value.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`            | string  | The client device ID. Skip the `deviceId` field if you are not remotely handing over the media playback to another device. | Optional |
| `handover`            | object  | Object containing information required for remote takeover of media playback. If media playback must be taken over, use the `handover` object as the `handover` object in `payload` of the [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand) directive message.     | Optional |
| `handover.customData` | string  | The information required to play the media.               | Required |
| `handover.deviceId`   | string  | The ID of the client device handing over media playback.  | Required |
| `token`               | string  | The token of the media to play. When the user presses the Play button after selecting a media on the playlist, the `playableItems[].token` field value of the [`TemplateRuntime.RenderPlayerInfo`](/Develop/References/CICInterface/TemplateRuntime.md#RenderPlayerInfo) directive message must be applied to this field. Or, the value of the `token` field value may need to be used if the [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand) directive message is received.  | Optional  |

### Remarks
* The [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued) event message must be sent to CIC when a user presses the Play button from the client device.

### Message example

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
      "namespace": "PlaybackController",
      "name": "PlayCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}
```

### See also
* [`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectPlayCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

## Previous directive {#Previous}
Instructs the client to start playing the previous audio stream in the playback queue. Upon receiving the directive message, the client must play the previous audio stream.

### Payload fields
None

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Previous",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also
* [`PlaybackController.Next`](#Next)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## PreviousCommandIssued event {#PreviousCommandIssued}
Reports to CIC if the user presses the Previous button on the client device or responds to the [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand) directive message from CIC. Upon receiving the event message, CIC sends the appropriate directive message to the client.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | The client device ID. Skip this field if you are not controlling the media playback remotely. | Optional |
| `source`      | object  | Object containing the original information of event. Includes the information to find out the situation or the background for sending this event message. <div class="tip"><p><strong>Tip!</strong></p><p>If the client is providing speech output (TTS) and audio playback to the user at the same time, the speech output becomes the control target. Using this field, you can specify  on which target this event message is trying to control. Omit this field to delegate so that Clova specifies the control target by itself.</p></div>  | Optional  |
| `source.namespace` | string | CIC API namespace Enter the related CIC API namespace value to identify what kind of situation or context this event message is sent from.: audio player. Only `"AudioPlayer"` value is currently available.  | Required  |

### Remarks
* The button on the client device can either be a physical button or a software button like a widget button on a music player.

### Message example

```json
// Example 1: An example that has not specified the original information
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
      "namespace": "PlaybackController",
      "name": "PreviousCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}

// Example 2: An example that has specified the original information as "AudioPlayer".
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
      "namespace": "PlaybackController",
      "name": "PreviousCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "source": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

## Replay directive {#Replay}
Instructs the client to replay the current audio stream from the beginning. Upon receiving the directive message, the client must play the currently playing audio stream from the beginning. If playing had been paused, then resume playing the audio stream.

### Payload fields
None

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Replay",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also
* [`PlaybackController.Pause`](#Pause)
* [`PlaybackController.Resume`](#Resume)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
* [Controlling audio playback](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- Start of the shared content: PlaybackController.Resume -->

## Resume directive {#Resume}
Instructs the client to resume playing the audio stream. Upon receiving the directive message, the client must resume playing the audio stream.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | The object that has control target. You can see  the target to control through this directive message. | Conditional  |
| target.namespace  | string  | CIC API namespace. This is the information to identify the control target. Available values are:<ul><li><code>"AudioPlayer"</code>: audio player</li><li><code>"MediaPlayer"</code>: media player</li></ul>  | Always  |

### Message example

```json
// Example 1: An example where target is not specified
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Resume",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}

// Example 2: An example where target is specified as "AudioPlayer"
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Resume",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also
* [`AudioPlayer.PlayResumed`](/Develop/References/CICInterface/AudioPlayer.md#PlayResumed)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
* [Controlling audio playback](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

## ResumeCommandIssued event {#ResumeCommandIssued}
Reports to CIC if the user presses the Play button or the Resume button on the client device, or responds to the [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand) directive message from CIC. Upon receiving the event message, CIC sends the appropriate directive message to the client.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | The client device ID. Skip this field if you are not controlling the media playback remotely. | Optional |
| `source`      | object  | Object containing the original information of event. Includes the information to find out the situation or the background for sending this event message. <div class="tip"><p><strong>Tip!</strong></p><p>If the client is providing speech output (TTS) and audio playback to the user at the same time, the speech output becomes the control target. Using this field, you can specify  on which target this event message is trying to control. Omit this field to delegate so that Clova specifies the control target by itself.</p></div>  | Optional  |
| `source.namespace` | string | CIC API namespace Enter the related CIC API namespace value to identify what kind of situation or context this event message is sent from.: audio player. Only `"AudioPlayer"` value is currently available.  | Required  |

### Remarks
* The button on the client device can either be a physical button or a software button like a widget button on a music player.
* This event message must be used even **when the user presses the Play button**.

### Message example

```json
// Example 1: An example that has not specified the original information
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
      "namespace": "PlaybackController",
      "name": "ResumeCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}

// Example 2: An example that has specified the original information as "AudioPlayer".
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
      "namespace": "PlaybackController",
      "name": "ResumeCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "source": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectResumeCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)
* [Controlling audio playback](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

## SetRepeatMode directive {#SetRepeatMode}

Instructs the client to change the playback state to a specified repeat mode. Upon receiving the directive message, the client must change the state ([context object](/Develop/References/Context_Objects.md)) of the repeat mode as the specified value. The client must also set the `repeatMode` field of the [`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState) to the specified value when sending the event message.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `repeatMode`  | string  | The selected repeat mode.<ul><li><code>NONE</code>: No repeat</li><code>REPEAT_ONE</code>: Repeat one song</li></ul>  | Always |

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "SetRepeatMode",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {
      "repeatMode": "NONE"
    }
  }
}
```

### See also
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)

## SetRepeatModeCommandIssued event {#SetRepeatModeCommandIssued}

Reports to CIC to change the play mode of a Clova device remotely from the Clova app or the companion app. Upon receiving the event message, CIC sends the [`PlaybackController.SetRepeatMode`](#SetRepeatMode) directive message to the target client.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The client does not need to create or handle this event message unless changing the play mode of the Clova device remotely from the companion app. To change the play mode on the client device, report to CIC using the <code>repeatMode</code> field of the <a href="/Develop/References/Context_Objects.md#PlaybackState"><code>AudioPlayer.PlaybackState</code></a> context information.</p>
</div>

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | The client device ID. Skip this field if you are not controlling the media playback remotely.                               | Optional |
| `repeatMode`  | string  | The selected repeat mode.<ul><li><code>NONE</code>: No repeat</li><code>REPEAT_ONE</code>: Repeat one song</li></ul>  | Required |

### Remarks
* If there is no function on the companion app to change the play mode of a Clova device, then there is no need to create this event message or handle any tasks related to this message.
* For the change of play mode on the client device, report to CIC using the `repeatMode` field of the [`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState) context information.

### Message example

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
      "namespace": "PlaybackController",
      "name": "SetRepeatModeCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "repeatMode": "NONE"
    }
  }
}
```

### See also
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.SetRepeatMode`](#SetRepeatMode)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

<!-- Start of the shared content: PlaybackController.Stop -->

## Stop directive {#Stop}
Instructs the client to stop playing an audio stream. Upon receiving the directive message, the client must stop playing the audio stream.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| target            | object  | The object that has control target. You can see  the target to control through this directive message. | Conditional  |
| target.namespace  | string  | CIC API namespace. This is the information to identify the control target. Available values are:<ul><li><code>"AudioPlayer"</code>: audio player</li><li><code>"MediaPlayer"</code>: media player</li></ul>  | Always  |

### Message example

```json
// Example 1: An example where target is not specified
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Stop",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}

// Example 2: An example where target is specified as "AudioPlayer"
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Stop",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {
      "target": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also
* [`AudioPlayer.PlayStopped`](/Develop/References/CICInterface/AudioPlayer.md#PlayStopped)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
* [Controlling audio playback](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

## StopCommandIssued event {#StopCommandIssued}
Reports to CIC if the user presses the Resume button on the client device or responds to the [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand) directive message from CIC. Upon receiving the event message, CIC sends the appropriate directive message to the client.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | The client device ID. Skip this field if you are not controlling the media playback remotely. | Optional |
| `source`      | object  | Object containing the original information of event. Includes the information to find out the situation or the background for sending this event message. <div class="tip"><p><strong>Tip!</strong></p><p>If the client is providing speech output (TTS) and audio playback to the user at the same time, the speech output becomes the control target. Using this field, you can specify  on which target this event message is trying to control. Omit this field to delegate so that Clova specifies the control target by itself.</p></div>  | Optional  |
| `source.namespace` | string | CIC API namespace Enter the related CIC API namespace value to identify what kind of situation or context this event message is sent from.: audio player. Only `"AudioPlayer"` value is currently available.  | Required  |

### Remarks
* The button on the client device can either be a physical button or a software button like a widget button on a music player.

### Message example

```json
// Example 1: An example that has not specified the original information
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
      "namespace": "PlaybackController",
      "name": "StopCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}

// Example 2: An example that has specified the original information as "AudioPlayer".
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
      "namespace": "PlaybackController",
      "name": "StopCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "source": {
        "namespace": "AudioPlayer"
      }
    }
  }
}
```

### See also
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectResumeCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [Controlling audio playback](/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ControlAudioPlayback)

## TurnOffRepeatMode directive {#TurnOffRepeatMode}
**(Deprecated)** Instructs the client to stop the repeat function for one song.

### Payload fields
None

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "TurnOffRepeatMode",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## TurnOnRepeatMode directive {#TurnOnRepeatMode}
**(Deprecated)** Instructs the client to start the repeat function for one song. Upon receiving the directive message, the client must repeat the current audio stream.

### Payload fields
None

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "TurnOnRepeatMode",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## Unmute directive {#Unmute}
Instructs the client to unmute the audio player. Upon receiving the directive message, the client must restore the volume level to before mute was applied.

### Payload fields
None

### Remarks

If the control is related to speaker output, Clova does not provide a voice guide with the [`SpeechSynthesizer.Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak) directive message. This is in consideration of the UX such as for a user listening to music. For this, you must implement an action to inform the user that the volume has been changed using the lights or a simple sound effect on the client.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Unmute",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## VolumeDown directive {#VolumeDown}
**(Deprecated)** Instructs the client to turn down the audio player volume. Upon receiving the directive message, the client must turn down the volume of the audio player. The volume adjustment level depends on the client UX standard.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The <code>PlaybackController.VolumeDown</code> directive message is to be deprecated. You must use the <a href="/Develop/References/CICInterface/DeviceControl.md#Decrease"><code>DiviceControl.Decrease</code></a> directive message instead of this directive message.</p>
</div>

### Payload fields
None

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "VolumeDown",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## VolumeUp directive {#VolumeUp}

**(Deprecated)** Instructs the client to turn up the audio player volume. Upon receiving the directive message, the client must turn up the volume of the audio player. The volume adjustment level depends on the client UX standard.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The <code>PlaybackController.VolumeUp</code> directive message is to be deprecated. You must use the <a href="/Develop/References/CICInterface/DeviceControl.md#Increase"><code>DiviceControl.Increase</code></a> directive message instead.</p>
</div>

### Payload fields
None

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "VolumeUp",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

### See also
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
