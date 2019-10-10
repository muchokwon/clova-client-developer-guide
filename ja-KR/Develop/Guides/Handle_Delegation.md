# 委任されたユーザーのリクエストを処理する

ユーザーはClovaアプリを使用する際、リクエストの処理結果をClovaアプリではなく、ユーザーが持っている別のクライアントデバイスで処理するように指定することができます。例えば、Clovaアプリで特定のアーティストの曲を再生するとき、ユーザーのスマートフォンではなく、Clovaフレンズスピーカーで再生するようにリクエストすることができます。そのことを、Clovaアプリがユーザーのリクエストを別のクライアントデバイスに委任したといいます。

Clovaアプリがリクエストの処理を委任すると、委任されるクライアントがDownchannelで不意にディレクティブを受信します。次は、ユーザーがリクエストを委任する仕組みを示します。**次の説明のうち、委任されるクライアントの動作を実装する必要があります。**

![](/Develop/Assets/Images/CIC_Handle_Event_Delegation.svg)

1. Clovaアプリは、CICにユーザーのリクエストを送信する際、別のクライアントデバイスにそのリクエストを委任します。
2. CICは、リクエストの処理を委任されたクライアントデバイスに、次のような[`Clova.HandleDelegatedEvent`](/Develop/References/MessageInterfaces/Clova.md#HandleDelegatedEvent)ディレクティブを[Downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)で送信します。
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
3. クライアントは、委任されたリクエストの処理結果をCICから受け取るために、[`Clova.ProcessDelegatedEvent`](/Develop/References/MessageInterfaces/Clova.md#ProcessDelegatedEvent)イベントをCICに送信する必要があります。<br />
  その際、ステップ2で受け取った`delegationId`フィールドの値を、そのまま`payload`フィールドに入力します。
  ```json
  {
    "context": [
      ...
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
4. CICは、クライアントに[`Clova.ProcessDelegatedEvent`](/Develop/References/MessageInterfaces/Clova.md#ProcessDelegatedEvent)イベントのレスポンスとして、ユーザーが委任したリクエストの処理結果を返します。
5. クライアントは、通常の[ディレクティブ](/Develop/Guides/Interact_with_CIC.md#HandleDirective)と同じく、レスポンスとして受け取ったディレクティブを処理します。
