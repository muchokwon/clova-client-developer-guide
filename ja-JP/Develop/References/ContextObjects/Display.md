## Device.Display {#Display}
`Device.Display`は、クライアントデバイスのディスプレイ装置の情報をCICにレポートするときに使用されるメッセージ形式です。クライアントデバイスが持っているディスプレイ装置の情報をClovaに送信し、画面比とDPIに合わせた画像のメディアコンテンツを応答として受信します。このコンテキストがCICに送信されない場合、Clovaは、クライアントがフルHD解像度のディスプレイ装置を持っているとみなします。

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

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `contentLayer`        | object | ディスプレイでコンテンツが表示される領域の解像度情報を持つオブジェクト。`size`の値が`"custom"`の場合、このフィールドは省略できません。  | 任意 |
| `contentLayer.width`  | number | ディスプレイでコンテンツが表示される領域の幅。ピクセル（px）単位です。                                           |  |
| `contentLayer.height` | number | ディスプレイでコンテンツが表示される領域の高さ。ピクセル（px）単位です。                                           |  |
| `dpi`         | number | ディスプレイ装置のDPI。`size`の値が`"none"`の場合にのみ、このフィールドを省略できます。                                 | 任意 |
| `orientation` | string | ディスプレイ装置の方向。`size`の値が`"none"`の場合にのみ、このフィールドを省略できます。<ul><li><code>"landscape"</code>：横方向</li><li><code>"portrait"</code>：縦方向</li></ul>  | 任意 |
| `size`        | string | ディスプレイ装置の解像度のサイズを示す値。あらかじめ指定された値または任意の解像度のサイズを表す値（`"custom"`）を入力できます。または、ディスプレイ装置がないことを表す値（`"none"`）を入力することもできます。<ul><li><code>"none"</code>：クライアントデバイスにディスプレイ装置がない</li><li><code>"s100"</code>：低解像度（160px X 107px）</li><li><code>"m100"</code>：中解像度（427px X 240px）</li><li><code>"l100"</code>：高解像度（640px X 360px）</li><li><code>"xl100"</code>：超高解像度（xlarge type、899px X 506px）</li><li><code>"custom"</code>：あらかじめ定義された規格ではない解像度。実際のデバイスの解像度を`contentLayer`フィールドに入力します。</li></ul> |  |


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

### 次の項目も参照してください。
* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
* [Custom Extensionのメッセージ]({{ book.DocMeta.ClovaCustomExtensionDeveloperGuideBaseURI}}/Develop/References/Custom_Extension_Message.{{ book.DocMeta.FileExtensionForExternalLink}})
