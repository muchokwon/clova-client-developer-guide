## Clova.Location {#Location}
`Clova.Location` is a format for reporting to CIC about the current location of the client. Use GPS or the network module of the client device to obtain location information.

### Object structure
{% raw %}
```json
{
  "header": {
    "namespace": "Clova",
    "name": "Location"
  },
  "payload": {
    "latitude": {{string}},
    "longitude": {{string}},
    "refreshedAt": {{string}}
  }
}
```
{% endraw %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `latitude`      | string  | The latitude of the location.                                                                                     | Required |
| `longitude`     | string  | The longitude of the location.                                                                                     | Required |
| `refreshedAt`   | string  | The time when the location was last checked in UTC (<a href="https://en.wikipedia.org/wiki/ISO_8601" target="_blank">ISO 8601</a>) | Required |

### Object example
{% raw %}
```json
{
  "header": {
    "namespace": "Clova",
    "name": "Location"
  },
  "payload": {
    "latitude": "37.3594915",
    "longitude": "127.1032242",
    "refreshedAt": "2017-04-06T13:34:15.074361+08:28"
  }
}
```
{% endraw %}

### See also
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
