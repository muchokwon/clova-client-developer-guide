# AlarmList Template
The AlarmList template is used in providing information of multiple alarms for the client to display on the client screen. When the user requests a list of alarms, CIC sends the list of alarms registered by the user to the client in the form of the AlarmList template.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>The following restrictions apply when using the AlarmList template:</p>
<ul>
  <li>Voice requests can be used only to add an alarm or to check a list of alarms.</li>
  <li>To modify or delete an alarm, the user must use the Clova app.</li>
</ul>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `alarmList[]`               | object array  | The object array that has the list of alarms registered by the user.                                                                                           |
| `alarmList[].repeatDay[]`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | The object array that has the repeat day(s) for a weekly alarm.  |
| `alarmList[].repeatPeriod`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The repeat cycle. Available values are: <ul><li>Empty string (<code>""</code>): One-time alarm</li><li><code>"daily"</code>: Daily alarm</li><li><code>"weekly"</code>: Weekly alarm</li></ul> |
| `alarmList[].scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | The date and time at which this alarm is to ring.                       |
| `alarmList[].token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The ID of this alarm.                               |
| `type`                      | string                                                                              | The type of this template. The value is always `"AlarmList"`.             |

## Template example

{% raw %}

```json
{
  "type": "AlarmList",
  "alarmList": [
    {
      "token": {
        "type": "string",
        "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-12-23T13:00:00Z"
      },
      "repeatPeriod": {
        "type": "string",
        "value": ""
      },
      "repeatDay": []
    },
    {
      "token": {
        "type": "string",
        "value": "b5403bd0-1598-495b-a466-9385c2b1103a"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-12-24T00:00:00Z"
      },
      "repeatPeriod": {
        "type": "string",
        "value": "daily"
      },
      "repeatDay": []
    },
    {
      "token": {
        "type": "string",
        "value": "da740e2a-01cd-4f2e-aedf-6c4285bae785"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-12-25T01:00:00Z"
      },
      "repeatPeriod": {
        "type": "string",
        "value": "weekly"
      },
      "repeatDay": [
        {
          "type": "string",
          "value": "saturday"
        },
        {
          "type": "string",
          "value": "sunday"
        }
      ]
    }
  ]
}
```

{% endraw %}

## UI example {#UIExample}

The following example shows how the AlarmList template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

![](/Develop/Assets/Images/Content_Template-AlarmList.png)

## See also
* [Alarm](/Develop/References/ContentTemplates/Alarm.md)
* [Alerts](/Develop/References/CICInterface/Alerts.md) interface
