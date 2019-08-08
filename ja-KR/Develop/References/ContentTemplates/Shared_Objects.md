# 共有オブジェクト
コンテンツテンプレートは、一部繰り返して使用されるデータ型を表すために、以下のようなオブジェクト（Shared Objects）を共有しています。

| オブジェクト            | 説明                                            |
|--------------------|---------------------------------------------------|
| [ActionObject](#ActionObject)             | クライアントが実行できるアクションを持つオブジェクト   |
| [CurrencyObject](#CurrencyObject)         | 通貨単位と金額を持つオブジェクト               |
| [DateObject](#DateObject)                 | 日付を持つオブジェクト                         |
| [DateTimeObject](#DateTimeObject)         | 日付と時間を持つオブジェクト                    |
| [NumberObject](#NumberObject)             | 3桁区切りされた数値、または風速などの単位が含まれた測定値を持つオブジェクト |
| [PercentageObject](#PercentageObject)     | パーセントの数値を持つオブジェクト                        |
| [PhoneNumberObject](#PhoneNumberObject)   | 電話番号を持つオブジェクト                     |
| [StringObject](#StringObject)             | テキストを持つオブジェクト                        |
| [TemperatureCObject](#TemperatureCObject) | 温度（摂氏）を持つオブジェクト                    |
| [TemperatureFObject](#TemperatureFObject) | 温度（華氏）を持つオブジェクト                    |
| [URIObject](#URIObject)                   | URIを持つオブジェクト                         |


# ActionObject {#ActionObject}
クライアントがユーザーに提供するアクションを持つオブジェクトです。

### Object fields
| フィールド名       | データ型    | フィールドの説明                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"action"`に固定されています。  |
| `value`         | string  | [アクションURIスキーム](/Develop/References/ContentTemplates/Common_Fields.md#ActionURIScheme)形式の値 |

### Object Example
{% raw %}

```json
{
  "type": "action",
  "value": "clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}Search?query=梨泰院のグルメ店"
}
```

{% endraw %}

# CurrencyObject {#CurrencyObject}
通貨と金額を持つオブジェクトです。

### Object fields
| フィールド名       | データ型    | フィールドの説明                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"currency"`に固定されています。  |
| `value`         | string  | 通貨と金額を組み合わせた情報               |

### Object Example
{% raw %}

```json
{
  "type": "currency",
  "value": "KRW500000"
}
```

{% endraw %}

## DateObject {#DateObject}
日付を持つオブジェクトです。

### Object fields
| フィールド名       | データ型    | フィールドの説明                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"date"`に固定されています。  |
| `value`         | string  | 日付の情報。コンテンツテンプレートのタイプのよって、YYYY-MM-DDまたはYYYYMMDDの形式で表されます。    |

### Object Example
{% raw %}

```json
// サンプル1
{
  "type": "date",
  "value": "2017/05/29"
}

// サンプル2
{
  "type": "date",
  "value": "2018/01/05"
}
```

{% endraw %}

## DateTimeObject {#DateTimeObject}
日付と時間を持つオブジェクトです。

### Object fields
| フィールド名       | データ型    | フィールドの説明                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"datetime"`に固定されています。   |
| `value`         | string  | 日付と時間。コンテンツテンプレートのタイプのよって、YYYY-MM-DDThh:mm:ssZまたはYYYYMMDD hh:mmの形式で表されます。 |

### Object Example
{% raw %}

```json
//例1：YYYY-MM-DDThh:mm:ssZの形式
{
  "type": "datetime",
  "value": "2017-07-26T18:00:00Z"
}

//例2：YYYYMMDD hh:mmの形式
{
  "type": "datetime",
  "value": "20170726 18:00"
}
```

{% endraw %}

## NumberObject {#NumberObject}
3桁区切りされた数値、または風速などの単位が含まれた測定値を持つオブジェクトです。

### Object fields
| フィールド名       | データ型    | フィールドの説明                     |
|---------------|---------|-----------------------------
| `type`          | string  | `"number"`に固定されています。    |
| `value`         | string  | 3桁区切りされた数値、または単位が含まれた測定値 |

### Object Example
{% raw %}

```json
// サンプル1
{
  "type": "number",
  "value": "19,304,213"
}

// サンプル2
{
  "type": "number",
  "value": "2m/s"
}
```

{% endraw %}

## PercentageObject {#PercentageObject}
パーセントの数値を持つオブジェクトです。

### Object fields
| フィールド名       | データ型    | フィールドの説明                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"percentage"`に固定されています。 |
| `value`         | numberまたはstring  | パーセントの数値。コンテンツテンプレートのタイプによって、数値のみの場合もあり、パーセント記号が含まれる場合もあります。  |

### Object Example
{% raw %}

```json
// サンプル1
{
  "type": "percentage",
  "value": 20.2341
}

// サンプル2
{
  "type": "percentage",
  "value": "20%"
}
```

{% endraw %}

## PhoneNumberObject {#PhoneNumberObject}
電話番号を持つオブジェクトです。

### Object fields
| フィールド名       | データ型    | フィールドの説明                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"phoneNum"`に固定されています。 |
| `value`         | string  | 電話番号                    |

### Object Example
{% raw %}

```json
{
  "type": "phoneNum",
  "value": "031-784-1000"
}
```

{% endraw %}

## StringObject {#StringObject}
テキストを持つオブジェクトです。

### Object fields
| フィールド名       | データ型    | フィールドの説明                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"string"`に固定されています。  |
| `value`         | string  | テキスト情報                      |

### Object Example
{% raw %}

```json
// サンプル1
{
  "type": "string",
  "value": "トッテナムに入団したソン・フンミン「EPLは、まさに夢に見ていた舞台」"
}

// サンプル2
{
  "type": "string",
  "value": "検索結果"
}
```

{% endraw %}

## TemperatureCObject {#TemperatureCObject}
摂氏温度を持つオブジェクトです。

### Object fields
| フィールド名       | データ型    | フィールドの説明                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"temperature-c"`または`"temperature"`が入力されます。 |
| `value`         | numberまたはstring | 摂氏温度の情報                         |

### Object Example
{% raw %}

```json
// サンプル1
{
  "type": "temperature-c",
  "value": 31
}

// サンプル2
{
  "type": "temperature",
  "value": "-4"
}
```

{% endraw %}

## TemperatureFObject {#TemperatureFObject}
華氏温度を持つオブジェクトです。

### Object fields
| フィールド名       | データ型    | フィールドの説明                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"temperature-f"`に固定されています。   |
| `value`         | number  | 華氏温度の情報                      |

### Object Example
{% raw %}

```json
{
  "type": "temperature-f",
  "value": 75
}
```

{% endraw %}

## URIObject {#URIObject}
URIを持つオブジェクトです。

### Object fields
| フィールド名       | データ型    | フィールドの説明                     |
|---------------|---------|-----------------------------|
| `type`          | string  | `"url"`に固定されています。   |
| `value`         | string  | URIの情報                        |

### Object Example
{% raw %}

```json
// サンプル1
{
  "type": "url",
  "value": "https://m.search.contentservice.example.com/search?where=m_image&mode=default&query=%EC%86%90%ED%9D%A5%EB%AF%BC%20%EC%9D%B4%EB%AF%B8%EC%A7%80#imgId=news4100000269062_1"
}

// サンプル2
{
  "type": "url",
  "value": "https://search.pstatic.example.net/common/?src=http%3A%2F%2Fimgnews.contentservice.example.com%2Fimage%2F410%2F2015%2F08%2F31%2F20150831_1441012614_99_20150831181804.jpg&type=b360"
}
```

{% endraw %}
