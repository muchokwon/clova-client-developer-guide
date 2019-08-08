# TimerListテンプレート
CICは、ユーザーがタイマーのリストをリクエストすると、登録されているタイマーのリストをTimerListテンプレート形式でクライアントに送信します。クライアントは、このテンプレートを使って、ユーザーが登録したタイマーのリストを画面に表示する必要があります。

<div class="tip">
<p><strong>ヒント</strong></p>
<p>TimerListテンプレートには、現在、次のような制約事項があります。</p>
<ul>
  <li>ユーザーは、音声を使って、タイマーの設定と、タイマーのリストの照会のみリクエストできます。</li>
  <li>ユーザーがタイマーを編集または削除するには、Clovaアプリを使用する必要があります。</li>
</ul>
</div>

## Template fields

| フィールド名       | データ型    | 説明                     |
|---------------|---------|-----------------------------|
| `timerList[]`               | object array  | ユーザーが登録したタイマーのリストを持つオブジェクト配列                                                                                         |
| `timerList[].scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | タイマーを鳴らす日付と時間を持つオブジェクト                    |
| `timerList[].token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | タイマーの識別子を持つオブジェクト                             |
| `type`                      | string                                                                              | コンテンツテンプレートのタイプを示す値。`"TimerList"`を持ちます。      |

## Template example

{% raw %}

```json
{
  "type": "Timer",
  "timerList": [
    {
      "token": {
        "type": "string",
        "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-12-24T00:00:10Z"
      }
    },
    {
      "token": {
        "type": "string",
        "value": "da740e2a-01cd-4f2e-aedf-6c4285bae785"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-12-24T00:01:00Z"
      }
    }
  ]
}
```

{% endraw %}

## UI example {#UIExample}

以下は、{{ book.ServiceEnv.OrientedService }}が配布したClovaのモバイルアプリで、TimerListテンプレートの内容を表したUIサンプルです。

![](/Develop/Assets/Images/Content_Template-TimerList.png)

## 次の項目も参照してください。
* [Alerts](/Develop/References/CICInterface/Alerts.md)インターフェース
* [Timer](/Develop/References/ContentTemplates/Timer.md)
