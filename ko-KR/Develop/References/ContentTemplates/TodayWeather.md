# TodayWeather Template
오늘 날씨 정보를 제공하는 템플릿입니다. 화면에 오늘 날씨 정보를 표시할 때 사용됩니다.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>오늘 날씨 정보를 표시한 예는 <a href="#UIExample">UI example</a>을 참조합니다.</p>
</div>

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `bgClipUrl`                 | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 배경 영상 파일의 URI 정보가 담긴 객체. <div class="warning"><p><strong>Warning!</strong></p><p>이 필드가 제공하는 미디어 콘텐츠는 Clova 서비스 외의 용도로 사용하실 수 없습니다.</p></div> |
| `bgDefaultUrl`              | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 배경 이미지 파일의 URI 정보가 담긴 객체. `bgClipUrl` 필드의 영상을 준비하는 동안 `bgDefaultUrl` 필드의 이미지를 표시할 수 있습니다. 또한, 기술적인 문제로 `bgClipUrl` 필드의 영상을 제공할 수 없을 때 사용할 수 있습니다. <div class="warning"><p><strong>Warning!</strong></p><p>이 필드가 제공하는 미디어 콘텐츠는 Clova 서비스 외의 용도로 사용하실 수 없습니다.</p></div> |
| `concentrationOfFineDust`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 미세 먼지 수준 정보가 담긴 객체. |
| `contentProviderText`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 콘텐츠 제공자의 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `hourlyWeatherList[]` | object array | 시간대별 날씨 정보를 가지는 객체 배열 |
| `hourlyWeatherList[].hourlyTemperature` | [TemperatureCObject](/Develop/References/ContentTemplates/Shared_Objects.md#TemperatureCObject) | 시간대별 온도 정보가 담긴 객체 |
| `hourlyWeatherList[].hourlyTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 시간대 정보가 담긴 객체 |
| `hourlyWeatherList[].rainfallProbability` | [PercentageObject](/Develop/References/ContentTemplates/Shared_Objects.md#PercentageObject) | 강수 확률 정보가 담긴 객체. 이 객체의 `value` 필드는 `null` 값을 가질 수도 있습니다. |
| `hourlyWeatherList[].temperatureImageCode` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 시간대별 [날씨 코드](#WeatherCode) 정보가 담긴 객체 |
| `hourlyWeatherList[].temperatureImageUrl` | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 시간대별 날씨 이미지 파일의 URI 정보가 담긴 객체 |
| `linkUrl`                   | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 콘텐츠 링크 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.   |
| `lastUpdate`                | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 날씨 정보가 최종 업데이트된 시간 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다. |
| `location`                  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 지역 정보가 담긴 객체 |
| `nowTemperature`            | [TemperatureCObject](/Develop/References/ContentTemplates/Shared_Objects.md#TemperatureCObject) | 현재 온도 정보가 담긴 객체 |
| `nowTemperatureImageCode`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 현재 [날씨 코드](#WeatherCode) 정보가 담긴 객체   |
| `nowTemperatureImageUrl`    | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 현재 날씨 이미지 파일의 URI 정보가 담긴 객체 |
| `nowWeather`                | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 현재 날씨 정보가 담긴 객체  |
| `referenceText`             | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 참조한 서비스의 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `referenceUrl`              | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 참조한 서비스의 이용 결과 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.   |
| `type`                      | string | Content template 구분자. `"TodayWeather"` 값을 가집니다. |

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
    "value": "좋음 29㎍/㎥"
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
    "value": "정자1동"
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
    "value": "맑음"
  },
  "contentProviderText" : {
    "type" : "string",
    "value" : "기상청"
  },
  "lastUpdate" : {
    "type" : "datetime",
    "value" : "2018-02-05T06:29:09Z"
  },
  "referenceText" : {
    "type" : "string",
    "value" : "날씨"
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
다음은 landscape 화면 형태에서 TodayWeather 템플릿의 내용을 표현한 UI 예제입니다.

* 예제 1

  ![Content_Template-TodayWeather-Example1](/Develop/Assets/Images/Content_Template-TodayWeather-Example1.png)

* 예제 2

  ![Content_Template-TodayWeather-Example2](/Develop/Assets/Images/Content_Template-TodayWeather-Example2.png)

* 예제 3

  ![Content_Template-TodayWeather-Example3](/Develop/Assets/Images/Content_Template-TodayWeather-Example3.png)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>화면의 어떤 부분에 어떤 필드의 데이터가 표시되어야 나타내는 이미지를 곧 업데이트할 예정입니다.</p>
</div>

## See also
* [Atmosphere](/Develop/References/ContentTemplates/Atmosphere.md)
* [TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)
* [WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)
* [Humidity](/Develop/References/ContentTemplates/Humidity.md)
* [WindSpeed](/Develop/References/ContentTemplates/WindSpeed.md)
