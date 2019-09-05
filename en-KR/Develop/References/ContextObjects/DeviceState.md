## Device.DeviceState {#DeviceState}
`Device.DeviceState` is a format for reporting to CIC about the state of the client device.

### Object structure
{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    "airplane": {{AirplaneInfoObject}},
    "battery": {{BatteryInfoObject}},
    "bluetooth": {{BluetoothInfoObject}},
    "cellular": {{CellularInfoObject}},
    "channel": {{ChannelInfoObject}},
    "energySavingMode": {{EnergySavingModeInfoObject}},
    "flashLight" {{FlashLightInfoObject}},
    "gps": {{GPSInfoObject}},
    "localTime": {{string}},
    "power": {{PowerInfoObject}},
    "screenBrightness": {{ScreenBrightnessInfoObject}},
    "soundMode": {{SoundModeInfoObject}},
    "soundOutput": {{SoundOutputInfoObject}},
    "volume": {{VolumeInfoObject}},
    "wifi": {{WifiInfoObject}}
  }
}
```

{% endraw %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `airplane`        | [AirplaneInfoObject](#AirplaneInfoObject)               | The airplane mode settings of a client device.      | Optional |
| `battery`         | [BatteryInfoObject](#BatteryInfoObject)                 | The battery information of a client device.              | Optional |
| `bluetooth`       | [BluetoothInfoObject](#BluetoothInfoObject)             | The Bluetooth status of a client device and information of paired devices.       | Optional |
| `cellular`        | [CellularInfoObject](#CellularInfoObject)               | The cellular network settings of a client device. | Optional |
| `channel`         | [ChannelInfoObject](#ChannelInfoObject)                 | The TV channel settings of a client device.         | Optional |
| `energySavingMode` | [EnergySavingModeInfoObject](#EnergySavingModeInfoObject) | The energy saving mode of a client device.     | Optional |
| `flashLight`      | [FlashLightInfoObject](#FlashLightInfoObject)           | The flash light settings of a client device.       | Optional |
| `gps`             | [GPSInfoObject](#GPSInfoObject)                         | The GPS settings of a client device.            | Optional |
| `localTime`       | string | The local time settings on a client device (`YYYY-MM-DDThh:mm:ss±hh:mm`, <a href="https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations" target="_blank">ISO 8601 Combined date and time representations</a>).              | Optional |
| `power`           | [PowerInfoObject](#PowerInfoObject)                     | The power status of a client device.            | Optional |
| `screenBrightness` | [ScreenBrightnessInfoObject](#ScreenBrightnessInfoObject) | The screen brightness of a client device.         | Optional |
| `soundMode`       | [SoundModeInfoObject](#SoundModeInfoObject)             | The sound mode of a client device.           | Optional |
| `soundOutput`     | [SoundOutputInfoObject](#SoundOutputInfoObject)         | This object contains the information on the playback device or method used to output the sound of the client device. | Optional |
| `volume`          | [VolumeInfoObject](#VolumeInfoObject)                   | The speaker volume of a client device.           | Optional |
| `wifi`            | [WifiInfoObject](#WifiInfoObject)                       | The Wi-Fi settings and connection state of a client device.    | Optional |

### Object example
{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    "localTime": "2017-04-06T13:34:15.074361+08:28",
    "bluetooth": {
        "actions": [
            "BtConnect",
            "BtDisconnect",
            "BtStartPairing",
            "BtStopPairing",
            "TurnOff",
            "TurnOn"
        ],
        "btlist": [
            {
                "name": "My Phone",
                "address": "44:00:10:f1:1f:f5",
                "connected": false
            },
            {
                "name": "My Speaker",
                "address": "29:01:11:1f:12:89",
                "connected": true
            }
        ],
        "state": "on"
    },
    "wifi": {
        "actions": [
            "TurnOff",
            "TurnOn"
        ],
        "networks": [
          {
            "name": "home_wlan",
            "connected": true
          },
          {
            "name": "guest_wlan",
            "connected": false
          }
        ],
        "state": "on"
    },
    "battery": {
        "actions": [],
        "value": 99,
        "charging": true
    },
    "flashLight": {
        "actions": [
            "TurnOff",
            "TurnOn"
        ],
        "state": "off"
    }
  }
}
```

{% endraw %}

### AirplaneInfoObject {#AirplaneInfoObject}
This object contains the airplane mode settings of a client device.

