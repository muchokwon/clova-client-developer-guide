<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

# AudioPlayer

<!-- Start of the shared content: CICAPIforAudioPlayback -->
AudioPlayerインターフェースは、クライアントからオーディオストリームの再生をリクエストするか、またはオーディオストリームの再生中に発生するイベントをCICにレポートする際に使用される名前空間です。AudioPlayerが提供するイベントとディレクティブは、次の通りです。

| メッセージ         | タイプ  | 説明                                   |
|------------------|---------|---------------------------------------------|
| [`ClearQueue`](#ClearQueue)           | ディレクティブ | クライアントに対して、オーディオストリームの再生キューをクリアするように指示します。                              |
| [`ExpectReportPlaybackState`](#ExpectReportPlaybackState) | ディレクティブ | クライアントに対して、現在の再生状態をレポートするように指示します。クライアントはこのディレクティブを受信すると、[`AudioPlayer.ReportPlaybackState`](#ReportPlaybackState)イベントをCICに送信する必要があります。 |
| [`Play`](#Play)                       | ディレクティブ | クライアントに対して、特定のオーディオストリームを再生するか、または再生キューに追加するように指示します。                         |
| [`PlaybackQueueCleared`](#PlaybackQueueCleared) | イベント   | クライアントがCICから[`AudioPlayer.ClearQueue`](#ClearQueue)ディレクティブを受信した場合、再生キューをクリアし、`PlaybackQueueCleared`を送信する必要があります。       |
| [`PlayFinished`](#PlayFinished)       | イベント     | クライアントがオーディオストリームの再生を終了するとき、そのオーディオストリームの情報をCICにレポートするために使用します。     |
| [`PlayPaused`](#PlayPaused)           | イベント     | クライアントがオーディオストリームの再生を一時停止するとき、そのオーディオストリームの情報をCICにレポートするために使用します。 |
| [`PlayResumed`](#PlayResumed)         | イベント     | クライアントがオーディオストリームの再生を再開するとき、そのオーディオストリームの情報をCICにレポートするために使用します。         |
| [`PlayStarted`](#PlayStarted)         | イベント     | クライアントがオーディオストリームの再生を開始するとき、そのオーディオストリームの情報をCICにレポートするために使用します。    |
| [`PlayStopped`](#PlayStopped)         | イベント     | クライアントがオーディオストリームの再生を停止するとき、そのオーディオストリームの情報をCICにレポートするために使用します。    |
| [`ProgressReportDelayPassed`](#ProgressReportPositionPassed) | イベント | オーディオストリームの再生が開始してから、指定された遅延期間が経過したタイミングの再生状態（[`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)）をCICにレポートするために使用します。オーディオストリームの遅延期間は、[`AudioPlayer.Play`](#Play)ディレクティブがクライアントに送信されるときに確認できます。 |
| [`ProgressReportIntervalPassed`](#ProgressReportPositionPassed)| イベント | オーディオストリームの再生が開始してから、指定された間隔ごとの再生状態（[`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)）を、CICにレポートします。レポートする間隔は、[`AudioPlayer.Play`](#Play)ディレクティブがクライアントに送信されるときに確認できます。|
| [`ProgressReportPositionPassed`](#ProgressReportPositionPassed) | イベント | オーディオストリームの再生が開始してから、指定されたタイミングに、そのときの再生状態（[`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)）をCICにレポートします。レポートするタイミングは、[`AudioPlayer.Play`](#Play)ディレクティブがクライアントに送信されるときに確認できます。|
| [`ReportPlaybackState`](#ReportPlaybackState)           | イベント  | クライアントから、現在のストリーム再生状態をCICにレポートします。クライアントがCICから[`AudioPlayer.ExpectReportPlaybackState`](#ExpectReportPlaybackState)ディレクティブを受信した場合、`AudioPlayer.ReportPlaybackState`イベントをCICに送信する必要があります。  |
{% if book.DocMeta.TargetReaderType == "Internal" -%}
| [`RequestPlaybackState`](#RequestPlaybackState)         | イベント  | クライアントのストリーム再生状態をCICにリクエストします。CICは、`AudioPlayer.RequestPlaybackState`イベントを受信した場合、ユーザーアカウントに登録されているすべての、または特定のクライアントに[`ExpectReportPlaybackState`](#ExpectReportPlaybackState)ディレクティブを送信します。  |
| [`StreamDeliver`](#StreamDeliver)     | ディレクティブ | [`AudioPlayer.StreamRequested`](#StreamRequested)イベントに対する応答です。実際に再生できるオーディオストリームの情報を受信するために使用します。 |
{% endif -%}
| [`StreamRequested`](#StreamRequested) | イベント     | オーディオストリームを再生するために、ストリーミングのURLなどの追加情報をCICにリクエストするイベントです。               |
{% if book.DocMeta.TargetReaderType == "Internal" -%}
| [`SynchronizePlaybackState`](#SynchronizePlaybackState) | ディレクティブ | クライアントのストリーム再生状態を同期するように指示します。`AudioPlayer.RequestPlaybackState`イベントを送信したクライアントは、`AudioPlayer.SynchronizePlaybackState`ディレクティブを受信します。 |
{% endif -%}

<!-- End of the shared content -->

## ClearQueueディレクティブ {#ClearQueue}
クライアントに対して、オーディオストリームの再生キューをクリアするように指示します。このディレクティブの`clearBehavior`フィールドの値は、キューをクリアする動作を区分します。クライアントがキューをクリアし、それと同時に現在再生中のオーディオストリームを中止するかを指定します。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `clearBehavior`           | string | クリアする動作を指定するフィールド<ul><li><code>"CLEAR_ALL"</code>：再生キューをすべてクリアして、現在再生中のオーディオストリームを中止します。</li><li><code>"CLEAR_ENQUEUED"</code>：再生キューをクリアして、現在のオーディオストリームの再生を続けます。</li></ul> |  |

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "ClearQueue",
      "dialogRequestId": "8b81296d-218e-4a08-897a-bee51daad907",
      "messageId": "823a703d-9447-438a-bad5-21fa7a62b623"
    },
    "payload": {
      "clearBehavior": "CLEAR_ALL"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlaybackQueueCleared`](#PlaybackQueueCleared)
* [`AudioPlayer.PlayStarted`](#PlayStarted)
* [`AudioPlayer.PlayStopped`](#PlayStopped)

## ExpectReportPlaybackStateディレクティブ {#ExpectReportPlaybackState}

クライアントに対して、現在の再生状態をレポートするように指示します。クライアントはこのディレクティブを受信すると、[`AudioPlayer.ReportPlaybackState`](#ReportPlaybackState)イベントをCICに送信する必要があります。

### Payload fields

なし

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "ExpectReportPlaybackState",
      "dialogRequestId": "9d7dc3ca-17ff-4df9-9800-91736ba2a3b6",
      "messageId": "46ccdf6c-609a-43a5-91c4-7a43b961f0c0"
    },
    "payload": {}
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.ReportPlaybackState`](#ReportPlaybackState)
* [オーディオ再生状態を共有する](/Develop/Guides/Handle_Audio_Playback.md#ShareAudioPlaybackState)

<!-- Start of the shared content: AudioPlayer.Play -->

## Playディレクティブ {#Play}
クライアントに対して、特定のオーディオストリームを再生するか、または再生キューに追加するように指示します。

### Payload fields
| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `audioItem`               | object | 再生するオーディオストリームのメタデータと、再生に必要なオーディオストリームの情報を持つオブジェクト                     |  |
| `audioItem.artImageUrl`   | string | オーディオコンテンツに関連する画像（アルバムの画像）のURI                                                  | 条件付き  |
| `audioItem.audioItemId`   | string | オーディオストリームを区別するID。クライアントはこの値に基づいて、重複するPlayディレクティブを削除できます。 |  |
| `audioItem.headerText`    | string | 主に、現在の再生リストのタイトルを表すテキストフィールド                                                | 条件付き  |
| `audioItem.stream`        | [AudioStreamInfoObject](#AudioStreamInfoObject) | 再生に必要なオーディオストリームの情報を持つオブジェクト        |  |
| `audioItem.titleSubText1` | string | 主にアーティスト名を表すテキストフィールド                                                          |  |
| `audioItem.titleSubText2` | string | 主にアルバム名を表すサブテキストフィールド                                                      | 条件付き |
| `audioItem.titleText`     | string | 現在のオーディオコンテンツのタイトルを表すテキストフィールド                                                         |   |
| `audioItem.type`          | string | **（Deprecated）** ストリーミングサービスを示すフィールド。オーディオのストリーミングサービスを提供するプロバイダー、またはサービス名です。このフィールドの値は、サービスによって異なるaudioItemオブジェクトのフィールドを確認し、それを解析するためのパーサー（parser）を選択する際に使用できます。 |  |
| `playBehavior`            | string | ディレクティブに含まれたオーディオストリームを、クライアントでいつ再生するかを指定するフィールド<ul><li><code>"REPLACE_ALL"</code>：再生キューをすべてクリアして、送信されたオーディオストリームをすぐに再生します。</li><li><code>"ENQUEUE"</code>：再生キューに、送信されたオーディオストリームを追加します。</li></ul> |  |
| `source`                  | object | オーディオストリーミングサービスの提供元                                                    |  |
| `source.logoUrl`          | string | オーディオストリーミングサービスのロゴ画像のURIこのフィールドまたはフィールドの値がなかったり、ロゴ画像を表示できない場合、`source.name`フィールド内のオーディオストリーミングサービスの名前を表示する必要があります。  | 条件付き |
| `source.name`             | string | オーディオストリーミングサービスの名前                                                        |  |

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p>直観的なユーザーのエクスペリエンスのために、なるべく`source.name`より`source.logoUrl`を使用することをお勧めします。</p>
</div>

### 備考
ストリーミングサービスの課金などの理由で、実際のストリーミング情報、つまりストリーミングのURIなどの情報を再生する直前に取得する必要があります。`audioItem.stream.urlPlayable`フィールドの値によって、次のように区分されます。
* `urlPlayable`フィールドの値が`true`の場合、`audioItem.stream.url`フィールドに含まれたURIでオーディオストリームをすぐに再生できます。
* `urlPlayable`フィールドの値が`false`の場合、`audioItem.stream.url`フィールドに含まれたURIではオーディオストリームをすぐに再生できず、[`AudioPlayer.StreamRequested`](#StreamRequested)イベントでオーディオストリームの情報を追加でリクエストする必要があります。

### Message example
{% raw %}

```json
// すぐ再生できるオーディオストリームのURIが含まれたサンプル
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "Play",
      "dialogRequestId": "34abac3-cb46-611c-5111-47eab87b7",
      "messageId": "ad13f0d6-bb11-ca23-99aa-312a0b213805"
    },
    "payload": {
      "audioItem": {
        "audioItemId": "90b77646-93ab-444f-acd9-60f9f278ca38",
        "episodeId": 22346122,
        "stream": {
          "beginAtInMilliseconds": 419704,
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": 60000,
            "progressReportPositionInMilliseconds": null
          },
          "token": "eyJ1cmwiOiJodHRwczovL2FwaS1leC5wb2RiYmFuZy5jb20vY2xvdmEvZmlsZS8xMjU0OC8yMjYxODcwMSIsInBsYXlUeXBlIjoiTk9ORSIsInBvZGNhc3RJZCI6MTI1NDgsImVwaXNvZGVJZCI6MjI2MTg3MDF9",
          "url": "https://streaming.example.com/clova/file/12548/22618701",
          "urlPlayable": true
        },
        "type": "podcast"
      },
      "source": {
        "name": "Potbbang",
        "logoUrl": "https://img.musicservice.example.net/logo_180125.png"
      },
      "playBehavior": "REPLACE_ALL"
    }
  }
}

// すぐ再生できないオーディオストリームのURIが含まれたサンプル
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "Play",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "audioItem": {
        "audioItemId": "9CPWU-8362fe7c-f75c-42c6-806b-6f3e00aba8f1-c1862201",
        "album": {
          "albumId": "2000240",
          "genres": [
            "Classic"
          ],
          "title": "Wonderland - Edvard Grieg : Piano Concerto, Lyric Pieces"
        },
        ...
        "stream": {
          "beginAtInMilliseconds": 0,
          "durationInMilliseconds": 60000,
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": null,
            "progressReportPositionInMilliseconds": 60000
          },
          "token": "TR-NM-17716562",
          "url": "clova:TR-NM-17716562",
          "urlPlayable": false
        },
        "title": "Symphony No.4 In A Op.90 'Italian' - III.Con Moto Moderato",
        "type": "SampleMusicProvider"
      },
      "source": {
        "name": "Sample Music Provider",
        "logoUrl": "https://img.musicservice.example.net/logo_180125.png"
      },
      "playBehavior": "REPLACE_ALL"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.PlayPaused`](#PlayPaused)
* [`AudioPlayer.PlayResumed`](#PlayResumed)
* [`AudioPlayer.PlayStarted`](#PlayStarted)
* [`AudioPlayer.PlayStopped`](#PlayStopped)
* [`AudioPlayer.ProgressReportDelayPassed`](#ProgressReportDelayPassed)
* [`AudioPlayer.ProgressReportIntervalPassed`](#ProgressReportIntervalPassed)
* [`AudioPlayer.ProgressReportPositionPassed`](#ProgressReportPositionPassed)
* [`AudioPlayer.StreamRequested`](#StreamRequested)
* [オーディオを再生する](/Develop/Guides/Handle_Audio_Playback.md#PlayAudioStream)

<!-- End of the shared content -->

## PlaybackQueueClearedイベント {#PlaybackQueueCleared}
クライアントがCICから[`AudioPlayer.ClearQueue`](#ClearQueue)ディレクティブを受信した場合、再生キューをクリアし、`PlaybackQueueCleared`を送信する必要があります。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `clearBehavior` | string | [`AudioPlayer.ClearQueue`](#ClearQueue)ディレクティブの`clearBehavior`フィールドの値。クリアするかどうかを示すフィールドです。このフィールドの内容に応じて処理してから、フィールドに値を設定して送信する必要があります。<ul><li><code>"CLEAR_ALL"</code>：再生キューをクリアし、現在再生中のオーディオストリームを中止します。</li><li><code>"CLEAR_ENQUEUED"</code>：再生キューをクリアし、現在再生中のオーディオストリームを続けて再生します。</li></ul> |  |


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
      "namespace": "AudioPlayer",
      "name": "PlaybackQueueCleared",
      "messageId": "1e1d8d52-9b51-454c-9fac-7597dc5f5246"
    },
    "payload": {
        "clearBehavior": "CLEAR_ALL"
    }
}
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.ClearQueue`](#ClearQueue)

<!-- Start of the shared content: AudioPlayer.PlayFinished -->

## PlayFinishedイベント {#PlayFinished}
クライアントがオーディオストリームの再生を終了するとき、そのオーディオストリームの情報をCICにレポートするために使用します。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 再生しているストリームの現在のオフセット。ミリ秒単位です。                         |   |
| `token`                | string | [`AudioPlayer.Play`](#Play)ディレクティブの`audioItem.stream.token`フィールドの値 |  |

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
      "namespace": "AudioPlayer",
      "name": "PlayFinished",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 183000
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [オーディオ再生の進行状況をレポートする](/Develop/Guides/Handle_Audio_Playback.md#ReportAudioPlaybackProgress)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlayPaused -->

## PlayPausedイベント {#PlayPaused}
クライアントがオーディオストリームの再生を一時停止するとき、そのオーディオストリームの情報をCICにレポートするために使用します。このイベントを送信するには、次のような事前のシナリオが必要です。

1. クライアントは、[`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)イベントで、オーディオストリームの再生を一時停止するようにリクエストするユーザーの音声をCICに送信します。
2. CICは、Clovaプラットフォームで認識された一時停止のリクエストを、[`PlaybackController.Pause`](/Develop/References/MessageInterfaces/PlaybackController.md#Pause)ディレクティブでクライアントに送信します。
3. クライアントはオーディオストリームの再生を一時停止し、PlayPausedイベントをCICに送信します。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 再生しているストリームの現在のオフセット。ミリ秒単位です。                         |   |
| `token`                | string | [`AudioPlayer.Play`](#Play)ディレクティブの`audioItem.stream.token`フィールドの値 |  |

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
      "namespace": "AudioPlayer",
      "name": "PlayPaused",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 83100
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayResumed`](#PlayResumed)
* [`PlaybackController.Pause`](/Develop/References/MessageInterfaces/PlaybackController.md#Pause)
* [オーディオ再生を制御する](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlayResumed -->

## PlayResumedイベント {#PlayResumed}

クライアントがオーディオストリームの再生を再開するとき、そのオーディオストリームの情報をCICにレポートするために使用します。このイベントを送信するには、次のような事前のシナリオが必要です。

1. クライアントは、[`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)イベントで、オーディオストリームの再生を再開するようにリクエストするユーザーの音声をCICに送信します。
2. CICは、Clovaプラットフォームで認識された再生再開のリクエストを、[`PlaybackController.Resume`](/Develop/References/MessageInterfaces/PlaybackController.md#Resume)ディレクティブでクライアントに送信します。
3. クライアントは、オーディオストリームの再生を再開し、PlayResumedイベントをCICに送信します。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 再生しているストリームの現在のオフセット。ミリ秒単位です。                         |   |
| `token`                | string | [`AudioPlayer.Play`](#Play)ディレクティブの`audioItem.stream.token`フィールドの値 |  |

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
      "namespace": "AudioPlayer",
      "name": "PlayResumed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 83100
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayPaused`](#PlayPaused)
* [`PlaybackController.Resume`](/Develop/References/MessageInterfaces/PlaybackController.md#Resume)
* [オーディオ再生を制御する](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlayStarted -->

## PlayStartedイベント {#PlayStarted}
クライアントがオーディオストリームの再生を開始するとき、そのオーディオストリームの情報をCICにレポートするために使用します。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 再生しているストリームの現在のオフセット。ミリ秒単位です。                         |   |
| `token`                | string | [`AudioPlayer.Play`](#Play)ディレクティブの`audioItem.stream.token`フィールドの値 |  |

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
      "namespace": "AudioPlayer",
      "name": "PlayStarted",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 0
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayStopped`](#PlayStopped)
* [オーディオを再生する](/Develop/Guides/Handle_Audio_Playback.md#PlayAudioStream)
* [オーディオ再生を制御する](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.PlayStopped -->

## PlayStoppedイベント {#PlayStopped}
クライアントがオーディオストリームの再生を停止するとき、そのオーディオストリームの情報をCICにレポートするために使用します。このイベントを送信するには、次のような事前のシナリオが必要です。

1. クライアントは[`SpeechRecognizer.Recognize`](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)イベントで、オーディオストリームの再生を停止するようにリクエストするユーザーの音声をCICに送信します。
2. CICは、Clovaプラットフォームで認識された停止のリクエストを、[`PlaybackController.Stop`](/Develop/References/MessageInterfaces/PlaybackController.md#Stop)ディレクティブでクライアントに送信します。
3. クライアントはオーディオストリームの再生を停止し、PlayStoppedイベントをCICに送信します。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 再生しているストリームの現在のオフセット。ミリ秒単位です。                         |   |
| `token`                | string | [`AudioPlayer.Play`](#Play)ディレクティブの`audioItem.stream.token`フィールドの値 |  |

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
      "namespace": "AudioPlayer",
      "name": "PlayStopped",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 83100
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayStarted`](#PlayStarted)
* [`PlaybackController.Stop`](/Develop/References/MessageInterfaces/PlaybackController.md#Stop)
* [オーディオ再生を制御する](/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.ProgressReportDelayPassed -->

## ProgressReportDelayPassedイベント {#ProgressReportDelayPassed}
オーディオストリームの再生が開始してから、指定された遅延期間が経過したタイミングの再生状態（[`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)）をCICにレポートするために使用します。オーディオストリームの遅延期間は、[`AudioPlayer.Play`](#Play)ディレクティブがクライアントに送信されるときに確認できます。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 再生しているストリームの現在のオフセット。ミリ秒単位です。                         |   |
| `token`                | string | [`AudioPlayer.Play`](#Play)ディレクティブの`audioItem.stream.token`フィールドの値 |  |

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
      "namespace": "AudioPlayer",
      "name": "ProgressReportDelayPassed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 60000
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.ProgressReportIntervalPassed`](#ProgressReportIntervalPassed)
* [`AudioPlayer.ProgressReportPositionPassed`](#ProgressReportPositionPassed)
* [オーディオ再生の進行状況をレポートする](/Develop/Guides/Handle_Audio_Playback.md#ReportAudioPlaybackProgress)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.ProgressReportIntervalPassed -->

## ProgressReportIntervalPassedイベント {#ProgressReportIntervalPassed}
オーディオストリームの再生が開始してから、指定された間隔ごとの再生状態（[`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)）を、CICにレポートします。レポートする間隔は、[`AudioPlayer.Play`](#Play)ディレクティブがクライアントに送信されるときに確認できます。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 再生しているストリームの現在のオフセット。ミリ秒単位です。                         |   |
| `token`                | string | [`AudioPlayer.Play`](#Play)ディレクティブの`audioItem.stream.token`フィールドの値 |  |

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
      "namespace": "AudioPlayer",
      "name": "ProgressReportIntervalPassed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 120000
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.ProgressReportDelayPassed`](#ProgressReportDelayPassed)
* [`AudioPlayer.ProgressReportPositionPassed`](#ProgressReportPositionPassed)
* [オーディオ再生の進行状況をレポートする](/Develop/Guides/Handle_Audio_Playback.md#ReportAudioPlaybackProgress)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.ProgressReportPositionPassed -->

## ProgressReportPositionPassedイベント {#ProgressReportPositionPassed}
オーディオストリームの再生が開始してから、指定されたタイミングに、そのときの再生状態（[`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)）をCICにレポートします。レポートするタイミングは、[`AudioPlayer.Play`](#Play)ディレクティブがクライアントに送信されるときに確認できます。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 再生しているストリームの現在のオフセット。ミリ秒単位です。                         |   |
| `token`                | string | [`AudioPlayer.Play`](#Play)ディレクティブの`audioItem.stream.token`フィールドの値 |  |

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
      "namespace": "AudioPlayer",
      "name": "ProgressReportPositionPassed",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "token": "TR-NM-4435786",
      "offsetInMilliseconds": 150000
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.ProgressReportDelayPassed`](#ProgressReportDelayPassed)
* [`AudioPlayer.ProgressReportIntervalPassed`](#ProgressReportIntervalPassed)
* [オーディオ再生の進行状況をレポートする](/Develop/Guides/Handle_Audio_Playback.md#ReportAudioPlaybackProgress)

<!-- End of the shared content -->

## ReportPlaybackStateイベント {#ReportPlaybackState}

クライアントから、現在のストリーム再生状態をCICにレポートします。クライアントがCICから[`AudioPlayer.ExpectReportPlaybackState`](#ExpectReportPlaybackState)ディレクティブを受信した場合、`AudioPlayer.ReportPlaybackState`イベントをCICに送信する必要があります。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields
| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds`   | number  | 再生しているストリームの現在のオフセット。ミリ秒単位です。  | 任意  |
| `playerActivity`         | string  | AudioPlayerの現在の状態。<ul><li><code>"IDLE"</code>：アイドル状態</li><li><code>"PLAYING"</code>：再生中</li><li><code>"PAUSED"</code>：一時停止</li><li><code>"STOPPED"</code>：停止状態</li></ul>  |   |
| `repeatMode`             | string  | リピート再生モード<ul><li><code>"NONE"</code>：リピート再生しない</li><li><code>"REPEAT_ONE"</code>：一曲リピート再生</li></ul>  |   |
| `stream`                 | [AudioStreamInfoObject](#AudioStreamInfoObject) | Playディレクティブの`audioItem.stream`                                     | 任意 |
| `token`                  | string  | [`AudioPlayer.Play`](#Play)ディレクティブの`audioItem.stream.token`フィールドの値                                          | 任意 |
| `totalInMilliseconds`    | number | 最近再生したメディアアイテムの全長。[`AudioPlayer.Play`](/Develop/References/MessageInterfaces/AudioPlayer.md#Play)ディレクティブで送信されたオーディオストリームの情報（[AudioStreamInfoObject](/Develop/References/MessageInterfaces/AudioPlayer.md#AudioStreamInfoObject)）に`durationInMilliseconds`フィールド値がある場合、このフィールドに入力します。ミリ秒単位で、`playerActivity`の値が`"IDLE"`の場合、このフィールドの値を入力する必要はありません。 | 任意 |

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
      "namespace": "AudioPlayer",
      "name": "ReportPlaybackState",
      "messageId": "3e57f11a-a02d-4b6f-b183-fdfb6105c650"
    },
    "payload": {
      "playerActivity": "IDLE",
      "repeatMode": "NONE"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.ExpectReportPlaybackState`](#ExpectReportPlaybackState)
* [`AudioPlayer.Play`](#Play)
* [オーディオ再生状態を共有する](/Develop/Guides/Handle_Audio_Playback.md#ShareAudioPlaybackState)

{% if book.DocMeta.TargetReaderType == "Internal" %}
## RequestPlaybackStateイベント {#RequestPlaybackState}

クライアントのストリーム再生状態をCICにリクエストします。CICは、`AudioPlayer.RequestPlaybackState`イベントを受信した場合、ユーザーアカウントに登録されているすべての、または特定のクライアントに[`AudioPlayer.ExpectReportPlaybackState`](#ExpectReportPlaybackState)ディレクティブを送信します。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields
| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | 対象クライアントのID。任意の形式の識別子（不透明なID）です。このフィールドが省略されると、ユーザーアカウントに登録されているすべてのクライアントにリクエストが送信されます。 | 任意 |

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
      "namespace": "AudioPlayer",
      "name": "RequestPlaybackState",
      "messageId": "15323cb4-859f-4d5e-b7f4-a7b60ff41866"
    },
    "payload": {
      "deviceId": "5a849a66-c182-4c1b-8a97-b63cac2d396c"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.ExpectReportPlaybackState`](#ExpectReportPlaybackState)
* [`AudioPlayer.Play`](#Play)
* [オーディオ再生状態を共有する](/Develop/Guides/Handle_Audio_Playback.md#ShareAudioPlaybackState)
{% endif %}

<!-- Start of the shared content: AudioPlayer.StreamDeliver -->

## StreamDeliverディレクティブ {#StreamDeliver}
[`AudioPlayer.StreamRequested`](#StreamRequested)イベントに対する応答です。実際に再生できるオーディオストリームの情報を受信するために使用します。クライアントがコンテンツを再生できるように、オーディオストリームの情報にはストリーミングURIが必ず含まれます。

### Payload fields
| フィールド名 | データ型 | 説明 | 任意 |
|---------|------|--------|:---------:|
| `audioItemId` | string | オーディオストリームの情報を区別するための値。クライアントはこの値に基づいて、重複するPlayディレクティブを削除できます。 |  |
| `audioStream` | [AudioStreamInfoObject](#AudioStreamInfoObject) | 再生に必要なオーディオストリームの情報を持つオブジェクト       |  |

### 備考
`StreamDeliver`ディレクティブで送信される`AudioStreamInfoObject`オブジェクトは、既存の[`AudioPlayer.Play`](#Play)ディレクティブで送信された`AudioStreamInfoObject`オブジェクトと重複しないように、一部の内容が省略されることがあります。ストリームを再生する際、`StreamDeliver`ディレクティブと、すでに受信した[`Play`](#Play)ディレクティブの`payload.audioStream`情報を組み合わせて使用する必要があります。

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "StreamDeliver",
      "dialogRequestId": "277b40c3-b046-4f61-a551-783b1547e7b7",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {
      "audioItemId": "9CPWU-8362fe7c-f75c-42c6-806b-6f3e00aba8f1-c1862201",
      "audioStream": {
          "format": "audio/mpeg",
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": null,
            "progressReportPositionInMilliseconds": 60000
          },
          "token": "TR-NM-17716562",
          "url": "https://musicservice.example.net/b767313e.mp3",
          "urlPlayable": true
      }
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.StreamRequested`](#StreamRequested)
* [オーディオを再生する](/Develop/Guides/Handle_Audio_Playback.md#PlayAudioStream)

<!-- End of the shared content -->

<!-- Start of the shared content: AudioPlayer.StreamRequested -->

## StreamRequestedイベント {#StreamRequested}
オーディオストリームを再生するために、ストリーミングのURIなど、追加の情報をCICにリクエストするイベントです。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields
| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `audioItemId`   | string  | [`AudioPlayer.Play`](#Play)ディレクティブの`audioItem.audioItemId`フィールドの値       |  |
| `audioStream`   | [AudioStreamInfoObject](#AudioStreamInfoObject) | Playディレクティブの`audioItem.stream` |  |

### 備考
ストリーミングサービスの課金などの理由により、ときには、実際のオーディオストリームの情報の発行を、再生直前に遅延させる必要が生じます。このイベントは、そのようにオーディオストリームの情報を前もって用意してはいけない場合のために設計されたAPIです。クライアントは、このイベントを再生直前より先に送信しないようにする必要があります。

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
      "namespace": "AudioPlayer",
      "name": "StreamRequested",
      "messageId": "198cf12-4020-b98a-b73b-1234ab312806"
    },
    "payload": {
      "audioItemId": "9CPWU-8362fe7c-f75c-42c6-806b-6f3e00aba8f1-c1862201",
      "audioStream": {
        "beginAtInMilliseconds": 0,
        "durationInMilliseconds": 60000,
        "progressReport": {
          "progressReportDelayInMilliseconds": null,
          "progressReportIntervalInMilliseconds": null,
          "progressReportPositionInMilliseconds": 60000
        },
        "token": "TR-NM-17716562",
        "url": "clova:TR-NM-17716562",
        "urlPlayable": false
      }
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.StreamDeliver`](#StreamDeliver)
* [オーディオを再生する](/Develop/Guides/Handle_Audio_Playback.md#PlayAudioStream)

<!-- End of the shared content -->

{% if book.DocMeta.TargetReaderType == "Internal" %}
## SynchronizePlaybackStateディレクティブ {#SynchronizePlaybackState}

クライアントのストリーム再生状態を同期するように指示します。`AudioPlayer.RequestPlaybackState`イベントを送信したクライアントは、`AudioPlayer.SynchronizePlaybackState`ディレクティブを受信します。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `deviceId`    | string  | 対象クライアントのID。任意の形式の識別子（不透明なID）です。  |   |
| `event`       | string  | クライアントで発生したイベント。<ul><li><code>PlayFinished</code></li><li><code>PlayPaused</code></li><li><code>PlayResumed</code></li><li><code>PlayStarted</code></li><li><code>PlayStopped</code></li></ul>  | 条件付き  |
| `playbackState`          | object  | クライアントの再生状態の情報を持つオブジェクト             | 条件付き  |
| `playbackState.offsetInMilliseconds`   | number  | 再生しているストリームの現在のオフセット。ミリ秒単位で、`playerActivity`の値が`"IDLE"`の場合、このフィールドは含まれません。  | 条件付き  |
| `playbackState.playerActivity`         | string  | AudioPlayerの現在の状態。<ul><li><code>"IDLE"</code>：アイドル状態</li><li><code>"PLAYING"</code>：再生中</li><li><code>"PAUSED"</code>：一時停止</li><li><code>"STOPPED"</code>：停止状態</li></ul>  |   |
| `playbackState.repeatMode`             | string  | 設定されているリピート再生モード。<ul><li><code>"NONE"</code>：リピート再生しない</li><li><code>"REPEAT_ONE"</code>：一曲リピート再生</li></ul>  |   |
| `playbackState.stream`                 | [AudioStreamInfoObject](#AudioStreamInfoObject) | オーディオストリームに関連する情報を持つオブジェクト。`playerActivity`の値が`"IDLE"`の場合、このフィールドは含まれません。  | 条件付き |
| `playbackState.token`                  | string  | ストリームの再生を識別するトークン。`playerActivity`の値が`"IDLE"`の場合、このフィールドは含まれません。                                | 条件付き |
| `playbackState.totalInMilliseconds`    | number | 最近再生したメディアの全長ミリ秒単位で、`playerActivity`の値が`"IDLE"`の場合、このフィールドは含まれません。                  | 条件付き |

### Message example
{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "SynchronizePlaybackState",
      "dialogRequestId": "9d7dc3ca-17ff-4df9-9800-91736ba2a3b6",
      "messageId": "46ccdf6c-609a-43a5-91c4-7a43b961f0c0"
    },
    "payload": {
      "deviceId": "2fe8f3e8-85be-4a45-863e-beec7314ba2e",
      "playbackState": {
        "playerActivity": "IDLE",
        "repeatMode": "NONE"
      }
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.ReportPlaybackState`](#ReportPlaybackState)
* [オーディオ再生状態を共有する](/Develop/Guides/Handle_Audio_Playback.md#ShareAudioPlaybackState)
{% endif %}

## 共有オブジェクト
AudioPlayer APIを使ってイベントまたはディレクティブを送信する際、ペイロード（`payload`）に次のオブジェクトを共有して使用します。

| オブジェクト            | 説明                                            |
|--------------------|---------------------------------------------------|
| [AudioStreamInfoObject](#AudioStreamInfoObject) | 再生するストリームの再生情報（ストリーミングのURL、認証情報など）を持つオブジェクト |

### AudioStreamInfoObject {#AudioStreamInfoObject}
再生するオーディオストリームのストリーミング情報を持つオブジェクトです。クライアントに対して再生するストリーミングの情報を送信したり、クライアントがCICに対して、現在再生しているコンテンツのストリーミング情報を送信するとき使用します。

#### Object fields
| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:-------------:|
| `beginAtInMilliseconds`  | number | 再生を開始するオフセット。ミリ秒単位で、この値が指定されている場合、クライアントは、そのオーディオストリームを指定されたオフセットから再生する必要があります。この値が0に設定されている場合、ストリームを最初から再生します。          |  |
| `customData`             | string | 現在のストリームに関連して、任意の形式を持つメタデータ情報。特定のカテゴリに分類されたり、定義されないストリーミング情報は、このフィールドに含まれるか、または入力される必要があります。オーディオストリーム再生のコンテキストに必要な追加の値を、サービスプロバイダーがカスタムで追加できます。<div class="warning"><p><strong>警告</strong></p><p>クライアントは、このフィールドの値を任意に使用してはいけません。問題が発生する恐れがあります。また、このフィールドの値は、オーディオ再生状態を送信する際、<a href="/Develop/References/Context_Objects.md#PlaybackState">PlaybackStateコンテキスト</a>の<code>stream</code>フィールドにそのまま含まれる必要があります。</p></div> | 任意/条件付き  |
| `durationInMilliseconds` | number | オーディオストリームの再生時間。クライアントは、`beginAtInMilliseconds`フィールドに指定されている再生のオフセットから、このフィールドに指定されている再生時間だけ、そのオーディオストリームをシークおよび再生できます。例えば、`beginAtInMilliseconds`フィールドの値が`10000`で、このフィールドの値が`60000`の場合、そのオーディオストリームの10秒から70秒までの区間を再生およびシークすることができます。ミリ秒単位です。   | 任意/条件付き  |
| `format`                 | string  | メディア形式（MIMEタイプ）このフィールドから、HLS（HTTP Live Streaming）コンテンツかどうかを確認できます。次の値を持ちます。デフォルト値は`"audio/mpeg"`です。<ul><li><code>"audio/mpeg"</code></li><li><code>"audio/mpegurl"</code></li><li><code>"audio/aac"</code></li><li><code>"application/vnd.apple.mpegurl"</code></li></ul><div class="note"><p><strong>メモ</strong></p><p>HLSでコンテンツを提供するExtensionの開発者は、<a href="mailto:{{ book.ServiceEnv.ExtensionAdminEmail }}">{{ book.ServiceEnv.ExtensionAdminEmail }}</a>までご連絡ください。</p></div>   | 任意/条件付き  |
| `progressReport`         | object  | 再生が開始してから、再生状態をレポートするタイミングを指定するオブジェクト                                                  | 任意/条件付き |
| `progressReport.progressReportDelayInMilliseconds`    | number | 再生が開始してから、指定された時間が経過した後に、再生状態をレポートするように指定する値です。ミリ秒単位で、このフィールドの値はnullの場合があります。  | 任意/条件付き |
| `progressReport.progressReportIntervalInMilliseconds` | number | 再生中に、指定された間隔ごとに再生状態をレポートするように指定する値です。ミリ秒単位で、このフィールドの値はnullの場合があります。        | 任意/条件付き |
| `progressReport.progressReportPositionInMilliseconds` | number | 再生中に、指定された再生位置を経過する度に、再生状態をレポートするように指定する値です。ミリ秒単位で、このフィールドの値はnullの場合があります。    | 任意/条件付き |
| `token`                  | string  | オーディオストリームのトークン。<div class="note"><p><strong>メモ</strong></p><p>このフィールドの最大の長さは2048バイトです。</p></div>                          |  |
| `url`                    | string  | オーディオストリームのURI<div class="note"><p><strong>メモ</strong></p><p>提供するオーディオコンテンツは、<a href="/Design/Audio.md#SupportedAudioFormat">プラットフォームでサポートされているオーディオ圧縮形式</a>である必要があります。</p></div><div class="note"><p><strong>メモ</strong></p><p>このフィールドの最大の長さは、2048バイトです。</p></div>  |  |
| `urlPlayable`            | boolean | `url`フィールドのオーディオストリームのURIがすぐに再生できるかを示す値<ul><li><code>true</code>：すぐに再生できるURI</li><li><code>false</code>：すぐ再生できないURI<a href="#StreamRequested"><code>AudioPlayer.StreamRequested</code></a>イベントでオーディオストリームの情報を追加でリクエストする必要があります。</li></ul>        |  |

#### 備考
* クライアントが`beginAtInMilliseconds`と`durationInMilliseconds`フィールドに指定されている区間に対して、ストリームの再生を終了すると、[`AudioPlayer.PlayFinished`](#PlayFinished)イベントをCICに送信する必要があります。
* クライアントは、ユーザーが`beginAtInMilliseconds`と`durationInMilliseconds`フィールドに指定された区間外ではシーク（seek）できないように、UIを提供する必要があります。
* クライアントの現在の再生状態をCICにレポートする際、[`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)の`totalInMilliseconds`フィールドの値は、`beginAtInMilliseconds`フィールドと`durationInMilliseconds`フィールドに指定された値の合計を入力します。

#### Object Example
{% raw %}

```json
// すぐ再生できるオーディオストリームのURIが含まれたオブジェクト
{
  "beginAtInMilliseconds": 0,
  "progressReport": {
    "progressReportDelayInMilliseconds": null,
    "progressReportIntervalInMilliseconds": 60000,
    "progressReportPositionInMilliseconds": null
  },
  "url": "https://api-ex.example.com/file/12548/22346122",
  "urlPlayable": true
}

// すぐに再生できないオーディオストリームのURIが含まれたサンプル
{
  "beginAtInMilliseconds": 0,
  "progressReport": {
      "progressReportDelayInMilliseconds": null,
      "progressReportIntervalInMilliseconds": null,
      "progressReportPositionInMilliseconds": 60000
  },
  "token": "TR-NM-4435786",
  "urlPlayable": false,
  "url": "clova:TR-NM-4435786"
}
```

{% endraw %}

#### 次の項目も参照してください。
* [`AudioPlayer.Play`](#Play)
* [`AudioPlayer.PlayFinished`](#PlayFinished)
* [`AudioPlayer.ProgressReportDelayPassed`](#ProgressReportDelayPassed)
* [`AudioPlayer.ProgressReportIntervalPassed`](#ProgressReportIntervalPassed)
* [`AudioPlayer.ProgressReportPositionPassed`](#ProgressReportPositionPassed)
* [`AudioPlayer.StreamRequested`](#StreamRequested)
