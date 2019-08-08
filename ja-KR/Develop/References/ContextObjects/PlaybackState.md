<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

<!-- Start of the shared content: AudioPlayer.PlaybackState -->

## AudioPlayer.PlaybackState {#PlaybackState}

`AudioPlayer.PlaybackState`は、現在再生しているか、または最後に再生したオーディオの情報をCICにレポートするときに使用されるメッセージ形式です。

### Object structure
{% raw %}
```json
{
  "header": {
    "namespace": "AudioPlayer",
    "name": "PlaybackState"
  },
  "payload": {
    "offsetInMilliseconds": {{number}},
    "playerActivity": {{string}},
    "repeatMode": {{string}},
    "stream": {{AudioStreamInfoObject}},
    "totalInMilliseconds": {{number}}
  }
}
```
{% endraw %}


### Payload fields

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `offsetInMilliseconds` | number | 最近再生したメディアの最後の再生ポイント（オフセット）ミリ秒単位で、`playerActivity`の値が`"IDLE"`の場合、このフィールドの値を入力する必要はありません。                                                  | 任意 |
| `playerActivity`       | string | プレイヤーの状態を示す値です。次のような値を持ちます。<ul><li><code>"IDLE"</code>：非アクティブ状態</li><li><code>"PLAYING"</code>：再生中</li><li><code>"PAUSED"</code>：一時停止状態</li><li><code>"STOPPED"</code>：停止状態</li></ul> |  |
| `repeatMode`           | string  | リピート再生モード<ul><li><code>"NONE"</code>：リピート再生しない</li><li><code>"REPEAT_ONE"</code>：一曲リピート再生</li></ul>                                                   |   |
| `stream`               | [AudioStreamInfoObject](/Develop/References/CICInterface/AudioPlayer.md#AudioStreamInfoObject) | 再生中のオーディオの詳細情報が保存されているオブジェクト`playerActivity`の値が`"IDLE"`の場合、このフィールドの値を入力する必要はありません。[`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)または[`AudioPlayer.StreamDeliver`](/Develop/References/CICInterface/AudioPlayer.md#StreamDeliver)ディレクティブで送信されたオーディオの情報（`stream`オブジェクト）の値を入力します。 | 任意 |
| `totalInMilliseconds`  | number | 最近再生したメディアの全長[`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)ディレクティブで送信されたオーディオストリーム情報（[AudioStreamInfoObject](/Develop/References/CICInterface/AudioPlayer.md#AudioStreamInfoObject)）に`durationInMilliseconds`フィールド値がある場合、このフィールドに入力します。ミリ秒単位で、`playerActivity`の値が`"IDLE"`の場合、このフィールドの値を入力する必要はありません。                                                               | 任意 |

### Object example

{% raw %}

```json
// Case 1：再生が停止された状態
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
      "progressReport": {
        "progressReportDelayInMilliseconds": null,
        "progressReportIntervalInMilliseconds": null,
        "progressReportPositionInMilliseconds": 60000
      },
      "token": "TR-NM-17740107",
      "url": "clova:TR-NM-17740107",
      "urlPlayable": false
    }
  }
}

//例2：プレイヤーが非アクティブになっている状態
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
```

{% endraw %}

### 次の項目も参照してください。
* [`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)
* [`AudioPlayer.StreamDeliver`](/Develop/References/CICInterface/AudioPlayer.md#StreamDeliver)
* [`AudioPlayer.StreamRequested`](/Develop/References/CICInterface/AudioPlayer.md#StreamRequested)

<!-- End of the shared content -->
