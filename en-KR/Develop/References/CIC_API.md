# CIC API reference
The CIC API is a set of REST APIs provided for clients. The following covers topics related to CIC API.
* [API basic information](#BasicInfo)
* [Establishing a downchannel](#EstablishDownchannel)
* [Sending event messages](#SendEvent)
* [Message format](#CICMessageFormat)
* [Interface](#CICInterface)

## API basic information {#BasicInfo}
Before using CIC, we recommend you take a look at the basics of the CIC API.
* [Base URI](#BaseURI)
* [Multipart messages](#MultipartMessage)

### Base URI {#BaseURI}
The base URI of the CIC API is as follows:

<pre><code>{{ book.ServiceEnv.CICBaseURI }}
</code></pre>

### Multipart messages {#MultipartMessage}
The client sends [event messages](#Event) to CIC as multipart messages over the HTTP/2 protocol.

![](/Develop/Assets/Images/HTTP2_Structure.png)

Suppose you are sending a user voice request to CIC. You need to send the [SpeechRecognizer.Recognize](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize) event message to CIC, along with the audio recording, as a multipart message. To compose a multipart message for your event message: set the `Content-Type` to `multipart/form-data`, fill in the first message part with the event message information in JSON, then fill in the second message part with binary data containing the voice recording of the user.

Specify a boundary term in the `boundary` field to distinguish message blocks. When a boundary term is used between message parts, insert a double hyphen (--) on the left of boundary term. After the last message part, insert a double hyphen (--) on both sides of the boundary term. Make sure the boundary terms are not contained inside a message part.

The following is a general example of a user request (event message) to CIC as a multipart message.

{% raw %}
```
:method = POST
:scheme = https
:path = /v1/events
User-Agent: {{User-Agent_string}}
Authorization: Bearer {{clova-access-token}}
Content-Type: multipart/form-data; boundary=this-is-boundary-text

--this-is-boundary-text
[ Message Header ]
Content-Disposition: form-data; name="metadata"
Content-Type: application/json; charset=UTF-8

[ Message Body ]
{
  "context": [
  ],
  "event": {
    "header": {
      "namespace": {{string}},
      "name": {{string}},
      "dialogRequestId": {{string}},
      "messageId": {{string}}
    },
    "payload": {{object}}
  }
}

--this-is-boundary-text
[ Message Header ]
Content-Disposition: form-data; name="audio"
Content-Type: application/octet-stream

[[ binary audio attachment ]]

--this-is-boundary-text--

```
{% endraw %}

In general, an HTTP response would contain [directive messages](#Directive) and an [HTTP status code](https://tools.ietf.org/html/rfc7231#section-6) (200), which indicates success. In our example, the following directives would be received.
* [`Synthesizer.Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak) is a directive to output speech and delivers additional speech data.
* Another directive to send additional information may be sent along with the `Synthesizer.Speak` directive. For example, if the user request had been to play some music, the [`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play) directive with streaming information would be returned.

As previously explained, responses sent from CIC to the client are also in the form of a multipart message consisting of directives and speech data. This multipart message has the following structure:

{% raw %}
```
HTTP/2 {{HTTP STATUS CODE}}
Content-Type: multipart/related; boundary=this-is-boundary-text;
Date: {{DATETIME}}

--this-is-boundary-text
[ Message Header ]
Content-Disposition: form-data; name="metadata"
Content-Type: application/json; charset=UTF-8

[ Message Body ]
{
  "directive": {
    "header": {
      "namespace": {{string}},
      "name": {{string}},
      "dialogRequestId": {{string}},
      "messageId": {{string}}
    },
    "payload": {{object}}
  }
}

--this-is-boundary-text
[ Message Header ]
Content-Disposition: form-data; name="audio"
Content-Type: application/octet-stream

[[ binary audio attachment ]]

--this-is-boundary-text

...

--this-is-boundary-text--

```
{% endraw %}


## Establishing a downchannel {#EstablishDownchannel}

```
GET /v1/directives
```

The foremost task of a client is establishing a downchannel with CIC. A downchannel is used to send cloud-initiated directives to the client when conditions are met or when needs arise. For more information on establishing a downchannel, see [Connecting with CIC](/Develop/Guides/Interact_with_CIC.md#CreateConnection).

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The client must create a downchannel in order to <a href="#SendEvent">send event messages</a> to CIC.</p>
</div>

### Request header

| Request header | Description                                                                |
|----------------|--------------------------------------------------------------------|
| Authorization  | <p>Enter the obtained Clova access token.</p><pre><code>Bearer [Clova access token]</code></pre> |
| User-agent     | <p>Enter the <a href="/Develop/Guides/Interact_with_CIC.md#UserAgentString">user agent string</a>.</p><pre><code>User-Agent: [User-Agent string]</code></pre>  |

### Request example

<pre><code>GET /v1/directives HTTP/2
Host: {{ book.ServiceEnv.CICBaseURI }}
User-Agent: MyOrganizationName/MyAppName/2.1.2-release (Android 7.0;SettopBox;target=KR;other=sample)
Authorization: Bearer XHapQasdfsdfFsdfasdflQQ7w
</code></pre>

### Response header

| Response header | Description                                                                |
|-----------------|--------------------------------------------------------------------|
| Content-type    | <p>Declare the <a href="#MultipartMessage">multipart message</a> type and delimiter:</p><pre><code>multipart/form-data; boundary=[boundary_term]</code></pre> |

### Response message header

| Response message header | Description                                                                |
|-------------------------|--------------------------------------------------------------------|
| Content-Disposition     | <pre><code>form-data; name="[data]"</code></pre>                   |
| Content-Type            | <pre><code>application/json; charset=UTF-8</code></pre>            |

### Response message
As a response to the client request, CIC returns an HTTP response containing the [Clova.Hello](/Develop/References/CICInterface/Clova.md#Hello) directive message. This directive message indicates that a downchannel has been established between CIC and the client.

### Status codes

| Status code       | Description                                  |
|---------------|--------------------------------------|
| 200 OK                    | Successful connection setup of a downchannel. This will be followed by a cloud-initiated directive.        |
| 400 Bad Request           | User request format is wrong.                                                                       |
| 401 Unauthorized          | Failed to authenticate the user. Try [user authentication](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken) again.                       |
| 429 Too Many Requests      | This is an error that occurs when the client has requested for more than two connections to <code>/v1/directives</code> simultaneously or in a very short period of time. The client must make one request for downchannel configuration.  |
| 500 Internal Server Error | An internal server error has occurred.                                                                                                      |

### Remarks
The client must maintain one open downchannel with CIC at all times. When there is an open downchannel and an additional request to create a downchannel is made to <code>/v1/directives</code>, the existing downchannel will be closed. Also, if the client attempts more than two connections to <code>/v1/directives</code>simultaneously or in a very short period of time, CIC will return the "429 Too Many Requests" error message to the client.

### Response example

{% raw %}
```
// If the request was successful
HTTP/2 200
Content-Type: multipart/related; boundary=b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba;
date: Fri, 04 Aug 2017 05:27:12 GMT

--b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba
Content-Disposition: form-data; name="helloDirective-836d8db7-5e72-4fb2-9834-7c59291e1f8e"
Content-Type: application/json; charset=utf-8

{
    "directive": {
        "header": {
            "messageId": "2ca2ec70-c39d-4741-8a34-8aedd3b24760",
            "namespace": "Clova",
            "name": "Hello"
        },
        "payload": {}
    }
}
--b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba

// If the request has failed
HTTP/2 400
content-type: multipart/related; boundary=883fd3b825c9b883f99b9ffb4d2a2cbd7a24c9c61bfa69d70c51140f34ca;
date: Fri, 04 Aug 2017 09:42:46 GMT

--883fd3b825c9b883f99b9ffb4d2a2cbd7a24c9c61bfa69d70c51140f34ca
Content-Disposition: form-data; name="exception-bde71903-dab4-46c5-9714-416cf12debf0"
Content-Type: application/json; charset=utf-8

{
  "directive": {
    "header": {
      "namespace": "System",
      "name": "Exception",
      "messageId": "369b362b-258c-4104-bdf8-dc276548fe51"
    },
    "payload": {
      "code": 400,
      "description": "Could not decode multipart"
    }
  }
}
--883fd3b825c9b883f99b9ffb4d2a2cbd7a24c9c61bfa69d70c51140f34ca--
```
{% endraw %}

## Sending event messages {#SendEvent}

```
POST /v1/events
```

Send event messages to send user voice requests or the current client state to CIC. Event messages are sent as HTTP requests and an HTTP response is returned containing directive messages. For more information, see [Sending event messages](/Develop/Guides/Interact_with_CIC.md#SendEvent) and [Handling directive messages](/Develop/Guides/Interact_with_CIC.md#HandleDirective).

### Request header

| Request header  | Description                                                                |
|-----------------|--------------------------------------------------------------------|
| Authorization   | <p>Enter the obtained Clova access token.</p><pre><code>Bearer [Clova access token]</code></pre> |
| Content-type    | <p>Declare the <a href="#MultipartMessage">multipart message</a> type and delimiter:</p><pre><code>multipart/form-data; boundary=[boundary_term]</code></pre>  |
| User-agent      | <p>Enter the <a href="/Develop/Guides/Interact_with_CIC.md#UserAgentString">user agent string</a>.</p><pre><code>User-Agent: [User-Agent string]</code></pre>  |

### Request message header

| Request message header  | Description                                                                |
|-------------------------|--------------------------------------------------------------------|
| Content-Disposition     | <pre><code>form-data; name="[data]"</code></pre>                   |
| Content-type            | <ul><li>JSON data: <code>application/json; charset=UTF-8</code></li><li>Binary audio data: <code>application/octet-stream</code></li></ul> |

### Request message
To send a user request or the client state to CIC, you need to send an [event message](#Event) and additional speech data in the form of a [multipart message](#MultipartMessage). The content and structure of an event message are determined by the type of information the event message is delivering. This information type is categorized as [interfaces](#CICInterface).

### Request example

<pre><code>POST /v1/events HTTP/2
Host: {{ book.ServiceEnv.CICBaseURI }}
Accept: */*
User-Agent: MyOrganizationName/MyAppName/2.1.2-release (Android 7.0;SettopBox;target=KR;other=sample)
Authorization: Bearer XHapQasdfsdfFsdfasdflQQ7w
> Content-Length: 456
> Content-Type: multipart/form-data; boundary=920d6335ba920d6337a319f

--920d6335ba920d6337a319f
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

--920d6335ba920d6337a319f
Content-Disposition: form-data; name="audio"
Content-Type: application/octet-stream

[[ binary audio attachment ]]
--920d6335ba920d6337a319f--
</code></pre>

### Response header

| Response header | Description                                                                |
|-----------------|--------------------------------------------------------------------|
| Content-type    | <p>Declare the <a href="#MultipartMessage">multipart message</a> type and delimiter:</p><pre><code>multipart/form-data; boundary=[boundary_term]</code></pre> |

### Response message header

| Response message header | Description                                                                |
|-------------------------|--------------------------------------------------------------------|
| Content-disposition     | The content metadata for internal use.                                         |
| Content-ID              | The message ID.:<ul><li>UUID format</li><li>A client can identify the messages to process by the <code>cid:[UUID]</code> in the <code>payload</code> field of a directive message</li></ul> |
| Content-type            | <ul><li>JSON data: <code>application/json; charset=UTF-8</code></li><li>Binary audio data: <code>application/octet-stream</code></li></ul>                     |

### Response message
CIC returns an HTTP response containing the [directive message](#Directive) telling a client to perform a specific action and additional audio data in a form of [multipart message](#MultipartMessage). The content and structure of a directive message is determined by the information in the directive message sent from CIC. This information type is categorized as [interfaces](#CICInterface).

### Status codes

| Status code       | Description                                  |
|---------------|--------------------------------------|
| 200 OK                    | CIC has successfully received an event message from a client and the response contains at least one directive message for the client. |
| 204 No Content            | CIC has successfully received an event message from a client and contains no directive message for the client.                    |
| 400 Bad Request           | Event message is delivered using an incorrect method or in an invalid format.                                                  |
| 401 Unauthorized          | Failed to authenticate the user. Try [user authentication](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken) again.                        |
| 412 Precondition Failed   | The pre-condition required to send user request is not satisfied. This error occurs when the client has not [established a downchannel](#EstablishDownchannel) or has not sent an event message via the [connection created when establishing a downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection).  |
| 500 Internal Server Error | An internal server error has occurred.                                                                                       |

### Response example

{% raw %}
```
// If the request was successful
HTTP/2 200
Content-Type: multipart/related; boundary=b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba;
date: Fri, 04 Aug 2017 05:27:12 GMT

--b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba
Content-Disposition: form-data; name="speakDirective-836d8db7-5e72-4fb2-9834-7c59291e1f8e"
Content-Type: application/json; charset=utf-8

{
  "directive":{
    "header":{
      "namespace":"SpeechSynthesizer",
      "name":"Speak",
      "messageId":"dd4d463e-85a3-4514-927e-c90103c2dd02",
      "dialogRequestId":"4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload":{
      "format":"AUDIO_MPEG",
      "token":"e81f7dec-63fb-453d-8bd8-6944bed9a306",
      "ttsLang":"ko",
      "url":"cid:d329085c-379e-48aa-b871-7ecebdbe831d",
      "x-clova-pause-before":0
    }
  }
}

--b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba
Content-Disposition: form-data; name="attachment-39b2f844-b168-4dc2-bea7-d5c249e446e3"
Content-ID: d329085c-379e-48aa-b871-7ecebdbe831d
Content-Type: application/octet-stream

[[ binary audio attachment ]]

--b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba
Content-Disposition: form-data; name="renderTextDirective-b2c92b0f-27af-4f5c-b7e7-af7a270c464b"
Content-Type: application/json; charset=utf-8

{
  "directive":{
    "header":{
      "namespace":"Clova",
      "name":"RenderText",
      "messageId":"0fa1b36f-e86c-4979-9494-b00a162c4515",
      "dialogRequestId":"4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload":{
      "text":"Nice to meet you."
    }
  }
}

--b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba--

// If the request has failed
HTTP/2 400
content-type: multipart/related; boundary=883fd3b825c9b883f99b9ffb4d2a2cbd7a24c9c61bfa69d70c51140f34ca;
date: Fri, 04 Aug 2017 09:42:46 GMT

--883fd3b825c9b883f99b9ffb4d2a2cbd7a24c9c61bfa69d70c51140f34ca
Content-Disposition: form-data; name="exception-bde71903-dab4-46c5-9714-416cf12debf0"
Content-Type: application/json; charset=utf-8

{
  "directive": {
    "header": {
      "namespace": "System",
      "name": "Exception",
      "messageId": "369b362b-258c-4104-bdf8-dc276548fe51"
    },
    "payload": {
      "code": 400,
      "description": "Could not decode multipart"
    }
  }
}
--883fd3b825c9b883f99b9ffb4d2a2cbd7a24c9c61bfa69d70c51140f34ca--
```
{% endraw %}

## Message format {#CICMessageFormat}
The CIC API uses following types of messages, each with a different structure.

* [Events](#Event)
* [Directives](#Directive)
* [Error messages](#Error)

### Events {#Event}
Event messages are used by clients to send user voice request or the client state to CIC. One of the main event messages used is the [`SpeechRecognizer.Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize) event message, which requests CIC to receive and recognize user voice requests.

#### Message structure
{% raw %}
```json
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlaybackState}}
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Speaker.VolumeState}}
  ],
  "event": {
    "header": {
      "namespace": {{string}},
      "name": {{string}},
      "dialogRequestId": {{string}},
      "messageId": {{string}}
    },
    "payload": {{object}}
  }
}
```
{% endraw %}

#### Message fields

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `context[]`                      | object array | The object array that has the client state information to send to CIC. The [context](/Develop/References/Context_Objects.md) shown below can be used as object components. When sending an event message, **you can choose to include the state for all clients that can be shown as context.**<ul><li><a href="/Develop/References/Context_Objects.md#AlertsState"><code>Alerts.AlertsState</code></a>: Alarm or timer state</li><li><a href="/Develop/References/Context_Objects.md#PlaybackState"><code>AudioPlayer.PlaybackState</code></a>: Latest playback information</li><li><a href="/Develop/References/Context_Objects.md#DeviceState"><code>Device.DeviceState</code></a>: Client device information</li><li><a href="/Develop/References/Context_Objects.md#Display"><code>Device.Display</code></a>: Client display information</li><li><a href="/Develop/References/Context_Objects.md#Location"><code>Clova.Location</code></a>: Client location</li><li><a href="/Develop/References/Context_Objects.md#SavedPlace"><code>Clova.SavedPlace</code></a>: Predefined location</li><li><a href="/Develop/References/Context_Objects.md#VolumeState"><code>Speaker.VolumeState</code></a>: Speaker</li></ul> | Required |
| `event`                        | object       | The header and payload of the event message.                                                                 | Required |
| `event.header`                 | object       | The header of the event message.                                                                                                 | Required |
| `event.header.dialogRequestId` | string       | The dialogue ID. The client must [create a dialogue ID](/Develop/Guides/ImplementClientFeatures/Manage_Dialogue_ID_And_Handle_Tasks.md#CreatingDialogueID) and enter it in this field when sending the [`SpeechRecognizer.Regcognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize) event message and the [`TextRecognizer.Recognize`](/Develop/References/CICInterface/TextRecognizer.md#Recognize) event message.| Optional |
| `event.header.messageId`       | string       | The message ID. The message ID used to distinguish individual messages.                                                                 | Required |
| `event.header.name`            | string       | The API name of an event message.                                                                                             | Required |
| `event.header.namespace`       | string       | The API namespace of an event message.                                                                                       | Required |
| `event.payload`                | object       | The information related to the event message. The configuration and field values of the object depend on the ([CIC interface](#CICInterface)) of the event message. | Required |

#### Message example
{% raw %}
```json
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
```
{% endraw %}

#### See also
* [Context](/Develop/References/Context_Objects.md)
* [Interface](#CICInterface)

### Directives {#Directive}
Directive messages are used when CIC returns responses to event messages from clients or when CIC sends information to clients under certain conditions. Directives are usually sent containing instructions for client to perform as a result of a user request. The client must handle the job or provide the results according to the intention contained in the directive.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The client must ignore any unsupported or unknown directive messages. Because the client can receive many directive messages from CIC at once as a multipart message, it is particularly important that the client only ignores the unsupported or unknown directive messages among the received directive messages.</p>
</div>

#### Message structure
{% raw %}
```json
{
  "directive": {
    "header": {
      "namespace": {{string}},
      "name": {{string}},
      "dialogRequestId": {{string}},
      "messageId": {{string}}
    },
    "payload": {{object}}
  }
}
```
{% endraw %}


#### Message fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `directive`                        | object | The header and `payload` of the directive message.                                                           | Always     |
| `directive.header`                 | object | The header of the directive message.                                                                                                 | Always     |
| `directive.header.dialogRequestId` | string | The dialogue ID. The dialogue ID used to identify the dialogue associated with the directive message. This field may not be provided if this directive is not a response to the [`SpeechRecognizer.Regcognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize) event message.  | Conditional  |
| `directive.header.messageId`       | string | The message ID. The message ID used to distinguish individual messages.                                                                | Always     |
| `directive.header.name`            | string | The API name of the directive message.                                                                                             | Always     |
| `directive.header.namespace`       | string | The API namespace of the directive message.                                                                                       | Always     |
| `directive.payload`                | object | The object that contains information related to the directive message. The configuration and field values of the `payload` object depend on the ([interface](#CICInterface)) of the directive message. | Always     |

#### Message example
{% raw %}
```json
{
  "directive": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "Speak",
      "messageId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "dialogRequestId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "format": "AUDIO_MPEG",
      "token": "b5fa5144-1e55-4193-8628-c70283083d9b",
      "ttsLang": "ko",
      "url": "cid:9d5d37a3-0e70-41a6-a671-e1a40c7ea4d8",
      "x-clova-pause-before": 0
    }
  }
}
```
{% endraw %}

#### See also
* [Interface](#CICInterface)

### Error messages {#Error}
Clova may not be able to provide its service properly if you send [event messages](#Event) in an invalid format or via incorrect methods, or if an internal server error occurs. In such cases, CIC returns corresponding error message to the client. To handle errors, implement your client to provide corresponding UX or UI for the given error.

#### Message structure
{% raw %}
```json
{
  "directive": {
    "header": {
      "namespace": "System",
      "name": "Exception",
      "messageId": {{string}}
    },
    "payload": {
      "code": {{number}},
      "description": {{string}}
    }
  }
}
```
{% endraw %}

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>Error messages are created in a similar message structure to the <a href="#Directive">directive messages</a>.</p>
</div>

#### Message fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `directive`                        | object | The header and `payload` of the error message.       | Always |
| `directive.header`                 | object | The header of the error message.                                             | Always |
| `directive.header.messageId`       | string | The message ID. The message ID used to distinguish individual messages.            | Always |
| `directive.header.name`            | string | The name of the error message. The value is always `"Exception"`.                | Always |
| `directive.header.namespace`       | string | The namespace of the error message. The value is always `"System"`.             | Always |
| `directive.payload`                | object | The information related to the error.                                | Always |
| `directive.payload.code`           | number | The error code. This has the same value as the HTTP response code of the message.           | Always |
| `directive.payload.description`    | string | The error message.                                                  | Always |

#### Error code reference

| Error code | Description                             |
|---------|---------------------------------|
| 400     | [Event message](#Event) is delivered using an incorrect method or in an invalid format.                                                |
| 401     | Failed to authenticate the user. Try [user authentication](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken) again. |
| 500     | An internal server error has occurred.                                                                                |

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>The error code above is identical to the HTTP status codes of response messages. More error codes may be added in the future.</p>
</div>

### Message example
{% raw %}
```json
{
  "directive": {
    "header": {
      "namespace": "System",
      "name": "Exception",
      "messageId": "369b362b-258c-4104-bdf8-dc276548fe51"
    },
    "payload": {
      "code": 400,
      "description": "Could not decode multipart"
    }
  }
}
```
{% endraw %}

## Interface {#CICInterface}

CIC messages are defined and categorized into interfaces by their purpose and usage and each interface is defined in a namespace. Use these interfaces to create event messages to send to CIC or to interpret directive messages returned from CIC.

The available namespaces are shown below. Click each link to find out more about a namespace.

* [Alerts](/Develop/References/CICInterface/Alerts.md)
* [AudioPlayer](/Develop/References/CICInterface/AudioPlayer.md)
* [Clova](/Develop/References/CICInterface/Clova.md)
* [DeviceControl](/Develop/References/CICInterface/DeviceControl.md)
* [Notifier](/Develop/References/CICInterface/Notifier.md)
* [PlaybackController](/Develop/References/CICInterface/PlaybackController.md)
* [Settings](/Develop/References/CICInterface/Settings.md)
* [SpeechRecognizer](/Develop/References/CICInterface/SpeechRecognizer.md)
* [SpeechSynthesizer](/Develop/References/CICInterface/SpeechSynthesizer.md)
* [System](/Develop/References/CICInterface/System.md)
* [TemplateRuntime](/Develop/References/CICInterface/TemplateRuntime.md)
* [TextRecognizer](/Develop/References/CICInterface/TextRecognizer.md)

See the following indexes for a full list of interfaces grouped into event messages and directive messages.
* [Index for event messages](/Develop/References/CICInterface/Index_for_Events.md)
* [Index for directive messages](/Develop/References/CICInterface/Index_for_Directives.md)
