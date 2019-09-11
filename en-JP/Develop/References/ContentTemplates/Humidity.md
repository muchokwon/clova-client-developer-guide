# Humidity Template
The Humidity template is used in providing humidity information for the client to display on the client screen.

<div class="note">
<p><strong>Note!</strong></p>
<p>See a <a href="#UIExample">UI example</a> of the Humidity template used in display.</p>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `bgClipUrl`     | [URLObject](/Develop/References/ContentTemplates/Shared_Objects.md#URLObject) | The URL of the video file to play in the background. <div class="danger"><p><strong>Caution!</strong></p><p>Due to a license issue, you are not permitted to use this URL.</p></div> |
| `contentProviderText`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information of the content provider. The `value` field of this object can have an empty string (`""`).  |
| `humidity`      | [PercentageObject](/Develop/References/ContentTemplates/Shared_Objects.md#PercentageObject) | The information on humidity. |
| `lastUpdate`                | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | The last update time of the weather information. The `value` field of this object can have an empty string (`""`). |
| `linkUrl`       | [URLObject](/Develop/References/ContentTemplates/Shared_Objects.md#URLObject) | The URL of the content. The `value` field of this object can have an empty string (`""`).  |
| `location`      | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information on the region. |
| `referenceText`             | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information of the referred service. The `value` field of this object can have an empty string (`""`).  |
| `referenceUrl`              | [URLObject](/Develop/References/ContentTemplates/Shared_Objects.md#URLObject)       | The information on the usage result URL of the referred service. The `value` field of this object can have an empty string (`""`).   |
| `temperatureCode`      | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information on the [weather code](#WeatherCode). The `value` field of this object can have an empty string (`""`).  |
| `type`          | string | The type of this template. The value is always `"Humidity"`. |

{% include "/Develop/References/ContentTemplates/Shared_Weather_Code.md" %}

## Template example

{% raw %}
```json
{
  "bgImageUrl": {
    "type": "url",
    "value": "https://ssl.pstatic.example.net/static/clova/service/weather/bg_cloud_night.mp4"
  },
  "humidity": {
    "type": "percentage",
    "value": "60%"
  },
  "location": {
    "type": "string",
    "value": "Jeongja1-dong"
  },
  "contentProviderText" : {
    "type" : "string",
    "value": "National weather service"
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
    "value": "Weather"
  },
  "referenceUrl" : {
    "type" : "url",
    "value" : "http://weather.contentservice.example.com/"
  },
  "type": "Humidity"
}
```
{% endraw %}

## UI example {#UIExample}
The following example shows how the Humidity template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

![Humidity](/Develop/Assets/Images/Content-Template-Humidity.png)

## See also
* [Atmosphere](/Develop/References/ContentTemplates/Atmosphere.md)
* [TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)
* [TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)
* [WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)
* [WindSpeed](/Develop/References/ContentTemplates/WindSpeed.md)
