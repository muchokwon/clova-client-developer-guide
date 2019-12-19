# Alerts

Alertsインターフェースは、クライアントでアラームを設定・編集・削除・開始・停止するための名前空間です。次の表は、アラームの種類を示しています。

| アラーム       | 説明                                |
|---------------|------------------------------------|
| アクションタイマー（`"ACTIONTIMER"`） | 指定された時間が過ぎると、特定の動作を実行します。                               |
| アラーム（`"ALARM"`）            | 指定された日付と時刻にアラームを鳴らします。                                            |
| リマインダー（`"REMINDER"`）      | 指定された日付と時刻に、ユーザーが入力した内容を表示するか、音声で再生してアラームを鳴らします。例えば、ユーザーが「明日7時に薬を飲むとリマインドして」と指示した場合、「明日7時」はアラームの時間で、「薬を飲む」はアラームを鳴らすときに通知する内容になります。クライアントは、アラームを実行する際、ユーザーにリマインドする内容を画面に表示したり、音声で再生する必要があります。       |
| タイマー（`"TIMER"`）           | 指定された時間が過ぎると、アラームを鳴らします。                                         |

ユーザーは、アラームを音声またはClovaアプリで追加できます。アラームの編集および削除は、Clovaアプリでのみ可能です。Clovaは、アラームに関連する情報をクラウドに保存し、ユーザーが設定したアラームをCICを介してクライアントに送信します。Clovaは、スヌーズアラームの場合、現在の時刻にもっとも近いアラームのみをクライアントに送信します。また、そのアラームが停止すると、次のスヌーズアラームをクライアントに送信します。

