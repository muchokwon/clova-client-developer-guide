# ReminderList Template
CIC는 사용자가 리마인더의 목록을 요청하면 사용자에게 등록된 리마인더의 목록을 ReminderList 템플릿 형태로 클라이언트에게 전달합니다. 클라이언트는 이 템플릿을 사용하여 사용자가 등록한 리마인더 목록을 화면에 표시해야 합니다.

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>ReminderList 템플릿은 현재 다음과 같은 제약 사항이 있습니다.</p>
  <ul>
    <li>사용자는 음성으로 리마인더 등록과 리마인더 목록 조회만 요청할 수 있습니다.</li>
    <li>사용자가 리마인더를 수정하거나 삭제하려면 Clova 앱을 이용해야 합니다.</li>
  </ul>
</div>

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `reminderList[]`               | object array  | 사용자가 등록한 리마인더 목록을 가지는 객체 배열                                                                                          |
| `reminderList[].content`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | **(Deprecated)** 추가한 리마인더에 사용자가 입력한 내용이 담긴 객체. `label` 필드로 대체될 예정입니다. |
| `reminderList[].label`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 추가한 리마인더에 사용자가 입력한 내용이 담긴 객체. |
| `reminderList[].repeatDay[]`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | 매주 반복되는 리마인더이면 반복할 요일 정보를 가지고 있는 객체 배열 |
| `reminderList[].repeatPeriod`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 반복 주기 정보를 가지는 객체입니다. 이 객체의 `value` 필드는 다음과 같은 값을 가집니다. <ul><li>빈 문자열(<code>""</code>): 일회성 리마인더</li><li><code>"daily"</code>: 매일 반복되는 리마인더</li><li><code>"weekly"</code>: 매주 반복되는 리마인더</li></ul> |
| `reminderList[].status`        | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 리마인더의 처리 여부를 나타내는 객체입니다. 이 객체의 `value` 필드는 다음과 같은 값을 가집니다. <ul><li><code>"TODO"</code>: 미완료된 리마인더</li><li><code>"DONE"</code>: 완료된 리마인더</li></ul> |
| `reminderList[].scheduledTime` | [DateTimeObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject) | 리마인더가 울릴 날짜와 시간 정보를 가지는 객체      |
| `reminderList[].token`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)     | 리마인더의 식별자 정보가 담긴 객체  |
| `type`                         | string                                                                              | Content template 구분자. `"ReminderList"` 값을 가집니다.             |

## Template example

{% raw %}

```json
{
  "type": "ReminderList",
  "reminderList": [
    {
      "token": {
        "type": "string",
        "value": "072c72b9-cfc5-4127-b4fe-557a10457232"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-10-09T09:00:00Z"
      },
      "repeatPeriod": {
        "type": "string",
        "value": ""
      },
      "repeatDay": [],
      "content": {
        "type": "string",
        "value": "입금하기"
      },
      "label": {
        "type": "string",
        "value": "입금하기"
      },
      "status": {
        "type": "string",
        "value": "DONE"
      }
    },
    {
      "token": {
        "type": "string",
        "value": "b5403bd0-1598-495b-a466-9385c2b1103a"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-10-09T09:00:00Z"
      },
      "repeatPeriod": {
        "type": "string",
        "value": "daily"
      },
      "repeatDay": [],
      "content": {
        "type": "string",
        "value": "비타민 먹기"
      },
      "label": {
        "type": "string",
        "value": "비타민 먹기"
      },
      "status": {
        "type": "string",
        "value": "TODO"
      }
    },
    {
      "token": {
        "type": "string",
        "value": "da740e2a-01cd-4f2e-aedf-6c4285bae785"
      },
      "scheduledTime": {
        "type": "datetime",
        "value": "2017-10-09T09:00:00Z"
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
      ],
      "content": {
        "type": "string",
        "value": "청소하기"
      },
      "label": {
        "type": "string",
        "value": "청소하기"
      },
      "status": {
        "type": "string",
        "value": "TODO"
      }
    }
  ]
}
```

{% endraw %}

## UI example {#UIExample}

다음은 landscape 화면 형태에서 ReminderList 템플릿의 내용을 표현한 UI 예제입니다.

![Content_Template-ReminderList](/Develop/Assets/Images/Content_Template-ReminderList.png)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>화면의 어떤 부분에 어떤 필드의 데이터가 표시되어야 나타내는 이미지를 곧 업데이트할 예정입니다.</p>
</div>

## See also
* [Alerts](/Develop/References/MessageInterfaces/Alerts.md) 인터페이스
* [Reminder](/Develop/References/ContentTemplates/Reminder.md)
