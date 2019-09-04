# Clova認証API
クライアントがCICに接続するには、[Clovaアクセストークンを作成する](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)必要があります。Clova認可サーバーは、Clovaアクセストークンの作成および管理に必要なClova認証APIを提供しています。ここでは、Clova認証APIについて説明しています。

## ベースURI
Clova認可サーバーのベースURIは、次のとおりです。

<pre><code>{{ book.ServiceEnv.AuthServerBaseURI }}
</code></pre>

## 認可コードをリクエストする {#RequestAuthorizationCode}

{{ book.ServiceEnv.TargetServiceForClientAuth }}アカウントの{{ "authorization code" if book.L10N.TargetCountryCode == "JP" else "access token" }}および[クライアント資格情報](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)などをパラメーターに渡して、認可コードをリクエストします。認可コードは、Clovaアクセストークンを取得する際に使用されます。

通常、ユーザー認証は、クライアントデバイスとペアリングされたアプリで行われます。ですが、ペアリングされたアプリからクライアントにClovaアクセストークンを送信するのはセキュリティ上の問題があるため、代わりにこのコードをクライアントに渡します。クライアントは、渡された認可コードをClova認可サーバーに送信して、[Clovaアクセストークンをリクエスト](#RequestClovaAccessToken)します。

### Endpoint

```
GET|POST /authorize
```

### Request header

| リクエストヘッダー | 説明                                                                |
|----------------|--------------------------------------------------------------------|
| Accept         | <p>次の値を入力します。</p><p><pre><code>application/json</code></pre></p>  |
| Authorization  | <p><a href="/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken">取得した{{ book.ServiceEnv.TargetServiceForClientAuth }}の{{ "authorization code" if book.L10N.TargetCountryCode == "JP" else "access token" }}</a>を入力します：</p><p><pre><code>Bearer [{{ book.ServiceEnv.TargetServiceForClientAuth }} {{ "authorization code" if book.L10N.TargetCountryCode == "JP" else "access token" }}]</code></pre></p>  |

### Query parameter

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `client_id`     | string  | クライアントID（[クライアント資格情報](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)を参照）          |  |
| `device_id`     | string  | クライアントデバイスのMACアドレスまたは生成したUUID                                                              |  |
{% if book.L10N.TargetCountryCode == "JP"  -%}
| `grant_type`    | string  | `uauth_auth_code_v2`を入力します。  |   |
{% endif -%}
| `model_id`      | string  | クライアントデバイスのモデルID                                                                          |  |
| `response_type` | string  | 応答タイプ現在、`"code"`のみサポートされています。                                                             |  |
| `state`         | string  | クロスサイトリクエストフォージェリ（cross-site request forgery）攻撃を防ぐために、クライアントによって使用されるステータストークンの値（URIエンコーディングを使用する） |  |

### Request example

<pre><code>$ curl -H "Authorization: Bearer Zc3d3QAR6zIxqceOpXoq"
       {{ book.ServiceEnv.AuthServerBaseURL }}authorize \
       --data-urlencode "client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ" \
       {% if book.L10N.TargetCountryCode == "JP"  -%}
       --data-urlencode "grant_type=uauth_auth_code_v2" \
       {% endif -%}
       --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
       --data-urlencode "model_id=test_model" \
       --data-urlencode "response_type=code" \
       --data-urlencode "state=FKjaJfMlakjdfTVbES5ccZ"
</code></pre>

### Response header

| レスポンスヘッダー | 説明                                                                |
|-----------------|--------------------------------------------------------------------|
| Content-Type    | <p>次の値が含まれます。</p><p><pre><code>application/json</code></pre></p>                   |

### Response JSON object fields

| フィールド名       | データ型    | 説明                     | 包含 |
|---------------|---------|-----------------------------|:---------:|
| `code`          | string | 認可サーバーから取得した認可コード。HTTPレスポンスメッセージが`200`または`451`のステータスコードを持つ場合、HTTPレスポンスのボディーに含まれています。      |       |
| `redirect_uri`  | string | サービスの利用規約を提供するページのURI。HTTPレスポンスメッセージが`451 Unavailable For Legal Reasons`のステータスコードを持つ場合、HTTPレスポンスメッセージのボディに含まれています。クライアントは、このフィールドに含まれたURIにリダイレクトして、ページを表示する必要があります。ユーザーが利用規約に同意すると、クライアントは、`302 Found`（URL redirection）のステータスコードを持つレスポンスを、次のURLと共に受信します。<ul><li><code>clova://agreement-success</code>：ユーザーが利用規約に同意した場合。クライアントは、Clovaアクセストークンを取得するために、次のステップに進みます。</li><li><code>clova://agreement-failure?error=[reason]</code>：ユーザーが利用規約に同意していないか、またはサーバーのエラーにより、利用規約に同意できなかったことを示します。クライアントは、適切な例外処理を行う必要があります。`error`パラメータで次の値が渡されることがあります。<ul><li><code>"server_error"</code>：内部サーバーのエラー</li><li><code>"terms_not_agreed"</code>：ユーザーが利用規約の必須項目に同意していない</li><li><code>"user-disagreement"</code>：ユーザーが利用規約に同意していない</li></ul><div class="tip"><p><strong>ヒント</strong></p><p>このフィールドに記載されたURI（利用規約への同意ページ）にリダイレクトするとき、<code>redirect_uri</code>パラメータを追加できます。<code>redirect_uri</code>を追加すると、利用規約ページの応答が以下の形になります。</p><ul><li><code>https://[redirect_uri]?code=[code]&state=[state]</code>：ユーザーが利用規約に同意した場合。</li><li><code>https://[redirect_uri]?code=[code]&state=[state]&error=[reason]</code>：ユーザーが利用規約に同意していないか、またはサーバーのエラーにより、利用規約に同意できなかった場合。</li></ul></div> | 条件付き      |
| `state`         | string | クロスサイトリクエストフォージェリ（cross-site request forgery）攻撃を防ぐために、クライアントから受け取ったステータストークンを復号化した値（URLデコーディングを適用）。HTTPレスポンスメッセージが`200`または`451`のステータスコードを持つ場合、HTTPレスポンスのボディーに含まれています。 |       |

### Status codes

| ステータスコード       | 説明                                  |
|---------------|--------------------------------------|
| 200 OK           | リクエスト成功を示します。                      |
| 400 Bad Request  | `client_id`などの必須パラメーターを入力しなかったか、パラメーターに無効なデータを入力したことを示します。 |
| 403 Forbidden    | ヘッダーに含まれた{{ book.ServiceEnv.TargetServiceForClientAuth }}アクセストークンが無効なことを示します。 |
| 423 Locked       | Clovaを退会してから1か月が経過していないユーザーの{{ book.ServiceEnv.TargetServiceForClientAuth }}アカウントで認証しようとしたことを示します。ユーザーはClovaサービスを退会すると、そのアカウントでは1か月間、Clovaサービスを利用できません。<div class="note"><p><strong>メモ</strong></p><p>クライアントが<code>423 Locked</code>ステータスコードを受信した場合、「Clovaサービスを退会したアカウントです。退会してから1か月後に再度ログインすると、Clovaサービスをご利用になれます」と案内する必要があります。</p></div>  |
| 451 Unavailable For Legal Reasons | ユーザーが利用規約に同意しなかったことを示します。クライアントはこのレスポンスを受信すると、`redirect_uri`フィールド内のURIにリダイレクトする必要があります。リダイレクトするURIは、ユーザーに利用規約への同意を求めるページです。  |
| 500 Server Internal Error | サーバー内部にエラーが発生し、認可コードを取得できなかったことを示します。 |

### Response example

{% raw %}

```json
//サンプル1：HTTPレスポンスメッセージが200 OKのステータスコードを持つサンプル
{
    "code": "cnl__eCSTdsdlkjfweyuxXvnlA",
    "state": "FKjaJfMlakjdfTVbES5ccZ"
}

//サンプル2：HTTPレスポンスメッセージが451 Unavailable For Legal Reasonsのステータスコードを持つサンプル
{
  "code":"4mrklvwoC_KNgDlvmslka",
  "redirect_uri":"https://example.net/clova/terms_3rd.html?code=4mrklvwoC_KNgDlvmslka&grant_type=code&state=FKjaJfMlakjdfTVbES5ccZ",
  "state":"FKjaJfMlakjdfTVbES5ccZ"
}
```

{% endraw %}

{% include "/Develop/References/ClovaAuthAPI/Guest_Mode.md" %}

### 次の項目も参照してください。
* [クライアント認証情報](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)
* [Clovaアクセストークンを作成する](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)
* [Clovaアクセストークンをリクエストする](#RequestClovaAccessToken)


## Clovaアクセストークンをリクエストする {#RequestClovaAccessToken}

[取得した認可コード](#RequestAuthorizationCode)を使って、Clova認可サーバーにClovaアクセストークンをリクエストします。

### Endpoint

```
GET|POST /token?grant_type=authorization_code
```

### Request header

| リクエストヘッダー | 説明                                                                |
|----------------|--------------------------------------------------------------------|
| Accept         | <p>次の値を入力します。</p><p><pre><code>application/json</code></pre></p>                   |

### Query parameter

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `client_id`     | string  | クライアントID（[クライアント資格情報](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)を参照）                                  |  |
| `client_secret` | string  | クライアントシークレット（[クライアント資格情報](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)を参照）                              |  |
| `code`          | string  | [取得した認可コード](#RequestAuthorizationCode)                                                               |  |
| `device_id`     | string  | クライアントデバイスのMACアドレスまたは生成したUUID                                                                                     |  |
| `model_id`      | string  | クライアントデバイスのモデルID                                                                                                 |  |

### Request example

<pre><code>$ curl {{ book.ServiceEnv.AuthServerBaseURI }}token?grant_type=authorization_code \
       --data-urlencode "client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ" \
       --data-urlencode "client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D" \
       --data-urlencode "code=cnl__eCSTdsdlkjfweyuxXvnlA" \
       --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
       --data-urlencode "model_id=test_model"
</code></pre>

### Response header

| レスポンスヘッダー | 説明                                                                |
|-----------------|--------------------------------------------------------------------|
| Content-Type    | <p>次の値が含まれます。</p><p><pre><code>application/json</code></pre></p>                   |

### Response JSON object fields

| フィールド名       | データ型    | 説明                     | 包含 |
|---------------|---------|-----------------------------|:---------:|
| `access_token`  | string  | Clovaアクセストークン                               |       |
| `expires_in`    | number  | Clovaアクセストークンの有効期限（秒単位）              |       |
| `refresh_token` | string  | Clovaアクセストークンを更新するためのリフレッシュトークン     |       |
| `token_type`    | string  | Clovaアクセストークンのタイプ。「Bearer」で固定されています。 |       |

### Status codes

| ステータスコード       | 説明                                  |
|---------------|--------------------------------------|
| 200 OK        | リクエスト成功を示します。                                                                                   |
| 400 Bad Request  | `client_id`などの必須パラメーターを入力しなかったか、パラメーターに無効なデータを入力したことを示します。 |
| 500 Internal Server Error | サーバー内部にエラーが発生し、アクセストークンを取得できなかったことを示します。                                             |

### Response example
{% raw %}
```json
{
    "access_token": "XHapQasdfsdfFsdfasdflQQ7w",
    "expires_in": 332000,
    "refresh_token": "GW-Ipsdfasdfdfs3IbHFBA",
    "token_type": "Bearer"
}
```
{% endraw %}

### 次の項目も参照してください。
* [クライアント認証情報](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)
* [Clovaアクセストークンを作成する](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)
* [認可コードをリクエストする](#RequestAuthorizationCode)


## Clovaアクセストークンを更新する {#RefreshClovaAccessToken}

取得したリフレッシュトークンを使って、Clovaアクセストークンを更新します。

### Endpoint

```
GET|POST /token?grant_type=refresh_token
```

### Request header

| リクエストヘッダー | 説明                                                                |
|----------------|--------------------------------------------------------------------|
| Accept         | <p>次の値を入力します。</p><p><pre><code>application/json</code></pre></p>                   |

### Query parameter

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `client_id`     | string  | クライアントID（[クライアント資格情報](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)を参照）                                  |  |
| `client_secret` | string  | クライアントシークレット（[クライアント資格情報](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)を参照）                              |  |
| `device_id`     | string  | クライアントデバイスのMACアドレスまたは生成したUUID                                            | 任意 |
| `model_id`      | string  | クライアントデバイスのモデル                                                           |  |
| `refresh_token` | string  | 認証に成功して取得したリフレッシュトークン                                              |  |

### Request example

<pre><code>$ curl {{ book.ServiceEnv.AuthServerBaseURI }}token?grant_type=refresh_token \
       --data-urlencode "client_id=c2Rmc2Rmc2FkZ2FzZnNhZGZ" \
       --data-urlencode "client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D" \
       --data-urlencode "refresh_token=GW-Ipsdfasdfdfs3IbHFBA" \
       --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
       --data-urlencode "model_id=test_model"
</code></pre>

### Response header

| レスポンスヘッダー | 説明                                                                |
|-----------------|--------------------------------------------------------------------|
| Content-Type    | <p>次の値が含まれます。</p><p><pre><code>application/json</code></pre></p>                   |

### Response JSON object fields

| フィールド名       | データ型    | 説明                     | 包含 |
|---------------|---------|-----------------------------|:---------:|
| `access_token`  | string  | Clovaアクセストークン                               |       |
| `expires_in`    | number  | Clovaアクセストークンの有効期限（秒単位）              |       |
| `refresh_token` | string  | Clovaアクセストークンを更新するためのリフレッシュトークン     |       |
| `token_type`    | string  | Clovaアクセストークンのタイプ。「Bearer」で固定されています。 |       |

### Status codes

| ステータスコード       | 説明                                  |
|---------------|--------------------------------------|
| 200 OK                    | リクエスト成功を示します。                                                                                |
| 400 Bad Request           | `client_id`などの必須パラメーターを入力しなかったか、パラメーターに無効なデータを入力したことを示します。 |
| 500 Internal Server Error | サーバー内部にエラーが発生し、アクセストークンを更新できなかったことを示します。                                                      |

### Response example
{% raw %}
```json
{
    "access_token": "xFcH08vYQcahQWouqIzWOw",
    "expires_in": 12960000,
    "refresh_token": "drJK-soIQI6vqEukqsLU2g",
    "token_type": "Bearer"
}
```
{% endraw %}

### 次の項目も参照してください。
* [クライアント認証情報](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)
* [Clovaアクセストークンをリクエストする](#RequestClovaAccessToken)


## Clovaアクセストークンを削除する {#DeleteClovaAccessToken}

[取得したClovaアクセストークン](#RequestClovaAccessToken)を削除します。削除したClovaアクセストークンの情報がレスポンスとして返されます。

### Endpoint

```
GET|POST /token?grant_type=delete
```

### Request header

| リクエストヘッダー | 説明                                                                |
|----------------|--------------------------------------------------------------------|
| Accept         | <p>次の値を入力します。</p><p><pre><code>application/json</code></pre></p>                   |

### Query parameter

| フィールド名       | データ型    | 説明                     | 必須/任意 |
|---------------|---------|-----------------------------|:---------:|
| `access_token`  | string  | 認証に成功して取得したClovaアクセストークン                                                                                 |  |
| `client_id`     | string  | クライアントID（[クライアント資格情報](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)を参照）                                  |  |
| `client_secret` | string  | クライアントシークレット（[クライアント資格情報](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)を参照）                              |  |
| `device_id`     | string  | クライアントデバイスのMACアドレスまたは生成したUUID                                            |  |
| `model_id`      | string  | クライアントデバイスのモデル                                                           |  |

### Request example

<pre><code>$ curl {{ book.ServiceEnv.AuthServerBaseURI }}token?grant_type=delete \
       --data-urlencode "access_token=xFcH08vYQcahQWouqIzWOw" \
       --data-urlencode "client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ" \
       --data-urlencode "client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D" \
       --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
       --data-urlencode "model_id=test_model"
</code></pre>

### Response header

| レスポンスヘッダー | 説明                                                                |
|-----------------|--------------------------------------------------------------------|
| Content-Type    | <p>次の値が含まれます。</p><p><pre><code>application/json</code></pre></p>                   |

### Response JSON object fields

| フィールド名       | データ型    | 説明                     | 包含 |
|---------------|---------|-----------------------------|:---------:|
| `access_token`  | string  | 削除したClovaアクセストークン                               |       |
| `client_id`     | string  | クライアントID                                       |       |
| `expires_in`    | number  | 削除したClovaアクセストークンが持っていた有効期限（秒単位）              |       |

### Status codes

| ステータスコード       | 説明                                  |
|---------------|--------------------------------------|
| 200 OK                    | リクエスト成功を示します。                                                                                                                       |
| 400 Bad Request           | `client_id`などの必須パラメーターを入力しなかったか、パラメーターに無効なデータを入力したことを示します。                                        |
| 401 Unauthorized          | 無効なクライアント資格情報（`client_id`もしくは`client_secret`）、またはユーザー情報（`device_id`もしくは`model_id`）をパラメーターで渡したことを示します。 |
| 500 Internal Server Error | サーバー内部にエラーが発生し、アクセストークンを削除できなかったことを示します。                                                                                             |

### Response example
{% raw %}
```json
{
    "access_token": "xFcH08vYQcahQWouqIzWOw",
    "expires_in": 12960000,
    "client_id": "c2Rmc2Rmc2FkZ2FzZnNhZGZ"
}
```
{% endraw %}

### 次の項目も参照してください。
* [クライアント認証情報](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)
* [Clovaアクセストークンをリクエストする](#RequestClovaAccessToken)