クライアントは、Alertsインターフェースを使用して、次の動作を実行する必要があります。
* CICからディレクティブを受信すると、ディレクティブの内容に応じてアラームを設定・編集・削除します。
* 指定された時刻にアラームを実行します。
* アラームを設定・編集・削除・開始・停止した場合、必ずCICにレポートする必要があります。そうすることで、CICから適切な結果をクライアントに返すことができます。
* イベントを送信する際、必ず[`Alerts.AlertsState`](/Develop/References/Context_Objects.md#AlertsState)コンテキストオブジェクトを含めて、クライアントに設定されているアラームの状態をレポートする必要があります。

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>アラームの設定・編集・削除・開始・停止の実装については、<a href="/Develop/Guides/Handle_Alerts.md">アラームを処理する</a>を参照してください。</p>
</div>

Alertsが提供するイベントとディレクティブは、次の通りです。

| メッセージ         | タイプ  | 説明                                   |
|------------------|-----------|---------------------------------------------|
| [`AlertStarted`](#AlertStarted)                 | イベント     | クライアントから、アラームが開始したことをCICにレポートします。 |
| [`AlertStopped`](#AlertStopped)                 | イベント     | クライアントから、アラームが停止したことをCICにレポートします。 |
| [`DeleteAlert`](#DeleteAlert)                   | ディレクティブ | クライアントに対して、特定のアラームを削除するように指示します。 |
| [`DeleteAlertFailed`](#DeleteAlertFailed)       | イベント     | クライアントから、クライアントに設定されている特定のアラームを削除できなかったことをCICにレポートします。 |
| [`DeleteAlertSucceeded`](#DeleteAlertSucceeded) | イベント     | クライアントから、クライアントに設定されている特定のアラームを正常に削除したことをCICにレポートします。 |
| [`RequestAlertStop`](#RequestAlertStop)         | イベント     | クライアントから、アクティブなアラームを停止するようにCICにリクエストします。  |
| [`RequestSynchronizeAlert`](#RequestSynchronizeAlert) | イベント | クライアントから、Clovaのクラウドに保存されたユーザーのアラームデータを同期する必要がある場合、CICにこのイベントを送信します。 |
| [`SetAlert`](#SetAlert)                         | ディレクティブ | クライアントに対して、アラームを追加するか、または特定のアラームを編集するように指示します。 |
| [`SetAlertFailed`](#SetAlertFailed)             | イベント     | クライアントから、特定のアラームを追加または編集できなかったことをCICにレポートします。 |
| [`SetAlertSucceeded`](#SetAlertSucceeded)       | イベント     | クライアントから、特定のアラームを正常に追加または編集したことをCICにレポートします。 |
| [`StopAlert`](#StopAlert)                       | ディレクティブ | クライアントに対して、特定のアラームを停止するように指示します。  |
| [`SynchronizeAlert`](#SynchronizeAlert)         | ディレクティブ | クライアントに対して、`payload`内にあるユーザーのアラームデータを同期するように指示します。  |

上記のメッセージのうち、[`RequestSynchronizeAlert`](#RequestSynchronizeAlert)イベントと[`SynchronizeAlert`](#SynchronizeAlert)ディレクティブは、Clovaとクライアントの間でアラームやスケジュールのように、ユーザーアカウントに関連する情報を同期する際に使用されます。次のような状況で同期が発生します。

* ユーザーアカウントを利用するクライアントデバイスが追加されたとき
* クライアントがネットワーク接続障害などの理由により、CICに再接続するとき
* 他のユーザーが使用を開始したため、クライアントデバイスに登録されているユーザーアカウントが変更されたとき
* クライアントデバイスとペアリングアプリとの接続が解除され、再設定されるとき

<div class="note">
  <p><strong>メモ</strong></p>
  <p>ネットワークが切れたり、ユーザーアカウントの連携が解除されたら、アカウントに設定されているアラームデータをデバイスから削除する必要があります。</p>
</div>

## AlertStartedイベント {#AlertStarted}

クライアントから、アラームが開始したことをCICにレポートします。クライアントは、アラームが開始したら、必ずこのイベントをCICに送信する必要があります。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 開始したアラームの識別子                  |  |
| `type`    | string | アラームの種類。次のいずれかになります。<ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  |  |

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
      "namespace": "Alerts",
      "name": "AlertStarted",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Alerts.AlertStopped`](#AlertStopped)
* [アラームを鳴らす](/Develop/Guides/Handle_Alerts.md#RingAlert)

## AlertStoppedイベント {#AlertStopped}

クライアントから、アラームが停止したことをCICにレポートします。クライアントは、鳴っていたアラームが停止した場合、必ずこのイベントをCICに送信する必要があります。
* **普通のアラームが停止した場合**、CICから[`Alerts.DeleteAlert`](#DeleteAlert)ディレクティブを受信します。
* **スヌーズアラームが停止した場合**、次のスヌーズアラームを設定するために、CICから[`Alerts.SetAlert`](#SetAlert)ディレクティブを受信します。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 停止したアラームの識別子                  |  |
| `type`    | string | アラームの種類。次のいずれかになります。<ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  |  |

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
      "namespace": "Alerts",
      "name": "AlertStopped",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Alerts.AlertStarted`](#AlertStarted)
* [アラームを停止する](/Develop/Guides/Handle_Alerts.md#StopAlert)

## DeleteAlertディレクティブ {#DeleteAlert}

クライアントに対して、特定のアラームを削除するように指示します。クライアントは特定のアラームを削除し、その結果を次のイベントでCICにレポートする必要があります。

* [`Alerts.DeleteAlertSucceeded`](#DeleteAlertSucceeded)イベント：特定のアラームを正常に削除した場合
* [`Alerts.DeleteAlertFailed`](#DeleteAlertFailed)イベント：特定のアラームを削除できなかった場合

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 削除するアラームの識別子              |  |
| `type`    | string | アラームの種類。次のいずれかになります。<ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  |  |

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Alerts",
      "name": "DeleteAlert",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806",
      "dialogRequestId": "6b4061db-fbc1-45a2-9c54-b7c62d366b98"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Alerts.DeleteAlertFailed`](#DeleteAlertFailed)
* [`Alerts.DeleteAlertSucceeded`](#DeleteAlertSucceeded)
* [アラームを編集および削除する](/Develop/Guides/Handle_Alerts.md#EditAlert)

## DeleteAlertFailedイベント {#DeleteAlertFailed}

クライアントから、クライアントに設定されている特定のアラームを削除できなかったことをCICにレポートします。クライアントは、[Alerts.DeleteAlert](#DeleteAlert)ディレクティブを受信して、特定のアラームを削除できなかった場合、必ずこのイベントをCICに送信する必要があります。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 削除できなかったアラームの識別子             |  |
| `type`    | string | アラームの種類。次のいずれかになります。<ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  |  |

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
      "namespace": "Alerts",
      "name": "DeleteAlertFailed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Alerts.DeleteAlert`](#DeleteAlert)
* [`Alerts.DeleteAlertSucceeded`](#DeleteAlertSucceeded)
* [アラームを編集および削除する](/Develop/Guides/Handle_Alerts.md#EditAlert)

## DeleteAlertSucceededイベント {#DeleteAlertSucceeded}

クライアントから、クライアントに設定されている特定のアラームを正常に削除したことをCICにレポートします。クライアントは、[Alerts.DeleteAlert](#DeleteAlert)ディレクティブを受信して、特定のアラームを正常に削除した場合、必ずこのイベントをCICに送信する必要があります。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 削除したアラームの識別子                  |  |
| `type`    | string | アラームの種類。次のいずれかになります。<ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  |  |

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
      "namespace": "Alerts",
      "name": "DeleteAlertSucceeded",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Alerts.DeleteAlert`](#DeleteAlert)
* [`Alerts.DeleteAlertFailed`](#DeleteAlertFailed)
* [アラームを編集および削除する](/Develop/Guides/Handle_Alerts.md#EditAlert)

## RequestAlertStopイベント {#RequestAlertStop}

クライアントから、アクティブなアラームを停止するようにClovaに対してリクエストします。クライアントは、ユーザーが、発話ではなく物理ボタン（ハードウェア）またはGUIボタン（ソフトウェア）でアラームを停止した場合、CICにこのイベントを送信する必要があります。クライアントがCICにこのイベントを送信すると、後でCICから[`Alerts.StopAlert`](#StopAlert)ディレクティブを受信します。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 停止するアラームの識別子                  |  |
| `type`    | string | アラームの種類。次のいずれかになります。<ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  |  |

### 備考
クライアントデバイスで、ユーザーがボタンを押して直接アラームを停止した場合にも、このイベントをCICに送信して、[`Alerts.StopAlert`](#StopAlert)ディレクティブを受信してからアラームを終了する必要があります。それはアラームを終了するプロセスの一貫性を保つためのもので、アラームの状態を同期する際に役立ちます。しかしそのせいで、アラームを終了するまで時間がかかる可能性があるので、[`Alerts.StopAlert`](#StopAlert)ディレクティブを受信するまでアラームの表示を止めるか、アラーム音をミュートにするなどの処理をする必要があります。

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
      "namespace": "Alerts",
      "name": "RequestAlertStop",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Alerts.StopAlert`](#StopAlert)
* [アラームを停止する](/Develop/Guides/Handle_Alerts.md#StopAlert)

## RequestSynchronizeAlertイベント {#RequestSynchronizeAlert}

クライアントから、Clovaのクラウドに保存されたユーザーのアラームデータを同期する必要がある場合、CICにこのイベントを送信します。CICはこのイベントを受信すると、クライアントに[`Alerts.SynchronizeAlert`](#SynchronizeAlert)ディレクティブを送信します。

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
      "namespace": "Alerts",
      "name": "RequestSynchronizeAlert",
      "messageId": "dd4f2794-6b14-4cc4-ae1b-5bfa1c469028"
    },
    "payload": {}
  }
}
```
{% endraw %}

### 次の項目も参照してください。
* [`System.SynchronizeAlert`](/Develop/References/MessageInterfaces/Alerts.md#SynchronizeAlert)
* [アラームを同期する](/Develop/Guides/Handle_Alerts.md#SyncAlert)

## SetAlertディレクティブ {#SetAlert}

クライアントに対して、アラームを追加するか、または特定のアラームを編集するように指示します。クライアントは、次のようにアラームを追加または編集することができます。

* **`token`フィールドの値と同じ識別子を持つアラームが、現在クライアントデバイスにない場合**、受信したアラームを追加します。
* **`token`フィールドの値と同じ識別子を持つアラームが、現在クライアントデバイスにある場合**、そのアラームを編集します。

また、その結果を次のイベントでCICにレポートする必要があります。

* [`Alerts.SetAlertSucceeded`](#SetAlertSucceeded)イベント：特定のアラームを正常に追加した場合
* [`Alerts.SetAlertFailed`](#SetAlertFailed)イベント：特定のアラームを追加できなかった場合

### Payload field {#SetAlertPayload}

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `assets[]`         | object array | リマインダー（`"REMINDER"`）またはアクションタイマー（`"ACTIONTIMER"`）タイプのアラームが実行される際、再生するTTSオーディオのリストを持つオブジェクト配列。リマインダーとアクションタイマーにのみこのフィールドが含まれます。   | 条件付き |
| `assets[].assetId` | string | TTSオーディオの識別子       |  |
| `assets[].url`     | string | TTSオーディオのURIもしこのフィールドの値が`"clova://alert/bell/{type}"`形式の場合、クライアントはアラームの種類（`type`）によって、クライアントが持っている着信音のうち、適切なものを再生する必要があります。現在、次のいずれかになります。<ul><li><code>"clova://alert/bell/reminder"</code>：リマインダーの着信音を再生する</li></ul>    |  |
| `assetPlayOrder[]` | string array | `assets`フィールド内のTTSオーディオの再生順序を保存している配列。配列のインデックスの順番に合わせて、再生するTTSオーディオの識別子（`assets[].assetId`）が入力されています。リマインダー（`"REMINDER"`）とアクションタイマー（`"ACTIONTIMER"`）にのみこのフィールドが含まれます。  | 条件付き  |
| `label`          | string | リマインダーまたはアクションタイマーの内容                             | 条件付き |
| `scheduledTime`  | string | アラームを鳴らす日付と時間の情報（YYYY-MM-DDThh:mm:ssZの形式）   |  |
| `token`          | string | 追加または編集するアラームの識別子                        |  |
| `type`           | string | アラームの種類。次のいずれかになります。<ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  |  |

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Alerts",
      "name": "SetAlert",
      "messageId": "9a440fa9-983a-48a8-8ad5-faee1250abde",
      "dialogRequestId": "688b051d-6832-4bfd-8cf8-5ff073cd2a82"
    },
    "payload": {
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
      "label": "入金する",
      "assetPlayOrder": [
        "5141f693-5b39-46b7-80e4-3d71ed5508da",
        "b403ebe5-f911-4c5c-98b3-9f5320510235"
      ]
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Alerts.SetAlertFailed`](#SetAlertFailed)
* [`Alerts.SetAlertSucceeded`](#SetAlertSucceeded)
* [アラームを設定する](/Develop/Guides/Handle_Alerts.md#RegisterAlert)
* [アラームを編集および削除する](/Develop/Guides/Handle_Alerts.md#EditAlert)

## SetAlertFailedイベント {#SetAlertFailed}

クライアントから、特定のアラームを追加または編集できなかったことをCICにレポートします。クライアントは、[Alerts.SetAlert](#SetAlert)ディレクティブを受信して、特定のアラームを追加または編集できなかった場合、必ずこのイベントをCICに送信する必要があります。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 追加または編集できなかったアラームの識別子     |  |
| `type`    | string | アラームの種類。次のいずれかになります。<ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  |  |

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
      "namespace": "Alerts",
      "name": "SetAlertFailed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Alerts.SetAlert`](#SetAlert)
* [`Alerts.SetAlertSucceeded`](#SetAlertSucceeded)
* [アラームを設定する](/Develop/Guides/Handle_Alerts.md#RegisterAlert)
* [アラームを編集および削除する](/Develop/Guides/Handle_Alerts.md#EditAlert)

## SetAlertSucceededイベント {#SetAlertSucceeded}

クライアントから、特定のアラームを正常に追加または編集したことをCICにレポートします。クライアントは、[Alerts.SetAlert](#SetAlert)ディレクティブを受信して、特定のアラームを正常に追加または編集した場合、必ずこのイベントをCICに送信する必要があります。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 追加または編集したアラームの識別子          |  |
| `type`    | string | アラームの種類。次のいずれかになります。<ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  |  |

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
      "namespace": "Alerts",
      "name": "SetAlertSucceeded",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Alerts.SetAlert`](#SetAlert)
* [`Alerts.SetAlertFailed`](#SetAlertFailed)
* [アラームを設定する](/Develop/Guides/Handle_Alerts.md#RegisterAlert)
* [アラームを編集および削除する](/Develop/Guides/Handle_Alerts.md#EditAlert)

## StopAlertディレクティブ {#StopAlert}

クライアントに対して、特定のアラームを停止するように指示します。クライアントは指定されたアラームを停止し、[`AlertStopped`](#AlertStopped)イベントでその結果をCICにレポートする必要があります。


### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`   | string | 停止するアラームの識別子              |  |
| `type`    | string | アラームの種類。次のいずれかになります。<ul><li><code>"ACTIONTIMER"</code></li><li><code>"ALARM"</code></li><li><code>"REMINDER"</code></li><li><code>"TIMER"</code></li></ul>  |  |

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Alerts",
      "name": "StopAlert",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806",
      "dialogRequestId": "6b4061db-fbc1-45a2-9c54-b7c62d366b98"
    },
    "payload": {
      "token": "876afa88-8ad5-427b-9878-2edb5b103117",
      "type": "ALARM"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Alerts.AlertStopped`](#AlertStopped)
* [アラームを停止する](/Develop/Guides/Handle_Alerts.md#StopAlert)

## SynchronizeAlertディレクティブ {#SynchronizeAlert}
クライアントに対して、`payload`内にあるユーザーのアラームデータを同期するように指示します。クライアントは、CICから受信したデータに応じて、クライアントに設定されているアラームの値を変更する必要があります。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `allAlerts[]`   | object array | クライアントが同期すべきアラームのリストを含むオブジェクト配列。[`Alerts.SetAlert`](#SetAlert)ディレクティブに使用される[`payload`](#SetAlertPayload)オブジェクトと同じ形式です。 |     |

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Alerts",
      "name": "SynchronizeAlert",
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
* [`Alerts.RequestSynchronizeAlert`](#RequestSynchronizeAlert)
* [アラームを同期する](/Develop/Guides/Handle_Alerts.md#SyncAlert)
