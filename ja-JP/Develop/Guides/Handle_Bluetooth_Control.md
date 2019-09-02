# クライアントのBluetooth制御を処理する

ユーザーは、発話したり、または操作したりすることで、クライアントのBluetooth接続動作を制御することができます。Clovaは、そのようなリクエストを受け取った場合、ユーザーからリクエストされたことを実行するように、ディレクティブをクライアントに送信します。クライアントは、Clovaから送信されるBluetoothの制御関連メッセージを適切に処理する必要があります。

ここでは、以下の内容について説明します。

* [Bluetoothデバイスとの接続のリクエストを処理する](#HandleBluetoothConnect)
* [Bluetoothデバイスを介するオーディオの再生を処理する](#HandleBluetoothPlayAudio)
* [Bluetoothデバイスとの接続解除を処理する](#HandleBluetoothDisconnect)
* [ペアリングされているBluetoothデバイスの削除を処理する](#HandleBluetoothDelete)

<div class="note">
  <p><strong>メモ</strong></p>
  <p>クライアントからディレクティブを受信し、正常に処理したり、または処理できなかったりする度に、<code>DeviceControl.ActionExecuted</code>または<code>DeviceControl.ActionFailed</code>イベントでCICにレポートする必要があります。詳細については、<a href="/Develop/Guides/Handle_Device_Control.md">クライアントの動作制御を処理する</a>の<a href="/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse">処理結果をレポートする</a>セクションを参照してください。</p>
</div>

## Bluetoothデバイスとの接続のリクエストを処理する {#HandleBluetoothConnect}

ユーザーが、前にクライアントと接続したことのないBluetoothデバイスと接続するには、先にクライアントからBluetoothペアリングモードに入る必要があります。ユーザーは、Clovaアプリからリモートで特定のクライアントのBluetoothペアリングモードに入ることができます。

次は、Bluetoothペアリングモードへの進入を処理する動作の流れを示します。

![](/Develop/Assets/Images/CIC_BluetoothControl_Work_Flow1.svg)

ユーザーがClovaアプリからBluetoothペアリングモードに入ろうとすると、ClovaはユーザーのクライアントからBluetoothペアリングモードに入るように[`DeviceControl.BtStartPairing`](/Develop/References/MessageInterfaces/DeviceControl.md#BtStartPairing)ディレクティブを送信します。このディレクティブを受信したクライアントは、Bluetoothペアリングモードに入る必要があります。次の[`DeviceControl.BtStartPairing`](/Develop/References/MessageInterfaces/DeviceControl.md#BtStartPairing)ディレクティブを受信します。

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtStartPairing",
      "messageId": "0f9950d1-c908-4e02-8c38-8e64e840634c",
      "dialogRequestId": "de0a1fd7-2ef1-4040-9469-3a5dd03ef46b"
    },
    "payload": {}
  }
}
```

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>ペアリングモードは、クライアントと特定のBluetoothデバイスを最初に接続するときにのみ入ります。</p>
</div>

クライアントがペアリングモードに入った状態で、新しいBluetoothデバイスと接続するようにリクエストされた場合、Clovaは、CICを通じてクライアントがBluetoothデバイスと接続するように処理する必要があります。

次は、新しいBluetoothデバイスへの接続を処理する流れを示します。

![](/Develop/Assets/Images/CIC_BluetoothControl_Work_Flow2.svg)

ユーザーが接続するデバイスを指定すると、CICは、クライアントに指定されたデバイスと接続するように[`DeviceControl.BtConnect`](/Develop/References/MessageInterfaces/DeviceControl.md#BtConnect)ディレクティブを送信します。次の[`DeviceControl.BtConnect`](/Develop/References/MessageInterfaces/DeviceControl.md#BtConnect)ディレクティブを受信します。

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtConnect",
      "messageId": "0f9950d1-c908-4e02-8c38-8e64e840634c",
      "dialogRequestId": "de0a1fd7-2ef1-4040-9469-3a5dd03ef46b"
    },
    "payload": {
      "name": "My Speaker",
      "address": "29:01:11:1f:12:89",
      "connected": false,
      "role": "source"
    }
  }
}
```

新しいデバイスと接続するとき、`payload`の内容を分析する必要があります。

`payload`の主なフィールドには、以下のものがあります。
* `address`：削除するBluetoothデバイスのデバイスアドレス
* `connected`：削除するBluetoothデバイスとの接続状態
* `name`：削除するBluetoothデバイスの名前
* `role`：指定されたBluetoothデバイスと接続するときのクライアントのロール

新しいBluetoothデバイスと接続するとき、BluetoothデバイスからPINコードを要求されます。BluetoothデバイスからPINコードを要求されると、[`DeviceControl.BtRequestForPINCode`](/Develop/References/MessageInterfaces/DeviceControl.md#BtRequestForPINCode)イベントでCICにそのことを送信する必要があります。

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtRequestForPINCode",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806",
    },
    "payload": {
      "deviceName": "friends device"
    }
  }
}
```

[`DeviceControl.BtRequestForPINCode`](/Develop/References/MessageInterfaces/DeviceControl.md#BtRequestForPINCode)イベントを受信し、ユーザーがPINコードを入力すると、CICは[`DeviceControl.BtConnectByPINCode`](/Develop/References/MessageInterfaces/DeviceControl.md#BtConnectByPINCode)ディレクティブで、クライアントがPINコードを要求したBluetoothデバイスと接続するように指示します。CICは、入力を受けた`pincode`フィールドの値をクライアントに送信します。

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtConnectByPINCode",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "pinCode": "1234"
    }
  }
}
```

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>前に接続したことのあるデバイスと接続するとき、ペアリングモードへの進入とPINコードの確認は省略されます。</p>
</div>

