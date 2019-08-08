# アラームを処理する

ユーザーは、発話でClovaにアラームを追加するようにリクエストすることができます。Clovaは、そのようなリクエストを受信すると、アラームを設定したり、設定した時刻にアラームを鳴らしたりするよう、ディレクティブをクライアントに送信します。クライアントは、Clovaから送信されるアラーム関連メッセージを適切に処理する必要があります。

ここでは、以下の内容について説明します。
* [アラームを設定する](#RegisterAlert)
* [アラームを鳴らす](#RingAlert)
* [アラームを停止する](#StopAlert)
* [アラームを編集および削除する](#EditAlert)
* [アラームを同期する](#SyncAlert)

<div class="note">
<p><strong>メモ</strong></p>
<p>アラームを処理する仕組みにより、クライアントがネットワークに接続しない場合、アラームに関連するすべてのアクションは正常に実行されません。</p>
</div>

## アラームを設定する {#RegisterAlert}

ユーザーの発話からアラームを設定する流れは、次の通りです。

![](/Develop/Assets/Images/CIC_Alerts_Add_Work_Flow.svg)

ユーザーがアラームを設定するよう発話でリクエスト（[`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)）すると、Clovaはユーザーの発話を解析し、ユーザーのクライアントデバイスがアラームを追加するように[`Alerts.SetAlert`](/Develop/References/CICInterface/Alerts.md#SetAlert)ディレクティブを送信します。

クライアントは`Alerts.SetAlert`ディレクティブを受信すると、そのディレクティブから以下の内容を確認して、クライアントデバイスにアラームを設定する必要があります。
* アラームの種類
* アラームの設定時刻
* アラームのラベルまたは内容（任意）
* アラームの設定時刻に再生するオーディオ（任意）

クライアントは、次のような`Alerts.SetAlert`ディレクティブを受信します。

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

上記のディレクティブには、以下の情報が含まれます。クライアントは、以下の内容に基づいてアラームを設定する必要があります。
* アラームの種類：`REMINDER`
* アラームの設定時刻：`2017-09-25T09:00:50+09:00`
* アラームのラベルまたは内容：「入金する」
* 再生するオーディオと順序
  * 1番目に再生するオーディオ：`clova://alert/bell/reminder`
  * 2番目に再生するオーディオ：`https://abc.de.fe/tts2`

クライアントがアラームを設定して、その結果をCICに送信します。アラームを正常に設定した場合、[`Alerts.SetAlertSucceeded`](/Develop/References/CICInterface/Alerts.md#SetAlertSucceeded)イベントをCICに送信します。

```json
{
  "context": [
    ...
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

アラームの設定に失敗した場合には、[`Alerts.SetAlertFailed`](/Develop/References/CICInterface/Alerts.md#SetAlertFailed)イベントを送信します。

```json
{
  "context": [
    ...
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

Clovaは、アラームを設定した結果をユーザーに通知するために、[`SpeechSynthesizer.Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak)ディレクティブと[`Clova.RenderTemplate`](/Develop/References/CICInterface/Clova.md#RenderTemplate)ディレクティブをクライアントに送信します。クライアントは、そのディレクティブの内容をユーザーに通知する必要があります。

## アラームを鳴らす {#RingAlert}

設定された時刻になると、Clovaはアラームを開始するためにディレクティブを送信します。アラームが開始される仕組みは、以下の通りです。

![](/Develop/Assets/Images/CIC_Alerts_Ring_Work_Flow.svg)

アラームが設定された時刻になると、クライアントはアラームを実行し、そのことを[`Alerts.AlertStarted`](/Develop/References/CICInterface/Alerts.md#AlertStarted)イベントでCICにレポートする必要があります。

{% raw %}
```json
{
  "context": [
    ...
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

アラームが実行されると、クライアントは、CICに送信するすべてのイベントに、アクティブなアラームの情報を含める必要があります。そのとき、[`Alert.AlertsState`](/Develop/References/Context_Objects.md#AlertsState)コンテキストの`activeAlerts`フィールドを使用する必要があります。

## アラームを停止する {#StopAlert}

ユーザーは、発話（[`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)）、物理ボタン（ハードウェア）、またはGUIボタン（ソフトウェア）でアラームを停止するようリクエストします。クライアントは、ユーザーからのアラーム停止のリクエストを、[`Alerts.RequestAlertStop`](/Develop/References/CICInterface/Alerts.md#RequestAlertStop)イベントでCICにレポートする必要があります。

```json
{
  "context": [
    ...
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

Clovaは、クライアントがアラームを停止するよう、クライアントに[`Alerts.StopAlert`](/Develop/References/CICInterface/Alerts.md#StopAlert)ディレクティブを送信します。

クライアントは`Alerts.StopAlert`ディレクティブを受信すると、そのディレクティブから以下の内容を確認して、アクティブなアラームを停止する必要があります。
* 停止するアラームの識別子
* アラームの種類

クライアントは、次のような`Alerts.StopAlert`ディレクティブを受信します。

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

クライアントは、ディレクティブに指定されている`token`、`type`と一致するアラームを停止する必要があります。

クライアントは、アラームを停止し、そのことを[`Alerts.AlertStopped`](/Develop/References/CICInterface/Alerts.md#AlertStopped)イベントでレポートする必要があります。

```json
{
  "context": [
    ...
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

アクションタイマータイプのアラームが設定されている場合、Clovaはクライアントに対して、ユーザーが予約したアクションに該当するディレクティブを送信します。

<div class="tip">
<p><strong>ヒント</strong></p>
<p>スヌーズアラームの場合、現在の時刻からもっとも近いアラーム1件のみをクライアントに設定し、実行します。クライアントに設定されているスヌーズアラームが一度実行されてから停止すると、次のスヌーズアラームをCICから新しく受信します。もし、ネットワークに長い間接続できない場合、スヌーズアラームが正常に動作しない可能性があります。</p>
</div>

<div class="tip">
<p><strong>ヒント</strong></p>
<p>上記の流れで、Clovaアプリでのみアラームを編集または削除できます。なお、ユーザーの発話に対するレスポンスとしてのディレクティブではない場合、ディレクティブは[Downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)ストリーム上でクライアントに送信されます。</p>
</div>

## アラームを編集および削除する {#EditAlert}

ユーザーがClovaアプリでアラームを編集したり、削除しようとすると、Clovaはそのリクエストを処理するために、[`Alerts.SetAlert`](/Develop/References/CICInterface/Alerts.md#SetAlert)ディレクティブまたは[`Alerts.DeleteAlert`](/Develop/References/CICInterface/Alerts.md#DeleteAlert)ディレクティブをクライアントに送信します。

アラームの編集と設定は同じプロセスになるので、アラームを編集する方法については[アラームを設定する](#RegisterAlert)を参照してください。

ユーザーからアラーム削除のリクエストを受けると、クライアントは`Alerts.DeleteAlert`ディレクティブを受信します。そのディレクティブから以下の内容を確認して、アクティブなアラームを停止する必要があります。
* 削除するアラームの識別子
* アラームの種類

クライアントは、次のような`Alerts.DeleteAlert`ディレクティブを受信します。

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

クライアントは、ディレクティブに指定されている`token`、`type`と一致するアラームを削除する必要があります。

クライアントがアラームを編集するか、または削除します。アラームを正常に削除した場合、[`Alerts.DeleteAlertSucceeded`](/Develop/References/CICInterface/Alerts.md#DeleteAlertSucceeded)イベントをCICに送信します。

{% raw %}
```json
{
  "context": [
    ...
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

アラームの削除に失敗した場合には、[`Alerts.DeleteAlertFailed`](/Develop/References/CICInterface/Alerts.md#DeleteAlertFailed)イベントを送信します。

{% raw %}
```json
{
  "context": [
    ...
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

## アラームを同期する {#SyncAlert}

ユーザーのクライアントが追加されたり、一部または特定のクライアントのネットワーク接続が一度解除され、再接続した場合、あるいはクライアントに登録されているユーザーのアカウントが変更された場合、クライアントはCICに接続または再接続すると[`System.RequestSynchronizeState`](/Develop/References/CICInterface/System.md#RequestSynchronizeState)イベントをCICを送信し、CICから[`System.SynchronizeState`](/Develop/References/CICInterface/System.md#SynchronizeState)ディレクティブを受信します。そのとき、`allAlerts`フィールドのアラームデータをデバイスのアラーム情報と同期する必要があります。

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
