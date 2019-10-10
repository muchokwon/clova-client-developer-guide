## Sending events {#SendEvent}
The client can send [event messages](/Develop/References/CIC_API.md#Event) to CIC. Event messages are used for sending client requests to CIC. Messages can be sent as an event message in a JSON format, but can also send user voice input as a [multipart message format](/Develop/References/CIC_API.md#MultipartMessage).

To send user speech data from the client to CIC, use the [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize) event message. The following describes the method of sending an event to CIC using `SpeechRecognizer.Recognize`.

1. Import an [HTTP/2 library](#RequiredLibrary) on the client and make sure you have a [Clova access token](#Authorization) ready.
2. Specify the HTTP header based on the [CIC API](/Develop/References/CIC_API.md#SendEvent) specification and send the HTTP request using the HTTP/2 library.
  ```
  :method = POST
  :scheme = https
  :path = /v1/events
  User-Agent: MyOrganizationName/MyAppName/2.1.2-release (Android 7.0;SettopBox;target=KR;other=sample)
  Authorization: Bearer XHapQasdfsdfFsdfasdflQQ7w-Example
  Content-Type: multipart/form-data; boundary=Boundary-Text
  ```
3. Create a [dialogue ID](/Develop/Guides/Manage_Dialogue_ID_And_Handle_Tasks.md) (`dialogRequestId`) and a message ID (`messageId`), both in the UUID format. These must be used to distinguish the directive message in the [message queue](#ManageMessageQ) later on.
4. The first part of the message should contain the message header and the message body in JSON, including event information based on the <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize">`SpeechRecognizer.Recognize`</a> API specification.
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
        "lang": "ko",
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
5. From the second message part, send the user speech data in 200 ms units. Following the changed data format, change the message header accordingly as below:
  ```
  --Boundary-Text
  Content-Disposition: form-data; name="audio"
  Content-Type: application/octet-stream<br/>
  [[ binary audio attachment ]]
  --Boundary-Text--
  ```
6. Continue sending the speech data until CIC returns the <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#StopCapture">`SpeechRecognizer.StopCapture`</a> directive message. Once sending is complete, CIC returns an HTTP response message to the client.

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>To process a text input request, use the <a href="/Develop/References/MessageInterfaces/TextRecognizer.md#Recognize"><code>TextRecognizer.Recognize</code></a> event.</p>
</div>

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Make sure that event message are sent to CIC using the <a href="#CreateConnection">connection created when establishing the downchannel</a>.</p>
</div>
