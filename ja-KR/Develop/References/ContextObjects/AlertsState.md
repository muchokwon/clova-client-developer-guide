## Alerts.AlertsState {#AlertsState}
`Alerts.AlertsState`クライアントのアラームの情報をCICにレポートするときに使用されるメッセージ形式です。

<div class="note">
  <p><strong>メモ</strong></p>
  <p>このコンテクストを作成するとき、<a href="/Develop/References/CICInterface/Alerts.md">Alerts</a>のディレクティブから受け取ったアラームの情報をそのまま入力する必要があります。</p>
</div>

### Object structure
{% raw %}

```json
{
  "header": {
    "namespace": "Alerts",
    "name": "AlertsState"
  },
  "payload": {
    "allAlerts": [
      {
        "token": {{string}},
        "type": {{string}},
        "scheduledTime": {{string}}
      }
    ],
    "activeAlerts": [
      {
        "token": {{string}},
        "type": {{string}},
        "scheduledTime": {{string}}
      }
    ]
  }
}
```

{% endraw %}


### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `activeAlerts[]` | [AlertInfoObject](#AlertInfoObject) array | クライアントで現在鳴っているアラームのリストを持つオブジェクト配列。現在鳴っているアラームがない場合、空の配列を入力します。  |  |
| `allAlerts[]`    | [AlertInfoObject](#AlertInfoObject) array | クライアントに設定されているすべてのアラームのリストを持つオブジェクト配列。この配列に、クライアントが設定しているすべてのアラームの情報を入力する必要があります。    |  |

### Object example

{% raw %}

```json
{
  "context": [
    {
      "header": {
        "namespace": "Alerts",
        "name": "AlertsState"
      },
      "payload": {
        "allAlerts": [
          {
            "token": "78434957-c0db-47d9-9104-3d7899df3d4e",
            "type": "ALARM",
            "scheduledTime": "2017-09-23T11:11:11Z"
          },
          {
            "token": "26346794-0a23-446a-a34f-a0f5d957bd88",
            "type": "TIMER",
            "scheduledTime": "2017-09-14T22:22:22Z"
          },
          {
            "token": "5b3eb1a0-3a98-43ca-b28b-040462c2d2c9",
            "type": "ALARM",
            "scheduledTime": "2017-09-16T12:33:44Z"
          }
        ],
        "activeAlerts": [
          {
            "token": "5b3eb1a0-3a98-43ca-b28b-040462c2d2c9",
            "type": "ALARM",
            "scheduledTime": "2017-09-16T12:33:44Z"
          }
        ]
      }
    }
  ],
  "event": {
    "header": {
      "namespace": "Alerts",
      "name": "AlertStopped",
      "messageId": "b19028a9-e566-4ad9-90c0-4c5fecc9b336",
      "dialogRequestId": "862f996c-69cc-4e53-94df-9aed82b23579"
    },
    "payload": {
      "token": "26346794-0a23-446a-a34f-a0f5d957bd88",
      "type": "TIMER",
      "scheduledTime": "2017-09-14T22:22:22Z"
    }
  }
}
```

{% endraw %}

### AlertInfoObject {#AlertInfoObject}
各アラームの情報を持つオブジェクトです。各アラームの情報をこのオブジェクトの形式に合わせて入力します。

#### Object fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `scheduledTime` | string | アラームを鳴らす日付と時間の情報（YYYY-MM-DDThh:mm:ssZの形式）   |  |
| `token`         | string | アラームの識別子                   |  |
| `type`          | string | アラームの種類。以下の値を持ちます。<ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  |  |

#### Object example

{% raw %}

```json
例1：ALARMタイプ
{
  "token": "5b3eb1a0-3a98-43ca-b28b-040462c2d2c9",
  "type": "ALARM",
  "scheduledTime": "2017-09-16T12:33:44Z"
}

例2：TIMERタイプ
{
  "token": "26346794-0a23-446a-a34f-a0f5d957bd88",
  "type": "TIMER",
  "scheduledTime": "2017-09-14T22:22:22Z"
}

例3：REMINDERタイプ
{
  "token": "26346794-0a23-446a-a34f-a0f5d957bd88",
  "type": "TIMER",
  "scheduledTime": "2017-09-14T22:22:22Z"
}

```

{% endraw %}

### 次の項目も参照してください。
* [`Alerts`](/Develop/References/CICInterface/Alerts.md)
