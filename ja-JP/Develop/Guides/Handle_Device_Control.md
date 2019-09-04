# クライアントの動作制御を処理する

ユーザーは、発話したり、または操作したりすることで、クライアントの動作を制御することができます。Clovaは、そのようなリクエストを受け取った場合、ユーザーからリクエストされたことを実行するように、ディレクティブをクライアントに送信します。クライアントは、Clovaから送信される動作制御関連メッセージを適切に処理する必要があります。

ここでは、以下の内容について説明します。

* [クライアントデバイスの設定を有効にする](#HandleClientFeatureToggle)
* [クライアントの音量を調整する](#HandleDeviceVolume)
* [処理結果をレポートする](#HandleActionExecutedResponse)
* [デバイスのステータスを共有する](#HandleDeviceStateReport)

<div class="note">
  <p><strong>メモ</strong></p>
  <p>クライアントからディレクティブを受信し、正常に処理したり、または処理できなかったりする度に、<code>DeviceControl.ActionExecuted</code>または<code>DeviceControl.ActionFailed</code>イベントでCICにレポートする必要があります。詳細については、<a href=#HandleActionExecutedResponse>処理結果をレポートする</a>セクションを参照してください。</p>
</div>

## クライアントデバイスの設定を有効にする {#HandleClientFeatureToggle}

ユーザーは、クライアントデバイスの特定の設定を、必要なときには有効にし、必要がないときには無効にすることができます。次の2つの方法で、デバイスの設定を有効にするようにリクエストすることができます。

* ユーザーが発話で機能を有効にするようにリクエストする（通常のやり方）
* ユーザーがClovaアプリからリモートで特定のクライアントの機能を有効にするようにリクエストする

次は、デバイス設定の有効化/無効化を処理する流れを示します。

![](/Develop/Assets/Images/CIC_DeviceControl_Work_Flow1.svg)

ユーザーがクライアントの制御を発話でリクエスト（[`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)）します。
クライアントは、ユーザーのリクエストをイベントで送信します。そのとき、イベントには[`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState)コンテキストが含まれる必要があります。
CICは、[`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState)コンテキストの`actions[]`フィールドを解析し、ユーザーからのクライアント制御のリクエストをそのクライアントで実行できるかどうかを判断します。
クライアントがそのリクエストを処理できる場合、CICはユーザーのクライアントで特定の機能を有効にするように[`DeviceControl.TurnOn`](/Develop/References/MessageInterfaces/DeviceControl.md#TurnOn)ディレクティブを送信します。このディレクティブには、機能を有効にするための情報が含まれているので、その情報から有効にする機能を特定する必要があります。次の[`DeviceControl.TurnOn`](/Develop/References/MessageInterfaces/DeviceControl.md#TurnOn)ディレクティブを受信します。

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "TurnOn",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "airplane"
    }
  }
}
```

`payload`の`target`フィールドは制御する対象を示すフィールドです。次のようなものがあります。

* `airplane`：機内モード
* `bluetooth`：Bluetooth
* `cellular`：セルラーネットワーク
* `flashlight`：フラッシュライト
* `gps`：GPS
* `powersave`：節電モード
* `soundmode`：サウンドモード
* `wifi`：WiFi

クライアントは、`target`フィールドの情報を使って、機能を有効にする必要があります。上記のディレクティブに基づいて、クライアントは機内モードを有効にする必要があります。

クライアントは、このディレクティブを処理して、その結果を[`DeviceControl.ActionExecuted`](/Develop/References/MessageInterfaces/DeviceControl.md#ActionExecuted)または[`DeviceControl.ActionFailed`](/Develop/References/MessageInterfaces/DeviceControl.md#ActionFailed)イベントでCICに送信する必要があります。

## クライアントの音量を調整する {#HandleDeviceVolume}

ユーザーは、オーディオを再生しているクライアントデバイスの音量を調整するようにClovaにリクエストすることがあります。次の3つの方法で、音量を調整するようにリクエストできます。

* ユーザーが発話で音量を調整するようにリクエストする（通常のやり方）
* ユーザーがクライアントのボタンを操作して音量を調整するようにリクエストする
* ユーザーがClovaアプリからリモートで特定のクライアントの音量を調整するようにリクエストする

ユーザーは、発話（[`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)）したり、デバイスを操作したりすることで、音量を調整するようにリクエストすることができます。ユーザーのリクエストがある場合、Clovaはユーザーの発話を解析し、ユーザーのクライアントで特定の機能を有効にするように[`DeviceControl.Increase`](/Develop/References/MessageInterfaces/DeviceControl.md#Increase)ディレクティブを送信します。また、クライアントは、コンテキストの[`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState)オブジェクトで、Bluetoothデバイスの情報をCICに随時送信する必要があります。

ユーザーが音量を上げ下げしたり、指定したりすると、クライアントは以下のディレクティブを受信します。このディレクティブの内容を確認し、クライアントで音量を調整する必要があります。

クライアントは、次のような`DeviceControl.Increase`ディレクティブを受信します。

{% raw %}
```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "Increase",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "volume"
    }
  }
}
```
{% endraw %}

`payload`の`target`フィールドは、制御の対象を示します。音量を調整するには、`target`が`volume`である必要があります。

上記のディレクティブに基づいて、クライアントは音量を上げる必要があります。ユーザーからの指定がない場合、デフォルト値だけ音量を上げる必要があります。デフォルト値は、開発会社で任意に設定できます。

クライアントは、このディレクティブを処理して、その結果を[`DeviceControl.ActionExecuted`](/Develop/References/MessageInterfaces/DeviceControl.md#ActionExecuted)または[`DeviceControl.ActionFailed`](/Develop/References/MessageInterfaces/DeviceControl.md#ActionFailed)イベントでCICに送信する必要があります。

## デバイスのステータスを共有する {#HandleDeviceStateReport}

Clovaアプリがユーザーのアカウントに登録されているクライアントデバイスのステータスを確認するために、以下のようにステータスを要求することもあります。このとき、ステータスを共有するようにリクエストされたクライアントは、**必ずクライアント自身のステータスを共有する必要があります。**

![](/Develop/Assets/Images/CIC_DeviceControl_Work_Flow2.svg)

1. クライアント（主にClovaアプリ）が[`DeviceControl.RequestStateSynchronization`](/Develop/References/MessageInterfaces/DeviceControl.md#RequestStateSynchronization)イベントをCICに送信します。
2. CICは、ユーザーのアカウントに登録されているすべてのクライアント（Clovaアプリを除く）に[`DeviceControl.ExpectReportState`](/Develop/References/MessageInterfaces/DeviceControl.md#ExpectReportState)ディレクティブを、[Downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)で送信します。
3. [`DeviceControl.ExpectReportState`](/Develop/References/MessageInterfaces/DeviceControl.md#ExpectReportState)ディレクティブを受信したクライアントは、**[`DeviceControl.ReportState`](/Develop/References/MessageInterfaces/DeviceControl.md#ReportState)イベントをCICに送信して、現在のステータスをレポートする必要があります。**
4. CICは、収集されたクライアントのステータスを、[`DeviceControl.SynchronizeState`](/Develop/References/MessageInterfaces/DeviceControl.md#SynchronizeState)ディレクティブを使って[Downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)でClovaアプリに送信します。
5. [`DeviceControl.SynchronizeState`](/Develop/References/MessageInterfaces/DeviceControl.md#SynchronizeState)ディレクティブを受信すると、Clovaアプリは他のクライアントのステータスを更新します。

<div class="note">
  <p><strong>メモ</strong></p>
  <p>クライアントは、ユーザーのアカウントに新規に追加されたり、CICに再接続されたりすると、<a href="/Develop/References/MessageInterfaces/DeviceControl.md#ExpectReportState"><code>DeviceControl.ExpectReportState</code></a>ディレクティブを受信します。その際、クライアントはClovaアプリにステータスを共有するときと同じ動作をする必要があります。</p>
</div>

## 処理結果をレポートする {#HandleActionExecutedResponse}

クライアントの制御を正常に処理したり、または処理できなかったりする度に、`DeviceControl.ActionExecuted`または`DeviceControl.ActionFailed`イベントでCICにレポートする必要があります。正常に制御した場合、以下のような[`DeviceControl.ActionExecuted`](/Develop/References/MessageInterfaces/DeviceControl.md#ActionExecuted)イベントをCICに送信します。

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "DeviceControl",
      "name": "ActionExecuted",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5"
    },
    "payload": {
      "target": "airplane",
      "command": "TurnOn"
    }
  }
}
```

正常に制御できなかった場合には、[`DeviceControl.ActionFailed`](/Develop/References/MessageInterfaces/DeviceControl.md#ActionFailed)イベントを送信します。

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "DeviceControl",
      "name": "ActionFailed",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5"
    },
    "payload": {
      "target": "airplane",
      "command": "TurnOn"
    }
  }
}
```

`payload`の`target`フィールドは制御する対象を示すフィールドです。次のようなものがあります。

* `airplane`：機内モード
* `app`：アプリ
* `bluetooth`：Bluetooth
* `cellular`：セルラーネットワーク
* `channel`：テレビのチャンネル
* `flashlight`：フラッシュライト
* `gps`：GPS
* `powersave`：節電モード
* `screenbrightness`：画面の明るさ
* `soundmode`：サウンドモード
* `volume`：スピーカーの音量
* `wifi`：WiFi

`payload`の`command`フィールドは、`target`フィールドの制御対象に指示したことを示すフィールドです。CICからクライアントに指示するときに送信したディレクティブ名を使用します。
