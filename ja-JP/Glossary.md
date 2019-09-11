<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

<!-- Start of the shared content: Glossary -->

# 用語および略語

<div class="note">
  <p><strong>メモ</strong></p>
  <p>このページは随時更新されます。</p>
</div>

### CEK
[Clova Extensions Kit](#CEK)の略語

### CIC
[Clova Interface Connect](#CIC)の略語

### CIC API {#CICAPI}
CICがクライアントに提供するREST APIです。クライアントは、CIC APIを使用してClovaと情報を交換します。

### Clova {#Clova}
[Clova](https://clova.line.me/)は、{{ book.ServiceEnv.OrientedService }}が開発およびサービスを提供しているAIプラットフォームです。ユーザーの音声やイメージを認識し、それを解析して、ユーザーの希望する情報やサービスを提供します。サードパーティの開発者は、Clovaの持つ技術を活用して、AIサービスを提供するデバイスまたは家電製品を制作できます。また、Clovaを利用して、保有しているコンテンツやサービスを提供することもできます。

### Clova Developer Center {#ClovaDeveloperConsole}
Clovaプラットフォームと連携するクライアントデバイス、または[Clova Extension](#ClovaExtension)を開発する開発者に次の内容を提供する<a target="_blank" href="{{ book.ServiceEnv.DeveloperConsoleURI }}">Webツール</a>です。
* クライアントデバイスを登録する
* クライアント資格情報を提供する

<div class="note">
  <p><strong>メモ</strong></p>
  <p>ClovaクライアントのためのClova Developer Centerは、現在開発中です。</p>
</div>

### Clova Extension {#ClovaExtension}
音楽、ショッピング、金融などの外部のサービス（サードパーティサービス）、または家庭のIoTデバイスの制御など、Clovaの機能を拡張して、ユーザーに様々な経験を提供するWebアプリケーションです。通常、Extensionと呼ばれます。Clovaプラットフォームは、現在次の2種類のClova Extensionをサポートおよび提供しています。エンドユーザーには、「スキル」という名称で提供されます。
* [Custom Extension](#CustomExtension)
* [Clova Home Extension](#ClovaHomeExtension)

### Clova Extensions Kit（CEK） {#CEK}
Clova Extensionを開発および配布する際に、必要なツールとインターフェースを提供するプラットフォームです。ClovaとExtensionのコミュニケーションをサポートします。

### Clova Home Extension {#ClovaHomeExtension}
IoTデバイス制御サービスを提供するための[Extension](#ClovaExtension)です。詳細については、[Clova Home Extensionを作成する]({{ book.DocMeta.ClovaHomeExtensionDeveloperGuideBaseURI }}/Develop/Guides/Build_Clova_Home_Extension.{{ book.DocMeta.FileExtensionForExternalLink}})ドキュメントを参照してください。

### Clova Interface Connect（CIC） {#CIC}
AIアシスタントサービスを提供するパソコン/モバイルアプリ、モバイルデバイスまたは家電製品などのクライアントに、Clovaとの連携ができるインターフェースを提供するプラットフォームです。詳細については、[CICの概要](/Develop/CIC_Overview.md)ドキュメントを参照してください。

### Clovaアクセストークン {#ClovaAccessToken}
クライアントが[Clova Interface Connect](#CIC)で[イベント](#Event)を送る際、Clovaがクライアントを認証する手段です。詳細については、[Clovaアクセストークンを生成する](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)ドキュメントを参照してください。

### Clovaアプリ {#ClovaApp}
{{ book.ServiceEnv.OrientedService }}が開発し、iOSおよびAndroidプラットフォームで配布するClovaアプリです。Clovaに指示を与えるだけでなく、クライアントデバイスを登録し、管理できるアプリです。

### Clova認証API {#ClovaAuthAPI}
クライアントが[Clovaアクセストークン](#ClovaAccessToken)を取得するために使用するAPIです。詳細については、[Clova認証API](/Develop/References/Clova_Auth_API.md)ドキュメントを参照してください。

### Custom Extension {#CustomExtension}
任意の拡張された機能を提供する[Extension](#ClovaExtension)です。Custom Extensionを利用すると、音楽、ショッピング、金融など、外部サービスの機能を提供できます。詳細については、[Clova Custom Extensionを作成する]({{ book.DocMeta.ClovaCustomExtensionDeveloperGuideBaseURI }}/Develop/Guides/Build_Custom_Extension.{{ book.DocMeta.FileExtensionForExternalLink}})ドキュメントを参照してください。

### Extension {#Extension}
[Clova Extension](#ClovaExtension)の別名

### HTTP/2 {#HTTP2}
HTTPのバージョンの1つです。[SPDY](https://en.wikipedia.org/wiki/SPDY)に基づき、インターネット技術タスクフォース（IETF）において開発されています。1997年にRFC 2068として規定されたHTTP/1.1をバージョンアップしたものであり、2014年12月にProposed Standard(標準への提唱)として制定され、2015年2月17日にIESGで正式な仕様として承認されました。2015年5月に<a href="https://tools.ietf.org/html/rfc7540" target="_blank">RFC 7540</a>として公開されました。

### OAuth 2.0
アクセス権限を委任するためのオープンスタンダードです。インターネットのユーザーが他のウェブサービスやアプリケーションのユーザーアカウントにアクセスできる権限を付与する規約です。Clovaプラットフォームでは、クライアントが[Clovaアクセストークン](#ClovaAccessToken)を取得したり、ユーザーが特定のExtensionを使用したりする際、自身のアカウントを連携するために使用されます。詳細については、[https://tools.ietf.org/html/rfc6749](https://tools.ietf.org/html/rfc6749)を参照してください。

### イベント {#Event}
クライアントから[Clova Interface Connect](#CIC)に渡すメッセージです。ユーザーのリクエストを渡したり、クライアントの状態が変更されたことを知らせたりするためにこのメッセージを送信します。

### クライアントの認証情報 {#ClientCredentialInfo}
[Clova Developer Center](#ClovaDeveloperConsole)でクライアントを登録し、取得した認証情報です。[Clovaアクセストークン](#ClovaAccessToken)の取得に使用されます。詳細については、[Clovaアクセストークンを作成する](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)ドキュメントを参照してください。

### コンテキスト {#Context}
クライアントの様々な状態を示す情報です。[コンテキストオブジェクト](#ContextObjects)として表現されます。詳細については、[コンテキスト](/Develop/References/Context_Objects.md)ドキュメントを参照してください。

### コンテキストオブジェクト {#ContextObjects}
クライアントの現在の[コンテキスト](#Context)を表現するオブジェクトです。詳細については、[コンテキスト](/Develop/References/Context_Objects.md)ドキュメントを参照してください。

### コンテンツテンプレート {#ContentTemplate}
CICから渡されるコンテンツ情報を定型化したフォーマットです。詳細については、[コンテンツテンプレート](/Develop/References/Content_Templates.md)ドキュメントを参照してください。

### ダイアログID {#DialogID}
ユーザーのリクエストを識別するために、ユーザーが発話を開始する度に作成される識別子です。ダイアログIDは、ユーザーが新しい発話を開始するたびに生成され、クライアントが[Recognize](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize)[イベント](#Event)を[Clova Interface Connect](#CIC)に渡す際に含まれます。ダイアログIDは、サーバー側からレスポンスを返す際に、どのイベントに対するレスポンスか結び付けるために使用され、[ディレクティブ](#Directive)にも含まれます。クライアントはディレクティブに含まれたダイアログIDから、どのイベントに対するレスポンスかを判断する必要があります。もしクライアントが現在持っているダイアログIDとディレクティブのダイアログIDが異なる場合、受信したディレクティブを無視する必要があります。詳細については、[ダイアログモデル](/Develop/Guides/Manage_Dialogue_ID_And_Handle_Tasks.md)ドキュメントを参照してください。

### ダウンチャネル {#Downchannel}
クライアントが[Clova Interface Connect](#CIC)からディレクティブを渡される際に使用される[HTTP/2](#HTTP2)ストリームです。詳細については、[CICに接続する](/Develop/Guides/Interact_with_CIC.md#ConnectToCIC)ドキュメントを参照してください。

### ディレクティブ {#Directive}
[Clova Interface Connect](#CIC)がクライアントのアクションを制御するように指定したメッセージです。ディレクティブは、クライアントがリクエストしたイベントに応答したり、特定の条件によってクライアントに情報を渡したりする際に使用されます。

### メッセージID {#MessageID}
個々のメッセージを区別するための識別子です。[イベント](#Event)と[ディレクティブ](#Directive)は、それぞれメッセージIDを持ちます。

<!-- End of the shared content -->
