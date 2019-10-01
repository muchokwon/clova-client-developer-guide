# Timer Template
The Timer template is used in providing timer information for the client to display on the client screen. When the user creates a timer, CIC sends the timer information to the client in the form of the Timer template.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>The following restrictions apply when using the Timer template:</p>
<ul>
  <li>Voice requests can be used only to add a timer or to check a list of timers.</li>
  <li>To modify or delete a timer, the user must use the Clova app.</li>
</ul>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | The date and time at which the added timer is to ring.             |
| `token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The ID of the timer.                     |
| `type`          | string                                                                              | The type of this template. The value is always `"Timer"`.        |

## Template example

{% raw %}

```json
{
  "type": "Timer",
  "token": {
    "type": "string",
    "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
  },
  "scheduledTime": {
    "type": "datetime",
    "value": "2017-12-24T00:00:00Z"
  }
}
```

{% endraw %}

## UI example {#UIExample}

The following example shows how the Timer template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

![](/Develop/Assets/Images/Content_Template-Timer.png)

## See also
* [Alerts](/Develop/References/CICInterface/Alerts.md) interface
* [TimerList](/Develop/References/ContentTemplates/TimerList.md)
