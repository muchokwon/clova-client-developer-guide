## Alerts.AlertsState {#AlertsState}
`Alerts.AlertsState` is a format for reporting to CIC about the alarm information of the client.

<div class="danger">
  <p><strong>Caution!</strong></p>
  <p>Fill in this context object with the alarm information provided by the <a href="/Develop/References/CICInterface/Alerts.html">Alerts</a> directive.</p>
</div>

### Object structure
{% raw %}

```json
{
  "header": {
    "namespace": "Alerts",
    "name": "AlertsState"
  },
  "payload": {
    "allAlerts": [
      {
        "token": {{string}},
        "type": {{string}},
        "scheduledTime": {{string}}
      }
    ],
    "activeAlerts": [
      {
        "token": {{string}},
        "type": {{string}},
        "scheduledTime": {{string}}
      }
    ]
  }
}
```

{% endraw %}


### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `allAlerts[]`    | [AlertInfoObject](#AlertInfoObject) array | The object array of the list of all alarms set on the client. Add information of all the alarms set on the client in this array.    | Required |
| `activeAlerts[]` | [AlertInfoObject](#AlertInfoObject) array | The object array of the list of alarms currently ringing on the client. Add an empty array if there is no alarm ringing.  | Required |

### Object example

{% raw %}

```json
{
  "context": [
    {
      "header": {
        "namespace": "Alerts",
        "name": "AlertsState"
      },
      "payload": {
        "allAlerts": [
          {
            "token": "78434957-c0db-47d9-9104-3d7899df3d4e",
            "type": "ALARM",
            "scheduledTime": "2017-09-23T11:11:11Z"
          },
          {
            "token": "26346794-0a23-446a-a34f-a0f5d957bd88",
            "type": "TIMER",
            "scheduledTime": "2017-09-14T22:22:22Z"
          },
          {
            "token": "5b3eb1a0-3a98-43ca-b28b-040462c2d2c9",
            "type": "ALARM",
            "scheduledTime": "2017-09-16T12:33:44Z"
          }
        ],
        "activeAlerts": [
          {
            "token": "5b3eb1a0-3a98-43ca-b28b-040462c2d2c9",
            "type": "ALARM",
            "scheduledTime": "2017-09-16T12:33:44Z"
          }
        ]
      }
    }
  ],
  "event": {
    "header": {
      "namespace": "Alerts",
      "name": "AlertStopped",
      "messageId": "b19028a9-e566-4ad9-90c0-4c5fecc9b336",
      "dialogRequestId": "862f996c-69cc-4e53-94df-9aed82b23579"
    },
    "payload": {
      "token": "26346794-0a23-446a-a34f-a0f5d957bd88",
      "type": "TIMER",
      "scheduledTime": "2017-09-14T22:22:22Z"
    }
  }
}
```

{% endraw %}

### AlertInfoObject {#AlertInfoObject}
This object contains a single alarm. Fill in the individual alarm details according to the format provided below.

#### Object fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `scheduledTime` | string | The set date and time for the alarm (YYYY-MM-DDThh:mm:ssZ).   | Required |
| `token`         | string | The ID of the alarm.                   | Required |
| `type`          | string | The alarm type. Available values are: <ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  | Required |

#### Object example

{% raw %}

```json
Example 1: ALARM type
{
  "token": "5b3eb1a0-3a98-43ca-b28b-040462c2d2c9",
  "type": "ALARM",
  "scheduledTime": "2017-09-16T12:33:44Z"
}

Example 2: TIMER type
{
  "token": "26346794-0a23-446a-a34f-a0f5d957bd88",
  "type": "TIMER",
  "scheduledTime": "2017-09-14T22:22:22Z"
}

Example 3: REMINDER type
{
  "token": "26346794-0a23-446a-a34f-a0f5d957bd88",
  "type": "TIMER",
  "scheduledTime": "2017-09-14T22:22:22Z"
}

```

{% endraw %}

### See also
* [`Alerts`](/Develop/References/CICInterface/Alerts.md)
