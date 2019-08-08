# WeeklyWeather Template
The WeeklyWeather template is used in providing weekly weather information for the client to display on the client screen.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>See a <a href="#UIExample">UI example</a> on how the WeeklyWeather template is used in display.</p>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `bgClipUrl`                       | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | The URI of the video file to play in the background. <div class="warning"><p><strong>Warning!</strong></p><p>Due to a license issue, you are not permitted to use this URL.</p></div> |
| `contentProviderText`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information of the content provider. The `value` field of this object can have an empty string (`""`).  |
| `dailyWeatherList[]`              | object array | The object array of daily forecasts. |
| `dailyWeatherList[].date`         | [DateObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateObject) | The date information. |
| `dailyWeatherList[].highTemperature` | [TemperatureCObject](/Develop/References/ContentTemplates/Shared_Objects.md#TemperatureCObject) | The highest temperature for the day. |
| `dailyWeatherList[].iconImageCode` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The [weather code](#WeatherCode) for the forecast. |
| `dailyWeatherList[].iconImageUrl` | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | The URI of the icon to represent the forecast. |
| `dailyWeatherList[].lowTemperature`  | [TemperatureCObject](/Develop/References/ContentTemplates/Shared_Objects.md#TemperatureCObject) | The lowest temperature for the day. |
| `description`               | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | A statement to describe that weekly forecasts are being displayed.  |
| `lastUpdate`                | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | The last update time of the weather information. The `value` field of this object can have an empty string (`""`). |
| `linkUrl`                   | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | The URI of the content. The `value` field of this object can have an empty string (`""`).   |
| `location`                  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information on the region. |
| `referenceText`             | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information of the referred service. The `value` field of this object can have an empty string (`""`).  |
| `referenceUrl`              | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | The information on the usage result URI of the referred service. The `value` field of this object can have an empty string (`""`).   |
| `type`                      | string | The type of this template. The value is always `"WeeklyWeather"`. |

{% include "/Develop/References/ContentTemplates/Shared_Weather_Code.md" %}

## Template example

{% raw %}
```json
{
  "bgImageUrl": {
    "type": "url",
    "value": "https://example.net/clova/weather/bg_cloud_daytime.mp4"
  },
  "dailyWeatherList": [
    {
      "date": {
        "type": "date",
        "value": "20170726"
      },
      "highTemperature": {
        "type": "temperature-c",
        "value": "33"
      },
      "iconImageCode": {
        "type": "string",
        "value": "3"
      },
      "iconImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_03.png"
      },
      "lowTemperature": {
        "type": "temperature-c",
        "value": "23"
      }
    },
    {
      "date": {
        "type": "date",
        "value": "20170727"
      },
      "highTemperature": {
        "type": "temperature-c",
        "value": "32"
      },
      "iconImageCode": {
        "type": "string",
        "value": "5"
      },
      "iconImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      },
      "lowTemperature": {
        "type": "temperature-c",
        "value": "23"
      }
    },
    {
      "date": {
        "type": "date",
        "value": "20170728"
      },
      "highTemperature": {
        "type": "temperature-c",
        "value": "31"
      },
      "iconImageCode": {
        "type": "string",
        "value": "9"
      },
      "iconImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_09.png"
      },
      "lowTemperature": {
        "type": "temperature-c",
        "value": "24"
      }
    },
    {
      "date": {
        "type": "date",
        "value": "20170729"
      },
      "highTemperature": {
        "type": "temperature-c",
        "value": "29"
      },
      "iconImageCode": {
        "type": "string",
        "value": "22"
      },
      "iconImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_22.png"
      },
      "lowTemperature": {
        "type": "temperature-c",
        "value": "24"
      }
    },
    {
      "date": {
        "type": "date",
        "value": "20170730"
      },
      "highTemperature": {
        "type": "temperature-c",
        "value": "29"
      },
      "iconImageCode": {
        "type": "string",
        "value": "5"
      },
      "iconImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      },
      "lowTemperature": {
        "type": "temperature-c",
        "value": "23"
      }
    },
    {
      "date": {
        "type": "date",
        "value": "20170731"
      },
      "highTemperature": {
        "type": "temperature-c",
        "value": "29"
      },
      "iconImageCode": {
        "type": "string",
        "value": "5"
      },
      "iconImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      },
      "lowTemperature": {
        "type": "temperature-c",
        "value": "23"
      }
    },
    {
      "date": {
        "type": "date",
        "value": "20170801"
      },
      "highTemperature": {
        "type": "temperature-c",
        "value": "30"
      },
      "iconImageCode": {
        "type": "string",
        "value": "5"
      },
      "iconImageUrl": {
        "type": "url",
        "value": "https://example.net/clova/weather/icon_05.png"
      },
      "lowTemperature": {
        "type": "temperature-c",
        "value": "22"
      }
    }
  ],
  "description": {
    "type": "string",
    "value": "Weekly weather"
  },
  "location": {
    "type": "string",
    "value": "Jeongja1-dong"
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
  "type": "WeeklyWeather"
}
```
{% endraw %}

## UI example {#UIExample}
The following example shows how the WeeklyWeather template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

![WeeklyWeather](/Develop/Assets/Images/Content-Template-WeeklyWeather.png)

## See also
* [Atmosphere](/Develop/References/ContentTemplates/Atmosphere.md)
* [TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)
* [TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)
* [Humidity](/Develop/References/ContentTemplates/Humidity.md)
* [WindSpeed](/Develop/References/ContentTemplates/WindSpeed.md)
