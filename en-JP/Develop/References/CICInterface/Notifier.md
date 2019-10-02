# Notifier

The Notifier namespace provides interfaces for CIC to instruct a client to turn on notification indicators or to turn off notification indicators on all the clients registered to the user account. For clients to keep the notification check status up to date, CIC sends the following directives, without having received events from clients:

| Message name         | Type  | Description                                   |
|------------------|-----------|---------------------------------------------|
| [`ClearIndicator`](#ClearIndicator)         | Directive | Instructs the client to turn off notification indicators.             |
| [`Notify`](#Notify)                         | Directive | Instructs the client to send notification details to the user.              |
| [`SetIndicator`](#SetIndicator)             | Directive | Instructs the client to display that there is a notification that the user has not read.      |

## ClearIndicator directive {#ClearIndicator}
Instructs the client to turn off notification indicators. The client must turn off all light indicators or sound effects for notification.

### Payload fields
None

### Remarks
This directive is sent through a [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection), which means that this directive is not a response to an event.

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Notifier",
      "name": "ClearIndicator",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524"
    },
    "payload": {}
  }
}
```

{% endraw %}

### See also
* [`Notifier.Notify`](#Notify)
* [`Notifier.SetIndicator`](#SetIndicator)


## Notify directive {#Notify}
Instructs the client to send notification details to the user. The client must turn on the light indicator by the specified method and play the audio related to the notification in the right order.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `assets[]`           | object array | The object array that has the audio content related to notifications.                          | Always |
| `assets[].assetId`   | string       | The ID of audio.                                        | Always |
| `assets[].url`       | string       | The URL of audio. The URL can be specified in the following ways:<ul><li><code>"clova://notifier/sound/default"</code>: A scheme used to refer to the default notification sound. The predefined default sound is played.</li><li>URL of audio (<code>"http(s)://~</code>): The URL of the audio that contains the notification details. The audio in the URL is played.</li></ul>    | Always |
| `assetPlayOrder[]`   | string array | The string array that expresses the order of playing the notification sound registered in the `assets[]` field. Play the notification sound in the order of the audio content ID stored in the array.            | Always  |
| `light`              | string       | The flash light settings.<ul><li><code>"DEFAULT"</code>: Turns on the light indicator.</li><li><code>"NONE"</code>: Does not turn on the light indicator.</li></ul>   | Always  |

### Remarks
This directive is sent through a [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection), which means that this directive is not a response to an event.

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Notifier",
      "name": "Notify",
      "messageId": "688a3d72-06c4-4628-b238-9ac454d3ea65",
    },
    "payload": {
      "light": "DEFAULT",
      "assets": [
        {
          "assetId": "e5179318-d061-42f9-af1b-417180142934",
          "url": "clova://notifier/sound/default"
        },
        {
          "assetId": "9d3df3c1-d0b4-4375-84a6-67c7ae000292",
          "url": "https://streaming.example.com/3325-b5c75045b4ae426885343f9b6abd0bfc-1508160634257"
        }
      ],
      "assetPlayOrder": [
        "e5179318-d061-42f9-af1b-417180142934",
        "9d3df3c1-d0b4-4375-84a6-67c7ae000292"
      ]
    }
  }
}
```

{% endraw %}

### See also
* [`Notifier.ClearIndicator`](#ClearIndicator)
* [`Notifier.SetIndicator`](#SetIndicator)
* [Sound](/Design/Sound.md)
* [Lights](/Design/Light.md)

## SetIndicator directive {#SetIndicator}
Instructs the client to display that there is a notification that the user has not read. Upon receipt, the client must turn on the light indicator or play the specified notification sound.

### Payload fields
| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `assets[]`           | object array | The object array that has the audio content related to notifications.                          | Always |
| `assets[].assetId`   | string       | The ID of audio.                                        | Always |
| `assets[].url`       | string       | The URL of audio. The URL can be specified in the following ways:<ul><li><code>"clova://notifier/sound/default"</code>: A scheme used to refer to the default notification sound. The predefined default sound is played.</li><li>URL of audio (<code>"http(s)://~</code>): The URL of the audio that contains the notification details. The audio in the URL is played.</li></ul>    | Always |
| `assetPlayOrder[]`   | string array | The string array that expresses the order of playing the notification sound registered in the `assets[]` field. Play the notification sound in the order of the audio content ID stored in the array.            | Always  |
| `new`                | boolean      | Indicates whether the notification to turn on the indicator is a new notification. <ul><li><code>true</code>: If the notification is new.</li><li><code>false</code>: If the notification is not new.</li></ul> | Always    |
| `light`              | string       | The flash light settings.<ul><li><code>"DEFAULT"</code>: Turns on the light indicator.</li><li><code>"NONE"</code>: Does not turn on the light indicator.</li></ul> | Always    |

### Remarks
This directive is sent through a [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection), which means that this directive is not a response to an event.

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Notifier",
      "name": "SetIndicator",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524"
    },
    "payload": {
      "assets": [{
        "assetId": "3ea201e8-135f-42fd-a75c-f125331ff9bd",
        "url": "clova://notifier/sound/default"
      }],
      "assetPlayOrder": ["3ea201e8-135f-42fd-a75c-f125331ff9bd"],
      "new": true,
      "light": "DEFAULT"
    }
  }
}
```

{% endraw %}

### See also
* [`Notifier.ClearIndicator`](#ClearIndicator)
* [`Notifier.Notify`](#Notify)
* [Sound](/Design/Sound.md)
* [Lights](/Design/Light.md)
