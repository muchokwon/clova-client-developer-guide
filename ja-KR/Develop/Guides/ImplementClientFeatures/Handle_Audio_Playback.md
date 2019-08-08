# オーディオ再生を処理する

Clovaはユーザーのリクエストに応じて、オーディオを再生したり、再生に関連するディレクティブをクライアントに送信します。クライアントは、Clovaがオーディオ再生に関連して送信するメッセージを処理して、クライアントのオーディオ再生状態をイベントを使ってCICに送信する必要があります。特に、CICにイベントを送信して、Clovaにクライアントの再生状態を知らせる必要があります。状況や条件に適切なイベントを使って、再生状態をレポートするようにしてください。ここでは、以下の内容について説明します。

* [オーディオを再生する](#PlayAudioStream)
* [オーディオ再生の進行状況をレポートする](#ReportAudioPlaybackProgress)
* [オーディオ再生をコントロールする](#ControlAudioPlayback)
* [オーディオ再生状態を共有する](#ShareAudioPlaybackState)
* [ストリームの情報を管理する](#ManageStreamInfo)

## オーディオを再生する {#PlayAudioStream}
ユーザーがオーディオの再生をリクエストすると、ClovaはCICを介して、ユーザーからリクエストされたオーディオアイテムを再生するようにクライアントに指示します。次は、オーディオを再生する動作の流れを示します。

![](/Develop/Assets/Images/CIC_Audio_Play_Work_Flow.svg)

ユーザーがオーディオの再生をリクエストすると、CICは、最初にクライアントに[`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)ディレクティブを送信します。このディレクティブには、オーディオの再生に必要な情報が含まれています。この情報を使って、オーディオファイルを探したり、プレイヤーにオーディオアイテムの情報を表示したりする必要があります。次の[`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)ディレクティブを受信します。

```json
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
```

`payload`の主なフィールドには、以下のものがあります。

* `audioItem`：再生するオーディオアイテムのメタデータが含まれたオブジェクト
* `audioItem.stream`：再生に必要なオーディオストリームの情報を持っているオブジェクト
* `source`：オーディオストリーミングサービスの提供元

クライアントは、`audioItem.stream`フィールドに含まれた情報を使ってオーディオを再生し、`audioItem`フィールドと`source`フィールドの内容をプレイヤーのUIに表示したり、参照したりします。

受信した[`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)ディレクティブ内の`audioItem.stream.urlPlayable`フィールドの値が`false`の場合、`audioItem.stream`フィールドの情報ではオーディオアイテムをすぐに再生できません。これは、サービスの課金およびセキュリティなどのせいで、オーディオを再生する直前に、そのオーディオの実際の情報をもう一度照会する必要がある場合に発生します。次は、すぐに再生できない[`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)ディレクティブのサンプルです。

```json
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

`audioItem.stream.urlPlayable`フィールドの値が`false`の場合、[`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)ディレクティブで受け取った`audioItem.stream`フィールドの値をそのまま使用して、次のように再びCICに[`AudioPlayer.StreamRequested`](/Develop/References/CICInterface/AudioPlayer.md#StreamRequested)イベントを送信する必要があります。

```json
{
  "context": [
    ...
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

上記のように[`AudioPlayer.StreamRequested`](/Develop/References/CICInterface/AudioPlayer.md#StreamRequested)イベントを送信すると、次のように実際に再生できる情報が含まれた[`AudioPlayer.StreamDeliver`](/Develop/References/CICInterface/AudioPlayer.md#StreamDeliver)ディレクティブをCICから受信します。クライアントは、新しく受信する情報でオーディオを再生します。

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

<div class="note">
  <p><strong>メモ</strong></p>
  <p>ストリームの再生情報は最新の状態を維持するために、常に管理される必要があります。詳細については、<a href="#ManageStreamInfo">ストリームの情報を管理する</a>を参照してください。</p>
</div>


## オーディオ再生の進行状況をレポートする {#ReportAudioPlaybackProgress}

Clovaは、オーディオ再生に関連して、ユーザーが今どのような状況にいるかを知る必要があります。クライアントはオーディオの再生を開始すると、次のように再生の進行状況をCICにレポートする必要があります。

![](/Develop/Assets/Images/CIC_Audio_Play_Progress_Reporting.svg)

上記の流れのとおり、一部の再生の進行状況は、[`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)ディレクティブの`audioItem.stream.progressReport`フィールド、または[`AudioPlayer.StreamDeliver`](/Develop/References/CICInterface/AudioPlayer.md#StreamDeliver)ディレクティブの`audioStream.progressReport`フィールドの値によって、レポートするかどうかが決まります。次の表は、再生の進行状況をレポートする必要がある状況と条件、使用するイベントを示します。

| 再生ポイント                                 | 条件                         | レポートするときに使用するイベント |
|-----------------------------------------|----------------------------|----------------------|
| オーディオ再生を開始した直後                         |                                                                       | [`AudioPlayer.PlayStarted`](/Develop/References/CICInterface/AudioPlayer.md#PlayStarted)イベント  |
| オーディオ再生を開始してから、指定された時間が経過するとき          | `progressReport.progressReportDelayInMilliseconds`フィールドが`null`ではない場合   | [`AudioPlayer.ProgressReportDelayPassed`](/Develop/References/CICInterface/AudioPlayer.md#ProgressReportDelayPassed)イベント  |
| オーディオ再生を開始してから、指定された間隔の時間が経過するとき  | `progressReport.progressReportIntervalInMilliseconds`フィールドが`null`ではない場合  | [`AudioPlayer.ProgressReportIntervalPassed`](/Develop/References/CICInterface/AudioPlayer.md#ProgressReportIntervalPassed)イベント  |
| オーディオ再生を開始してから、特定のオフセットが経過するとき  | `progressReport.progressReportPositionInMilliseconds`フィールドが`null`ではない場合  | [`AudioPlayer.ProgressReportPositionPassed`](/Develop/References/CICInterface/AudioPlayer.md#ProgressReportPositionPassed)イベント  |
| オーディオ再生が完了した直後                         |                                                                       | [`AudioPlayer.PlayFinished`](/Develop/References/CICInterface/AudioPlayer.md#PlayFinished)イベント  |

<div class="note">
  <p><strong>メモ</strong></p>
  <p>受信した<a href="/Develop/References/CICInterface/AudioPlayer.md#Play"><code>AudioPlayer.Play</code></a>ディレクティブの<code>audioItem.stream.urlPlayable</code>フィールドが<code>false</code>の場合、後から<a href="/Develop/References/CICInterface/AudioPlayer.md#StreamDeliver"><code>AudioPlayer.StreamDeliver</code></a>ディレクティブでストリームの情報が新しく送信されることがあります。<code>AudioPlayer.Play</code>ディレクティブの後に受信する<code>AudioPlayer.StreamDeliver</code>ディレクティブで<code>progressReport.progressReportDelayInMilliseconds</code>、<code>progressReport.progressReportIntervalInMilliseconds</code>、<code>progressReport.progressReportPositionInMilliseconds</code>フィールドの値が変更されている場合、その値を適用してオーディオ再生の進行状況をレポートする必要があります。詳細については、<a href="#ManageStreamInfo">ストリームの情報を管理する</a>を参照してください。</p>
</div>

Clovaは、上記のイベントから、ユーザーが今聞いているオーディオアイテムと、そのオーディオアイテムの再生ポイントを把握します。次は、[`AudioPlayer.ProgressReportIntervalPassed`](/Develop/References/CICInterface/AudioPlayer.md#ProgressReportIntervalPassed)イベントのサンプルです。このとき、[オーディオ再生に関連するコンテキストを含めて](#BuildPlaybackStateContext)送信する必要があります。

```json
{
  "context": [
    ...
    {
      "header": {
        "namespace": "AudioPlayer",
        "name": "PlaybackState"
      },
      "payload": {
        "offsetInMilliseconds": 10000,
        "totalInMilliseconds": 300000,
        "playerActivity": "PLAYING",
        "repeatMode": "NONE",
        "stream": {
          "beginAtInMilliseconds": 0,
          "format": "audio/mpeg",
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": 120000,
            "progressReportPositionInMilliseconds": 600000
          },
          "token": "TR-NM-4435786",
          "url": "https://musicservice.example.net/b767313e.mp3",
          "urlPlayable": true
        }
      }
    }
    ...
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



## オーディオ再生をコントロールする {#ControlAudioPlayback}

ユーザーは、クライアントがオーディオ再生を一時停止（pause）、再開（resume）、停止（stop）、もう一度再生（replay）するようにClovaにリクエストすることができます。オーディオ再生のコントロールは、次の3つの方法でリクエストできます。

* ユーザーが発話で再生をコントロールする
* ユーザーがクライアントデバイスのボタンで再生をコントロールする
* ユーザーがClovaアプリからリモートで特定のクライアントの再生をコントロールする

Clovaは、ユーザーのオーディオ再生状況を把握する必要があります。そのため、オーディオ再生のコントロールは、クライアントでは直接行われません。再生のコントロールは、すべてClovaを介するディレクティブで行われ、主に[`PlaybackController`](/Develop/References/CICInterface/PlaybackController.md)インターフェースで実装される必要があります。また、その結果は[`AudioPlayer`](/Develop/References/CICInterface/AudioPlayer.md)インターフェースのイベントでレポートされる必要があります。

次は、オーディオの再生が一時停止する動作の流れを示します。

![](/Develop/Assets/Images/CIC_Audio_Playback_Control_Flow.svg)

通常、ユーザーの発話から行われる再生のコントロールは、Clovaで解析され、該当する再生コントロールのディレクティブ（[`PlaybackController.Pause`](/Develop/References/CICInterface/PlaybackController.md#Pause)）がクライアントに送信されます。ユーザーがクライアントのボタンを押して一時停止するようにリクエストした場合、次のような[`PlaybackController.PauseCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#PauseCommandIssued)イベントで一時停止ボタンが押されたことをClovaにレポートする必要があります。

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "PlaybackController",
      "name": "PauseCommandIssued",
      "messageId": "4e4080d6-c440-498a-bb73-ae86c6312806"
    },
    "payload": {}
  }
}
```

ユーザーがClovaアプリからリモートでクライアントの再生コントロールをリクエストした場合、Clovaはそのクライアントに[`PlaybackController.ExpectPauseCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectPauseCommand)のようなディレクティブを送信します。これは、ユーザーがリモートでボタンを押したときと同じ動作をするように指示するメッセージです。次のメッセージを受信した場合、[`PlaybackController.PauseCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#PauseCommandIssued)イベントをClovaに送信する必要があります。

```json
// ユーザーが一時停止ボタンを押したときと同じ動作をするようにリクエストする
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "ExpectPauseCommand",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

Clovaは、ユーザーからリクエストされた再生コントロールを、次のようなディレクティブ（[`PlaybackController.Pause`](/Develop/References/CICInterface/PlaybackController.md#Pause)）で渡します。クライアントは、受信したディレクティブに応じてオーディオの再生をコントロールします。

```json
{
  "directive": {
    "header": {
      "namespace": "PlaybackController",
      "name": "Pause",
      "dialogRequestId": "42390b12-ae91-4121-aa0a-37f74e8e422b",
      "messageId": "b1f88d7d-bbb8-44fa-a0a2-c5a7553e6f8a"
    },
    "payload": {}
  }
}
```

最後に、クライアントは再生コントロールが完了すると、プレイヤーの動作の変化をCICにレポートする必要があります。次は、オーディオの再生を一時停止した後、CICに送信する[AudioPlayer.PlayPaused](/Develop/References/CICInterface/AudioPlayer.md#PlayPaused)イベントです。このとき、AudioPlayerの状態に応じて[コンテキストを作成](#BuildPlaybackStateContext)する必要があります。

```json
{
  "context": [
    ...
    {
      "header": {
        "namespace": "AudioPlayer",
        "name": "PlaybackState"
      },
      "payload": {
        "offsetInMilliseconds": 83100,
        "totalInMilliseconds": 300000,
        "playerActivity": "PAUSED",
        "repeatMode": "NONE",
        "stream": {
          "beginAtInMilliseconds": 0,
          "format": "audio/mpeg",
          "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": null,
            "progressReportPositionInMilliseconds": 60000
          },
          "token": "TR-NM-4435786",
          "url": "https://musicservice.example.net/b767313e.mp3",
          "urlPlayable": true
        }
      }
    }
    ...
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

## ストリームの情報を管理する {#ManageStreamInfo}

クライアントは、[オーディオを再生する](#PlayAudioStream)とき、CICからオーディオストリームに関連する情報を受信します。クライアントは、CICから[`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)ディレクティブを受信したときから、そのディレクティブの`audioItem.stream`フィールドに含まれたストリームの情報を管理する必要があります。ここでは、以下の項目について説明します。

* [ストリームの情報を更新する](#UpdateStreamInfo)
* [ストリームのコンテキストを作成する](#BuildPlaybackStateContext)

<div class="note">
  <p><strong>メモ</strong></p>
  <p>ここで説明されている内容に従わないと、ストリームを正常に再生できないか、あるいはユーザーがオーディオを再び再生するようにリクエストしても、Clovaがそのリクエストを処理できない恐れがあります。</p>
</div>

### ストリームの情報を更新する {#UpdateStreamInfo}

`AudioPlayer.Play`ディレクティブを受信した場合、`audioItem.stream`フィールド以下の値を保管する必要があります。特に、`audioItem.stream.urlPlayable`フィールドが`false`の場合、実際に再生できるストリームの情報を追加で取得するために、クライアントは`audioItem.stream`フィールド以下の情報を保管している必要があります。

```json
{
  "directive": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "Play",
      ...
    },
    "payload": {
      "audioItem": {
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
        ...
      },
      ...
    }
  }
}
```

<div class="note">
  <p><strong>メモ</strong></p>
  <p>受信した<a href="/Develop/References/CICInterface/AudioPlayer.md#Play"><code>AudioPlayer.Play</code></a>ディレクティブの<code>audioItem.stream.urlPlayable</code>フィールドが<code>false</code>の場合、<strong>後から<a href="/Develop/References/CICInterface/AudioPlayer.md#StreamDeliver"><code>AudioPlayer.StreamDeliver</code></a>ディレクティブで受信するストリームの情報と統合する必要があります。<strong></p>
</div>

その後、[`AudioPlayer.StreamRequested`](/Develop/References/CICInterface/AudioPlayer.md#StreamRequested)イベントを使用して、ストリームの再生に必要な情報をリクエストします。このとき、保管していた`audioItem.stream`フィールドの値を含める必要があります。

```json
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "StreamRequested",
      ...
    },
    "payload": {
      ...
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

すると、以下のように、実際に再生できる情報が含まれた[`AudioPlayer.StreamDeliver`](/Develop/References/CICInterface/AudioPlayer.md#StreamDeliver)ディレクティブをCICから受信します。ここに含まれている`audioStream`フィールドの内容を**必ず統合する必要があります。**更新されたフィールドの内容は以前の内容に上書きし、更新がない部分はそのまま保持します。

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

上記のように、管理されているストリームの情報は、[コンテキストとして作成](#BuildPlaybackStateContext)およびレポートされる必要があります。

### ストリームのコンテキストを作成する {#BuildPlaybackStateContext}

オーディオ再生の状態は、クライアントがCICにイベントを送信する度に、コンテキストによってレポートされる必要があります。クライアントのAudioPlayerが無効になっている場合、以下のように`playerActivity`フィールドの値を`"IDLE"`にして送信します。

```json
{
  "context": [
    ...
    {
      "header": {
        "namespace": "AudioPlayer",
        "name": "PlaybackState"
      },
      "payload": {
        "playerActivity": "IDLE",
        "repeatMode": "NONE"
      }
    }
    ...
  ],
  "event": {
    ...
  }
}
```

AudioPlayerが有効になっている場合には、AudioPlayerの状態に応じて`playerActivity`フィールドを次のいずれかに設定します。

* `"PLAYING"`
* `"PAUSE"`
* `"STOPPED"`

このとき、[`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)ディレクティブや[`AudioPlayer.StreamDeliver`](/Develop/References/CICInterface/AudioPlayer.md#StreamDeliver)ディレクティブで受信し、[管理しているストリームの情報](#UpdateStreamInfo)がコンテキストに含まれる必要があります。

```json
{
  "context": [
    ...
    {
      "header": {
        "namespace": "AudioPlayer",
        "name": "PlaybackState"
      },
      "payload": {
        "offsetInMilliseconds": 10000,
        "totalInMilliseconds": 300000,
        "playerActivity": "STOPPED",
        "repeatMode": "NONE",
        "stream": {
          "beginAtInMilliseconds": 0,
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
    ...
  ],
  "event": {
    ...
  }
}
```

ユーザーがストリームの再生を停止するようにリクエストし、AudioPlayerの状態が`"PAUSE"`に遷移した場合にも、保管していたストリームの情報を破棄するべきではありません。ストリームの情報を破棄すると、ユーザーがオーディオを再び再生するようにリクエストしたときに、CICはクライアントで再生していたストリームの情報をコンテキストから確認できなくなります。そのため、Clovaは引き続き再生などの機能をユーザーに提供できない恐れがあります。なので、クライアントは、再起動などの特殊な状況を除いて、ストリームの情報を保管する必要があります。ストリームの情報を削除する必要がある場合、Clovaからクライアントのストリームの情報を削除するように[AudioPlayer.ClearQueue](/Develop/References/CICInterface/AudioPlayer.md#ClearQueue)のようなディレクティブを送信します。


## オーディオ再生状態を共有する {#ShareAudioPlaybackState}

1つのクライアントは、ユーザーのアカウントに登録されているすべてのクライアントまたは特定のクライアントと、オーディオ再生状態を共有することができます。次は、オーディオ再生状態を共有される動作の流れを示します。

![](/Develop/Assets/Images/CIC_Playback_State_Sync_Work_Flow.svg)

1. Clovaアプリは、{{ "[`AudioPlayer.RequestPlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#RequestPlaybackState)イベントを使用して" if book.DocMeta.TargetReaderType == "Internal" }}CICに対して、ユーザーのアカウントに登録されているすべてのクライアント、または特定のクライアントのオーディオ再生状態をリクエストします。
2. CICは、[`AudioPlayer.ExpectReportPlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#ExpectReportPlaybackState)ディレクティブで、ユーザーのアカウントに登録されているすべてのクライアント、または特定のクライアントに、現在のオーディオ再生状態をレポートするように指示します。
3. オーディオ再生状態をレポートするように指示されたクライアントは、[`AudioPlayer.ReportPlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#ReportPlaybackState)イベントで、CICに現在のオーディオ再生状態をレポートします。
4. CICは、{{ "[`AudioPlayer.SynchronizePlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#SynchronizePlaybackState)を使用して" if book.DocMeta.TargetReaderType == "Internal" }}他のクライアントのオーディオ再生状態をリクエストしたクライアントに状態情報を送信し、同期するように指示します。

クライアントは[`AudioPlayer.ExpectReportPlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#ExpectReportPlaybackState)ディレクティブを受信すると、[`AudioPlayer.ReportPlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#ReportPlaybackState)イベントをCICに送信する必要があります。そのとき、次の情報を`payload`フィールドに含める必要があります。

* オーディオプレイヤーの状態
* リピート再生するかどうか
* 再生しているオーディオの現在の再生ポイント（オーディオを再生しているとき）
* [`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)ディレクティブで受信した[`AudioStreamInfoObject`](/Develop/References/CICInterface/AudioPlayer.md#AudioStreamInfoObject)オブジェクト（オーディオを再生しているか、または一時停止しているとき）
* [`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)ディレクティブの`audioItem.stream.token`フィールドの値（オーディオを再生しているか、または位置停止しているとき）
* 再生しているオーディオアイテムの全体の長さ（任意）

```json
// オーディオを再生していないとき
{
  "context": [
    ...
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

// オーディオを再生しているか、または一時停止しているとき
{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "AudioPlayer",
      "name": "ReportPlaybackState",
      "messageId": "aa1b20b1-7562-4236-a7a6-65a235571bac"
    },
    "payload": {
      "playerActivity": "PLAYING",
      "repeatMode": "NONE",
      "offsetInMilliseconds": 13000,
      "stream": {
        "beginAtInMilliseconds": 0,
        "progressReport": {
            "progressReportDelayInMilliseconds": null,
            "progressReportIntervalInMilliseconds": null,
            "progressReportPositionInMilliseconds": 30000
        },
        "token": "TR-NM-4435786",
        "urlPlayable": false,
        "url": "clova:TR-NM-4435786"
      },
      "token": "TR-NM-4435786"
    }
  }
}
```
