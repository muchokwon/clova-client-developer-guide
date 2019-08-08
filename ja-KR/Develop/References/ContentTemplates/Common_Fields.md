# 共通フィールド
すべてのコンテンツテンプレートは、次の共通フィールドを持つことができます。共通フィールドは、コンテンツテンプレートオブジェクトの最上位に位置します。

| フィールド名        | データ型    | 説明                     | 任意 |
|----------------|---------|-----------------------------|:---------:|
| `actionList[]`     | [ActionObject](/Develop/References/ContentTemplates/Shared_Objects.md#ActionObject) array | UI操作などのユーザーインタラクションに応じて、ユーザーに提供するアクションを定義したオブジェクト配列です。ユーザーに提供するアクションは、[アクションURIスキーム](#ActionURIScheme)形式で送信されます。[CardList](/Develop/References/ContentTemplates/CardList.md)タイプのコンテンツテンプレートは、`cardList[]`フィールドの下位に定義されます。 | 条件付き |
| `failureMessage` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | UIにコンテンツテンプレートを表示できないときに表示するメッセージが含まれます。例えば、クライアントが`meta.version`に記されたコンテンツテンプレートのバージョンをサポートしていなかったり、テンプレートの情報を表示することに問題があるときに表示するメッセージです。 |  |
| `meta`             | object | コンテンツテンプレートに関するメタデータが含まれます。 |  |
| `meta.version`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | コンテンツテンプレートのバージョンが含まれます。 |  |
| `subtitle`         | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | サブタイトルや補助情報を表示するためのテキストを含みます。 | 条件付き |

## 共通フィールドのサンプル

{% raw %}
```json
{
  "type": "Atmosphere",
  "valueOfAtmosphere": {
    "type": "number",
    "value": "23㎍/㎥"
  },
  ...
  "failureMessage": {
    "type": "string",
    "value": "京畿道城南市盆唐区亭子1洞の今日のPM10指数は、良好です"
  },
  "subtitle": {
  "type": "string",
  "value": "亭子1洞の今日のPM10指数は、良好です"
  },​
  "meta": {
    "version": {
      "type": "string",
      "value": "v0.1"
    }
  }
}
```
{% endraw %}

## アクションURIスキーム {#ActionURIScheme}
共通フィールドの内、`actionList`には次のようなアクションURIスキーム形式の値が含まれています。この値は、ユーザーのUIインタラクションに応じて提供するアクションを定義したものです。クライアントは、アクションURIスキームの値を確認し、それに応じたアクションをユーザーに提供する必要があります。

| アクションURIスキーム           | クライアントのアクション                                               |
|-----------------------------|------------------------------------------------------------------|
| [clova://app-launch/default-addressbook](#AppLaunchDefaultAddrBook) | デフォルトのアドレス帳を実行する   |
| [clova://app-launch/default-browser](#AppLaunchDefaultBrowser)      | デフォルトのウェブブラウザを実行する |
| [clova://app-launch/default-camera](#AppLaunchDefaultCamera)        | デフォルトのカメラアプリを実行する   |
| [clova://app-launch/default-email](#AppLaunchDefaultEmail)          | デフォルトのメールアプリを実行する    |
| [clova://app-launch/default-gallery](#AppLaunchDefaultGallery)      | デフォルトのギャラリーアプリを実行する   |
| [clova://audio-repeat](#AudioRepeat)                                | オーディオを再生する     |
| [clova://device-control](#DeviceControl)                            | デバイスをコントロールする       |
| [clova://guide/talking](#GuideTalking)     | コマンドのヘルプを提供する                              |
| [clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}Search](#NaverSearch)        | {{ book.ServiceEnv.OrientedService }}アプリで特定のキーワードを検索する                    |
| [clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}-maps](#NaverMaps)           | {{ book.ServiceEnv.OrientedService }}マップアプリを実行する                            |
| [clova://ttsRepeat](#TTSRepeat)            | Text to speech発話を出力する                     |
| [clova://webview](#WebView)                | WebViewでウェブページを開く                          |

<div class="note">
  <p><strong>メモ</strong></p>
  <p>アクションURIスキームとディレクティブの違いは、ディレクティブの場合、クライアントがディレクティブを受信したらすぐに関連するアクションを実行するが、アクションURIスキームの場合、画面や他の手段で提供されるUIでユーザーのインタラクションが発生すると関連するアクションを提供する必要があります。クライアントは、上記の表に基づいて、アクションURIスキームに定義されたアクションを提供する必要があります。任意のアクションを実行すべきではありません。</p>
</div>

### clova://app-launch/default-addressbook {#AppLaunchDefaultAddrBook}

クライアントは、このスキームに応じて、デフォルトのアドレス帳アプリを実行する必要があります。このアクションURIスキームのサンプルは以下の通りです。

```
clova://app-launch/default-addressbook
```

### clova://app-launch/default-browser {#AppLaunchDefaultBrowser}

クライアントは、このスキームに応じて、デフォルトのウェブブラウザを実行する必要があります。このアクションURIスキームのサンプルは以下の通りです。

```
clova://app-launch/default-browser
```

### clova://app-launch/default-camera {#AppLaunchDefaultCamera}

クライアントは、このスキームに応じて、デフォルトのカメラアプリを実行する必要があります。このアクションURIスキームのサンプルは以下の通りです。

```
clova://app-launch/default-camera
```

### clova://app-launch/default-email {#AppLaunchDefaultEmail}

クライアントは、このスキームに応じて、デフォルトのメールアプリを実行する必要があります。このアクションURIスキームのサンプルは以下の通りです。

```
clova://app-launch/default-email
```

### clova://app-launch/default-gallery {#AppLaunchDefaultGallery}

クライアントは、このスキームに応じて、デフォルトのギャラリーアプリを実行する必要があります。このアクションURIスキームのサンプルは以下の通りです。

```
clova://app-launch/default-gallery
```

### clova://audio-repeat {#AudioRepeat}

クライアントは、このスキームに応じて、オーディオを再生する必要があります。

| パラメーター    | 説明                         | 任意 |
|---------------|-----------------------------|:--------:|
| url           | オーディオのURI                |  |

このアクションURIスキームのサンプルは以下の通りです。

```
clova://audio-repeat?url=https://target.audioFile.url
```

### clova://device-control {#DeviceControl}

クライアントは、このスキームに応じて、特定の機能をコントロールする必要があります。

| パラメーター    | 説明                         | 任意 |
|---------------|-----------------------------|:--------:|
| command       | コントロールのコマンド。<ul><li>BtConnect</li><li>BtDisconnect</li><li>BtStartPairing</li><li>BtStopPairing</li><li>Decrease</li><li>Increase</li><li>LaunchApp</li><li>OpenScreen</li><li>SetValue</li><li>TurnOn</li><li>TurnOff</li></ul>                      |  |
| target        | コントロールする対象。<ul><li><code>"airplane"</code>：機内モード</li><li><code>"app"</code>：アプリ</li><li><code>"bluetooth"</code>：Bluetooth</li><li><code>"cellular"</code>：セルラーネットワーク</li><li><code>"channel"</code>：テレビチャンネル</li><li><code>"flashlight"</code>：フラッシュライト</li><li><code>"gps"</code>：GPS</li><li><code>"powersave"</code>：省電力モード</li><li><code>"screenbrightness"</code>：画面の明るさ</li><li><code>"soundmode"</code>：サウンドモード</li><li><code>"volume"</code>：スピーカーの音量</li><li><code>"wifi"</code>：WiFi</li></ul> |  |
| value         | 設定値。`command`パラメーターが`setValue`の場合、スピーカーの音量（`"volume"`）または画面の明るさ（`"screenbrightness"`）を設定したり、テレビのチャンネル（`"channel"`）を設定したりするときに指定されます。 | 条件付き |

このアクションURIスキームのサンプルは以下の通りです。

```
clova://device-control?command=SetValue&target=volume&value=5
clova://device-control?command=TurnOn&target=flashlight
```

### clova://guide/talking {#GuideTalking}

クライアントは、このスキームに応じて、コマンドのヘルプを提供する必要があります。このアクションURIスキームのサンプルは以下の通りです。


```
clova://guide/talking
```

### clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}Search {#NaverSearch}

クライアントは、このスキームに応じて、{{ book.ServiceEnv.OrientedService }}アプリを実行し、検索を開始する必要があります。

| パラメーター    | 説明                         | 常時/条件付き |
|---------------|-----------------------------|:---------:|
| url           | {{ book.ServiceEnv.OrientedService }}アプリで開くページのURI |  |

このアクションURIスキームのサンプルは以下の通りです。

```
clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}Search?url=http://target.page.url
```

### clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}-maps {#NaverMaps}

クライアントは、このスキームに応じて、{{ book.ServiceEnv.OrientedService }}マップアプリを実行し、ルート検索を開始する必要があります。

| パラメーター    | 説明                         | 任意 |
|---------------|-----------------------------|:---------:|
| url           | {{ book.ServiceEnv.OrientedService }}マップアプリで開くURI   |  |

このアクションURIスキームのサンプルは以下の通りです。

```
clova://{{ book.ServiceEnv.OrientedServiceWithLowerCase }}-maps?url=http://target.map.url
```

### clova://ttsRepeat {#TTSRepeat}

クライアントは、このスキームに応じて、特定のテキストを音声で出力する必要があります。

| パラメーター    | 説明                         | 任意 |
|---------------|-----------------------------|:---------:|
| lang          | TTS（Text to Speech）の対象言語。<ul><li><code>"en"</code>：英語</li><li><code>"ja"</code>：日本語</li><li><code>"ko"</code>：韓国語</li></ul> |  |
| text          | 発話するテキスト                   | 条件付き |

このアクションURIスキームのサンプルは以下の通りです。

```
clova://ttsRepeat?lang=en&text=hello
```

### clova://webview {#WebView}
クライアントは、このスキームに応じて、WebViewで特定のページを開く必要があります。

| パラメーター    | 説明                         | 任意 |
|---------------|-----------------------------|:---------:|
| url           | 対象ページのURI              |  |
| auth_required | 認証が必要かどうかを示します。このパラメーターが`true`の場合、対象ページを開くときに認証APIを使用する必要があります。`false`か、または設定されていない場合、認証は必要ありません。 | 条件付き |

このアクションURIスキームのサンプルは以下の通りです。

```
clova://webview?url=https://target.page.url&auth_required=true
```
