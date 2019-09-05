## Sending events {#SendEvent}
The client can send [event messages](/Develop/References/CIC_API.md#Event) to CIC. Event messages are used for sending client requests to CIC. Messages can be sent as an event message in a JSON format, but can also send user voice input as a [multipart message format](/Develop/References/CIC_API.md#MultipartMessage).

To send user speech data from the client to CIC, use the [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize) event message. The following describes the method of sending an event to CIC using `SpeechRecognizer.Recognize`.

<ol>
  <li>Import an <a href="#RequiredLibrary">HTTP/2 library</a> on the client and make sure you have a <a href="#Authorization">Clova access token</a> ready.</li>
  <li>
    <p>Specify the HTTP header based on the <a href="/Develop/References/CIC_API.md#SendEvent">CIC API</a> specification and send the HTTP request using the HTTP/2 library.</p>
    <pre><code>:method = POST
:scheme = https
:path = /v1/events
User-Agent: MyOrganizationName/MyAppName/2.1.2-release (Android 7.0;SettopBox;target=KR;other=sample)
Authorization: Bearer XHapQasdfsdfFsdfasdflQQ7w-Example
Content-Type: multipart/form-data; boundary=Boundary-Text
</code></pre>
  </li>
  <li>Create a <a href="/Develop/Guides/Manage_Dialogue_ID_And_Handle_Tasks.md">dialogue ID</a> (<code>dialogRequestId</code>) and a message ID (<code>messageId</code>), both in UUID format. Make sure the IDs are unique, as they will later be used to find a matching directive message from <a href="#ManageMessageQ">message queues</a>.</li>
  <li>
    <p>The first part of the message should contain the message header and the message body in JSON, including event information based on the <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize"><code>SpeechRecognizer.Recognize</code></a> API specification.</p>
    <pre><code>--Boundary-Text
Content-Disposition: form-data; name="metadata"
Content-Type: application/json; charset=UTF-8<br/>
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
</code></pre>
  </li>
  <li>
    <p>From the second message part, send the user speech data in 200ms units. Following the changed data format, change the message header accordingly as below:</p>
    <pre><code>--Boundary-Text
Content-Disposition: form-data; name="audio"
Content-Type: application/octet-stream<br/>
[[ binary audio attachment ]]
--Boundary-Text--
</code></pre>
  </li>
  <li>Continue sending the speech data until CIC returns the <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#StopCapture"><code>SpeechRecognizer.StopCapture</code></a> directive message. Once the sending is complete, CIC returns an HTTP response message to the client.</li>
</ol>

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>To process a text input request, use the <a href="/Develop/References/MessageInterfaces/TextRecognizer.md#Recognize"><code>TextRecognizer.Recognize</code></a> event.</p>
</div>

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Make sure that event message are sent to CIC using the <a href="#CreateConnection">connection created when establishing the downchannel</a>.</p>
</div>
