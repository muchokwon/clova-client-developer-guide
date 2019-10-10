# Reminderテンプレート
CICは、ユーザーがリマインダーを作成すると、そのリマインダーの情報をReminderテンプレート形式でクライアントに送信します。クライアントは、このテンプレートを使って、ユーザーが作成したリマインダーの情報を画面に表示する必要があります。

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>Reminderテンプレートには、現在、次のような制約事項があります。</p>
  <ul>
    <li>ユーザーは、音声を使って、リマインダーの登録とリマインダーのリストの照会のみリクエストできます。</li>
    <li>ユーザーがリマインダーを編集または削除するには、Clovaアプリを使用する必要があります。</li>
  </ul>
</div>

## Template fields

| フィールド名       | データ型    | 説明                     |
|---------------|---------|-----------------------------|
| `content`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | **（Deprecated）** 追加されたリマインダーにユーザーが入力した内容を持つオブジェクト。将来、`label`フィールドに置き換えられる予定です。 |
| `label`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 追加されたリマインダーにユーザーが入力した内容を持つオブジェクト。 |
| `repeatDay[]`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | 毎週繰り返しのリマインダーの場合、繰り返しの曜日を持つオブジェクト配列 |
| `repeatPeriod`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 繰り返しの周期を持つオブジェクトです。このオブジェクトの`value`フィールドは、以下の値を持ちます。<ul><li>空文字列（<code>""</code>）一回性のリマインダー</li><li><code>"daily"</code>：毎日繰り返しのリマインダー</li><li><code>"weekly"</code>：毎週繰り返しのリマインダー</li></ul> |
| `status`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | リマインダーが処理されたかを示すオブジェクトです。このオブジェクトの`value`フィールドは、以下の値を持ちます。<ul><li><code>"TODO"</code>：未完了のリマインダー</li><li><code>"DONE"</code>：完了済みのリマインダー</li></ul> |
| `scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | リマインダーを設定する日付と時間を持つオブジェクト      |
| `token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 追加されたリマインダーの識別子を持つオブジェクト  |
| `type`          | string                                                                              | コンテンツテンプレートのタイプを示す値。`"Reminder"`を持ちます。  |

## Template example

{% raw %}

```json
// 一回性のリマインダー
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
    "value": "入金する"
  },
  "label": {
    "type": "string",
    "value": "入金する"
  },
  "status": {
    "type": "string",
    "value": "DONE"
  }
}

// 毎日繰り返しのリマインダー
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
    "value": "ビタミンを飲む"
  },
  "label": {
    "type": "string",
    "value": "ビタミンを飲む"
  },
  "status": {
    "type": "string",
    "value": "TODO"
  }
}

// 毎週繰り返しのリマインダー
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
    "value": "掃除をする"
  },
  "label": {
    "type": "string",
    "value": "掃除をする"
  },
  "status": {
    "type": "string",
    "value": "TODO"
  }
}
```

{% endraw %}

## UI example {#UIExample}

以下は、{{ book.ServiceEnv.OrientedService }}が配布したClovaのモバイルアプリで、Reminderテンプレートの内容を表したUIサンプルです。

![](/Develop/Assets/Images/Content_Template-Reminder.png)

## 次の項目も参照してください。
* [Alerts](/Develop/References/MessageInterfaces/Alerts.md)インターフェース
* [ReminderList](/Develop/References/ContentTemplates/ReminderList.md)
