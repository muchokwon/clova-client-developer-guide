# TomorrowWeather Template
The TomorrowWeather template is used in providing tomorrow's weather forecast for the client to display on the client screen.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>See a <a href="#UIExample">UI example</a> on how the TomorrowWeather template is used in display.</p>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `bgClipUrl`                 | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | The URI of the video file to play in the background. <div class="warning"><p><strong>Warning!</strong></p><p>Due to a license issue, you are not permitted to use this URL.</p></div> |
| `contentProviderText`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information of the content provider. The `value` field of this object can have an empty string (`""`).  |
| `highTemperature`           | [TemperatureCObject](/Develop/References/ContentTemplates/Shared_Objects.md#TemperatureCObject) | The highest temperature for tomorrow. |
| `highTempWeather`           | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information on weather at the highest temperature.  |
| `hourlyWeatherList[]` | object array | The object array of hourly weather information. |
| `hourlyWeatherList[].hourlyTemperature` | [TemperatureCObject](/Develop/References/ContentTemplates/Shared_Objects.md#TemperatureCObject) | The hourly temperature. |
| `hourlyWeatherList[].hourlyTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | The time of the weather forecast. |
| `hourlyWeatherList[].rainfallProbability` | [PercentageObject](/Develop/References/ContentTemplates/Shared_Objects.md#PercentageObject) | The chance of rain. The `value` field of this object array element may have a (`null`) value.      |
| `hourlyWeatherList[].temperatureImageCode` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The [weather code](#WeatherCode) for the forecast. |
| `hourlyWeatherList[].temperatureImageUrl` | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | The URI of the icon to represent the forecast. |
| `lastUpdate`                | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | The last update time of the weather information. The `value` field of this object can have an empty string (`""`). |
| `linkUrl`                   | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | The URI of the content. The `value` field of this object can have an empty string (`""`).      |
| `location`                  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information on the region. |
| `lowTemperature`           | [TemperatureCObject](/Develop/References/ContentTemplates/Shared_Objects.md#TemperatureCObject) | The lowest temperature for tomorrow. |
| `lowTempWeather`           | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information on weather at the lowest temperature.  |
| `referenceText`             | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information of the referred service. The `value` field of this object can have an empty string (`""`).  |
| `referenceUrl`              | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | The information on the usage result URI of the referred service. The `value` field of this object can have an empty string (`""`).   |
| `type`                      | string | The type of this template. The value is always `"TomorrowWeather"`. |

{% include "/Develop/References/ContentTemplates/Shared_Weather_Code.md" %}

## Template example

{% raw %}
```json
{
  "bgClipUrl": {
    "type": "url",
    "value": "https://example.net/clova/weather/bg_cloud_daytime.mp4"
  },
  "highTempWeather": {
    "type": "string",
    "value": "Mostly cloudy"
  },
  "highTemperature": {
    "type": "temperature-c",
    "value": "32"
  },
  "hourlyWeatherList": [
    {
      "hourlyTemperature": {
        "type": "temperature-c",
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
        "type": "temperature-c",
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
        "type": "temperature-c",
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
        "type": "temperature-c",
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
        "type": "temperature-c",
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
        "type": "temperature-c",
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
        "type": "temperature-c",
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
        "type": "temperature-c",
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
    },
    {
      "hourlyTemperature": {
        "type": "temperature-c",
        "value": "26"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170728 00:00"
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
        "type": "temperature-c",
        "value": "24"
      },
      "hourlyTime": {
        "type": "datetime",
        "value": "20170728 03:00"
      },
      "rainfallProbability": {
        "type": "percentage",
        "value": "80%"
      },
      "temperatureImageCode": {
        "type": "string",
        "value": "9"
      },
      "temperatureImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_09.png"
      }
    }
  ],
  "location": {
    "type": "string",
    "value": "Jeongja1-dong"
  },
  "lowTempWeather": {
    "type": "string",
    "value": "Mostly cloudy"
  },
  "lowTemperature": {
    "type": "temperature-c",
    "value": "23"
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
  "type": "TomorrowWeather"
}
```
{% endraw %}

## UI example {#UIExample}
The following example shows how the TomorrowWeather template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

![TomorrowWeather](/Develop/Assets/Images/Content-Template-TomorrowWeather.png)

## See also
* [Atmosphere](/Develop/References/ContentTemplates/Atmosphere.md)
* [TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)
* [WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)
* [Humidity](/Develop/References/ContentTemplates/Humidity.md)
* [WindSpeed](/Develop/References/ContentTemplates/WindSpeed.md)
