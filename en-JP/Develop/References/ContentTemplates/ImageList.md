# ImageList Template
The ImageList template is used in providing a list of images plus additional information for the client to display on the client screen. This template is often used to display a list of thumbnails or the original image.

<div class="note">
<p><strong>Note!</strong></p>
<p>See a <a href="#UIExample">UI example</a> of the ImageList template used in display.</p>
</div>

## Template fields

| Field name       | Data type    | Description                     |
|---------------|---------|-----------------------------|
| `ImageList[]`                | object array | The object array that has the information on the image list.                        |
| `ImageList[].imageReference` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The name or description of the source of the image. The `value` field of this object can have an empty string (`""`).      |
| `ImageList[].imageTitle`     | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The title of the image. The `value` field of this object can have an empty string (`""`).           |
| `ImageList[].imageUrl`       | [URLObject](/Develop/References/ContentTemplates/Shared_Objects.md#URLObject)       | The URL of the image. The `value` field of this object can have an empty string (`""`).      |
| `ImageList[].referenceText`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | The information of the referred service. The `value` field of this object can have an empty string (`""`).  |
| `ImageList[].referenceUrl`   | [URLObject](/Develop/References/ContentTemplates/Shared_Objects.md#URLObject)       | The information on the usage result URL of the referred service. The `value` field of this object can have an empty string (`""`).   |
| `ImageList[].thumbImageUrl`  | [URLObject](/Develop/References/ContentTemplates/Shared_Objects.md#URLObject)       | The URL of the thumbnail image. The `value` field of this object can have an empty string (`""`).      |
| `type`                       | string       | The type of this template. The value is always `"ImageList"`.        |

## Template example

{% raw %}
```json
// Example 1.
// User request: Show me the pictures of cars
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
        "value": "Chauffeur at your service as fast as lightning"
      },
      "imageUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m_image&mode=default&query=%EC%9E%90%EB%8F%99%EC%B0%A8%20%EC%9D%B4%EB%AF%B8%EC%A7%80#imgId=post7533909_3"
      },
      "referenceText": {
        "type": "string",
        "value": "search result"
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
        "value": "Car"
      },
      "imageUrl": {
        "type": "url",
        "value": "https://m.search.contentservice.example.com/search?where=m_image&mode=default&query=%EC%9E%90%EB%8F%99%EC%B0%A8%20%EC%9D%B4%EB%AF%B8%EC%A7%80#imgId=gallery2004021016070294818_1"
      },
      "referenceText": {
        "type": "string",
        "value": "search result"
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
The following example shows how the ImageList template is used on the Clova app distributed by {{ book.ServiceEnv.OrientedService }}.

| Thumbnail list | Original image |
|-------|-------|
| ![Thumbnail](/Develop/Assets/Images/Content_Template-Thumbnail_List.png) | ![Original](/Develop/Assets/Images/Content_Template-Original_Image.png) |

## See also
* [CardList](/Develop/References/ContentTemplates/ImageList.md)
* [ImageText](/Develop/References/ContentTemplates/ImageText.md)
* [Popup](/Develop/References/ContentTemplates/Popup.md)
* [Text](/Develop/References/ContentTemplates/Text.md)
