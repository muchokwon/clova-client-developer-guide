# Textテンプレート
画面に表示するテキストデータを提供するテンプレートです。強調、パラグラフ、表形式のテキストを表示するときに使用されます。

<div class="tip">
<p><strong>ヒント</strong></p>
<p>Textテンプレートの表示形式については、<a href="#UIExample">UI example</a>を参照してください。</p>
</div>

## Template fields

| フィールド名       | データ型    | 説明                     |
|---------------|---------|-----------------------------|
| `bgUrl`                  | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 背景画像ファイルのURIを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。               |
| `emotionCode`            | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 感情表現が定義されたコード。感情コードを利用して、クライアントであらかじめ定義された感情表現を表示することができます。クライアントに感情を表現する機能がない場合、このコードを無視します。<div class="note"><p><strong>メモ</strong></p><p>感情コードの仕様の詳細は、関連する提携担当者までお問い合わせください。</p></div> |
| `highlightText`          | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)または[NumberObject](/Develop/References/ContentTemplates/Shared_Objects.md#NumberObject) | 強調するテキストまたは数値を持つオブジェクト。数値の場合、桁区切り記号を使用することもできます。このオブジェクトの`value`フィールドは、空文字列（`""`）または`null`値を持つ場合があります。 |
| `imageUrl`               | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 画像のURIを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。                              |
| `linkUrl`                | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 地図の画像が含まれた場合、ウェブの地図に移動するURIを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。 |
| `mainText`               | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | メインテキストを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。                                     |
| `motionCode`             | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | アクションが定義されたコード。アクションコードを利用して、クライアントであらかじめ定義されたアクションを実行することができます。クライアントにアクションを表現する機能がない場合、このコードを無視します。<div class="note"><p><strong>メモ</strong></p><p>アクションコードの仕様の詳細は、関連する提携担当者までお問い合わせください。</p></div> |
| `paragraphText`          | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | パラグラフのテキストを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。                                |
| `provider.logoUrl`       | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 参照したサービスのロゴ画像のURIを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。  |
| `provider.name`          | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 参照したサービスの名称のテキストを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。  |
| `referenceText`          | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 参照したサービスの情報を持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。  |
| `referenceUrl`           | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 参照したサービスの利用結果ページのURIを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。   |
| `sentenceText`           | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 文章のテキストを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。                                |
| `subText`                | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | サブテキストを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。                                     |
| `tableList[]`           | object array                                                                    | 表形式のテキストを持つオブジェクト配列。行が2つまたは3つある表を構成します。     |
| `tableList[].item1`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 1行目に表示されるテキストを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。                    |
| `tableList[].item2`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 2行目に表示されるテキストを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。                    |
| `tableList[].item2Link` | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)または[PhoneNumberObject](/Develop/References/ContentTemplates/Shared_Objects.md#PhoneNumberObject) | 2行目に表示されるテキストリンク先URIまたは電話番号を持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。 |
| `tableList[].item3`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 3行目に表示されるテキストを持つオブジェクト。このフィールドは省略できます。 |
| `type`                   | string                                                                          | コンテンツテンプレートのタイプを示す値。`"Text"`を持ちます。             |

## Template example

{% raw %}

```json
// サンプル1.
// ユーザーのリクエスト：今1ドルいくらなの? （強調テキストで表示する）
{
  "actionList": [
    {
      "type": "action",
      "value": ""
    }
  ],
  "bgUrl": {
    "type": "url",
    "value": ""
  },
  "highlightText": {
    "type": "number",
    "value": "1,119"
  },
  "mainText": {
    "type": "string",
    "value": "ウォン"
  },
  "paragraphText": {
    "type": "string",
    "value": ""
  },
  "referenceText": {
    "type": "string",
    "value": "KEBハナ銀行"
  },
  "referenceUrl": {
    "type": "url",
    "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=1%eb%8b%ac%eb%9f%ac+%ed%99%98%ec%9c%a8"
  },
  "sentenceText": {
    "type": "string",
    "value": ""
  },
  "subText": {
    "type": "string",
    "value": "-0.13%"
  },
  "tableList": [
    {
      "item1": {
        "type": "string",
        "value": ""
      },
      "item2": {
        "type": "string",
        "value": ""
      },
      "item2Link": {
        "type": "",
        "value": ""
      }
    }
  ],
  "emotionCode": {
    "type": "string",
    "value": ""
  },
  "motionCode": {
    "type": "string",
    "value": ""
  },
  "type": "Text"
}

// サンプル2.
// ユーザーのリクエスト：トッテナムの監督は誰なの? （パラグラフのテキストで表示する）
{
  "actionList": [
    {
      "type": "action",
      "value": ""
    }
  ],
  "bgUrl": {
    "type": "url",
    "value": ""
  },
  "highlightText": {
    "type": "string",
    "value": ""
  },
  "mainText": {
    "type": "string",
    "value": ""
  },
  "paragraphText": {
    "type": "string",
    "value": "トッテナム・ホットスパーFCの監督\n マウリシオ・ポチェッティーノ"
  },
  "referenceText": {
    "type": "string",
    "value": "検索結果"
  },
  "referenceUrl": {
    "type": "url",
    "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=%ed%86%a0%ed%8a%b8%eb%84%98+%ea%b0%90%eb%8f%85%ec%9d%b4+%eb%88%84%ea%b5%ac%ec%95%bc?"
  },
  "sentenceText": {
    "type": "string",
    "value": ""
  },
  "subText": {
    "type": "string",
    "value": ""
  },
  "tableList": [
    {
      "item1": {
        "type": "string",
        "value": ""
      },
      "item2": {
        "type": "string",
        "value": ""
      },
      "item2Link": {
        "type": "",
        "value": ""
      }
    }
  ],
  "emotionCode": {
    "type": "string",
    "value": ""
  },
  "motionCode": {
    "type": "string",
    "value": ""
  },
  "type": "Text"
}

// サンプル3.
// ユーザーのリクエスト：花屋の電話番号を教えて（表形式のテキストで表示する）
{
  "actionList": [
    {
      "type": "action",
      "value": ""
    }
  ],
  "bgUrl": {
    "type": "url",
    "value": ""
  },
  "highlightText": {
    "type": "string",
    "value": ""
  },
  "mainText": {
    "type": "string",
    "value": ""
  },
  "paragraphText": {
    "type": "string",
    "value": ""
  },
  "referenceText": {
    "type": "string",
    "value": "検索結果"
  },
  "referenceUrl": {
    "type": "url",
    "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=%ea%bd%83%ec%a7%91"
  },
  "sentenceText": {
    "type": "string",
    "value": ""
  },
  "subText": {
    "type": "string",
    "value": ""
  },
  "tableList": [
    {
      "item1": {
        "type": "string",
        "value": "タッジーマッジーフラワー盆唐亭子店"
      },
      "item2": {
        "type": "string",
        "value": "031-716-6676"
      },
      "item2Link": {
        "type": "phoneNum",
        "value": "031-716-6676"
      }
    },
    {
      "item1": {
        "type": "string",
        "value": "サマセットフラワー"
      },
      "item2": {
        "type": "string",
        "value": "031-712-3310"
      },
      "item2Link": {
        "type": "phoneNum",
        "value": "031-712-3310"
      }
    },
    ...
  ],
  "emotionCode": {
    "type": "string",
    "value": ""
  },
  "motionCode": {
    "type": "string",
    "value": ""
  },
  "type": "Text"
}

// サンプル4.
// ユーザーのリクエスト：ごめんね。（感情を表現する）

{
  "actionList": [
    {
      "type": "action",
      "value": ""
    }
  ],
  "bgUrl": {
    "type": "url",
    "value": ""
  },
  "highlightText": {
    "type": "string",
    "value": ""
  },
  "mainText": {
    "type": "string",
    "value": ""
  },
  "paragraphText": {
    "type": "string",
    "value": "気にしなくていいですよ"
  },
  "referenceText": {
    "type": "string",
    "value": ""
  },
  "referenceUrl": {
    "type": "url",
    "value": ""
  },
  "sentenceText": {
    "type": "string",
    "value": ""
  },
  "subText": {
    "type": "string",
    "value": ""
  },
  "tableList": [
    {
      "item1": {
        "type": "string",
        "value": ""
      },
      "item2": {
        "type": "string",
        "value": ""
      },
      "item2Link": {
        "type": "",
        "value": ""
      }
    }
  ],
  "emotionCode": {
    "type": "string",
    "value": "EmotionCode7"
  },
  "motionCode": {
    "type": "string",
    "value": ""
  },
  "type": "Text"
}

// サンプル5.
// ユーザーのリクエスト：ダンスを踊って。（アクションを表現する）

{
  "actionList": [
    {
      "type": "action",
      "value": ""
    }
  ],
  "bgUrl": {
    "type": "url",
    "value": ""
  },
  "highlightText": {
    "type": "string",
    "value": ""
  },
  "mainText": {
    "type": "string",
    "value": ""
  },
  "paragraphText": {
    "type": "string",
    "value": "それは私の能力の範囲外です"
  },
  "referenceText": {
    "type": "string",
    "value": ""
  },
  "referenceUrl": {
    "type": "url",
    "value": ""
  },
  "sentenceText": {
    "type": "string",
    "value": ""
  },
  "subText": {
    "type": "string",
    "value": ""
  },
  "tableList": [
    {
      "item1": {
        "type": "string",
        "value": ""
      },
      "item2": {
        "type": "string",
        "value": ""
      },
      "item2Link": {
        "type": "",
        "value": ""
      }
    }
  ],
  "emotionCode": {
    "type": "string",
    "value": "MotionDance"
  },
  "motionCode": {
    "type": "string",
    "value": ""
  },
  "type": "Text"
}
```

{% endraw %}

## UI example {#UIExample}
以下は、{{ book.ServiceEnv.OrientedService }}が配布したClovaのモバイルアプリで、Textテンプレートの内容を表したUIサンプルです。

| 強調テキスト | パラグラフのテキスト | 表形式のテキスト |
|-------|-------|-------|
| ![Highlight](/Develop/Assets/Images/Content_Template-Highlight_Text.png) | ![Paragraph](/Develop/Assets/Images/Content_Template-Paragragh_Text.png) | ![Table](/Develop/Assets/Images/Content_Template-Table_Text.png) |

## 次の項目も参照してください。
* [CardList](/Develop/References/ContentTemplates/CardList.md)
* [ImageList](/Develop/References/ContentTemplates/ImageList.md)
* [ImageText](/Develop/References/ContentTemplates/ImageText.md)
* [Popup](/Develop/References/ContentTemplates/Popup.md)
