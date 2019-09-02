# WeeklyWeatherテンプレート
1週間の天気情報を提供するテンプレートです。画面に1週間の天気情報を表示するときに使用されます。

<div class="tip">
<p><strong>ヒント</strong></p>
<p>1週間の天気を表示するサンプルは、<a href="#UIExample">UI example</a>を参照してください。</p>
</div>

## Template fields

| フィールド名       | データ型    | 説明                     |
|---------------|---------|-----------------------------|
| `bgClipUrl`                       | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 背景動画ファイルのURIを持つオブジェクト。<div class="warning"><p><strong>警告</strong></p><p>このフィールドのデータは、ライセンスの問題により、提携会社では使用できません。</p></div> |
| `contentProviderText`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | コンテンツ提供元の情報を持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。  |
| `dailyWeatherList[]`              | object array | 1日の天気情報を持つオブジェクト配列 |
| `dailyWeatherList[].date`         | [DateObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateObject) | 日付を持つオブジェクト |
| `dailyWeatherList[].highTemperature` | [TemperatureCObject](/Develop/References/ContentTemplates/Shared_Objects.md#TemperatureCObject) | 該当する日付の最高気温を持つオブジェクト |
| `dailyWeatherList[].iconImageCode` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 日付ごとの[天気コード](#WeatherCode)を持つオブジェクト |
| `dailyWeatherList[].iconImageUrl` | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 該当する日付の天気情報を表現する画像アイコンファイルのURIを持つオブジェクト |
| `dailyWeatherList[].lowTemperature`  | [TemperatureCObject](/Develop/References/ContentTemplates/Shared_Objects.md#TemperatureCObject) | 該当する日付の最低気温を持つオブジェクト |
| `description`               | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 1週間の天気情報であることを示すテキストを持つオブジェクト  |
| `lastUpdate`                | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 天気情報の最終更新時間を持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。 |
| `linkUrl`                   | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | コンテンツのリンク先のURIを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。   |
| `location`                  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 地域の情報を持つオブジェクト |
| `referenceText`             | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 参照したサービスの情報を持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。  |
| `referenceUrl`              | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 参照したサービスの利用結果ページのURIを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。   |
| `type`                      | string | コンテンツテンプレートのタイプを示す値。`"WeeklyWeather"`を持ちます。 |

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
    "value": "1週間の天気です。"
  },
  "location": {
    "type": "string",
    "value": "亭子1洞"
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
  "type": "WeeklyWeather"
}
```
{% endraw %}

## UI example {#UIExample}
以下は、{{ book.ServiceEnv.OrientedService }}が配布したClovaのモバイルアプリで、WeeklyWeatherテンプレートの内容を表したUIサンプルです。

![WeeklyWeather](/Develop/Assets/Images/Content-Template-WeeklyWeather.png)

## 次の項目も参照してください。
* [Atmosphere](/Develop/References/ContentTemplates/Atmosphere.md)
* [TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)
* [TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)
* [Humidity](/Develop/References/ContentTemplates/Humidity.md)
* [WindSpeed](/Develop/References/ContentTemplates/WindSpeed.md)
