# ImageText Template
화면에 표시해야 할 이미지와 텍스트 데이터를 함께 제공하는 템플릿입니다. 주로 인물 정보, 책 정보, 드라마 정보를 표시할 때 사용됩니다.

<div class="tip">
<p><strong>Tip!</strong></p>
<p>ImageText 템플릿의 표시 형태는 <a href="#UIExample">UI example</a>을 참조합니다.</p>
</div>

## Template fields

| 필드 이름       | 자료형    | 필드 설명                     |
|---------------|---------|-----------------------------|
| `appLinkUrl`     | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)             | 지도 이미지가 포함되었을 때 {{ book.ServiceEnv.OrientedService }} 지도 앱으로 이동하는 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `imageUrl`       | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)             | 이미지의 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.                                |
| `linkUrl`        | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)             | 지도 이미지가 포함되었을 때 웹 지도로 이동하는 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.   |
| `mainText`       | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)       | 메인 문구가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.                                       |
| `referenceText`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)       | 참조한 서비스의 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.  |
| `referenceUrl`   | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)             | 참조한 서비스의 이용 결과 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.   |
| `subTextList[]`    | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) array | 보조 문구가 담긴 배열. 이 객체 배열 요소의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.                               |
| `thumbImageUrl`  | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject)             | 썸네일 이미지의 URI 정보가 담긴 객체. 이 객체의 `value` 필드는 빈 문자열(`""`)을 가질 수도 있습니다.                           |
| `thumbImageType` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject)       | 썸네일 이미지의 유형 정보가 담긴 객체이며, 다음과 같은 값을 가집니다. <ul><li><code>"인물"</code>: 인물 정보 타입</li><li><code>"책"</code>: 정보 타입</li><li><code>"앨범"</code>: 음반 앨범 정보 타입</li><li>빈문자열(<code>""</code>): 썸네일 정보가 없음</li></ul> |
| `type`           | string  | Content template 구분자. `"ImageText"` 값을 가집니다.      |

## Template example

{% tabs example1="인물 정보", example2="책 정보", example3="드라마 정보" %}

