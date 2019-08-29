<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

# SpeechSynthesizer

<!-- Start of the shared content: CICAPIforAudioPlayback -->
SpeechSynthesizerインターフェースは、クライアントから特定のテキストをTTS（text-to-speech）で音声に合成するようにCICにリクエストしたり、CICから合成された音声をクライアントに送る際に使用する名前空間です。SpeechSynthesizerでは、次のイベントとディレクティブを提供します。

| メッセージ         | タイプ  | 説明                                   |
|------------------|---------|---------------------------------------------|
| [`Request`](#Request)                 | イベント     | CICに、特定のテキストを音声に合成するようにリクエストします。                                               |
| [`Speak`](#Speak)                     | ディレクティブ | クライアントに、合成された音声をスピーカーで出力するように指示します。                                        |
| [`SpeechFinished`](#SpeechFinished)   | イベント     | クライアントから、合成音声の再生を完了したことをCICにレポートします。                                 |
| [`SpeechStarted`](#SpeechStarted)     | イベント     | クライアントから、合成音声の再生を開始したことをCICにレポートします。                                 |
| [`SpeechStopped`](#SpeechStopped)     | イベント     | クライアントから、合成音声の再生を停止したことをCICにレポートします。                                 |

<!-- End of the shared content -->

## Requestイベント {#Request}

CICに、特定のテキストを音声に合成するようにリクエストします。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields
| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `lang`  | string | 音声合成に使用する言語。<ul><li><code>"en"</code>：英語</li><li><code>"ja"</code>：日本語</li><li><code>"ko"</code>：韓国語</li><li><code>"zh"</code>：中国語</li></ul> |     |
| `text`  | string | 音声合成をリクエストする対象となるテキスト           |     |

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
      "namespace": "SpeechSynthesizer",
      "name": "Request",
      "messageId": "ab63d4cb-49f0-4a92-94fc-5ee356193551"
    },
    "payload": {
      "text": "音声ファイルを作って",
      "lang": "ko"
    }
  }
}
```
{% endraw %}

### 次の項目も参照してください。
* [`SpeechSynthesizer.Speak`](#Speak)

## Speakディレクティブ {#Speak}
クライアントに、合成された音声をスピーカーで出力するように指示します。クライアントは、1つのリクエストに対する応答として、複数の`SpeechSynthesizer.Speak`ディレクティブを受信することがあります。従って、メッセージを受信した順番通りに合成音声を再生する必要があります。合成音声は[マルチパートのメッセージ](/Develop/References/CIC_API.md#MultipartMessage)として送信されることもあり、オーディオストリームのURLで送信されることもあります。

### Payload fields
| フィールド名       | データ型    | 説明                     | 常時/条件付き |
|---------------|---------|-----------------------------|:---------:|
| `contentType`          | string  | 再生する音声ファイルのMIMEタイプ。このフィールドは、再生する音声ファイルがHLS形式の場合に提供されます。`"application/vnd.apple.mpegurl"`の値を持ちます。 | 条件付き  |
| `format`               | string  | ファイル形式。現在、`"AUDIO_MPEG"`に固定されています。 |     |
| `token`                | string  | TTSを識別するためのトークン。<div class="note"><p><strong>メモ</strong></p><p>このフィールドの最大の長さは2048バイトです。</p></div>                    |     |
| `ttsLang`              | string  | TTS音声合成に使用する言語。<ul><li><code>"en"</code>：英語</li><li><code>"ja"</code>：日本語</li><li><code>"ko"</code>：韓国語</li><li><code>"zh"</code>：中国語</li></ul> | 条件付き    |
| `url`                  | string  | 再生する音声ファイルのURI。<div class="note"><p><strong>メモ</strong></p><p>このフィールドの最大の長さは2048バイトです。</p></div>                        |     |
| `x-clova-pause-before` | number  | ファイルの再生を開始するまでの時間。整数形式の値で、ミリ秒単位です。        | 条件付き    |

### 備考

`url`は、次の2つの形式があります。クライアントは、形式によって次のように音声を出力する必要があります。

| 形式 | 説明 |
|---------|-------------------------------|
| `cid:{Content-ID}`形式 | クライアントの`url`値が`cid:{Content-ID}`形式の場合、合成された音声がマルチパートのメッセージで送信されます。ヘッダーの`Content-ID`が同じメッセージのバイナリオーディオデータを再生します。オーディオデータが含まれたメッセージは、その順序が保証されないため、受信したディレクティブの`Content-ID`を基準に音声を出力します。|
| URI形式 | 受信した`url`のオーディオストリームを再生します。  |

### Message example

{% raw %}
```
// cid:{Content-ID}形式
// Content-IDが22f2ca4e-3b08-4d33-b32a-7eb62a8c0369のオーディオデータを再生します。

--Boundary-Text
Content-Disposition: form-data; name="speakDirective1"
Content-Type: application/json; charset=utf-8

{
  "directive": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "Speak",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524",
      "dialogRequestId": "caa7862a-3566-4aef-98de-489be0973e18"
    },
    "payload": {
      "format": "AUDIO_MPEG",
      "token": "cbba5103-8ce4-4e65-869b-f94d5878f579",
      "ttsLang": "ko",
      "url": "cid:22f2ca4e-3b08-4d33-b32a-7eb62a8c0369",
      "x-clova-pause-before": 0
    }
  }
}

