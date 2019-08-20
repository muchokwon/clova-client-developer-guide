# TimerList Template
CIC는 사용자가 타이머의 목록을 요청하면 사용자에게 등록된 타이머의 목록을 TimerList 템플릿 형태로 클라이언트에게 전달합니다. 클라이언트는 이 템플릿을 사용하여 사용자가 등록한 타이머 목록을 화면에 표시해야 합니다.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>TimerList 템플릿은 현재 다음과 같은 제약 사항이 있습니다.</p>
<ul>
  <li>사용자는 음성으로 타이머 등록과 타이머 목록 조회만 요청할 수 있습니다.</li>
  <li>사용자가 타이머를 수정하거나 삭제하려면 Clova 앱을 이용해야 합니다.</li>
</ul>
</div>

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `timerList[]`               | object array  | 사용자의 등록한 타이머 목록을 가지는 객체 배열                                                                                         |
| `timerList[].scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 타이머가 울릴 날짜와 시간 정보를 가지는 객체                    |
| `timerList[].token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 타이머의 식별자 정보가 담긴 객체                             |
| `type`                      | string                                                                              | Content template 구분자. `"TimerList"` 값을 가집니다.      |

## Template example

{% raw %}

```json
{
  "type": "Timer",
  "timerList": [
    {
      "token": {
        "type": "string",
        "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-12-24T00:00:10Z"
      }
    },
    {
      "token": {
        "type": "string",
        "value": "da740e2a-01cd-4f2e-aedf-6c4285bae785"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-12-24T00:01:00Z"
      }
    }
  ]
}
```

{% endraw %}

## UI example {#UIExample}

다음은 {{ book.ServiceEnv.OrientedService }}가 배포한 모바일용 Clova 앱에서 TimerList 템플릿의 내용을 표현한 UI 예제입니다.

![](/Develop/Assets/Images/Content_Template-TimerList.png)

## See also
* [Alerts](/Develop/References/MessageInterfaces/Alerts.md) 인터페이스
* [Timer](/Develop/References/ContentTemplates/Timer.md)
