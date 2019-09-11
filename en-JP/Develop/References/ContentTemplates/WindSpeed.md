# WindSpeed Template
The WindSpeed template is used in providing wind speed information for the client to display on the client screen.

<div class="note">
<p><strong>Note!</strong></p>
<p>See a <a href="#UIExample">UI example</a> on how the WindSpeed template is used in display.</p>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `bgClipUrl`     | [URLObject](/Develop/References/ContentTemplates/Shared_Objects.md#URLObject) | The URL of the video file to play in the background. <div class="danger"><p><strong>Caution!</strong></p><p>Due to a license issue, you are not permitted to use this URL.</p></div> |
| `contentProviderText`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information of the content provider. The `value` field of this object can have an empty string (`""`).  |
| `lastUpdate`                | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | The last update time of the weather information. The `value` field of this object can have an empty string (`""`). |
| `linkUrl`       | [URLObject](/Develop/References/ContentTemplates/Shared_Objects.md#URLObject) | The URL of the content. The `value` field of this object can have an empty string (`""`).   |
| `location`      | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information on the region. The `value` field of this object can have an empty string (`""`).   |
| `referenceText`             | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information of the referred service. The `value` field of this object can have an empty string (`""`).  |
| `referenceUrl`              | [URLObject](/Develop/References/ContentTemplates/Shared_Objects.md#URLObject)       | The information on the usage result URL of the referred service. The `value` field of this object can have an empty string (`""`).   |
| `temperatureCode`      | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information on the [weather code](#WeatherCode). The `value` field of this object can have an empty string (`""`).  |
| `type`          | string | The type of this template. The value is always "WindSpeed". |
| `windDirection` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information on wind direction. |
| `windSpeed`     | [NumberObject](/Develop/References/ContentTemplates/Shared_Objects.md#NumberObject) | The information on wind speed. |

{% include "/Develop/References/ContentTemplates/Shared_Weather_Code.md" %}

## Template example

{% raw %}
```json
{
  "bgImageUrl": {
    "type": "url",
    "value": "https://ssl.pstatic.example.net/static/clova/service/weather/bg_cloud_night.mp4"
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
  "type": "WindSpeed",
  "windDirection": {
    "type": "string",
    "value": "W"
  },
  "windSpeed": {
    "type": "number",
    "value": "1m/s"
  }
}
```
{% endraw %}

## UI example {#UIExample}
The following example shows how the WindSpeed template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

![WindSpeed](/Develop/Assets/Images/Content-Template-WindSpeed.png)

## See also
* [Atmosphere](/Develop/References/ContentTemplates/Atmosphere.md)
* [TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)
* [TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)
* [WeeklyWeather](/Develop/References/ContentTemplates/Humidity.md)
* [Humidity](/Develop/References/ContentTemplates/Humidity.md)
