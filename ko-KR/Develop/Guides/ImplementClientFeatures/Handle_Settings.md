# 설정 정보 처리하기

사용자는 클라이언트 기기 자체나 Clova 앱에서 클라이언트의 설정을 변경하거나 Clova 앱에서 특정 클라이언트의 설정 정보를 조회할 수 있습니다. 이를 위해 Clova는 클라이언트가 설정 정보를 보고하도록 만들거나 설정 정보를 변경하도록 지시 메시지를 클라이언트에게 보냅니다. 이때, [`Settings`](/Develop/References/CICInterface/Settings.md) 인터페이스를 사용하며, 다음과 같은 처리가 필요합니다.

* 사용자가 Clova 앱에서 클라이언트 기기의 설정 정보를 조회할 때 **설정 정보 동기화**가 필요합니다.
* 사용자가 클라이언트 기기의 **설정을 Clova 앱에서 원격으로 변경**했을 때 이를 기기의 설정 정보에 반영해야 합니다.
* 사용자가 클라이언트 기기에서 **직접 설정을 변경**했을 때 이를 Clova 앱에 반영해야 합니다.

사용자가 Clova 앱에서 클라이언트 기기의 설정 정보를 조회할 때 Clova 앱은 클라이언트 기기의 최신 설정 정보를 사용자에게 제공해야 합니다. 이런 **설정 정보 동기화**는 다음과 같은 동작 흐름을 가집니다.

![](/Develop/Assets/Images/CIC_Settings_Synchronize_Settings_Info.svg)

설정 정보를 동기화할 때 CIC는 다음과 같은 [`Settings.ExpectReport`](/Develop/References/CICInterface/Settings.md#ExpectReport) 지시 메시지를 클라이언트로 보냅니다.

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

위 지시 메시지를 받은 클라이언트는 현재 설정 정보를 담은 [`Settings.Report`](/Develop/References/CICInterface/Settings.md#Report) 이벤트 메시지를 CIC로 전송하면 됩니다. Clova는 이 이벤트 메시지를 통해 보고 받은 정보를 Clova 앱이 업데이트하도록 만듭니다.

```json
{
  "context": [
    ...
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

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>설정 데이터는 클라이언트마다 다르게 정의될 수 있습니다. 클라이언트 설정 정보에 대한 사전 정의는 제휴 담당자에게 문의합니다.</p>
</div>

사용자가 클라이언트 기기의 **설정을 Clova 앱에서 원격으로 변경**하면 이를 클라이언트 기기의 설정 정보에 반영해야 합니다. 이런 원격 설정 변경은 다음과 같은 동작 흐름으로 구현됩니다.

![](/Develop/Assets/Images/CIC_Settings_Change_Settings_Via_Clova_App.svg)

사용자가 Clova 앱에서 클라이언트의 설정을 원격으로 바꿨을 때 CIC는 다음과 같은 [`Settings.Update`](/Develop/References/CICInterface/Settings.md#Update) 지시 메시지를 클라이언트 기기에 전송합니다.

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

위 지시 메시지에 정의된 설정 정보에 따라 클라이언트의 설정을 바꾸고 현재 설정 정보를 담은 [`Settings.Report`](/Develop/References/CICInterface/Settings.md#Report) 이벤트 메시지를 CIC로 전송하면 됩니다. 사용자는 Clova 앱을 통해 변경된 설정 정보를 확인할 수 있게 됩니다.

```json
{
  "context": [
    ...
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

사용자가 직접 클라이언트 기기에서 **직접 설정을 변경**할 수 있으며 동작 흐름은 다음과 같습니다. 이때 클라이언트는 사용자가 변경한 후의 설정 정보를 [`Settings.Report`](/Develop/References/CICInterface/Settings.md#Report) 이벤트 메시지를 이용하여 CIC로 전송하면 됩니다.

![](/Develop/Assets/Images/CIC_Settings_Change_Settings_On_Device.svg)

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>설정 데이터는 클라이언트마다 다르게 정의될 수 있습니다. 클라이언트 설정 정보에 대한 사전 정의는 제휴 담당자에게 문의합니다.</p>
</div>
