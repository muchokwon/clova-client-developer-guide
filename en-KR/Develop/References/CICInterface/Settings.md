# Settings

The Settings namespace provides an interface to update or synchronize the client settings between Clova and the client. The Settings namespace provides the following event messages and directive messages.

| Message name         | Type  | Description                                 |
|------------------|-----------|-------------------------------------------|
| [`ExpectReport`](#ExpectReport) | Directive | Instructs the client to report the current settings information. Upon receiving the directive message, the client must send the [`Settings.Report`](#Report) event message to CIC. |
| [`Report`](#Report)             | Event     | The client reports the current settings information to CIC. If the [`Settings.ExpectReport`](#ExpectReport) directive message is received from CIC, the client must send the `Settings.Report` event message to CIC.  |
| [`Update`](#Update)             | Directive | Instructs the client to apply the values saved to `payload` as the setting value.  |

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>For more information on updating or synchronizing the settings, see <a href="/Develop/Guides/ImplementClientFeatures/Handle_Settings.md">Handling settings</a>.</p>
</div>

## ExpectReport directive {#ExpectReport}
Instructs the client to report the current settings information. Upon receiving the directive message, the client must send the [`Settings.Report`](#Report) event message to CIC.

### Payload fields

None

### Remarks

* This directive message is sent through a [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection), not as a response to an event message.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "Settings",
      "name": "ExpectReport",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524"
    },
    "payload": {}
  }
}
```

### See also
* [`Settings.Report`](#Report)
* [Handling settings](/Develop/Guides/ImplementClientFeatures/Handle_Settings.md)

## Report event {#Report}
The client reports the current settings information to CIC. If the [`Settings.ExpectReport`](#ExpectReport) directive message is received from CIC, the client must send the `Settings.Report` event message to CIC.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `configuration` | object | Object containing the predefined settings information of the client. All sub-fields of this object have a string type.<div class="tip"><p><strong>Tip!</strong></p><p>The settings datamay be defined differently for each client. For any inquiries about predefining the client settings information, contact the Clova partnership team.</p></div> | Required   |

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
      "namespace": "Settings",
      "name": "Report",
      "messageId": "b120c3e0-e6b9-4a3d-96de-71539e5f6214"
    },
    "payload": {
      "configuration": {
        "key1": "value1",
        "key2": "value2",
        ...
      }
    }
  }
}
```
{% endraw %}

### See also
* [`Settings.ExpectReport`](#ExpectReport)
* [Handling settings](/Develop/Guides/ImplementClientFeatures/Handle_Settings.md)

## Update directive {#Update}
Instructs the client to apply the values saved to `payload` as the setting value.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `configuration` | object | Object containing the predefined settings information of the client. All sub-fields of this object have a string type.<div class="tip"><p><strong>Tip!</strong></p><p>For any inquiries about predefining the client settings information, contact the Clova partnership team.</p></div> | Always   |

### Remarks

* This directive message is sent through a [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection), not as a response to an event message.

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Settings",
      "name": "Update",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524"
    },
    "payload": {
      "configuration": {
        "key1": "value1",
        "key2": "value2",
        ...
      }
    }
  }
}
```

{% endraw %}

### See also
* [`Settings.Report`](#Report)
* [Handling settings](/Develop/Guides/ImplementClientFeatures/Handle_Settings.md)
