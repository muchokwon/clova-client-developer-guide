# Settings

Settings 인터페이스는 Clova와 클라이언트 사이에서 클라이언트의 설정 정보를 업데이트하거나 동기화해야 할 때 사용되는 네임스페이스입니다. Settins가 제공하는 이벤트 메시지와 지시 메시지는 다음과 같습니다.

| 메시지 이름         | 메시지 타입  | 메시지 설명                                 |
|------------------|-----------|-------------------------------------------|
| [`ExpectReport`](#ExpectReport) | Directive | 클라이언트에게 현재의 설정 정보를 보고하도록 지시합니다. 클라이언트는 이 지시 메시지를 받으면 [`Settings.Report`](#Report) 이벤트 메시지를 CIC로 전송해야 합니다. |
| [`Report`](#Report)             | Event     | 클라이언트가 현재의 설정 정보를 CIC에게 보고합니다. 클라이언트가 CIC로부터 [`Settings.ExpectReport`](#ExpectReport) 지시 메시지를 받았다면 `Settings.Report` 이벤트 메시지를 CIC로 전송해야 합니다.  |
| [`Update`](#Update)             | Directive | 클라이언트에게 `payload`에 저장된 값을 설정값으로 적용하도록 지시합니다.  |

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>설정 정보를 업데이트하거나 동기화하는 설명은 <a href="/Develop/Guides/Handle_Settings.md">설정 정보 처리하기</a>를 참조합니다.</p>
</div>

## ExpectReport directive {#ExpectReport}
클라이언트에게 현재의 설정 정보를 보고하도록 지시합니다. 클라이언트는 이 지시 메시지를 받으면 [`Settings.Report`](#Report) 이벤트 메시지를 CIC로 전송해야 합니다.

### Payload fields

없음

### Remarks

* 이 지시 메시지는 이벤트 메시지에 대한 응답이 아닌 [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)을 통해 전달됩니다.

### Message example

```json
{
  "directive": {
    "header": {
      "namespace": "Settings",
      "name": "ExpectReport",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524"
    },
    "payload": {}
  }
}
```

### See also
* [`Settings.Report`](#Report)
* [설정 정보 처리하기](/Develop/Guides/Handle_Settings.md)

## Report event {#Report}
클라이언트가 현재의 설정 정보를 CIC에게 보고합니다. 클라이언트가 CIC로부터 [`Settings.ExpectReport`](#ExpectReport) 지시 메시지를 받았다면 `Settings.Report` 이벤트 메시지를 CIC로 전송해야 합니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `configuration` | object | 사전 정의된 클라이언트의 설정 정보를 가지는 객체. 이 객체의 하위 필드는 모두 string 타입을 가집니다.<div class="tip"><p><strong>Tip!</strong></p><p>설정 데이터는 클라이언트마다 다르게 정의될 수 있습니다. 클라이언트 설정 정보에 대한 사전 정의는 제휴 담당자에게 문의합니다.</p></div> | 필수   |

### Message example
{% raw %}
```json
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Speaker.VolumeState}},
    {{SpeechSynthesizer.SpeechState}}
  ],
  "event": {
    "header": {
      "namespace": "Settings",
      "name": "Report",
      "messageId": "b120c3e0-e6b9-4a3d-96de-71539e5f6214"
    },
    "payload": {
      "configuration": {
        "key1": "value1",
        "key2": "value2",
        ...
      }
    }
  }
}
```
{% endraw %}

### See also
* [`Settings.ExpectReport`](#ExpectReport)
* [설정 정보 처리하기](/Develop/Guides/Handle_Settings.md)

## Update directive {#Update}
클라이언트에게 `payload`에 저장된 값을 설정값으로 적용하도록 지시합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `configuration` | object | 사전 정의된 클라이언트의 설정 정보를 가지는 객체. 이 객체의 하위 필드는 모두 string 타입을 가집니다.<div class="tip"><p><strong>Tip!</strong></p><p>클라이언트 설정 정보에 대한 사전 정의는 제휴 담당자에게 문의합니다.</p></div> | 항상   |

### Remarks

* 이 지시 메시지는 이벤트 메시지에 대한 응답이 아닌 [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)을 통해 전달됩니다.

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Settings",
      "name": "Update",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524"
    },
    "payload": {
      "configuration": {
        "key1": "value1",
        "key2": "value2",
        ...
      }
    }
  }
}
```

{% endraw %}

### See also
* [`Settings.Report`](#Report)
* [설정 정보 처리하기](/Develop/Guides/Handle_Settings.md)