#### Object fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`     | string array | A list of the [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) APIs the client can support for airplane mode. Enter the actions that can be performed by the client from the action list.<ul><li>"TurnOff"</li><li>"TurnOn"</li></ul> | Required |
| `state`         | string | Indicates the state of the airplane mode:<ul><li><code>"off"</code>: Airplane mode is turned off</li><li><code>"on"</code>: Airplane mode is turned on</li></ul> | Required |

#### Object example

{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "airplane": {
        "actions": [
            "TurnOff",
            "TurnOn"
        ],
        "state": "on"
    },
    ...
  }
}
```

{% endraw %}

### BatteryInfoObject {#BatteryInfoObject}
This object contains the battery state of a client device.

#### Object fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`     | string array | A list of the [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) APIs the client can support for battery. There are no currently supported actions. | Required |
| `charging`      | boolean | Indicates whether the client is charging or not.<ul><li><code>true</code>: The client is charging</li><li><code>false</code>: The client is not charging</li></ul> | Required |
| `value`         | number | Remaining battery You must enter a number from 0 to 100 and the unit is in percent (%). | Required |

#### Object example

{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "battery": {
        "actions": [],
        "value": 98,
        "charging": false
    },
    ...
  }
}
```

{% endraw %}

### BluetoothInfoObject {#BluetoothInfoObject}
This object contains the Bluetooth information including Bluetooth status and paired devices.

#### Object fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | A list of the [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) APIs the client can support for Bluetooth. Enter the actions that can be performed by the client from the action list. <ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li><li><code>"BtConnect"</code></li><li><code>"BtConnectByPINCode"</code></li><li><code>"BtDisconnect"</code></li><li><code>"BtStartPairing"</code></li><li><code>"BtStopPairing"</code></li></ul> | Required |
| `btlist[]`              | object array | The object array of information on the Bluetooth device that has a pairing history with the client.         | Required |
| `btlist[].address`      | string       | The address of the Bluetooth device.                  | Required |
| `btlist[].connected`    | boolean      | Indicates whether or not the Bluetooth device is connected to the client device. <ul><li><code>true</code>: Connected</li><li><code>false</code>: Not connected</li></ul> | Required |
| `btlist[].name`         | string       | The name of the Bluetooth device.                      | Required |
| `btlist[].role`         | string       | Role of a client when connecting to the Bluetooth device.<ul><li><code>"sink"</code>: Role of receiving audio stream (mainly a speaker)</li><li><code>"source"</code>: Role of transmitting audio stream (sender of audio data)</li></ul> | Required |
| `connecting`            | string       | Indicates whether or not the client is connected to a Bluetooth device. <ul><li><code>"on"</code>: Connected</li><li><code>"off"</code>: Not connected</li></ul> | Required |
| `pairing`               | string       | Indicates whether or not the Bluetooth pairing mode is turned on. <ul><li><code>"on"</code>: Pairing mode is on</li><li><code>"off"</code>: Pairing mode is off</li></ul> | Required |
| `playerInfo`            | object       | Object containing the information of music being played through the Bluetooth connection.  | Optional |
| `playerInfo.albumTitle` | string       | Album title of the music being played over Bluetooth.                 | Optional |
| `playerInfo.artistName` | string       | Artist of the music being played over Bluetooth.                 | Optional |
| `playerInfo.state`      | string       | Playback state of the music being played over Bluetooth. <ul><li><code>"paused"</code>: Paused</li><li><code>"playing"</code>: Playing</li><li><code>"stopped"</code>: Stopped</li></ul>:                  | Optional |
| `playerInfo.trackTitle` | string       | Title of the music being played over Bluetooth.                     | Optional |
| `scanlist[]`            | object array | The object array of information on scanned Bluetooth devices.   | Required |
| `scanlist[].address`    | string       | The address of the Bluetooth device.                  | Required |
| `scanlist[].name`       | string       | The name of the Bluetooth device.                      | Required |
| `scanlist[].role`       | string       | Role of a client when connecting to the Bluetooth device.<ul><li><code>"sink"</code>: Role of receiving audio stream (mainly a speaker)</li><li><code>"source"</code>: Role of transmitting audio stream (sender of audio data)</li></ul> | Required |
| `scanning`              | string       | Indicates whether or not the Bluetooth scanning mode is on. <ul><li><code>"on"</code>: Scanning mode is on</li><li><code>"off"</code>: Scanning mode is off</li></ul> | Required |
| `state`                 | string       | Indicates the Bluetooth status. <ul><li><code>"off"</code>: Bluetooth is turned off</li><li><code>"on"</code>: Bluetooth is turned on</li></ul> | Required |

#### Object example

{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "bluetooth": {
        "actions": [
            "BtConnect",
            "BtDisconnect",
            "BtStartPairing",
            "BtStopPairing",
            "TurnOff",
            "TurnOn"
        ],
        "btlist": [
            {
                "name": "My Phone",
                "address": "44:00:10:f1:1f:f5",
                "connected": false,
                "role": "source"
            },
            {
                "name": "My Speaker",
                "address": "29:01:11:1f:12:89",
                "connected": true,
                "role": "sink"
            }
        ],
        "scanlist": [
            {
                "name": "A New Phone",
                "address": "04:11:10:01:1a:45",
                "role": "sink"
            },
            {
                "name": "A New Speaker",
                "address": "19:02:11:6f:3f:74",
                "role": "source"
            }
        ],
        "state": "on",
        "pairing": "on",
        "scanning": "on",
        "connecting": "off"
    },
    ...
  }
}
```

