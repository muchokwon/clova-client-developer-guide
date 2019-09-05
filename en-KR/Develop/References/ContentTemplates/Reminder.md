# Reminder Template
The Reminder template is used for providing reminder information for the client to display on the client screen.  When the user creates a reminder, CIC sends the reminder information to the client in the form of the Reminder template.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>The following restrictions apply when using the Reminder template:</p>
<ul>
  <li>Voice requests can be used only to add a reminder or to check a list of reminders.</li>
  <li>To modify or delete a reminder, the user must use the Clova app.</li>
</ul>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `content`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | **(Deprecated)** The content of the reminder added by the user. This object is scheduled to be replaced with the `label` field. |
| `label`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The message of the reminder added by the user. |
| `repeatDay[]`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | The object array that has the repeat day(s) for a weekly reminder. |
| `repeatPeriod`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The repeat cycle. Available values of the `value` field are: <ul><li>Empty string (<code>""</code>): One-time reminder</li><li><code>"daily"</code>: Daily reminder</li><li><code>"weekly"</code>: Weekly reminder</li></ul> |
| `status`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | Indicates whether the task of the reminder is completed or not. Available values of `value` field are: <ul><li><code>"TODO"</code>: Reminder is not yet completed</li><li><code>"DONE"</code>: Reminder is completed</li></ul> |
| `scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | The date and time at which this reminder is to go off.      |
| `token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The ID of the reminder.  |
| `type`          | string                                                                              | The type of this template. The value is always `"Reminder"`.  |

## Template example

{% raw %}

```json
// One-time reminder
{
  "type": "Reminder",
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
}

// Daily reminder
{
  "type": "Reminder",
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
}

// Weekly reminder
{
  "type": "Reminder",
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
```

{% endraw %}

## UI example {#UIExample}

The following example shows how the Reminder template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

![](/Develop/Assets/Images/Content_Template-Reminder.png)

## See also
* [Alerts](/Develop/References/MessageInterfaces/Alerts.md) interface
* [ReminderList](/Develop/References/ContentTemplates/ReminderList.md)
