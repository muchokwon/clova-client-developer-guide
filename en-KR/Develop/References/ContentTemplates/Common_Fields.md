# Common fields
All content templates contain the following common fields. The common fields in content template objects are the top-level element.

| Field name        | Data type    | Description                     | Included |
|----------------|---------|-----------------------------|:---------:|
| `actionList[]`     | [ActionObject](/Develop/References/ContentTemplates/Shared_Objects.md#ActionObject) array | The object array that defines the action to provide to the user in response to the user interaction such as UI touch. The action to provide to the user is sent in the [action URI scheme](#ActionURIScheme) format. The [CardList](/Develop/References/ContentTemplates/CardList.md) template has this field defined inside the `cardList[]` field. | Conditional |
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

## Action URI scheme {#ActionURIScheme}
Among the common fields, the action URI scheme values below are included in the `actionList` field. These values define which actions to provide in response to the UI interaction of the user. After checking each action URI scheme value, the client must provide the corresponding action to the user.

| Action URI scheme           | Action to be performed by client                                               |
|-----------------------------|------------------------------------------------------------------|
| [clova://app-launch/default-addressbook](#AppLaunchDefaultAddrBook) | Launch the default contacts app   |
| [clova://app-launch/default-browser](#AppLaunchDefaultBrowser)      | Launch the default Web browser |
| [clova://app-launch/default-camera](#AppLaunchDefaultCamera)        | Launch the default camera app   |
| [clova://app-launch/default-email](#AppLaunchDefaultEmail)          | Launch the default email app    |
| [clova://app-launch/default-gallery](#AppLaunchDefaultGallery)      | Launch the default gallery app   |
| [clova://audio-repeat](#AudioRepeat)                                | Output audio     |
| [clova://device-control](#DeviceControl)                            | Control the device       |
| [clova://guide/talking](#GuideTalking)     | Provide a guide on commands                              |
| [clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}Search](#NaverSearch)        | Search a specific keyword on the {{ book.ServiceEnv.OrientedService }} app                    |
| [clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}-maps](#NaverMaps)           | Launch the {{ book.ServiceEnv.OrientedService }} map app                            |
| [clova://ttsRepeat](#TTSRepeat)            | Start TTS                     |
| [clova://webview](#WebView)                | Open a webpage in a WebView                          |

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The difference of an action URI scheme from a directive message is that for a directive message, the client must perform the corresponding action immediately upon receiving a directive message. But for an action URI scheme, the client must perform the corresponding action when a user interaction has taken place on UI that is provided on a screen or as other means. When providing an action for an action URI scheme, the client must provide the action that matches the purpose as defined in the table above and must not perform a random action.</p>
</div>

### clova://app-launch/default-addressbook {#AppLaunchDefaultAddrBook}

The client must run the default address book app in response to this scheme. The following is an example of the action URI scheme.

```
clova://app-launch/default-addressbook
```

### clova://app-launch/default-browser {#AppLaunchDefaultBrowser}

The client must run the default web browser in response to this scheme. The following is an example of the action URI scheme.

```
clova://app-launch/default-browser
```

### clova://app-launch/default-camera {#AppLaunchDefaultCamera}

The client must run the default camera app in response to this scheme. The following is an example of the action URI scheme.

```
clova://app-launch/default-camera
```

### clova://app-launch/default-email {#AppLaunchDefaultEmail}

The client must run the default email app in response to this scheme. The following is an example of the action URI scheme.

```
clova://app-launch/default-email
```

### clova://app-launch/default-gallery {#AppLaunchDefaultGallery}

The client must run the default gallery app in response to this scheme. The following is an example of the action URI scheme.

```
clova://app-launch/default-gallery
```

### clova://audio-repeat {#AudioRepeat}

The client must play the default audio file in response to this scheme.

| Parameter name    | Description                         | Included |
|---------------|-----------------------------|:--------:|
| url           | Audio file URI                | Always |

The following is an example of the action URI scheme.

```
clova://audio-repeat?url=https://target.audioFile.url
```

### clova://device-control {#DeviceControl}

The client must control a specific function in response to this scheme.

| Parameter name    | Description                         | Included |
|---------------|-----------------------------|:--------:|
| command       | Control command<ul><li>BtConnect</li><li>BtDisconnect</li><li>BtStartPairing</li><li>BtStopPairing</li><li>Decrease</li><li>Increase</li><li>LaunchApp</li><li>OpenScreen</li><li>SetValue</li><li>TurnOn</li><li>TurnOff</li></ul>                      | Always |
| target        | The target to be controlled.<ul><li><code>"airplane"</code>: Airplane mode</li><li><code>"app"</code>: App</li><li><code>"bluetooth"</code>: Bluetooth</li><li><code>"cellular"</code>: Cellular network</li><li><code>"channel"</code>: TV channel</li><li><code>"flashlight"</code>: Flashlight</li><li><code>"gps"</code>: GPS</li><li><code>"powersave"</code>: Power saving mode</li><li><code>"screenbrightness"</code>: Screen brightness</li><li><code>"soundmode"</code>: Sound mode</li><li><code>"volume"</code>: Speaker volume</li><li><code>"wifi"</code>: Wi-Fi</li></ul> | Always |
| value         | Setting value. This value is being designated when the `command` parameter is a `setValue`. It is also applied when setting speaker volume (`"volume"`) or screen brightness (`"screenbrightness"`) or controlling TV channel (`"channel"`). | Conditional |

The following is an example of the action URI scheme.

```
clova://device-control?command=SetValue&target=volume&value=5
clova://device-control?command=TurnOn&target=flashlight
```

### clova://guide/talking {#GuideTalking}

The client must provide a command helper in response to this scheme. The following is an example of the action URI scheme.


```
clova://guide/talking
```

### clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}Search {#NaverSearch}

The client must run the {{ book.ServiceEnv.OrientedService }} app and perform search in response to this scheme.

| Parameter name    | Description                         | Included |
|---------------|-----------------------------|:---------:|
| url           | The URI page to open in the {{ book.ServiceEnv.OrientedService }} app. | Always |

The following is an example of the action URI scheme.

```
clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}Search?url=http://target.page.url
```

### clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}-maps {#NaverMaps}

The client must run the {{ book.ServiceEnv.OrientedService }} map app and find routes in response to this scheme.

| Parameter name    | Description                         | Included |
|---------------|-----------------------------|:---------:|
| url           | The URI to open with the {{ book.ServiceEnv.OrientedService }} map app.   | Always |

The following is an example of the action URI scheme.

```
clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}-maps?url=http://target.map.url
```

### clova://ttsRepeat {#TTSRepeat}

The client must output specific texts as voice in response to this scheme.

| Parameter name    | Description                         | Included |
|---------------|-----------------------------|:---------:|
| lang          | The language of the text to be read out<ul><li><code>"en"</code>: English</li><li><code>"ja"</code>: Japanese</li><li><code>"ko"</code>: Korean</li></ul> | Always |
| text          | The text to be read out.                   | Conditional |

The following is an example of the action URI scheme.

```
clova://ttsRepeat?lang=en&text=hello
```

### clova://webview {#WebView}
The client must open a specific page in WebView in response to this scheme.

| Parameter name    | Description                         | Included |
|---------------|-----------------------------|:---------:|
| url           | The URI of the page to open.              | Always |
| auth_required | Indicates whether authentication is required or not: `true`: Use the auth API to open the page. `false`: Authentication is not required. | Conditional |

The following is an example of the action URI scheme.

```
clova://webview?url=https://target.page.url&auth_required=true
```
