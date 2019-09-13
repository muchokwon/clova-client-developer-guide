# PlaybackController

The PlaybackController namespace provides interfaces for playing audio and controlling sound on a client. The PlaybackController namespace provides the following events and directives.

| Message name         | Type  | Description                                   |
|------------------|-----------|---------------------------------------------|
| [`CustomCommandIssued`](#CustomCommandIssued)  | Event     | Reports to CIC when the user presses one of the shortcut buttons on the client device.  |
| [`ExpectNextCommand`](#ExpectNextCommand)      | Directive | Instructs the client to send the [`PlaybackController.NextCommandIssued`](#NextCommandIssued) event to CIC just like the effect of a user pressing the Next button from the client device.  |
| [`ExpectPauseCommand`](#ExpectPauseCommand)    | Directive | Instructs the client to send the [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued) event to CIC just like the effect of a user pressing the Pause button from the client device.  |
| [`ExpectPlayCommand`](#ExpectPlayCommand)      | Directive | Instructs the client to send the [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued) event to CIC just like the effect of a user pressing the Play button from the client device.  |
| [`ExpectPreviousCommand`](#ExpectPreviousCommand)  | Directive | Instructs the client to send the [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued) event to CIC just like the effect of a user pressing the Previous button from the client device.  |
| [`ExpectResumeCommand`](#ExpectResumeCommand)  | Directive | Instructs the client to send the [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued) event to CIC just like the effect of a user pressing the Resume button from the client device.  |
| [`ExpectStopCommand`](#ExpectStopCommand)      | Directive | Instructs the client to send the [`PlaybackController.StopCommandIssued`](#StopCommandIssued) event to CIC just like the effect of a user pressing the Stop button from the client device.  |
| [`Mute`](#Mute)                                | Directive | Instructs the client to mute the audio player.            |
| [`Next`](#Next)                                | Directive | Instructs the client to start playing the next audio stream in the playback queue.   |
| [`NextCommandIssued`](#NextCommandIssued)      | Event     | Reports to CIC if the user presses the Next button on the client device or responds to the [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand) directive from CIC. |
| [`Pause`](#Pause)                              | Directive | Instructs the client to pause the current audio stream.        |
| [`PauseCommandIssued`](#PauseCommandIssued)    | Event     | Reports to CIC if the user presses the Pause button on the client device or responds to the [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand) directive from CIC.  |
| [`PlayCommandIssued`](#PlayCommandIssued)      | Event     | Reports to CIC if the user presses the Play button on the client device or responds to the [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand) directive from CIC.  |
| [`Previous`](#Previous)                        | Directive | Instructs the client to start playing the previous audio stream in the playback queue. |
| [`PreviousCommandIssued`](#PreviousCommandIssued) | Event | Reports to CIC if the user presses the Previous button on the client device or responds to the [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand) directive from CIC. |
| [`Replay`](#Replay)                            | Directive | Instructs the client to replay the current audio stream from the beginning.         |
| [`Resume`](#Resume)                            | Directive | Instructs the client to resume playing the audio stream.                |
| [`ResumeCommandIssued`](#ResumeCommandIssued)  | Directive | Reports to CIC if the user presses the Resume button on the client device or responds to the [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand) directive from CIC.  |
| [`SetRepeatMode`](#SetRepeatMode)              | Directive | Instructs the client to change the playback state to a specified repeat mode.  |
| [`SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued) | Event | Reports to CIC when the user presses the Repeat button on the client device.  |
| [`Stop`](#Stop)                                | Directive | Instructs the client to stop playing an audio stream.                |
| [`StopCommandIssued`](#StopCommandIssued)      | Event     | Reports to CIC if the user presses the Resume button on the client device or responds to the [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand) directive from CIC.  |
| [`TurnOffRepeatMode`](#TurnOffRepeatMode)      | Directive | **(Deprecated)** Instructs the client to stop the repeat function for one song.                  |
| [`TurnOnRepeatMode`](#TurnOnRepeatMode)        | Directive | **(Deprecated)** Instructs the client to start the repeat function for one song.                  |
| [`Unmute`](#Unmute)                            | Directive | Instructs the client to unmute the audio player.              |
| [`VolumeDown`](#VolumeDown)                    | Directive | **(Deprecated)** Instructs the client to turn down the audio player volume.                      |
| [`VolumeUp`](#VolumeUp)                        | Directive | **(Deprecated)** Instructs the client to turn up the audio player volume.                      |

## CustomCommandIssued event {#CustomCommandIssued}
Reports to CIC when the user presses one of the shortcut buttons on the client device. Upon receiving the event, CIC sends the appropriate directive to the client.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields
| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `button`      | string  | The name of the shortcut button on the client device (e.g., <code>"CUSTOM_BUTTON_2"</code>). | Required |

### Remarks
* The button on the client device can either be a physical button or a software button like a widget button on a music player.

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
{% endraw %}

### See also
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

## ExpectNextCommand directive {#ExpectNextCommand}
Instructs the client to send the [`PlaybackController.NextCommandIssued`](#NextCommandIssued) event to CIC just like the effect of a user pressing the Next button from the client device. Upon receiving this directive, the client must perform the relevant action and send the [`PlaybackController.NextCommandIssued`](#NextCommandIssued) event to CIC.

### Payload fields
None

### Message example
{% raw %}
```json
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
```
{% endraw %}

### See also
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)

## ExpectPauseCommand directive {#ExpectPauseCommand}
Instructs the client to send the [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued) event to CIC just like the effect of a user pressing the Pause button from the client device. Upon receiving this directive, the client must send the [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued) event to CIC.

### Payload fields
None

### Message example
{% raw %}
```json
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
{% endraw %}

### See also
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [Controlling audio playback](/Develop/Guides/Implement_Client_Features.md#ControlAudioPlayback)

## ExpectPlayCommand directive {#ExpectPlayCommand}
Instructs the client to send the [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued) event to CIC just like the effect of a user pressing the Play button from the client device. The client can also receive this directive when attempting to play the currently playing media stream from another device. Upon receiving this directive, the client must perform the relevant action and send the [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued) event to CIC.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `handover`            | object  | Object containing information required for remote takeover of media playback. This object is included in the directive if the media playback must be taken over. So, if the `handover` object is included in the directive, the client must use the details of this object in the `handover` object in the `payload` of the [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued) event.     | Conditional |
| `handover.deviceId`   | string  | The ID of the client device handing over media playback.  | Always |
| `handover.customData` | string  | The information required to play the media.               | Always |
| `token`               | string  | The token of the media to play. The `token` field is included in the directive if the media playback must be taken over. So, if a value exists in the `token` field, the client must include this value in the `token` field when sending the [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued) event.  | Conditional  |

### Message example
{% raw %}
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
{% endraw %}

### See also
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)

## ExpectPreviousCommand directive {#ExpectPreviousCommand}
Instructs the client to send the [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued) event to CIC just like the effect of a user pressing the Previous button from the client device. Upon receiving this directive, the client must perform the relevant action and send the [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued) event to CIC.

### Payload fields
None

### Message example
{% raw %}
```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectPreviousCommand",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```
{% endraw %}

### See also
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)

## ExpectResumeCommand directive {#ExpectResumeCommand}
Instructs the client to send the [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued) event to CIC just like the effect of a user pressing the Resume button from the client device. Upon receiving this directive, the client must send the [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued) event to CIC.

### Payload fields
None

### Message example
{% raw %}
```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectResumeCommand",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```
{% endraw %}

### See also
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [Controlling audio playback](/Develop/Guides/Implement_Client_Features.md#ControlAudioPlayback)

## ExpectStopCommand directive {#ExpectStopCommand}
Instructs the client to send the [`PlaybackController.StopCommandIssued`](#StopCommandIssued) event to CIC just like the effect of a user pressing the Stop button from the client device. Upon receiving this directive, the client must send the [`PlaybackController.StopCommandIssued`](#StopCommandIssued) event to CIC.

### Payload fields
None

### Message example
{% raw %}
```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectStopCommand",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```
{% endraw %}

### See also
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)
* [Controlling audio playback](/Develop/Guides/Implement_Client_Features.md#ControlAudioPlayback)

## Mute directive {#Mute}
Instructs the client to mute the audio player. Upon receiving this directive, the client must mute the speaker for the audio stream playback.

### Payload fields
None

### Remarks

If the control is related to speaker output, Clova does not provide a voice guide with the [`SpeechSynthesizer.Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak) directive. This is in consideration of the UX such as for a user listening to music. For this, you must implement an action to inform the user that the volume has been changed using the lights or a simple sound effect on the client.

### Message example
{% raw %}
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
{% endraw %}

### See also
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## Next directive {#Next}
Instructs the client to start playing the next audio stream in the playback queue. Upon receiving this directive, the client must play the next audio stream in queue.

### Payload fields
None

### Message example
{% raw %}
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
{% endraw %}

### See also
* [`PlaybackController.Previous`](#Previous)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## NextCommandIssued event {#NextCommandIssued}
Reports to CIC if the user presses the Next button on the client device or responds to the [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand) directive from CIC. Upon receiving the event, CIC sends the appropriate directive to the client.


### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | The client device ID. Skip this field if you are not controlling the media playback remotely. | Optional |

### Remarks
* The button on the client device can either be a physical button or a software button like a widget button on a music player.

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
      "namespace": "PlaybackController",
      "name": "NextCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}
```
{% endraw %}

### See also
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

## Pause directive {#Pause}
Instructs the client to pause the current audio stream. Upon receiving the directive, the client must pause playing the audio stream.

### Payload fields
None

### Message example
{% raw %}
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
{% endraw %}

### See also
* [`AudioPlayer.PlayPaused`](/Develop/References/CICInterface/AudioPlayer.md#PlayPaused)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
* [Controlling audio playback](/Develop/Guides/Implement_Client_Features.md#ControlAudioPlayback)

## PauseCommandIssued event {#PauseCommandIssued}
Reports to CIC if the user presses the Pause button on the client device or responds to the [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand) directive from CIC. Upon receiving the event, CIC sends the appropriate directive to the client.


### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | The client device ID. Skip this field if you are not controlling the media playback remotely. | Optional |

### Remarks
* The button on the client device can either be a physical button or a software button like a widget button on a music player.

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
      "namespace": "PlaybackController",
      "name": "PauseCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}
```
{% endraw %}

### See also
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectPauseCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)
* [Controlling audio playback](/Develop/Guides/Implement_Client_Features.md#ControlAudioPlayback)

## PlayCommandIssued event {#PlayCommandIssued}
Reports to CIC if the user presses the Play button on the client device or responds to the [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand) directive from CIC. Upon receiving the event, CIC sends the appropriate directive to the client. If the `handover` field exists is in the `payload` of the [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand) directive received from CIC, the client must take over the media playback using the field value.


### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`            | string  | The client device ID. Skip the `deviceId` field if you are not remotely handing over the media playback to another device. | Optional |
| `handover`            | object  | Object containing information required for remote takeover of media playback. If media playback must be taken over, use the `handover` object as the `handover` object in `payload` of the [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand) directive.     | Optional |
| `handover.deviceId`   | string  | The ID of the client device handing over media playback.  | Required |
| `handover.customData` | string  | The information required to play the media.               | Required |
| `token`               | string  | The token of the media to play. When the user presses the Play button after selecting a media on the playlist, the `playableItems[].token` field value of the [`TemplateRuntime.RenderPlayerInfo`](/Develop/References/CICInterface/TemplateRuntime.md#RenderPlayerInfo) directive must be applied to this field. Or, the value of the `token` field value may need to be used if the [`PlaybackController.ExpectPlayCommand`](#ExpectPlayCommand) directive is received.  | Optional  |

### Remarks
* The button on the client device can either be a physical button or a software button like a widget button on a music player.

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
      "namespace": "PlaybackController",
      "name": "PlayCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}
```
{% endraw %}

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
Instructs the client to start playing the previous audio stream in the playback queue. Upon receiving this directive, the client must play the previous audio stream.

### Payload fields
None

### Message example
{% raw %}
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
{% endraw %}

### See also
* [`PlaybackController.Next`](#Next)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)


## PreviousCommandIssued event {#PreviousCommandIssued}
Reports to CIC if the user presses the Previous button on the client device or responds to the [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand) directive from CIC. Upon receiving the event, CIC sends the appropriate directive to the client.


### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | The client device ID. Skip this field if you are not controlling the media playback remotely. | Optional |

### Remarks
* The button on the client device can either be a physical button or a software button like a widget button on a music player.

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
      "namespace": "PlaybackController",
      "name": "PreviousCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}
```
{% endraw %}

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
Instructs the client to replay the current audio stream from the beginning. Upon receiving this directive, the client must play the currently playing audio stream from the beginning. If playing had been paused, then resume playing the audio stream.

### Payload fields
None

### Message example
{% raw %}
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
{% endraw %}

### See also
* [`PlaybackController.Pause`](#Pause)
* [`PlaybackController.Resume`](#Resume)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
* [Controlling audio playback](/Develop/Guides/Implement_Client_Features.md#ControlAudioPlayback)

## Resume directive {#Resume}
Instructs the client to resume playing the audio stream. Upon receiving this directive, the client must resume playing the audio stream.

### Payload fields
None

### Message example
{% raw %}
```json
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
```
{% endraw %}

### See also
* [`AudioPlayer.PlayResumed`](/Develop/References/CICInterface/AudioPlayer.md#PlayResumed)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
* [Controlling audio playback](/Develop/Guides/Implement_Client_Features.md#ControlAudioPlayback)

## ResumeCommandIssued event {#ResumeCommandIssued}
Reports to CIC if the user presses the Resume button on the client device or responds to the [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand) directive from CIC. Upon receiving the event, CIC sends the appropriate directive to the client.


### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | The client device ID. Skip this field if you are not controlling the media playback remotely. | Optional |

### Remarks
* The button on the client device can either be a physical button or a software button like a widget button on a music player.

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
      "namespace": "PlaybackController",
      "name": "ResumeCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}
```
{% endraw %}

### See also
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectResumeCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)
* [Controlling audio playback](/Develop/Guides/Implement_Client_Features.md#ControlAudioPlayback)

## SetRepeatMode directive {#SetRepeatMode}

Instructs the client to change the playback state to a specified repeat mode. Upon receiving this directive, the client must change the state ([context object](/Develop/References/Context_Objects.md)) of the repeat mode as the specified value. The client must also set the `repeatMode` field of the [`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState) to the specified value when sending the event.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `repeatMode`  | string  | The selected repeat mode.<ul><li><code>NONE</code>: No repeat</li><code>REPEAT_ONE</code>: Repeat one song</li></ul>  | Always |

### Message example
{% raw %}
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
{% endraw %}

### See also
* [`PlaybackController.ExpectNextCommand`](#ExpectNextCommand)
* [`PlaybackController.ExpectPauseCommand`](#ExpectPauseCommand)
* [`PlaybackController.ExpectPreviousCommand`](#ExpectPreviousCommand)
* [`PlaybackController.ExpectResumeCommand`](#ExpectResumeCommand)
* [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)

## SetRepeatModeCommandIssued event {#SetRepeatModeCommandIssued}
Reports to CIC when the user presses the Repeat button on the client device. Upon receiving the event, CIC sends the [`PlaybackController.SetRepeatMode`](#SetRepeatMode) directive to the target client.


### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | The client device ID. Skip this field if you are not controlling the media playback remotely.                               | Optional |
| `repeatMode`  | string  | The selected repeat mode.<ul><li><code>NONE</code>: No repeat</li><code>REPEAT_ONE</code>: Repeat one song</li></ul>  | Required |

### Remarks
* The button on the client device can either be a physical button or a software button like a widget button on a music player.

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
{% endraw %}

### See also
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.SetRepeatMode`](#SetRepeatMode)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.StopCommandIssued`](#StopCommandIssued)

## Stop directive {#Stop}
Instructs the client to stop playing an audio stream. Upon receiving the directive, the client must stop playing the audio stream.

### Payload fields
None

### Message example
{% raw %}
```json
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
```
{% endraw %}

### See also
* [`AudioPlayer.PlayStopped`](/Develop/References/CICInterface/AudioPlayer.md#PlayStopped)
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
* [Controlling audio playback](/Develop/Guides/Implement_Client_Features.md#ControlAudioPlayback)

## StopCommandIssued event {#StopCommandIssued}
Reports to CIC if the user presses the Resume button on the client device or responds to the [`PlaybackController.ExpectStopCommand`](#ExpectStopCommand) directive from CIC. Upon receiving the event, CIC sends the appropriate directive to the client.


### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | The client device ID. Skip this field if you are not controlling the media playback remotely. | Optional |

### Remarks
* The button on the client device can either be a physical button or a software button like a widget button on a music player.

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
      "namespace": "PlaybackController",
      "name": "StopCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}
```
{% endraw %}

### See also
* [`PlaybackController.CustomCommandIssued`](#CustomCommandIssued)
* [`PlaybackController.ExpectResumeCommand`](#ExpectNextCommand)
* [`PlaybackController.NextCommandIssued`](#NextCommandIssued)
* [`PlaybackController.PauseCommandIssued`](#PauseCommandIssued)
* [`PlaybackController.PlayCommandIssued`](#PlayCommandIssued)
* [`PlaybackController.PreviousCommandIssued`](#PreviousCommandIssued)
* [`PlaybackController.ResumeCommandIssued`](#ResumeCommandIssued)
* [`PlaybackController.SetRepeatModeCommandIssued`](#SetRepeatModeCommandIssued)
* [Controlling audio playback](/Develop/Guides/Implement_Client_Features.md#ControlAudioPlayback)

## TurnOffRepeatMode directive {#TurnOffRepeatMode}
**(Deprecated)** Instructs the client to stop the repeat function for one song.

### Payload fields
None

### Message example
{% raw %}
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
{% endraw %}

### See also
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## TurnOnRepeatMode directive {#TurnOnRepeatMode}
**(Deprecated)** Instructs the client to start the repeat function for one song. Upon receiving the directive, the client must repeat the current audio stream.

### Payload fields
None

### Message example
{% raw %}
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
{% endraw %}

### See also
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## Unmute directive {#Unmute}
Instructs the client to unmute the audio player. Upon receiving the directive, the client must restore the volume level to before mute was applied.

### Payload fields
None

### Remarks

If the control is related to speaker output, Clova does not provide a voice guide with the [`SpeechSynthesizer.Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak) directive. This is in consideration of the UX such as for a user listening to music. For this, you must implement an action to inform the user that the volume has been changed using the lights or a simple sound effect on the client.

### Message example
{% raw %}
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
{% endraw %}

### See also
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## VolumeDown directive {#VolumeDown}
**(Deprecated)** Instructs the client to turn down the audio player volume. Upon receiving this directive, the client must turn down the volume of the audio player. The volume adjustment level depends on the client UX standard.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The <code>PlaybackController.VolumeDown</code> directive is to be deprecated. We recommend using the <a href="/Develop/References/CICInterface/DeviceControl.md#Decrease"><code>DiviceControl.Decrease</code></a>directive instead.</p>
</div>

### Payload fields
None

### Message example
{% raw %}
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
{% endraw %}

### See also
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)

## VolumeUp directive {#VolumeUp}

**(Deprecated)** Instructs the client to turn up the audio player volume. Upon receiving this directive, the client must turn up the volume of the audio player. The volume adjustment level depends on the client UX standard.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The <code>PlaybackController.VolumeUp</code> directive is to be deprecated. We recommend using the <a href="/Develop/References/CICInterface/DeviceControl.md#Increase"><code>DiviceControl.Increase</code></a>directive instead.</p>
</div>

### Payload fields
None

### Message example
{% raw %}
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
{% endraw %}

### See also
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
