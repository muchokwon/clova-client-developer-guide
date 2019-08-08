# TodayWeather Template
The TodayWeather template is used in providing today's weather for the client to display on the client screen.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>See a <a href="#UIExample">UI example</a> on how the TodayWeather template is used in display.</p>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `bgClipUrl`                 | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | The URI of the video file to play in the background. <div class="warning"><p><strong>Warning!</strong></p><p>Due to a license issue, you are not permitted to use this URL.</p></div> |
| `concentrationOfFineDust`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The level of fine dust. |
| `contentProviderText`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information of the content provider. The `value` field of this object can have an empty string (`""`).  |
| `hourlyWeatherList[]` | object array | The object array of hourly weather information. |
| `hourlyWeatherList[].hourlyTemperature` | [TemperatureCObject](/Develop/References/ContentTemplates/Shared_Objects.md#TemperatureCObject) | The hourly temperature. |
| `hourlyWeatherList[].hourlyTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | The time of the weather forecast. |
| `hourlyWeatherList[].rainfallProbability` | [PercentageObject](/Develop/References/ContentTemplates/Shared_Objects.md#PercentageObject) | The chance of rain. The `value` field of this object array element may have a (`null`) value. |
| `hourlyWeatherList[].temperatureImageCode` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The [weather code](#WeatherCode) for the forecast. |
| `hourlyWeatherList[].temperatureImageUrl` | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | The URI of the icon to represent the forecast. |
| `linkUrl`                   | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | The URI of the content. The `value` field of this object can have an empty string (`""`).   |
| `lastUpdate`                | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | The last update time of the weather information. The `value` field of this object can have an empty string (`""`). |
| `location`                  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information on the region. |
| `nowTemperature`            | [TemperatureCObject](/Develop/References/ContentTemplates/Shared_Objects.md#TemperatureCObject) | The current temperature. |
| `nowTemperatureImageCode`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information on the current [weather code](#WeatherCode).   |
| `nowTemperatureImageUrl`    | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | The URI of the current weather image file. |
| `nowWeather`                | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The current weather.  |
| `referenceText`             | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information of the referred service. The `value` field of this object can have an empty string (`""`).  |
| `referenceUrl`              | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | The information on the usage result URI of the referred service. The `value` field of this object can have an empty string (`""`).   |
| `type`                      | string | The type of this template. The value is always `"TodayWeather"`. |

{% include "/Develop/References/ContentTemplates/Shared_Weather_Code.md" %}

## Template example

{% raw %}
```json
{
  "bgClipUrl": {
    "type": "url",
    "value": "https://example.net/clova/weather/bg_clean_daytime.mp4"
  },
  "concentrationOfFineDust": {
    "type": "string",
    "Moderate 29 ㎍/㎥"
  },
  "hourlyWeatherList": [
    {
      "hourlyTemperature": {
        "type": "temperature",
        "value": "30"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170726 18:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature",
        "value": "27"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170726 21:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature",
        "value": "25"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170727 00:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature",
        "value": "23"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170727 03:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature",
        "value": "23"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170727 06:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature",
        "value": "25"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170727 09:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature",
        "value": "27"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170727 12:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature",
        "value": "29"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170727 15:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature",
        "value": "28"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170727 18:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    },
    {
      "hourlyTemperature": {
        "type": "temperature",
        "value": "26"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170727 21:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "20%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "5"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      }
    }
  ],
  "location": {
    "type": "string",
    "value": "Jeongja1-dong"
  },
  "nowTemperature": {
    "type": "temperature-c",
    "value": "31"
  },
  "nowTemperatureImageCode": {
  "type": "string",
  "value": "5"
  },
  "nowTemperatureImageUrl": {
    "type": "url",
    "value": "https://example.net/clova/weather/icon_05.png"
  },
  "nowWeather": {
    "type": "string",
    "value": "Sunny"
  },
  "contentProviderText" : {
    "type" : "string",
    "value": "National weather service"
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
    "value" : "https://weather.contentservice.example.com/"
  },
  "type": "TodayWeather"
}
```
{% endraw %}

## UI example {#UIExample}
The following example shows how the TodayWeather template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

![TodayWeather](/Develop/Assets/Images/Content-Template-TodayWeather.png)

## See also
* [Atmosphere](/Develop/References/ContentTemplates/Atmosphere.md)
* [TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)
* [WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)
* [Humidity](/Develop/References/ContentTemplates/Humidity.md)
* [WindSpeed](/Develop/References/ContentTemplates/WindSpeed.md)
