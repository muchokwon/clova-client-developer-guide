# CardListテンプレート
画面にカードリスト形式で表現されるデータを定型化したテンプレートです。CardListには、以下のカードタイプがあります。カードのタイプによって、`card`オブジェクトの有効なフィールドが異なる場合があります。

| カードのタイプ      | 説明                             | 有効なフィールド（`card`オブジェクト）                           |
|--------------|------------------------------------|-----------------------------------------------|
| `Type1` | コンテンツの画像、タイトル、説明を表示するカードタイプです。      | `description`、`imageUrl`、`referenceText`、`referenceUrl`、`title`             |
| `Type2` | コンテンツの画像、タイトル、説明、リンク先を表示するカードタイプです。 | `description`、`imageUrl`、`linkUrl`、`referenceText`、`referenceUrl`、`title`  |
| `Type3` | ビデオコンテンツを表示するカードタイプです。                 | `imageUrl`、`referenceText`、`referenceUrl`、`title`、`videoUrl`                |
| `Type4` | ニュースコンテンツを表示するカードタイプです。                  | `description`、`press`、`pressIconUrl`、`publishDate`、`title`                  |
| `Type5` | オーディオコンテンツを表示するカードタイプです。                 | `description`、`imageUrl`、`title`、`videoUrl`                                  |
| `Type6` | メディアのサムネイルを表示するカードタイプです。                   | `imageUrl`、`linkUrl`                                                      |

<div class="tip">
<p><strong>ヒント</strong></p>
<p>タイプごとの表示形式については、<a href="#UIExample">UI example</a>を参照してください。</p>
</div>

## Template fields

| フィールド名       | データ型    | 説明                     |
|---------------|---------|-----------------------------|
| `cardList[]`                | object array | カードリストを表現するオブジェクト配列 |
| `cardList[].contentProviderText`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | コンテンツ提供元の情報を持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。  |
| `cardList[].description[]`    | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | コンテンツの説明を持つオブジェクト配列          |
| `cardList[].imageUrl`       | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)             | 表示する画像のURIを持つオブジェクト。カードのタイプによって、このオブジェクトの`value`フィールドは空文字列（`""`）を持つ場合があります。  |
| `cardList[].linkUrl`        | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)             | コンテンツのURIを持つオブジェクト。カードのタイプによって、このオブジェクトの`value`フィールドは空文字列（`""`）を持つ場合があります。         |
| `cardList[].press`          | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)       | 報道機関の名前を持つオブジェクト。カードのタイプによって、このオブジェクトの`value`フィールドは空文字列（`""`）を持つ場合があります。             |
| `cardList[].pressIconUrl`   | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)             | 報道機関のアイコンのURIを持つオブジェクト。カードのタイプによって、このオブジェクトの`value`フィールドは空文字列（`""`）を持つ場合があります。    |
| `cardList[].publishDate`    | [DateObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateObject)           | 記事の日付を持つオブジェクト。カードのタイプによって、このオブジェクトの`value`フィールドは空文字列（`""`）を持つ場合があります。            |
| `cardList[].referenceText`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)       | 参照したサービスの情報を持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。  |
| `cardList[].referenceUrl`   | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)             | 参照したサービスの利用結果ページのURIを持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。   |
| `cardList[].title`          | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)       | コンテンツのタイトルを持つオブジェクト             |
| `cardList[].videoUrl`       | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)             | 再生するビデオまたはオーディオのURIを持つオブジェクト。カードのタイプによって、このオブジェクトの`value`フィールドは空文字列（`""`）を持つ場合があります。    |
| `noticeLink`                | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)             | `noticeText`フィールドに含まれた内容に関連して、詳細ページや提供元のURIを持つオブジェクトこのオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。**このオブジェクトの`value`フィールドの値が空文字列（`""`）でない場合、必ず画面にリンクが表示される必要があります。**  |
| `noticeText`                | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)       | コンテンツの著作権または法的事項に関する内容を持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。**このオブジェクトの`value`フィールドの値が空文字列（`""`）でない場合、必ず画面に表示される必要があります。**  |
| `subType`                   | string  | カードのタイプを示します。次の4つのタイプが指定されます。<ul><li><code>Type1</code></li><li><code>Type2</code></li><li><code>Type3</code></li><li><code>Type4</code></li></ul><div class="note"><p><strong>メモ</strong></p><p>現在、<code>Type1</code>、<code>Type2</code>、<code>Type3</code>、<code>Type5</code>、<code>Type6</code>は、<strong>空文字列で表現されます</strong>。タイプを判断するとき、<code>card</code>オブジェクトのフィールド構成を確認する必要があります。</p></div>                                                    |
| `type`                      | string  | コンテンツテンプレートのタイプを示す値。`"CardList"`を持ちます。                                                                       |

