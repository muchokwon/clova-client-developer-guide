# Popup Template
The Popup template is used in providing information on texts or buttons to display as toast, alert, or popup. The valid field may vary depending on the popup type.

| Popup Type       | Description                      | Valid fields                         |
|---------------|-----------------------------|-----------------------------|
| Toast         | A toast consisting of links and text.    | `toastLinkText`, `toastLinkUrl`, `toastText`                  |
| Alert         | An alert consisting of text and a confirmation button.   | `alertText`                                                   |
| Popup (Single button) | A popup consisting of a title, text, and a button (link). | `mainText`, `positiveButtonText`, `positiveButtonUrl`, `title`   |
| Popup (Two buttons) | A popup consisting of a title, text, and two buttons. | `negativeButtonText`, `negativeButtonUrl`, `mainText`, `positiveButtonText`, `positiveButtonUrl`, `title` |

<div class="note">
<p><strong>Note!</strong></p>
<p>See <a href="#UIExample">UI examples</a> of the Popup template used in display.</p>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `alertText`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The warning message for an alert. The `value` field of this object can have an empty string (`""`). |
| `displayType`      | string                                                                          | The type of information to display. Available values are:<ul><li><code>"POPUP"</code></li><li><code>"ALERT"</code></li><li><code>"TOAST"</code></li></ul>  |
| `negativeButtonText`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | For popups, the button label that carries a negative meaning such as **No**. The `value` field of this object can have an empty string (`""`). |
| `negativeButtonUrl`    | [URLObject](/Develop/References/ContentTemplates/Shared_Objects.md#URLObject)       | For popups, the URL information to connect to the button label that carries a negative meaning such as **No**. The `value` field of this object can have an empty string (`""`). |
| `mainText`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The text to display on the popup. The `value` field of this object can have an empty string (`""`). |
| `positiveButtonText`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | For popups, the button label that carries a positive meaning, such as **Yes**. For popups with a single button, this object is used as text to display on the button with a meaning such as **Confirmation**. The `value` field of this object can have an empty string (`""`). |
| `positiveButtonUrl`   | [URLObject](/Develop/References/ContentTemplates/Shared_Objects.md#URLObject)       | For popups, the URL information to connect to the button label that carries a positive meaning such as **Yes**. For popups with a single button, this object is used as the URL to connect to the button with a meaning such as **Confirmation**. The `value` field of this object can have an empty string (`""`). |
| `title`            | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The title of this popup. The `value` field of this object can have an empty string (`""`). |
| `toastLinkText`    | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The link text to display on the toast. The `value` field of this object can have an empty string (`""`). |
| `toastLinkUrl`     | [URLObject](/Develop/References/ContentTemplates/Shared_Objects.md#URLObject)       | The URL of the link to display on the toast. The `value` field of this object can have an empty string (`""`). |
| `toastText`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The text to display on the toast. The `value` field of this object can have an empty string (`""`). |
| `type`             | string                                                                          | The type of this template. The value is always `"Popup"`.     |

## Template example

{% raw %}
```json
// Example 1. Toast type
{
  "type": "Popup",
  "displayType": "TOAST",
  "toastText": {
    "type": "string",
    "value": "You are currently listening to the one minute preview. Teach Clova about your taste in music and get a coupon for 100 songs on Music!"
  },
  "toastLinkText": {
    "type": "string",
    "value": "Learn more >"
  },
  "toastLinkUrl": {
    "type": "url",
    "value": "https://..."
  },
  "alertText": {
    "type": "string",
    "value": ""
  },
  "title": {
    "type": "string",
    "value": ""
  },
  "mainText": {
    "type": "string",
    "value": ""
  },
  "negativeButtonText": {
    "type": "string",
    "value": ""
  },
  "negativeButtonUrl": {
    "type": "url",
    "value": ""
  },
  "positiveButtonText": {
    "type": "string",
    "value": ""
  },
  "positiveButtonUrl": {
    "type": "url",
    "value": ""
  }
}

// Example 2. Alert type
{
  "type": "Popup",
  "displayType": "ALERT",
  "toastText": {
    "type": "string",
    "value": ""
  },
  "toastLinkText": {
    "type": "string",
    "value": ""
  },
  "toastLinkUrl": {
    "type": "url",
    "value": ""
  },
  "alertText": {
    "type": "string",
    "value": "Pausing music as it looks like you are listening on another device."
  },
  "title": {
    "type": "string",
    "value": ""
  },
  "mainText": {
    "type": "string",
    "value": ""
  },
  "negativeButtonText": {
    "type": "string",
    "value": ""
  },
  "negativeButtonUrl": {
    "type": "url",
    "value": ""
  },
  "positiveButtonText": {
    "type": "string",
    "value": ""
  },
  "positiveButtonUrl": {
    "type": "url",
    "value": ""
  }
}

// Example 3. Popup type with a single button
{
  "type": "Popup",
  "displayType": "POPUP",
  "toastText": {
    "type": "string",
    "value": ""
  },
  "toastLinkText": {
    "type": "string",
    "value": ""
  },
  "toastLinkUrl": {
    "type": "url",
    "value": ""
  },
  "alertText": {
    "type": "string",
    "value": ""
  },
  "title": {
    "type": "string",
    "value": "Clova now knows your music taste!"
  },
  "mainText": {
    "type": "string",
    "value": "Enjoy Clova's recommendations with a free Music coupon for 100 songs!"
  },
  "negativeButtonText": {
    "type": "string",
    "value": ""
  },
  "negativeButtonUrl": {
    "type": "url",
    "value": ""
  },
  "positiveButtonText": {
    "type": "string",
    "value": "Get now"
  },
  "positiveButtonUrl": {
    "type": "url",
    "value": "https://..."
  }
}

// Example 4. Popup type with two buttons
{
  "type": "Popup",
  "displayType": "POPUP",
  "toastText": {
    "type": "string",
    "value": ""
  },
  "toastLinkText": {
    "type": "string",
    "value": ""
  },
  "toastLinkUrl": {
    "type": "url",
    "value": ""
  },
  "alertText": {
    "type": "string",
    "value": ""
  },
  "title": {
    "type": "string",
    "value": "Clova now knows your music taste!"
  },
  "mainText": {
    "type": "string",
    "value": "I can give you better recommendations now that I know your taste."
  },
  "negativeButtonText": {
    "type": "string",
    "value": "Continue"
  },
  "negativeButtonUrl": {
    "type": "url",
    "value": "https://..."
  },
  "positiveButtonText": {
    "type": "string",
    "value": "End"
  },
  "positiveButtonUrl": {
    "type": "url",
    "value": "https://..."
  }
}
```
{% endraw %}

## UI example {#UIExample}
The following examples show how the Popup template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

| Toast type | Alert type |
|-----------|-----------|
| ![Type1](/Develop/Assets/Images/Content-Template-Toast.png) | ![Type2](/Develop/Assets/Images/Content-Template-Alert.png) |

| Popup (Single button) | Popup (Two buttons) |
|-------------------|--------------------|
| ![Type3](/Develop/Assets/Images/Content-Template-Popup_with_One_Button.png) | ![Type](/Develop/Assets/Images/Content-Template-Popup_with_Two_Buttons.png) |

## See also
* [CardList](/Develop/References/ContentTemplates/CardList.md)
* [ImageList](/Develop/References/ContentTemplates/ImageList.md)
* [ImageText](/Develop/References/ContentTemplates/ImageText.md)
* [Text](/Develop/References/ContentTemplates/Text.md)