--Boundary-Text
Content-Disposition: form-data; name="attachment"
Content-ID: 22f2ca4e-3b08-4d33-b32a-7eb62a8c0369
Content-Type: application/octet-stream

{ Audio Attachment }
--Boundary-Text--


// URI形式
--Boundary-Text
{
  "directive": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "Speak",
      "messageId": "0313b471-ad7f-4cdd-b4e1-c046ca8b4b58",
      "dialogRequestId": "efa43b14-67f4-4f00-86bc-dfa08a08ad0b"
    },
    "payload": {
      "format": "AUDIO_MPEG",
      "token": "64ffeb07-4b86-4659-9f59-07a77b363a0b",
      "url": "https://example.net/clova/clova_song/1.mp3"
    }
  }
}

// URL形式（HLSストリーム）
--Boundary-Text
{
  "directive": {
    "header": {
      "namespace": "SpeechSynthesizer",
      "name": "Speak",
      "messageId": "28950dbe-a5a3-4171-9307-0b81d5530a7f",
      "dialogRequestId": "8dce05a4-83ec-42f4-ac6c-a6660c192b96"
    },
    "payload": {
      "contentType": "application/vnd.apple.mpegurl",
      "format": "AUDIO_MPEG",
      "token": "ae97dc8c-859e-4cbb-9884-d32c94029835",
      "url": "https://example.net/clova/clova_song/1.m3u8"
    }
  }
}
--Boundary-Text--
```

{% endraw %}

### 次の項目も参照してください。
* [`SpeechSynthesizer.Request`](#Request)
* [`SpeechSynthesizer.SpeechFinished`](#SpeechFinished)
* [`SpeechSynthesizer.SpeechStarted`](#SpeechStarted)
* [`SpeechSynthesizer.SpeechStopped`](#SpeechStopped)

<!-- Start of the shared content: SpeechSynthesizer.SpeechFinished -->

## SpeechFinishedイベント {#SpeechFinished}
クライアントから、合成音声の再生を完了したことをCICにレポートします。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 常時/条件付き |
|---------------|---------|-----------------------------|:---------:|
| `token`       | string  | [`SpeechSynthesizer.Speak`](#Speak)ディレクティブで送信されたTTSを識別するためのトークン。 |     |

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
      "namespace": "SpeechSynthesizer",
      "name": "SpeechFinished",
      "messageId": "15472673-49a0-4aa1-8cf0-6355669ea473"
    },
    "payload": {
      "token": "cd14ad7a-9611-4b55-8ff5-c9097265950a"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`SpeechSynthesizer.Speak`](#Speak)
* [`SpeechSynthesizer.SpeechStarted`](#SpeechStarted)
* [`SpeechSynthesizer.SpeechStopped`](#SpeechStopped)

<!-- End of the shared content -->

<!-- Start of the shared content: SpeechSynthesizer.SpeechStarted -->

## SpeechStartedイベント {#SpeechStarted}
クライアントから、合成音声の再生を開始したことをCICにレポートします。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 常時/条件付き |
|---------------|---------|-----------------------------|:---------:|
| `token`       | string  | [`SpeechSynthesizer.Speak`](#Speak)ディレクティブで送信されたTTSを識別するためのトークン。 |     |

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
      "namespace": "SpeechSynthesizer",
      "name": "SpeechStarted",
      "messageId": "380c805c-0f19-4ed2-84e2-056f2f4016de"
    },
    "payload": {
      "token": "cd14ad7a-9611-4b55-8ff5-c9097265950a"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`SpeechSynthesizer.Speak`](#Speak)
* [`SpeechSynthesizer.SpeechFinished`](#SpeechFinished)
* [`SpeechSynthesizer.SpeechStopped`](#SpeechStopped)

<!-- End of the shared content -->

<!-- Start of the shared content: SpeechSynthesizer.SpeechStopped -->

## SpeechStoppedイベント {#SpeechStopped}
クライアントから、合成音声の再生を停止したことをCICにレポートします。

### Context fields

{% include "/Develop/References/MessageInterfaces/Context_Objects_List.md" %}

### Payload fields

| フィールド名       | データ型    | 説明                     | 常時/条件付き |
|---------------|---------|-----------------------------|:---------:|
| `token`       | string  | [`SpeechSynthesizer.Speak`](#Speak)ディレクティブで送信されたTTSを識別するためのトークン。 |     |

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
      "namespace": "SpeechSynthesizer",
      "name": "SpeechStopped",
      "messageId": "9a511e5c-4f20-413a-94cc-48172fc8710e"
    },
    "payload": {
      "token": "cd14ad7a-9611-4b55-8ff5-c9097265950a"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`SpeechSynthesizer.Speak`](#Speak)
* [`SpeechSynthesizer.SpeechFinished`](#SpeechFinished)
* [`SpeechSynthesizer.SpeechStarted`](#SpeechStarted)

<!-- End of the shared content -->
