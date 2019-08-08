# ActionTimerListテンプレート
CICは、ユーザーがアクションタイマーのリストをリクエストすると、登録されているアクションタイマーのリストをActionTimerListテンプレート形式でクライアントに送信します。クライアントは、このテンプレートを使って、ユーザーが登録したアクションタイマーのリストを画面に表示する必要があります。

<div class="tip">
<p><strong>ヒント</strong></p>
<p>ActionTimerListテンプレートには、現在、次のような制約事項があります。</p>
<ul>
  <li>ユーザーは、音声を使って、アクションタイマーの設定と、アクションタイマーのリストの照会のみリクエストできます。</li>
  <li>ユーザーがアクションタイマーを編集または削除するには、Clovaアプリを使用する必要があります。</li>
</ul>
</div>

## Template fields

| フィールド名       | データ型    | 説明                     |
|---------------|---------|-----------------------------|
| `actionTimerList[]`               | object array  | ユーザーが登録したアクションタイマーのリストを持つオブジェクト配列                                              |
| `actionTimerList[].action`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | ユーザーがアクションタイマーに設定したアクションを持つオブジェクト。**現在、空文字列（`""`）が入力されます。将来の拡張のために予約されているフィールドです。** |
| `actionTimerList[].label`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | ユーザーが設定したアクションの内容を持つオブジェクト |
| `actionTimerList[].repeatDay[]`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | 毎週繰り返しのアクションタイマーで、繰り返しの曜日を持つオブジェクト配列 |
| `actionTimerList[].repeatPeriod`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 繰り返しの周期を持つオブジェクトです。このオブジェクトの`value`フィールドは、以下の値を持ちます。<ul><li>空文字列（<code>""</code>）：一回性のアクションタイマー</li><li><code>"daily"</code>：毎日繰り返しのアクションタイマー</li><li><code>"weekly"</code>：毎週繰り返しのアクションタイマー</li></ul> |
| `actionTimerList[].scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | アクションタイマーを鳴らす日付と時間を持つオブジェクト      |
| `actionTimerList[].token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | アクションタイマーの識別子を持つオブジェクト              |
| `type`        | string                                                                                                | コンテンツテンプレートのタイプを示す値。`"ActionTimerList"`を持ちます。             |

## Template example

{% raw %}

```json
{
  "type": "ActionTimerList",
  "actionTimerList": [
    {
      "token": {
        "type": "string",
        "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-10-01T14:00:00Z"
      },
      "action": {
        "type": "string",
        "value": ""
      },
      "label": {
        "type": "string",
        "value": "音楽を再生する"
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
        "value": "2017-10-02T09:00:00Z"
      },
      "action": {
        "type": "string",
        "value": ""
      },
      "label": {
        "type": "string",
        "value": "音楽を再生する"
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
        "value": "2017-10-03T11:00:00Z"
      },
      "action": {
        "type": "string",
        "value": ""
      },
      "label": {
        "type": "string",
        "value": "音楽を再生する"
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
  ]
}
```

{% endraw %}

## UI example {#UIExample}

以下は、{{ book.ServiceEnv.OrientedService }}が配布したClovaのモバイルアプリで、ActionTimerListテンプレートの内容を表したUIサンプルです。

![](/Develop/Assets/Images/Content_Template-ActionTimerList.png)

## 次の項目も参照してください。
* [ActionTimer](/Develop/References/ContentTemplates/ActionTimer.md)
* [Alerts](/Develop/References/CICInterface/Alerts.md)インターフェース
