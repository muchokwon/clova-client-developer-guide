# DeviceControl

The DeviceControl namespace provides interfaces for controlling client devices or reporting to CIC the result of changing client device settings.

Some user requests may contain a control request of their client device. If the analyzed user request is for controlling the client device, the client will receive a directive message of the `DeviceControl` namespace. Then the client must perform the task instructed by the directive message and then send the result to CIC. For more information, see the [Handling client action control](/Develop/Guides/Handle_Device_Control.md).

The client device can be connected to a third-party Bluetooth device using the `DeviceControl` message. CIC controls the connection to a third-party Bluetooth device by sending a directive message for Bluetooth pairing and connection to the client. The client will frequently report the information related to the paired Bluetooth device using the [`BluetoothInfoObject`](/Develop/References/Context_Objects.md#BluetoothInfoObject) of the [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) context. For more information on creating a connection, refer to each directive message and event message.

The DeviceControl namespace provides the following event messages and directive messages.

| Message name         | Type  | Description                                   |
|------------------|-----------|---------------------------------------------|
| [`ActionExecuted`](#ActionExecuted)       | Event     | Reports to CIC if the client has taken an action on the specified feature or mode.           |
| [`ActionFailed`](#ActionFailed)           | Event     | Reports to CIC if the client cannot or has failed to take an action on the specified feature or mode. |
| [`BtConnect`](#BtConnect)                 | Directive | Instructs the client to connect to a Bluetooth speaker.                               |
| [`BtConnectByPINCode`](#BtConnectByPINCode) | Directive | Instructs the client to connect to the Bluetooth speaker that has requested a PIN code.                        |
| [`BtDelete`](#BtDelete)                   | Directive | Instructs the client to remove a specific device from the list of paired Bluetooth devices.                        |
| [`BtDisconnect`](#BtDisconnect)           | Directive | Instructs the client to disconnect from the Bluetooth speaker.                               |
| [`BtPlay`](#BtPlay)                       | Directive | Instructs the client to play audio content through the connected Bluetooth device.                          |
| [`BtRequestForPINCode`](#BtRequestForPINCode) | Event | Sends the PIN code input request of the Bluetooth speaker to CIC.     |
| [`BtRequestToCancelPinCodeInput`](#BtRequestToCancelPinCodeInput) | Event | Sends the cancellation request for the PIN code input from the Bluetooth speaker to CIC. |
| [`BtRescan`](#BtRescan)                   | Directive | Instructs the client to rescan for Bluetooth devices.                               |
| [`BtStartPairing`](#BtStartPairing)       | Directive | Instructs the client to start the Bluetooth pairing mode.                                       |
| [`BtStopPairing`](#BtStopPairing)         | Directive | Instructs the client to turn off the Bluetooth pairing mode.                                       |
| [`Decrease`](#Decrease)                   | Directive | Instructs the client to turn down the speaker volume or lower screen brightness by a default unit.                     |
| [`ExpectReportState`](#ExpectReportState) | Directive | Instructs the client to report the current state of the client to CIC.                                 |
| [`Increase`](#Increase)                   | Directive | Instructs the client to turn up the speaker volume or increase screen brightness by a default unit.                     |
| [`LaunchApp`](#LaunchApp)                 | Directive | **(Deprecated)** Instructs the client to execute a specified app.                           |
| [`Open`](#Open)                           | Directive | Instructs the client to display a specific screen.                                               |
| [`OpenScreen`](#OpenScreen)               | Directive | **(Deprecated)** Instructs the client to launch the settings screen.                              |
| [`ReportState`](#ReportState)             | Event     | Reports to CIC of the current device state.                     |
| [`RequestStateSynchronization`](#RequestStateSynchronization) | Event   | Requests for the current state of other client devices registered to the current user account.  |
| [`SetValue`](#SetValue)                   | Directive | Instructs the client to set the speaker volume or screen brightness to a specified value.                    |
| [`SynchronizeState`](#SynchronizeState)   | Directive | Instructs the client to update states of other client devices registered on the user account.         |
| [`TurnOff`](#TurnOff)                     | Directive | Instructs the client to turn off or disable a specified feature or mode.                           |
| [`TurnOn`](#TurnOn)                       | Directive | Instructs the client to turn on or enable a specified feature or mode.                                   |

## ActionExecuted event {#ActionExecuted}

Reports to CIC if the client has taken an action on the specified feature or mode.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `command`     | string  | Completed action.<ul><li> <code>"BtConnect"</code></li><li><code>"BtConnectByPINCode"</code></li><li><code>"BtDisconnect"</code></li><li><code>"BtStartPairing"</code></li><li><code>"BtStopPairing"</code></li><li><code>"Decrease"</code></li><li><code>"Increase"</code></li><li><code>"Open"</code></li><li><code>"SetValue"</code></li><li><code>"TurnOn"</code></li><li><code>"TurnOff"</code></li></ul> | Required   |
| `target`      | string  | The control target.<ul><li><code>"airplane"</code>: Airplane mode</li><li><code>"app"</code>: App</li><li><code>"bluetooth"</code>: Bluetooth</li><li><code>"cellular"</code>: Cellular network</li><li><code>"channel"</code>: TV channel</li><li><code>"flashlight"</code>: Flashlight</li><li><code>"gps"</code>: GPS</li><li><code>"powersave"</code>: Power saving mode</li><li><code>"screenbrightness"</code>: Screen brightness</li><li><code>"soundmode"</code>: Sound mode</li><li><code>"volume"</code>: Speaker volume</li><li><code>"wifi"</code>: Wi-Fi</li></ul> | Required     |

### Remarks

Upon receiving the event message, CIC sends the [`SynchronizeState`](#SynchronizeState) directive message to all the clients registered to the user account to inform the state change on a specified client device.

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
      "namespace": "DeviceControl",
      "name": "ActionExecuted",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5"
    },
    "payload": {
      "target": "gps",
      "command": "TurnOn"
    }
  }
}
```

{% endraw %}

### See also
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtConnectByPINCode`](#BtConnectByPINCode)
* [`DeviceControl.BtDisconnect`](#BtDisconnect)
* [`DeviceControl.BtStartPairing`](#BtStartPairing)
* [`DeviceControl.BtStopPairing`](#BtStopPairing)
* [`DeviceControl.Decrease`](#Decrease)
* [`DeviceControl.Increase`](#Increase)
* [`DeviceControl.Open`](#Open)
* [`DeviceControl.SetValue`](#SetValue)
* [`DeviceControl.TurnOff`](#TurnOff)
* [`DeviceControl.TurnOn`](#TurnOn)
* [Handling client action control](/Develop/Guides/Handle_Device_Control.md)
* [Handling client Bluetooth control](/Develop/Guides/Handle_Bluetooth_Control.md)

## ActionFailed event {#ActionFailed}

Reports to CIC if the client cannot or has failed to take an action on the specified feature or mode.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `command`     | string  | Failed action. <ul><li><code>"BtConnect"</code></li><li><code>"BtConnectByPINCode"</code></li><li><code>"BtDisconnect"</code></li><li><code>"BtStartPairing"</code></li><li><code>"BtStopPairing"</code></li><li><code>"Decrease"</code></li><li><code>"Increase"</code></li><li><code>"Open"</code></li><li><code>"SetValue"</code></li><li><code>"TurnOn"</code></li><li><code>"TurnOff"</code></li></ul> | Required   |
| `target`      | string  | The control target.<ul><li><code>"airplane"</code>: Airplane mode</li><li><code>"app"</code>: App</li><li><code>"bluetooth"</code>: Bluetooth</li><li><code>"cellular"</code>: Cellular network</li><li><code>"channel"</code>: TV channel</li><li><code>"flashlight"</code>: Flashlight</li><li><code>"gps"</code>: GPS</li><li><code>"powersave"</code>: Power saving mode</li><li><code>"screenbrightness"</code>: Screen brightness</li><li><code>"soundmode"</code>: Sound mode</li><li><code>"volume"</code>: Speaker volume</li><li><code>"wifi"</code>: Wi-Fi</li></ul> | Required     |

### Remarks

* Upon receiving the event message, CIC sends the [`SynchronizeState`](#SynchronizeState) directive message to all the clients registered to the user account to notify the state change of the client device.
* If the client has failed to execute an app after receiving the [`LaunchApp`](#LaunchApp) directive message, set the `target` field to `"app"`.

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
      "namespace": "DeviceControl",
      "name": "ActionFailed",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5"
    },
    "payload": {
      "target": "gps",
      "command": "TurnOn"
    }
  }
}
```

{% endraw %}

### See also
* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtConnectByPINCode`](#BtConnectByPINCode)
* [`DeviceControl.BtDisconnect`](#BtDisconnect)
* [`DeviceControl.BtStartPairing`](#BtStartPairing)
* [`DeviceControl.BtStopPairing`](#BtStopPairing)
* [`DeviceControl.Decrease`](#Decrease)
* [`DeviceControl.Increase`](#Increase)
* [`DeviceControl.Open`](#Open)
* [`DeviceControl.SetValue`](#SetValue)
* [`DeviceControl.TurnOff`](#TurnOff)
* [`DeviceControl.TurnOn`](#TurnOn)
* [Handling client action control](/Develop/Guides/Handle_Device_Control.md)
* [Handling client Bluetooth control](/Develop/Guides/Handle_Bluetooth_Control.md)

## BtConnect directive {#BtConnect}

Instructs the client to connect to one of the paired Bluetooth speakers. CIC may or may not specify the Bluetooth device to connect.
* When the device is not specified, the client must have its own standard to determine which of the paired devices to connect to. For example, a client may have a policy to attempt connecting in the order of the latest connected devices.
* Determines which paired device to connect depending on whether the client plays the role of <code>"sink"</code> or plays the role of <code>"source"</code> when only the client role is specified.
* When the device is specified, the client must attempt connecting with the specified device.

### Payload fields

* To not specify a device to connect to:
None

* When only the client role is specified.

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `role`        | string  | Role of a client when connecting to a Bluetooth device.<ul><li><code>"sink"</code>: Role of receiving audio stream (mainly speaker).</li><li><code>"source"</code>: Role of transmitting audio stream (sender of audio data).</li></ul> | Always     |

* To specify a device to connect to:

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `address`     | string  | The address of the Bluetooth device to connect to.     | Always     |
| `connected`   | boolean | Indicates the connection to the specified Bluetooth device. <ul><li><code>true</code>: Connected</li><li><code>false</code>: Not connected</li></ul>      | Always     |
| `name`        | string  | The name of the Bluetooth device to connect to.         | Always     |
| `role`        | string  | Role of a client when connecting to the Bluetooth device.<ul><li><code>"sink"</code>: Role of receiving audio stream (mainly a speaker)</li><li><code>"source"</code>: Role of transmitting audio stream (sender of audio data)</li></ul> | Always     |

### Remarks

* If the received directive message does not contain the `payload`, the client must attempt connection with one of the paired Bluetooth devices.
* If the received message only contains the `role` field in the `payload`, connection must be attempted with one of the paired Bluetooth devices according to the client role.
* The client must send the result of handling this directive message to CIC using the [`DeviceControl.ActionExecuted`](#ActionExecuted) or [`DeviceControl.ActionFailed`](#ActionFailed) event message.
* When reporting to CIC, the client must include the actual connection result in the [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) context of the [`DeviceControl.ReportState`](#ReportState) event message.

### Message example

{% raw %}

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
      "role": "sink"
    }
  }
}
```

{% endraw %}

### See also
* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtDisconnect`](#BtDisconnect)
* [`DeviceControl.BtStartPairing`](#BtStartPairing)
* [`DeviceControl.BtStopPairing`](#BtStopPairing)
* [`DeviceControl.TurnOff`](#TurnOff)
* [`DeviceControl.TurnOn`](#TurnOn)
* [Handling Bluetooth device unpairing](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothDisconnect)
* [Handling Bluetooth connection requests](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)
* [Reporting handled results](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [Enabling client device settings](/Develop/Guides/Handle_Device_Control.md#HandleClientFeatureToggle)

## BtConnectByPINCode directive {#BtConnectByPINCode}

Instructs the client to connect to the Bluetooth speaker that has requested a PIN code. CIC provides the user input PIN code in response to this message.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `pinCode`     | string  | The PIN code of the Bluetooth speaker. The client must stop pairing if this field is an empty string (`""`). | Always     |

### Remarks

* Do not send this directive message when requesting to connect with a Bluetooth speaker that does not use a PIN code.
* The client must send the result of handling this directive message to CIC using the [`DeviceControl.ActionExecuted`](#ActionExecuted) or [`DeviceControl.ActionFailed`](#ActionFailed) event message.
* When reporting to CIC, the client must include the actual connection result in the [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) context of the [`DeviceControl.ReportState`](#ReportState) event message.

### Message example

{% raw %}
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
{% endraw %}

### See also

* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtRequestForPINCode`](#BtRequestForPINCode)
* [`DeviceControl.ReportState`](#ReportState)
* [Handling Bluetooth connection requests](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)
* [Reporting handled results](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)

## BtDelete directive {#BtDelete}

Instructs the client to remove a specific device from the list of paired Bluetooth devices.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `address`     | string  | The address of the Bluetooth device to remove.     | Always     |
| `connected`   | boolean | Indicates the connection to the Bluetooth device to remove. <ul><li><code>true</code>: Connected</li><li><code>false</code>: Not connected</li></ul>      | Always     |
| `name`        | string  | The name of the Bluetooth device to remove.         | Always     |
| `role`        | string  | Role of a client when connecting to the Bluetooth device.<ul><li><code>"sink"</code>: Role of receiving audio stream (mainly a speaker)</li><li><code>"source"</code>: Role of transmitting audio stream (sender of audio data)</li></ul> | Always     |

### Remarks

* The client must frequently report the states of paired Bluetooth devices to CIC using the [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) object.
* The client must send the result of handling this directive message to CIC using the [`DeviceControl.ActionExecuted`](#ActionExecuted) or [`DeviceControl.ActionFailed`](#ActionFailed) event message.

### Message example

{% raw %}

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

{% endraw %}

### See also
* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtStartPairing`](#BtStartPairing)
* [`DeviceControl.BtStopPairing`](#BtStopPairing)
* [Handling Bluetooth connection requests](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)
* [Reporting handled results](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [Deleting paired Bluetooth device](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothDelete)

## BtDisconnect directive {#BtDisconnect}

Instructs the client to disconnect a Bluetooth speaker.

### Payload fields

* When disconnecting from all connected devices

None

* To specify a device to connect to:

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `address`     | string  | The address of the Bluetooth device to disconnect.     | Always     |
| `connected`   | boolean | Indicates the connection to the Bluetooth device to disconnect. <ul><li><code>true</code>: Connected</li><li><code>false</code>: Not connected</li></ul>      | Always     |
| `name`        | string  | The name of the Bluetooth device to disconnect.         | Always     |
| `role`        | string  | Role of a client when connecting to the Bluetooth device.<ul><li><code>"sink"</code>: Role of receiving audio stream (mainly a speaker)</li><li><code>"source"</code>: Role of transmitting audio stream (sender of audio data)</li></ul> | Always     |

### Remarks

* The client must frequently report the states of paired Bluetooth devices to CIC using the [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) object.
* The client must send the result of handling this directive message to CIC using the [`DeviceControl.ActionExecuted`](#ActionExecuted) or [`DeviceControl.ActionFailed`](#ActionFailed) event message.

### Message example

{% raw %}

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

{% endraw %}

### See also
* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtStartPairing`](#BtStartPairing)
* [`DeviceControl.BtStopPairing`](#BtStopPairing)
* [`DeviceControl.TurnOff`](#TurnOff)
* [`DeviceControl.TurnOn`](#TurnOn)
* [Handling Bluetooth device unpairing](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothDisconnect)
* [Handling Bluetooth connection requests](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)
* [Reporting handled results](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [Deleting paired Bluetooth device](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothDelete)

## BtPlay directive {#BtPlay}

Instructs the client to play audio content through the connected Bluetooth device. This directive message is sent when the user makes a command like "Play music over Bluetooth." Upon connection with another device via Bluetooth, the client takes the role of receiving and outputting the audio stream (`"sink"`) or the role of transmitting the audio stream (`"source"`). The client must correctly handle this directive message for each role (`role`).

* If the client role is `"sink"`: Receive the audio stream from the Bluetooth device and output sound via the speaker.
* If the client role is `"source"`: Play the audio stream that was paused or previously played from the client via the connected Bluetooth device. If there were no audio stream that was previously played or it cannot be specified, send the same event message as the user requesting with an utterance "Play music" to CIC or send an audio content playback request that is suitable for the product UI/UX to CIC.

### Payload fields

None

### Remarks

* The client must frequently report the states of paired Bluetooth devices to CIC using the [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) object.
* The client must send the result of handling this directive message to CIC using the [`DeviceControl.ActionExecuted`](#ActionExecuted) or [`DeviceControl.ActionFailed`](#ActionFailed) event message.

### Message example

{% raw %}

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

{% endraw %}

### See also
* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtConnect`](#BtConnect)

## BtRequestForPINCode event {#BtRequestForPINCode}

Sends the PIN code input request of the Bluetooth speaker to CIC.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `deviceName`  | string  | The name of the Bluetooth device to connect. This name is displayed on the PIN code input screen. | Required     |

### Remarks

* Send this event message only when the Bluetooth device to connect requests a PIN code. Generally, the PIN code is requested at initial connection.
* After receiving this event message, CIC sends a PIN code to the client through the [`DeviceControl.BtConnectByPINCode`](#BtConnectByPINCode) directive message.

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
{% endraw %}

### See also

* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtConnectByPINCode`](#BtConnectByPINCode)
* [`DeviceControl.BtRequestToCancelPinCodeInput`](#BtRequestToCancelPinCodeInput)
* [Handling Bluetooth connection requests](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)

## BtRequestToCancelPinCodeInput event {#BtRequestToCancelPinCodeInput}

Sends the cancellation request for the PIN code input from the Bluetooth speaker to CIC.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

None

### Remarks

* You can cancel the PIN code input request in special circumstances such as no PIN input for a prolonged time or a lost Bluetooth connection.

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
      "namespace": "DeviceControl",
      "name": "BtRequestToCancelPinCodeInput",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806",
    },
    "payload": {}
  }
}
```
{% endraw %}

### See also

* [`DeviceControl.BtRequestForPINCode`](#BtRequestForPINCode)
* [Handling Bluetooth connection requests](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)

## BtRescan directive {#BtRescan}

Instructs the client to rescan for Bluetooth devices. CIC sends this directive message to the client if a pairing screen that displays the list of available Bluetooth devices in range is displayed or a user requests to refresh the list.

### Payload fields

None

### Remarks

* The client must frequently report the states of paired Bluetooth devices to CIC using the [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) object.
* The client must send the result of handling this directive message to CIC using the [`DeviceControl.ActionExecuted`](#ActionExecuted) or [`DeviceControl.ActionFailed`](#ActionFailed) event message.

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtRescan",
      "messageId": "0f9950d1-c908-4e02-8c38-8e64e840634c",
      "dialogRequestId": "de0a1fd7-2ef1-4040-9469-3a5dd03ef46b"
    },
    "payload": {}
  }
}
```

{% endraw %}

### See also
* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtStartPairing`](#BtStartPairing)
* [`DeviceControl.BtStopPairing`](#BtStopPairing)
* [Handling Bluetooth connection requests](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)
* [Reporting handled results](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [Deleting paired Bluetooth device](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothDelete)

## BtStartPairing directive {#BtStartPairing}

Instructs the client to start the Bluetooth pairing mode.

### Payload fields

None

### Remarks

* The client must frequently report the states of paired Bluetooth devices to CIC using the [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) object.
* The client must send the result of handling this directive message to CIC using the [`DeviceControl.ActionExecuted`](#ActionExecuted) or [`DeviceControl.ActionFailed`](#ActionFailed) event message.

### Message example

{% raw %}

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

{% endraw %}

### See also
* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtDisconnect`](#BtDisconnect)
* [`DeviceControl.BtStopPairing`](#BtStopPairing)
* [`DeviceControl.TurnOff`](#TurnOff)
* [`DeviceControl.TurnOn`](#TurnOn)
* [Handling Bluetooth device unpairing](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothDisconnect)
* [Handling Bluetooth connection requests](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)
* [Reporting handled results](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)

## BtStopPairing directive {#BtStopPairing}

Instructs the client to turn off the Bluetooth pairing mode.

### Payload fields

None

### Remarks

* The client must frequently report the states of paired Bluetooth devices to CIC using the [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) object.
* The client must send the result of handling this directive message to CIC using the [`DeviceControl.ActionExecuted`](#ActionExecuted) or [`DeviceControl.ActionFailed`](#ActionFailed) event message.

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "BtStopPairing",
      "messageId": "0f9950d1-c908-4e02-8c38-8e64e840634c",
      "dialogRequestId": "de0a1fd7-2ef1-4040-9469-3a5dd03ef46b"
    },
    "payload": {}
  }
}
```

{% endraw %}

### See also
* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.BtConnect`](#BtConnect)
* [`DeviceControl.BtDisconnect`](#BtDisconnect)
* [`DeviceControl.BtStartPairing`](#BtStopPairing)
* [`DeviceControl.TurnOff`](#TurnOff)
* [`DeviceControl.TurnOn`](#TurnOn)
* [Handling Bluetooth device unpairing](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothDisconnect)
* [Handling Bluetooth connection requests](/Develop/Guides/Handle_Bluetooth_Control.md#HandleBluetoothConnect)
* [Reporting handled results](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [Enabling client device settings](/Develop/Guides/Handle_Device_Control.md#HandleClientFeatureToggle)

## Decrease directive {#Decrease}

Instructs the client to turn down the speaker volume or lower the screen brightness by the default unit defined on the client or change the TV channel downward.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `target`      | string  | The control target.<ul><li><code>"channel"</code>: TV channel</li><li><code>"screenbrightness"</code>: Screen brightness</li><li><code>"volume"</code>: Speaker volume</li></ul> | Always     |
| `value`       | string  | The amount of screen brightness or speaker volume to change.       | Conditional    |

### Remarks

* You can determine what the default amount of change will be in case the `value` field is empty.
* The client must frequently report the current speaker volume and screen brightness to CIC using the [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) context object.
* Even if the user requests a change of value that exceeds the range of screen brightness or volume that the device can express, Clova sends this directive message by adjusting the requested amount to the device.
* The client must send the result of handling this directive message to CIC using the [`DeviceControl.ActionExecuted`](#ActionExecuted) or [`DeviceControl.ActionFailed`](#ActionFailed) event message.
* Clova normally provides a voice guide ([`SpeechSynthesizer.Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak) directive message ) when sending a directive message to the client for device control. However, if the control is related to speaker output like the `"volume"` is set in the `target` field, Clova does not provide a voice guide with the [`SpeechSynthesizer.Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak) directive message. This is in consideration of the UX such as for a user listening to music. For this, you must implement an action to inform the user that the volume has been changed using the lights or a simple sound effect on the client.

### Message example

{% raw %}

```json
// When a request is made to lower the volume without a requested amount
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "Decrease",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "volume"
    }
  }
}

// When a request is made to lower the volume with a requested amount
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "Decrease",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "volume"
    }
  }
}

// When a request is made to lower the volume with a requested amount
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "Decrease",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "volume",
      "value": "3"
    }
  }
}
```

{% endraw %}

### See also
* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.Increase`](#Increase)
* [`DeviceControl.SetValue`](#SetValue)
* [Reporting handled results](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [Adjusting device volume](/Develop/Guides/Handle_Device_Control.md#HandleDeviceVolume)

## ExpectReportState directive {#ExpectReportState}

Instructs the client to report the current state of the client to CIC. Upon receiving the directive message, the client must immediately report its current state by sending the [`DeviceControl.ReportState`](#ReportState) event message to CIC. After reporting, the client must report the state at every interval in the `intervalInSeconds` field for the duration in the `durationInSeconds` field.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `durationInSeconds` | number  | The duration of report. Reports the current state of the device for the specified duration. The unit is in seconds. If this field is undefined, only one report is made to CIC. | Conditional     |
| `intervalInSeconds` | number  | The reporting interval. Reports the current state of the device at the specified interval. The unit is in seconds. This field is valid only when a value exists in the `durationInSeconds` field. | Conditional     |

### Remarks

* The `DeviceControl.ExpectReportState` directive message is received when the [`DeviceControl.RequestStateSynchronization`](#RequestStateSynchronization) event message is sent to CIC from other clients for synchronization.
* This directive message is sent through a [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection), not as a response to an event message.

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "ExpectReportState",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "durationInSeconds": 600,
      "intervalInSeconds": 60
    }
  }
}
```

{% endraw %}

### See also
* [`DeviceControl.ReportState`](#ReportState)
* [`DeviceControl.RequestStateSynchronization`](#RequestStateSynchronization)

## Increase directive {#Increase}

Instructs the client to turn up the speaker volume or increase the screen brightness by the default unit defined on the client or change the TV channel upward.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `target`      | string  | The control target.<ul><li><code>"channel"</code>: TV channel</li><li><code>"screenbrightness"</code>: Screen brightness</li><li><code>"volume"</code>: Speaker volume</li></ul> | Always     |
| `value`       | string  | The amount of screen brightness or speaker volume to change.       | Conditional    |

### Remarks

* You can determine what the default amount of change will be in case the `value` field is empty.
* Even if the user requests a change of value that exceeds the range of screen brightness or volume that the device can express, Clova sends this directive message by adjusting the requested amount to the device.
* The client must frequently report the current speaker volume and screen brightness to CIC using the [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) context object.
* The client must send the result of handling this directive message to CIC using the [`DeviceControl.ActionExecuted`](#ActionExecuted) or [`DeviceControl.ActionFailed`](#ActionFailed) event message.
* Clova normally provides a voice guide ([`SpeechSynthesizer.Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak) directive message ) when sending a directive message to the client for device control. However, if the control is related to speaker output like the `"volume"` is set in the `target` field, Clova does not provide a voice guide with the [`SpeechSynthesizer.Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak) directive message. This is in consideration of the UX such as for a user listening to music. For this, you must implement an action to inform the user that the volume has been changed using the lights or a simple sound effect on the client.

### Message example

{% raw %}

```json
// // When a request is made to increase the volume without a requested amount
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

// // When a request is made to increase the volume with a requested amount
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

// // When a request is made to increase the volume with a requested amount
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "Increase",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "volume",
      "value": "3"
    }
  }
}
```

{% endraw %}

### See also
* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.Decrease`](#Decrease)
* [`DeviceControl.SetValue`](#SetValue)
* [Reporting handled results](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [Adjusting device volume](/Develop/Guides/Handle_Device_Control.md#HandleDeviceVolume)

## LaunchApp directive {#LaunchApp}

**(Deprecated)** Instructs the client to execute a specified app. The app is specified by a custom URI scheme, a relay page URI, or the app name.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `target`      | string  | The target app to launch. The app can be specified in one of the following ways: <ul><li>Custom URI scheme: A custom URI scheme of the target app (e.g., <code>"{{ book.ServiceEnv.OrientedServiceWithLowerCase }}searchapp://..."</code>).</li><li>Relay page URI: A relay page URI that executes the app if the app is already installed (e.g., <code>"http://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}app.{{ book.ServiceEnv.OrientedServiceWithLowerCase }}.com/..."</code>).</li><li>App name: The name of the app recognized from user speech (e.g., <code>"{{ book.ServiceEnv.OrientedService }} App"</code>).</li></ul> | Always     |

### Remarks

* If the client cannot or has failed to execute the specified app, send the result to CIC using the [`DeviceControl.ActionFailed`](#ActionFailed) event message.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "LaunchApp",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "{{ book.ServiceEnv.OrientedServiceWithLowerCase }}searchapp://..."
    }
  }
}
```

### See also
* [`DeviceControl.ActionFailed`](#ActionFailed)

## Open directive {#Open}

Instructs the client to display a specific screen.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `target`      | string  | The screen to display. Available values are:<ul><li><code>"home"</code>: Home screen</li><li><code>"settings"</code>: Settings screen</li></ul> | Always   |

### Remarks

* The client must send the result of handling this directive message to CIC using the [`DeviceControl.ActionExecuted`](#ActionExecuted) or [`DeviceControl.ActionFailed`](#ActionFailed) event message.

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "Open",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "15ecb9a6-e727-4d58-8ba1-42cc8b63d5c0"
    },
    "payload": {
      "target": "settings"
    }
  }
}
```

{% endraw %}

### See also
* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)

## OpenScreen directive {#OpenScreen}

**(Deprecated)** Instructs the client to launch the settings screen.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `target`      | string  | The screen to display. Currently the value is always `"settings"`. | Always     |

### Remarks

The client must send the result of handling this directive message to CIC using the [`DeviceControl.ActionExecuted`](#ActionExecuted) or [`DeviceControl.ActionFailed`](#ActionFailed) event message.

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "OpenScreen",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "settings"
    }
  }
}
```

{% endraw %}

### See also
* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)

## ReportState event {#ReportState}

Reports to CIC of the current device state.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

None

### Remarks

* Upon receiving the [`DeviceControl.ExpectReportState`](#ExpectReportState) directive message from CIC, the client must report its current state using the `DeviceControl.ReportState` event message.
* The state reported through this event message is delivered to all the clients registered to the user account, through the [`DeviceControl.SynchronizeState`](#SynchronizeState) directive message.
* No directive message is returned as a response to this event message and the HTTP response is returned as `204 No Content`.

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
      "namespace": "DeviceControl",
      "name": "ReportState",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5"
    },
    "payload": {}
  }
}
```

{% endraw %}

### See also
* [`DeviceControl.ExpectReportState`](#ExpectReportState)
* [`DeviceControl.SynchronizeState`](#SynchronizeState)
* [Sharing device state information](/Develop/Guides/Handle_Device_Control.md#HandleDeviceStateReport)

## RequestStateSynchronization event {#RequestStateSynchronization}

Requests for the current state of other client devices registered to the current user account. In return, CIC will send the [`DeviceControl.ExpectReportState`](#ExpectReportState) directive message to all or specific clients registered to the user account.

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | The ID of the target client device. The MAC address or the created UUID format value of the client device. If this field is omitted, CIC sends the [`DeviceControl.ExpectReportState`](#ExpectReportState) directive message to all client devices registered in the user account. <div class="danger"><p><strong>Caution!</strong></p><p>A prior arrangement is necessary to use this feature. Please contact the Clova partnership team.</p></div> | Optional     |

### Remarks

* As a response to this event message, CIC will send the [`DeviceControl.SynchronizeState`](#SynchronizeState) directive message through a [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection).
* No directive message is returned as a response to this event message and the HTTP response is returned as `204 No Content`.

### Message example

{% raw %}

```json
// Example 1: Requests for the state of all devices
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
      "namespace": "DeviceControl",
      "name": "RequestStateSynchronization",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5"
    },
    "payload": {}
  }
}

// Example 2: Requests for the state of a specific device
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
      "namespace": "DeviceControl",
      "name": "RequestStateSynchronization",
      "messageId": "aa8e5c92-c5cb-46ac-af09-ff3da47e1c40"
    },
    "payload": {
      "deviceId":""
    }
  }
}
```

{% endraw %}

### See also
* [`DeviceControl.ExpectReportState`](#ExpectReportState)
* [`DeviceControl.SynchronizeState`](#SynchronizeState)

## SetValue directive {#SetValue}

Instructs the client to set the speaker volume level or screen brightness level to the given value or change the TV channel to the given value.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `target`      | string  | The control target.<ul><li><code>"channel"</code>: TV channel</li><li><code>"screenbrightness"</code>: Screen brightness</li><li><code>"volume"</code>: Speaker volume</li></ul> | Always     |
| `value`       | string  | The value to change to or the TV channel or name.        | Always     |

### Remarks

* The client must frequently report the current speaker volume and screen brightness to CIC using the [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) context object.
* The client must send the result of handling this directive message to CIC using the [`DeviceControl.ActionExecuted`](#ActionExecuted) or [`DeviceControl.ActionFailed`](#ActionFailed) event message.
* Clova normally provides a voice guide ([`SpeechSynthesizer.Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak) directive message ) when sending a directive message to the client for device control. However, if the control is related to speaker output like the `"volume"` is set in the `target` field, Clova does not provide a voice guide with the [`SpeechSynthesizer.Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak) directive message. This is in consideration of the UX such as for a user listening to music. For this, you must implement an action to inform the user that the volume has been changed using the lights or a simple sound effect on the client.

### Message example

{% raw %}

```json
// Change volume
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "SetValue",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "volume",
      "value": "30"
    }
  }
}
```

{% endraw %}

### See also
* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.Decrease`](#Decrease)
* [`DeviceControl.Increase`](#Increase)
* [`DeviceControl.SetValue`](#SetValue)
* [Reporting handled results](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [Adjusting device volume](/Develop/Guides/Handle_Device_Control.md#HandleDeviceVolume)

## SynchronizeState directive {#SynchronizeState}

Instructs the client to update the states of other client devices registered to the user account. Users can use several clients simultaneously with a single user account. For instance, a client may be an app or Wave, which is an exclusive speaker for Clova. Suppose a client app can control other speaker clients. The app will receive the results of controlling other clients through the `DeviceControl.SynchronizeState` directive message and update the states of other clients.

A client may receive this directive message in the following cases:

* When CIC receives the [`DeviceControl.ActionExecuted`](#ActionExecuted) event message or [`DeviceControl.ActionFailed`](#ActionFailed) event message, CIC broadcasts the `DeviceControl.SynchronizeState` directive message to all the clients registered to the user account to report the state change of the client device.
* When CIC receives the [`DeviceControl.ReportState`](#ReportState) event message, CIC broadcasts the `DeviceControl.SynchronizeState` directive message to all the clients registered to the user account to report the state change of the client device.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | The ID of the client device with the updated state. | Always     |
| `deviceState` | [Device.DeviceState](/Develop/References/Context_Objects.md#DeviceState) object | The update information on the device state. If this field is omitted, it means the client device which corresponds to the device ID value of the `deviceId` field is offline.      | Conditional  |

### Remarks

The `DeviceControl.SynchronizeState` directive message is broadcasted to all the clients registered to the user account through a [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection) and does not contain a [dialogue ID (`dialogRequestId`)](/Develop/Guides/Manage_Dialogue_ID_And_Handle_Tasks.md#HandleDirectivesByDialogueID).

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "SynchronizeState",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5"
    },
    "payload": {
      "deviceId": "90de78d7-0735-43a8-8bdc-acc3c8bc4a80",
      "deviceState": {{Device.DeviceState}}
    }
  }
}
```

{% endraw %}

### See also
* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.ReportState`](#ReportState)
* [Reporting handled results](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)

## TurnOff directive {#TurnOff}

Instructs the client to turn off or disable a specified feature or mode. For example, you can use this directive message to turn off the Bluetooth of the client device.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `target`      | string  | The control target.<ul><li><code>"airplane"</code>: Airplane mode</li><li><code>"bluetooth"</code>: Bluetooth</li><li><code>"cellular"</code>: Cellular network</li><li><code>"energysave"</code>: Energy saving mode</li><li><code>"flashlight"</code>: Flashlight</li><li><code>"gps"</code>: GPS</li><li><code>"power"</code>: Power state</li><li><code>"ring"</code>: Ring mode</li><li><code>"silent"</code>: Silent mode</li><li><code>"vibrate"</code>: Vibration mode</li><li><code>"wifi"</code>: Wi-Fi</li></ul> | Always     |

### Remarks

* When turning off or disabling a certain feature or mode, follow the policy of the client device. For example, when sound is disabled, the client policy will determine whether or not the client activates vibration or mute mode.
* The client must frequently report the client state to CIC using the [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) context object.
* The client must send the result of handling this directive message to CIC using the [`DeviceControl.ActionExecuted`](#ActionExecuted) or [`DeviceControl.ActionFailed`](#ActionFailed) event message.

<div class="danger">
  <p><strong>Caution!</strong></p>
  <p>Turn the power off of the client device if the <code>target</code> field is set to <code>"power"</code>.</p>
</div>

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "DeviceControl",
      "name": "TurnOff",
      "messageId": "23bdfff7-b655-46d4-8655-8bb473bf2bf5",
      "dialogRequestId": "3c6eef8b-8427-4b46-a367-0a7a46432519"
    },
    "payload": {
      "target": "bluetooth"
    }
  }
}
```

{% endraw %}

### See also
* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.TurnOn`](#TurnOn)
* [Reporting handled results](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [Enabling client device settings](/Develop/Guides/Handle_Device_Control.md#HandleClientFeatureToggle)

## TurnOn directive {#TurnOn}

Instructs the client to turn on or enable a specified feature or mode. For example, you can use this directive message to turn on the Bluetooth of the client device.

### Payload fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `target`      | string  | The control target.<ul><li><code>"airplane"</code>: Airplane mode</li><li><code>"bluetooth"</code>: Bluetooth</li><li><code>"cellular"</code>: Cellular network</li><li><code>"energysave"</code>: Energy saving mode</li><li><code>"flashlight"</code>: Flashlight</li><li><code>"gps"</code>: GPS</li><li><code>"power"</code>: Power state</li><li><code>"ring"</code>: Ring mode</li><li><code>"silent"</code>: Silent mode</li><li><code>"vibrate"</code>: Vibration mode</li><li><code>"wifi"</code>: Wi-Fi</li></ul> | Always     |

### Remarks

* The client must frequently report the client state to CIC using the [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) context object.
* The client must send the result of handling this directive message to CIC using the [`DeviceControl.ActionExecuted`](#ActionExecuted) or [`DeviceControl.ActionFailed`](#ActionFailed) event message.

### Message example

{% raw %}

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
      "target": "bluetooth"
    }
  }
}
```

{% endraw %}

### See also
* [`DeviceControl.ActionExecuted`](#ActionExecuted)
* [`DeviceControl.ActionFailed`](#ActionFailed)
* [`DeviceControl.TurnOn`](#TurnOn)
* [Reporting handled results](/Develop/Guides/Handle_Device_Control.md#HandleActionExecutedResponse)
* [Enabling client device settings](/Develop/Guides/Handle_Device_Control.md#HandleClientFeatureToggle)
