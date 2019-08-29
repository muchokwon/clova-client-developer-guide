# CIC API
CIC APIは、CICがクライアントに提供するREST APIです。このドキュメントでは、CIC APIについて次の内容を説明しています。
* [APIの基本情報](#BasicInfo)
* [Downchannelを確立する](#EstablishDownchannel)
* [イベントを送信する](#SendEvent)
* [メッセージフォーマット](#CICMessageFormat)

## APIの基本情報 {#BasicInfo}
CIC APIを使用する前に、次の内容を知っておく必要があります。
* [ベースURI](#BaseURI)
* [マルチパートのメッセージ](#MultipartMessage)

### ベースURI {#BaseURI}
CIC APIのベースURIは、次の通りです。

<pre><code>{{ book.ServiceEnv.CICBaseURI }}
</code></pre>

### マルチパートのメッセージ {#MultipartMessage}
クライアントは、HTTP/2を使って、[イベント](#Event)を次のようなマルチパートのメッセージで送信します。

![](/Develop/Assets/Images/HTTP2_Structure.png)

例えば、ユーザーの音声入力をCICに送るには、[SpeechRecognizer.Recognize](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)イベントと共に、キャプチャしたユーザーの音声データを送信する必要があります。クライアントは、`Content-Type`を`multipart/form-data`に設定し、1つ目のメッセージパートにイベント情報のJSONデータを、2つ目のパートにユーザーの音声のバイナリデータを格納して送信することができます。

その際、メッセージを分けるすために、`boundary`にバウンダリ文字列を指定する必要があります。バウンダリ文字列は、メッセージパートの間に使用される場合、2個のダッシュ「--」で始まります。また、メッセージの終わりを表すときは、2個のダッシュ「--」が必要です。バウンダリ文字列は、メッセージボディで使用されないものにする必要があります。

次は、クライアントがCICにユーザーのリクエスト（イベント）を送信するときの一般的なメッセージ形式です。

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

通常のHTTPレスポンスとして、成功を示す[HTTPステータスコード](https://tools.ietf.org/html/rfc7231#section-6)（200）と共に[ディレクティブ](#Directive)が送信されます。レスポンスは、次のようなメッセージの組み合わせになります。
* [`Synthesizer.Speak`](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak)は音声を出力するディレクティブで、音声データが添付されます。
* `Synthesizer.Speak`ディレクティブと共に、追加の情報を持つディレクティブが送信されることがあります。例えば、ストリーム情報が含まれた[`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play)ディレクティブが追加で送信されることがあります。

上記の説明のように、CICからクライアントに対するレスポンスも、ディレクティブと音声データで構成されるマルチパートのメッセージが送信されます。次のような構造になります。

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


## Downchannelを確立する {#EstablishDownchannel}

```
GET /v1/directives
```

クライアントは、最初にCICとのDownchannelを確立する必要があります。Downchannelは、特定の条件や必要に応じて、CICで開始される（Cloud-initiated）ディレクティブを受信する際に使用されます。Downchannelの確立方法についての詳細は、[始めて接続する](/Develop/Guides/Interact_with_CIC.md#CreateConnection)を参照してください。

<div class="note">
  <p><strong>メモ</strong></p>
  <p>Downchannelを構成しないと、CICに<a href="#SendEvent">イベントを送信する</a>ことができません。</p>
</div>

### Request header

| リクエストヘッダー | 説明                                                                |
|----------------|--------------------------------------------------------------------|
| Authorization  | <p>取得したClovaアクセストークンを入力します。</p><pre><code>Bearer [Clova access token]</code></pre> |
| User-Agent     | <p><a href="/Develop/Guides/Interact_with_CIC.md#UserAgentString">User agent文字列</a>を入力します。</p><pre><code>User-Agent: [User-Agent string]</code></pre>  |

### Request example

<pre><code>GET /v1/directives HTTP/2
Host: {{ book.ServiceEnv.CICBaseURI }}
User-Agent: MyOrganizationName/MyAppName/2.1.2-release (Android 7.0;SettopBox;target=KR;other=sample)
Authorization: Bearer XHapQasdfsdfFsdfasdflQQ7w
</code></pre>

### Response header

| レスポンスヘッダー | 説明                                                                |
|-----------------|--------------------------------------------------------------------|
| Content-Type    | <p><a href="#MultipartMessage">マルチパートメッセージ</a>のタイプおよびバウンダリ文字列を指定します：</p><pre><code>multipart/form-data; boundary=[boundary_term]</code></pre> |

### Response message header

| レスポンスメッセージのヘッダー | 説明                                                                |
|-------------------------|--------------------------------------------------------------------|
| Content-Disposition     | <pre><code>form-data; name="[data]"</code></pre>                   |
| Content-Type            | <pre><code>application/json; charset=UTF-8</code></pre>            |

### Response message
CICは、HTTPレスポンスでクライアントに[Clova.Hello](/Develop/References/MessageInterfaces/Clova.md#Hello)ディレクティブを送信します。Downchannelが確立したことを示します。

### Status codes

| ステータスコード       | 説明                                  |
|---------------|--------------------------------------|
| 200 OK                    | Downchannelが正常に接続および設定されたことを示します。これにより、CICで開始される（Cloud-initiated）ディレクティブを受信することができます。        |
| 400 Bad Request           | リクエストに誤りがあったことを示します。                                                                       |
| 401 Unauthorized          | ユーザー認証に失敗したことを示します。[ユーザー認証](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)を再度行う必要があります。                       |
| 429 Too Many Request      | クライアントが同時またはごく短い時間内に、<code>/v1/directives</code>に複数の接続をリクエストしたことを示します。クライアントがDownchannelを確立する際、リクエストを一度だけにする必要があります。  |
| 500 Internal Server Error | サーバー内部にエラーが発生したことを示します。                                                                                                      |

### 備考
クライアントは、CICと常時1つのDownchannelを維持する必要があります。もし、すでにDownchannelが確立された状態で、<code>/v1/directives</code>にDownchannel確立のリクエストが追加で送信されると、前のDownchannelは解除されます。また、クライアントが同時またはごく短い時間内に、<code>/v1/directives</code>に2回以上接続しようとすると、CICはクライアントに対して429 Too Many Requestのエラーメッセージを送信します。

### Response example

{% raw %}
```
// リクエスト成功
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

// リクエスト失敗
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

## イベントを送信する {#SendEvent}

```
POST /v1/events
```

クライアントは、ユーザーから入力された音声やクライアントの現在のステータスを送信するために、イベントを送信する必要があります。HTTPリクエストでCICにイベントを送信し、HTTPレスポンスでディレクティブを受信します。イベントの送信や、受信したディレクティブの処理についての詳細は、[イベントを送信する](/Develop/Guides/Interact_with_CIC.md#SendEvent)と[ディレクティブを処理する](/Develop/Guides/Interact_with_CIC.md#HandleDirective)を参照してください。

### Request header

| リクエストヘッダー  | 説明                                                                |
|-----------------|--------------------------------------------------------------------|
| Authorization   | <p>取得したClovaアクセストークンを入力します。</p><pre><code>Bearer [Clova access token]</code></pre> |
| Content-Type    | <p><a href="#MultipartMessage">マルチパートメッセージ</a>のタイプおよびバウンダリ文字列を指定します：</p><pre><code>multipart/form-data; boundary=[boundary_term]</code></pre>  |
| User-Agent      | <p><a href="/Develop/Guides/Interact_with_CIC.md#UserAgentString">User agent文字列</a>を入力します。</p><pre><code>User-Agent: [User-Agent string]</code></pre>  |

### Request message header

| リクエストメッセージのヘッダー  | 説明                                                                |
|-------------------------|--------------------------------------------------------------------|
| Content-Disposition     | <pre><code>form-data; name="[data]"</code></pre>                   |
| Content-Type            | <ul><li>JSONデータ：<code>application/json; charset=UTF-8</code></li><li>バイナリオーディオデータ：<code>application/octet-stream</code></li></ul> |

### Request message
ユーザーのリクエスト、またはクライアントの情報をCICに送る際、[イベント](#Event)と追加の音声情報を[マルチパートのメッセージ](#MultipartMessage)で送信する必要があります。イベントは、含まれる情報によって内容と構成が異なり、[メッセージインターフェース](/Develop/References/Message_Interfaces.md)で区分されています。

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

--920d6335ba920d6337a319f
Content-Disposition: form-data; name="audio"
Content-Type: application/octet-stream

[[ binary audio attachment ]]
--920d6335ba920d6337a319f--
</code></pre>

### Response header

| レスポンスヘッダー | 説明                                                                |
|-----------------|--------------------------------------------------------------------|
| Content-Type    | <p><a href="#MultipartMessage">マルチパートメッセージ</a>のタイプおよびバウンダリ文字列を指定します：</p><pre><code>multipart/form-data; boundary=[boundary_term]</code></pre> |

### Response message header

| レスポンスメッセージのヘッダー | 説明                                                                |
|-------------------------|--------------------------------------------------------------------|
| Content-Disposition     | 内部で使用されるコンテンツのメタ情報                                         |
| Content-ID              | メッセージの識別子<ul><li>UUID形式</li><li>クライアントは、ディレクティブの<code>payload</code>に含まれた<code>cid:[UUID]</code>で、処理するメッセージを識別できます。</li></ul> |
| Content-Type            | <ul><li>JSONデータ：<code>application/json; charset=UTF-8</code></li><li>バイナリオーディオデータ：<code>application/octet-stream</code></li></ul>                     |

### Response message
CICはHTTPレスポンスで、クライアントに動作を実行するように指示する[ディレクティブ](#Directive)と、追加の音声情報を[マルチパートメッセージ](#MultipartMessage)で送信します。ディレクティブによって含まれる内容と構成が異なり、[メッセージインターフェース](/Develop/References/Message_Interfaces.md)で区分されています。

### Status codes

| ステータスコード       | 説明                                  |
|---------------|--------------------------------------|
| 200 OK                    | クライアントから送信したイベントがCICで正常に受信され、レスポンスにクライアントが実行するディレクティブが1つ以上含まれていることを示します。 |
| 204 No Content            | クライアントから送信したイベントがCICで正常に受信され、レスポンスにクライアントが実行するディレクティブが含まれていないことを示します。                    |
| 400 Bad Request           | イベントに誤りがあったことを示します。                                                  |
| 401 Unauthorized          | ユーザー認証に失敗したことを示します。[ユーザー認証](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)を再度行う必要があります。                        |
| 412 Precondition Failed   | リクエストを送信するための前提条件が満たされていないことを示します。主に、クライアントが[Downchannelを確立](#EstablishDownchannel)していないか、または[Downchannelを確立するときに構成した接続](/Develop/Guides/Interact_with_CIC.md#CreateConnection)でイベントを送信していない場合に発生します。  |
| 500 Internal Server Error | サーバー内部にエラーが発生したことを示します。                                                                                       |

### Response example

{% raw %}
```
// リクエスト成功
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
      "text":"初めまして"
    }
  }
}

--b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba--

// リクエスト失敗
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

## メッセージフォーマット {#CICMessageFormat}
CIC APIで使用されるメッセージは、次のようなものがあり、それぞれフォーマットが異なります。

* [イベント](#Event)
* [ディレクティブ](#Directive)
* [エラーメッセージ](#Error)

### イベント {#Event}
イベントは、クライアントからCICに対して、ユーザーの発話またはクライアントの情報を送る際に使用されます。代表的なイベントとして、ユーザーからの音声入力を受け、認識をリクエストする[`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)があります。

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

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `context[]`                      | object array | CICに送るクライアントのステータス情報を持つオブジェクト。次のような[コンテキスト](/Develop/References/Context_Objects.md)オブジェクトをこの配列の要素として含めることができます。イベントを送信する際、**コンテキストで表現できるすべてのクライアントの状態情報を含めます。**<ul><li><a href="/Develop/References/Context_Objects.md#AlertsState"><code>Alerts.AlertsState</code></a>：アラームまたはタイマーの状態</li><li><a href="/Develop/References/Context_Objects.md#PlaybackState"><code>AudioPlayer.PlaybackState</code></a>：最近の再生情報</li><li><a href="/Develop/References/Context_Objects.md#DeviceState"><code>Device.DeviceState</code></a>：クライアントのデバイス情報</li><li><a href="/Develop/References/Context_Objects.md#Display"><code>Device.Display</code></a>：クライアントのディスプレイ情報</li><li><a href="/Develop/References/Context_Objects.md#Location"><code>Clova.Location</code></a>：クライアントの位置情報</li><li><a href="/Develop/References/Context_Objects.md#SavedPlace"><code>Clova.SavedPlace</code></a>：事前定義された位置情報</li><li><a href="/Develop/References/Context_Objects.md#VolumeState"><code>Speaker.VolumeState</code></a>：スピーカーの情報</li></ul> |  |
| `event`                        | object       | イベントのヘッダーと必要なデータ（payload）を持つオブジェクト                                                                 |  |
| `event.header`                 | object       | イベントのヘッダー                                                                                                 |  |
| `event.header.dialogRequestId` | string       | ダイアログID（Dialog ID）。クライアントは、[`SpeechRecognizer.Regcognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)と[`TextRecognizer.Recognize`](/Develop/References/MessageInterfaces/TextRecognizer.md#Recognize)イベントを送信するとき、必ず[ダイアログIDを作成](/Develop/Guides/Manage_Dialogue_ID_And_Handle_Tasks.md#CreatingDialogueID)してこのフィールドに入力する必要があります。|任意 |
| `event.header.messageId`       | string       | メッセージID。メッセージを区別するための識別子です。                                                                 |  |
| `event.header.name`            | string       | イベントのAPI名                                                                                             |  |
| `event.header.namespace`       | string       | イベントのAPI名前空間                                                                                       |  |
| `event.payload`                | object       | イベントに関連する情報を持つオブジェクト。使用する[メッセージインターフェース](/Develop/References/Message_Interfaces.md)によって、ペイロードオブジェクトの構成とフィールド値が異なります。 |  |

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
```
{% endraw %}

#### 次の項目も参照してください。
* [コンテキスト](/Develop/References/Context_Objects.md)
* [メッセージインターフェース](/Develop/References/Message_Interfaces.md)

### ディレクティブ {#Directive}
ディレクティブは、クライアントがリクエストしたイベントに応答したり、特定の条件によってクライアントに情報を渡す際に使用されます。ディレクティブは、主にユーザーの音声が認識されてから、クライアントにその意図を実行するように指示します。クライアントは、ディレクティブに含まれた意図に応じて、適切な結果をユーザーに提供したり、作業を処理する必要があります。

<div class="note">
  <p><strong>メモ</strong></p>
  <p>クライアントでサポートしないことにしたディレクティブや、不明のディレクティブを受信した場合、そのディレクティブを無視する必要があります。クライアントは、CICからマルチパートメッセージ形式で、一度に複数のディレクティブを受信することがあります。サポートされていないか、または不明のディレクティブが含まれている場合、そのディレクティブのみを無視する必要があります。</p>
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

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `directive`                        | object | ディレクティブのヘッダーと必要なデータ（`payload`）を持つオブジェクト                                                           |      |
| `directive.header`                 | object | ディレクティブのヘッダー                                                                                                 |      |
| `directive.header.dialogRequestId` | string | ダイアログID（Dialog ID）。クライアント側で、受信した応答がどの対話に対するものなのかを確認するために使用されます。ディレクティブが[`SpeechRecognizer.Regcognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)イベントに対する応答ではない場合、このフィールドがディレクティブに含まれないこともあります。  | 条件付き  |
| `directive.header.messageId`       | string | メッセージID。メッセージを区別するための識別子です。                                                                |      |
| `directive.header.name`            | string | ディレクティブのAPI名                                                                                             |      |
| `directive.header.namespace`       | string | ディレクティブのAPI名前空間                                                                                       |      |
| `directive.payload`                | object | ディレクティブに関する情報を持つオブジェクト。使用する[メッセージインターフェース](/Develop/References/Message_Interfaces.md)によって、`payload`オブジェクトの構成とフィールド値が異なります。 |      |

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

#### 次の項目も参照してください。
* [メッセージインターフェース](/Develop/References/Message_Interfaces.md)

### エラーメッセージ {#Error}
無効なメソッドや形式で[イベント](#Event)が送信されたり、サーバー内部のエラーなどの理由により、Clovaが正常にサービスを提供できない場合があります。その際、CICはエラーメッセージをクライアントに送信します。クライアントはエラーメッセージに応じて、適切なUX/UIを提供する必要があります。

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
  <p><strong>ヒント</strong></p>
  <p>エラーメッセージは、<a href="#Directive">ディレクティブ</a>と類似の構造を持ちます。</p>
</div>

#### Message fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `directive`                        | object | エラーメッセージのヘッダーと必要なデータ（`payload`）を持つオブジェクト       |  |
| `directive.header`                 | object | エラーメッセージのヘッダー                                             |  |
| `directive.header.messageId`       | string | メッセージID。メッセージを区別するための識別子です。            |  |
| `directive.header.name`            | string | エラーメッセージの名前。`"Exception"`に固定されています。                |  |
| `directive.header.namespace`       | string | エラーメッセージの名前空間。`"System"`に固定されています。             |  |
| `directive.payload`                | object | エラーに関連する情報を持つオブジェクト                                |  |
| `directive.payload.code`           | number | エラーコード。メッセージが含まれたHTTPレスポンスのステータスコードと同じです。           |  |
| `directive.payload.description`    | string | エラーメッセージ                                                  |  |

#### Error code reference

| エラーコード | 説明                             |
|---------|---------------------------------|
| 400     | [イベント](#Event)に誤りがあったことを示します。                                                |
| 401     | ユーザー認証に失敗したことを示します。[ユーザー認証](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)を再度行う必要があります。 |
| 500     | サーバー内部にエラーが発生したことを示します。                                                                                |

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>上記のエラーコードは、レスポンスメッセージのHTTPステータスコードと同じです。なお、エラーコードは追加される可能性があります。</p>
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
