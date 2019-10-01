# CardList Template
The Cardlist template has standardized the data to be displayed on the screen as card list type. CardList has the following card types. The valid field of `card` object may vary depending on each card type.

| Card type | Description                                         | Valid fields (`card` object)                                                              |
|---------|-------------------------------------------------|---------------------------------------------------------------------------------|
| `Type1` | Card that displays an image, title, and description.      | `description`, `imageUrl`, `referenceText`, `referenceUrl`, `title`             |
| `Type2` | Card that displays an image, title, description, and link. | `description`, `imageUrl`, `linkUrl`, `referenceText`, `referenceUrl`, `title`  |
| `Type3` | Card that displays video content.                 | `imageUrl`, `referenceText`, `referenceUrl`, `title`, `videoUrl`                |
| `Type4` | Card that displays news content.                  | `description`, `press`, `pressIconUrl`, `publishDate`, `title`                  |
| `Type5` | Card that displays audio content.                 | `description`, `imageUrl`, `title`, `videoUrl`                                  |
| `Type6` | Card that displays a media thumbnail.                   | `imageUrl`, `linkUrl`                                                      |

<div class="note">
<p><strong>Note!</strong></p>
<p>See a <a href="#UIExample">UI example</a> for each card type in display.</p>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `cardList[]`                | object array | The object array that has a list of information units to be displayed as cards. |
| `cardList[].contentProviderText`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information of the content provider. The `value` field of this object can have an empty string (`""`).  |
| `cardList[].description[]`    | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | The object array that has the information about the content this card is to display.                    |
| `cardList[].description[]`    | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | The object array that has the information about the content this card is to display.          |
| `cardList[].imageUrl`       | [URLObject](/Develop/References/ContentTemplates/Shared_Objects.md#URLObject)             | The URL of the image to display. The `value` field of this object may have an empty string (`""`) depending on the card type.  |
| `cardList[].linkUrl`        | [URLObject](/Develop/References/ContentTemplates/Shared_Objects.md#URLObject)             | The URL information of the content. The `value` field of this object may have an empty string (`""`) depending on the card type.         |
| `cardList[].press`          | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)       | The name of the news press. The `value` field of this object may have an empty string (`""`) depending on the card type.             |
| `cardList[].pressIconUrl`   | [URLObject](/Develop/References/ContentTemplates/Shared_Objects.md#URLObject)             | The URL to the news press icon. The `value` field of this object may have an empty string (`""`) depending on the card type.    |
| `cardList[].publishDate`    | [DateObject](/Develop/References/ContentTemplates/Shared_Objects.md#DateObject)           | The published date of the associated article. The `value` field of this object may have an empty string (`""`) depending on the card type.            |
| `cardList[].referenceText`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)       | The information of the referred service. The `value` field of this object can have an empty string (`""`).  |
| `cardList[].referenceUrl`   | [URLObject](/Develop/References/ContentTemplates/Shared_Objects.md#URLObject)             | The information on the usage result URL of the referred service. The `value` field of this object can have an empty string (`""`).   |
| `cardList[].title`          | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)       | The title of the content.             |
| `cardList[].videoUrl`       | [URLObject](/Develop/References/ContentTemplates/Shared_Objects.md#URLObject)             | The URL of the video or audio to play. The `value` field of this object may have an empty string (`""`) depending on the card type.    |
| `subType`                   | string  | The type of this card. Available types are: <ul><li><code>Type1</code></li><li><code>Type2</code></li><li><code>Type3</code></li><li><code>Type4</code></li></ul><div class="note"><p><strong>Note!</strong></p><p><code>Type1</code>, <code>Type2</code>, <code>Type3</code>, <code>Type5</code>, and <code>Type6</code> are displayed as an <strong>empty string</strong>. You must determine the type by checking the fields of the <code>card</code> object.</p></div>                                                    |
| `type`                      | string  | The type of this template. The value is always `"CardList"`.                                                                       |

## Template example

{% raw %}
```json
// Example for Type1 and Type2
// User request: Can you recommend a horror movie?

{
  "subType": "",
  "type": "CardList",
  "cardList": [
    {
      "contentProviderText" : {
        "type" : "string",
        "value": "movie"
      },
      "description": [
        {
          "type": "string",
          "value": "Horror, thriller"
        },
        {
          "type": "string",
          "value": "Aaron Eckhart, David Mazouz, Carice van Houten, Catalina Sandino Moreno, Keir O'Donnell, Matt Nable, John Pirruccello, Emjay Anthony, Karolina Wydra, Mark Steger, Tomas Arana, Petra Sprecher, Mark Henry, Ashley Green Elizabeth"
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": "http://movie.phinf.contentservice.example.net/20170410_12/1491786049305s4W0n_JPEG/movie_image.jpg?type=w640_2"
      },
      "linkUrl": {
        "type": "url",
        "value": "http://movie.contentservice.example.com/movie/bi/mi/basic.nhn?code=118965"
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
        "value": "search result"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=+%ec%98%81%ed%99%94"
      },
      "title": {
        "type": "string",
        "value": "Incarnate"
      },
      "videoUrl": {
        "type": "url",
        "value": ""
      }
    },
    {
      "contentProviderText" : {
        "type" : "string",
        "value": "movie"
      },
      "description": [
        {
          "type": "string",
          "value": "Horror"
        },
        {
          "type": "string",
          "value": "Matilda Anna Ingrid Lutz, Alex Roe, Johnny Galecki, Vincent D'Onofrio, Aimee Teegarden, Bonnie Morgan, Laura Wiggins, Zach Roerig, Lizzie Brochere"
        },
        {
          "type": "string",
          "value": ""
        }
      ],
      "imageUrl": {
        "type": "url",
        "value": "http://movie.phinf.contentservice.example.net/20170317_53/1489741954272MquSW_JPEG/movie_image.jpg?type=w640_2"
      },
      "linkUrl": {
        "type": "url",
        "value": "http://movie.contentservice.example.com/movie/bi/mi/basic.nhn?code=137909"
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
        "value": "search result"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=+%ec%98%81%ed%99%94"
      },
      "title": {
        "type": "string",
        "value": "Rings"
      },
      "videoUrl": {
        "type": "url",
        "value": ""
      }
    },
    ...
  ]
}

// Example for Type3
// User request: Show me a soccer video.
{
  "subType": "",
  "type": "CardList",
  "cardList": [
    {
      "contentProviderText" : {
        "type": "string",
        "value": "TV"
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
        "value": "http://hol.phinf.contentservice.example.net/00/587/820/58782072_0.jpg"
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
        "value": "search result"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=%ec%b6%95%ea%b5%ac+%eb%8f%99%ec%98%81%ec%83%81+%eb%b3%b4%ec%97%ac%ec%a4%98"
      },
      "title": {
        "type": "string",
        "value": "[Global reaction] U20 World Cup, Italy, Japan, International netizen\"The dirtiest match\" - Reaction of international netizens"
      },
      "videoUrl": {
        "type": "url",
        "value": "http://m.tv.contentservice.example.com/v/1720910"
      }
    },
    {
      "contentProviderText" : {
        "type": "string",
        "value": "TV"
      },
      "description": [
        {
          "type": "string",
          "value": "06:31"
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
        "value": "http://hol.phinf.contentservice.example.net/00/587/815/58781581_0.jpg"
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
        "value": "search result"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=%ec%b6%95%ea%b5%ac+%eb%8f%99%ec%98%81%ec%83%81+%eb%b3%b4%ec%97%ac%ec%a4%98"
      },
      "title": {
        "type": "string",
        "value": "FA cup finals: Arsenal v Chelsea post-match interview"
      },
      "videoUrl": {
        "type": "url",
        "value": "http://m.sports.contentservice.example.com/video.nhn?id=310230"
      }
    },
    ...
  ]
}


// Example for Type4
// User request: Show me the latest news.
{
  "subType": "Type4",
  "type": "CardList",
  "cardList": [
    {
      "contentProviderText" : {
        "type" : "string",
        "value": "news"
      },
      "description": [
        {
          "type": "string",
          "value": "The weir gates of the much-troubled Four Major Rivers Project will open next month. The decision was made as President Moon Jae-in ordered an audit on the Four Major Rivers Project on the 22nd. Through this opportunity, the Four Major Rivers Project controversy has surfaced again."
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
        "value": "http://news.contentservice.example.com/main/read.nhn?mode=LSD&mid=sec&sid1=001&oid=025&aid=0002720454"
      },
      "press": {
        "type": "string",
        "value": "Korea JoongAng Daily"
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
        "value": "[People Mic] The audit on the "Green Tea Latte" Four Major Rivers Projects. What do the people think?"
      },
      "videoUrl": {
        "type": "url",
        "value": ""
      }
    },
    {
      "contentProviderText" : {
        "type" : "string",
        "value": "news"
      },
      "description": [
        {
          "type": "string",
          "value": "Thousands more visit on the last weekend of May. Visitors cool down and enjoy the sand art (Busan=Yonhap News). Reporter Cho, Jeong-ho = With the arrival of early summer-like weather on 28th, visitors on the Haeundae beach are enjoying the sand festival and the 25 m wide and 5 m high sculpture. May 28, 2017 (National=Yonhap News). The weather on 28th, the last weekend of May, already feels like summer with..."
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
        "value": "http://news.contentservice.example.com/main/read.nhn?mode=LSD&mid=sec&sid1=001&oid=001&aid=0009297247"
      },
      "press": {
        "type": "string",
        "value": "Yonhap News"
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
        "value": "Festivals held throughout the country such as Dano Festival… Thousands leave for early vacation as summer weather hits the country"
      },
      "videoUrl": {
        "type": "url",
        "value": ""
      }
    },
    ...
  ]
}

// Example for Type5
// User request: Play the ASMR.
{
  "cardList": [
    {
      "contentProviderText" : {
        "type" : "string",
        "value": "music"
      },
      "description": [
        {
          "type": "string",
          "value": "07:25"
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
        "value": "http://tvcast1.phinf.contentservice.example.net/20180105_40/rYaFz_1515134168871cxwhn_JPEG/1515134043644.jpg"
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
        "value": "search result"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=asmr+%ec%b0%be%ea%b8%b0"
      },
      "title": {
        "type": "string",
        "value": "[<mark>ASMR</mark>] Do you want some coffee?"
      },
      "videoUrl": {
        "type": "url",
        "value": "http://m.tv.contentservice.example.com/v/2509121"
      }
    },
    {
      "contentProviderText" : {
        "type" : "string",
        "value": "music"
      },
      "description": [
        {
          "type": "string",
          "value": "05:05"
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
        "value": "http://tvcast2.phinf.contentservice.example.net/20180104_140/7QzKq_15150467287668gEkL_JPEG/1515046724731.jpg"
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
        "value": "search result"
      },
      "referenceUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=asmr+%ec%b0%be%ea%b8%b0"
      },
      "title": {
        "type": "string",
        "value": "[<mark>ASMR</mark>] Video of boiling water sound"
      },
      "videoUrl": {
        "type": "url",
        "value": "http://m.tv.contentservice.example.com/v/2503662"
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
The following examples show how each card type is used to present information on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

| `Type1` | `Type2` |
|-------|-------|
| ![Type1](/Develop/Assets/Images/Content_Template-Content_Card_Type.png) | ![Type2](/Develop/Assets/Images/Content_Template-Content_Card_with_Link_Type.png) |

| `Type3` | `Type4` |
|-------|-------|
| ![Type3](/Develop/Assets/Images/Content_Template-Video_Card_Type.png) | ![Type4](/Develop/Assets/Images/Content_Template-News_Card_Type.png) |

<div class="note">
<p><strong>Note!</strong></p>
<p>UI examples for <code>Type5</code> and <code>Type6</code> will be added soon.</p>
</div>

## See also
* [ImageList](/Develop/References/ContentTemplates/ImageList.md)
* [ImageText](/Develop/References/ContentTemplates/ImageText.md)
* [Popup](/Develop/References/ContentTemplates/Popup.md)
* [Text](/Develop/References/ContentTemplates/Text.md)
