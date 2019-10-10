# Notifier

Notifierインターフェースは、CICがクライアントデバイスに通知があることを表示したり、ユーザーのアカウントに登録されているすべてのクライアントに通知インジケーターをクリアしたりするように指示します。CICは、クライアントがユーザーの通知の確認を最新状態に維持させるために、クライアントからのリクエストがなくても次のようなディレクティブを送信します。

| メッセージ         | タイプ  | 説明                                   |
|------------------|---------|---------------------------------------------|
| [`ClearIndicator`](#ClearIndicator)         | ディレクティブ | クライアントに、通知インジケーターをすべてクリアするように指示します。             |
| [`Notify`](#Notify)                         | ディレクティブ | クライアントに、通知の内容をユーザーに伝えるように指示します。              |
| [`SetIndicator`](#SetIndicator)             | ディレクティブ | クライアントに、未読の通知があることを表示するように指示します。      |

## ClearIndicatorディレクティブ {#ClearIndicator}
クライアントに、通知インジケーターをすべてクリアするように指示します。クライアントは、通知があることを示すランプや効果音をすべてクリアする必要があります。

### Payload fields
なし

### 備考
このディレクティブは、イベントに対する応答としてではなく、[Downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)で送信されます。

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Notifier",
      "name": "ClearIndicator",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524"
    },
    "payload": {}
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Notifier.Notify`](#Notify)
* [`Notifier.SetIndicator`](#SetIndicator)


## Notifyディレクティブ {#Notify}
クライアントに、通知の内容をユーザーに伝えるように指示します。クライアントは、指定されたやり方で通知があることを示すランプを点灯し、通知に関連するオーディオコンテンツを順番通りに再生する必要があります。

### Payload fields

| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `assetPlayOrder[]`   | string array | `assets[]`フィールドに登録された通知音の再生順序を表す文字列配列です。配列に格納されたオーディオコンテンツの識別子の順番通りに通知音を再生します。            |   |
| `assets[]`           | object array | 通知に関連するオーディオコンテンツを持つオブジェクト配列                          |  |
| `assets[].assetId`   | string       | オーディオコンテンツを区別するための識別子                                        |  |
| `assets[].url`       | string       | オーディオコンテンツのURIです。次のようなスキームまたはオーディオコンテンツのURIを持ちます。<ul><li><code>"clova://notifier/sound/default"</code>：デフォルトの通知音を示すスキームです。あらかじめ定義された通知音を再生します。</li><li>オーディオコンテンツのURI（<code>"http(s)://~</code>）：通知の内容が含まれたオーディオコンテンツのURI。そのURIのオーディオコンテンツを再生します。</li></ul>    |  |
| `light`              | string       | ランプの設定情報<ul><li><code>"DEFAULT"</code>：通知があることを示すランプを点灯します。</li><li><code>"NONE"</code>：通知があることを示すランプを点灯しません。</li></ul>   |   |

### 備考
このディレクティブは、イベントに対する応答としてではなく、[Downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)で送信されます。

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Notifier",
      "name": "Notify",
      "messageId": "688a3d72-06c4-4628-b238-9ac454d3ea65",
    },
    "payload": {
      "light": "DEFAULT",
      "assets": [
        {
          "assetId": "e5179318-d061-42f9-af1b-417180142934",
          "url": "clova://notifier/sound/default"
        },
        {
          "assetId": "9d3df3c1-d0b4-4375-84a6-67c7ae000292",
          "url": "https://streaming.example.com/3325-b5c75045b4ae426885343f9b6abd0bfc-1508160634257"
        }
      ],
      "assetPlayOrder": [
        "e5179318-d061-42f9-af1b-417180142934",
        "9d3df3c1-d0b4-4375-84a6-67c7ae000292"
      ]
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Notifier.ClearIndicator`](#ClearIndicator)
* [`Notifier.SetIndicator`](#SetIndicator)
* [ランプ](/Design/Light.md)
* [オーディオ](/Design/Audio.md)

## SetIndicatorディレクティブ {#SetIndicator}
クライアントに、未読の通知があることを表示するように指示します。クライアントは、通知があることを示すランプを点灯したり、指定されたオーディオインジケーターを再生する必要があります。

### Payload fields
| フィールド名       | データ型    | 説明                     | 任意 |
|---------------|---------|-----------------------------|:---------:|
| `assetPlayOrder[]`   | string array | `assets[]`フィールドに登録された通知音の再生順序を表す文字列配列です。配列に格納されたオーディオコンテンツの識別子の順番通りに通知音を再生します。            |   |
| `assets[]`           | object array | 通知に関連するオーディオコンテンツを持つオブジェクト配列                          |  |
| `assets[].assetId`   | string       | オーディオコンテンツを区別するための識別子                                        |  |
| `assets[].url`       | string       | オーディオコンテンツのURIです。次のようなスキームまたはオーディオコンテンツのURIを持ちます。<ul><li><code>"clova://notifier/sound/default"</code>：デフォルトの通知音を示すスキームです。あらかじめ定義された通知音を再生します。</li><li>オーディオコンテンツのURI（<code>"http(s)://~</code>）：通知の内容が含まれたオーディオコンテンツのURI。そのURIのオーディオコンテンツを再生します。</li></ul>    |  |
| `light`              | string       | ランプの設定情報<ul><li><code>"DEFAULT"</code>：通知があることを示すランプを点灯します。</li><li><code>"NONE"</code>：通知があることを示すランプを点灯しません。</li></ul> |     |
| `new`                | boolean      | 新規の通知に関するディレクティブかどうかを示すフィールドです。<ul><li><code>true</code>：新規の通知である場合</li><li><code>false</code>：新規の通知ではない場合</li></ul> |     |

### 備考
このディレクティブは、イベントに対する応答としてではなく、[Downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection)で送信されます。

### Message example

{% raw %}

```json
{
  "directive": {
    "header": {
      "namespace": "Notifier",
      "name": "SetIndicator",
      "messageId": "29745c13-0d70-408e-a4cc-946afba67524"
    },
    "payload": {
      "assets": [{
        "assetId": "3ea201e8-135f-42fd-a75c-f125331ff9bd",
        "url": "clova://notifier/sound/default"
      }],
      "assetPlayOrder": ["3ea201e8-135f-42fd-a75c-f125331ff9bd"],
      "new": true,
      "light": "DEFAULT"
    }
  }
}
```

{% endraw %}

### 次の項目も参照してください。
* [`Notifier.ClearIndicator`](#ClearIndicator)
* [`Notifier.Notify`](#Notify)
* [ランプ](/Design/Light.md)
* [オーディオ](/Design/Audio.md)
