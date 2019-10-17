# TomorrowWeatherテンプレート
明日の天気情報を提供するテンプレートです。画面に明日の天気情報を表示するときに使用されます。

<div class="tip">
<p><strong>ヒント</strong></p>
<p>明日の天気を表示するサンプルは、<a href="#UIExample">UI example</a>を参照してください。</p>
</div>

## Template fields

| フィールド名       | データ型    | 説明                     |
|---------------|---------|-----------------------------|
| `bgClipUrl`                 | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 背景動画ファイルのURIを持つオブジェクト。<div class="warning"><p><strong>警告</strong></p><p>このフィールドのデータは、ライセンスの問題により、提携会社では使用できません。</p></div> |
| `contentProviderText`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | コンテンツ提供元の情報を持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。  |
| `highTemperature`           | [TemperatureCObject](/Develop/References/ContentTemplates/Shared_Objects.md#TemperatureCObject) | 明日の最高気温を持つオブジェクト |
| `highTempWeather`           | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 気温が一番高いときの天気情報を持つオブジェクト  |
| `hourlyWeatherList[]` | object array | 時間ごとの天気情報を持つオブジェクト配列 |
| `hourlyWeatherList[].hourlyTemperature` | [TemperatureCObject](/Develop/References/ContentTemplates/Shared_Objects.md#TemperatureCObject) | 時間ごとの気温情報を持つオブジェクト |
| `hourlyWeatherList[].hourlyTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 時間ごとの情報を持つオブジェクト |
| `hourlyWeatherList[].rainfallProbability` | [PercentageObject](/Develop/References/ContentTemplates/Shared_Objects.md#PercentageObject) | 降水確率を持つオブジェクト。このオブジェクトの`value`フィールドは、`null`値を持つ場合があります。      |
| `hourlyWeatherList[].temperatureImageCode` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 時間ごとの[天気コード](#WeatherCode)を持つオブジェクト |
| `hourlyWeatherList[].temperatureImageUrl` | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 時間ごとの天気の画像ファイルのURIを持つオブジェクト |
| `lastUpdate`                | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 天気情報の最終更新時間を持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。 |
| `linkUrl`                   | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | コンテンツのリンク先のURIを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。      |
| `location`                  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 地域の情報を持つオブジェクト |
| `lowTemperature`           | [TemperatureCObject](/Develop/References/ContentTemplates/Shared_Objects.md#TemperatureCObject) | 明日の最低気温を持つオブジェクト |
| `lowTempWeather`           | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 気温が一番低いときの天気情報を持つオブジェクト  |
| `referenceText`             | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 参照したサービスの情報を持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。  |
| `referenceUrl`              | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 参照したサービスの利用結果ページのURIを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。   |
| `tomorrowWeatherImageUrl` | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 明日の天気画像ファイルのURIを持つオブジェクト |
| `type`                      | string | コンテンツテンプレートのタイプを示す値。`"TomorrowWeather"`を持ちます。 |

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
    "value": "曇り"
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
        "value": "20170727 0:00"
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
        "value": "20170727 3:00"
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
        "value": "20170727 6:00"
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
        "value": "20170727 9:00"
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
        "value": "20170728 0:00"
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
        "value": "20170728 3:00"
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
    "value": "亭子1洞"
  },
  "lowTempWeather": {
    "type": "string",
    "value": "曇り"
  },
  "lowTemperature": {
    "type": "temperature-c",
    "value": "23"
  },
  "contentProviderText" : {
    "type" : "string",
    "value" : "気象庁"
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
  "type": "TomorrowWeather"
}
```
{% endraw %}

## UI example {#UIExample}
以下は、{{ book.ServiceEnv.OrientedService }}が配布したClovaのモバイルアプリで、TomorrowWeatherテンプレートの内容を表したUIサンプルです。

![TomorrowWeather](/Develop/Assets/Images/Content-Template-TomorrowWeather.png)

## 次の項目も参照してください。
* [Atmosphere](/Develop/References/ContentTemplates/Atmosphere.md)
* [TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)
* [WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)
* [Humidity](/Develop/References/ContentTemplates/Humidity.md)
* [WindSpeed](/Develop/References/ContentTemplates/WindSpeed.md)
