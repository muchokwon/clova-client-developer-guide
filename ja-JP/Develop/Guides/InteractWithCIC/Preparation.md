## 準備事項 {#Preparation}
クライアントは、HTTP/2プロトコルでCICに接続します。CICに接続するには、次の項目をあらかじめ用意する必要があります。

* HTTP/2プロトコルで接続するための[ライブラリ](#RequiredLibrary)
* クライアントデバイスの[User-Agent文字列](#UserAgentString)
* Clovaアクセストークンを作成するための[クライアント認証情報](#ClientAuthInfo)


### 必要なライブラリ {#RequiredLibrary}
HTTP/2プロトコルで接続するために、クライアントを開発する際、次のライブラリを使用することを推奨します。

| 開発言語 | ライブラリ                            |
|---------|------------------------------------|
| C/C++   | [nghttp2](https://nghttp2.org/)、[libcurl](https://curl.haxx.se/libcurl/) |
| Java    | [OkHttp](https://square.github.io/okhttp/)、[Netty](https://netty.io/)、[Jetty](https://www.eclipse.org/jetty/) |


### User-Agent文字列 {#UserAgentString}

クライアントは、ClovaおよびClovaに関連するWebサービスを利用する際、User-Agent文字列をHTTPヘッダーに含める必要があります。クライアントは次の項目を処理するためにHTTP通信を使用し、User-Agent文字列はそのすべての状況で積極的に使用されます。

* [CICサーバーと通信](#ConnectToCIC)する
* アプリのWebViewでページを開く
* UIに必要な画像をダウンロードする
* メディアプレイヤーでオーディオストリームをリクエストする

<div class="note">
  <p><strong>メモ</strong></p>
  <p>HTTPヘッダーにUser-Agent文字列が含まれなかったり、ルールに合わないUser-Agent文字列を送信した場合、クライアントの接続やリクエストが拒否されることがあります。</p>
</div>

User-Agent文字列を作成する際、順守すべき文法（[BNF記法](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form)）は次の通りです。

```
letter     = "A" | "B" | "C" | "D" | "E" | "F" | "G" | "H" | "I" | "J" | "K" | "L" | "M"
           | "N" | "O" | "P" | "Q" | "R" | "S" | "T" | "U" | "V" | "W" | "X" | "Y" | "Z"
           | "a" | "b" | "c" | "d" | "e" | "f" | "g" | "h" | "i" | "j" | "k" | "l" | "m"
           | "n" | "o" | "p" | "q" | "r" | "s" | "t" | "u" | "v" | "w" | "x" | "y" | "z" ;
digit      = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" ;
whitespace = " " ;
number     = digit , { digit } ;
character  = letter | digit | "_" | "-" | ".";
word       = character , { character } ;
string     = word
           | word , { whitespace | word } , word ;
safe_char  = letter | digit | "_" ;
safe_word  = safe_char , { safe_char } ;

urlencoded_character = character | "%" ;
urlencoded_string    = urlencoded_character , { urlencoded_character } ;

slash               = "/" ;
slash_delimiter     = [ whitespace ] , slash , [ whitespace ] ;
semicolon           = ";" ;
semicolon_delimiter = [ whitespace ] , semicolon , [ whitespace ] ;

build      = safe_word
           | safe_word , ".", build ;
release    = safe_word
           | safe_word , ".", release ;
version    = number , ".", number , ".", "number"
           | number , ".", number , ".", "number" , "-" , release
           | number , ".", number , ".", "number" , "+" , build
           | number , ".", number , ".", "number" , "-" , release , "+" , build ;

Manufacturer_Identifier = string ;
Product_Identifier      = string ;
Firmware_Version        = version ;
OS_Information          = string ;
HW_Information          = string ;
property                = word , "=" , urlencoded_string ;
extra_information       = property , { semicolon_delimiter , property } ;

User_Agent = Manufacturer_Identifier , slash_delimiter ,
             Product_Identifier , slash_delimiter ,
             Firmware_Version ,
             [ whitespace ] , "(" ,
             OS_Information , semicolon_delimiter ,
             HW_Information ,
             [ semicolon_delimiter , extra_informations ] , ")" ;
```

次は、この記法によってUser-Agent文字列を作成したサンプルです。**ちなみに、`Firmware_Version`に対するバージョン番号を入力する際には、[セマンティックバージョニング](https://semver.org/)を使用する必要があります。**

```
MyCompanyName/MyProductName/1.0.1 (KindOfOS 0.9;CustomHardwareInfo;sampleKey1=sampleValue1;sampleKey2=sampleValue2)
MyOrganizationName/MyAppName/2.1.2-release (Android 7.0;SettopBox;target=KR;other=sample)
AbbreviationOfCompanyName/MyClient/0.8.2+build (Linux 7.0;Raspberry_pi;domain=Sample%40Clova;type=Test%20Type)
MCN/SampleClient/1.7.0 (SampleIoTOS 2.1;SmartHub)
```

<div class="note">
  <p><strong>メモ</strong></p>
  <p>User-Agent文字列は、今後Clova Developer Centerで登録することになります。CICのためのClova Developer Centerは現在開発中のため、User-Agent文字列の登録は提携担当者までお問い合わせください。</p>
</div>

### クライアントの認証情報 {#ClientAuthInfo}
ユーザーは、クライアントが{{ book.ServiceEnv.TargetServiceForClientAuth }}アカウントを認証した後、Clovaが提供するサービスを利用できます。クライアントは、ユーザーから入力された{{ book.ServiceEnv.TargetServiceForClientAuth }}のアカウント情報に基づいて、{{ book.ServiceEnv.TargetServiceForClientAuth }}アカウントのアクセストークンを取得します。このアクセストークンを再びClova認可サーバーに送信して、[Clovaアクセストークンを取得](#CreateClovaAccessToken)します。

その際、{{ book.ServiceEnv.TargetServiceForClientAuth }}アカウントのアクセストークンのみでなく、Clova Developer Centerから取得したクライアント資格情報も一緒に認可サーバーに送信する必要があります（[Clova認証API](/Develop/References/Clova_Auth_API.md)を使用します）。従って、Clova Developer Centerからクライアント資格情報をあらかじめ取得しておく必要があります。クライアント資格情報は次の通りです。

| 資格情報                   | 説明                                              |
|---------------------------|--------------------------------------------------|
| `CLOVA_OAUTH_CLIENT_ID`     | Clova Developer Centerに登録したクライアントID         |
| `CLOVA_OAUTH_CLIENT_SECRET` | Clova Developer Centerから取得したクライアントシークレット |

<div class="note">
  <p><strong>メモ</strong></p>
  <p>クライアントのためのCIC - Clova Developer Centerは現在開発中です。クライアント資格情報は、Clova事務局から取得する必要があります。</p>
</div>
