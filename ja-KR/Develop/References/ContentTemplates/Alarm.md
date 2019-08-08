# Alarmテンプレート
CICは、ユーザーがアラームを作成すると、そのアラームの情報をAlarmテンプレート形式でクライアントに送信します。クライアントは、このテンプレートを使って、ユーザーが作成したアラームの情報を画面に表示する必要があります。

<div class="tip">
<p><strong>ヒント</strong></p>
<p>Alarmテンプレートには、現在、次のような制約事項があります。</p>
<ul>
  <li>ユーザーは、音声を使って、アラームの登録とアラームのリストの照会のみリクエストできます。</li>
  <li>ユーザーがアラームを編集または削除するには、Clovaアプリを使用する必要があります。</li>
</ul>
</div>

## Template fields

| フィールド名       | データ型    | 説明                     |
|---------------|---------|-----------------------------|
| `repeatDay[]`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | 毎週繰り返しのアラームで、繰り返しの曜日を持つオブジェクト配列     |
| `repeatPeriod`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 繰り返しの周期を持つオブジェクトです。このオブジェクトの`value`フィールドは、以下の値を持ちます。<ul><li>空文字列（<code>""</code>）：一回性のアラーム</li><li><code>"daily"</code>：毎日繰り返しのアラーム</li><li><code>"weekly"</code>：毎週繰り返しのアラーム</li></ul> |
| `scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | アラームを鳴らす日付と時間を持つオブジェクト                         |
| `token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 追加されたアラームの識別子を持つオブジェクト                            |
| `type`          | string                                                                              | コンテンツテンプレートのタイプを示す値。`"Alarm"`を持ちます。             |

## Template example

{% raw %}

```json

// 一回性のアラーム
{
  "type": "Alarm",
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
  "repeatDay": []
}

// 毎日繰り返しのアラーム
{
  "type": "Alarm",
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
  "repeatDay": []
}

// 毎週繰り返しのアラーム
{
  "token": {
    "type": "string",
    "value": "da740e2a-01cd-4f2e-aedf-6c4285bae785"
  },
  "type": "Alarm",
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
  ]
}
```

{% endraw %}

## UI example {#UIExample}

以下は、{{ book.ServiceEnv.OrientedService }}が配布したClovaのモバイルアプリで、Alarmテンプレートの内容を表したUIサンプルです。

![](/Develop/Assets/Images/Content_Template-Alarm.png)

## 次の項目も参照してください。
* [AlarmList](/Develop/References/ContentTemplates/AlarmList.md)
* [Alerts](/Develop/References/CICInterface/Alerts.md)インターフェース
