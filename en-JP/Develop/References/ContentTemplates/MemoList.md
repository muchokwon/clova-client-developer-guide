# MemoList Template
The MemoList template is used in providing a list of memos for the client to display on the client screen. When the user requests a list of memos, CIC sends the list of memos registered by the user to the client in the form of the MemoList template.

<div class="note">
<p><strong>Note!</strong></p>
<p>The following restrictions apply when using the MemoList template:</p>
<ul>
  <li>Voice requests can be used only to add a memo or to check a list of memos.</li>
  <li>To modify or delete a memo, the user must use the Clova app.</li>
</ul>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `memoList[]`              | object array  | The object array that has the list of memos registered by the user.                                        |
| `memoList[].content`      | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The content of the memo.  |
| `memoList[].lastModified` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | The last modification time of the memo. |
| `memoList[].token`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | The ID of this memo.  |
| `type`                    | string                                                                              | The type of this template. The value is always `"MemoList"`.             |

## Template example

{% raw %}

```json
{
  "type": "MemoList",
  "memoList": [
    {
      "token": {
        "type": "string",
        "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
      },
      "content": {
        "type": "string",
        "value": "My Wi-Fi password: 12345678"
      },
      "lastModified": {
        "type": "datetime",
        "value": "2017-12-24T00:00:00Z"
      }
    },
    {
      "token": {
        "type": "string",
        "value": "b5403bd0-1598-495b-a466-9385c2b1103a"
      },
      "content": {
        "type": "string",
        "value": "To do list: Do homework, Make a girlfriend"
      },
      "lastModified": {
        "type": "datetime",
        "value": "2017-12-24T01:00:00Z"
      }
    },
    {
      "token": {
        "type": "string",
        "value": "da740e2a-01cd-4f2e-aedf-6c4285bae785"
      },
      "content": {
        "type": "string",
        "value": "Bucket list: Spend 10 billion dollars, Do absolutely nothing, Sleep for 72 hours"
      },
      "lastModified": {
        "type": "datetime",
        "value": "2017-12-24T02:00:00Z"
      }
    }
  ]
}
```

{% endraw %}

## UI example {#UIExample}

The following example shows how the MemoList template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

![](/Develop/Assets/Images/Content_Template-MemoList.png)

## See also
* [Memo](/Develop/References/ContentTemplates/Memo.md)
