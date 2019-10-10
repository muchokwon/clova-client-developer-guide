# ScheduleList Template
The ScheduleList template is used in providing information about multiple schedules for the client to display on the client screen. When the user requests a list of schedules, CIC sends the schedule list registered by the user to the client in the form of the ScheduleList template.

  <div class="tip">
  <p><strong>Tip!</strong></p>
  <p>The following restrictions apply when using the ScheduleList template:</p>
  <ul>
    <li>To register, modify, or delete the account for calendar, the user must use the Clova app.</li>
    <li>Voice requests can be used only to add a schedule or to check a list of schedules.</li>
    <li>To modify or delete a schedule, the user must use the Clova app.</li>
  </ul>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `scheduleList[]`        | object array | The object array that has the schedules registered by the user.   |
| `scheduleList[].content`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The content of schedule entered by the user. |
| `scheduleList[].end`           | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) or [DateObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateObject)  | The end date and time of this schedule. For an all-day schedule, the object type is DateObject containing the date of the schedule only. |
| `scheduleList[].start`         | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) or [DateObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateObject)  | The start date and time of this schedule. For an all-day schedule, the object type is DateObject containing the date of the schedule only. |
| `scheduleList[].repeatDay[]`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | The object array that has the repeat day(s) for a weekly schedule. |
| `scheduleList[].repeatPeriod`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The repeat cycle. Available values of `value` field are: <ul><li>Empty string (<code>""</code>): One-time schedule </li><li><code>"daily"</code>: Daily schedule</li><li><code>"weekly"</code>: Weekly schedule</li></ul> |
| `scheduleList[].token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The ID of the schedule.  |
| `type`        | string                                                                              | The type of this template. The value is always `"ScheduleList"`.             |

## Template example

{% raw %}

```json
{
  "type": "ScheduleList",
  "scheduleList": [
    {
      "type": "Schedule",
      "token": {
        "type": "string",
        "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
      },
      "start": {
        "type": "datetime",
        "dateTime": "2017-09-30T12:00:00+09:00"
      },
      "end": {
        "type": "datetime",
        "dateTime": "2017-09-30T13:00:00+09:00"
      },
      "content": {
        "type": "string",
        "value": "Kim's wedding"
      },
      "repeatPeriod": {
        "type": "string",
        "value": ""
      },
      "repeatDay": []
    },
    {
      "type": "Schedule",
      "token": {
        "type": "string",
        "value": "b5403bd0-1598-495b-a466-9385c2b1103a"
      },
      "start": {
        "type": "datetime",
        "dateTime": "2017-08-02T10:00:00+09:00"
      },
      "end": {
        "type": "datetime",
        "dateTime": "2017-08-02T11:00:00+09:00"
      },
      "content": {
        "type": "string",
        "value": "Daily scrum"
      },
      "repeatPeriod": {
        "type": "string",
        "value": "daily"
      },
      "repeatDay": []
    },
    {
      "type": "Schedule",
      "token": {
        "type": "string",
        "value": "da740e2a-01cd-4f2e-aedf-6c4285bae785"
      },
      "start": {
        "type": "datetime",
        "dateTime": "2017-08-02T10:00:00+09:00"
      },
      "end": {
        "type": "datetime",
        "dateTime": "2017-08-02T11:00:00+09:00"
      },
      "content": {
        "type": "string",
        "value": "Weekly meeting"
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
    },
    {
      "type": "Schedule",
      "token": {
        "type": "string",
        "value": "5c8b4f7b-d8bd-4817-a1c3-eb9c9522277e"
      },
      "start": {
        "type": "date",
        "dateTime": "2017-09-29"
      },
      "end": {
        "type": "datetime",
        "dateTime": "2017-09-29"
      },
      "content": {
        "type": "string",
        "value": "Play shop"
      },
      "repeatPeriod": {
        "type": "string",
        "value": ""
      },
      "repeatDay": []
    }
  ]
}
```

{% endraw %}

## UI example {#UIExample}

The following example shows how the ScheduleList template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

![](/Develop/Assets/Images/Content_Template-ScheduleList.png)

## See also
* [Alerts](/Develop/References/MessageInterfaces/Alerts.md) interface
* [ScheduleList](/Develop/References/ContentTemplates/ScheduleList.md)
