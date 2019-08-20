# ActionTimerList Template
CIC는 사용자가 액션 타이머의 목록을 요청하면 사용자에게 등록된 액션 타이머의 목록을 ActionTimerList 템플릿 형태로 클라이언트에게 전달합니다. 클라이언트는 이 템플릿을 사용하여 사용자가 등록한 액션 타이머 목록을 화면에 표시해야 합니다.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>ActionTimerList 템플릿은 현재 다음과 같은 제약 사항이 있습니다.</p>
<ul>
  <li>사용자는 음성으로 액션 타이머 등록과 액션 타이머 목록 조회만 요청할 수 있습니다.</li>
  <li>사용자가 액션 타이머를 수정하거나 삭제하려면 Clova 앱을 이용해야 합니다.</li>
</ul>
</div>

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `actionTimerList[]`               | object array  | 사용자가 등록한 액션 타이머 목록을 가지는 객체 배열                                              |
| `actionTimerList[].action`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 액션 타이머에 사용자가 설정한 동작이 담긴 객체. **현재 빈 문자열(`""`)이 입력되며 추후 확장을 위해 예약해둔 필드입니다.** |
| `actionTimerList[].label`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 사용자가 입력한 동작 내용이 담긴 객체 |
| `actionTimerList[].repeatDay[]`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | 매주 반복되는 액션 타이머이면 반복할 요일 정보를 가지고 있는 객체 배열 |
| `actionTimerList[].repeatPeriod`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 반복 주기 정보를 가지는 객체입니다. 이 객체의 `value` 필드는 다음과 같은 값을 가집니다. <ul><li>빈 문자열(<code>""</code>): 일회성 액션 타이머</li><li><code>"daily"</code>: 매일 반복되는 액션 타이머</li><li><code>"weekly"</code>: 매주 반복되는 액션 타이머</li></ul> |
| `actionTimerList[].scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 액션 타이머가 울릴 날짜와 시간 정보를 가지는 객체      |
| `actionTimerList[].token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 액션 타이머의 식별자 정보가 담긴 객체              |
| `type`        | string                                                                                                | Content template 구분자. `"ActionTimerList"` 값을 가집니다.             |

## Template example

{% raw %}

```json
{
  "type": "ActionTimerList",
  "actionTimerList": [
    {
      "token": {
        "type": "string",
        "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-10-01T14:00:00Z"
      },
      "action": {
        "type": "string",
        "value": ""
      },
      "label": {
        "type": "string",
        "value": "음악재생"
      },
      "repeatPeriod": {
        "type": "string",
        "value": ""
      },
      "repeatDay": []
    },
    {
      "token": {
        "type": "string",
        "value": "b5403bd0-1598-495b-a466-9385c2b1103a"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-10-02T09:00:00Z"
      },
      "action": {
        "type": "string",
        "value": ""
      },
      "label": {
        "type": "string",
        "value": "음악재생"
      },
      "repeatPeriod": {
        "type": "string",
        "value": "daily"
      },
      "repeatDay": []
    },
    {
      "token": {
        "type": "string",
        "value": "da740e2a-01cd-4f2e-aedf-6c4285bae785"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-10-03T11:00:00Z"
      },
      "action": {
        "type": "string",
        "value": ""
      },
      "label": {
        "type": "string",
        "value": "음악재생"
      },
      "repeatPeriod": {
        "type": "string",
        "value": "weekly"
      },
      "repeatDay": [
        {
          "type": "string",
          "value": "monday"
        }
      ]
    }
  ]
}
```

{% endraw %}

## UI example {#UIExample}

다음은 {{ book.ServiceEnv.OrientedService }}가 배포한 모바일용 Clova 앱에서 ActionTimerList 템플릿의 내용을 표현한 UI 예제입니다.

![](/Develop/Assets/Images/Content_Template-ActionTimerList.png)

## See also
* [ActionTimer](/Develop/References/ContentTemplates/ActionTimer.md)
* [Alerts](/Develop/References/MessageInterfaces/Alerts.md) 인터페이스
