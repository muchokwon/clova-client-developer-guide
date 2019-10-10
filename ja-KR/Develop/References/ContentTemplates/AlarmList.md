# AlarmListテンプレート
CICは、ユーザーがアラームのリストをリクエストすると、登録されているアラームのリストをAlarmListテンプレート形式でクライアントに送信します。クライアントは、このテンプレートを使って、ユーザーが登録したアラームのリストを画面に表示する必要があります。

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>AlarmListテンプレートには、現在、次のような制約事項があります。</p>
  <ul>
    <li>ユーザーは、音声を使って、アラームの登録とアラームのリストの照会のみリクエストできます。</li>
    <li>ユーザーがアラームを編集または削除するには、Clovaアプリを使用する必要があります。</li>
  </ul>
</div>

## Template fields

| フィールド名       | データ型    | 説明                     |
|---------------|---------|-----------------------------|
| `alarmList[]`               | object array  | ユーザーが登録したアラームのリストを持つオブジェクト配列                                                                                           |
| `alarmList[].repeatDay[]`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | 毎週繰り返しのアラームで、繰り返しの曜日を持つオブジェクト配列  |
| `alarmList[].repeatPeriod`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 繰り返しの周期を持つオブジェクトです。このオブジェクトの`value`フィールドは、以下の値を持ちます。<ul><li>空文字列（<code>""</code>）一回性のアラーム</li><li><code>"daily"</code>：毎日繰り返しのアラーム</li><li><code>"weekly"</code>：毎週繰り返しのアラーム</li></ul> |
| `alarmList[].scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | アラームを鳴らす日付と時間を持つオブジェクト                       |
| `alarmList[].token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | アラームの識別子を持つオブジェクト                               |
| `type`                      | string                                                                              | コンテンツテンプレートのタイプを示す値。`"AlarmList"`に固定されています。             |

## Template example

{% raw %}

```json
{
  "type": "AlarmList",
  "alarmList": [
    {
      "token": {
        "type": "string",
        "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-12-23T13:00:00Z"
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
        "value": "2017-12-24T00:00:00Z"
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
        "value": "2017-12-25T01:00:00Z"
      },
      "repeatPeriod": {
        "type": "string",
        "value": "weekly"
      },
      "repeatDay": [
        {
          "type": "string",
          "value": "saturday"
        },
        {
          "type": "string",
          "value": "sunday"
        }
      ]
    }
  ]
}
```

{% endraw %}

## UI example {#UIExample}

以下は、{{ book.ServiceEnv.OrientedService }}が配布したClovaのモバイルアプリで、AlarmListテンプレートの内容を表したUIサンプルです。

![](/Develop/Assets/Images/Content_Template-AlarmList.png)

## 次の項目も参照してください。
* [Alarm](/Develop/References/ContentTemplates/Alarm.md)
* [Alerts](/Develop/References/MessageInterfaces/Alerts.md)インターフェース
