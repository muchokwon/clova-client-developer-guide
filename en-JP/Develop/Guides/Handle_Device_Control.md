# Handling device control

Users can control the action of client through utterance or operation. Upon receiving such a request, Clova sends a directive message to the client to perform the action requested by the user. And the client must be able to suitably handle all action control-related messages sent by Clova for each situation.

This section explains the following:

* [Enabling client device settings](#HandleClientFeatureToggle)
* [Adjusting device volume](#HandleDeviceVolume)
* [Reporting handled results](#HandleActionExecutedResponse)
* [Sharing device state information](#HandleDeviceStateReport)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>After receiving the directive message, the client must always report the result of the response to CIC using the <code>DeviceControl.ActionExecuted</code> or <code>DeviceControl.ActionFailed</code> event messages, every time the handled outcome is successful or not. For more information, see <a href=#HandleActionExecutedResponse>Reporting handled results</a>.</p>
</div>

## Enabling client device settings {#HandleClientFeatureToggle}

Users can activate specific settings of the client device and deactivate when unnecessary. Enabling device settings can be requested using the following two methods:

* User attempts function activation control using speech (general method)
* User attempts function activation control of a specific client remotely from the Clova app

The flow of activating or deactivating device settings is as follows:

![](/Develop/Assets/Images/CIC_DeviceControl_Work_Flow1.svg)

The user makes a voice request ([`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)) to control the client.
The client sends the user request as an event message. For this process, the [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) context must be included in the event message.
CIC determines whether the client can perform the control request of the user by analyzing the `actions[]` field in the [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) context.
When the client is able to handle the request, CIC sends the [`DeviceControl.TurnOn`](/Develop/References/CICInterface/DeviceControl.md#TurnOn) directive message to enable a specific function from the client. This directive message contains the information necessary to enable the function, so the client must find the correct function to activate using the information in the message. An example of a [`DeviceControl.TurnOn`](/Develop/References/CICInterface/DeviceControl.md#TurnOn) directive message that can be received is as follows:

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

The `target` field of `payload` shows the control target and may contain the following information.

* `airplane`: Airplane mode
* `bluetooth`: Bluetooth
* `cellular`: Mobile communication
* `flashlight`: Flashlight
* `gps`: GPS
* `powersave`: Power saving mode
* `soundmode`: Sound mode
* `wifi`: Wi-Fi

The client must enable the function using the information contained in the `target` field. Based on the directive message above, the client must enable airplane mode.

The client must send the result of handling this directive message to CIC using the [`DeviceControl.ActionExecuted`](/Develop/References/CICInterface/DeviceControl.md#ActionExecuted) or [`DeviceControl.ActionFailed`](/Develop/References/CICInterface/DeviceControl.md#ActionFailed) event message.

## Adjusting device volume {#HandleDeviceVolume}

Users can make a request Clova to adjust the volume of the client device playing the audio. This volume adjustment can be requested using the three methods below.

* User attempts volume adjustment using speech (general method)
* User attempts volume adjustment using a button on the client
* User attempts volume adjustment of a specific client remotely from the Clova app

The user may request to adjust volume through speech ([`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)) or device operation. Once the user makes such request, Clova analyzes the user utterance and sends the [`DeviceControl.Increase`](/Develop/References/CICInterface/DeviceControl.md#Increase) directive message to enable a specific function from the client. Also, the client must frequently report the state of the connected Bluetooth device to CIC using the context information, [`Device.DeviceState`](/Develop/References/Context_Objects.md#DeviceState) object.

As the user increases or decreases the volume or specifies a value, the client receives the directive message below. The client must adjust the volume according to the information in the received directive message.

An example of a `DeviceControl.Increase` directive message that can be received is as follows:

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

The `target` field of `payload` is the field to show the control target. The `target` must be `volume` in order to adjust the volume.

Based on the directive message above, the client must increase the volume. Unless specified from the user, the volume must be adjusted by the default increase amount. The default increase amount can be set by the developer company.

The client must send the result of handling this directive message to CIC using the [`DeviceControl.ActionExecuted`](/Develop/References/CICInterface/DeviceControl.md#ActionExecuted) or [`DeviceControl.ActionFailed`](/Develop/References/CICInterface/DeviceControl.md#ActionFailed) event message.

## Sharing device state information {#HandleDeviceStateReport}

There are times when the Clova app needs to check the states of clients registered in the user account. When a client receives a request to share state information **the client must share its state.**

![](/Develop/Assets/Images/CIC_DeviceControl_Work_Flow2.svg)

1. The client (usually the Clova app) sends the [`DeviceControl.RequestStateSynchronization`](/Develop/References/CICInterface/DeviceControl.md#RequestStateSynchronization) event message to CIC.
2. CIC sends the [`DeviceControl.ExpectReportState`](/Develop/References/CICInterface/DeviceControl.md#ExpectReportState) directive message to all clients (excluding the Clova app) registered in the user account through the [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection).
3. Upon receiving the [`DeviceControl.ExpectReportState`](/Develop/References/CICInterface/DeviceControl.md#ExpectReportState) directive message, the client **must report its current state by sending the [`DeviceControl.ReportState`](/Develop/References/CICInterface/DeviceControl.md#ReportState) event message to CIC.**
4. CIC uses the [`DeviceControl.SynchronizeState`](/Develop/References/CICInterface/DeviceControl.md#SynchronizeState) directive message to send the collected state information to the Clova app using the [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection).
5. Once receiving the [`DeviceControl.SynchronizeState`](/Develop/References/CICInterface/DeviceControl.md#SynchronizeState) directive message , the Clova app updates the state of other clients.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The client will receives the <a href="/Develop/References/CICInterface/DeviceControl.md#ExpectReportState"><code>DeviceControl.ExpectReportState</code></a> directive message when it is newly added to the user account or is reconnected to CIC. The client can perform the same actions for the directive message just like for the process of sharing the state information with the Clova app.</p>
</div>

## Reporting handled results {#HandleActionExecutedResponse}

The client must always report the result of the response to CIC using the `DeviceControl.ActionExecuted` or `DeviceControl.ActionFailed` event messages, every time the handled outcome is successful or not. The [`DeviceControl.ActionExecuted`](/Develop/References/CICInterface/DeviceControl.md#ActionExecuted) event message below is sent to CIC when the control is successful.

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

If failed, the client sends an [`DeviceControl.ActionFailed`](/Develop/References/CICInterface/DeviceControl.md#ActionFailed) event message.

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

The `target` field of `payload` shows the control target and may contain the following information.

* `airplane`: Airplane mode
* `app`: App
* `bluetooth`: Bluetooth
* `cellular`: Mobile communication
* `channel`: TV channel
* `flashlight`: Flashlight
* `gps`: GPS
* `powersave`: Power saving mode
* `screenbrightness`: Screen brightness
* `soundmode`: Sound mode
* `volume`: Speaker volume
* `wifi`: Wi-Fi

The `command` field of `payload` is the field showing the command given to the control target of the `target` field, and uses the name of the directive message sent from CIC to give a command to the client.
