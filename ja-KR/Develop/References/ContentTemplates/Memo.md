# Memoテンプレート
CICは、ユーザーがメモを作成すると、そのメモの情報をMemoテンプレート形式でクライアントに送信します。クライアントは、このテンプレートを使って、ユーザーが作成したメモを画面に表示する必要があります。

<div class="tip">
<p><strong>ヒント</strong></p>
<p>Memoテンプレートには、現在、次のような制約事項があります。</p>
<ul>
  <li>ユーザーは、音声を使って、メモの登録とメモのリストの照会のみリクエストできます。</li>
  <li>ユーザーがメモを編集または削除するには、Clovaアプリを使用する必要があります。</li>
</ul>
</div>

## Template fields

| フィールド名       | データ型    | 説明                     |
|---------------|---------|-----------------------------|
| `content`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | メモの内容を持つオブジェクト  |
| `timestamp`   | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | メモの作成時間を持つオブジェクト |
| `token`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 追加されたメモの識別子を持つオブジェクト  |
| `type`        | string                                                                              | コンテンツテンプレートのタイプを示す値。`"Memo"`を持ちます。             |

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
    "value": "私のWi-Fiのパスワード：12345678"
  },
  "timestamp": {
    "type": "datetime",
    "value": "2017-12-24T00:00:00Z"
  }
}
```

{% endraw %}

## UI example {#UIExample}

以下は、{{ book.ServiceEnv.OrientedService }}が配布したClovaのモバイルアプリで、Memoテンプレートの内容を表したUIサンプルです。

![](/Develop/Assets/Images/Content_Template-Memo.png)

## 次の項目も参照してください。
* [MemoList](/Develop/References/ContentTemplates/MemoList.md)
