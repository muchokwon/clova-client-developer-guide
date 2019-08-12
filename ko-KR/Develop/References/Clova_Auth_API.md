# Clova 인증 API
클라이언트가 CIC에 연결하려면 [Clova access token을 생성](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)해야 합니다. Clova 인증 서버는 Clova access token 생성 및 관리에 필요한 Clova 인증 API를 제공하고 있으며, 여기에서는 Clova 인증 API에 대해 설명합니다.

## Base URI
Clova 인증 서버의 base URI는 다음과 같습니다.

<pre><code>{{ book.ServiceEnv.AuthServerBaseURI }}
</code></pre>

## Authorization code 요청 {#RequestAuthorizationCode}

{{ book.ServiceEnv.TargetServiceForClientAuth }} 계정 {{ "authorization code" if book.L10N.TargetCountryCode == "JP" else "access token" }} 및 [클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo) 등을 파라미터로 전달해 authorization code를 요청합니다. Authorization code는 Clova access token을 발급받을 때 사용됩니다.

일반적으로 사용자 인증을 받기 위해 클라이언트 기기와 페어링(Pairing)된 앱에서 인증을 처리합니다. 다만, 페어링된 앱에서 클라이언트 쪽으로 Clova access token을 전송하는 것은 보안상 이슈가 있기 때문에 이 코드를 대신 클라이언트로 보냅니다. 클라이언트는 전달받은 authorization code를 다시 Clova 인증 서버로 전달하여 [Clova access token을 요청](#RequestClovaAccessToken)해야 합니다.

### Endpoint

```
GET|POST /authorize
```

### Request header

| Request header | 설명                                                                |
|----------------|--------------------------------------------------------------------|
| Accept         | <p>다음 값을 입력합니다.</p><p><pre><code>application/json</code></pre></p>  |
| Authorization  | <p><a href="/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken">획득한 {{ book.ServiceEnv.TargetServiceForClientAuth }} {{ "authorization code" if book.L10N.TargetCountryCode == "JP" else "access token" }}</a>을 입력:</p><p><pre><code>Bearer [{{ book.ServiceEnv.TargetServiceForClientAuth }} {{ "authorization code" if book.L10N.TargetCountryCode == "JP" else "access token" }}]</code></pre></p>  |

### Query parameter

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `client_id`     | string  | 클라이언트 ID ([클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo) 참조)          | 필수 |
| `device_id`     | string  | 클라이언트 기기의 MAC 주소나 생성한 UUID                                                              | 필수 |
{% if book.L10N.TargetCountryCode == "JP"  -%}
| `grant_type`    | string  | `uauth_auth_code_v2`를 입력  | 필수  |
{% endif -%}
| `model_id`      | string  | 클라이언트 기기의 모델 ID                                                                          | 필수 |
| `response_type` | string  | 응답 유형. 현재 `"code"`만 지원합니다.                                                             | 필수 |
| `state`         | string  | 요청 위조(cross-site request forgery) 공격을 방지하기 위해 클라이언트에서 사용하는 상태 token 값(URI 인코딩 적용) | 필수 |

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

| Response header | 설명                                                                |
|-----------------|--------------------------------------------------------------------|
| Content-Type    | <p>다음 값을 포함하고 있습니다.</p><p><pre><code>application/json</code></pre></p>                   |

### Response JSON object fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `code`          | string | 인증 서버로부터 발급받은 authorization code. HTTP 응답 메시지가 `200`이나 `451`의 상태 코드를 가질 때 HTTP 응답 메시지 본문에 포함되는 필드입니다.      | 항상      |
| `redirect_uri`  | string | 서비스 이용 약관과 관련된 내용을 제공하는 페이지 URI. HTTP 응답 메시지가 `451 Unavailable For Legal Reasons`의 상태 코드를 가질 때 HTTP 응답 메시지 본문에 포함되는 필드입니다. 클라이언트는 이 필드에 포함된 URI로 이동하여 페이지를 표시해야 합니다. 사용자가 이용 약관에 동의하면 클라이언트는 `302 Found`(URL redirection) 상태 코드를 가진 응답을 다음 URL과 함께 수신하게 됩니다. <ul><li><code>clova://agreement-success</code>: 사용자가 이용 약관 동의를 완료함. 클라이언트는 Clova access token 발급을 위해 다음 단계를 계속 진행할 수 있습니다.</li><li><code>clova://agreement-failure?error=[reason]</code>: 사용자가 이용 약관에 동의하지 않았거나 서버 오류로 이용 약관 동의에 실패함. 클라이언트는 적절한 예외 처리를 해야 합니다. `error` 파라미터의 값으로 다음과 같은 값이 전달될 수 있습니다.<ul><li><code>"server_error"</code>: 내부 서버 오류</li><li><code>"terms_not_agreed"</code>: 필수 약관 동의가 수행되지 않음</li><li><code>"user-disagreement"</code>: 사용자가 약관에 동의하지 않음</li></ul><div class="tip"><p><strong>Tip!</strong></p><p>이 필드에 명시된 URI(약관 동의 페이지)로 페이지를 이동(redirect)할 때 <code>redirect_uri</code> 파라미터를 추가할 수 있습니다. <code>redirect_uri</code> 추가하면 이용 약관 페이지의 응답이 아래와 같은 형태를 가지게 됩니다.</p><ul><li><code>https://[redirect_uri]?code=[code]&state=[state]</code>: 사용자가 이용 약관 동의를 완료함.</li><li><code>https://[redirect_uri]?code=[code]&state=[state]&error=[reason]</code>: 사용자가 이용 약관에 동의하지 않았거나 서버 오류로 이용 약관 동의에 실패함.</li></ul></div> | 조건부      |
| `state`         | string | 요청 위조(cross-site request forgery) 공격을 방지하기 위해 클라이언트에서 전달받은 상태 token을 복호화한 값(URL 디코딩 적용). HTTP 응답 메시지가 `200`이나 `451`의 상태 코드를 가질 때 HTTP 응답 메시지 본문에 포함되는 필드입니다. | 항상      |

### Status codes

| 상태 코드       | 설명                                  |
|---------------|--------------------------------------|
| 200 OK           | 요청 처리 성공 시 받는 응답                      |
| 400 Bad Request  | `client_id` 필드와 같이 필수 파라미터를 입력하지 않거나 유효하지 않은 데이터를 파라미터로 입력했을 때 받는 응답 |
| 403 Forbidden    | 헤더에 포함된 {{ book.ServiceEnv.TargetServiceForClientAuth }} access token이 유효하지 않을 때 받는 응답 |
| 423 Locked       | Clova를 탈퇴한지 한 달이 안된 사용자의 {{ book.ServiceEnv.TargetServiceForClientAuth }} 계정으로 인증을 시도했을 때 받는 응답. 사용자가 Clova 서비스를 탈퇴하면 사용자는 1 개월 동안 해당 계정으로 Clova 서비스를 사용할 수 없습니다. <div class="note"><p><strong>Note!</strong></p><p>클라이언트가 <code>423 Locked</code> 상태 코드를 받게 되면 "Clova 서비스를 탈퇴한 계정입니다. 탈퇴한 후 한 달 뒤에 해당 계정으로 다시 로그인하면 Clova 서비스를 사용하실 수 있습니다."라고 안내 문구를 제공해야 합니다.</p></div>  |
| 451 Unavailable For Legal Reasons | 사용자가 이용 약관을 동의하지 않았을 때 받는 응답. 클라이언트는 이 응답을 받으면 `redirect_uri` 필드에 있는 주소로 이동하여 웹 페이지를 표시해야 합니다. 해당 URI는 사용자에게 서비스 이용 약관에 대해 동의를 받는 페이지입니다.  |
| 500 Server Internal Error | 서버 내부 오류로 인한 authorization code 발급 실패 시 받는 응답 |

### Response example

{% raw %}

```json
// 예제 1: HTTP 응답 메시지가 200 OK 상태 코드를 가지는 예
{
    "code": "cnl__eCSTdsdlkjfweyuxXvnlA",
    "state": "FKjaJfMlakjdfTVbES5ccZ"
}

// 예제 2: HTTP 응답 메시지가 451 Unavailable For Legal Reasons 상태 코드를 가지는 예
{
  "code":"4mrklvwoC_KNgDlvmslka",
  "redirect_uri":"https://example.net/clova/terms_3rd.html?code=4mrklvwoC_KNgDlvmslka&grant_type=code&state=FKjaJfMlakjdfTVbES5ccZ",
  "state":"FKjaJfMlakjdfTVbES5ccZ"
}
```

{% endraw %}

{% include "/Develop/References/ClovaAuthAPI/Guest_Mode.md" %}

### See also
* [클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)
* [Clova access token 생성하기](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)
* [Clova access token 요청](#RequestClovaAccessToken)


## Clova access token 요청 {#RequestClovaAccessToken}

[발급받은 authorization code](#RequestAuthorizationCode)를 사용하여 Clova 인증 서버에 Clova access token을 요청합니다.

### Endpoint

```
GET|POST /token?grant_type=authorization_code
```

### Request header

| Request header | 설명                                                                |
|----------------|--------------------------------------------------------------------|
| Accept         | <p>다음 값을 입력합니다.</p><p><pre><code>application/json</code></pre></p>                   |

### Query parameter

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `client_id`     | string  | 클라이언트 ID([클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo) 참조)                                  | 필수 |
| `client_secret` | string  | 클라이언트 Secret([클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo) 참조)                              | 필수 |
| `code`          | string  | [발급받은 authorization code](#RequestAuthorizationCode)                                                               | 필수 |
| `device_id`     | string  | 클라이언트 기기의 MAC 주소나 생성한 UUID                                                                                     | 필수 |
| `model_id`      | string  | 클라이언트 기기의 모델 ID                                                                                                 | 필수 |

### Request example

<pre><code>$ curl {{ book.ServiceEnv.AuthServerBaseURI }}token?grant_type=authorization_code \
       --data-urlencode "client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ" \
       --data-urlencode "client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D" \
       --data-urlencode "code=cnl__eCSTdsdlkjfweyuxXvnlA" \
       --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
       --data-urlencode "model_id=test_model"
</code></pre>

### Response header

| Response header | 설명                                                                |
|-----------------|--------------------------------------------------------------------|
| Content-Type    | <p>다음 값을 포함하고 있습니다.</p><p><pre><code>application/json</code></pre></p>                   |

### Response JSON object fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `access_token`  | string  | Clova access token                               | 항상      |
| `expires_in`    | number  | Clova access token의 유효 기간(초 단위)              | 항상      |
| `refresh_token` | string  | Clova access token을 갱신하기 위한 refresh token     | 항상      |
| `token_type`    | string  | Clova access token의 타입. "Bearer"로 고정 반환됩니다. | 항상      |

### Status codes

| 상태 코드       | 설명                                  |
|---------------|--------------------------------------|
| 200 OK        | 요청 처리 성공                                                                                   |
| 400 Bad Request  | `client_id` 필드와 같이 필수 파라미터를 입력하지 않거나 유효하지 않은 데이터를 파라미터로 입력했을 때 발생하는 실패 |
| 500 Internal Server Error | 서버 내부 오류로 인한 access token 발급 실패                                             |

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

### See also
* [클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)
* [Clova access token 생성하기](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)
* [Authorization code 요청하기](#RequestAuthorizationCode)


## Clova access token 갱신 {#RefreshClovaAccessToken}

발급받은 refresh token을 사용하여 Clova access token을 갱신합니다.

### Endpoint

```
GET|POST /token?grant_type=refresh_token
```

### Request header

| Request header | 설명                                                                |
|----------------|--------------------------------------------------------------------|
| Accept         | <p>다음 값을 입력합니다.</p><p><pre><code>application/json</code></pre></p>                   |

### Query parameter

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `client_id`     | string  | 클라이언트 ID([클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo) 참조)                                  | 필수 |
| `client_secret` | string  | 클라이언트 Secret([클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo) 참조)                              | 필수 |
| `device_id`     | string  | 클라이언트 기기의 MAC 주소나 생성한 UUID                                            | 선택 |
| `model_id`      | string  | 클라이언트 기기의 모델                                                           | 필수 |
| `refresh_token` | string  | 인증 성공 후 발급받은 refresh token                                              | 필수 |

### Request example

<pre><code>$ curl {{ book.ServiceEnv.AuthServerBaseURI }}token?grant_type=refresh_token \
       --data-urlencode "client_id=c2Rmc2Rmc2FkZ2FzZnNhZGZ" \
       --data-urlencode "client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D" \
       --data-urlencode "refresh_token=GW-Ipsdfasdfdfs3IbHFBA" \
       --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
       --data-urlencode "model_id=test_model"
</code></pre>

### Response header

| Response header | 설명                                                                |
|-----------------|--------------------------------------------------------------------|
| Content-Type    | <p>다음 값을 포함하고 있습니다.</p><p><pre><code>application/json</code></pre></p>                   |

### Response JSON object fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `access_token`  | string  | Clova access token                               | 항상      |
| `expires_in`    | number  | Clova access token의 유효 기간(초 단위)              | 항상      |
| `refresh_token` | string  | Clova access token을 갱신하기 위한 refresh token     | 항상      |
| `token_type`    | string  | Clova access token의 타입. "Bearer"로 고정 반환됩니다. | 항상      |

### Status codes

| 상태 코드       | 설명                                  |
|---------------|--------------------------------------|
| 200 OK                    | 요청 처리 성공                                                                                |
| 400 Bad Request           | `client_id` 필드와 같이 필수 파라미터를 입력하지 않거나 유효하지 않은 데이터를 파라미터로 입력했을 때 발생하는 실패 |
| 500 Internal Server Error | 서버 내부 오류로 인한 access token 갱신 실패                                                      |

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

### See also
* [클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)
* [Clova access token 요청](#RequestClovaAccessToken)


## Clova access token 삭제 {#DeleteClovaAccessToken}

[발급받은 Clova access token](#RequestClovaAccessToken)을 삭제합니다. 응답으로 삭제한 Clova access token에 대한 정보가 반환됩니다.

### Endpoint

```
GET|POST /token?grant_type=delete
```

### Request header

| Request header | 설명                                                                |
|----------------|--------------------------------------------------------------------|
| Accept         | <p>다음 값을 입력합니다.</p><p><pre><code>application/json</code></pre></p>                   |

### Query parameter

| 필드 이름       | 자료형    | 필드 설명                     | 필수 여부 |
|---------------|---------|-----------------------------|:---------:|
| `access_token`  | string  | 인증 성공 후 발급받은 Clova access token                                                                                 | 필수 |
| `client_id`     | string  | 클라이언트 ID([클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo) 참조)                                  | 필수 |
| `client_secret` | string  | 클라이언트 Secret([클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo) 참조)                              | 필수 |
| `device_id`     | string  | 클라이언트 기기의 MAC 주소나 생성한 UUID                                            | 필수 |
| `model_id`      | string  | 클라이언트 기기의 모델                                                           | 필수 |

### Request example

<pre><code>$ curl {{ book.ServiceEnv.AuthServerBaseURI }}token?grant_type=delete \
       --data-urlencode "access_token=xFcH08vYQcahQWouqIzWOw" \
       --data-urlencode "client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ" \
       --data-urlencode "client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D" \
       --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
       --data-urlencode "model_id=test_model"
</code></pre>

### Response header

| Response header | 설명                                                                |
|-----------------|--------------------------------------------------------------------|
| Content-Type    | <p>다음 값을 포함하고 있습니다.</p><p><pre><code>application/json</code></pre></p>                   |

### Response JSON object fields

| 필드 이름       | 자료형    | 필드 설명                     | 포함 여부 |
|---------------|---------|-----------------------------|:---------:|
| `access_token`  | string  | 삭제한 Clova access token                               | 항상      |
| `client_id`     | string  | 클라이언트 ID                                       | 항상      |
| `expires_in`    | number  | 삭제한 Clova access token이 가졌던 유효 기간(초 단위)              | 항상      |

### Status codes

| 상태 코드       | 설명                                  |
|---------------|--------------------------------------|
| 200 OK                    | 요청 처리 성공                                                                                                                       |
| 400 Bad Request           | `client_id` 필드와 같이 필수 파라미터를 입력하지 않거나 유효하지 않은 데이터를 파라미터로 입력했을 때 발생하는 실패                                        |
| 401 Unauthorized          | 유효하지 않은 클라이언트 인증 정보(`client_id` 또는 `client_secret`) 또는 사용자 정보(`device_id` 또는 `model_id`)를 파라미터로 전달했을 때 발생하는 실패 |
| 500 Internal Server Error | 서버 내부 오류로 인한 access token 삭제 실패                                                                                             |

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

### See also
* [클라이언트 인증 정보](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)
* [Clova access token 요청](#RequestClovaAccessToken)
