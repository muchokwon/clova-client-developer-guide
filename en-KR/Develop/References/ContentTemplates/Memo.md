# Memo Template
The Memo template is used in providing memo information for the client to display on the client screen. When the user creates a memo, CIC sends the memo to the client in the form of the Memo template.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>The following restrictions apply when using the Memo template:</p>
<ul>
  <li>Voice requests can be used only to add a memo or to check a list of memos.</li>
  <li>To modify or delete a memo, the user must use the Clova app.</li>
</ul>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `content`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The content of the memo.  |
| `timestamp`   | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | The time at which this memo was created. |
| `token`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The ID of this memo.  |
| `type`        | string                                                                              | The type of this template. The value is always `"Memo"`.             |

## Template example

{% raw %}

```json
{
  "type": "Memo",
  "token": {
    "type": "string",
    "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
  },
  "content": {
    "type": "string",
    "value": "My Wi-Fi password: 12345678"
  },
  "timestamp": {
    "type": "datetime",
    "value": "2017-12-24T00:00:00Z"
  }
}
```

{% endraw %}

## UI example {#UIExample}

The following example shows how the Memo template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

![](/Develop/Assets/Images/Content_Template-Memo.png)

## See also
* [MemoList](/Develop/References/ContentTemplates/MemoList.md)
