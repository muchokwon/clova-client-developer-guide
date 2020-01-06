## Device.Audio {#Audio}

`Device.Audio`는 클라이언트 기기의 오디오 장치 정보를 CIC에게 보고할 때 사용하는 메시지 포맷입니다. 필수로 지원해야 하는 [플랫폼 지원 오디오 포맷](/Design/Audio.md#SupportedAudioFormat)뿐만 아니라 클라이언트 기기 지원하는 미디어 포맷 정보를 Clova에게 전달함으로써 클라이언트가 재생 가능한 오디오 콘텐츠를 응답으로 받을 수 있게 됩니다.

### Object structure
{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "Audio"
  },
  "payload": {
    "acceptContentType": [
      {
        "mimeType": {{string}},
        "audioCodec": {{string}},
        "container": {{string}}
      }
	  ]
  }
}
```

{% endraw %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `acceptContentType[]`                   | object array | 클라이언트 기기가 재생할 수 있는 미디어 포맷 목록. 클라이언트 기기가 재생할 수 있는 미디어 포맷 중 선호되는 미디어 포맷 순서로 배열을 구성해야 합니다. Clova 플랫폼은 이 목록과 목록의 순서를 참고하여 적절한 타입의 오디오 콘텐츠를 제공합니다.  | 필수 |
| `acceptContentType[].audioCodec`        | string | 오디오 압축 포맷(codec)  | 필수 |
| `acceptContentType[].container`         | string | 컨테이너 포맷  | 필수 |
| `acceptContentType[].mimeType`          | string | MIME 타입  | 필수 |


### Object example
{% raw %}

```json
{
  "header": {
    "namespace": "Device",
    "name": "Audio"
  },
  "payload": {
    "acceptContentType": [
      { "mimeType": "application/vnd.apple.mpegurl", "audioCodec": "mp3", "container": "mp3" },
      { "mimeType": "application/vnd.apple.mpegurl", "audioCodec": "aac", "container": "3gp,mp4,m4a,aac,ts" },
      { "mimeType": "audio/mpeg", "audioCodec": "mp3", "container":  "mp3" },
      { "mimeType": "audio/aac", "audioCodec": "aac", "container": "3gp,mp4,m4a,aac,ts" }
    ]
  }
}
```

{% endraw %}

### See also
* [플랫폼 지원 오디오 포맷](/Design/Audio.md#SupportedAudioFormat) - 필수 지원 포맷
