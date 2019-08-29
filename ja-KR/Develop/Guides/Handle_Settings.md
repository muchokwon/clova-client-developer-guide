# 設定情報を処理する

ユーザーは、クライアントデバイス本体またはClovaアプリからクライアントの設定を変更したり、Clovaアプリから特定のクライアントの設定情報を照会したりすることができます。そのために、Clovaはクライアントにディレクティブを送信して、クライアントが設定情報をレポートするか、または設定情報を変更するように指示します。そのとき、[`Settings`](/Develop/References/MessageInterfaces/Settings.md)インターフェースを使用します。次のように処理する必要があります。

* ユーザーがClovaアプリでクライアントデバイスの設定情報を照会するとき、**設定情報を同期する**必要があります。
* ユーザーがクライアントデバイスの**設定をClovaアプリからリモートで変更**したとき、それをデバイスの設定に反映する必要があります。
* ユーザーがクライアントデバイスで**設定を直接変更**したとき、それをClovaアプリに反映する必要があります。

ユーザーがClovaアプリでクライアントデバイスの設定情報を照会するとき、Clovaアプリは、クライアントデバイスの最新の設定情報をユーザーに提供する必要があります。以下の図は、このような**設定情報の同期**の動作の流れを示しています。

![](/Develop/Assets/Images/CIC_Settings_Synchronize_Settings_Info.svg)

設定情報を同期するとき、CICは、次のような[`Settings.ExpectReport`](/Develop/References/MessageInterfaces/Settings.md#ExpectReport)ディレクティブをクライアントに送信します。

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

上記のディレクティブを受信したクライアントは、現在の設定情報が含まれた[`Settings.Report`](/Develop/References/MessageInterfaces/Settings.md#Report)イベントをCICに送信する必要があります。Clovaは、そのイベントでレポートされた情報を、Clovaアプリが更新するように指示します。

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
  <p><strong>ヒント</strong></p>
  <p>設定データは、クライアントによって異なる形式で定義することができます。クライアントの設定情報の事前定義は、提携担当者までお問い合わせください。</p>
</div>

ユーザーがクライアントデバイスの**設定をClovaアプリからリモートで変更**したとき、それをデバイスの設定に反映する必要があります。そのようなリモートでの設定変更は、以下のような動作の流れで実装されます。

![](/Develop/Assets/Images/CIC_Settings_Change_Settings_Via_Clova_App.svg)

ユーザーがClovaアプリからクライアントの設定をリモートで変更すると、CICは次のような[`Settings.Update`](/Develop/References/MessageInterfaces/Settings.md#Update)ディレクティブをクライアントデバイスに送信します。

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

上記のディレクティブに定義された設定情報に応じてクライアントの設定を変更し、現在の設定情報が含まれた[`Settings.Report`](/Develop/References/MessageInterfaces/Settings.md#Report)イベントをCICに送信する必要があります。ユーザーは、Clovaアプリから変更された設定情報を確認することができます。

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

ユーザーがクライアントデバイスで**設定を直接変更する**こともできます。その場合の動作の流れは、以下のとおりです。クライアントは、変更済みの設定情報を[`Settings.Report`](/Develop/References/MessageInterfaces/Settings.md#Report)イベントでCICに送信する必要があります。

![](/Develop/Assets/Images/CIC_Settings_Change_Settings_On_Device.svg)

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>設定データは、クライアントによって異なる形式で定義することができます。クライアントの設定情報の事前定義は、提携担当者までお問い合わせください。</p>
</div>
