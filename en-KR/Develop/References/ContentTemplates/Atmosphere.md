# Atmosphere Template
The Atmosphere template is used in providing atmosphere information for the client to display on the client screen. The type of atmosphere information provided includes information on fine dust, ultrafine dust, ozone, UV rays, and yellow dust.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>See a <a href="#UIExample">UI example</a> for the Atmosphere template used in the display.</p>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `announcementOfAtmosphere`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The statement on the forecast. This field is omitted when showing the current atmosphere information. When omitted, the `value` field of this object has an empty string (`""`). |
| `bgClipUrl`     | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | The URI of the video file to play in the background. <div class="warning"><p><strong>Warning!</strong></p><p>Due to a license issue, you are not permitted to use this URL.</p></div> |
| `concentrationOfAtmosphere` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The current level of air quality. This field is omitted when forecasting. When omitted, the `value` field of this object has an empty string (`""`). |
| `contentProviderText`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information of the content provider. The `value` field of this object can have an empty string (`""`).  |
| `halfDayAtmosphereList[]`             | object array | The object array that has multiple atmosphere information in half day (morning/afternoon) units.                                    |
| `halfDayAtmosphereList[].atmosphereImageUrl` | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | The URI to the image associated with the atmosphere information specified by the `halfDayAtmosphereList[].durationHalfDay` field. |
| `halfDayAtmosphereList[].concentrationOfAtmosphere`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The level of air quality for the time specified by the `halfDayAtmosphereList[].durationHalfDay` field.  |
| `halfDayAtmosphereList[].durationHalfDay`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The time scope for the associated atmosphere information. The `value` field of this object holds values such as `Tomorrow morning`, `Tomorrow afternoon`, `Morning in two days from now`, `Afternoon in two days from now`.  |
| `lastUpdate`                | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | The last update time of the weather information. The `value` field of this object can have an empty string (`""`). |
| `linkUrl`       | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | The URI of the content. The `value` field of this object can have an empty string (`""`).  |
| `location`      | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information on the region. |
| `referenceText`             | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information of the referred service. The `value` field of this object can have an empty string (`""`).  |
| `referenceUrl`              | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | The information on the usage result URI of the referred service. The `value` field of this object can have an empty string (`""`).   |
| `temperatureCode`      | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information on the [weather code](#WeatherCode). The `value` field of this object can have an empty string (`""`).  |
| `type`          | string | The type of this template. The value is always `"Atmosphere"`. |
| `valueOfAtmosphere`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The current air quality index. The unit of the index is included. The `value` field of this object can have an empty string (`""`). |

{% include "/Develop/References/ContentTemplates/Shared_Weather_Code.md" %}

## Template example

{% raw %}
```json
// Current atmosphere information
{
  "announcementOfAtmosphere": {
    "type": "string",
    "value": ""
  },
  "bgClipUrl": {
    "type": "url",
    "value": "https://example.net/clova/weather/bg_clean_daytime.mp4"
  },
  "concentrationOfAtmosphere": {
    "type": "string",
    "value": "Good"
  },
  "contentProviderText" : {
    "type" : "string",
    "value": "National weather service"
  },
  "failureMessage": {
    "type": "string",
    "value": "The fine dust level for today in Shinjuku is good"
  },
  "halfDayAtmosphereList": [
    {
      "atmosphereImageUrl": {
        "type": "url",
        "value": "https://static.contentservice.example.net/clova/service/weather/air_icon_02.png"
      },
      "concentrationOfAtmosphere": {
        "type": "string",
        "value": "Normal"
      },
      "durationHalfDay": {
        "type": "string",
        "value": "Tomorrow morning"
      }
    },
    {
      "atmosphereImageUrl": {
        "type": "url",
        "value": "https://static.contentservice.example.net/clova/service/weather/air_icon_02.png"
      },
      "concentrationOfAtmosphere": {
        "type": "string",
        "value": "Normal"
      },
      "durationHalfDay": {
        "type": "string",
        "value": "Tomorrow afternoon"
      }
    },
    {
      "atmosphereImageUrl": {
        "type": "url",
        "value": "https://static.contentservice.example.net/clova/service/weather/air_icon_02.png"
      },
      "concentrationOfAtmosphere": {
        "type": "string",
        "value": "Normal"
      },
      "durationHalfDay": {
        "type": "string",
        "value": "Morning in two days from now"
      }
    },
    {
      "atmosphereImageUrl": {
        "type": "url",
        "value": "https://static.contentservice.example.net/clova/service/weather/air_icon_02.png"
      },
      "concentrationOfAtmosphere": {
        "type": "string",
        "value": "Normal"
      },
      "durationHalfDay": {
        "type": "string",
        "value": "Afternoon in two days from now"
      }
    }
  ],
  "location": {
    "type": "string",
    "value": "Jeongja1-dong"
  },
  "meta": {
    "version": {
      "type": "string",
      "value": "v0.1"
    }
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
  "temperatureCode": {
    "type": "string",
    "value": "5"
  },
  "type": "Atmosphere",
  "valueOfAtmosphere": {
    "type": "number",
    "value": "27㎍/㎥"
  }
}

// Atmosphere information for tomorrow
{
  "announcementOfAtmosphere": {
    "type": "string",
    "value": "Here is tomorrow's fine dust level"
  },
  "bgClipUrl": {
    "type": "url",
    "value": "https://example.net/clova/weather/bg_cloud_daytime.mp4"
  },
  "concentrationOfAtmosphere": {
    "type": "string",
    "value": ""
  },
  "failureMessage": {
    "type": "string",
    "value": "The fine dust level for tomorrow in Shinjuku is average"
  },
  "halfDayAtmosphereList": [
    {
      "atmosphereImageUrl": {
        "type": "url",
        "value": "https://static.contentservice.example.net/clova/service/weather/air_icon_02.png"
      },
      "concentrationOfAtmosphere": {
        "type": "string",
        "value": "Normal"
      },
      "durationHalfDay": {
        "type": "string",
        "value": "Tomorrow morning"
      }
    },
    {
      "atmosphereImageUrl": {
        "type": "url",
        "value": "https://static.contentservice.example.net/clova/service/weather/air_icon_02.png"
      },
      "concentrationOfAtmosphere": {
        "type": "string",
        "value": "Normal"
      },
      "durationHalfDay": {
        "type": "string",
        "value": "Tomorrow afternoon"
      }
    },
    {
      "atmosphereImageUrl": {
        "type": "url",
        "value": "https://static.contentservice.example.net/clova/service/weather/air_icon_02.png"
      },
      "concentrationOfAtmosphere": {
        "type": "string",
        "value": "Normal"
      },
      "durationHalfDay": {
        "type": "string",
        "value": "Morning in two days from now"
      }
    },
    {
      "atmosphereImageUrl": {
        "type": "url",
        "value": "https://static.contentservice.example.net/clova/service/weather/air_icon_02.png"
      },
      "concentrationOfAtmosphere": {
        "type": "string",
        "value": "Normal"
      },
      "durationHalfDay": {
        "type": "string",
        "value": "Afternoon in two days from now"
      }
    }
  ],
  "location": {
    "type": "string",
    "value": "Jeongja1-dong"
  },
  "meta": {
    "version": {
      "type": "string",
      "value": "v0.1"
    }
  },
  "type": "Atmosphere",
  "valueOfAtmosphere": {
    "type": "number",
    "value": ""
  }
}
```
{% endraw %}

## UI example {#UIExample}
The following examples show how the Atmosphere template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

| Atmosphere state now | Atmosphere state tomorrow |
|-------------|------------|
| ![Now](/Develop/Assets/Images/Content-Template-Atmosphere_Now.png) | ![Original](/Develop/Assets/Images/Content-Template-Atmosphere_Tomorrow.png) |

## See also
* [Humidity](/Develop/References/ContentTemplates/Humidity.md)
* [TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)
* [TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)
* [WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)
* [WindSpeed](/Develop/References/ContentTemplates/WindSpeed.md)
