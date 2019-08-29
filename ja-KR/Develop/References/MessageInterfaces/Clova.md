# Clova

Clovaインターフェースは、CICからユーザーリクエストの認識結果をクライアントに送信する際に使用する名前空間です。ユーザーのリクエストを[`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)イベントで受信すると、Clovaはその意味を解析します。CICは認識された結果に応じて、以下のディレクティブをクライアントに送信します。クライアントは、以下のディレクティブを処理して、Clovaの機能をユーザーに提供する必要があります。

| メッセージ         | タイプ  | 説明                                   |
|------------------|-----------|---------------------------------------------|
| [`ExpectLogin`](#ExpectLogin)                    | ディレクティブ | クライアントに対して、ユーザーの{{ book.ServiceEnv.OrientedService }}アカウント認証（ログイン）を行うように指示します。 |
| [`FinishExtension`](#FinishExtension)            | ディレクティブ | クライアントに対して、特定のExtensionを終了するように指示します。             |
| [`HandleDelegatedEvent`](#HandleDelegatedEvent)  | ディレクティブ | クライアントに対して、Clovaアプリから[委任されたユーザーのリクエストを処理する](/Develop/Guides/Handle_Delegation.md)ように指示します。   |
| [`Hello`](#Hello)                                | ディレクティブ | クライアントに対して、Downchannelが確立したことを通知します。       |
| [`Help`](#Help)                                  | ディレクティブ | クライアントに対して、あらかじめ用意されたヘルプを提供するように指示します。       |
| [`LaunchURI`](#LaunchURI)                        | ディレクティブ | クライアントに、URIで表現されるウェブサイトまたはアプリを開いたり、起動するように指示します。                         |
| [`ProcessDelegatedEvent`](#ProcessDelegatedEvent) | イベント    | クライアントが、[委任されたユーザーのリクエスト](/Develop/Guides/Handle_Delegation.md)の結果をCICから受信するために使用します。  |
| [`RenderTemplate`](#RenderTemplate)              | ディレクティブ | クライアントに対して、テンプレートを表示するように指示します。                     |
| [`RenderText`](#RenderText)                      | ディレクティブ | クライアントに対して、テキストを表示するように指示します。                     |
| [`StartExtension`](#StartExtension)              | ディレクティブ | クライアントに対して、特定のExtensionを起動するように指示します。            |

## ExpectLoginディレクティブ {#ExpectLogin}

クライアントに対して、ユーザーの{{ book.ServiceEnv.OrientedService }}アカウント認証（ログイン）を行うように指示します。クライアントが[ゲストモード](/Develop/References/Clova_Auth_API.md#GuestMode)で動作しているときに、{{ book.ServiceEnv.OrientedService }}アカウント認証を必要とするサービスをユーザーに提供しようとする場合、CICはクライアントにこのディレクティブを送信します。

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `guideInfo`            | object  | **（Extension開発用）** ユーザーにアカウント認証を求めるとき、音声案内（TTS）のためのフレーズがあるオブジェクト。このオブジェクトフィールドがない場合、ユーザーにアカウント認証を求める際、音声案内を出力するように指示する<code>SpeechSynthesizer.Speak</code>ディレクティブを送信しません。<div class="tip"><p><strong>ヒント</strong></p><p>クライアントは、このディレクティブを受信する際、payloadがないディレクティブを受信します。</p></div><div class="note"><p><strong>メモ</strong></p><p>このディレクティブをExtensionの応答として使用する場合、Extensionのレスポンスメッセージの<code>response.outputSpeech</code>フィールドは空のオブジェクトである必要があります。</p></div>         | 条件付き     |
| `guideInfo.message`    | string  | **（Extension開発用）** ユーザーにアカウント認証を求めるとき、音声案内（TTS）のフレーズを入力します。入力したフレーズは、音声として合成され、クライアントから出力されます。<code>guideInfo.useDefault</code>フィールドが<code>true</code>の場合、このフィールドの値を入力する必要があります。それ以外の場合は、このフィールドを省略します。  | 選択  |
| `guideInfo.useDefault` | boolean | **（Extension開発用）** ユーザーにアカウント認証を求めるとき、デフォルトの案内フレーズを使用するかを示すフィールドです。<ul><li><code>true</code>：デフォルトの案内フレーズを使用する</li><li><code>false</code>：別の案内フレーズを使用する。別の案内フレーズを<code>guideInfo.message</code>フィールドに定義します。</li></ul>  |  |

### 備考
* ログインが成功したとき、前のリクエストの処理は継続されません。必要な場合、ユーザーから再度リクエストする必要があります。
* `Clova.ExpectLogin`ディレクティブのpayloadはExtensionのために提供されます。クライアントに送信されるときにはpayloadがありません。
* `Clova.ExpectLogin`ディレクティブをExtensionの応答として使用する場合、Extensionのレスポンスメッセージの`response.outputSpeech`フィールドは、空のオブジェクトである必要があります。

<div class="note">
  <p><strong>メモ</strong></p>
  <p>ゲストモードを使用するには、Clova事務局までお問い合わせください。</p>
</div>

### Message example

{% raw %}

```json
//サンプル1：クライアントが受信するメッセージ

{
  "directive": {
    "header": {
      "messageId": "2ca2ec70-c39d-4741-8a34-8aedd3b24760",
      "namespace": "Clova",
      "name": "ExpectLogin"
    },
    "payload": {}
  }
}

//サンプル2：Extensionのレスポンスメッセージに使用されたサンプル
{
  "version": "0.1.0",
  "sessionAttributes": {},
  "response": {
    "outputSpeech": {},
    "card": {},
    "directives": [
      {
        "header": {
          "namespace": "Clova",
          "name": "ExpectLogin"
        },
        "payload": {
          "guideInfo": {
            "useDefault": "false",
            "message": "ログインが必要なサービスです。ログインしてください。"
          }
        }
      }
    ],
    "shouldEndSession": true
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [Clovaアクセストークンを作成する](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)
* [ゲストモード](/Develop/References/Clova_Auth_API.md#GuestMode)

## FinishExtensionディレクティブ {#FinishExtension}

クライアントに対して、特定のExtensionを終了するように指示します。クライアントはFinishExtensionディレクティブを受信すると、それに対応するExtensionを終了する必要があります。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `extension`   | string  | 終了するExtensionの名前          |      |

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Clova",
      "name": "FinishExtension",
      "messageId": "80855309-77b8-403f-bec5-b50f565d24a2",
      "dialogRequestId": "3db18cee-caac-4101-891f-b5f5c2e7fa9c"
    },
    "payload": {
      "extension": "SampleExtension"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Clova.StartExtension`](#StartExtension)

## HandleDelegatedEventディレクティブ {#HandleDelegatedEvent}

クライアントに対して、Clovaアプリから[委任されたユーザーのリクエストを処理する](/Develop/Guides/Handle_Delegation.md)ように指示します。ユーザーはClovaアプリを使用する際、リクエストの処理結果をClovaアプリではなく、ユーザーが持っている別のクライアントデバイスで処理するように指定することができます。このディレクティブは、結果の処理を委任されたクライアントデバイスに送信されます。ディレクティブを受信したクライアントは、[`ProcessDelegatedEvent`](#ProcessDelegatedEvent)イベントをCICに送信して、ユーザーから委任されたリクエストの処理結果を受信する必要があります。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `delegationId` | string  | 委任されたリクエストのID。後で[`ProcessDelegatedEvent`](#ProcessDelegatedEvent)イベントを送信する際、`payload`に含まれる必要があります。         |      |

### 備考

このディレクティブはイベントに対する応答ではなく、[Downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)で送信されます。

### Message example

{% raw %}

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

{% endraw %}

### 次の項目も参照してください。
* [Clova.ProcessDelegatedEvent](#ProcessDelegatedEvent)
* [委任されたユーザーのリクエストを処理する](/Develop/Guides/Handle_Delegation.md)

## Helloディレクティブ {#Hello}

クライアントに対して、Downchannelが確立したことを通知します。クライアントは、このディレクティブでClovaサービスへ正常に[接続](/Develop/Guides/Interact_with_CIC.md#CreateConnection)できたかを確認できます。

### Payload fields

なし

### 備考
このディレクティブはダイアログID（`dialogRequestId`）を持ちません。

### Message example

{% raw %}

```json
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
```

{% endraw %}

### 次の項目も参照してください。
* [CICに接続する](/Develop/Guides/Interact_with_CIC.md#ConnectToCIC)

## Helpディレクティブ {#Help}

クライアントに対して、ヘルプを提供するように指示します。ユーザーがヘルプをリクエストしたとき、このディレクティブを受信します。あらかじめ用意されたヘルプの音声を再生するか、ヘルプのUIを画面に表示する必要があります。

### Payload fields

なし

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "messageId": "2ca2ec70-c39d-4741-8a34-8aedd3b24760",
      "namespace": "Clova",
      "name": "Help"
    },
    "payload": {}
  }
}
```

{% endraw %}

### 次の項目も参照してください。
なし

## LaunchURIディレクティブ {#LaunchURI}

クライアントに、URIで表現されるウェブサイトまたはアプリを開いたり、起動するように指示します。このディレクティブを受信したクライアントは、`targets[].uri`フィールドを使用してウェブサイトまたはアプリを起動する必要があります。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `targets[]`              | object array | URIの情報を持つオブジェクト配列                       |      |
| `targets[].description`  | string       | URIで表現される対象（アプリまたはウェブサイト）の説明           | 任意     |
| `targets[].iconImageUrl` | string       | URIで表現される対象のアイコン画像                 | 任意     |
| `targets[].marketUrl`    | string       | **URIで表現される対象がアプリの場合**、アプリストアのアドレス  | 任意     |
| `targets[].packageName`  | string       | **URIで表現される対象がアプリの場合**、アプリのパッケージ名  | 任意     |
| `targets[].title`        | string       | URIで表現される対象のタイトル                        |      |
| `targets[].uri`          | string       | 対象URIの情報                                  |      |

### 備考

* クライアントは、`targets[].uri`のURIから<a href="http://ogp.me/" target="_blank">Open Graph Protocol</a>のデータを利用してプレビューを表示できますが、一部のデータをすぐに表示するために、`targets[].iconImageUrl`、`targets[].title`、`targets[].description`などのフィールドを使用できます。
* **URIで表現される対象がアプリの場合**、`targets[].marketUrl`、`targets[].packageName`フィールドを使用できます。`targets[].marketUrl`は、アプリがインストールされていなく、対象のアプリを起動できない場合に使用されます。また、`targets[].packageName`は、`targets[].uri`でアプリを起動できない場合に参考できる付加情報です。
* クライアントは、`targets[]`フィールドの配列内のすべての対象を開いたり起動したりする必要があるわけではありません。配列の先頭の要素のURIで対象を開いたり起動したりするようにし、失敗した場合には次の要素情報で同じ動作を試す必要があります。配列構造は、対象を起動できない場合に備えて、候補群を一緒に渡すためのものです。

### Message example

```json
// App type example
{
  "directive": {
    "header": {
      "namespace": "Clova",
      "name": "LaunchURI",
      "messageId": "086ccadf-cd88-4cff-9706-cc4f0801c929",
      "dialogRequestId": "de20ea1a-ea39-4c2a-a033-73fa014b2fd5"
    },
    "payload": {
      "targets": [
        {
          "uri": "sampleapp2://main",
          "title": "Sample app2",
          "iconImageUrl": "https://example.com/sampleappicon.png",
          "marketUrl": "https://play.google.com/store/apps/details?id=com.example.sampleapp",
          "packageName": "com.example.sampleapp",
          "description": "Sample app2"
        },
        {
          "uri": "sampleapp://main",
          "title": "Sample app",
          "iconImageUrl": "https://example.com/sampleappicon.png",
          "marketUrl": "https://play.google.com/store/apps/details?id=com.example.sampleapp",
          "packageName": "com.example.sampleapp",
          "description": "Sample app"
        }
      ]
    }
  }
}

// Site type example
{
  "directive": {
    "header": {
      "namespace": "Clova",
      "name": "LaunchURI",
      "messageId": "fcb0919e-9847-46ec-90bc-ab0fe8216771",
      "dialogRequestId": "9ab7256a-6add-4b4a-a0b8-481f41d36a9d"
    },
    "payload": {
      "targets": [
        {
          "uri": "http://example.org",
          "title": "Example Domain"
        }
      ]
    }
  }
}
```

### 次の項目も参照してください。
なし

## ProcessDelegatedEventイベント {#ProcessDelegatedEvent}

クライアントが、[委任されたユーザーのリクエスト](/Develop/Guides/Handle_Delegation.md)の結果をCICから受信するために使用します。このイベントをCICに送信する際、[`HandleDelegatedEvent`](#HandleDelegatedEvent)ディレクティブで受信した`delegationId`をこのメッセージの`payload`に含める必要があります。クライアントは、そのディレクティブに対する応答として、ユーザーがClovaアプリにリクエストしたことの処理結果を受信します。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `delegationId` | string  | 委任されたリクエストのID。[`HandleDelegatedEvent`](#HandleDelegatedEvent)ディレクティブで受信した`delegationId`フィールドの値をそのまま入力する必要があります。         |      |

### Message example
{% raw %}
```json
{
  "context": [
    {{Alerts.AlertsState}},
    {{AudioPlayer.PlayerState}},
    {{Device.DeviceState}},
    {{Device.Display}},
    {{Clova.Location}},
    {{Clova.SavedPlace}},
    {{Speaker.VolumeState}},
    {{SpeechSynthesizer.SpeechState}}
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
{% endraw %}

### 次の項目も参照してください。
* [`Clova.HandleDelegatedEvent`](#HandleDelegatedEvent)
* [`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)
* [委任されたユーザーのリクエストを処理する](/Develop/Guides/Handle_Delegation.md)

## RenderTemplateディレクティブ {#RenderTemplate}

クライアントに対して、データをコンテンツテンプレートに基づいて表示するように指示します。ユーザーの音声から認識された結果のコンテンツが含まれます。

### Payload fields
`payload`は、[コンテンツテンプレート](/Develop/References/Content_Templates.md)の種類によって形式が異なります。現在、次のコンテンツテンプレートが提供されています。

* コンテンツのUIタイプごとのテンプレート
  * [CardList](/Develop/References/ContentTemplates/CardList.md)
  * [ImageList](/Develop/References/ContentTemplates/ImageList.md)
  * [ImageText](/Develop/References/ContentTemplates/ImageText.md)
  * [Popup](/Develop/References/ContentTemplates/Popup.md)
  * [Text](/Develop/References/ContentTemplates/Text.md)

* PIMSテンプレート
  * [ActionTimer](/Develop/References/ContentTemplates/ActionTimer.md)
  * [ActionTimerList](/Develop/References/ContentTemplates/ActionTimerList.md)
  * [Alarm](/Develop/References/ContentTemplates/Alarm.md)
  * [AlarmList](/Develop/References/ContentTemplates/AlarmList.md)
  * [Memo](/Develop/References/ContentTemplates/Memo.md)
  * [MemoList](/Develop/References/ContentTemplates/MemoList.md)
  * [Reminder](/Develop/References/ContentTemplates/Reminder.md)
  * [ReminderList](/Develop/References/ContentTemplates/ReminderList.md)
  * [Schedule](/Develop/References/ContentTemplates/Schedule.md)
  * [ScheduleList](/Develop/References/ContentTemplates/ScheduleList.md)
  * [Timer](/Develop/References/ContentTemplates/Timer.md)
  * [TimerList](/Develop/References/ContentTemplates/TimerList.md)

* 天気テンプレート
  * [Atmosphere](/Develop/References/ContentTemplates/Atmosphere.md)
  * [Humidity](/Develop/References/ContentTemplates/Humidity.md)
  * [TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)
  * [TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)
  * [WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)
  * [WindSpeed](/Develop/References/ContentTemplates/WindSpeed.md)

* 共通のフィールドおよび共有オブジェクト
  * [共通フィールド](/Develop/References/ContentTemplates/Common_Fields.md)
  * [共有オブジェクト](/Develop/References/ContentTemplates/Shared_Objects.md)

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Clova",
      "name": "RenderTemplate",
      "messageId": "c7b9882e-d3bf-45b6-b6f4-2fc47d04e60e",
      "dialogRequestId": "ca10e609-1d24-4418-bc2a-21b70c5ea1a7"
    },
    "payload": {
        {{TemplateFormat}}
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Clova.RenderText`](#RenderText)
* [Content template](/Develop/References/Content_Templates.md)

## RenderTextディレクティブ {#RenderText}

クライアントに対して、テキストメッセージを表示するように指示します。ユーザーに表示するテキストが含まれます。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `text`          | string  | ユーザーに表示するテキスト        |      |

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Clova",
      "name": "RenderText",
      "messageId": "6f9d8b54-21dc-4811-8e14-b9b60717cbd4",
      "dialogRequestId": "5690395e-8c0d-4123-9d3f-937eaa9285dd"
    },
    "payload": {
      "text": "楽しい曲を再生します。"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Clova.RenderTemplate`](#RenderTemplate)

## StartExtensionディレクティブ {#StartExtension}

クライアントに対して、特定のExtensionを起動するように指示します。クライアントは、StartExtensionディレクティブを受信すると、それに対応するExtensionを開始する必要があります。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `extension`   | string  | 開始するExtensionの名前          |      |

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Clova",
      "name": "StartExtension",
      "messageId": "103b8847-9109-42a5-8d3f-fc370a243d6f",
      "dialogRequestId": "8b509a36-9081-4783-b1cd-58d406205956"
    },
    "payload": {
      "extension": "SampleExtension"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Clova.FinishExtension`](#FinishExtension)
