# フルスクリーン表示のためのデザインガイド {#FullScreenDesignGuides}

ディスプレイを持つクライアントデバイスでコンテンツを表示する場合には、ドメインごとに使用を推奨する[コンテンツテンプレート](/Develop/References/Content_Templates.md)が決められています。また、テキストの推奨フォントファミリーが指定されています。

ここでは、テレビなどのディスプレイ装置でフルスクリーン表示を行う場合のフォントやレイアウトについて説明します。

* [フォント](#fonts)
* [推奨するコンテンツテンプレート](#ContentTemplates)
* [適用例](#ApplicationsByDomains)

<div class="note">
  <p><strong>メモ</strong></p>
  <p>フルスクリーン表示のためのデザインガイドの内容は、必須ではなく推奨です。詳細なレイアウトルールが必要な場合は、Clova事務局までお問い合わせください。</p>
</div>

## フォント {#fonts}

推奨するフォントファミリーは次のとおりです。

| ウェイト   | 英数字・記号     | 日本語  |
| ---------- | ---------------- | ------- |
| Regular    | Calibre Regular  | 新ゴ M  |
| Bold       | Calibre Medium   | 新ゴ DB |
| Extra bold | Calibre Semibold | 新ゴ DB |

## 推奨するコンテンツテンプレート {#ContentTemplates}

コンテンツテンプレートは、CICから送信されるコンテンツ情報をカテゴリごとに定型化したものです。フルスクリーン表示を行う場合は、ドメインごと推奨するコンテンツテンプレートが指定されています。コンテンツテンプレートとドメインの組み合わせの例は次のとおりです。

| コンテンツテンプレート名     | 使用を推奨するドメイン                                       |
| ---------------------------- | ------------------------------------------------------------ |
| [Text](#Text)                | <ul><li>Answering Engine</li><li>Calendar</li><li>Chat</li><li>Train info</li><li>Translation</li></ul> |
| [ImageText](#ImageText)      | <ul><li>Answering Engine</li><li>Fortune</li></ul>           |
| [CardList](#CardList)        | <ul><li>News</li><li>Schedule</li><li>Briefing</li><li>Memo</li></ul> |
| [Weather](#Weather)          | Weather                                                      |
| [Media Player](#MediaPlayer) | <ul><li>Music</li><li>Radio</li><li>Picture book</li><li>Song</li><li>Sound Effect</li></ul> |
| [Plain](#Plain)              | その他のドメイン                                             |

## 適用例 {#ApplicationsByDomains}

コンテンツテンプレートの適用例を

* [Text](#Text)
* [ImageText](#ImageText)
* [CardList](#CardList)
* [Weather](#Weather)
* [Media Player](#MediaPlayer)
* [Plain](#Plain)

### Text {#Text}

Textテンプレートは、画面に表示するテキストデータを提供するテンプレートです。フルスクリーン表示を行う場合は、Answering Engine、Calendar、Chat、Train info、Translationなどのドメインで使用されます。

| ノーマルモード | ダークモード |
| -------------- | ------------ |
| ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Text_Light.png) | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Text_Dark.png) |

<div class="warning">
  <p><strong>注意</strong></p>
  <p>ResponseにWikipediaのロゴが含まれる場合は、ロゴの表示は必須です。</p>
</div>

#### 次の項目も参照してください。
* [コンテンツテンプレート](/Develop/References/Content_Templates.md)
* [Text](/Develop/References/ContentTemplates/Text.md)

### ImageText {#ImageText}

ImageTextテンプレートは、画面に表示する画像とテキストデータを一緒に提供するテンプレートです。フルスクリーン表示を行う場合は、Answering Engine、Fortuneなどのドメインで使用されます。

| ノーマルモード | ダークモード |
| -------------- | ------------ |
| ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_ImageText_Light.png) | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_ImageText_Dark.png) |

<div class="warning">
  <p><strong>注意</strong></p>
  <p>ResponseにWikipediaのロゴが含まれる場合は、ロゴの表示は必須です。</p>
</div>

#### 次の項目も参照してください。
* [コンテンツテンプレート](/Develop/References/Content_Templates.md)
* [ImageText](/Develop/References/ContentTemplates/ImageText.md)


### CardList {#CardList}

CardListテンプレートは、画面にカードリスト形式で表現されるデータを定型化したテンプレートです。フルスクリーン表示を行う場合は、News、Schedule、Briefing、Memoなどのドメインで使用されます。

| ノーマルモード | ダークモード |
| -------------- | ------------ |
| ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_CardList_Light.png) | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_CardList_Dark.png) |

#### 次の項目も参照してください。
* [コンテンツテンプレート](/Develop/References/Content_Templates.md)
* [CardList](/Develop/References/ContentTemplates/CardList.md)

### Weather {#Weather}

Weatherテンプレートは、天気情報を提供するテンプレートです。現在の天気（[TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)）、1日の天気予報（[TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)）、1週間の天気予報（[WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)）の適用例は次のとおりです。

| ノーマルモード | ダークモード |
| -------------- | ------------ |
| 現在の天気     | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Current_Weather_Light.png) | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Current_Weather_Dark.png) |
| 1日の予報 | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Daily_Forcast_Light.png) | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Daily_Forcast_Dark.png) |
| 複数日の予報 | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Multi_Day_Forcast_Light.png) | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Multi_Day_Forcast_Dark.png) |

#### 次の項目も参照してください。
* [コンテンツテンプレート](/Develop/References/Content_Templates.md)
* [TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)
* [TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)
* [WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)

### Media Player {#MediaPlayer}

Media Playerは、音楽などの音声コンテンツと画像、メディアの操作インターフェースを一緒に提供するテンプレートです。
Music、Radio、Picture book、Song、Sound Effectなどのドメインで使用されます。

| ノーマルモード | ダークモード |
| -------------- | ------------ |
| ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Media_Player_Light.png) | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Media_Player_Dark.png) |

#### 次の項目も参照してください。
* [コンテンツテンプレート](/Develop/References/Content_Templates.md)
* [TemplateRuntime](/Develop/References/MessageInterfaces/TemplateRuntime.md)

### Plain {#Plain}

Plainテンプレートは、前述のドメインに当てはまらない場合や、特別な画面表示が必要ない場合に使用されるテンプレートです。

| ノーマルモード | ダークモード |
| -------------- | ------------ |
| ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Plain_Light.png) | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Plain_Dark.png) |

#### 次の項目も参照してください。
* [コンテンツテンプレート](/Develop/References/Content_Templates.md)
