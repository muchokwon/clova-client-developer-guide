# フルスクリーン表示のためのデザインガイド {#FullScreenDesignGuides}

ディスプレイを持つクライアントデバイスでコンテンツを表示する場合には、ドメインごとに使用を推奨する[デザインテンプレート](/Develop/References/Content_Templates.md)があります。また、テキストの推奨フォントファミリーが指定されています。

ここでは、テレビなどのディスプレイ装置でフルスクリーン表示を行う場合のフォントやレイアウトについて説明します。

* [フォント](#fonts)
* [デザインテンプレート](#DesignTemplates)
  - [Text](#Text)
  - [ImageText](#ImageText)
  - [CardList](#CardList)
  - [Weather](#Weather)
  - [Media Player](#MediaPlayer)
  - [Plain](#Plain)

<div class="note">
  <p><strong>メモ</strong></p>
  <p>フルスクリーン表示のためのデザインガイドの内容は、必須ではなく推奨です。詳細なレイアウトルールが必要な場合は、Clova事務局までお問い合わせください。</p>
</div>

## フォント {#fonts}

クライアントデバイスでテキストを表示する際の推奨フォントファミリーは次のとおりです。

| ウェイト   | 英数字・記号     | 日本語  |
| ---------- | ---------------- | ------- |
| Regular    | Calibre Regular  | 新ゴ M  |
| Bold       | Calibre Medium   | 新ゴ DB |
| Extra bold | Calibre Semibold | 新ゴ DB |

## デザインテンプレート {#DesignTemplates}

デザインテンプレートとは、CICから送信されるコンテンツ情報をカテゴリごとに定型化した[コンテンツテンプレート](/Develop/References/Content_Templates.md)と、コンテンツテンプレートのどのフィールドを使用してレイアウトするかをあらかじめルール化したものです。デザインテンプレートとドメインの組み合わせの例は次のとおりです。

| デザインテンプレート名       | 使用を推奨するドメイン                                       |
| ---------------------------- | ------------------------------------------------------------ |
| [Text](#Text)                | <ul><li>Answering Engine</li><li>Calendar</li><li>Chat</li><li>Train info</li><li>Translation</li></ul> |
| [ImageText](#ImageText)      | <ul><li>Answering Engine</li><li>Fortune</li></ul>           |
| [CardList](#CardList)        | <ul><li>News</li><li>Schedule</li><li>Briefing</li><li>Memo</li></ul> |
| [Weather](#Weather)          | Weather                                                      |
| [Media Player](#MediaPlayer) | <ul><li>Music</li><li>Radio</li><li>Picture book</li><li>Song</li><li>Sound Effect</li></ul> |
| [Plain](#Plain)              | その他のドメイン                                             |

以下は、各デザインテンプレートの適用例です。

### Text {#Text}

Textは、画面に表示するテキストデータを提供するデザインテンプレートです。Answering Engine、Calendar、Chat、Train info、Translationなどのドメインで使用されます。

| ノーマルモード | ダークモード |
| -------------- | ------------ |
| <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_Text_Light.png" width="450"> | <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_Text_Dark.png" width="450"> |

#### UI example

![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Text_Example.png)

| <!-- --> | フィールド名 | データ型 | 説明 |
| -------- | ------------ | -------- | ---- |
| 1        | `mainText`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | メインテキストを持つオブジェクト |
| 2        | `paragraphText` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | パラグラフのテキストを持つオブジェクト |
| 3        | `provider.logoUrl`または`provider.name` | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | <ul><li>`provider.logoUrl`：参照したサービスのロゴ画像のURIを持つオブジェクト</li><li>`provider.name`：参照したサービスの名称のテキストを持つオブジェクト</li></ul><div class="note"><p><strong>メモ</strong></p><p>レスポンスに<code>provider</code>情報が含まれる場合は、掲載は必須です。</p><p>画像とテキストのどちらを表示するかは、スキルによって異なります。</p></div> |

<div class="warning">
  <p><strong>注意</strong></p>
  <p>レスポンスにWikipediaのロゴ画像が含まれる場合は、ロゴの表示は必須です。</p>
</div>

#### 次の項目も参照してください。
* [コンテンツテンプレート](/Develop/References/Content_Templates.md)
* [Textテンプレート](/Develop/References/ContentTemplates/Text.md)

### ImageText {#ImageText}

ImageTextは、画面に表示する画像とテキストデータを一緒に提供するデザインテンプレートです。Answering Engine、Fortuneなどのドメインで使用されます。

| ノーマルモード | ダークモード |
| -------------- | ------------ |
| <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_ImageText_Light.png" width="450"> | <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_ImageText_Dark.png" width="450"> |

#### UI example

![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_ImageText_Example.png)

| <!-- --> | フィールド名 | データ型 | 説明 |
| -------- | ------------ | -------- | ---- |
| 1        | `mainText`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | メインテキストを持つオブジェクト |
| 2        | `paragraphText` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | パラグラフのテキストを持つオブジェクト |
| 3        | `provider.logoUrl`または`provider.name` | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | <ul><li>`provider.logoUrl`：参照したサービスのロゴ画像のURIを持つオブジェクト</li><li>`provider.name`：参照したサービスの名称のテキストを持つオブジェクト</li></ul><div class="note"><p><strong>メモ</strong></p><p>レスポンスに<code>provider</code>情報が含まれる場合は、掲載は必須です。</p><p>画像とテキストのどちらを表示するかは、スキルによって異なります。</p></div> |
| 4        | `imageUrl`       | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 画像のURIを持つオブジェクト|

<div class="warning">
  <p><strong>注意</strong></p>
  <p>レスポンスにWikipediaのロゴ画像が含まれる場合は、ロゴの表示は必須です。</p>
</div>

#### 次の項目も参照してください。
* [コンテンツテンプレート](/Develop/References/Content_Templates.md)
* [ImageTextテンプレート](/Develop/References/ContentTemplates/ImageText.md)

### CardList {#CardList}

CardListは、画面にカードリスト形式で表現されるデータを定型化したデザインテンプレートです。News、Schedule、Briefing、Memoなどのドメインで使用されます。

| ノーマルモード | ダークモード |
| -------------- | ------------ |
| <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_CardList_Light.png" width="450"> | <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_CardList_Dark.png" width="450"> |

#### UI example

![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_CardList_Example.png)

CardListデザインテンプレートは、次の2種類のコンテンツテンプレートに対応しています。

1. [CardListテンプレート](/Develop/References/ContentTemplates/CardList.md) カードタイプ:Type 2

| <!-- --> | フィールド名 | データ型 | 説明 |
| -------- | ------------ | -------- | ---- |
| 1        | `mainText`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | メインテキストを持つオブジェクト |
| 2        | `paragraphText` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | パラグラフのテキストを持つオブジェクト |
| 3        | `provider.logoUrl`または`provider.name` | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | <ul><li>`provider.logoUrl`：参照したサービスのロゴ画像のURIを持つオブジェクト</li><li>`provider.name`：参照したサービスの名称のテキストを持つオブジェクト</li></ul><div class="note"><p><strong>メモ</strong></p><p>レスポンスに<code>provider</code>情報が含まれる場合は、掲載は必須です。</p><p>画像とテキストのどちらを表示するかは、スキルによって異なります。</p></div> |
| 4        | `imageUrl`       | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 画像のURIを持つオブジェクト|

2. [ScheduleList](/Develop/References/ContentTemplates/ScheduleList.md)

| <!-- --> | フィールド名 | データ型 | 説明 |
| -------- | ------------ | -------- | ---- |
| 1        | `mainText`   | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | メインテキストを持つオブジェクト |
| 2        | `paragraphText` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | パラグラフのテキストを持つオブジェクト |
| 3        | `provider.logoUrl`または`provider.name` | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | <ul><li>`provider.logoUrl`：参照したサービスのロゴ画像のURIを持つオブジェクト</li><li>`provider.name`：参照したサービスの名称のテキストを持つオブジェクト</li></ul><div class="note"><p><strong>メモ</strong></p><p>レスポンスに<code>provider</code>情報が含まれる場合は、掲載は必須です。</p><p>画像とテキストのどちらを表示するかは、スキルによって異なります。</p></div> |
| 4        | `imageUrl`       | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 画像のURIを持つオブジェクト|

#### 次の項目も参照してください。
* [コンテンツテンプレート](/Develop/References/Content_Templates.md)
* [CardListテンプレート](/Develop/References/ContentTemplates/CardList.md)

### Weather {#Weather}

Weatherテンプレートは、天気情報を提供するテンプレートです。現在の天気（[TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)）、1日の天気予報（[TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)）、1週間の天気予報（[WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)）の適用例は次のとおりです。

| <!-- --> | ノーマルモード | ダークモード |
| -------- | -------------- | ------------ |
| 現在の天気     | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Current_Weather_Light.png) | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Current_Weather_Dark.png) |
| 1日の予報 | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Daily_Forcast_Light.png) | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Daily_Forcast_Dark.png) |
| 複数日の予報 | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Multi_Day_Forcast_Light.png) | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Multi_Day_Forcast_Dark.png) |

#### 次の項目も参照してください。
* [コンテンツテンプレート](/Develop/References/Content_Templates.md)
* [TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)
* [TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)
* [WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)

### Media Player {#MediaPlayer}

Media Playerは、音楽などの音声コンテンツと画像、操作インターフェースを一緒に提供するデザインテンプレートです。
Music、Radio、Picture book、Song、Sound Effectなどのドメインで使用されます。

| ノーマルモード | ダークモード |
| -------------- | ------------ |
| ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Media_Player_Light.png) | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Media_Player_Dark.png) |

| ノーマルモード | ダークモード |
| -------------- | ------------ |
| <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_Text_Light.png" width="450"> | <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_Text_Dark.png" width="450"> |

#### 次の項目も参照してください。
* [コンテンツテンプレート](/Develop/References/Content_Templates.md)
* [TemplateRuntime](/Develop/References/MessageInterfaces/TemplateRuntime.md)

### Plain {#Plain}

Plainテンプレートは、前述のドメインに当てはまらない場合や、特別な画面表示が必要ない場合に使用されるデザインテンプレートです。

| ノーマルモード | ダークモード |
| -------------- | ------------ |
| <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_Plain_Light.png" width="450"> | <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_Plain_Dark.png" width="450"> |

#### 次の項目も参照してください。
* [コンテンツテンプレート](/Develop/References/Content_Templates.md)