## Bluetoothデバイスを介するオーディオの再生を処理する {#HandleBluetoothPlayAudio}

ユーザーがクライアントと接続されたBluetoothデバイスを介してオーディオを再生するようにリクエストすると、ClovaはCICを介してクライアントがオーディオを再生するように指示します。Bluetoothデバイスを介するオーディオ再生は、次の2つの方法でリクエストすることができます。

* ユーザーが発話でリクエストする（通常のやり方）
* ユーザーがClovaアプリからリモートで特定のクライアントから再生するようにリクエストする

次は、ユーザーが発話でBluetoothデバイスからオーディオを再生するようにリクエストしたとき、それを処理する流れを示します。

![](/Develop/Assets/Images/CIC_BluetoothControl_Work_Flow3.svg)

ユーザーがBluetoothデバイスからのオーディオ再生をリクエストすると、CICはクライアントに[`DeviceControl.BtPlay`](/Develop/References/MessageInterfaces/DeviceControl.md#BtPlay)ディレクティブを送信します。このとき、クライアントのロールによって、オーディオが出力されるデバイスが決まります。また、クライアントは、コンテキストの[`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState)オブジェクトで、Bluetoothデバイスの情報をCICに随時送信する必要があります。次の[`DeviceControl.BtPlay`](/Develop/References/MessageInterfaces/DeviceControl.md#BtPlay)ディレクティブを受信します。

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtPlay",
      "messageId": "0b75c599-ead8-44a7-ad12-95370b43e7f6",
      "dialogRequestId": "91ee9636-5ede-4658-9df2-ab869e160f52"
    },
    "payload": {}
  }
}
```

クライアントのロールが`"sink"`なら、接続されたBluetoothデバイスからオーディオストリームを受信し、スピーカーでオーディオを出力します。
クライアントのロールが`"source"`なら、クライアントから一時停止されたか、または前に再生したオーディオストリームを、接続されたBluetoothデバイスで再生します。前に再生したオーディオがなかったり、特定のオーディオを指定できない場合、ユーザーから「音楽を再生して」という発話でリクエストを受け取ったときに該当するイベントをCICに送信したり、製品のUI/UXに適切なオーディオコンテンツ再生のリクエストをCICに送信します。

## Bluetoothデバイスとの接続解除を処理する {#HandleBluetoothDisconnect}

ユーザーが今接続されているBluetoothデバイスとの接続を解除するようにリクエストすると、ClovaはCICを介して、クライアントがBluetoothデバイスとの接続を解除するように指示します。次は、接続解除を処理する動作の流れを示します。

![](/Develop/Assets/Images/CIC_BluetoothControl_Work_Flow4.svg)

ユーザーがBluetoothデバイスとの接続解除をリクエストすると、CICはクライアントに[`DeviceControl.BtDisconnect`](/Develop/References/MessageInterfaces/DeviceControl.md#BtDisconnect)ディレクティブを送信します。ディレクティブの内容によって、クライアントは指定されたBluetoothデバイスまたは接続されたすべてのBluetoothデバイスとの接続を解除する必要があります。また、クライアントは、コンテキストの[`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState)オブジェクトで、Bluetoothデバイスの情報をCICに随時送信する必要があります。接続されたすべてのBluetoothデバイスとの接続を解除するとき、以下のような[`DeviceControl.BtDisconnect`](/Develop/References/MessageInterfaces/DeviceControl.md#BtDisconnect)ディレクティブを受信します。

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtDisconnect",
      "messageId": "0f9950d1-c908-4e02-8c38-8e64e840634c",
      "dialogRequestId": "de0a1fd7-2ef1-4040-9469-3a5dd03ef46b"
    },
    "payload": {}
  }
}
```

## ペアリングされているBluetoothデバイスの削除を処理する {#HandleBluetoothDelete}

ユーザーが保存されているBluetoothデバイスを削除するようにリクエストすると、ClovaはCICを介して、クライアントが指定されたBluetoothデバイスを削除するように指示します。次は、保存されているBluetoothデバイスの削除を処理する動作の流れを示します。

![](/Develop/Assets/Images/CIC_BluetoothControl_Work_Flow5.svg)

ユーザーがBluetoothデバイスの削除をリクエストすると、CICはクライアントに[`DeviceControl.BtDelete`](/Develop/References/MessageInterfaces/DeviceControl.md#BtDelete)ディレクティブを送信します。クライアントは、受信したディレクティブを分析し、指定されたデバイスを削除します。次の[`DeviceControl.BtDelete`](/Develop/References/MessageInterfaces/DeviceControl.md#BtDelete)ディレクティブを受信します。

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtDelete",
      "messageId": "0f9950d1-c908-4e02-8c38-8e64e840634c",
      "dialogRequestId": "de0a1fd7-2ef1-4040-9469-3a5dd03ef46b"
    },
    "payload": {
      "name": "My Speaker",
      "address": "29:01:11:1f:12:89",
      "connected": false,
      "role": "source"
    }
  }
}
```

`payload`の主なフィールドには、以下のものがあります。
* `address`：削除するBluetoothデバイスのデバイスアドレス
* `connected`：削除するBluetoothデバイスとの接続状態
* `name`：削除するBluetoothデバイスの名前
* `role`：指定されたBluetoothデバイスと接続するときのクライアントのロール
