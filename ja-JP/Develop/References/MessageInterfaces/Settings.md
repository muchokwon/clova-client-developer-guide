# Settings

Settingsインターフェースは、Clovaとクライアントの間で、クライアントの設定を更新したり、同期するときに使用する名前空間です。Settingsでは、次のイベントとディレクティブを提供します。

| メッセージ         | タイプ  | 説明                                 |
|------------------|-----------|-------------------------------------------|
| [`ExpectReport`](#ExpectReport) | ディレクティブ | クライアントに対して、現在の設定をレポートするように指示します。クライアントはこのディレクティブを受信すると、[`Settings.Report`](#Report)イベントをCICに送信する必要があります。 |
| [`Report`](#Report)             | イベント     | クライアントが現在の設定をCICにレポートします。クライアントがCICから[`Settings.ExpectReport`](#ExpectReport)ディレクティブを受信した場合、`Settings.Report`イベントをCICに送信する必要があります。  |
| [`Update`](#Update)             | ディレクティブ | クライアントに対して、`payload`に指定されている値を設定値として適用するように指示します。  |

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>設定情報の更新や同期についてはは、<a href="/Develop/Guides/Handle_Settings.md">設定情報を処理する</a>を参照してください。</p>
</div>

## ExpectReportディレクティブ {#ExpectReport}
クライアントに対して、現在の設定をレポートするように指示します。クライアントはこのディレクティブを受信すると、[`Settings.Report`](#Report)イベントをCICに送信する必要があります。

### Payload fields

なし

### 備考

* このディレクティブはイベントに対する応答ではなく、[Downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)で送信されます。

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "Settings",
      "name": "ExpectReport",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524"
    },
    "payload": {}
  }
}
```

### 次の項目も参照してください。
* [`Settings.Report`](#Report)
* [設定情報を処理する](/Develop/Guides/Handle_Settings.md)

## Reportイベント {#Report}
クライアントが現在の設定をCICにレポートします。クライアントがCICから[`Settings.ExpectReport`](#ExpectReport)ディレクティブを受信した場合、`Settings.Report`イベントをCICに送信する必要があります。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `configuration` | object | 事前に定義されたクライアント設定を持つオブジェクト。このオブジェクトのサブフィールドは、すべて文字列型になります。<div class="tip"><p><strong>ヒント</strong></p><p>設定データは、クライアントごとに異なる形式で定義することができます。クライアント設定の事前定義については、提携担当者までお問い合わせください。</p></div> |    |

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
      "namespace": "Settings",
      "name": "Report",
      "messageId": "b120c3e0-e6b9-4a3d-96de-71539e5f6214"
    },
    "payload": {
      "configuration": {
        "key1": "value1",
        "key2": "value2",
        ...
      }
    }
  }
}
```
{% endraw %}

### 次の項目も参照してください。
* [`Settings.ExpectReport`](#ExpectReport)
* [設定情報を処理する](/Develop/Guides/Handle_Settings.md)

## Updateディレクティブ {#Update}
クライアントに対して、`payload`に指定されている値を設定値として適用するように指示します。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `configuration` | object | 事前に定義されたクライアント設定を持つオブジェクト。このオブジェクトのサブフィールドは、すべて文字列型になります。<div class="tip"><p><strong>ヒント</strong></p><p>クライアント設定の事前定義については、Clova事務局までお問い合わせください。</p></div> |    |

### 備考

* このディレクティブはイベントに対する応答ではなく、[Downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)で送信されます。

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Settings",
      "name": "Update",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524"
    },
    "payload": {
      "configuration": {
        "key1": "value1",
        "key2": "value2",
        ...
      }
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Settings.Report`](#Report)
* [設定情報を処理する](/Develop/Guides/Handle_Settings.md)
