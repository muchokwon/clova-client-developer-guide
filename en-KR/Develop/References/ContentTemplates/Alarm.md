# Alarm Template
The Alarm template is used in providing alarm information for the client to display on the client screen. When the user creates an alarm, CIC sends the created alarm information to the client in the form of the Alarm template.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>The following restrictions apply when using the Alarm template:</p>
<ul>
  <li>Voice requests can be used only to add an alarm or to check a list of alarms.</li>
  <li>To modify or delete an alarm, the user must use the Clova app.</li>
</ul>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `repeatDay[]`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | The object array that has the repeat day(s) for a weekly alarm.     |
| `repeatPeriod`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The repeat cycle. Available values of `value` field are: <ul><li>Empty String (<code>""</code>): One-time alarm </li><li><code>"daily"</code>: Daily alarm</li><li><code>"weekly"</code>: Weekly alarm</li></ul> |
| `scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | The date and time at which this alarm is to ring.                         |
| `token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The ID of this alarm.                            |
| `type`          | string                                                                              | The type of this template. The value is always `"Alarm"`.             |

## Template example

{% raw %}

```json

// One-time alarm
{
  "type": "Alarm",
  "token": {
    "type": "string",
    "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
  },
  "scheduledTime": {
    "type": "datetime",
    "value": "2017-10-09T09:00:00Z"
  },
  "repeatPeriod": {
    "type": "string",
    "value": ""
  },
  "repeatDay": []
}

// Daily alarm
{
  "type": "Alarm",
  "token": {
    "type": "string",
    "value": "b5403bd0-1598-495b-a466-9385c2b1103a"
  },
  "scheduledTime": {
    "type": "datetime",
    "value": "2017-10-09T09:00:00Z"
  },
  "repeatPeriod": {
    "type": "string",
    "value": "daily"
  },
  "repeatDay": []
}

// Weekly alarm
{
  "token": {
    "type": "string",
    "value": "da740e2a-01cd-4f2e-aedf-6c4285bae785"
  },
  "type": "Alarm",
  "scheduledTime": {
    "type": "datetime",
    "value": "2017-10-09T09:00:00Z"
  },
  "repeatPeriod": {
    "type": "string",
    "value": "weekly"
  },
  "repeatDay": [
    {
      "type": "string",
      "value": "monday"
    }
  ]
}
```

{% endraw %}

## UI example {#UIExample}

The following example shows how the Alarm template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

![](/Develop/Assets/Images/Content_Template-Alarm.png)

## See also
* [AlarmList](/Develop/References/ContentTemplates/AlarmList.md)
* [Alerts](/Develop/References/MessageInterfaces/Alerts.md) interface
