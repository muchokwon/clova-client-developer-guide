# Humidityテンプレート
湿度の情報を提供するテンプレートです。画面に湿度の情報を表示するときに使用されます。

<div class="tip">
<p><strong>ヒント</strong></p>
<p>湿度の情報を表示するサンプルは、<a href="#UIExample">UI example</a>を参照してください。</p>
</div>

## Template fields

| フィールド名       | データ型    | 説明                     |
|---------------|---------|-----------------------------|
| `bgClipUrl`     | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 背景動画ファイルのURIを持つオブジェクト。<div class="warning"><p><strong>警告</strong></p><p>このフィールドのデータは、ライセンスの問題により、提携会社では使用できません。</p></div> |
| `contentProviderText`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | コンテンツ提供元の情報を持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。  |
| `humidity`      | [PercentageObject](/Develop/References/ContentTemplates/Shared_Objects.md#PercentageObject) | 湿度を持つオブジェクト。 |
| `lastUpdate`                | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 天気情報の最終更新時間を持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。 |
| `linkUrl`       | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | コンテンツのリンク先のURIを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。  |
| `location`      | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 地域の情報を持つオブジェクト |
| `referenceText`             | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 参照したサービスの情報を持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。  |
| `referenceUrl`              | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 参照したサービスの利用結果ページのURIを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。   |
| `temperatureCode`      | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | [天気コード](#WeatherCode)を持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。  |
| `type`          | string | コンテンツテンプレートのタイプを示す値。`"Humidity"`を持ちます。 |

{% include "/Develop/References/ContentTemplates/Shared_Weather_Code.md" %}

## Template example

{% raw %}
```json
{
  "bgImageUrl": {
    "type": "url",
    "value": "https://example.net/clova/weather/bg_cloud_night.mp4"
  },
  "humidity": {
    "type": "percentage",
    "value": "60%"
  },
  "location": {
    "type": "string",
    "value": "亭子1洞"
  },
  "contentProviderText" : {
    "type" : "string",
    "value" : "気象庁"
  },
  "temperatureCode": {
    "type": "string",
    "value": "5"
  },
  "lastUpdate" : {
    "type" : "datetime",
    "value" : "2018-02-05T06:29:09Z"
  },
  "referenceText" : {
    "type" : "string",
    "value" : "天気"
  },
  "referenceUrl" : {
    "type" : "url",
    "value" : "https://weather.contentservice.example.com/"
  },
  "type": "Humidity"
}
```
{% endraw %}

## UI example {#UIExample}
以下は、{{ book.ServiceEnv.OrientedService }}が配布したClovaのモバイルアプリで、Humidityテンプレートの内容を表したUIサンプルです。

![Humidity](/Develop/Assets/Images/Content-Template-Humidity.png)

## 次の項目も参照してください。
* [Atmosphere](/Develop/References/ContentTemplates/Atmosphere.md)
* [TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)
* [TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)
* [WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)
* [WindSpeed](/Develop/References/ContentTemplates/WindSpeed.md)
