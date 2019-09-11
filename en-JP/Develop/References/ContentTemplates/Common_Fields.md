# Common fields
All content templates contain the following common fields. The common fields in content template objects are the top-level element.

| Field name        | Data type    | Description                     | Included |
|----------------|---------|-----------------------------|:---------:|
| `actionList[]`     | [ActionObject](/Develop/References/ContentTemplates/Shared_Objects.md#ActionObject) array | The object array that defines the action to provide to the user in response to the user interaction such as UI touch. The action to provide to the user is sent in the [action URL scheme](#ActionURLScheme) format. The [CardList](/Develop/References/ContentTemplates/CardList.md) template has this field defined inside the `cardList[]` field. | Conditional |
| `failureMessage` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The message to display when the client fails to display the content in the content template. For example, if the client does not support the content template version specified by the `meta.version` field, the client must display this message instead of displaying the provided content. | Always |
| `meta`             | object | The metadata for the content template. | Always |
| `meta.version`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The version of the content template. | Always |
| `subtitle`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The text used to display subtitles or secondary information. | Conditional |

## Common field example

{% raw %}
```json
{
  "type": "Atmosphere",
  "valueOfAtmosphere": {
    "type": "number",
    "value": "23㎍/㎥"
  },
  ...
  "failureMessage": {
    "type": "string",
    "value": "Today, the fine dust levels in Shinjuku are good."
  },
  "subtitle": {
  "type": "string",
  "value": "Today, the fine dust levels in Shinjuku are good."
  },​
  "meta": {
    "version": {
      "type": "string",
      "value": "v0.1"
    }
  }
}
```
{% endraw %}

## Action URL scheme {#ActionURLScheme}
Among the common fields, the following action URL scheme values are included in the `actionList`. These values define which actions to provide in response to the UI interaction of the user. After checking each action URL scheme value, the client must provide the corresponding action to the user.

| Action URL scheme           | The action to be performed by client                                               |
|-----------------------------|------------------------------------------------------------------|
| [clova://app-launch/default-addressbook](#AppLaunchDefaultAddrBook) | Launch the default contacts app   |
| [clova://app-launch/default-browser](#AppLaunchDefaultBrowser)      | Launch the default Web browser |
| [clova://app-launch/default-camera](#AppLaunchDefaultCamera)        | Launch the default camera app   |
| [clova://app-launch/default-email](#AppLaunchDefaultEmail)          | Launch the default email app    |
| [clova://app-launch/default-gallery](#AppLaunchDefaultGallery)      | Launch the default gallery app   |
| [clova://audio-repeat](#AudioRepeat)                                | Output audio     |
| [clova://device-control](#DeviceControl)                            | Control the device       |
| [clova://guide/talking](#GuideTalking)     | Provide a guide on commands                              |
| [clova://ttsRepeat](#TTSRepeat)            | Start TTS                     |
| [clova://webview](#WebView)                | Open a webpage in a WebView                          |

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The difference of an action URL scheme from a directive is that for a directive, the client must perform the corresponding action immediately when it receives a directive. But for an action URL scheme, the client must perform the corresponding action when a user interaction has taken place on UI that is provided on a screen or as other means. When providing an action for an action URL scheme, the client must provide the action that matches the purpose as defined in the table above and must not perform a random action.</p>
</div>

### clova://app-launch/default-addressbook {#AppLaunchDefaultAddrBook}

The client must run the default address book app in response to this scheme. The following is an example of the action URL scheme.

```
clova://app-launch/default-addressbook
```

### clova://app-launch/default-browser {#AppLaunchDefaultBrowser}

The client must run the default web browser in response to this scheme. The following is an example of the action URL scheme.

```
clova://app-launch/default-browser
```

### clova://app-launch/default-camera {#AppLaunchDefaultCamera}

The client must run the default camera app in response to this scheme. The following is an example of the action URL scheme.

```
clova://app-launch/default-camera
```

### clova://app-launch/default-email {#AppLaunchDefaultEmail}

The client must run the default email app in response to this scheme. The following is an example of the action URL scheme.

```
clova://app-launch/default-email
```

### clova://app-launch/default-gallery {#AppLaunchDefaultGallery}

The client must run the default gallery app in response to this scheme. The following is an example of the action URL scheme.

```
clova://app-launch/default-gallery
```

### clova://audio-repeat {#AudioRepeat}

The client must play the default audio file in response to this scheme.

| Parameter name    | Description                         | Included |
|---------------|-----------------------------|:--------:|
| url           | Audio file URL                | Always |

The following is an example of the action URL scheme.

```
clova://audio-repeat?url=http://target.audioFile.url
```

### clova://device-control {#DeviceControl}

The client must control a specific function in response to this scheme.

| Parameter name    | Description                         | Included |
|---------------|-----------------------------|:--------:|
| command       | Control command <ul><li>BtConnect</li><li>BtDisconnect</li><li>BtStartPairing</li><li>BtStopPairing</li><li>Decrease</li><li>Increase</li><li>LaunchApp</li><li>OpenScreen</li><li>SetValue</li><li>TurnOn</li><li>TurnOff</li></ul>                      | Always |
| target        | The control target. <ul><li><code>"airplane"</code>: Airplane mode</li><li><code>"app"</code>: App</li><li><code>"bluetooth"</code>: Bluetooth</li><li><code>"cellular"</code>: Cellular network</li><li><code>"channel"</code>: TV channel</li><li><code>"flashlight"</code>: Flashlight</li><li><code>"gps"</code>: GPS</li><li><code>"powersave"</code>: Power saving mode</li><li><code>"screenbrightness"</code>: Screen brightness</li><li><code>"soundmode"</code>: Sound mode</li><li><code>"volume"</code>: Speaker volume</li><li><code>"wifi"</code>: Wi-Fi</li></ul> | Always |
| value         | Setting value. This value is being designated when the `command` parameter is a `setValue`. It is also applied when setting speaker volume (`"volume"`) or screen brightness (`"screenbrightness"`) or controlling TV channel (`"channel"`). | Conditional |

The following is an example of the action URL scheme.

```
clova://device-control?command=SetValue&target=volume&value=5
clova://device-control?command=TurnOn&target=flashlight
```

### clova://guide/talking {#GuideTalking}

The client must provide a command helper in response to this scheme. The following is an example of the action URL scheme.


```
clova://guide/talking
```

### clova://ttsRepeat {#TTSRepeat}

The client must output specific texts as voice in response to this scheme.

| Parameter name    | Description                         | Included |
|---------------|-----------------------------|:---------:|
| lang          | The language of the text to be read out. <ul><li><code>"en"</code>: English</li><li><code>"ja"</code>: Japanese</li><li><code>"ko"</code>: Korean</li></ul> | Always |
| text          | The text to be read out.                   | Conditional |

The following is an example of the action URL scheme.

```
clova://ttsRepeat?lang=en&text=hello
```

### clova://webview {#WebView}
The client must open a specific page in WebView in response to this scheme.

| Parameter name    | Description                         | Included |
|---------------|-----------------------------|:---------:|
| url           | The URL of the page to open              | Always |
| auth_required | Indicates whether authentication is required or not: `true`: Use the auth API to open the page. `false`: Authentication is not required. | Conditional |

The following is an example of the action URL scheme.

```
clova://webview?url=http://target.page.url&auth_required=true
```
