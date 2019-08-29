## Device.DeviceState {#DeviceState}
`Device.DeviceState`は、クライアントデバイスの状態をCICにレポートするときに使用されるメッセージ形式です。

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

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `airplane`        | [AirplaneInfoObject](#AirplaneInfoObject)               | クライアントデバイスの機内モードの設定情報をレポートするときに使用されるオブジェクト      | 任意 |
| `battery`         | [BatteryInfoObject](#BatteryInfoObject)                 | クライアントデバイスのバッテリーの情報をレポートするときに使用されるオブジェクト              | 任意 |
| `bluetooth`       | [BluetoothInfoObject](#BluetoothInfoObject)             | クライアントデバイスのBluetoothの状態およびBluetoothデバイスの接続状態をレポートするときに使用されるオブジェクト       | 任意 |
| `cellular`        | [CellularInfoObject](#CellularInfoObject)               | クライアントデバイスのセルラーネットワークの状態をレポートするときに使用されるオブジェクト | 任意 |
| `channel`         | [ChannelInfoObject](#ChannelInfoObject)                 | クライアントデバイスのテレビチャンネルの設定情報をレポートするときに使用されるオブジェクト         | 任意 |
| `energySavingMode` | [EnergySavingModeInfoObject](#EnergySavingModeInfoObject) | クライアントデバイスの省エネモードの情報をレポートするときに使用されるオブジェクト     | 任意 |
| `flashLight`      | [FlashLightInfoObject](#FlashLightInfoObject)           | クライアントデバイスのフラッシュライトの設定情報をレポートするときに使用されるオブジェクト       | 任意 |
| `gps`             | [GPSInfoObject](#GPSInfoObject)                         | クライアントデバイスのGPS設定情報をレポートするときに使用されるオブジェクト            | 任意 |
| `localTime`       | string | クライアントデバイスに設定されている現地時刻（`YYYY-MM-DDThh:mm:ss±hh:mm`、<a href="https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations" target="_blank">ISO 8601 Combined date and time representations</a>）              | 任意 |
| `power`           | [PowerInfoObject](#PowerInfoObject)                     | クライアントデバイスの電源の状態をレポートするときに使用されるオブジェクト            | 任意 |
| `screenBrightness` | [ScreenBrightnessInfoObject](#ScreenBrightnessInfoObject) | クライアントデバイスの画面の明るさの情報をレポートするときに使用されるオブジェクト         | 任意 |
| `soundMode`       | [SoundModeInfoObject](#SoundModeInfoObject)             | クライアントデバイスのサウンドモードの情報をレポートするときに使用されるオブジェクト           | 任意 |
| `soundOutput`     | [SoundOutputInfoObject](#SoundOutputInfoObject)         | クライアントデバイスの音声出力に使用されているオーディオ装置や再生方式に関する情報をレポートするときに使用されるオブジェクトです。 | 任意 |
| `volume`          | [VolumeInfoObject](#VolumeInfoObject)                   | クライアントデバイスのスピーカーの音量情報をレポートするときに使用されるオブジェクト           | 任意 |
| `wifi`            | [WifiInfoObject](#WifiInfoObject)                       | クライアントデバイスのWi-Fiの状態と、Wi-Fiネットワークの接続情報をレポートするときに使用されるオブジェクト    | 任意 |

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
クライアントデバイスの機内モードの設定情報をレポートするときに使用されるオブジェクトです。

#### Object fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`     | string array | 機内モードに関連して実行できる[`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md)APIのリスト。以下のリストのうち、クライアントデバイスが実際に実行できるアクションを入力します。<ul><li>"TurnOff"</li><li>"TurnOn"</li></ul> |  |
| `state`         | string | 機内モードの設定状態。<ul><li><code>"off"</code>：オフになっている</li><li><code>"on"</code>：オンになっている</li></ul> |  |

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
クライアントデバイスのバッテリーの状態情報をレポートするときに使用されるオブジェクトです。

#### Object fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`     | string array | バッテリーに関連して実行できる[`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md)APIのリスト。現在、サポートされているアクションがありません。 |  |
| `charging`      | boolean | 充電しているかどうかを示す値。<ul><li><code>true</code>：充電している</li><li><code>false</code>：充電していない</li></ul> |  |
| `value`         | number | バッテリーの残量。0から100までの数値を入力します。単位はパーセント（%）です。 |  |

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
クライアントデバイスのBluetoothの状態およびBluetoothデバイスの接続状態をレポートするときに使用されるオブジェクトです。

#### Object fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | Bluetooth接続に関連して実行できる[`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md)APIのリスト。次のリストのうち、クライアントデバイスが実際に実行できるアクションを入力します。<ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li><li><code>"BtConnect"</code></li><li><code>"BtConnectByPINCode"</code></li><li><code>"BtDisconnect"</code></li><li><code>"BtStartPairing"</code></li><li><code>"BtStopPairing"</code></li></ul> |  |
| `btlist[]`              | object array | ペアリングされたことがあるBluetoothデバイスの情報を持つオブジェクト配列         |  |
| `btlist[].address`      | string       | Bluetoothデバイスのデバイスアドレス                  |  |
| `btlist[].connected`    | boolean      | Bluetoothデバイスとの接続状態。<ul><li><code>true</code>：接続している</li><li><code>false</code>：接続していない</li></ul> |  |
| `btlist[].name`         | string       | Bluetoothデバイスの名前                      |  |
| `btlist[].role`         | string       | そのBluetoothデバイスと接続するときのクライアントの役割。<ul><li><code>"sink"</code>：オーディオストリームを受信する役割（主にスピーカー）</li><li><code>"source"</code>：オーディオストリームを送信する役割（ストリームデータの送信者）</li></ul> |  |
| `connecting`            | string       | Bluetoothデバイスとの接続を試行しているか。<ul><li><code>"on"</code>：接続を試行している</li><li><code>"off"</code>：接続を試行していない</li></ul> |  |
| `pairing`               | string       | Bluetoothペアリングモードの状態。<ul><li><code>"on"</code>：ペアリングモードがオンになっている</li><li><code>"off"</code>：ペアリングモードがオフになっている</li></ul> |  |
| `playerInfo`            | object       | Bluetooth接続で再生されているストリームの情報を持つオブジェクト  | 任意 |
| `playerInfo.albumTitle` | string       | Bluetoothで再生されているストリームのアルバムのタイトル                 | 任意 |
| `playerInfo.artistName` | string       | Bluetoothで再生されているストリームのアーティスト名                 | 任意 |
| `playerInfo.state`      | string       | Bluetoothで再生されているストリームの再生状態。<ul><li><code>"paused"</code>：再生が一時停止されている</li><li><code>"playing"</code>：再生中</li><li><code>"stopped"</code>：再生が停止されている</li></ul>                  | 任意 |
| `playerInfo.trackTitle` | string       | Bluetoothで再生されているストリームの曲名                     | 任意 |
| `scanlist[]`            | object array | スキャンされたBluetoothデバイスの情報を持つオブジェクト配列   |  |
| `scanlist[].address`    | string       | Bluetoothデバイスのデバイスアドレス                  |  |
| `scanlist[].name`       | string       | Bluetoothデバイスの名前                      |  |
| `scanlist[].role`       | string       | そのBluetoothデバイスと接続するときのクライアントの役割。<ul><li><code>"sink"</code>：オーディオストリームを受信する役割（主にスピーカー）</li><li><code>"source"</code>：オーディオストリームを送信する役割（ストリームデータの送信者）</li></ul> |  |
| `scanning`              | string       | Bluetoothスキャンモードの状態。<ul><li><code>"on"</code>：スキャンモードがオンになっている</li><li><code>"off"</code>：スキャンモードがオフになっている</li></ul> |  |
| `state`                 | string       | Bluetoothのオン/オフ状態<ul><li><code>"off"</code>：オフになっている</li><li><code>"on"</code>：オンになっている</li></ul> |  |

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
クライアントデバイスのセルラーネットワークの状態をレポートするときに使用されるオブジェクトです。

#### Object fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | セルラーネットワークに関連して実行できる[`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md)APIのリスト。次のリストのうち、クライアントデバイスが実際に実行できるアクションを入力します。<ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> |  |
| `state`              | string       | セルラーネットワークのオン/オフ状態。<ul><li><code>"off"</code>：オフになっている</li><li><code>"on"</code>：オンになっている</li></ul> |  |

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
クライアントデバイスのテレビチャンネルの設定情報をレポートするときに使用されるオブジェクトです。

#### Object fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`     | string array | テレビチャンネルの設定に関連して実行できる[`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md)APIのリスト。次のリストのうち、クライアントデバイスが実際に実行できるアクションを入力します。<ul><li><code>"Decrease"</code></li><li><code>"Increase"</code></li><li><code>"SetValue"</code></li></ul> |  |

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
クライアントデバイスの省エネモードの情報をレポートするときに使用されるオブジェクトです。

#### Object fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | 省エネモードに関連して実行できる[`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md)APIのリスト。次のリストのうち、クライアントデバイスが実際に実行できるアクションを入力します。<ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> |  |
| `state`              | string       | 省エネモードの設定状態。<ul><li><code>"off"</code>：オフになっている</li><li><code>"on"</code>：オンになっている</li></ul> |  |

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
クライアントデバイスのフラッシュライトの設定情報をレポートするときに使用されるオブジェクトです。

#### Object fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | フラッシュライトに関連して実行できる[`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md)APIのリスト。次のリストのうち、クライアントデバイスが実際に実行できるアクションを入力します。<ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> |  |
| `state`              | string       | フラッシュライトの現在の状態。<ul><li><code>"off"</code>：オフになっている</li><li><code>"on"</code>：オンになっている</li></ul> |  |

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
クライアントデバイスのGPSの状態をレポートするときに使用されるオブジェクトです。

#### Object fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | GPSに関連して実行できる[`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md)APIのリスト。次のリストのうち、クライアントデバイスが実際に実行できるアクションを入力します。<ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> |  |
| `state`              | string       | GPSの現在の状態。<ul><li><code>"off"</code>：オフになっている</li><li><code>"on"</code>：オンになっている</li></ul> |  |

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
クライアントデバイスの電源の状態をレポートするときに使用されるオブジェクトです。

#### Object fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | 電源の状態に関連して実行できる[`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md)APIのリスト。次のリストのうち、クライアントデバイスが実際に実行できるアクションを入力します。<ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> |  |
| `state`              | string       | 電源の状態。<ul><li><code>"active"</code>：クライアントデバイスがオンになっている</li><li><code>"idle"</code>：クライアントデバイスがオフになっている</li></ul> |  |

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
クライアントデバイスの画面の明るさの状態情報をレポートするときに使用されるオブジェクトです。

#### Object fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | 画面の明るさに関連して実行できる[`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md)APIのリスト。次のリストのうち、クライアントデバイスが実際に実行できるアクションを入力します。<ul><li><code>"Decrease"</code></li><li><code>"Increase"</code></li><li><code>"SetValue"</code></li></ul> |  |
| `max`                | number       | クライアントデバイスの画面に設定できる明るさの最大値    |  |
| `min`                | number       | クライアントデバイスの画面に設定できる明るさの最小値    |  |
| `value`              | number       | 現在のクライアントデバイスの画面の明るさ                   |  |

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
クライアントデバイスのサウンドモードの情報をレポートするときに使用されるオブジェクトです。

#### Object fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | サウンドモードに関連して実行できる[`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md)APIのリスト。次のリストのうち、クライアントデバイスが実際に実行できるアクションを入力します。<ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul>  |  |
| `state`              | string       | サウンドモードの設定状態。<ul><li><code>"ring"</code>：着信モード</li><li><code>"silent"</code>：サイレントモード</li><li><code>"vibrate"</code>：バイブモード</li></ul> |  |

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
クライアントデバイスの音声出力に使用されているオーディオ装置や再生方式に関する情報をレポートするときに使用されるオブジェクトです。

#### Object fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `type`        | string  | 音声出力に使用されているオーディオ装置または再生方式。<ul><li><code>"builtin"</code>：内蔵スピーカー</li><li><code>"aux"</code>：補助入力</li><li><code>"bluetooth"</code>：Bluetooth</li><li><code>"airplay"</code>：<a target="_blank" href="https://support.apple.com/en-gb/HT204289">AirPlay</a></li></ul> |  |

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
クライアントデバイスのスピーカーの音量情報をレポートするときに使用されるオブジェクト

#### Object fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`          | string array | スピーカー音量に関連して実行できる[`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md)APIのリスト。次のリストのうち、クライアントデバイスが実際に実行できるアクションを入力します。<ul><li><code>"Decrease"</code></li><li><code>"Increase"</code></li><li><code>"SetValue"</code></li></ul> |  |
| `max`                | number       | クライアントデバイスのスピーカーに設定できる音量の最大値    |  |
| `min`                | number       | クライアントデバイスのスピーカーに設定できる音量の最小値    |  |
| `value`              | number       | クライアントデバイスの現在のスピーカー音量の大きさ               |  |
| `warning`            | number       | クライアントデバイスのスピーカーに、特定の数値以上を設定する場合に警告を出す値。このフィールドの値が`8`で、ユーザーが`8`以上の値を音量に設定しようとすると、ユーザーに対して、「10は大変大きな音量です。続行しますか?」と聞き返します。 | 任意 |

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
クライアントデバイスのWi-Fiの状態と、Wi-Fiネットワークの接続情報をレポートするときに使用されるオブジェクトです。

#### Object fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `actions[]`            | string array | Wi-Fiに関連して実行できる[`DeviceControl`](/Develop/References/MessageInterfaces/DeviceControl.md)APIのリスト。次のリストのうち、クライアントデバイスが実際に実行できるアクションを入力します。<ul><li><code>"TurnOff"</code></li><li><code>"TurnOn"</code></li></ul> |  |
| `networks[]`           | object array | 検索されたWi-Fiネットワークの情報を持つオブジェクト配列 |  |
| `networks[].connected` | boolean      | Wi-Fiネットワークの接続状態。<ul><li><code>true</code>：接続している</li><li><code>false</code>：接続していない</li></ul> |  |
| `networks[].name`      | string       | Wi-Fiネットワークの名前（SSID）               |  |
| `state`                | string       | Wi-Fiのオン/オフ状態。<ul><li><code>"off"</code>：オフになっている</li><li><code>"on"</code>：オンになっている</li></ul> |  |

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

### 次の項目も参照してください。
* [`DeviceControl` API](/Develop/References/MessageInterfaces/DeviceControl.md)
* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