## Template example

{% raw %}
```json
// Type1、Type2のサンプル：
// ユーザーのリクエスト：ホラー映画をおすすめして

{
  "subType": "",
  "type": "CardList",
  "cardList": [
    {
      "contentProviderText" : {
        "type" : "string",
        "value" : "映画"
      },
      "description": [
        {
          "type": "string",
          "value": "ホラー, スリラー"
        },
        {
          "type": "string",
          "value": "アーロン・エッカート, デヴィッド・マズーズ, カリス・ファン・ハウテン, カタリーナ・サンディーノ・モレーノ, キーア・オドネル, マット・ネイブル, ジョン・プルッチェロ, エムジェイ・アンソニー, カロリーナ・ヴィドラ, マーク・スティガー, トーマス・アラナ, ペトラ・シュプレッヒャー, マーク・ヘンリー, アシュリー・グリーン・エリザベス"
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": "https://movie.phinf.contentservice.example.net/20170410_12/1491786049305s4W0n_JPEG/movie_image.jpg?type=w640_2"
      },
      "linkUrl": {
        "type": "url",
        "value": "https://movie.contentservice.example.com/movie/bi/mi/basic.nhn?code=118965"
      },
      "press": {
        "type": "string",
        "value": ""
      },
      "publishDate": {
        "type": "date",
        "value": ""
      },
      "referenceText": {
        "type": "string",
        "value": "検索結果"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=+%ec%98%81%ed%99%94"
      },
      "title": {
        "type": "string",
        "value": "ドクター・エクソシスト"
      },
      "videoUrl": {
        "type": "url",
        "value": ""
      }
    },
    {
      "contentProviderText" : {
        "type" : "string",
        "value" : "映画"
      },
      "description": [
        {
          "type": "string",
          "value": "ホラー"
        },
        {
          "type": "string",
          "value": "マチルダ・アンナ・イングリッド・ルッツ, アレックス・ロー, ジョニー・ガレッキ, ヴィンセント・ドノフリオ, エイミー・ティーガーデン, ボニー・モーガン, ローラ・スレイド・ウィッジンズ, ザック・ローリグ, リジー・ブロシュレ"
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": "https://movie.phinf.contentservice.example.net/20170317_53/1489741954272MquSW_JPEG/movie_image.jpg?type=w640_2"
      },
      "linkUrl": {
        "type": "url",
        "value": "https://movie.contentservice.example.com/movie/bi/mi/basic.nhn?code=137909"
      },
      "press": {
        "type": "string",
        "value": ""
      },
      "publishDate": {
        "type": "date",
        "value": ""
      },
      "referenceText": {
        "type": "string",
        "value": "検索結果"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=+%ec%98%81%ed%99%94"
      },
      "title": {
        "type": "string",
        "value": "ザ・リング／リバース"
      },
      "videoUrl": {
        "type": "url",
        "value": ""
      }
    },
    ...
  ]
}

// Type3のサンプル
// ユーザーのリクエスト：サッカーの動画を見せて
{
  "subType": "",
  "type": "CardList",
  "cardList": [
    {
      "contentProviderText" : {
        "type" : "string",
        "value" : "テレビ"
      },
      "description": [
        {
          "type": "string",
          "value": "04:14"
        },
        {
          "type": "string",
          "value": ""
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": "https://hol.phinf.contentservice.example.net/00/587/820/58782072_0.jpg"
      },
      "linkUrl": {
        "type": "url",
        "value": ""
      },
      "press": {
        "type": "string",
        "value": ""
      },
      "publishDate": {
        "type": "date",
        "value": ""
      },
      "referenceText": {
        "type": "string",
        "value": "検索結果"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=%ec%b6%95%ea%b5%ac+%eb%8f%99%ec%98%81%ec%83%81+%eb%b3%b4%ec%97%ac%ec%a4%98"
      },
      "title": {
        "type": "string",
        "value": "[海外の反応] FIFA U-20ワールドカップ、日本vsイタリア、ネット上で「最悪のゲーム」 - 海外のネット上の反応"
      },
      "videoUrl": {
        "type": "url",
        "value": "https://m.tv.contentservice.example.com/v/1720910"
      }
    },
    {
      "contentProviderText" : {
        "type" : "string",
        "value" : "テレビ"
      },
      "description": [
        {
          "type": "string",
          "value": "6:31"
        },
        {
          "type": "string",
          "value": ""
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": "https://hol.phinf.contentservice.example.net/00/587/815/58781581_0.jpg"
      },
      "linkUrl": {
        "type": "url",
        "value": ""
      },
      "press": {
        "type": "string",
        "value": ""
      },
      "publishDate": {
        "type": "date",
        "value": ""
      },
      "referenceText": {
        "type": "string",
        "value": "検索結果"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=%ec%b6%95%ea%b5%ac+%eb%8f%99%ec%98%81%ec%83%81+%eb%b3%b4%ec%97%ac%ec%a4%98"
      },
      "title": {
        "type": "string",
        "value": "FAカップ決勝：アーセナルvsチェルシー、試合後のインタビュー"
      },
      "videoUrl": {
        "type": "url",
        "value": "https://m.sports.contentservice.example.com/video.nhn?id=310230"
      }
    },
    ...
  ]
}


// Type4のサンプル
// ユーザーのリクエスト：最新のニュースを見せて
{
  "subType": "Type4",
  "type": "CardList",
  "noticeText": {
    "type": "string",
    "value": "自動抽出による要約です。本文の重要な内容が抜けている可能性があります。"
  },
  "cardList": [
    {
      "contentProviderText" : {
        "type" : "string",
        "value" : "ニュース"
      },
      "description": [
        {
          "type": "string",
          "value": "来月、長年議論されていた「4大河川」の取水ダムが、遂に解放されます。ムン・ジェイン大統領は、22日、「4大河川事業」の監査を指示し、取水ダムを開放することを決定しました。これを機に、4大河川事業をめぐる論争が再び水面上に浮かび上がる模様です"
        },
        {
          "type": "string",
          "value": ""
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": ""
      },
      "linkUrl": {
        "type": "url",
        "value": "https://news.contentservice.example.com/main/read.nhn?mode=LSD&mid=sec&sid1=001&oid=025&aid=0002720454"
      },
      "press": {
        "type": "string",
        "value": "中央日報"
      },
      "publishDate": {
        "type": "date",
        "value": "2017-05-28"
      },
      "referenceText": {
        "type": "string",
        "value": ""
      },
      "referenceUrl": {
        "type": "url",
        "value": ""
      },
      "title": {
        "type": "string",
        "value": "[市民のマイク]「抹茶ラテと化した4大河川」の監査、市民の意見は?"
      },
      "videoUrl": {
        "type": "url",
        "value": ""
      }
    },
    {
      "contentProviderText" : {
        "type" : "string",
        "value" : "ニュース"
      },
      "description": [
        {
          "type": "string",
          "value": "5月の最後の週末、暑さも忘れさせる砂の作品（釜山=聯合ニュース）ジョ・ジョンホ記者 = 初夏の暑さが訪れた28日、海雲台砂祭りが開催された釜山海雲台海水浴場で、見物客らが幅25ｍ、高さ5ｍの大きな砂の作品を鑑賞しています。2017.5.28（全国総合=聯合ニュース）5月の最後の週末の28日、初夏の暑さが…"
        },
        {
          "type": "string",
          "value": ""
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": ""
      },
      "linkUrl": {
        "type": "url",
        "value": "https://news.contentservice.example.com/main/read.nhn?mode=LSD&mid=sec&sid1=001&oid=001&aid=0009297247"
      },
      "press": {
        "type": "string",
        "value": "聯合ニュース"
      },
      "publishDate": {
        "type": "date",
        "value": "2017-05-28"
      },
      "referenceText": {
        "type": "string",
        "value": ""
      },
      "referenceUrl": {
        "type": "url",
        "value": ""
      },
      "title": {
        "type": "string",
        "value": "端午祭りなど、全国で祭りの催し…初夏の暑さに避暑の行列"
      },
      "videoUrl": {
        "type": "url",
        "value": ""
      }
    },
    ...
  ]
}

// Type5のサンプル
// ユーザーのリクエスト：ASMRを聞かせて
{
  "cardList": [
    {
      "contentProviderText" : {
        "type" : "string",
        "value" : "ミュージック"
      },
      "description": [
        {
          "type": "string",
          "value": "7:25"
        },
        {
          "type": "string",
          "value": ""
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": "https://tvcast1.phinf.contentservice.example.net/20180105_40/rYaFz_1515134168871cxwhn_JPEG/1515134043644.jpg"
      },
      "linkUrl": {
        "type": "url",
        "value": ""
      },
      "press": {
        "type": "string",
        "value": ""
      },
      "publishDate": {
        "type": "date",
        "value": ""
      },
      "referenceText": {
        "type": "string",
        "value": "検索結果"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=asmr+%ec%b0%be%ea%b8%b0"
      },
      "title": {
        "type": "string",
        "value": "[<mark>ASMR</mark>] コーヒー一杯どうですか?"
      },
      "videoUrl": {
        "type": "url",
        "value": "https://m.tv.contentservice.example.com/v/2509121"
      }
    },
    {
      "contentProviderText" : {
        "type" : "string",
        "value" : "ミュージック"
      },
      "description": [
        {
          "type": "string",
          "value": "5:05"
        },
        {
          "type": "string",
          "value": ""
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": "https://tvcast2.phinf.contentservice.example.net/20180104_140/7QzKq_15150467287668gEkL_JPEG/1515046724731.jpg"
      },
      "linkUrl": {
        "type": "url",
        "value": ""
      },
      "press": {
        "type": "string",
        "value": ""
      },
      "publishDate": {
        "type": "date",
        "value": ""
      },
      "referenceText": {
        "type": "string",
        "value": "検索結果"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=asmr+%ec%b0%be%ea%b8%b0"
      },
      "title": {
        "type": "string",
        "value": "[<mark>ASMR</mark>] 「お湯が沸騰する音」の動画"
      },
      "videoUrl": {
        "type": "url",
        "value": "https://m.tv.contentservice.example.com/v/2503662"
      }
    },
    ...
  ],
  "meta": {
    "version": {
      "type": "string",
      "value": "v0.1"
    }
  },
  "subType": "",
  "type": "CardList"
}

```
{% endraw %}

## UI example {#UIExample}
以下は、{{ book.ServiceEnv.OrientedService }}が配布したClovaのモバイルアプリで、CardListテンプレートの内容を表したUIサンプルです。

| `Type1` | `Type2` |
|-------|-------|
| ![Type1](/Develop/Assets/Images/Content_Template-Content_Card_Type.png) | ![Type2](/Develop/Assets/Images/Content_Template-Content_Card_with_Link_Type.png) |

| `Type3` | `Type4` |
|-------|-------|
| ![Type3](/Develop/Assets/Images/Content_Template-Video_Card_Type.png) | ![Type4](/Develop/Assets/Images/Content_Template-News_Card_Type.png) |

<div class="tip">
<p><strong>ヒント</strong></p>
<p><code>Type5</code>、<code>Type6</code>が使用された画面のサンプルは、現在準備中です。</p>
</div>

## 次の項目も参照してください。
* [ImageList](/Develop/References/ContentTemplates/ImageList.md)
* [ImageText](/Develop/References/ContentTemplates/ImageText.md)
* [Popup](/Develop/References/ContentTemplates/Popup.md)
* [Text](/Develop/References/ContentTemplates/Text.md)