{% content "example1" %}
```json
// 예제 1.
// 사용자 요청: 박보검이 누구야? (인물 정보)

{
  "type": "ImageText",
  "actionList": [{
    "type": "action",
    "value": ""
  }],
  "appLinkUrl": {
    "type": "url",
    "value": ""
  },
  "display": {
    "leftImageUrl": {
      "type": "url",
      "value": "https://ssl.example.net/sstatic/people/portrait/201908/20190828135041569.jpg"
    },
    "leftMainTextA": {
      "type": "string",
      "value": "박보검"
    },
    "leftMainTextB": {
      "type": "string",
      "value": ""
    },
    "leftSubTextList": [],
    "listItems": [],
    "referenceText": {
      "type": "string",
      "value": "네이버 검색결과"
    },
    "referenceUrl": {
      "type": "url",
      "value": "https://m.search.example.com/search.example?where\u003dm\u0026sm\u003dmob_lic\u0026query\u003d%EB%B0%95%EB%B3%B4%EA%B2%80%20%ED%94%84%EB%A1%9C%ED%95%84"
    },
    "rightMainTextA": {
      "type": "string",
      "value": "블러썸 엔터테인먼트"
    },
    "rightMainTextB": {
      "type": "string",
      "value": "탤런트, 영화배우"
    },
    "rightSubTextList": [{
      "type": "string",
      "value": "27세 1993년 6월 16일"
    }, {
      "type": "string",
      "value": ""
    }, {
      "type": "string",
      "value": ""
    }]
  },
  "imageUrl": {
    "type": "url",
    "value": ""
  },
  "linkUrl": {
    "type": "url",
    "value": ""
  },
  "mainText": {
    "type": "string",
    "value": "박보검"
  },
  "referenceText": {
    "type": "string",
    "value": "네이버 검색결과"
  },
  "referenceUrl": {
    "type": "url",
    "value": "https://m.search.example.com/search.example?where\u003dm\u0026sm\u003dmob_lic\u0026query\u003d%EB%B0%95%EB%B3%B4%EA%B2%80%20%ED%94%84%EB%A1%9C%ED%95%84"
  },
  "subText": {
    "type": "string",
    "value": ""
  },
  "subTextList": [{
    "type": "string",
    "value": "탤런트, 영화배우"
  }, {
    "type": "string",
    "value": "1993년 6월 16일"
  }],
  "thumbImageType": {
    "type": "string",
    "value": "인물"
  },
  "thumbImageUrl": {
    "type": "url",
    "value": "https://ssl.example.net/sstatic/people/portrait/201908/20190828135041569.jpg"
  }
}
```
{% content "example2" %}
```json
// 예제 2.
// 달팽이 식당이 뭐야? (책 정보)

{
  "type": "ImageText",
  "actionList": [{
    "type": "action",
    "value": ""
  }],
  "appLinkUrl": {
    "type": "url",
    "value": ""
  },
  "imageUrl": {
    "type": "url",
    "value": "https://s.example.net/movie.phinf/20160802_267/1470106362241IL294_JPEG/movie_image.jpg?type\u003dw640_2"
  },
  "linkUrl": {
    "type": "url",
    "value": ""
  },
  "mainText": {
    "type": "string",
    "value": "달팽이 식당"
  },
  "referenceText": {
    "type": "string",
    "value": "네이버 검색결과"
  },
  "referenceUrl": {
    "type": "url",
    "value": "https://m.search.example.com/search.example?where\u003dm\u0026sm\u003dmob_lic\u0026query\u003d%EB%8B%AC%ED%8C%BD%EC%9D%B4%20%EC%8B%9D%EB%8B%B9"
  },
  "subText": {
    "type": "string",
    "value": ""
  },
  "subTextList": [{
    "type": "string",
    "value": "드라마"
  }, {
    "type": "string",
    "value": ""
  }, {
    "type": "string",
    "value": "도미나가 마이"
  }, {
    "type": "string",
    "value": "시바사키 코우, 요 키미코, 에나미 쿄코, 미츠시마 히카리, 미우라 토모카즈, 시다 미라이, 타키모토 미오리, 다나카 테츠시, 브라더 톰, 아데이토"
  }],
  "thumbImageType": {
    "type": "string",
    "value": "인물"
  },
  "thumbImageUrl": {
    "type": "url",
    "value": "https://s.example.net/movie.phinf/20160802_267/1470106362241IL294_JPEG/movie_image.jpg?type\u003dw640_2"
  }
}
```
{% content "example3" %}
```json
// 예제 3.
// 눈이 부시게가 뭐야? (드라마 정보)

{
  "type": "ImageText",
  "actionList": [{
    "type": "action",
    "value": ""
  }],
  "appLinkUrl": {
    "type": "url",
    "value": ""
  },
  "imageUrl": {
    "type": "url",
    "value": ""
  },
  "linkUrl": {
    "type": "url",
    "value": ""
  },
  "mainText": {
    "type": "string",
    "value": "눈이 부시게"
  },
  "referenceText": {
    "type": "string",
    "value": "네이버 검색결과"
  },
  "referenceUrl": {
    "type": "url",
    "value": "https://m.search.naver.com/search.naver?where\u003dm\u0026sm\u003dmob_lic\u0026query\u003d%EB%88%88%EC%9D%B4%20%EB%B6%80%EC%8B%9C%EA%B2%8C"
  },
  "subText": {
    "type": "string",
    "value": ""
  },
  "subTextList": [{
    "type": "string",
    "value": "드라마"
  }, {
    "type": "string",
    "value": "JTBC 2019.02.11 ~ 2019.03.19"
  }, {
    "type": "string",
    "value": "김혜자, 한지민, 남주혁, 손호준, 안내상, 이정은, 김희원, 김가은, 송상은, 정영숙, 황정민"
  }, {
    "type": "string",
    "value": "닐슨코리아 9.7%"
  }],
  "thumbImageType": {
    "type": "string",
    "value": "인물"
  },
  "thumbImageUrl": {
    "type": "url",
    "value": "http://sstatic.naver.net/keypage/image/dss/57/74/31/50/57_8743150_poster_image_1548036460716.jpg"
  }
}
```
{% endtabs %}

## UI example {#UIExample}
다음은 {{ book.ServiceEnv.OrientedService }}가 배포한 모바일용 Clova 앱에서 ImageText 템플릿의 내용을 표현한 UI 예제입니다.

{% tabs people="인물 정보", book="책 정보", drama="드라마 정보" %}

{% content "people" %}
![Type1](/Develop/Assets/Images/Content_Template-ImageText-People.png)

{% content "book" %}
![Type2](/Develop/Assets/Images/Content_Template-ImageText-Book.png)

{% content "drama" %}
![Type3](/Develop/Assets/Images/Content_Template-ImageText-Drama.png)

{% endtabs %}

## See also
* [CardList](/Develop/References/ContentTemplates/CardList.md)
* [ImageList](/Develop/References/ContentTemplates/ImageList.md)
* [Popup](/Develop/References/ContentTemplates/Popup.md)
* [Text](/Develop/References/ContentTemplates/Text.md)
