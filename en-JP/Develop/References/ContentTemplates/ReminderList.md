# ReminderList Template
The ReminderList template is used in providing information about multiple reminders for the client to display on the client screen. When the user requests for a list of reminders, CIC sends the list of reminders registered by the user to the client in the form of the ReminderList template.

<div class="note">
<p><strong>Note!</strong></p>
<p>The following restrictions apply when using the ReminderList template:</p>
<ul>
  <li>Voice requests can be used only to add a reminder or to check a list of reminders.</li>
  <li>To modify or delete a reminder, the user must use the Clova app.</li>
</ul>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `reminderList[]`               | object array  | The object array of reminders added by the user.                                                                                          |
| `reminderList[].content`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | **(Deprecated)** The content of the reminder added by the user. This object is scheduled to be replaced with the `label` field. |
| `reminderList[].label`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The message of the reminder added by the user. |
| `reminderList[].repeatDay[]`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | The object array that has the repeat day(s) for a weekly reminder. |
| `reminderList[].repeatPeriod`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The repeat cycle. Available values are: <ul><li>Empty string (<code>""</code>): One-time reminder</li><li><code>"daily"</code>: Daily reminder</li><li><code>"weekly"</code>: Weekly reminder</li></ul> |
| `reminderList[].status`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | Indicates whether the task of the reminder is completed or not. Available values are: <ul><li><code>"TODO"</code>: Reminder is not yet completed</li><li><code>"DONE"</code>: Reminder is completed</li></ul> |
| `reminderList[].scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | The date and time at which this reminder is to go off.      |
| `reminderList[].token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The ID of the reminder.  |
| `type`                         | string                                                                              | The type of this template. The value is always `"ReminderList"`.             |

## Template example

{% raw %}

```json
{
  "type": "ReminderList",
  "reminderList": [
    {
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
      "repeatDay": [],
      "content": {
        "type": "string",
        "value": "Transfer money"
      },
      "label": {
        "type": "string",
        "value": "Transfer money"
      },
      "status": {
        "type": "string",
        "value": "DONE"
      }
    },
    {
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
      "repeatDay": [],
      "content": {
        "type": "string",
        "value": "Take vitamins"
      },
      "label": {
        "type": "string",
        "value": "Take vitamins"
      },
      "status": {
        "type": "string",
        "value": "TODO"
      }
    },
    {
      "token": {
        "type": "string",
        "value": "da740e2a-01cd-4f2e-aedf-6c4285bae785"
      },
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
      ],
      "content": {
        "type": "string",
        "value": "Clean the house"
      },
      "label": {
        "type": "string",
        "value": "Clean the house"
      },
      "status": {
        "type": "string",
        "value": "TODO"
      }
    }
  ]
}
```

{% endraw %}

## UI example {#UIExample}

The following example shows how the ReminderList template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

![](/Develop/Assets/Images/Content_Template-ReminderList.png)

## See also
* [Alerts](/Develop/References/CICInterface/Alerts.md) interface
* [Reminder](/Develop/References/ContentTemplates/Reminder.md)
