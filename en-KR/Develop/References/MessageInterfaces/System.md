# System

The System namespace provides directive messages and event messages that are required to synchronize system information such as firmware information of the client device between Clova and the client as follows:

| Message name         | Type  | Description                                 |
|------------------|-----------|-------------------------------------------|
| [`RequestSynchronizeState`](#RequestSynchronizeState)  | Event     | Reports to CIC that the client needs to synchronize information related to the system. |
| [`SynchronizeState`](#SynchronizeState)                | Directive | Instructs the client to synchronize the data in the `payload`.            |

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>The system namespace is currently being reorganized and scheduled to be updated soon.</p>
</div>

## RequestSynchronizeState event {#RequestSynchronizeState}
Reports to CIC that the client needs to synchronize information related to the system. CIC will send the [`System.SynchronizeState`](#SynchronizeState) directive message in return.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

None

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
      "namespace": "System",
      "name": "RequestSynchronizeState",
      "messageId": "b120c3e0-e6b9-4a3d-96de-71539e5f6214"
    },
    "payload": {}
  }
}
```
{% endraw %}

### See also
* [`System.SynchronizeState`](/Develop/References/MessageInterfaces/System.md#SynchronizeState)

## SynchronizeState directive {#SynchronizeState}
Instructs the client to synchronize the data in the `payload`. Upon receiving the directive message, the client should modify the set value according to the data from CIC.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `allAlerts[]`   | object array | **(Deprecated)** An object array that has a list of alarms to synchronize. It is in the same format as the [`payload`](/Develop/References/MessageInterfaces/Alerts.md#SetAlertPayload) object used in the [`Alerts.SetAlert`](/Develop/References/MessageInterfaces/Alerts.md#SetAlert) directive message. | Always    |

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Synchronization of alarm information through the <code>System.SynchronizeState</code> directive messages will no longer be supported. This feature will be supported through the <a href="/Develop/References/MessageInterfaces/Alerts.md#RequestSynchronizeAlert"><code>Alerts.RequestSynchronizeAlert</code></a> event message and the <a href="/Develop/References/MessageInterfaces/Alerts.md#SynchronizeAlert"><code>Alerts.SynchronizeAlert</code></a> directive message, and these messages must be used. In the future, a field to synchronize the system information will be added to the <code>System.SynchronizeState</code> directive message.</p>
</div>

### Remarks
More features are expected to be provided in the future.

### Message example

{% raw %}

```json
// Deprecated example
{
  "directive": {
    "header": {
      "namespace": "System",
      "name": "SynchronizeState",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524"
    },
    "payload": {
      "allAlerts": [
        {
          "type": "REMINDER",
          "token": "77179dbd-b65f-4341-a579-c1b2b97fc5b7",
          "scheduledTime": "2017-09-25T09:00:50+09:00",
          "assets": [
            {
              "assetId": "5141f693-5b39-46b7-80e4-3d71ed5508da",
              "url": "clova://alert/bell/reminder"
            },
            {
              "assetId": "b403ebe5-f911-4c5c-98b3-9f5320510235",
              "url": "https://abc.de.fe/tts2"
            }
          ],
          "assetPlayOrder": ["5141f693-5b39-46b7-80e4-3d71ed5508da", "b403ebe5-f911-4c5c-98b3-9f5320510235"]
        },
        {
          "type": "ALARM",
          "token": "ee4da70c-8328-4456-ab6f-c28cec626ae6",
          "scheduledTime": "2017-09-26T11:00:50+09:00"
        },
        ...
      ]
    }
  }
}
```

{% endraw %}

### See also
* [`System.RequestSynchronizeState`](#RequestSynchronizeState)
