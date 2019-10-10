## イベントを送信する {#SendEvent}
クライアントは、CICに[イベント](/Develop/References/CIC_API.md#Event)を送信することができます。イベントは、クライアントからのリクエストをCICに送るために使用されます。JSON形式のメッセージと、ユーザーからの音声入力を[マルチパートメッセージ](/Develop/References/CIC_API.md#MultipartMessage)で送信します。

クライアントからユーザーの音声データをCICに送信する際、[`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)イベントを使用します。以下では、`SpeechRecognizer.Recognize`を使ってCICにイベントを送信する方法について説明します。

1. イベントを送信するために、クライアントに[HTTP/2ライブラリ](#RequiredLibrary)と[Clovaアクセストークン](#Authorization)を用意します。
2. 以下のように、[CIC API](/Develop/References/CIC_API.md#SendEvent)に準拠して、HTTPヘッダーに用意した値を設定し、HTTP/2ライブラリを使用してリクエストを送信します。
  ```
  :method = POST
  :scheme = https
  :path = /v1/events
  User-Agent: MyOrganizationName/MyAppName/2.1.2-release (Android 7.0;SettopBox;target=KR;other=sample)
  Authorization: Bearer XHapQasdfsdfFsdfasdflQQ7w-Example
  Content-Type: multipart/form-data; boundary=Boundary-Text
  ```
3. イベントに含まれる[ダイアログID](/Develop/Guides/Manage_Dialogue_ID_And_Handle_Tasks.md)（`dialogRequestId`）とメッセージID（`messageId`）をUUID形式で作成します。後に、[メッセージキュー](#ManageMessageQ)からディレクティブを識別するときに使用する必要があります。
4. 最初のメッセージパートに、<a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize">`SpeechRecognizer.Recognize`</a>API仕様に準拠して作成されたJSON形式のイベントとメッセージヘッダーを入力して、CICに送信します。
  ```json
  --Boundary-Text
  Content-Disposition: form-data; name="metadata"
  Content-Type: application/json; charset=UTF-8
  {
    "context": [
      {
        "header": {
          "namespace": "Alerts",
          "name": "AlertsState"
        },
        "payload": {
          "allAlerts": [
            ...
          ],
          "activeAlerts": [
            ...
          ]
        }
      },
      ...
      {
        "header": {
          "namespace": "Speaker",
          "name": "VolumeState"
        },
        "payload": {
          "volume": 25,
          "muted": false
        }
      }
    ],
    "event": {
      "header": {
        "namespace": "SpeechRecognizer",
        "name": "Recognize",
        "messageId": "277b40c3-b046-4f61-a551-783b1547e7b7",
        "dialogRequestId": "4e4080d6-c440-498a-bb73-ae86c6312806"
      },
      "payload": {
        "lang": "ja",
        "profile": "CLOSE_TALK",
        "format": "AUDIO_L16_RATE_16000_CHANNELS_1",
        "initiator": {
          "type": "WAKEWORD",
          "inputSource": "SELF",
          "payload": {
            "deviceUUID": "f003af9d-14c5-424b-b1f9-f0134bd0ed86",
            "wakeWord": {
              "name": "clova",
              "confidence": 0.812312,
              "indices": {
                "startIndexInSamples": 0,
                "endIndexInSamples": 16000
              }
            }
          }
        }
      }
    }
  }
  --Boundary-Text--
  ```
5. 2つ目のメッセージパートから、ユーザーが入力した音声データを200ミリ秒ずつ切って送信します。データ形式が異なるため、メッセージヘッダーを次のように作成します。
  ```
  --Boundary-Text
  Content-Disposition: form-data; name="audio"
  Content-Type: application/octet-stream<br/>
  [[ binary audio attachment ]]
  --Boundary-Text--
  ```
6. CICから<a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#StopCapture">`SpeechRecognizer.StopCapture`</a>ディレクティブを受信するまで、音声データを送り続けます。送信が完了すると、CICからHTTPレスポンスメッセージを受信します。

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p><a href="/Develop/References/MessageInterfaces/TextRecognizer.md#Recognize"><code>TextRecognizer.Recognize</code></a>を使用して、ユーザーのテキスト入力を処理することもできます。</p>
</div>

<div class="note">
  <p><strong>メモ</strong></p>
  <p>CICイベントは、必ず<a href="#CreateConnection">Downchannelを確立するときに構成した接続</a>で送信する必要があります。</p>
</div>
