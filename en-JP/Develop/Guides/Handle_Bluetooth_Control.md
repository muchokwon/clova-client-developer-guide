## Handling client Bluetooth control {#HandleBluetoothControl}

Users can control the Bluetooth connection action of a client through utterance or operation. Upon receiving such a request, Clova sends a directive to the client to perform the action requested by the user. And the client must be able to suitably handle all Bluetooth control-related messages sent by Clova for each situation.

This section explains the following:

* [Handling Bluetooth pairing mode control](#HandleBluetoothPairing)
* [Handling connection requests on new Bluetooth devices](#HandleBluetoothConnect)
* [Handling connection requests on saved Bluetooth devices](#HandleBluetoothConnectExistingDevice)
* [Handling Bluetooth audio playback](#HandleBluetoothPlayAudio)
* [Handling Bluetooth device unpairing](#HandleBluetoothDisconnect)
* [Deleting paired Bluetooth device](#HandleBluetoothDelete)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>After receiving the directive message, the client must always report the result of the response to CIC using the <code>DeviceControl.ActionExecuted</code> or <code>DeviceControl.ActionFailed</code> event messages, every time the handled outcome is successful or not. For more information, see <a href=/CIC/Handle_Device_Control.md#HandleActionExecutedResponse>Reporting handled results</a> of the <a href=/CIC/Handle_Device_Control.md>Handling client action control</a>.</p>
</div>

### Handling Bluetooth connection requests {#HandleBluetoothConnect}

For a user to connect the client with a new Bluetooth device that was never connected before, the client must first enter into the Bluetooth pairing mode. The user can remotely enter the Bluetooth pairing mode of a specific client from the Clova app.

The action flow of handling the entry into the Bluetooth pairing mode is as follows:

![](/Develop/Assets/Images/CIC_BluetoothControl_Work_Flow1.svg)

After a user attempts to enter the Bluetooth pairing mode using Clova app, Clova sends the [`DeviceControl.BtStartPairing`](/Develop/References/CICInterface/DeviceControl.md#BtStartPairing) directive message to the client to enter the Bluetooth pairing mode. After receiving this directive message, the client must enter the Bluetooth pairing mode. An example of a [`DeviceControl.BtStartPairing`](/Develop/References/CICInterface/DeviceControl.md#BtStartPairing) directive message that can be received is as follows:

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

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The client enters the pairing mode only when connecting to a Bluetooth device for the first time.</p>
</div>

Once the client requests a connection to the new Bluetooth device in the pairing mode, Clova must handle the request via CIC so that the client can connect with the Bluetooth device.

The action flow of handling paring with a new Bluetooth device is as follows:

![](/Develop/Assets/Images/CIC_BluetoothControl_Work_Flow2.svg)

Once the user specifies the device to pair with, CIC sends the [`DeviceControl.BtConnect`](/Develop/References/CICInterface/AudioPlayer.md#BtConnect) directive message to pair with the specified device to the client. An example of a [`DeviceControl.BtConnect`](/Develop/References/CICInterface/AudioPlayer.md#BtConnect) directive message that can be received is as follows:

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

Details of `payload` must be analyzed when pairing with a new device.

A simple description of the main fields of `payload` is shown below.
* `address`: The address of the Bluetooth device to remove.
* `connected`: Indicates the connection on the Bluetooth device to remove.
* `name`: The name of the Bluetooth device to remove.
* `role`: The role of the client when connecting to the Bluetooth device.

When connecting with a new Bluetooth device, the Bluetooth device requests for a PIN code. Upon request, this request must be sent to CIC using the [`DeviceControl.BtRequestForPINCode`](/Develop/References/CICInterface/AudioPlayer.md#BtRequestForPINCode) event message.

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

Once the [`DeviceControl.BtRequestForPINCode`](/Develop/References/CICInterface/AudioPlayer.md#BtRequestForPINCode) event message is received and the user enters the PIN code, CIC directs the client to connect with the Bluetooth device that has requested the PIN code using the [`DeviceControl.BtConnectByPINCode`](/Develop/References/CICInterface/AudioPlayer.md#BtConnectByPINCode) directive message. CIC sends the `pincode` field value used for connection to the client.

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

<div class="note">
  <p><strong>Note!</strong></p>
  <p>When connecting with devices that was already paired, the process of entering the pairing mode and checking the PIN code is omitted.</p>
</div>

### Handling Bluetooth audio playback {#HandleBluetoothPlayAudio}

Once the user requests to play audio through the Bluetooth device connected with the client, Clova directs the client to play the audio via CIC. The audio playback through a Bluetooth device can be requested using the following two methods:

* User attempts playback control using speech (general method)
* User attempts playback control on a specific client remotely from the Clova app

The action flow of handling the Bluetooth device audio playback request through user utterance is as follows:

![](/Develop/Assets/Images/CIC_BluetoothControl_Work_Flow3.svg)

Once the user requests to play audio through the Bluetooth device, CIC sends the [`DeviceControl.BtPlay`](/Develop/References/CICInterface/DeviceControl.md#BtPlay) directive message to the client. At this time, the output location of the audio is determined according to the role of client. Also, the client must frequently report the state of the connected Bluetooth device to CIC using the context information, [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) object. An example of a [`DeviceControl.BtPlay`](/Develop/References/CICInterface/DeviceControl.md#BtPlay) directive message that can be received is as follows:

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

If the client role is `"sink"`, the client receives the audio stream from the connected Bluetooth device and output sound via the speaker.
If the client role is `"source"`, the client plays the audio stream that was paused or previously played from the client via the connected Bluetooth device. If there were no audio stream that was previously played or it cannot be specified, send the same event message as the user requesting with an utterance "Play music" to CIC or send an audio content playback request that is suitable for the product UI/UX to CIC.

### Handling Bluetooth disconnection {#HandleBluetoothDisconnect}

If a user requests to disconnect a currently connected Bluetooth device, Clova directs the client to unpair the Bluetooth device via CIC. The action flow of handling the disconnection is shown below.

![](/Develop/Assets/Images/CIC_BluetoothControl_Work_Flow4.svg)

Once the user requests to disconnect the Bluetooth device, CIC sends the [`DeviceControl.BtDisconnect`](/Develop/References/CICInterface/DeviceControl.md#BtDisconnect) directive message to the client. The client must disconnect the specified Bluetooth device or all connected Bluetooth devices according to the contents of the directive message. Also, the client must frequently report the state of the connected Bluetooth device to CIC using the context information, [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) object. An example of a [`DeviceControl.BtDisconnect`](/Develop/References/CICInterface/DeviceControl.md#BtDisconnect) directive message that can be received when disconnecting all Bluetooth devices, is as follows:

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

### Deleting paired Bluetooth devices {#HandleBluetoothDelete}

Once the user requests to delete the saved Bluetooth device, Clova directs the client to delete the specified Bluetooth device via CIC. The action flow of deleting the saved Bluetooth device is as follows:

![](/Develop/Assets/Images/CIC_BluetoothControl_Work_Flow5.svg)

Once the user requests to delete the Bluetooth device, CIC sends the [`DeviceControl.BtDelete`](/Develop/References/CICInterface/DeviceControl.md#BtDelete) directive message to the client. The client analyzes the received directive message and deletes the specified device. An example of a [`DeviceControl.BtDelete`](/Develop/References/CICInterface/DeviceControl.md#BtDelete) directive message that can be received is as follows:

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

A simple description of the main fields of `payload` is shown below.
* `address`: The address of the Bluetooth device to remove.
* `connected`: Indicates the connection on the Bluetooth device to remove.
* `name`: The name of the Bluetooth device to remove.
