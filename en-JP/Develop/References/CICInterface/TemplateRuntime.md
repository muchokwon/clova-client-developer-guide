# TemplateRuntime

The TemplateRuntime namespace is used when the client or CIC requests or sends the playback metadata to display on the media player. The [`AudioPlayer`](/Develop/References/CICInterface/AudioPlayer.md) interface must be used when performing tasks related to the information required for the actual audio stream playback and the `TemplateRuntime` interface must be used when performing tasks related to the playback metadata information such as play list, album image, or lyrics. Through this, the client can retrieve the playback metadata of another client device as well as itself and also provide the information to the user.

| Message name         | Type  | Description                                   |
|------------------|-----------|---------------------------------------------|
| [`ExpectRequestPlayerInfo`](#ExpectRequestPlayerInfo)   | Directive | Instructs the client to request the playback metadata. Upon receiving the directive message, the client must send the [`TemplateRuntime.RequestPlayerInfo`](#RequestPlayerInfo) event message to CIC. |
| [`LikeCommandIssued`](#LikeCommandIssued)               | Event     | Reports to CIC when the user presses the Like button for specific media from the client device. |
| [`RenderPlayerInfo`](#RenderPlayerInfo)                 | Directive | Instructs the client to display the sent playback metadata such as a playlist, album image, and lyrics on the media player. |
| [`RequestPlayerInfo`](#RequestPlayerInfo)               | Event     | Requests CIC for playback metadata such as a playlist, album image, and lyrics to display on the media player. |
| [`SubscribeCommandIssued`](#SubscribeCommandIssued)     | Event     | Reports to CIC when the user presses the Subscribe button for specific media from the client device.  |
| [`UnlikeCommandIssued`](#UnlikeCommandIssued)           | Event     | Reports to CIC when the user presses the Unlike button for specific media from the client device. |
| [`UnsubscribeCommandIssued`](#UnsubscribeCommandIssued) | Event     | Reports to CIC when the user presses the Unsubscribe button for specific media from the client device. |
| [`UpdateLike`](#UpdateLike)                             | Directive | Instructs the client to update the Like status of a media to a specific value depending on whether the user gives a Like from a media player.  |
| [`UpdateSubscribe`](#UpdateSubscribe)                   | Directive | Instructs the client to update the Subscribe status of a media to a specific value depending on whether the user decides to subscribe from a media player.  |

## ExpectRequestPlayerInfo directive {#ExpectRequestPlayerInfo}

Instructs the client to request playback metadata such as play list, album image, and lyrics. Upon receiving the directive message, the client must send the [`TemplateRuntime.RequestPlayerInfo`](#RequestPlayerInfo) event message to CIC.

### Payload fields

None

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "ExpectRequestPlayerInfo",
      "dialogRequestId": "9d7dc3ca-17ff-4df9-9800-91736ba2a3b6",
      "messageId": "46ccdf6c-609a-43a5-91c4-7a43b961f0c0"
    },
    "payload": {}
  }
}
```

{% endraw %}

### See also
* [`TemplateRuntime.RequestPlayerInfo`](#RequestPlayerInfo)

## LikeCommandIssued event {#LikeCommandIssued}
Reports to CIC when the user presses the Like button for specific media from the client device. Upon receiving the event message, CIC sends the appropriate directive message to the client.


### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `token`    | string  | The token of the media content. Make sure to enter the token value provided in the `playableItems[].token` field of the [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) directive message. | Required |

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
      "namespace": "TemplateRuntime",
      "name": "LikeCommandIssued",
      "messageId": "2fcb6a62-393d-46ad-a5c4-b3db9b640045"
    },
    "payload": {
      "token": "eJyr5lIqSSyITy4tKs4vUrJSUE"
    }
  }
}
```
{% endraw %}

### See also
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.UnlikeCommandIssued`](#UnlikeCommandIssued)
* [`TemplateRuntime.UpdateLike`](#UpdateLike)

## RenderPlayerInfo directive {#RenderPlayerInfo}

Instructs the client to display the sent playback metadata such as a playlist, album image, and lyrics on the media player. When the user requests to play music, the client plays the media by receiving the [`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play) directive message. If necessary, a client with a display may have to express information related to playback on the media player. For this process, the playback metadata can be requested from CIC using the [`TemplateRuntime.RequestPlayerInfo`](#RequestPlayerInfo) event message and the `TemplateRuntime.RenderPlayerInfo` directive message is returned. The `TemplateRuntime.RenderPlayerInfo` directive message contains playback metadata on the media to play now and media to play later. The client is able to display metadata and play list of the currently playing media by providing the playback metadata of the `TemplateRuntime.RenderPlayerInfo` directive message to the user. It also provides the base data (`token`) that can process the user request to play specific media in the playlist, or to perform actions such as Like ([`TemplateRuntime.LikeCommandIssued`](#LikeCommandIssued)) or Unlike ([`TemplateRuntime.UnlikeCommandIssued`](#UnlikeCommandIssued)).

### Payload fields
| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `controls[]`                | object array | The object array that has the button information that the client must display on the media player.             | Always |
| `controls[].enabled`        | boolean      | Indicates whether the buttons specified in `controls[].name` must be enabled from the media player.<ul><li><code>true</code>: Enable</li><li><code>false</code>: Disable</li></ul>  | Always  |
| `controls[].name`           | string       | The button name. Available values are:<ul><li><code>"NEXT"</code>: Next</li><li><code>"PLAY_PAUSE"</code>: Play/Pause</li><li><code>"PREVIOUS"</code>: Previous</li></ul>  | Always  |
| `controls[].selected`       | boolean      | Indicates whether the media content is selected. This value can be used for displaying user preferences. For example, if this value is set as `true`, the content must be expressed on the relevant UI of the media player since the user has selected it as a preference. <ul><li><code>true</code>: Selected</li><li><code>false</code>: Not selected</li></ul> | Always  |
| `controls[].type`           | string       | The type of button. Currently, only the `"BUTTON"` value is available.  | Always |
| `displayType`               | string | Display format of the media content.<ul><li><code>"list"</code>: Display a list</li><li><code>"single"</code>: Display a single item</li></ul>       | Always |
| `playableItems[]`           | object array | The object containing the list of media content that can be played. This field can be an empty array.  | Always |
| `playableItems[].artImageUrl`  | string    | The URI of the image on the media content. This URI is the location of the album cover image or other relevant icons.      | Conditional |
| `playableItems[].controls[]`                | object array  | The object array of button information that must be displayed when playing a specific media content. This object array is omissible.  | Conditional |
| `playableItems[].controls[].enabled`        | boolean      | Indicates whether the buttons specified in `playableItems[].controls[].name` must be enabled from the media player.<ul><li><code>true</code>: Enable</li><li><code>false</code>: Disable</li></ul>  | Always  |
| `playableItems[].controls[].name`           | string       | The name of a button or a control UI. Available values are:<ul><li><code>"BACKWARD_15S"</code>: Rewind 15 seconds</li><li><code>"BACKWARD_30S"</code>: Rewind 30 seconds</li><li><code>"BACKWARD_60S"</code>: Rewind 60 seconds</li><li><code>"FORWARD_15S"</code>: Fast forward 15 seconds</li><li><code>"FORWARD_30S"</code>: Fast forward 30 seconds</li><li><code>"FORWARD_60S"</code>: Fast forward 60 seconds</li><li><code>"NEXT"</code>: Next</li><li><code>"PLAY_PAUSE"</code>: Play/Pause</li><li><code>"PREVIOUS"</code>: Previous</li><li><code>"PROGRESS_BAR"</code>: Progress Bar</li><li><code>"REPEAT"</code>: Repeat</li><li><code>"SUBSCRIBE_UNSUBSCRIBE"</code>: Subscribe/Unsubscribe</li></ul>  | Always  |
| `playableItems[].controls[].selected`       | boolean      | Indicates whether the media content is selected. This value can be used for displaying user preferences. For example, if this value is set as `true`, the content must be expressed on the relevant UI of the media player since the user has selected it as a preference. <ul><li><code>true</code>: Selected</li><li><code>false</code>: Not selected</li></ul> | Always  |
| `playableItems[].controls[].type`           | string       | The type of button. Currently, only the `"BUTTON"` value is available.  | Always |
| `playableItems[].headerText`       | string        | The text field used mainly to indicate the title of current play list.                                                | Conditional  |
| `playableItems[].isLive`           | boolean       | Indicates whether the content is a real-time content.<ul><li><code>true</code>: Real-time content</li><li><code>false</code>: Not a real-time content</li></ul><div class="note"><p><strong>Note!</strong></p><p>If the content is a real-time content, you must display an icon to indicate its state (e.g., a live icon).</p></div>  | Conditional  |
| `playableItems[].lyrics[]`         | object array  | The object array containing the lyrics information.                                                            | Conditional  |
| `playableItems[].lyrics[].data`    | string        | The lyrics data. Either this field or the `playableItems[].lyrics[].url` field always exists.              | Conditional  |
| `playableItems[].lyrics[].format`  | string        | The format of the lyrics data.<ul><li><code>"LRC"</code>: <a href="https://en.wikipedia.org/wiki/LRC_(file_format)" target="_blank">LRC format</a></li><li><code>"PLAIN"</code>: Plain text format</li></ul>  | Always  |
| `playableItems[].lyrics[].url`     | string        | The URI of the lyrics data. Either this field or the `playableItems[].lyrics[].data` field always exists.        | Conditional  |
| `playableItems[].showAdultIcon`    | boolean       | Indicates whether to display the icon for adult media.<ul><li><code>true</code>: Must be displayed.</li><li><code>false</code>: Must not be displayed.</li></ul>   | Always  |
| `playableItems[].titleSubText1`    | string        | The text field used mainly to indicate the name of the artist.                                                          | Always |
| `playableItems[].titleSubText2`    | string        | The text field used mainly to indicate the name of the album.                                                      | Conditional |
| `playableItems[].titleText`        | string        | The text field used mainly to indicate the title of the currently playing music.                                                         | Always  |
| `playableItems[].token`            | string        | The token of the media content.                                                                     | Always |
| `provider`                         | object        | The information of the media content provider.                                                         | Conditional |
| `provider.logoUrl`                 | string        | The URI for the logo of the media content provider. If this field is unavailable or undefined, or if the logo cannot be displayed, you must at least display the name of the media content provider specified in the `provider.name` field. | Conditional |
| `provider.name`                    | string        | The name of the media content provider.                                                                   | Always  |
| `provider.smallLogoUrl`            | string        | The URI for the small logo of the media content provider.                                                | Conditional |

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "RenderPlayerInfo",
      "dialogRequestId": "34abac3-cb46-611c-5111-47eab87b7",
      "messageId": "ad13f0d6-bb11-ca23-99aa-312a0b213805"
    },
    "payload": {
      "controls": [
        {
          "enabled": true,
          "name": "PLAY_PAUSE",
          "selected": false,
          "type": "BUTTON"
        },
        {
          "enabled": true,
          "name": "NEXT",
          "selected": false,
          "type": "BUTTON"
        },
        {
          "enabled": true,
          "name": "PREVIOUS",
          "selected": false,
          "type": "BUTTON"
        }
      ],
      "displayType": "list",
      "playableItems": [
        {
          "artImageUrl": "http://musicmeta.musicservice.example.com/example/album/662058.jpg",
          "controls": [
            {
              "enabled": true,
              "name": "LIKE_DISLIKE",
              "selected": false,
              "type": "BUTTON"
            }
          ],
          "headerText": "Classic",
          "lyrics": [
            {
              "data": null,
              "format": "PLAIN",
              "url": null
            }
          ],
          "isLive": false,
          "showAdultIcon": false,
          "titleSubText1": "Alice Sara Ott, Symphonie Orchester Des Bayerischen Rundfunks, Esa-Pekka Salonen",
          "titleSubText2": "Wonderland - Edvard Grieg : Piano Concerto, Lyric Pieces",
          "titleText": "Grieg : Piano Concerto In A Minor, Op.16 - 3. Allegro moderato molto e marcato (Live)",
          "token": "eJyr5lIqSSyITy4tKs4vUrJSUE="
        },
        {
          "artImageUrl": "http://musicmeta.musicservice.example.com/example/album/202646.jpg",
          "controls": [
            {
              "enabled": true,
              "name": "LIKE_DISLIKE",
              "selected": false,
              "type": "BUTTON"
            }
          ],
          "headerText": "Classic",
          "lyrics": [
            {
              "data": null,
              "format": "PLAIN",
              "url": null
            }
          ],
          "isLive": true,
          "showAdultIcon": false,
          "titleSubText1": "Berliner Philharmoniker, Herbert Von Karajan",
          "titleSubText2": "Mendelssohn : Violin Concerto; A Midsummer Night`s Dream",
          "titleText": "Symphony No.4 In A Op.90 'Italian' - III. Con Moto Moderato",
          "token": "eJyr5lIqSSyITy4tKs4vUrJSUEo2"
        },
        ...
      ],
      "provider": {
        "logoUrl": "https://img.musicservice.example.net/logo_180125.png",
        "name": "SampleMusicProvider",
        "smallLogoUrl": "https://img.musicservice.example.net/smallLogo_180125.png"
      }
    }
  }
}
```

{% endraw %}

### See also
* [`AudioPlayer.Play`](#Play)
* [`TemplateRuntime.LikeCommandIssued`](#LikeCommandIssued)
* [`TemplateRuntime.RequestPlayerInfo`](#RequestPlayerInfo)
* [`TemplateRuntime.SubscribeCommandIssued`](#SubscribeCommandIssued)
* [`TemplateRuntime.UnlikeCommandIssued`](#UnlikeCommandIssued)
* [`TemplateRuntime.UnsubscribeCommandIssued`](#UnsubscribeCommandIssued)

## RequestPlayerInfo event {#RequestPlayerInfo}
Requests CIC for playback metadata such as a playlist, album image, and lyrics to display on the media player. Upon receiving the event message, the CIC must send the [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) directive message to the client.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `range`        | object  | The scope of the playback metadata. If this field is empty, the client will receive a random number of metadata.   | Optional  |
| `range.after`  | number  | Requests n number of playback metadata included in the next playlist from the existing media content. For example, if the value of `range.after` is set as `5` without specifying the value of `range.before` field, the playback metadata equivalent to a total of six media content, including the base media content, is received. | Optional  |
| `range.before` | number  | Requests n number of playback metadata included in the previous playlist from the base media content.  | Optional  |
| `token`        | string  | The token of the media content which becomes the starting standard when importing the playback metadata. Make sure to enter the token value provided in the `playableItems[].token` field of the [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) directive message. | Required |

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
      "namespace": "TemplateRuntime",
      "name": "RequestPlayerInfo",
      "messageId": "2fcb6a62-393d-46ad-a5c4-b3db9b640045"
    },
    "payload": {
      "token": "eJyr5lIqSSyITy4tKs4vUrJSUE"
    }
  }
}
```
{% endraw %}

### See also
* [`TemplateRuntime.ExpectRequestPlayerInfo`](#ExpectRequestPlayerInfo)
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)

## SubscribeCommandIssued event {#SubscribeCommandIssued}
Reports to CIC when the user presses the Subscribe button for specific media from the client device. Upon receiving the event message, CIC sends the appropriate directive message to the client.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `token`    | string  | The token of the media content. Make sure to enter the token value provided in the `playableItems[].token` field of the [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) directive message. | Required |

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
      "namespace": "TemplateRuntime",
      "name": "SubscribeCommandIssued",
      "messageId": "ec3deb51-2ed8-47ae-ac17-d2ce24370f8f"
    },
    "payload": {
      "token": "SSyITy4teJyr5lIqKs4vUrJSUE"
    }
  }
}
```
{% endraw %}

### See also
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.UnsubscribeCommandIssued`](#UnsubscribeCommandIssued)
* [`TemplateRuntime.UpdateSubscribe`](#UpdateSubscribe)

## UnlikeCommandIssued event {#UnlikeCommandIssued}
Reports to CIC when the user presses the Unlike button for specific media from the client device. Upon receiving the event message, CIC sends the appropriate directive message to the client.


### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `token`    | string  | The token of the media content. Make sure to enter the token value provided in the `playableItems[].token` field of the [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) directive message. | Required |

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
      "namespace": "TemplateRuntime",
      "name": "UnlikeCommandIssued",
      "messageId": "2fcb6a62-393d-46ad-a5c4-b3db9b640045"
    },
    "payload": {
      "token": "eJyr5lIqSSyITy4tKs4vUrJSUE"
    }
  }
}
```
{% endraw %}

### See also
* [`TemplateRuntime.LikeCommandIssued`](#LikeCommandIssued)
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.UpdateLike`](#UpdateLike)

## UnsubscribeCommandIssued event {#UnsubscribeCommandIssued}
Reports to CIC when the user presses the Unsubscribe button for specific media from the client device. Upon receiving the event message, CIC sends the appropriate directive message to the client.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `token`    | string  | The token of the media content. Make sure to enter the token value provided in the `playableItems[].token` field of the [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) directive message. | Required |

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
      "namespace": "TemplateRuntime",
      "name": "UnsubscribeCommandIssued",
      "messageId": "04deb09e-54cc-4525-9e97-4ff4168872b5"
    },
    "payload": {
      "token": "r5lIqKs4vUSSyITy4teJyrJSUE"
    }
  }
}
```
{% endraw %}

### See also
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.SubscribeCommandIssued`](#SubscribeCommandIssued)
* [`TemplateRuntime.UpdateSubscribe`](#UpdateSubscribe)

## UpdateLike directive {#UpdateLike}

Instructs the client to update the Like status of a media to a specific value depending on whether the user gives a Like from a media player. This directive message is sent as a response to a user utterance. Or, it is sent as a response to the action of pressing the Like button ([`TemplateRuntime.LikeCommandIssued`](#LikeCommandIssued)) or the Unlike button ([`TemplateRuntime.UnlikeCommandIssued`](#UnlikeCommandIssued)). It is also sent for synchronization purposes when the user performs actions related to Likes from another device.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `like`        | boolean | Indicates whether to display the Like status<ul><li><code>true</code>: Display Liked</li><li><code>false</code>: Do not display Liked</li></ul>             | Always |
| `token`       | string  | The token of the media content. The value corresponding to the `playableItems[].token` field of the [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) directive message is included in the token value.      | Always |

## Remarks

When this directive message is sent for synchronization purposes, it is sent through a [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection) and not as a response to an event message.

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "UpdateLike",
      "messageId": "fb65457e-bd2e-4876-b739-2b8cc5b67486",
      "dialogRequestId": "94e045dd-78c7-415e-b73b-f0cb9cbfd75d"
    },
    "payload": {
      "token": "r5lIqKs4vUSSyITy4teJyrJSUE",
      "like": true
    }
  }
}
```

{% endraw %}

### See also
* [`TemplateRuntime.LikeCommandIssued`](#LikeCommandIssued)
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.UnlikeCommandIssued`](#UnlikeCommandIssued)

## UpdateSubscribe directive {#UpdateSubscribe}

Instructs the client to update the Subscribe state of a specific media to a specific value depending on whether the user decides to subscribe from a media player. This directive message is sent as a response to a user utterance. Or, it is sent as a response to the action of pressing the Subscribe button ([`TemplateRuntime.SubscribeCommandIssued`](#SubscribeCommandIssued)) or the Unsubscribe button ([`TemplateRuntime.UnsubscribeCommandIssued`](#UnsubscribeCommandIssued)). It is also sent for synchronization purposes when the user performs actions related to Subscribe from another device.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `subscribe`   | boolean | Indicates the subscription status.<ul><li><code>true</code>: Subscribed</li><li><code>false</code>: Not subscribed</li></ul>             | Always |
| `token`       | string  | The token of the media content. The value corresponding to the `playableItems[].token` field of the [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo) directive message is included in the token value.      | Always |

## Remarks

When this directive message is sent for synchronization purposes, it is sent through a [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection) and not as a response to an event message.

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "UpdateSubscribe",
      "messageId": "fb65457e-bd2e-4876-b739-2b8cc5b67486",
      "dialogRequestId": "94e045dd-78c7-415e-b73b-f0cb9cbfd75d"
    },
    "payload": {
      "token": "r5lIqKs4vUSSyITy4teJyrJSUE",
      "subscribe": true
    }
  }
}
```

{% endraw %}

### See also
* [`TemplateRuntime.SubscribeCommandIssued`](#SubscribeCommandIssued)
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.UnsubscribeCommandIssued`](#UnsubscribeCommandIssued)
