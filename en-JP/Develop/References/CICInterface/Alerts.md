# Alerts

The Alerts namespace provides interfaces for you to add, change, remove, start, or stop an alert. Available alarm types are as follows:

| Alarm type       | Description                                |
|---------------|------------------------------------|
| Action timer (`"ACTIONTIMER"`) | Performs a specified action after a set time has elapsed.                               |
| Alarm (`"ALARM"`)            | Sounds at a specified date and time.                                            |
| Reminder (`"REMINDER"`)      | Displays or reads content input by the user at a specified date and time. If the user makes a request to "Ring the alarm tomorrow at 7 p.m. to take the medicine," the alarm will be scheduled to ring at 7 p.m. and "Take the medicine" will be the alarm message. When it is time to ring the alarm, play, or display the alarm message provided by CIC.       |
| Timer (`"TIMER"`)           | Sounds after a specified time has elapsed.                                         |

A user can add an alarm by voice request or through the Clova app, but changing or removing the alarm is only available through the app. Clova stores alarm details on the cloud and informs clients to alert users through CIC. For repeated alarms, Clova informs the client only of the first upcoming alarm, instead of giving the bulk alarm information at once. After the first upcoming alarm rings and stops, Clova lets the client know of the next repeated alarm.

You must implement the following on a client, using the Alerts interfaces:
* Upon receiving a directive message from CIC, add, change, or remove the alarm as instructed by the directive message.
* Ring the alarm at the set time.
* Report to CIC when adding, changing, removing, starting, or stopping the alarm. Only then will a corresponding result be returned to the client from CIC.
* Include the [`Alerts.AlertsState`](/Develop/References/Context_Objects.md#AlertsState) context object in your event messages to report to CIC the alarm state of the client.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>For more information on how to implement registering, modifying, deleting, starting, or stopping alarms, see <a href="/Develop/Guides/Handle_Alerts.md">Handling alerts</a>.</p>
</div>

The Alerts namespace provides the following event messages and directive message.

| Message name         | Type  | Description                                   |
|------------------|-----------|---------------------------------------------|
| [`AlertStarted`](#AlertStarted)                 | Event     | Reports to CIC that the client has started ringing an alarm. |
| [`AlertStopped`](#AlertStopped)                 | Event     | Reports to CIC that the client has stopped ringing an alarm. |
| [`DeleteAlert`](#DeleteAlert)                   | Directive | Instructs the client to delete the specified alarm. |
| [`DeleteAlertFailed`](#DeleteAlertFailed)       | Event     | Reports to CIC that the client has failed to delete the specified alarm. |
| [`DeleteAlertSucceeded`](#DeleteAlertSucceeded) | Event     | Reports to CIC that the client has successfully deleted the specified alarm. |
| [`RequestAlertStop`](#RequestAlertStop)         | Event     | Requests CIC to stop the ringing alarm.  |
| [`RequestSynchronizeAlert`](#RequestSynchronizeAlert) | Event | Reports to CIC that the client needs to synchronize the alarm information of the user stored in Clova cloud. |
| [`SetAlert`](#SetAlert)                         | Directive | Instructs the client to add an alarm or to change the specified alarm. |
| [`SetAlertFailed`](#SetAlertFailed)             | Event     | Reports to CIC that the client has failed to add or change the specified alarm. |
| [`SetAlertSucceeded`](#SetAlertSucceeded)       | Event     | Reports to CIC that the client has successfully added or changed the specified alarm. |
| [`StopAlert`](#StopAlert)                       | Directive | Instructs the client to stop the specified alarm.  |
| [`SynchronizeAlert`](#SynchronizeAlert)         | Directive | Instructs the client to synchronize the alarm data of a user in the `payload` field.  |

Among the messages above, the [`RequestSynchronizeAlert`](#RequestSynchronizeAlert) event message and [`SynchronizeAlert`](#SynchronizeAlert) directive message are used when synchronizing information related to user accounts such as alarms and schedules between Clova and the client. Such synchronization is needed in the following cases:

* When a user has added another client to their account.
* When the client is reconnected to CIC due to a network error or other issues.
* When the account registered on the client has changed to a new user.
* When the client is reset after getting disconnected from the pair app.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>If the client experiences network disconnection or a user logs out from the client, you must delete the alarm information registered to the user account.</p>
</div>

## AlertStarted event {#AlertStarted}

Reports to CIC that the client has started ringing an alarm. Once the alarm starts, the client must send this event message to CIC.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | ID of the started alarm.                  | Required |
| `type`    | string | The alarm type. Available values are: <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | Required |

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
      "namespace": "Alerts",
      "name": "AlertStarted",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### See also
* [`Alerts.AlertStopped`](#AlertStopped)
* [Starting alarms](/Develop/Guides/Handle_Alerts.md#RingAlert)

## AlertStopped event {#AlertStopped}

Reports to CIC that the client has stopped ringing an alarm. Once the ringing alarm stops, the client must send this event message to CIC.
* **When a regular alarm is stopped,** the client receives a [`Alerts.DeleteAlert`](#DeleteAlert) directive from CIC.
* **When a repeated alarm is stopped,** the client receives a [`Alerts.SetAlert`](#SetAlert) directive from CIC to set the next repeated alarm.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | ID of the stopped alarm.                  | Required |
| `type`    | string | The alarm type. Available values are: <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | Required |

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
      "namespace": "Alerts",
      "name": "AlertStopped",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### See also
* [`Alerts.AlertStarted`](#AlertStarted)
* [Stopping alarms](/Develop/Guides/Handle_Alerts.md#StopAlert)

## DeleteAlert directive {#DeleteAlert}

Instructs the client to delete the specified alarm. Upon receipt, the client must delete the specified alarm and report the result to CIC using either one of the event messages below.

* [`Alerts.DeleteAlertSucceeded`](#DeleteAlertSucceeded) event: When the client has successfully deleted the specified alarm.
* [`Alerts.DeleteAlertFailed`](#DeleteAlertFailed) event: When the client has failed to delete the specified alarm.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | ID of the alarm to delete.              | Always |
| `type`    | string | The alarm type. Available values are: <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | Always |

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Alerts",
      "name": "DeleteAlert",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806",
      "dialogRequestId": "6b4061db-fbc1-45a2-9c54-b7c62d366b98"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### See also
* [`Alerts.DeleteAlertFailed`](#DeleteAlertFailed)
* [`Alerts.DeleteAlertSucceeded`](#DeleteAlertSucceeded)
* [Editing or deleting alarms](/Develop/Guides/Handle_Alerts.md#EditAlert)

## DeleteAlertFailed event {#DeleteAlertFailed}

Reports to CIC that the client has failed to delete the specified alarm. The client must send this event message to CIC if it fails to delete the specified alarm for the [Alerts.DeleteAlert](#DeleteAlert) directive.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | ID of the alarm the client has failed to delete.             | Required |
| `type`    | string | The alarm type. Available values are: <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | Required |

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
      "namespace": "Alerts",
      "name": "DeleteAlertFailed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### See also
* [`Alerts.DeleteAlert`](#DeleteAlert)
* [`Alerts.DeleteAlertSucceeded`](#DeleteAlertSucceeded)
* [Editing or deleting alarms](/Develop/Guides/Handle_Alerts.md#EditAlert)

## DeleteAlertSucceeded event {#DeleteAlertSucceeded}

Reports to CIC that the client has successfully deleted the specified alarm. The client must send this event message to CIC once it successfully deletes the specified alarm for the [Alerts.DeleteAlert](#DeleteAlert) directive message.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | ID of the deleted alarm.                  | Required |
| `type`    | string | The alarm type. Available values are: <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | Required |

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
      "namespace": "Alerts",
      "name": "DeleteAlertSucceeded",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### See also
* [`Alerts.DeleteAlert`](#DeleteAlert)
* [`Alerts.DeleteAlertFailed`](#DeleteAlertFailed)
* [Editing or deleting alarms](/Develop/Guides/Handle_Alerts.md#EditAlert)

## RequestAlertStop event {#RequestAlertStop}

Requests to CIC to stop the ringing alarm. The client must send this event message to CIC when the user stops the alarm, not with a voice command, but by pressing a button on the client device or the client app. CIC will send the [`Alerts.StopAlert`](#StopAlert) directive message as a response to this event message.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | ID of the alarm to stop.                  | Required |
| `type`    | string | The alarm type. Available values are: <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | Required |

### Remarks
Stopping a ringing alarm requires informing CIC of the stoppage and getting confirmation from CIC to stop. So, a user pressing a button to stop the alarm is not the end of the task. You need to send this event message to CIC and receive the [`Alerts.StopAlert`](#StopAlert) directive message. This process guarantees consistency in stopping an alarm and is helpful for synchronizing alarm information between clients and CIC. But, to provide a seamless UX to users, you have the option to remove the alarm display or mute the alarm while waiting for the [`Alerts.StopAlert`](#StopAlert) directive from CIC.

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
      "namespace": "Alerts",
      "name": "RequestAlertStop",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### See also
* [`Alerts.StopAlert`](#StopAlert)
* [Stopping alarms](/Develop/Guides/Handle_Alerts.md#StopAlert)

## RequestSynchronizeAlert event {#RequestSynchronizeAlert}

Reports to CIC that the client needs to synchronize the alarm information of the user stored in Clova cloud. CIC will send the [`Alerts.SynchronizeAlert`](#SynchronizeAlert) directive in return.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

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
      "namespace": "Alerts",
      "name": "RequestSynchronizeAlert",
      "messageId": "dd4f2794-6b14-4cc4-ae1b-5bfa1c469028"
    },
    "payload": {}
  }
}
```
{% endraw %}

### See also
* [`System.SynchronizeAlert`](/Develop/References/CICInterface/Alerts.md#SynchronizeAlert)
* [Synchronizing alarms](/Develop/Guides/Handle_Alerts.md#SyncAlert)

## SetAlert directive {#SetAlert}

Instructs the client to add an alarm or to change the specified alarm. Add an alarm or change the specified alarm, based on the following guide:

* Add an alarm **if the client has no alarm with the ID identical to the `token` field** of this directive.
* Make a change on the alarm **if the client already has an alarm with the ID identical to the `token` field** of this directive.

Report the result of executing the instruction to CIC using either one of the following event messages:

* [`Alerts.SetAlertSucceeded`](#SetAlertSucceeded) event: When the client has successfully added or changed or changed the specified alarm.
* [`Alerts.SetAlertFailed`](#SetAlertFailed) event: When the client has failed to add or change the specified alarm.

### Payload field {#SetAlertPayload}

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `assets[]`         | object array | The object array that has the TTS audio list to play when it is time to ring the reminder (`"REMINDER"`) type or action timer (`"ACTIONTIMER"`) alarm. This field is included only when the alarm is a reminder or an action timer.   | Conditional |
| `assets[].assetId` | string | ID of the TTS audio.       | Always |
| `assets[].url`     | string | The URL of the TTS audio. If the value of this field is in the `"clova://alert/bell/{type}"` format, the client must ring an appropriate ringtone from the client ringtones according to the alarm type (`type`). Available values are: <ul><li><code>"clova://alert/bell/reminder"</code>: Play a reminder ringtone</li></ul>    | Always |
| `assetPlayOrder[]` | string array | The string array defining the playback order of TTS audio in the `assets` fields. This array contains the TTS audio ID (`assets[].assetId`) to play in index order. This field is included only when the alarm is a reminder (`"REMINDER"`) or an action timer (`"ACTIONTIMER"`).  | Conditional  |
| `label`          | string | The details of the reminder or action timer.                             | Conditional |
| `scheduledTime`  | string | The set date and time for the alarm (YYYY-MM-DDThh:mm:ssZ).   | Always |
| `token`          | string | ID of the alarm to add or change.                        | Always |
| `type`           | string | The alarm type. Available values are: <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | Always |

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Alerts",
      "name": "SetAlert",
      "messageId": "9a440fa9-983a-48a8-8ad5-faee1250abde",
      "dialogRequestId": "688b051d-6832-4bfd-8cf8-5ff073cd2a82"
    },
    "payload": {
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
          "url": "http://abc.de.fe/tts2"
        }
      ],
      "label": " Transfer money",
      "assetPlayOrder": [
        "5141f693-5b39-46b7-80e4-3d71ed5508da",
        "b403ebe5-f911-4c5c-98b3-9f5320510235"
      ]
    }
  }
}
```

{% endraw %}

### See also
* [`Alerts.SetAlertFailed`](#SetAlertFailed)
* [`Alerts.SetAlertSucceeded`](#SetAlertSucceeded)
* [Registering alarms](/Develop/Guides/Handle_Alerts.md#RegisterAlert)
* [Editing or deleting alarms](/Develop/Guides/Handle_Alerts.md#EditAlert)

## SetAlertFailed event {#SetAlertFailed}

Reports to CIC that the client has failed to add or change the specified alarm. The client must send this event message to CIC if it fails to add or change the specified alarm for the [Alerts.SetAlert](#SetAlert) directive message.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | ID of the alarm the client has failed to add or change.     | Required |
| `type`    | string | The alarm type. Available values are: <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | Required |

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
      "namespace": "Alerts",
      "name": "SetAlertFailed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### See also
* [`Alerts.SetAlert`](#SetAlert)
* [`Alerts.SetAlertSucceeded`](#SetAlertSucceeded)
* [Registering alarms](/Develop/Guides/Handle_Alerts.md#RegisterAlert)
* [Editing or deleting alarms](/Develop/Guides/Handle_Alerts.md#EditAlert)

## SetAlertSucceeded event {#SetAlertSucceeded}

Reports to CIC that the client has successfully added or changed the specified alarm. Send this event message when you succeed in adding or changing the specified alarm for the [Alerts.SetAlert](#SetAlert) directive.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | ID of the added or changed alarm.          | Required |
| `type`    | string | The alarm type. Available values are: <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | Required |

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
      "namespace": "Alerts",
      "name": "SetAlertSucceeded",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### See also
* [`Alerts.SetAlert`](#SetAlert)
* [`Alerts.SetAlertFailed`](#SetAlertFailed)
* [Registering alarms](/Develop/Guides/Handle_Alerts.md#RegisterAlert)
* [Editing or deleting alarms](/Develop/Guides/Handle_Alerts.md#EditAlert)

## StopAlert directive {#StopAlert}

Instructs the client to stop the specified alarm. Upon receiving the directive, the client must stop the specified alarm and report the result to CIC with the [`AlertStopped`](#AlertStopped) event message.


### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | ID of the alarm to stop.              | Always |
| `type`    | string | The alarm type. Available values are: <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | Always |

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Alerts",
      "name": "StopAlert",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806",
      "dialogRequestId": "6b4061db-fbc1-45a2-9c54-b7c62d366b98"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### See also
* [`Alerts.AlertStopped`](#AlertStopped)
* [Stopping alarms](/Develop/Guides/Handle_Alerts.md#StopAlert)

## SynchronizeAlert directive {#SynchronizeAlert}
Instructs the client to synchronize the alarm data of a user in the `payload` field. Upon receiving the directive, the client should modify the set alarm values according to the data from CIC.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `allAlerts[]`   | object array | An object array that has the list of alarms for synchronization. The alarm information is specified in the format used in the [`payload`](#SetAlertPayload) of the [`Alerts.SetAlert`](#SetAlert) directive. | Always    |

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Alerts",
      "name": "SynchronizeAlert",
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
              "url": "http://abc.de.fe/tts2"
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
* [`Alerts.RequestSynchronizeAlert`](#RequestSynchronizeAlert)
* [Synchronizing alarms](/Develop/Guides/Handle_Alerts.md#SyncAlert)
