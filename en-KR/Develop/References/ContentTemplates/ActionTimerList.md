# ActionTimerList Template
The ActionTimerList template is used in providing a list of action timers for the client to display on the client screen. When the user requests a list of action timers, CIC sends the list of action timers registered by the user to the client in the form of the ActionTimerList template.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>The following restrictions apply when using the ActionTimerList template:</p>
<ul>
  <li>Voice requests can be used only to add an action timer or to check a list of action timers.</li>
  <li>To modify or delete an action timer, the user must use the Clova app.</li>
</ul>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `actionTimerList[]`               | object array  | The object array that has the action timers registered by the user.                                              |
| `actionTimerList[].action`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The action the user has set on this action timer. **The value is always an empty string (`""`). This field is reserved for future extension.** |
| `actionTimerList[].label`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The action the user has entered. |
| `actionTimerList[].repeatDay[]`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | The object array that has the repeat day(s) for a weekly action timer. |
| `actionTimerList[].repeatPeriod`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The repeat cycle. Available values are: <ul><li>Empty string (<code>""</code>): One-time action timer</li><li><code>"daily"</code>: Daily action timer</li><li><code>"weekly"</code>: Weekly action timer</li></ul> |
| `actionTimerList[].scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | The date and time at which this action timer is to go off.      |
| `actionTimerList[].token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The ID of this action timer.              |
| `type`        | string                                                                                                | The type of this template. The value is always `"ActionTimerList"`.             |

## Template example

{% raw %}

```json
{
  "type": "ActionTimerList",
  "actionTimerList": [
    {
      "token": {
        "type": "string",
        "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-10-01T14:00:00Z"
      },
      "action": {
        "type": "string",
        "value": ""
      },
      "label": {
        "type": "string",
        "value": "Play music"
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
        "value": "2017-10-02T09:00:00Z"
      },
      "action": {
        "type": "string",
        "value": ""
      },
      "label": {
        "type": "string",
        "value": "Play music"
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
        "value": "2017-10-03T11:00:00Z"
      },
      "action": {
        "type": "string",
        "value": ""
      },
      "label": {
        "type": "string",
        "value": "Play music"
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
  ]
}
```

{% endraw %}

## UI example {#UIExample}

The following example shows how the ActionTimerList template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

![](/Develop/Assets/Images/Content_Template-ActionTimerList.png)

## See also
* [ActionTimer](/Develop/References/ContentTemplates/ActionTimer.md)
* [Alerts](/Develop/References/CICInterface/Alerts.md) interface
