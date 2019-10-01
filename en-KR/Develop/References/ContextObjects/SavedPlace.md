## Clova.SavedPlace {#SavedPlace}
`Clova.SavedPlace` is a format for reporting to CIC about the locations saved on the client device.

### Object structure
{% raw %}
```json
{
  "header": {
    "namespace": "Clova",
    "name": "SavedPlace"
  },
  "payload": {
    "places": [
      {
        "latitude": {{string}},
        "longitude": {{string}},
        "refreshedAt": {{string}},
        "name": {{string}}
      }
    ]
  }
}
```
{% endraw %}

### Payload fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `places[]`             | object array | The object array of the location information saved on the client.                                          | Required |
| `places[].latitude`    | string       | The latitude of the location.                                                                          | Required |
| `places[].longitude`   | string       | The longitude of the location.                                                                          | Required |
| `places[].name`        | string       | The name of the location. Available values are: <ul><li><code>"Work"</code></li><li><code>"Home"</code></li></ul>       | Required |
| `places[].refreshedAt` | string       | The time when this location was last saved (`YYYY-MM-DDThh:mm:ss±hh:mm`, <a href="https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations" target="_blank">ISO 8601 Combined date and time representations</a>).  | Required |


### Object example
{% raw %}
```json
{
  "header": {
    "namespace": "Clova",
    "name": "SavedPlace"
  },
  "payload": {
    "places": [
      {
        "latitude": "37.3594915",
        "longitude": "127.1032242",
        "refreshedAt": "2017-04-06T13:34:15.074361+08:28",
        "name": "Home"
      },
      {
        "latitude": "36.3542315",
        "longitude": "125.1345242",
        "refreshedAt": "2017-03-12T10:21:33.089723+08:28",
        "name": "Work"
      }
    ]
  }
}
```
{% endraw %}

### See also
* [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)
