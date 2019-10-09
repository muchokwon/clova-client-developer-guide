# フルスクリーン表示のためのデザインガイド

ディスプレイを持つクライアントデバイスでコンテンツを表示する場合には、推奨フォントと、ドメインごとに使用を推奨する[デザインテンプレート](#DesignTemplates)が指定されています。

ここでは、テレビなどのディスプレイ装置でフルスクリーン表示を行う場合のデザインについて説明します。

* [フォント](#fonts)
* [デザインテンプレート](#DesignTemplates)
  - [Text](#Text)
  - [ImageText](#ImageText)
  - [CardList](#CardList)
  - [Weather](#Weather)
  - [Media Player](#MediaPlayer)
  - [Plain](#Plain)
* [Clovaを呼び出したときの表示](#WakeClova)

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

デザインテンプレートとは、CICから渡されるコンテンツ情報を定型化した[コンテンツテンプレート](/Develop/References/Content_Templates.md)と、コンテンツテンプレートのどのオブジェクトを使用するかをあらかじめルール化したものです。デザインテンプレートとドメインの主な組み合わせの例は次のとおりです。

| デザインテンプレート         | ドメイン                                           |
| ---------------------------- | -------------------------------------------------- |
| [Text](#Text)                | <ul><li>Answering Engine</li><li>Calendar</li><li>Chat</li><li>Train info</li><li>Translation</li></ul> |
| [ImageText](#ImageText)      | <ul><li>Answering Engine</li><li>Fortune</li></ul> |
| [CardList](#CardList)        | <ul><li>News</li><li>Schedule</li><li>Briefing</li><li>Memo</li></ul> |
| [Weather](#Weather)          | Weather                                            |
| [Media Player](#MediaPlayer) | <ul><li>Music</li><li>Radio</li><li>Picture book</li><li>Song</li><li>Sound Effect</li></ul> |
| [Plain](#Plain)              | その他のドメイン                                   |

以下は、各デザインテンプレートの適用例です。

### Text {#Text}

Textは、画面に表示するテキストデータを提供するデザインテンプレートです。主にAnswering Engine、Calendar、Chat、Train info、Translationなどのドメインで使用されます。

#### UI examples
| ノーマルモード | ダークモード |
| -------------- | ------------ |
| <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_Text_Light.png" width="450"> | <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_Text_Dark.png" width="450"> |

#### 使用するコンテンツテンプレート
![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Text_Example.png)

[Textテンプレート](/Develop/References/ContentTemplates/Text.md)

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

ImageTextは、画面に表示する画像とテキストデータを一緒に提供するデザインテンプレートです。主にAnswering Engine、Fortuneなどのドメインで使用されます。

#### UI examples

| ノーマルモード | ダークモード |
| -------------- | ------------ |
| <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_ImageText_Light.png" width="450"> | <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_ImageText_Dark.png" width="450"> |

#### 使用するコンテンツテンプレート

![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_ImageText_Example.png)

[ImageTextテンプレート](/Develop/References/ContentTemplates/ImageText.md)

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

#### UI examples

| ノーマルモード | ダークモード |
| -------------- | ------------ |
| <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_CardList_Light.png" width="450"> | <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_CardList_Dark.png" width="450"> |

#### 使用するコンテンツテンプレート

CardListデザインテンプレートは、次の2種類のコンテンツテンプレートに対応しています。

![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_CardList_Example.png)

- [CardListテンプレート](/Develop/References/ContentTemplates/CardList.md) Type2

| <!-- --> | フィールド名 | データ型 | 説明 |
| -------- | ------------ | -------- | ---- |
| 1        | `listTitle`  | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | コンテンツのタイトルを持つオブジェクト |
| 2        | `cardList[].contentProviderText` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | コンテンツ提供元の情報を持つオブジェクト |
| 3        | `cardList[].description[]` | [StringObject](/Develop/References/ContentTemplates/Shared_Objects.md#StringObject) | コンテンツの説明を持つオブジェクト配列 |
| 4        | `cardList[].imageUrl` | [URIObject](/Develop/References/ContentTemplates/Shared_Objects.md#URIObject) | 表示する画像のURIを持つオブジェクト |

- [ScheduleList](/Develop/References/ContentTemplates/ScheduleList.md)

<table>
	<tbody>
		<tr>
			<td><!-- --></td>
			<td>フィールド名</td>
			<td>データ型</td>
			<td>説明</td>
		</tr>
		<tr>
			<td>1</td>
			<td>（なし）</td>
			<td><!-- --></td>
			<td><!-- --></td>
		</tr>
		<tr>
			<td rowspan="4">2</td>
			<td><code>scheduleList[].start</code></td>
			<td><a href="/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject">DateTimeObject</a>または<a href="/Develop/References/ContentTemplates/Shared_Objects.md#DateObject">DateObject</a></td>
			<td>スケジュールが開始する日付と時間</td>
		</tr>
		<tr>
			<td><code>scheduleList[].end</code></td>
			<td><a href="/Develop/References/ContentTemplates/Shared_Objects.md#DateTimeObject">DateTimeObject</a>または<a href="/Develop/References/ContentTemplates/Shared_Objects.md#DateObject">DateObject</a></td>
			<td>スケジュールが終了する日付と時間</td>
		</tr>
		<tr>
			<td><code>scheduleList[].repeatDay[]</code></td>
			<td><a href="/Develop/References/ContentTemplates/Shared_Objects.md#StringObject">StringObject</a></td>
			<td>毎週繰り返しのスケジュールの場合、繰り返しの曜日を持つオブジェクト配列</td>
		</tr>
		<tr>
			<td><code>scheduleList[].repeatPeriod</code></td>
			<td><a href="/Develop/References/ContentTemplates/Shared_Objects.md#StringObject">StringObject</a></td>
			<td>繰り返しの周期を持つオブジェクト</td>
		</tr>
		<tr>
			<td>3</td>
			<td><code>scheduleList[].content</code></td>
			<td><a href="/Develop/References/ContentTemplates/Shared_Objects.md#StringObject">StringObject</a></td>
			<td>ユーザーが入力したスケジュールの内容を持つオブジェクト</td>
		</tr>
		<tr>
			<td>4</td>
			<td>（なし）</td>
			<td><!-- --></td>
			<td><!-- --></td>
		</tr>
	</tbody>
</table>

#### 次の項目も参照してください。
* [コンテンツテンプレート](/Develop/References/Content_Templates.md)
* [CardListテンプレート](/Develop/References/ContentTemplates/CardList.md)
* [ScheduleListテンプレート](/Develop/References/ContentTemplates/ScheduleList.md)

### Weather {#Weather}

Weatherテンプレートは、天気情報を提供するテンプレートです。現在の天気（[TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)）、1日の天気予報（[TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)）、1週間の天気予報（[WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)）などを表示することができます。

#### UI examples

| <!-- --> | ノーマルモード | ダークモード |
| -------- | -------------- | ------------ |
| 現在の天気     | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Current_Weather_Light.png) | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Current_Weather_Dark.png) |
| 1日の予報 | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Daily_Forcast_Light.png) | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Daily_Forcast_Dark.png) |
| 複数日の予報 | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Multi_Day_Forcast_Light.png) | ![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Multi_Day_Forcast_Dark.png) |


#### 使用するコンテンツテンプレート

Weatherデザインテンプレートは、次のコンテンツテンプレートに対応しています。

- [TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)

- [TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)



- [WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)


#### 次の項目も参照してください。
* [コンテンツテンプレート](/Develop/References/Content_Templates.md)
* [TodayWeather](/Develop/References/ContentTemplates/TodayWeather.md)
* [TomorrowWeather](/Develop/References/ContentTemplates/TomorrowWeather.md)
* [WeeklyWeather](/Develop/References/ContentTemplates/WeeklyWeather.md)

### Media Player {#MediaPlayer}

Media Playerは、音楽などのオーディオコンテンツと画像、操作インターフェースを一緒に提供するデザインテンプレートです。
主にMusic、Radio、Picture book、Song、Sound Effectなどのドメインで使用されます。

#### UI example

| ノーマルモード | ダークモード |
| -------------- | ------------ |
| <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_Media_Player_Light.png" width="450"> | <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_Media_Player_Dark.png" width="450"> |

#### UI example

![](/Design/Assets/Images/Clova-Client-Full_Screen_Template_Media_Player_Example.png)


#### 次の項目も参照してください。
* [コンテンツテンプレート](/Develop/References/Content_Templates.md)
* [TemplateRuntime](/Develop/References/MessageInterfaces/TemplateRuntime.md)

### Plain {#Plain}

Plainは、前述のドメインに当てはまらない場合や、特別な画面表示が必要ない場合に使用されるデザインテンプレートです。

#### UI example

| ノーマルモード | ダークモード |
| -------------- | ------------ |
| <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_Plain_Light.png" width="450"> | <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Template_Plain_Dark.png" width="450"> |

## Clovaを呼び出したときの表示 {#WakeClova}

フルスクリーン表示をしている際にClovaを呼び出した場合は、[Green Dot VUI](/Design/Screen.md#GreenDotVUI)や認識結果などを画面の下部に配置します。

#### UI example

| ノーマルモード | ダークモード |
| -------------- | ------------ |
| <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Wake_Clova_Light.png" width="450"> | <img src="/Design/Assets/Images/Clova-Client-Full_Screen_Wake_Clova_Dark.png" width="450"> |