{% endraw %}

### CellularInfoObject {#CellularInfoObject}
This object contains the cellular network settings of a client device.

#### Object fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | A list of the [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) APIs the client can support for cellular network settings. Enter the actions that can be performed by the client from the action list. <ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> | Required |
| `state`              | string       | Indicates the state of the cellular network. <ul><li><code>"off"</code>: Cellular network is turned off</li><li><code>"on"</code>: Cellular network is turned on</li></ul> | Required |

#### Object example

{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "cellular": {
        "actions": [
            "TurnOff",
            "TurnOn"
        ],
        "state": "off"
    },
    ...
  }
}
```

{% endraw %}

### ChannelInfoObject {#ChannelInfoObject}
This object contains the TV channel settings of a client device.

#### Object fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`     | string array | A list of the [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) APIs the client can support for TV channel settings. Enter the actions that can be performed by the client from the action list.<ul><li><code>"Decrease"</code></li><li><code>"Increase"</code></li><li><code>"SetValue"</code></li></ul> | Required |

#### Object example

{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "channel": {
        "actions": [
            "Decrease",
            "Increase",
            "SetValue"
        ]
    },
    ...
  }
}
```

{% endraw %}


### EnergySavingModeInfoObject {#EnergySavingModeInfoObject}
EnergySavingModeInfoObject contains the energy saving mode of a client device.

#### Object fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | A list of the [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) APIs the client can support for power saving mode. Enter the actions that can be performed by the client from the action list. <ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> | Required |
| `state`              | string       | Indicates the state of the power saving mode. <ul><li><code>"off"</code>: Power saving mode is turned off</li><li><code>"on"</code>: Power saving mode is turned on</li></ul> | Required |

#### Object example

{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "energySavingMode": {
        "actions": [
            "TurnOff",
            "TurnOn"
        ],
        "state": "off"
    },
    ...
  }
}
```

{% endraw %}


### FlashLightInfoObject {#FlashLightInfoObject}
This object contains the flashlight settings of a client device.

#### Object fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | A list of the [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) APIs the client can support for the flashlight. Enter the actions that can be performed by the client from the action list. <ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> | Required |
| `state`              | string       | Indicates the state of the flashlight. <ul><li><code>"off"</code>: Flashlight is turned off</li><li><code>"on"</code>: Flashlight is turned on</li></ul> | Required |

#### Object example

{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "flashLight": {
        "actions": [
            "TurnOff",
            "TurnOn"
        ],
        "state": "off"
    },
    ...
  }
}
```

{% endraw %}

### GPSInfoObject {#GPSInfoObject}
This object contains the GPS state of a client device.

#### Object fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | A list of the [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) APIs the client can support for GPS. Enter the actions that can be performed by the client from the action list. <ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> | Required |
| `state`              | string       | Indicates the state of the GPS. <ul><li><code>"off"</code>: GPS is turned off</li><li><code>"on"</code>: GPS is turned on</li></ul> | Required |

#### Object example

{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "gps": {
        "actions": [
            "TurnOff",
            "TurnOn"
        ],
        "state": "off"
    },
    ...
  }
}
```

{% endraw %}

### PowerInfoObject {#PowerInfoObject}
This object contains the power state of a client device.

#### Object fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | A list of the [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) APIs the client can support for power states. Enter the actions that can be performed by the client from the action list. <ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> | Required |
| `state`              | string       | Indicates the power state of the device. <ul><li><code>"active"</code>: The client device is turned on</li><li><code>"idle"</code>: The client device is turned off</li></ul> | Required |

#### Object example

