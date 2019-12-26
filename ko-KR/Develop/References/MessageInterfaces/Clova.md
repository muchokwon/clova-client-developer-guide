# Clova

Clova 인터페이스는 CIC가 사용자 요청이 인식된 결과를 클라이언트로 전달할 때 사용하는 네임스페이스입니다. 사용자의 요청이 [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize) 이벤트 메시지로 전달되면 Clova에서 그 의미를 분석합니다. CIC는 인식된 결과에 따라 아래 지시 메시지를 클라이언트에게 전달합니다. 클라이언트는 아래 지시 메시지들을 처리하여 Clova에서 제공하는 기능을 사용자에게 제공해야 합니다.

| 메시지 이름         | 메시지 타입  | 메시지 설명                                   |
|------------------|-----------|---------------------------------------------|
| [`ExpectLogin`](#ExpectLogin)                    | Directive | 클라이언트에게 사용자로부터 {{ book.ServiceEnv.OrientedService }} 계정 인증(login)을 받도록 지시합니다. |
| [`FinishExtension`](#FinishExtension)            | Directive | 클라이언트에게 특정 Extension을 종료하도록 지시합니다.             |
| [`HandleDelegatedEvent`](#HandleDelegatedEvent)  | Directive | 클라이언트에게 Clova 앱으로부터 [위임된 사용자의 요청을 처리](/Develop/Guides/Handle_Delegation.md)하도록 지시합니다.   |
| [`Hello`](#Hello)                                | Directive | 클라이언트에게 downchannel 연결 설정이 완료되었음을 알립니다.       |
| [`Help`](#Help)                                  | Directive | 클라이언트에게 미리 준비해둔 도움말 정보를 제공하도록 지시합니다.       |
| [`LaunchURI`](#LaunchURI)                        | Directive | 클라이언트에게 URI로 표현되는 사이트 혹은 앱을 열거나 실행하도록 지시합니다.                         |
| [`ProcessDelegatedEvent`](#ProcessDelegatedEvent) | Event    | 클라이언트가 [위임된 사용자 요청](/Develop/Guides/Handle_Delegation.md)에 대한 결과를 CIC로부터 받기 위해 사용됩니다.  |
| [`RenderTemplate`](#RenderTemplate)              | Directive | 클라이언트에게 템플릿을 표시하도록 지시합니다.                     |
| [`RenderText`](#RenderText)                      | Directive | 클라이언트에게 텍스트를 표시하도록 지시합니다.                     |
| [`StartExtension`](#StartExtension)              | Directive | 클라이언트에게 특정 Extension을 시작하도록 지시합니다.            |

## ExpectLogin directive {#ExpectLogin}

클라이언트에게 사용자로부터 {{ book.ServiceEnv.OrientedService }} 계정 인증(login)을 받도록 지시합니다. 클라이언트가 [guest 모드](/Develop/References/Clova_Auth_API.md#GuestMode)로 동작하는 중에 {{ book.ServiceEnv.OrientedService }} 계정 인증이 필요한 서비스를 사용자에게 제공해야 할 때 CIC는 클라이언트에게 이 지시 메시지를 전달합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `guideInfo`            | object  | **(Extension 개발용)** 사용자에게 계정 인증을 요청할 때 안내할 음성(TTS)용 문구 정보를 가지는 객체. 이 객체 필드가 없으면 사용자에게 계정 인증을 요청할 때 안내 음성을 출력하도록 <code>SpeechSynthesizer.Speak</code> 지시 메시지를 전달하지 않습니다.<div class="tip"><p><strong>Tip!</strong></p><p>클라이언트는 이 지시 메시지를 받을 때 payload가 없는 지시 메시지를 받게 됩니다.</p></div><div class="note"><p><strong>Note!</strong></p><p>이 지시 메시지를 extension의 응답으로 사용할 때, extension 응답 메시지의 <code>response.outputSpeech</code> 필드는 반드시 빈 객체여야 합니다.</p></div>         | 조건부     |
| `guideInfo.message`    | string  | **(Extension 개발용)** 사용자에게 계정 인증을 요청할 때 제공할 안내 음성에 대한 문구를 입력합니다. 입력된 문구는 음성 합성 후 클라이언트를 통해 출력됩니다. <code>guideInfo.useDefault</code> 필드가 <code>true</code>이면 이 필드 값을 반드시 입력하고 아니면 이 필드를 생략합니다.  | 선택  |
| `guideInfo.useDefault` | boolean | **(Extension 개발용)** 사용자에게 계정 인증을 요청할 때 기본 안내 메시지를 사용할지 결정하는 필드입니다.<ul><li><code>true</code>: 기본 안내 메시지 사용</li><li><code>false</code>: 별도의 안내 메시지를 사용. 별도 안내 메시지를 <code>guideInfo.message</code> 필드에 정의합니다.</li></ul>  | 필수 |

### Remarks
* 로그인이 성공했을 때 이전의 요청은 이어서 처리되지 않습니다. 필요하면 사용자가 재요청을 하여야 합니다.
* `Clova.ExpectLogin` 지시 메시지의 payload는 extension을 위해 제공되며, 클라이언트에게 전달될 때는 payload가 존재하지 않습니다.
* `Clova.ExpectLogin` 지시 메시지를 extension의 응답으로 사용할 때, extension 응답 메시지의 `response.outputSpeech` 필드는 반드시 빈 객체여야 합니다.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Guest 모드를 사용하려면 제휴 담당자에게 연락하시기 바랍니다.</p>
</div>

### Message example

{% raw %}

```json
// 예제 1: 클라이언트 수신 메시지

{
  "directive": {
    "header": {
      "messageId": "2ca2ec70-c39d-4741-8a34-8aedd3b24760",
      "namespace": "Clova",
      "name": "ExpectLogin"
    },
    "payload": {}
  }
}

// 예제 2: Extension 응답 메시지에 사용된 예
{
  "version": "0.1.0",
  "sessionAttributes": {},
  "response": {
    "outputSpeech": {},
    "card": {},
    "directives": [
      {
        "header": {
          "namespace": "Clova",
          "name": "ExpectLogin"
        },
        "payload": {
          "guideInfo": {
            "useDefault": "false",
            "message": "로그인이 필요한 서비스입니다. 로그인해주세요."
          }
        }
      }
    ],
    "shouldEndSession": true
  }
}
```

{% endraw %}

### See also
* [Clova access token 생성하기](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)
* [Guest 모드](/Develop/References/Clova_Auth_API.md#GuestMode)

## FinishExtension directive {#FinishExtension}

클라이언트에게 특정 Extension 사용을 종료하도록 지시합니다. 클라이언트는 FinishExtension 지시 메시지를 수신하면 해당 값에 대응하는 Extension을 종료해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `extension`   | string  | 종료할 Extension 이름          | 항상     |

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Clova",
      "name": "FinishExtension",
      "messageId": "80855309-77b8-403f-bec5-b50f565d24a2",
      "dialogRequestId": "3db18cee-caac-4101-891f-b5f5c2e7fa9c"
    },
    "payload": {
      "extension": "SampleExtension"
    }
  }
}
```

{% endraw %}

### See also
* [`Clova.StartExtension`](#StartExtension)

## HandleDelegatedEvent directive {#HandleDelegatedEvent}

클라이언트에게 Clova 앱으로부터 [위임된 사용자의 요청을 처리](/Develop/Guides/Handle_Delegation.md)하도록 지시합니다. 사용자는 Clova 앱을 사용할 때 요청에 대한 처리 결과를 Clova 앱이 아닌 사용자의 다른 클라이언트 기기가 결과를 받아서 처리하도록 지정할 수 있습니다. 이 지시 메시지는 결과 처리를 위임받은 클라이언트 기기에게 전달되며, 이 지시 메시지를 받은 클라이언트는 [`ProcessDelegatedEvent`](#ProcessDelegatedEvent) 이벤트 메시지를 CIC로 전송하여 사용자가 위임한 요청의 처리 결과를 받아야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `delegationId` | string  | 위임된 요청의 ID. 추후 [`ProcessDelegatedEvent`](#ProcessDelegatedEvent) 이벤트 메시지를 전송할 때 `payload`에 이 값을 포함시켜야 합니다.         | 항상     |

### Remarks

이 지시 메시지는 이벤트 메시지에 대한 응답이 아닌 [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)을 통해 전달됩니다.

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "messageId": "b17df741-2b5b-4db4-a608-85ecb1307b33",
      "namespace": "Clova",
      "name": "HandleDelegatedEvent"
    },
    "payload": {
      "delegationId": "99e86204-710a-4e94-b949-a763e78348a7"
    }
  }
}
```

{% endraw %}

### See also
* [Clova.ProcessDelegatedEvent](#ProcessDelegatedEvent)
* [위임된 사용자 요청 처리하기](/Develop/Guides/Handle_Delegation.md)

## Hello directive {#Hello}

클라이언트에게 downchannel 연결 설정이 완료되었음을 알립니다. 클라이언트는 이 지시 메시지를 통해 Clova 서비스에 대한 [접속 시도](/Develop/Guides/Interact_with_CIC.md#CreateConnection)가 제대로 수행되었는지 확인할 수 있습니다.

### Payload fields

없음

### Remarks
이 지시 메시지는 대화 ID(`dialogRequestId`)를 가지지 않습니다.

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "messageId": "2ca2ec70-c39d-4741-8a34-8aedd3b24760",
      "namespace": "Clova",
      "name": "Hello"
    },
    "payload": {}
  }
}
```

{% endraw %}

### See also
* [CIC 연결하기](/Develop/Guides/Interact_with_CIC.md#ConnectToCIC)

## Help directive {#Help}

클라이언트에게 도움말 정보를 제공하도록 지시합니다. 사용자가 도움말을 요청하면 이 지시 메시지를 전달받게 되며, 미리 준비해둔 도움말 음성 정보를 들려주거나 도움말 UI를 화면에 표시해야 합니다.

### Payload fields

없음

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "messageId": "2ca2ec70-c39d-4741-8a34-8aedd3b24760",
      "namespace": "Clova",
      "name": "Help"
    },
    "payload": {}
  }
}
```

{% endraw %}

### See also
없음

## LaunchURI directive {#LaunchURI}

클라이언트에게 URI로 표현되는 사이트 혹은 앱을 열거나 실행하도록 지시합니다. 이 지시 메시지를 수신한 클라이언트는 `targets[].uri` 필드를 이용하여 사이트 또는 앱을 실행해야 합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `targets[]`              | object array | URI 정보를 가지는 객체 배열                       | 항상     |
| `targets[].description`  | string       | URI로 표현되는 대상(앱 또는 사이트)의 설명           | 선택     |
| `targets[].iconImageUrl` | string       | URI로 표현되는 대상의 아이콘 이미지                 | 선택     |
| `targets[].marketUrl`    | string       | **URI로 표현되는 대상이 앱이면**, 앱 스토어의 주소  | 선택     |
| `targets[].packageName`  | string       | **URI로 표현되는 대상이 앱이면**, 앱의 패키지 이름  | 선택     |
| `targets[].title`        | string       | URI로 표현되는 대상의 제목                        | 필수     |
| `targets[].uri`          | string       | 대상 URI 정보                                  | 필수     |

### Remarks

* 클라이언트는 `targets[].uri`의 URI로부터 <a href="http://ogp.me/" target="_blank">Open Graph Protocol</a> 데이터를 이용하여 미리 보기를 표현할 수 있지만 일부 데이터를 즉시 표기하기 위해 `targets[].iconImageUrl`, `targets[].title`, `targets[].description` 등과 같은 필드를 이용할 수 있습니다.
* **URI로 표현되는 대상이 앱이면**, `targets[].marketUrl`, `targets[].packageName` 필드를 이용할 수 있습니다. `targets[].marketUrl`은 앱 미설치로 인해 대상 앱을 실행할 수 없을 때 사용되며, `targets[].packageName`는 `targets[].uri`로 앱을 실행할 수 없을 때 참고할 수 있는 부가 정보입니다.
* 클라이언트는 `targets[]` 필드의 배열에 있는 모든 대상을 열거나 실행해야 하는 것이 아닙니다. 배열 요소 중 첫 번째 요소의 URI 정보로 대상을 열거나 실행하도록 시도해야 하며, 실패 시 다음 배열 요소의 정보로 같은 동작을 시도해야 합니다. 즉, 배열 구조는 실행 불가 대상을 대비한 후보군을 함께 보내기 위해 사용됩니다.

### Message example

```json
// App type example
{
  "directive": {
    "header": {
      "namespace": "Clova",
      "name": "LaunchURI",
      "messageId": "086ccadf-cd88-4cff-9706-cc4f0801c929",
      "dialogRequestId": "de20ea1a-ea39-4c2a-a033-73fa014b2fd5"
    },
    "payload": {
      "targets": [
        {
          "uri": "sampleapp2://main",
          "title": "Sample app2",
          "iconImageUrl": "https://example.com/sampleappicon.png",
          "marketUrl": "https://play.google.com/store/apps/details?id=com.example.sampleapp",
          "packageName": "com.example.sampleapp",
          "description": "Sample app2"
        },
        {
          "uri": "sampleapp://main",
          "title": "Sample app",
          "iconImageUrl": "https://example.com/sampleappicon.png",
          "marketUrl": "https://play.google.com/store/apps/details?id=com.example.sampleapp",
          "packageName": "com.example.sampleapp",
          "description": "Sample app"
        }
      ]
    }
  }
}

// Site type example
{
  "directive": {
    "header": {
      "namespace": "Clova",
      "name": "LaunchURI",
      "messageId": "fcb0919e-9847-46ec-90bc-ab0fe8216771",
      "dialogRequestId": "9ab7256a-6add-4b4a-a0b8-481f41d36a9d"
    },
    "payload": {
      "targets": [
        {
          "uri": "http://example.org",
          "title": "Example Domain"
        }
      ]
    }
  }
}
```

### See also
없음

## ProcessDelegatedEvent event {#ProcessDelegatedEvent}

클라이언트가 [위임된 사용자 요청](/Develop/Guides/Handle_Delegation.md)에 대한 결과를 CIC로부터 받기 위해 사용됩니다. 이 이벤트 메시지를 CIC로 보낼 때 [`HandleDelegatedEvent`](#HandleDelegatedEvent) 지시 메시지를 통해 전달받은 `delegationId` 값을 이 메시지의 `payload`에 포함시켜야 합니다. 클라이언트는 이 지시 메시지에 대한 응답으로 사용자가 Clova 앱으로 요청했던 것에 대한 결과를 받게 됩니다.

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `delegationId` | string  | 위임된 요청의 ID. [`HandleDelegatedEvent`](#HandleDelegatedEvent) 지시 메시지를 통해 전달받은 `delegationId` 필드 값을 그대로 입력해야 합니다.         | 필수     |

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
      "namespace": "Clova",
      "name": "ProcessDelegatedEvent",
      "messageId": "b120c3e0-e6b9-4a3d-96de-71539e5f6214"
    },
    "payload": {
      "delegationId": "99e86204-710a-4e94-b949-a763e78348a7"
    }
  }
}
```
{% endraw %}

### See also
* [`Clova.HandleDelegatedEvent`](#HandleDelegatedEvent)
* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
* [위임된 사용자 요청 처리하기](/Develop/Guides/Handle_Delegation.md)

## RenderTemplate directive {#RenderTemplate}

클라이언트에게 데이터를 content template에 따라 표시하도록 지시합니다. 사용자 음성 인식으로 파악된 결과 콘텐츠가 함께 전달됩니다.

### Payload fields
`payload`는 [content template](/Develop/References/Content_Templates.md) 종류에 따라 포맷이 달라집니다. 현재 다음과 같은 content template을 제공하고 있습니다.

* 콘텐츠 UI 유형별 템플릿
  * [CardList](/Develop/References/ContentTemplates/CardList.md)
  * [ImageList](/Develop/References/ContentTemplates/ImageList.md)
  * [ImageText](/Develop/References/ContentTemplates/ImageText.md)
  * [Popup](/Develop/References/ContentTemplates/Popup.md)
  * [Text](/Develop/References/ContentTemplates/Text.md)

* PIMS 템플릿
  * [ActionTimer](/Develop/References/ContentTemplates/ActionTimer.md)
  * [ActionTimerList](/Develop/References/ContentTemplates/ActionTimerList.md)
  * [Alarm](/Develop/References/ContentTemplates/Alarm.md)
  * [AlarmList](/Develop/References/ContentTemplates/AlarmList.md)
  * [Memo](/Develop/References/ContentTemplates/Memo.md)
  * [MemoList](/Develop/References/ContentTemplates/MemoList.md)
  * [Reminder](/Develop/References/ContentTemplates/Reminder.md)
  * [ReminderList](/Develop/References/ContentTemplates/ReminderList.md)
  * [Schedule](/Develop/References/ContentTemplates/Schedule.md)
  * [ScheduleList](/Develop/References/ContentTemplates/ScheduleList.md)
  * [Timer](/Develop/References/ContentTemplates/Timer.md)
  * [TimerList](/Develop/References/ContentTemplates/TimerList.md)

* 날씨 템플릿
  * [Atmosphere](/Develop/References/ContentTemplates/Atmosphere.md)
  * [Humidity](/Develop/References/ContentTemplates/Humidity.md)
  * [TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)
  * [TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)
  * [WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)
  * [WindSpeed](/Develop/References/ContentTemplates/WindSpeed.md)

* 공통 필드 및 공유 객체
  * [공통 필드](/Develop/References/ContentTemplates/Common_Fields.md)
  * [공유 객체](/Develop/References/ContentTemplates/Shared_Objects.md)

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Clova",
      "name": "RenderTemplate",
      "messageId": "c7b9882e-d3bf-45b6-b6f4-2fc47d04e60e",
      "dialogRequestId": "ca10e609-1d24-4418-bc2a-21b70c5ea1a7"
    },
    "payload": {
        {{TemplateFormat}}
    }
  }
}
```

{% endraw %}

### See also
* [`Clova.RenderText`](#RenderText)
* [Content template](/Develop/References/Content_Templates.md)

## RenderText directive {#RenderText}

클라이언트에게 텍스트 메시지를 표시하도록 지시합니다. 사용자에게 보여줄 텍스트가 함께 전달됩니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `text`          | string  | 사용자에게 보여줄 텍스트        | 항상     |

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Clova",
      "name": "RenderText",
      "messageId": "6f9d8b54-21dc-4811-8e14-b9b60717cbd4",
      "dialogRequestId": "5690395e-8c0d-4123-9d3f-937eaa9285dd"
    },
    "payload": {
      "text": "신나는 노래 들려드릴게요."
    }
  }
}
```

{% endraw %}

### See also
* [`Clova.RenderTemplate`](#RenderTemplate)

## StartExtension directive {#StartExtension}

클라이언트에게 특정 Extension을 시작하도록 지시합니다. 클라이언트는 StartExtension 지시 메시지를 수신하면 해당 값에 대응하는 Extension을 시작합니다.

### Payload fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `extension`   | string  | 시작할 Extension 이름          | 항상     |

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Clova",
      "name": "StartExtension",
      "messageId": "103b8847-9109-42a5-8d3f-fc370a243d6f",
      "dialogRequestId": "8b509a36-9081-4783-b1cd-58d406205956"
    },
    "payload": {
      "extension": "SampleExtension"
    }
  }
}
```

{% endraw %}

### See also
* [`Clova.FinishExtension`](#FinishExtension)
