# Handling alerts

Users can make a voice request to ask Clova to add an alarm. Once Clova receives such request, Clova registers the alarm or sends a directive message to the client to sound the alarm at the set time. And the client must be able to suitably handle all alarm-related messages sent by Clova for each situation.

This section explains the following:
* [Registering alarms](#RegisterAlert)
* [Starting alarms](#RingAlert)
* [Stopping alarms](#StopAlert)
* [Editing or deleting alarms](#EditAlert)
* [Synchronizing alarms](#SyncAlert)

<div class="note">
<p><strong>Note!</strong></p>
<p>Note that maintaining a network connection is crucial in providing the alarm service, as alarm information is provided by CIC.</p>
</div>

## Registering alarms {#RegisterAlert}

The flow of registering an alarm via user voice is as follows:

![](/Develop/Assets/Images/CIC_Alerts_Add_Work_Flow.svg)

When the user makes a request([`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)) for alarm registration via voice, Clova analyzes the user utterance and sends the [`Alerts.SetAlert`](/Develop/References/MessageInterfaces/Alerts.md#SetAlert) directive message to the user client for the client to add the alarm.

The client receives the `Alerts.SetAlert` directive message. Upon receipt, the client must register the alarm on the client device by checking the following details from this directive message:
* Alarm type
* Alarm time
* Alarm name or other details (optional)
* Alarm song (optional)

An example of an `Alerts.SetAlert` directive message that can be received is as follows:

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
          "url": "https://abc.de.fe/tts2"
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

The directive message above holds the information shown below. And the client must add an alarm based on this information.
* Alarm type: `REMINDER`
* Alarm time: `2017-09-25T09:00:50+09:00`
* Alarm name or other details: `Transfer money`
* Alarm song and order
  * First song to play: `clova://alert/bell/reminder`
  * Second song to play: `https://abc.de.fe/tts2`

The client attempts to register the alarm and then passes the result to CIC. If alarm registration was successful, the client sends an [`Alerts.SetAlertSucceeded`](/Develop/References/MessageInterfaces/Alerts.md#SetAlertSucceeded) event message to CIC.

```json
{
  "context": [
    ...
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

If alarm registration failed, the client sends an [`Alerts.SetAlertFailed`](/Develop/References/MessageInterfaces/Alerts.md#SetAlertFailed) event message.

```json
{
  "context": [
    ...
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

To inform the user of the request result, Clova sends the [`SpeechSynthesizer.Speak`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak) and the [`Clova.RenderTemplate`](/Develop/References/MessageInterfaces/Clova.md#RenderTemplate) directive messages to the client. The client must then send the details of this directive messages to the user.

## Starting alarms {#RingAlert}

When it becomes the set time, Clova sends a directive message to sound the alarm. The workflow for starting an alarm is explained below.

![](/Develop/Assets/Images/CIC_Alerts_Ring_Work_Flow.svg)

The client sounds the alarm at the set time and reports to CIC that the client has started the alarm with the [`Alerts.AlertStarted`](/Develop/References/MessageInterfaces/Alerts.md#AlertStarted) event message.

{% raw %}
```json
{
  "context": [
    ...
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

Once the alarm sounds, the client must provide the details of the ringing alarm in all event messages sent to CIC at this time. For this process, you must use the `activeAlerts` field of the [`Alert.AlertsState`](/Develop/References/Context_Objects.md#AlertsState) context.

## Stopping alarms {#StopAlert}

The user then requests to stop the alarm via voice([`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)) or via pressing a button on the client device or client app. Upon user request, the client must report the alarm stop request of the user to CIC as an [`Alerts.RequestAlertStop`](/Develop/References/MessageInterfaces/Alerts.md#RequestAlertStop) event message.

```json
{
  "context": [
    ...
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

Clova sends the [`Alerts.StopAlert`](/Develop/References/MessageInterfaces/Alerts.md#StopAlert) directive message to the client to stop the alarm.

The client receives the `Alerts.StopAlert` directive message. Upon receipt, the client must stop the alarm ringing on the client device by checking the following details from this directive message:
* ID of the alarm to stop
* Alarm type

An example of an `Alerts.StopAlert` directive message that can be received is as follows:

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

The client must stop the alarm with the same `token` and `type` values specified in the directive message.

Once the client stops the alarm, it must then send an [`Alerts.AlertStopped`](/Develop/References/MessageInterfaces/Alerts.md#AlertStopped) event message to CIC to report that the client has stopped the alarm.

```json
{
  "context": [
    ...
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

If the registered alarm is an action timer alarm, Clova sends a directive message corresponding to the user scheduled action to the client.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>For a repeated alarm, CIC informs the client only of the first upcoming alarm. Only after the given alarm has rung and stopped will the client get the information of the upcoming repeated alarms. If a client loses network connection for a prolonged period, repeated alarms will not work properly, as no succeeding alarm information would be received.</p>
</div>

<div class="tip">
<p><strong>Tip!</strong></p>
<p>Alarms can be changed or removed in the middle of the workflow but only through the Clova app. Note that any directive messages that are not a response to a voice request are designed to be forwarded to the client through a <a href="/Develop/Guides/Interact_with_CIC.md#CreateConnection">downchannel</a>.</p>
</div>

## Editing or deleting alarms {#EditAlert}

When a user edits or deletes an alarm from the Clova app, Clova sends an [`Alerts.SetAlert`](/Develop/References/MessageInterfaces/Alerts.md#SetAlert) directive message or an [`Alerts.DeleteAlert`](/Develop/References/MessageInterfaces/Alerts.md#DeleteAlert) directive accordingly to handle the user request.

For more information, see [Registering alerts](#RegisterAlert) as the process of editing an alarm is almost identical to the process of registering an alarm.

When the user requests to delete an alarm, the client receives the `Alerts.DeleteAlert` directive message. The client must then delete the alarm ringing on the client device by checking the following details from this directive message:
* ID of the alarm to delete
* Alarm type

An example of a `Alerts.DeleteAlert` directive message that can be received is as follows:

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

The client must delete the alarm with the same `token` and `type` values specified in the directive message.

The client performs alarm modification or deletion. If alarm deletion was successful, the client sends an [`Alerts.DeleteAlertSucceeded`](/Develop/References/MessageInterfaces/Alerts.md#DeleteAlertSucceeded) event message to CIC.

{% raw %}
```json
{
  "context": [
    ...
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

If alarm deletion failed, the client sends an [`Alerts.DeleteAlertFailed`](/Develop/References/MessageInterfaces/Alerts.md#DeleteAlertFailed) event message.

{% raw %}
```json
{
  "context": [
    ...
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

## Synchronizing alarms {#SyncAlert}

The client sends the [`System.RequestSynchronizeState`](/Develop/References/MessageInterfaces/System.md#RequestSynchronizeState) event message to CIC when: a new client is added, one or multiple clients are reconnected after a lost network connection, or a client is connected after a change in the user account registered to the client. The client then receives a [`System.SynchronizeState`](/Develop/References/MessageInterfaces/System.md#SynchronizeState) directive message from CIC and must synchronize the alarm information in the `allAlerts` field of the directive message with the device alarm information.

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
