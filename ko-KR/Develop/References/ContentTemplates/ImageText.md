# ImageText Template
화면에 표시해야 할 이미지와 텍스트 데이터를 함께 제공하는 템플릿입니다. 썸네일 이미지와 텍스트를 표시하거나 지도와 텍스트를 함께 표시할 때 사용됩니다.

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

{% raw %}

```json
// 예제 1.
// 사용자 요청: 리오넬 메시의 소속팀은? (썸네일 이미지와 텍스트 표시)
{
  "type": "ImageText",
  "imageUrl": {
    "type": "url",
    "value": ""
  },
  "mainText": {
    "type": "string",
    "value": "리오넬 메시"
  },
  "referenceText": {
    "type": "string",
    "value": "검색결과"
  },
  "referenceUrl": {
    "type": "url",
    "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=%eb%a6%ac%ec%98%a4%eb%84%ac+%eb%a9%94%ec%8b%9c+%ec%86%8c%ec%86%8d%ed%8c%80"
  },
  "subTextList": [
    {
      "type": "string",
      "value": "FC 바르셀로나"
    }
  ],
  "thumbImageType": {
    "type": "string",
    "value": "인물"
  },
  "thumbImageUrl": {
    "type": "url",
    "value": "https://sstatic.contentservice.example.net/people/3/201607071816066361.jpg"
  }
}

// 예제 2.
// 사용자 요청: 현재 위치 알려줘 (지도 이미지와 텍스트 표시)
{
  "appLinkUrl": {
    "type": "url",
    "value": "nmap://map?lat=37.3594589&lng=127.1047745&level=13&mode=1&traffic=false&bicycle=false&cadastral=false&appname=com.contentservice.clova"
  },
  "imageUrl": {
    "type": "url",
    "value": "https://simg.pstatic.example.net/static.map/image?caller=mw_search&crs=EPSG:4326&scale=2&format=jpg&dataversion=163.2&version=1.1&baselayer=default&center=127.1047745,37.3594589&markers=type,default2_s,127.1047745,37.3594589&level=10&h=402&w=515"
  },
  "linkUrl": {
    "type": "url",
    "value": "https://m.map.contentservice.example.com/map.nhn?lat=37.3594589&lng=127.1047745&dlevel=&mapMode=&pinTitle=&boundary=&traffic="
  },
  "mainText": {
    "type": "string",
    "value": "경기도 성남시 분당구 정자1동"
  },
  "meta": {
    "version": {
      "type": "string",
      "value": "v0.1"
    }
  },
  "referenceText": {
    "type": "string",
    "value": "검색결과"
  },
  "referenceUrl": {
    "type": "url",
    "value": "https://m.search.contentservice.example.com/search?where=m&sm=mob_lic&query=%ed%98%84%ec%9e%ac+%ec%9c%84%ec%b9%98"
  },
  "subTextList": [
    {
      "type": "string",
      "value": ""
    }
  ],
  "thumbImageType": {
    "type": "string",
    "value": ""
  },
  "thumbImageUrl": {
    "type": "url",
    "value": ""
  },
  "type": "ImageText"
}
```

{% endraw %}

## UI example {#UIExample}
다음은 {{ book.ServiceEnv.OrientedService }}가 배포한 모바일용 Clova 앱에서 ImageText 템플릿의 내용을 표현한 UI 예제입니다.

| 썸네일 이미지와 텍스트 | 지도 이미지와 텍스트 |
|-------|-------|
| ![Thumbnail](/Develop/Assets/Images/Content_Template-Thumbimage_and_Text.png) | ![Map and text](/Develop/Assets/Images/Content_Template-Mapimage_and_Text.png) |

## See also
* [CardList](/Develop/References/ContentTemplates/CardList.md)
* [ImageList](/Develop/References/ContentTemplates/ImageList.md)
* [Popup](/Develop/References/ContentTemplates/Popup.md)
* [Text](/Develop/References/ContentTemplates/Text.md)
