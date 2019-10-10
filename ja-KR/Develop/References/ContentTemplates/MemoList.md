# MemoListテンプレート
CICは、ユーザーがメモのリストをリクエストすると、登録されているメモのリストをMemoListテンプレート形式でクライアントに送信します。クライアントは、このテンプレートを使って、ユーザーが登録したメモのリストを画面に表示する必要があります。

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>MemoListテンプレートには、現在、次のような制約事項があります。</p>
  <ul>
    <li>ユーザーは、音声を使って、メモの登録とメモのリストの照会のみリクエストできます。</li>
    <li>ユーザーがメモを編集または削除するには、Clovaアプリを使用する必要があります。</li>
  </ul>
</div>

## Template fields

| フィールド名       | データ型    | 説明                     |
|---------------|---------|-----------------------------|
| `memoList[]`              | object array  | ユーザーが登録したメモのリストを持つオブジェクト配列                                        |
| `memoList[].content`      | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | メモの内容を持つオブジェクト  |
| `memoList[].lastModified` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | メモの最終更新時間を持つオブジェクト。 |
| `memoList[].token`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | メモの識別子を持つオブジェクト  |
| `type`                    | string                                                                              | コンテンツテンプレートのタイプを示す値。`"MemoList"`を持ちます。             |

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
        "value": "私のWi-Fiのパスワード：12345678"
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
        "value": "ToDoリスト：宿題をする、彼女を作る"
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
        "value": "バケットリスト：100億を使ってみる、何もしないでいる、72時間寝る"
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

以下は、{{ book.ServiceEnv.OrientedService }}が配布したClovaのモバイルアプリで、MemoListテンプレートの内容を表したUIサンプルです。

![](/Develop/Assets/Images/Content_Template-MemoList.png)

## 次の項目も参照してください。
* [Memo](/Develop/References/ContentTemplates/Memo.md)
