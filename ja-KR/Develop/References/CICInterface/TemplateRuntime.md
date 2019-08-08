<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

# TemplateRuntime

<!-- Start of the shared content: CICAPIforAudioPlayback -->
TemplateRuntimeインターフェースは、クライアントまたはCICがメディアプレーヤーに表示するメタデータをリクエストしたり、送信したりする際に使用します。実際にオーディオストリームの再生に必要なデータに関連する作業を処理する際には[`AudioPlayer`](/Develop/References/CICInterface/AudioPlayer.md)インターフェースを、再生リストやアルバムの画像、歌詞のようなメタデータに関連する作業を処理する際には、`TemplateRuntime`インターフェースを使用します。該当するクライアントのみでなく、他のクライアントデバイスの再生メタデータを照会し、ユーザーに提供することもできます。

| メッセージ         | タイプ  | 説明                                   |
|------------------|---------|---------------------------------------------|
| [`ExpectRequestPlayerInfo`](#ExpectRequestPlayerInfo)   | ディレクティブ | クライアントに、再生メタデータをリクエストするように指示します。クライアントはこのディレクティブを受信すると、[`TemplateRuntime.RequestPlayerInfo`](#RequestPlayerInfo)イベントをCICに送信する必要があります。 |
| [`LikeCommandIssued`](#LikeCommandIssued)               | イベント     | ユーザーがクライアントデバイスのメディアプレーヤーで、特定のメディアに対して「いいね」ボタン（Like）を押した場合、クライアントはこのイベントをCICに送信する必要があります。 |
| [`RenderPlayerInfo`](#RenderPlayerInfo)                 | ディレクティブ | CICから、メディアプレーヤーに表示する再生リスト、アルバムの画像、歌詞のような再生メタデータをクライアントに送信し、表示するように指示します。 |
| [`RequestPlayerInfo`](#RequestPlayerInfo)               | イベント     | クライアントから、メディアプレーヤーに表示する再生リスト、アルバムの画像、歌詞のような再生メタデータをCICにリクエストします。 |
| [`SubscribeCommandIssued`](#SubscribeCommandIssued)     | イベント     | ユーザーがクライアントデバイスのメディアプレーヤーでチャンネル登録ボタン（Subscribe）を押した場合、クライアントはこのイベントをCICに送信する必要があります。  |
| [`UnlikeCommandIssued`](#UnlikeCommandIssued)           | イベント     | ユーザーがクライアントデバイスのメディアプレーヤーで、特定のメディアに対して「いいね」を取り消すボタン（Unlike）を押した場合、クライアントはこのイベントをCICに送信する必要があります。 |
| [`UnsubscribeCommandIssued`](#UnsubscribeCommandIssued) | イベント     | ユーザーがクライアントデバイスのメディアプレーヤーでチャンネル登録解除ボタン（Unsubscribe）を押した場合、クライアントはこのイベントをCICに送信する必要があります。 |
| [`UpdateLike`](#UpdateLike)                             | ディレクティブ | クライアントに、メディアプレーヤー上で、特定のコンテンツに対するユーザーの「いいね」（like）の有無によって「いいね」の表示を指定された値に更新するように指示します。  |
| [`UpdateSubscribe`](#UpdateSubscribe)                   | ディレクティブ | クライアントに、メディアプレーヤー上で、特定のコンテンツに対するユーザーのチャンネル登録（subscribe）有無によって、チャンネル登録の表示を指定された値に更新するように指示します。  |

<!-- End of the shared content -->

## ExpectRequestPlayerInfoディレクティブ {#ExpectRequestPlayerInfo}

クライアントに再生リスト、アルバムの画像、歌詞のような再生メタデータをリクエストするように指示します。クライアントはこのディレクティブを受信すると、[`TemplateRuntime.RequestPlayerInfo`](#RequestPlayerInfo)イベントをCICに送信する必要があります。

### Payload fields

なし

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "ExpectRequestPlayerInfo",
      "dialogRequestId": "9d7dc3ca-17ff-4df9-9800-91736ba2a3b6",
      "messageId": "46ccdf6c-609a-43a5-91c4-7a43b961f0c0"
    },
    "payload": {}
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`TemplateRuntime.RequestPlayerInfo`](#RequestPlayerInfo)

## LikeCommandIssuedイベント {#LikeCommandIssued}
ユーザーがクライアントデバイスのメディアプレーヤーで、特定のメディアに対して「いいね」ボタン（Like）を押した場合、クライアントはこのイベントをCICに送信する必要があります。CICはこのイベントを受信すると、適切なディレクティブをクライアントに送信します。


### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`    | string  | メディアコンテンツのトークン。[`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)ディレクティブの`playableItems[].token`フィールドで提供されたトークンが入力される必要があります。 |  |

### 備考
* クライアントデバイスのボタンは、ハードウェア方式の物理ボタンの場合もあり、オーディオプレーヤーウィジェットのボタンのようなソフトウェア方式のボタンの場合もあります。

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
      "namespace": "TemplateRuntime",
      "name": "LikeCommandIssued",
      "messageId": "2fcb6a62-393d-46ad-a5c4-b3db9b640045"
    },
    "payload": {
      "token": "eJyr5lIqSSyITy4tKs4vUrJSUE"
    }
  }
}
```
{% endraw %}

### 次の項目も参照してください。
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.UnlikeCommandIssued`](#UnlikeCommandIssued)
* [`TemplateRuntime.UpdateLike`](#UpdateLike)

<!-- Start of the shared content: TemplateRuntime.RenderPlayerInfo -->

## RenderPlayerInfoディレクティブ {#RenderPlayerInfo}

CICから、メディアプレーヤーに表示する再生リスト、アルバムの画像、歌詞のような再生メタデータをクライアントに送信し、表示するように指示します。ユーザーからオーディオの再生をリクエストされると、クライアントは[`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)ディレクティブを受信してメディアを再生します。ディスプレイを持つクライアントは、必要に応じてメディアプレーヤーに再生関連情報を表示する必要があります。その際、[`TemplateRuntime.RequestPlayerInfo`](#RequestPlayerInfo)イベントで再生メタデータをCICにリクエストし、`TemplateRuntime.RenderPlayerInfo`ディレクティブを受信します。`TemplateRuntime.RenderPlayerInfo`ディレクティブには、現在再生するメディアコンテンツと、後で再生するメディアコンテンツの再生メタデータが含まれます。クライアントは、`TemplateRuntime.RenderPlayerInfo`ディレクティブの再生メタデータをユーザーに提供して、現在再生しているメディアのメタデータおよび再生リストを表示することができます。ユーザーからリスト内の特定のメディアを再生するようにリクエストされたり、「いいね」（[`TemplateRuntime.LikeCommandIssued`](#LikeCommandIssued)）、「いいね」の取り消し（[`TemplateRuntime.UnlikeCommandIssued`](#UnlikeCommandIssued)）のような動作を処理する際、ベースとなるデータ（`token`）を提供します。

### Payload fields
| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `controls[]`                | object array | クライアントがメディアプレーヤーで表示すべきボタンの情報を持つオブジェクト配列です。             |  |
| `controls[].enabled`        | boolean      | `controls[].name`で設定されたボタンを、メディアプレーヤーで有効にするかを示します。<ul><li><code>true</code>：有効にする</li><li><code>false</code>：無効にする</li></ul>  |   |
| `controls[].name`           | string       | ボタンの名前。次のいずれかになります。<ul><li><code>"NEXT"</code>：「次」ボタン</li><li><code>"PLAY_PAUSE"</code>：「再生/一時停止」ボタン</li><li><code>"PREVIOUS"</code>：「前」ボタン</li></ul>  |   |
| `controls[].selected`       | boolean      | メディアコンテンツが選択されているかを示します。この値は、ユーザーの「好き」という概念を表す際に使用することができます。この値が`true`に設定されていたら、ユーザーが好きなアイテムとして登録したコンテンツであることを示します。メディアプレーヤーの関連するUIで、そのことを表す必要があります。<ul><li><code>true</code>：選択済み</li><li><code>false</code>：未選択</li></ul> |   |
| `controls[].type`           | string       | ボタンのタイプ。現在、`"BUTTON"`のみ使用します。  |  |
| `displayType`               | string | メディアコンテンツを表示する形式。<ul><li><code>"list"</code>：リストで表示する</li><li><code>"single"</code>：1つのアイテムを表示する</li></ul>       |  |
| `playableItems[]`           | object array | 再生できるメディアコンテンツのリストを持つオブジェクト配列です。このフィールドは、空の配列の場合があります。  |  |
| `playableItems[].artImageUrl`  | string    | メディアコンテンツ関連画像のURI。アルバムのジャケット画像や関連アイコンなどの画像があるURIです。      | 条件付き |
| `playableItems[].controls[]`                | object array  | 特定のメディアコンテンツを再生するとき、表示すべきボタンの情報を持つオブジェクト配列です。このオブジェクト配列は省略できます。  | 条件付き |
| `playableItems[].controls[].enabled`        | boolean      | `playableItems[].controls[].name`で設定されたボタンを、メディアプレーヤーで有効にするかを示します。<ul><li><code>true</code>：有効にする</li><li><code>false</code>：無効にする</li></ul>  |   |
| `playableItems[].controls[].name`           | string       | ボタンまたはコントロールUIの名前。次のいずれかになります。<ul><li><code>"BACKWARD_15S"</code>：「15秒巻き戻しする」ボタン</li><li><code>"BACKWARD_30S"</code>：「30秒巻き戻しする」ボタン</li><li><code>"BACKWARD_60S"</code>：「60秒巻き戻しする」ボタン</li><li><code>"FORWARD_15S"</code>：「15秒早送りする」ボタン</li><li><code>"FORWARD_30S"</code>：「30秒早送りする」ボタン</li><li><code>"FORWARD_60S"</code>：「60秒早送りする」ボタン</li><li><code>"NEXT"</code>：「次へ」ボタン</li><li><code>"PLAY_PAUSE"</code>：「再生/一時停止」ボタン</li><li><code>"PREVIOUS"</code>：「前へ」ボタン</li><li><code>"PROGRESS_BAR"</code>：進行状況バー</li><li><code>"REPEAT"</code>：「リピート」ボタン</li><li><code>"SUBSCRIBE_UNSUBSCRIBE"</code>：「チャンネル登録/チャンネル登録解除」ボタン</li></ul>  |   |
| `playableItems[].controls[].selected`       | boolean      | メディアコンテンツが選択されているかを示します。この値は、ユーザーの「好き」という概念を表す際に使用することができます。この値が`true`に設定されていたら、ユーザーが好きなアイテムとして登録したコンテンツであることを示します。メディアプレーヤーの関連するUIで、そのことを表す必要があります。<ul><li><code>true</code>：選択済み</li><li><code>false</code>：未選択</li></ul> |   |
| `playableItems[].controls[].type`           | string       | ボタンのタイプ。現在、`"BUTTON"`のみ使用します。  |  |
| `playableItems[].headerText`       | string        | 主に、現在の再生リストのタイトルを表すテキストフィールド                                                | 条件付き  |
| `playableItems[].isLive`           | boolean       | リアルタイムのコンテンツかどうかを示す値。<ul><li><code>true</code>：リアルタイムのコンテンツ</li><li><code>false</code>：リアルタイムのコンテンツではない</li></ul><div class="note"><p><strong>メモ</strong></p><p>リアルタイムのコンテンツは、リアルタイムのコンテンツであることを表すアイコン（例：liveアイコン）を表示する必要があります。</p></div>  | 条件付き  |
| `playableItems[].lyrics[]`         | object array  | 歌詞のデータを持つオブジェクト配列。                                                            | 条件付き  |
| `playableItems[].lyrics[].data`    | string        | 歌詞のデータ。このフィールドと`playableItems[].lyrics[].url`フィールドのうち、1つは存在します。              | 条件付き  |
| `playableItems[].lyrics[].format`  | string        | 歌詞データの形式。<ul><li><code>"LRC"</code>：<a href="https://en.wikipedia.org/wiki/LRC_(file_format)" target="_blank">LRC形式</a></li><li><code>"PLAIN"</code>：テキスト形式</li></ul>  |   |
| `playableItems[].lyrics[].url`     | string        | 歌詞データのURI。このフィールドと`playableItems[].lyrics[].data`フィールドのうち、1つは存在します。        | 条件付き  |
| `playableItems[].showAdultIcon`    | boolean       | 成人向けコンテンツを示すアイコンを表示するかどうか。<ul><li><code>true</code>：表示する。</li><li><code>false</code>：表示しない。</li></ul>   |   |
| `playableItems[].titleSubText1`    | string        | 主にアーティスト名を表すテキストフィールド                                                          |  |
| `playableItems[].titleSubText2`    | string        | 主にアルバム名を表すサブテキストフィールド                                                      | 条件付き |
| `playableItems[].titleText`        | string        | 現在のオーディオコンテンツのタイトルを表すテキストフィールド                                                         |   |
| `playableItems[].token`            | string        | メディアコンテンツのトークン                                                                     |  |
| `provider`                         | object        | メディアコンテンツ提供元の情報を持つオブジェクト                                                         | 条件付き |
| `provider.logoUrl`                 | string        | メディアコンテンツ提供元のロゴ画像のURI。このフィールドまたはフィールドの値がなかったり、ロゴ画像を表示できない場合、`provider.name`フィールド内のメディアコンテンツ提供元の名前を表示する必要があります。 | 条件付き |
| `provider.name`                    | string        | メディアコンテンツ提供元の名前                                                                   |   |
| `provider.smallLogoUrl`            | string        | メディアコンテンツ提供元の小さなロゴ画像のURI                                                | 条件付き |

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "RenderPlayerInfo",
      "dialogRequestId": "34abac3-cb46-611c-5111-47eab87b7",
      "messageId": "ad13f0d6-bb11-ca23-99aa-312a0b213805"
    },
    "payload": {
      "controls": [
        {
          "enabled": true,
          "name": "PLAY_PAUSE",
          "selected": false,
          "type": "BUTTON"
        },
        {
          "enabled": true,
          "name": "NEXT",
          "selected": false,
          "type": "BUTTON"
        },
        {
          "enabled": true,
          "name": "PREVIOUS",
          "selected": false,
          "type": "BUTTON"
        }
      ],
      "displayType": "list",
      "playableItems": [
        {
          "artImageUrl": "https://musicmeta.musicservice.example.com/example/album/662058.jpg",
          "controls": [
            {
              "enabled": true,
              "name": "LIKE_DISLIKE",
              "selected": false,
              "type": "BUTTON"
            }
          ],
          "headerText": "Classic",
          "lyrics": [
            {
              "data": null,
              "format": "PLAIN",
              "url": null
            }
          ],
          "isLive": false,
          "showAdultIcon": false,
          "titleSubText1": "Alice Sara Ott, Symphonie Orchester Des Bayerischen Rundfunks, Esa-Pekka Salonen",
          "titleSubText2": "Wonderland - Edvard Grieg : Piano Concerto, Lyric Pieces",
          "titleText": "Grieg : Piano Concerto In A Minor, Op.16 - 3. Allegro moderato molto e marcato (Live)",
          "token": "eJyr5lIqSSyITy4tKs4vUrJSUE="
        },
        {
          "artImageUrl": "https://musicmeta.musicservice.example.com/example/album/202646.jpg",
          "controls": [
            {
              "enabled": true,
              "name": "LIKE_DISLIKE",
              "selected": false,
              "type": "BUTTON"
            }
          ],
          "headerText": "Classic",
          "lyrics": [
            {
              "data": null,
              "format": "PLAIN",
              "url": null
            }
          ],
          "isLive": true,
          "showAdultIcon": false,
          "titleSubText1": "Berliner Philharmoniker, Herbert Von Karajan",
          "titleSubText2": "Mendelssohn : Violin Concerto; A Midsummer Night`s Dream",
          "titleText": "Symphony No.4 In A Op.90 'Italian' - III.Con Moto Moderato",
          "token": "eJyr5lIqSSyITy4tKs4vUrJSUEo2"
        },
        ...
      ],
      "provider": {
        "logoUrl": "https://img.musicservice.example.net/logo_180125.png",
        "name": "SampleMusicProvider",
        "smallLogoUrl": "https://img.musicservice.example.net/smallLogo_180125.png"
      }
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`TemplateRuntime.LikeCommandIssued`](#LikeCommandIssued)
* [`TemplateRuntime.RequestPlayerInfo`](#RequestPlayerInfo)
* [`TemplateRuntime.SubscribeCommandIssued`](#SubscribeCommandIssued)
* [`TemplateRuntime.UnlikeCommandIssued`](#UnlikeCommandIssued)
* [`TemplateRuntime.UnsubscribeCommandIssued`](#UnsubscribeCommandIssued)

<!-- End of the shared content -->

<!-- Start of the shared content: TemplateRuntime.RequestPlayerInfo -->

## RequestPlayerInfo event {#RequestPlayerInfo}
クライアントから、メディアプレーヤーに表示する再生リスト、アルバムの画像、歌詞のような再生メタデータをCICにリクエストします。CICはこのイベントを受信すると、[`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)ディレクティブをクライアントに送信します。

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `range`        | object  | 再生メタデータの範囲を指定するオブジェクト。このフィールドが使用されていない場合、クライアントは任意の数のメタデータを受信します。   | 任意  |
| `range.after`  | number  | 基準となるメディアコンテンツから、n個以降の再生リストに含まれた再生メタデータをリクエストします。例えば、`range.before`フィールドの値を指定しないで、`range.after`を`5`に設定すると、基準のメディアコンテンツを含めて、合計6つのメディアコンテンツに該当する再生メタデータを受信します。 | 任意  |
| `range.before` | number  | 基準となるメディアコンテンツから、n個前以前の再生リストに含まれた再生メタデータをリクエストします。  | 任意  |
| `token`        | string  | 再生メタデータを読み取るとき、基準となるメディアコンテンツのトークン。[`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)ディレクティブの`playableItems[].token`フィールドで提供されたトークンが入力される必要があります。 |  |

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
      "namespace": "TemplateRuntime",
      "name": "RequestPlayerInfo",
      "messageId": "2fcb6a62-393d-46ad-a5c4-b3db9b640045"
    },
    "payload": {
      "token": "eJyr5lIqSSyITy4tKs4vUrJSUE"
    }
  }
}
```
{% endraw %}

### 次の項目も参照してください。
* [`TemplateRuntime.ExpectRequestPlayerInfo`](#ExpectRequestPlayerInfo)
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)

<!-- End of the shared content -->

## SubscribeCommandIssuedイベント {#SubscribeCommandIssued}
ユーザーがクライアントデバイスのメディアプレーヤーでチャンネル登録ボタン（Subscribe）を押した場合、クライアントはこのイベントをCICに送信する必要があります。CICはこのイベントを受信すると、適切なディレクティブをクライアントに送信します。

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`    | string  | メディアコンテンツのトークン。[`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)ディレクティブの`playableItems[].token`フィールドで提供されたトークンが入力される必要があります。 |  |

### 備考
* クライアントデバイスのボタンは、ハードウェア方式の物理ボタンの場合もあり、オーディオプレーヤーウィジェットのボタンのようなソフトウェア方式のボタンの場合もあります。

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
      "namespace": "TemplateRuntime",
      "name": "SubscribeCommandIssued",
      "messageId": "ec3deb51-2ed8-47ae-ac17-d2ce24370f8f"
    },
    "payload": {
      "token": "SSyITy4teJyr5lIqKs4vUrJSUE"
    }
  }
}
```
{% endraw %}

### 次の項目も参照してください。
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.UnsubscribeCommandIssued`](#UnsubscribeCommandIssued)
* [`TemplateRuntime.UpdateSubscribe`](#UpdateSubscribe)

## UnlikeCommandIssuedイベント {#UnlikeCommandIssued}
ユーザーがクライアントデバイスのメディアプレーヤーで、特定のメディアに対して「いいね」を取り消すボタン（Unlike）を押した場合、クライアントはこのイベントをCICに送信する必要があります。CICはこのイベントを受信すると、適切なディレクティブをクライアントに送信します。


### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`    | string  | メディアコンテンツのトークン。[`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)ディレクティブの`playableItems[].token`フィールドで提供されたトークンが入力される必要があります。 |  |

### 備考
* クライアントデバイスのボタンは、ハードウェア方式の物理ボタンの場合もあり、オーディオプレーヤーウィジェットのボタンのようなソフトウェア方式のボタンの場合もあります。

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
      "namespace": "TemplateRuntime",
      "name": "UnlikeCommandIssued",
      "messageId": "2fcb6a62-393d-46ad-a5c4-b3db9b640045"
    },
    "payload": {
      "token": "eJyr5lIqSSyITy4tKs4vUrJSUE"
    }
  }
}
```
{% endraw %}

### 次の項目も参照してください。
* [`TemplateRuntime.LikeCommandIssued`](#LikeCommandIssued)
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.UpdateLike`](#UpdateLike)

## UnsubscribeCommandIssuedイベント {#UnsubscribeCommandIssued}
ユーザーがクライアントデバイスのメディアプレーヤーでチャンネル登録解除ボタン（Unsubscribe）を押した場合、クライアントはこのイベントをCICに送信する必要があります。CICはこのイベントを受信すると、適切なディレクティブをクライアントに送信します。

### Context fields

{% include "/Develop/References/CICInterface/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `token`    | string  | メディアコンテンツのトークン。[`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)ディレクティブの`playableItems[].token`フィールドで提供されたトークンが入力される必要があります。 |  |

### 備考
* クライアントデバイスのボタンは、ハードウェア方式の物理ボタンの場合もあり、オーディオプレーヤーウィジェットのボタンのようなソフトウェア方式のボタンの場合もあります。

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
      "namespace": "TemplateRuntime",
      "name": "UnsubscribeCommandIssued",
      "messageId": "04deb09e-54cc-4525-9e97-4ff4168872b5"
    },
    "payload": {
      "token": "r5lIqKs4vUSSyITy4teJyrJSUE"
    }
  }
}
```
{% endraw %}

### 次の項目も参照してください。
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.SubscribeCommandIssued`](#SubscribeCommandIssued)
* [`TemplateRuntime.UpdateSubscribe`](#UpdateSubscribe)

## UpdateLikeディレクティブ {#UpdateLike}

クライアントに、メディアプレーヤー上で、特定のコンテンツに対するユーザーの「いいね」（like）の有無によって「いいね」の表示を指定された値に更新するように指示します。ユーザーの発話や「いいね」ボタン（[`TemplateRuntime.LikeCommandIssued`](#LikeCommandIssued)）、「いいね」を取り消すボタン（[`TemplateRuntime.UnlikeCommandIssued`](#UnlikeCommandIssued)）を押すアクションに対するレスポンスとして、またはユーザーが他のデバイスで「いいね」に関連するアクションを行った場合、そのことを同期するために送信されます。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `like`        | boolean | 「いいね」を表示するかどうか。<ul><li><code>true</code>：「いいね」を表示する</li><li><code>false</code>：「いいね」を表示しない</li></ul>             |  |
| `token`       | string  | メディアコンテンツのトークン。[`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)ディレクティブの`playableItems[].token`フィールドに該当する値が含まれます。      |  |

## 備考

同期のため、このディレクティブはイベントに対する応答ではなく、[Downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)で送信されます。

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "UpdateLike",
      "messageId": "fb65457e-bd2e-4876-b739-2b8cc5b67486",
      "dialogRequestId": "94e045dd-78c7-415e-b73b-f0cb9cbfd75d"
    },
    "payload": {
      "token": "r5lIqKs4vUSSyITy4teJyrJSUE",
      "like": true
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`TemplateRuntime.LikeCommandIssued`](#LikeCommandIssued)
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.UnlikeCommandIssued`](#UnlikeCommandIssued)

## UpdateSubscribeディレクティブ {#UpdateSubscribe}

クライアントに、メディアプレーヤー上で、特定のコンテンツに対するユーザーのチャンネル登録有無によってチャンネル登録（subscribe）の表示を指定された値に更新するように指示します。ユーザーの発話やチャンネル登録ボタン（[`TemplateRuntime.SubscribeCommandIssued`](#SubscribeCommandIssued)）、チャンネル登録解除ボタン（[`TemplateRuntime.UnsubscribeCommandIssued`](#UnsubscribeCommandIssued)）を押すアクションに対するレスポンスとして、またはユーザーが他のデバイスでチャンネル登録に関連するアクションを行った場合、そのことを同期するために送信されます。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `subscribe`   | boolean | チャンネル登録しているかどうか。<ul><li><code>true</code>：チャンネル登録している</li><li><code>false</code>：チャンネル登録していない</li></ul>             |  |
| `token`       | string  | メディアコンテンツのトークン。[`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)ディレクティブの`playableItems[].token`フィールドに該当する値が含まれます。      |  |

## 備考

同期のため、このディレクティブはイベントに対する応答ではなく、[Downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)で送信されます。

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "TemplateRuntime",
      "name": "UpdateSubscribe",
      "messageId": "fb65457e-bd2e-4876-b739-2b8cc5b67486",
      "dialogRequestId": "94e045dd-78c7-415e-b73b-f0cb9cbfd75d"
    },
    "payload": {
      "token": "r5lIqKs4vUSSyITy4teJyrJSUE",
      "subscribe": true
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`TemplateRuntime.SubscribeCommandIssued`](#SubscribeCommandIssued)
* [`TemplateRuntime.RenderPlayerInfo`](#RenderPlayerInfo)
* [`TemplateRuntime.UnsubscribeCommandIssued`](#UnsubscribeCommandIssued)
