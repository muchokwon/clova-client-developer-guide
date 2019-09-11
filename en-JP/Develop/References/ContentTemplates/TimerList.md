# TimerList Template
The TimerList template is used in providing information of multiple timers for the client to display on the client screen. When the user requests for a list of timers, CIC sends the list of timers registered by the user to the client in the form of the TimerList template.

<div class="note">
<p><strong>Note!</strong></p>
<p>The following restrictions apply when using the TimerList template:</p>
<ul>
  <li>Voice requests can be used only to add a timer or to check a list of timers.</li>
  <li>To modify or delete a timer, the user must use the Clova app.</li>
</ul>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `timerList[]`               | object array  | The object array of a list of timers registered by the user.                                                                                         |
| `timerList[].scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | The date and time at which the timer is to ring.                    |
| `timerList[].token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The ID of the timer.                             |
| `type`                      | string                                                                              | The type of this template. The value is always `"TimerList"`.      |

## Template example

{% raw %}

```json
{
  "type": "Timer",
  "timerList": [
    {
      "token": {
        "type": "string",
        "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-12-24T00:00:10Z"
      }
    },
    {
      "token": {
        "type": "string",
        "value": "da740e2a-01cd-4f2e-aedf-6c4285bae785"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-12-24T00:01:00Z"
      }
    }
  ]
}
```

{% endraw %}

## UI example {#UIExample}

The following example shows how the TimerList template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

![](/Develop/Assets/Images/Content_Template-TimerList.png)

## See also
* [Alerts](/Develop/References/CICInterface/Alerts.md) interface
* [Timer](/Develop/References/ContentTemplates/Timer.md)
