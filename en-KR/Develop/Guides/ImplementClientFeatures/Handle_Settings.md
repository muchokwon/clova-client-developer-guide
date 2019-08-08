# Handling settings

Users can change the client settings from the client device or Clova app, or look up the settings of a specific client from the Clova app. To do this, Clova sends a directive to the client to report or change the settings. The [`Settings`](/Develop/References/CICInterface/Settings.md) interface is used for this process and requires the following tasks to be completed:

* When the user looks up the client device settings from the Clova app, **the settings information needs to be synchronized**.
* When the user has **changed the settings remotely from the Clova app**, the new settings must be applied to the device settings.
* When the user has **changed the settings directly from the client device**, the new settings must be applied to the app settings.

When the user looks up the client device settings from the Clova app, the Clova app must provide the latest settings of the client device to the user. **Synchronization of settings information** is implemented in the following action flow:

![](/Develop/Assets/Images/CIC_Settings_Synchronize_Settings_Info.svg)

When synchronizing the settings information, CIC sends the [`Settings.ExpectReport`](/Develop/References/CICInterface/Settings.md#ExpectReport) directive below to the client..

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

After receiving the directive message above, the client sends the [`Settings.Report`](/Develop/References/CICInterface/Settings.md#Report) event message containing the current settings information to CIC. Clova makes the Clova app update the information reported via this event message.

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
  <p>The settings information may be defined differently for each client. For any inquiries about predefining the client settings information, contact the Clova partnership team.</p>
</div>

When the user has **changed the settings remotely from the Clova app**, the new settings must be applied to the client device settings. Remote change of settings information is implemented in the following action flow:

![](/Develop/Assets/Images/CIC_Settings_Change_Settings_Via_Clova_App.svg)

When the user has changed the client settings remotely from the Clova app, CIC sends the [`Settings.Update`](/Develop/References/CICInterface/Settings.md#Update) directive message below to the client device.

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

Have the client change its settings according to the settings information defined in the above directive message. Then, send the [`Settings.Report`](/Develop/References/CICInterface/Settings.md#Report) event message containing the current settings C. The user will be able to check the changed settings from the Clova app.

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

The user can **change the settings directly** from the client device, and this is implemented in the action flow as shown below. Here, the client can send the changed settings information to CIC using the [`Settings.Report`](/Develop/References/CICInterface/Settings.md#Report) event message.

![](/Develop/Assets/Images/CIC_Settings_Change_Settings_On_Device.svg)

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>The settings information may be defined differently for each client. For any inquiries about predefining the client settings information, contact the Clova partnership team.</p>
</div>
