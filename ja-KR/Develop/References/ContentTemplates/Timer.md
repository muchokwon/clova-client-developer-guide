# Timerテンプレート
CICは、ユーザーがタイマーを作成すると、そのタイマーの情報をTimerテンプレート形式でクライアントに送信します。クライアントは、このテンプレートを使って、ユーザーが作成したタイマーの情報を画面に表示する必要があります。

<div class="tip">
<p><strong>ヒント</strong></p>
<p>Timerテンプレートには、現在、次のような制約事項があります。</p>
<ul>
  <li>ユーザーは、音声を使って、タイマーの設定と、タイマーのリストの照会のみリクエストできます。</li>
  <li>ユーザーがタイマーを編集または削除するには、Clovaアプリを使用する必要があります。</li>
</ul>
</div>

## Template fields

| フィールド名       | データ型    | 説明                     |
|---------------|---------|-----------------------------|
| `scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 追加されたタイマーを鳴らす日付と時間を持つオブジェクト             |
| `token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 追加されたタイマーの識別子を持つオブジェクト                     |
| `type`          | string                                                                              | コンテンツテンプレートのタイプを示す値。`"Timer"`を持ちます。        |

## Template example

{% raw %}

```json
{
  "type": "Timer",
  "token": {
    "type": "string",
    "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
  },
  "scheduledTime": {
    "type": "datetime",
    "value": "2017-12-24T00:00:00Z"
  }
}
```

{% endraw %}

## UI example {#UIExample}

以下は、{{ book.ServiceEnv.OrientedService }}が配布したClovaのモバイルアプリで、Timerテンプレートの内容を表したUIサンプルです。

![](/Develop/Assets/Images/Content_Template-Timer.png)

## 次の項目も参照してください。
* [Alerts](/Develop/References/MessageInterfaces/Alerts.md)インターフェース
* [TimerList](/Develop/References/ContentTemplates/TimerList.md)
