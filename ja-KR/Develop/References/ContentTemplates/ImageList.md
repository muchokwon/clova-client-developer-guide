# ImageListテンプレート
画面に1つ以上の画像と、画像ごとの説明を提供するテンプレートです。サムネイルのリストを表示したり、ユーザーがサムネイルを選択したとき、大きな画像を表示したりするときに使用されます。

<div class="tip">
<p><strong>ヒント</strong></p>
<p>ImageListテンプレートの表示形式については、<a href="#UIExample">UI example</a>を参照してください。</p>
</div>

## Template fields

| フィールド名       | データ型    | 説明                     |
|---------------|---------|-----------------------------|
| `thumbImageUrlList[]`                | object array | 画像のリストを表すオブジェクト配列                        |
| `thumbImageUrlList[].imageReference` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 画像の提供元の情報を持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。      |
| `thumbImageUrlList[].imageTitle`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 画像のタイトルを持つオブジェクトこのオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。           |
| `thumbImageUrlList[].imageUrl`       | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 画像のURLがあるオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。      |
| `thumbImageUrlList[].referenceText`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | 参照したサービスの情報を持つオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。  |
| `thumbImageUrlList[].referenceUrl`   | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | 参照したサービスの利用結果ページのURLがあるオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。   |
| `thumbImageUrlList[].thumbImageUrl`  | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)       | サムネイル画像のURLがあるオブジェクト。このオブジェクトの`value`フィールドは、空文字列（`""`）を持つ場合があります。      |
| `type`                       | string       | コンテンツテンプレートのタイプを示す値。`"ImageList"`を持ちます。        |

## Template example

{% raw %}
```json
// サンプル1.
// ユーザーのリクエスト：車の写真を見せて
{
  "type": "ImageList",
  "thumbImageUrlList": [
    {
      "imageReference": {
        "type": "string",
        "value": ""
      },
      "imageTitle": {
        "type": "string",
        "value": "チャンウォン運転代行サービス、疾風のごとく"
      },
      "imageUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m_image&mode=default&query=%EC%9E%90%EB%8F%99%EC%B0%A8%20%EC%9D%B4%EB%AF%B8%EC%A7%80#imgId=post7533909_3"
      },
      "referenceText": {
        "type": "string",
        "value": "検索結果"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=%ec%9e%90%eb%8f%99%ec%b0%a8+%ec%82%ac%ec%a7%84+%eb%b3%b4%ec%97%ac%ec%a4%98"
      },
      "thumbImageUrl": {
        "type": "url",
        "value": "https://search.pstatic.example.net/common/?src=http%3A%2F%2Fpost.phinf.contentservice.example.net%2FMjAxNzA1MDZfMTg4%2FMDAxNDk0MDYyNDAwMDY3.C6LJCKXrha2u8dIqOOX0RhQNGrVVfkp3WbLO8U-xzRwg.IEYdykQp6xguEy4bnQ83JhDy1QZOtO4n1Lx5MBwivFwg.JPEG%2FIz2FmvAaRVzSf2Z-sNWzYQVU5z6Q.jpg&type=b360"
      }
    },
    {
      "imageReference": {
        "type": "string",
        "value": ""
      },
      "imageTitle": {
        "type": "string",
        "value": "車"
      },
      "imageUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m_image&mode=default&query=%EC%9E%90%EB%8F%99%EC%B0%A8%20%EC%9D%B4%EB%AF%B8%EC%A7%80#imgId=gallery2004021016070294818_1"
      },
      "referenceText": {
        "type": "string",
        "value": "検索結果"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=%ec%9e%90%eb%8f%99%ec%b0%a8+%ec%82%ac%ec%a7%84+%eb%b3%b4%ec%97%ac%ec%a4%98"
      },
      "thumbImageUrl": {
        "type": "url",
        "value": "https://search.pstatic.example.net/common/?src=http%3A%2F%2Fthumb.photo.contentservice.example.net%2Fdata15%2Fgallery%2F2004-02%2F10%2F07%2F18m2948m0.jpg&type=b360"
      }
    },
    ...
  ]
}

```
{% endraw %}

## UI example {#UIExample}
以下は、{{ book.ServiceEnv.OrientedService }}が配布したClovaのモバイルアプリで、ImageListテンプレートの内容を表したUIサンプルです。

| サムネイルのリスト | 選択した画像の表示 |
|-------|-------|
| ![Thumbnail](/Develop/Assets/Images/Content_Template-Thumbnail_List.png) | ![Original](/Develop/Assets/Images/Content_Template-Original_Image.png) |

## 次の項目も参照してください。
* [CardList](/Develop/References/ContentTemplates/ImageList.md)
* [ImageText](/Develop/References/ContentTemplates/ImageText.md)
* [Popup](/Develop/References/ContentTemplates/Popup.md)
* [Text](/Develop/References/ContentTemplates/Text.md)
