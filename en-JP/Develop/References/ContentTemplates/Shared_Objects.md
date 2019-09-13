# Shared Objects
The content templates use the following JSON objects to represent commonly used data structures.

| Object name            | Description                                            |
|--------------------|---------------------------------------------------|
| [ActionObject](#ActionObject)             | The actions that can be performed by the client.   |
| [CurrencyObject](#CurrencyObject)         | The currency and amount of money.               |
| [DateObject](#DateObject)                 | The date information.                         |
| [DateTimeObject](#DateTimeObject)         | The date and time information.                    |
| [NumberObject](#NumberObject)             | The number with thousands separators, or a measurement with the number and unit like wind speed. |
| [PercentageObject](#PercentageObject)     | The percentage information.                        |
| [PhoneNumberObject](#PhoneNumberObject)   | The phone number.                     |
| [StringObject](#StringObject)             | The text information.                        |
| [TemperatureCObject](#TemperatureCObject) | The temperature information (Celsius).                    |
| [TemperatureFObject](#TemperatureFObject) | The temperature information (Fahrenheit).                    |
| [URLObject](#URLObject)                   | The URL information.                         |


# ActionObject {#ActionObject}
This object contains the actions that the client must perform.

### Object fields
| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `type`          | string  | The value is always `"action"`.  |
| `value`         | string  | The value in the form of [action URL scheme](/Develop/References/ContentTemplates/Common_Fields.md#ActionURLScheme). |

### Object Example
{% raw %}

```json
{
  "type": "action",
  "value": "clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}Search?query=Best restaurants in Itaewon"
}
```

{% endraw %}

# CurrencyObject {#CurrencyObject}
This object contains the amount and currency of money.

### Object fields
| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `type`          | string  | The value is always `"currency"`.  |
| `value`         | string  | The information on the amount and currency of money.               |

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
This object contains the date information.

### Object fields
| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `type`          | string  | The value is always `"date"`.  |
| `value`         | string  | The information on date. The value is displayed as YYYY-MM-DD or YYYYMMDD format depending on the content template type.    |

### Object Example
{% raw %}

```json
// Example 1
{
  "type": "date",
  "value": "2017-05-29"
}

// Example 2
{
  "type": "date",
  "value": "2018-01-05"
}
```

{% endraw %}

## DateTimeObject {#DateTimeObject}
This object contains the date and time information.

### Object fields
| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `type`          | string  | The value is always `"datetime"`.   |
| `value`         | string  | The information on date and time. The value is displayed as YYYY-MM-DDThh:mm:ssZ or YYYYMMDD hh:mm format depending on the content template type. |

### Object Example
{% raw %}

```json
// Example 1: YYYY-MM-DDThh:mm:ssZ format
{
  "type": "datetime",
  "value": "2017-07-26T18:00:00Z"
}

// Example 2: YYYYMMDD hh:mm format
{
  "type": "datetime",
  "value": "20170726 18:00"
}
```

{% endraw %}

## NumberObject {#NumberObject}
This object contains the number with thousands separators, or a measurement with the number and unit like wind speed.

### Object fields
| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------
| `type`          | string  | The value is always `"number"`.    |
| `value`         | string  | The number with thousands separators, or a measurement with the number and unit. |

### Object Example
{% raw %}

```json
// Example 1
{
  "type": "number",
  "value": "19,304,213"
}

// Example 2
{
  "type": "number",
  "value": "2m/s"
}
```

{% endraw %}

## PercentageObject {#PercentageObject}
This object contains the percentage information.

### Object fields
| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `type`          | string  | The value is always `"percentage"`. |
| `value`         | number or string  | The percentage information. This may contain only a number or may also contain a percentage sign depending on the content template type.  |

### Object Example
{% raw %}

```json
// Example 1
{
  "type": "percentage",
  "value": 20.2341
}

// Example 2
{
  "type": "percentage",
  "value": "20%"
}
```

{% endraw %}

## PhoneNumberObject {#PhoneNumberObject}
This object contains the phone number.

### Object fields
| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `type`          | string  | The value is always `"phoneNum"`. |
| `value`         | string  | The phone number.                    |

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
This object contains text.

### Object fields
| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `type`          | string  | The value is always `"string"`.  |
| `value`         | string  | The text information.                      |

### Object Example
{% raw %}

```json
// Example 1
{
  "type": "string",
  "value": "Son Heung-min joins Tottenham. "It's a dream-come-true to play in the EPL""
}

// Example 2
{
  "type": "string",
  "value": "search result"
}
```

{% endraw %}

## TemperatureCObject {#TemperatureCObject}
This object contains temperature information in Celsius.

### Object fields
| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `type`          | string  | Available values are `"temperature-c"` or `"temperature"`. |
| `value`         | number or string | The temperature in Celsius.                         |

### Object Example
{% raw %}

```json
// Example 1
{
  "type": "temperature-c",
  "value": 31
}

// Example 2
{
  "type": "temperature",
  "value": "-4"
}
```

{% endraw %}

## TemperatureFObject {#TemperatureFObject}
This object contains temperature in Fahrenheit.

### Object fields
| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `type`          | string  | The value is always `"temperature-f"`.   |
| `value`         | number  | The temperature in Fahrenheit.                      |

### Object Example
{% raw %}

```json
{
  "type": "temperature-f",
  "value": 75
}
```

{% endraw %}

## URLObject {#URLObject}
This object contains the URL information.

### Object fields
| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `type`          | string  | The value is always `"url"`.   |
| `value`         | string  | A URL                        |

### Object Example
{% raw %}

```json
// Example 1
{
  "type": "url",
  "value": "https://m.search.contentservice.example.com/search?where=m_image&mode=default&query=%EC%86%90%ED%9D%A5%EB%AF%BC%20%EC%9D%B4%EB%AF%B8%EC%A7%80#imgId=news4100000269062_1"
}

// Example 2
{
  "type": "url",
  "value": "https://search.pstatic.example.net/common/?src=http%3A%2F%2Fimgnews.contentservice.example.com%2Fimage%2F410%2F2015%2F08%2F31%2F20150831_1441012614_99_20150831181804.jpg&type=b360"
}
```

{% endraw %}