{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "power": {
        "actions": [
            "TurnOff",
            "TurnOn"
        ],
        "state": "active"
    },
    ...
  }
}
```

{% endraw %}

### ScreenBrightnessInfoObject {#ScreenBrightnessInfoObject}
The object containing the screen brightness of a client device.

#### Object fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | A list of the [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) APIs the client can support for screen brightness. Enter the actions that can be performed by the client from the action list. <ul><li><code>"Decrease"</code></li><li><code>"Increase"</code></li><li><code>"SetValue"</code></li></ul> | Required |
| `max`                | number       | The maximum screen brightness available on the client device.    | Required |
| `min`                | number       | The minimum screen brightness available on the client device.    | Required |
| `value`              | number       | The current screen brightness of the client device.                   | Required |

#### Object example

{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "screenBrightness": {
        "actions": [
            "Decrease",
            "Increase",
            "SetValue"
        ],
        "min": 0,
        "max": 100,
        "value": 70
    },
    ...
  }
}
```

{% endraw %}

### SoundModeInfoObject {#SoundModeInfoObject}
This object contains the sound settings of a client device.

#### Object fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | A list of the [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) APIs the client can support for sound modes. Enter the actions that can be performed by the client from the action list. <ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul>  | Required |
| `state`              | string       | Indicates the sound mode on the client device. <ul><li><code>"ring"</code>: Ring mode</li><li><code>"silent"</code>: Silent mode</li><li><code>"vibrate"</code>: Vibration mode</li></ul> | Required |

#### Object example

{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "soundMode": {
        "actions": [
            "TurnOff",
            "TurnOn"
        ],
        "state": "vibrate"
    },
    ...
  }
}
```

{% endraw %}

### SoundOutputInfoObject {#SoundOutputInfoObject}
This object contains the information on the playback device or method used to output the sound of the client device.

#### Object fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `type`        | string  | The playback device or method used to output sound. <ul><li><code>"builtin"</code>: Built-in speaker</li><li><code>"aux"</code>: Cable terminal</li><li><code>"bluetooth"</code>: Bluetooth</li><li><code>"airplay"</code>: <a target="_blank" href="https://support.apple.com/en-gb/HT204289">AirPlay</a></li></ul> | Required |

#### Object example

{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "soundOutput": {
        "type": "builtin"
    },
    ...
  }
}
```

{% endraw %}

### VolumeInfoObject {#VolumeInfoObject}
This object contains the volume information of the client device.

#### Object fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | A list of the [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) APIs the client can support for speaker volume. Enter the actions that can be performed by the client from the action list. <ul><li><code>"Decrease"</code></li><li><code>"Increase"</code></li><li><code>"SetValue"</code></li></ul> | Required |
| `max`                | number       | The maximum speaker volume available on the client device.    | Required |
| `min`                | number       | The minimum speaker volume available on the client device.    | Required |
| `value`              | number       | The current speaker volume of the client device.               | Required |
| `warning`            | number       | The threshold volume at which to warn the user. For example, if this field is set to `8` and if the user sets the volume to `8`, Clova will send a message to the client to inform the user, "Volume 10 is too loud. Do you want to lower the setting?" | Optional |

#### Object example

{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "volume": {
        "actions": [
            "Decrease",
            "Increase",
            "SetValue"
        ],
        "min": 0,
        "max": 60,
        "warning": 50,
        "value": 40
    },
    ...
  }
}
```

{% endraw %}

### WifiInfoObject {#WifiInfoObject}
This object contains information on the Wi-Fi settings and connection state of a client device.

#### Object fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`            | string array | A list of the [`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md) APIs the client can support for Wi-Fi. Enter the actions that can be performed by the client from the action list. <ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> | Required |
| `networks[]`           | object array | The object array of information on scanned Wi-Fi. | Required |
| `networks[].connected` | boolean      | Indicates the Wi-Fi connection state. <ul><li><code>true</code>: Connected</li><li><code>false</code>: Not connected</li></ul> | Required |
| `networks[].name`      | string       | The name of the Wi-Fi (SSID).               | Required |
| `state`                | string       | Indicates the state of the Wi-Fi mode. <ul><li><code>"off"</code>: Wi-Fi is turned off</li><li><code>"on"</code>: Wi-Fi is turned on</li></ul> | Required |

#### Object example

{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "DeviceState"
  },
  "payload": {
    ...
    "wifi": {
      "actions": [
          "TurnOff",
          "TurnOn"
      ],
      "networks": [
        {
          "name": "home_wlan",
          "connected": true
        },
        {
          "name": "guest_wlan",
          "connected": false
        }
      ],
      "state": "on"
    },
    ...
  }
}
```

{% endraw %}

### See also
* [`DeviceControl` API](/Develop/References/MessageInterfaces/DeviceControl.md)
* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
