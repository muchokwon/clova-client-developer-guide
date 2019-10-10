## ディレクティブを処理する {#HandleDirective}
ディレクティブはCICから受信するもので、クライアントに特定のアクションを指示するメッセージです。ディレクティブは[イベント](#SendEvent)に対するレスポンスとして、またはDownchannelで送信されます。レスポンスは主に[マルチパートメッセージ](/Develop/References/CIC_API.md#MultipartMessage)形式です。JSON形式の[ディレクティブ](/Develop/References/CIC_API.md#Directive)を最初に受信し、[CIC API](/Develop/References/CIC_API.md)に基づいて、次のように追加の情報（音声データ、コンテンツ情報）を含めるメッセージを続けて受信することがあります。

| コンテンツタイプ            | 説明                                             |
|---------------------|-------------------------------------------------|
| 音声データ            | クライアントスピーカーで出力する音声情報                  |
| JSON形式のコンテンツ情報 | <ul><li>クライアントの画面で表示するデータ（<a href="/Develop/References/Content_Templates.md">コンテンツテンプレート</a>を参照）</li><li>楽曲など再生するコンテンツの位置と認証情報を持つデータ</li></ul> |

クライアントは、次のようにディレクティブを処理する必要があります。

1. 特定のイベントに対するレスポンスや、Downchannelで受信するディレクティブを、あらかじめ指定した[メッセージキュー](#ManageMessageQ)に格納します。
2. 受信した[ディレクティブ](/Develop/References/CIC_API.md#Directive)のメッセージヘッダーを解析（parsing）します。<br />
  通常、`dialogRequestId`はユーザーのリクエスト、`namespace`と`name`は[API](/Develop/References/CIC_API.md)の区別に使用されます。以下は、受信したディレクティブのサンプルです。
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
3. 受信したディレクティブの[ダイアログID](/Develop/Guides/Manage_Dialogue_ID_And_Handle_Tasks.md)（`dialogRequestId`）が、クライアントが保管しているものと一致するかを確認します。
  * **クライアントが保管している最終ダイアログIDと一致する場合**、APIリファレンスに基づいて、必要なアクションを実行します。ディレクティブの`payload`に含まれた[`cid`値を使用](/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak)して、クライアントの動作に必要な追加情報（音声データ）を[メッセージキュー](#ManageMessageQ)から識別できます。`cid`は、次のようにマルチパートメッセージのうち、1つのパートとして送信された音声データの`Content-ID`メッセージヘッダーを表します。
    ```
    --b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba
    Content-Disposition: form-data; name="attachment-39b2f844-b168-4dc2-bea7-d5c249e446e3"
    Content-ID: d329085c-379e-48aa-b871-7ecebdbe831d
    Content-Type: application/octet-stream<br />
    [[ binary audio attachment ]]<br />
    --b4bc211bbd32e5cb5989bc7ab2d3088fdd72dcc6696253151c98176f88ba
    ```
  * **クライアントが保管しているダイアログIDと一致しない場合**、そのディレクティブに関連するすべてのメッセージを無視して、キューから削除します。

<div class="note">
  <p><strong>メモ</strong></p>
  <p>クライアントでサポートしないことにしたディレクティブや、不明のディレクティブを受信した場合、そのディレクティブを無視する必要があります。クライアントは、CICからマルチパートメッセージ形式で、一度に複数のディレクティブを受信することがあります。サポートされていないか、または不明のディレクティブが含まれている場合、そのディレクティブのみを無視する必要があります。</p>
</div>
