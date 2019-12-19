# System

Systemインターフェースは、Clovaとクライアントの間で、クライアントデバイスのファームウェアなどのシステム関連情報を同期する必要があるとき、関連するディレクティブとイベントを次のように提供します。

| メッセージ         | タイプ  | 説明                                 |
|------------------|-----------|-------------------------------------------|
| [`RequestSynchronizeState`](#RequestSynchronizeState)  | イベント     | クライアントがシステム関連情報を同期する必要があるとき、このイベントをCICに送信します。 |
| [`SynchronizeState`](#SynchronizeState)                | ディレクティブ | クライアントに、`payload`のデータを同期するように指示します。            |

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>現在、System名前空間は編集中で、今後更新される予定です。</p>
</div>

## RequestSynchronizeStateイベント {#RequestSynchronizeState}
クライアントがシステム関連情報を同期する必要があるとき、このイベントをCICに送信します。CICはこのイベントを受信すると、クライアントに[`System.SynchronizeState`](#SynchronizeState)ディレクティブを送信します。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

なし

### Message example
{% raw %}
```json
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Speaker.VolumeState}},
    {{SpeechSynthesizer.SpeechState}}
  ],
  "event": {
    "header": {
      "namespace": "System",
      "name": "RequestSynchronizeState",
      "messageId": "b120c3e0-e6b9-4a3d-96de-71539e5f6214"
    },
    "payload": {}
  }
}
```
{% endraw %}

### 次の項目も参照してください。
* [`System.SynchronizeState`](/Develop/References/MessageInterfaces/System.md#SynchronizeState)

## SynchronizeStateディレクティブ {#SynchronizeState}
クライアントに、`payload`のデータを同期するように指示します。クライアントは、CICから受信したデータに応じて、クライアントに設定されている値を変更する必要があります。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `allAlerts[]`   | object array | **（Deprecated）** クライアントが同期すべきアラームのリストを含むオブジェクト配列。[`Alerts.SetAlert`](/Develop/References/MessageInterfaces/Alerts.md#SetAlert)ディレクティブに使用される[`payload`](/Develop/References/MessageInterfaces/Alerts.md#SetAlertPayload)オブジェクトと同じ形式です。 |     |

<div class="note">
  <p><strong>メモ</strong></p>
  <p><code>System.SynchronizeState</code>ディレクティブでのアラーム情報の同期は、サポートされない予定です。その機能は<a href="/Develop/References/MessageInterfaces/Alerts.md#RequestSynchronizeAlert"><code>Alerts.RequestSynchronizeAlert</code></a>イベントと<a href="/Develop/References/MessageInterfaces/Alerts.md#SynchronizeAlert"><code>Alerts.SynchronizeAlert</code></a>ディレクティブでサポートされる予定なので、これを使用する必要があります。今後、<code>System.SynchronizeState</code>ディレクティブにシステム関連情報を同期するフィールドが追加される予定です。</p>
</div>

### 備考
今後、同期の対象になる情報が増える可能性があります。

### Message example

{% raw %}

```json
// Deprecated example
{
  "directive": {
    "header": {
      "namespace": "System",
      "name": "SynchronizeState",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524"
    },
    "payload": {
      "allAlerts": [
        {
          "type": "REMINDER",
          "token": "77179dbd-b65f-4341-a579-c1b2b97fc5b7",
          "scheduledTime": "2017-09-25T09:00:50+09:00",
          "assets": [
            {
              "assetId": "5141f693-5b39-46b7-80e4-3d71ed5508da",
              "url": "clova://alert/bell/reminder"
            },
            {
              "assetId": "b403ebe5-f911-4c5c-98b3-9f5320510235",
              "url": "https://abc.de.fe/tts2"
            }
          ],
          "assetPlayOrder": ["5141f693-5b39-46b7-80e4-3d71ed5508da", "b403ebe5-f911-4c5c-98b3-9f5320510235"]
        },
        {
          "type": "ALARM",
          "token": "ee4da70c-8328-4456-ab6f-c28cec626ae6",
          "scheduledTime": "2017-09-26T11:00:50+09:00"
        },
        ...
      ]
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`System.RequestSynchronizeState`](#RequestSynchronizeState)
