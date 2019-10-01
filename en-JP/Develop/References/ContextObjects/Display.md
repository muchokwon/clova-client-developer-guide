## Device.Display {#Display}
`Device.Display` is a format for reporting to CIC the display information of the client device. By providing the display information to Clova, the client will be supplied with visual content in the best resolution and DPI for the client. If client does not provide its display information to Clova, Clova assumes the client has full HD resolution.

### Object structure
{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "Display"
  },
  "payload": {
    "contentLayer": {
      "width": {{number}},
      "height": {{number}}
    },
    "dpi": {{number}},
    "orientation": {{string}},
    "size": {{string}}
  }
}
```

{% endraw %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `contentLayer`        | object | The resolution of the client display used for content. This field is mandatory if the `size` field is `"custom"`.  | Optional |
| `contentLayer.width`  | number | The width of the content area shown on the display. The unit is pixels (px).                                           | Required |
| `contentLayer.height` | number | The height of the content area shown on the display. The unit is pixels (px).                                           | Required |
| `dpi`         | number | The DPI of the display. This field is omissible only if the `size` field is `"none"`.                                 | Optional |
| `orientation` | string | The direction of the display. This field is omissible only if the `size` field is `"none"`.<ul><li><code>"landscape"</code>: Horizontal display</li><li><code>"portrait"</code>: Vertical display</li></ul>  | Optional |
| `size`        | string | The resolution of the display. You may enter a predefined size value or a customized value (`"custom"`) for the resolution size. You may also enter the value (`"none"`), which means that there is no display device.<ul><li><code>"none"</code>: No display in client device</li><li><code>"s100"</code>: Low resolution (160px X 107px)</li><li><code>"m100"</code>: Medium resolution (427px X 240px)</li><li><code>"l100"</code>: High resolution (640px X 360px)</li><li><code>"xl100"</code>: Ultrahigh resolution (xlarge type, 899px X 506px)</li><li><code>"custom"</code>: Resolution is not in a predefined specification. Enter the custom resolution value of the device in the `contentLayer` field.</li></ul> | Required |


### Object example
{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "Display"
  },
  "payload": {
    "orientation": "portrait",
    "dpi": 320,
    "size": "custom",
    "contentLayer": {
      "width": 640,
      "height": 280
    }
  }
}
```

{% endraw %}

### See also
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
