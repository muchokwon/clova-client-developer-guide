# Clova auth API
To connect a client with CIC, you must [create a Clova access token](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken). The Clova authorization server provides the Clova auth API for you to create and manage Clova access tokens.

## Base URI
The base URI of the Clova authorization server is as follows.

<pre><code>{{ book.ServiceEnv.AuthAPIBaseURI }}
</code></pre>

## Requesting an authorization code {#RequestAuthorizationCode}

This API requests an authorization code by conveying the {{ book.ServiceEnv.TargetServiceForClientAuth }} account {{ "authorization code" if book.L10N.TargetCountryCode == "JP" else "access token" }} and [client credentials](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo) as parameters. The obtained authorization code will be used in generating a Clova access token.

Typically, user authentication is processed on the pair app. However, transferring a Clova access token from the pair app to the client may have a potential security issue. To avoid security issues, the app forwards the authorization code to the client instead of the Clova access token. Upon receiving an authorization code, the client is to pass the code to the Clova authorization server and [request a Clova access token](#RequestClovaAccessToken).

### Endpoint

```
GET|POST /authorize
```

### Request header

| Request header | Description                                                                |
|----------------|--------------------------------------------------------------------|
| Accept         | <p>Enter the following value:</p><p><pre><code>application/json</code></pre></p>  |
| Authorization  | <p><a href="/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken">Enter the {{ book.ServiceEnv.TargetServiceForClientAuth }} {{ "authorization code" if book.L10N.TargetCountryCode == "JP" else "access token" }}</a> acquired:</p><p><pre><code>Bearer [{{ book.ServiceEnv.TargetServiceForClientAuth }} {{ "authorization code" if book.L10N.TargetCountryCode == "JP" else "access token" }}]</code></pre></p>.  |

### Query parameter

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `client_id`     | string  | The client ID (See [client credentials](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)).          | Required |
| `device_id`     | string  | The MAC address or UUID of the client device.                                                              | Required |
{% if book.L10N.TargetCountryCode == "JP"  -%}
| `grant_type`    | string  | Enter `uauth_auth_code_v2`.  | Required  |
{% endif -%}
| `model_id`      | string  | The model ID of the client device.                                                                          | Required |
| `response_type` | string  | The response type. The value is always `"code"`.                                                             | Required |
| `state`         | string  | The state of the token (URI encoded) used by the client to prevent cross-site request forgery attacks. | Required |

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

| Response header | Description                                                                |
|-----------------|--------------------------------------------------------------------|
| Content-type    | <p>Contains the following:</p><p><pre><code>application/json</code></pre></p>                   |

### Response JSON object fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `code`          | string | The authorization code issued by the Clova authorization server. This field is returned in the body of the HTTP response message if the status code is `200` or `451`.      | Always      |
| `redirect_uri`  | string | The URI of the Terms and Conditions page. The field is returned in the body of the HTTP response message if the status code is `451 Unavailable For Legal Reasons`. The client must go to the redirect URI and display the page. When the user submits the agreement, the client will receive a response containing the `302 Found`(URL redirection) status code along with either one of the following URLs. <ul><li><code>clova://agreement-success</code>: The user has successfully agreed to the Terms and Conditions. The client can continue to the next process to create a Clova access token.</li><li><code>clova://agreement-failure?error=[reason]</code>: The user did not agree to the Terms and Conditions or the user agreement has failed due to a server error. We recommend you take appropriate actions regarding the failure. The following values may be sent as `error` parameter values.<ul><li><code>"server_error"</code>: Internal server error.</li><li><code>"terms_not_agreed"</code>: The required terms were not agreed upon.</li><li><code>"user-disagreement"</code>: The user did not agree to the Terms and Conditions.</li></ul><div class="tip"><p><strong>Tip!</strong></p><p>When redirecting the page to the URI specified in this field (Agree to the Terms and Conditions page), the <code>redirect_uri</code> parameter can be added. <code>redirect_uri</code> If added, response of the Terms and Conditions page will have the following form</p><ul><li><code>https://[redirect_uri]?code=[code]&state=[state]</code>: The user has agreed to the Terms and Conditions.</li><li><code>https://[redirect_uri]?code=[code]&state=[state]&error=[reason]</code>: The user has not agreed to the Terms and Conditions or has failed to agree to the Terms and Conditions due to the server error.</li></ul></div> | Conditional      |
| `state`         | string | The state of the token sent by the client to prevent cross-site request forgery attacks (URL decoded). This field is returned in the body of the HTTP response message if the status code is `200` or `451`. | Always      |

### Status codes

| Status code       | Description                                  |
|---------------|--------------------------------------|
| 200 OK           | The request has been successfully processed.                      |
| 400 Bad Request  | Required parameters such as `client_id` are missing or invalid parameters have been used. |
| 403 Forbidden    | The {{ book.ServiceEnv.TargetServiceForClientAuth }} access token specified in the header is invalid. |
| 423 Locked       | Authentication attempted using the {{ book.ServiceEnv.TargetServiceForClientAuth }} account of a user who unsubscribed from the Clova membership less than one month ago. A user who has unsubscribed from Clova cannot use the Clova service with that account for a month. <div class="note"><p><strong>Note!</strong></p><p>If the client receives a <code>423 Locked</code> status code, a guidance message saying "This account has been unsubscribed from the Clova service. You can use this account again if you log in to the account one month after the date of unsubscription." must be displayed.</p></div>  |
| 451 Unavailable For Legal Reasons | The user has not agreed to the Terms and Conditions. The client should go to the URI in the `redirect_uri` field and display the webpage. This URI points to the Terms and Conditions page.  |
| 500 Server Internal Error | Failed to issue an authorization code due to an internal server error. |

### Response example

{% raw %}

```json
// Example 1: Example of the HTTP response message having 200 OK
{
    "code": "cnl__eCSTdsdlkjfweyuxXvnlA",
    "state": "FKjaJfMlakjdfTVbES5ccZ"
}

// Example 2: Example of the HTTP response message having 451 Unavailable For Legal Reasons
{
  "code":"4mrklvwoC_KNgDlvmslka",
  "redirect_uri":"https://example.net/clova/terms_3rd.html?code=4mrklvwoC_KNgDlvmslka&grant_type=code&state=FKjaJfMlakjdfTVbES5ccZ",
  "state":"FKjaJfMlakjdfTVbES5ccZ"
}
```

{% endraw %}

{% include "/Develop/References/ClovaAuthAPI/Guest_Mode.md" %}

### See also
* [Client credentials](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)
* [Creating Clova access tokens](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)
* [Requesting a Clova access token](#RequestClovaAccessToken)


## Requesting a Clova access token {#RequestClovaAccessToken}

This API requests for a Clova access token to the Clova authorization server with the [issued authorization](#RequestAuthorizationCode).

### Endpoint

```
GET|POST /token?grant_type=authorization_code
```

### Request header

| Request header | Description                                                                |
|----------------|--------------------------------------------------------------------|
| Accept         | <p>Enter the following value:</p><p><pre><code>application/json</code></pre></p>                   |

### Query parameter

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `client_id`     | string  | The client ID (See [client credentials](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)).                                  | Required |
| `client_secret` | string  | The client secret (See [client credentials](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)).                              | Required |
| `code`          | string  | [The issued authorization code](#RequestAuthorizationCode)                                                               | Required |
| `device_id`     | string  | The MAC address or UUID of the client device.                                                                                     | Required |
| `model_id`      | string  | The model ID of the client device.                                                                                                 | Required |

### Request example

<pre><code>$ curl {{ book.ServiceEnv.AuthAPIBaseURI }}token?grant_type=authorization_code \
       --data-urlencode "client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ" \
       --data-urlencode "client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D" \
       --data-urlencode "code=cnl__eCSTdsdlkjfweyuxXvnlA" \
       --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
       --data-urlencode "model_id=test_model"
</code></pre>

### Response header

| Response header | Description                                                                |
|-----------------|--------------------------------------------------------------------|
| Content-type    | <p>Contains the following:</p><p><pre><code>application/json</code></pre></p>                   |

### Response JSON object fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `access_token`  | string  | The Clova access token.                               | Always      |
| `expires_in`    | number  | The validity of the Clova access token (in seconds).              | Always      |
| `refresh_token` | string  | The refresh token required to renew the Clova access token.     | Always      |
| `token_type`    | string  | The type of the Clova access token. The return value is always "Bearer." | Always      |

### Status codes

| Status code       | Description                                  |
|---------------|--------------------------------------|
| 200 OK        | The request has been processed successfully.                                                                                   |
| 400 Bad Request  | Required parameters such as `client_id` are missing or invalid parameters have been used. |
| 500 Internal Server Error | Failed to issue an access token due to an internal server error.                                             |

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
* [Client credentials](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)
* [Creating Clova access tokens](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)
* [Requesting an authorization code](#RequestAuthorizationCode)


## Refreshing a Clova access token {#RefreshClovaAccessToken}

This API renews the Clova access token with a refresh token.

### Endpoint

```
GET|POST /token?grant_type=refresh_token
```

### Request header

| Request header | Description                                                                |
|----------------|--------------------------------------------------------------------|
| Accept         | <p>Enter the following value:</p><p><pre><code>application/json</code></pre></p>                   |

### Query parameter

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `client_id`     | string  | The client ID (See [client credentials](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)).                                  | Required |
| `client_secret` | string  | The client secret (See [client credentials](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)).                              | Required |
| `device_id`     | string  | The MAC address or UUID of the client device.                                            | Optional |
| `model_id`      | string  | The model of the client device.                                                           | Required |
| `refresh_token` | string  | The refresh token issued with the Clova access token.                                              | Required |

### Request example

<pre><code>$ curl {{ book.ServiceEnv.AuthAPIBaseURI }}token?grant_type=refresh_token \
       --data-urlencode "client_id=c2Rmc2Rmc2FkZ2FzZnNhZGZ" \
       --data-urlencode "client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D" \
       --data-urlencode "refresh_token=GW-Ipsdfasdfdfs3IbHFBA" \
       --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
       --data-urlencode "model_id=test_model"
</code></pre>

### Response header

| Response header | Description                                                                |
|-----------------|--------------------------------------------------------------------|
| Content-type    | <p>Contains the following:</p><p><pre><code>application/json</code></pre></p>                   |

### Response JSON object fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `access_token`  | string  | The Clova access token.                               | Always      |
| `expires_in`    | number  | The validity of the Clova access token (in seconds).              | Always      |
| `refresh_token` | string  | The refresh token required to renew the Clova access token.     | Always      |
| `token_type`    | string  | The type of the Clova access token. The return value is always "Bearer." | Always      |

### Status codes

| Status code       | Description                                  |
|---------------|--------------------------------------|
| 200 OK                    | The request has been processed successfully.                                                                                |
| 400 Bad Request           | Required parameters such as `client_id` are missing or invalid parameters have been used. |
| 500 Internal Server Error | Failed to refresh an access token due to an internal server error.                                                      |

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
* [Client credentials](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)
* [Requesting a Clova access token](#RequestClovaAccessToken)


## Deleting a Clova access token {#DeleteClovaAccessToken}

This API deletes the [issued Clova access token](#RequestClovaAccessToken). The deleted information on the Clova access token is returned as a response.

### Endpoint

```
GET|POST /token?grant_type=delete
```

### Request header

| Request header | Description                                                                |
|----------------|--------------------------------------------------------------------|
| Accept         | <p>Enter the following value:</p><p><pre><code>application/json</code></pre></p>                   |

### Query parameter

| Field name       | Data type    | Description                     | Required |
|---------------|---------|-----------------------------|:---------:|
| `access_token`  | string  | The Clova access token issued.                                                                                 | Required |
| `client_id`     | string  | The client ID (See [client credentials](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)).                                  | Required |
| `client_secret` | string  | The client secret (See [client credentials](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)).                              | Required |
| `device_id`     | string  | The MAC address or UUID of the client device.                                            | Required |
| `model_id`      | string  | The model of the client device.                                                           | Required |

### Request example

<pre><code>$ curl {{ book.ServiceEnv.AuthAPIBaseURI }}token?grant_type=delete \
       --data-urlencode "access_token=xFcH08vYQcahQWouqIzWOw" \
       --data-urlencode "client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ" \
       --data-urlencode "client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D" \
       --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
       --data-urlencode "model_id=test_model"
</code></pre>

### Response header

| Response header | Description                                                                |
|-----------------|--------------------------------------------------------------------|
| Content-type    | <p>Contains the following:</p><p><pre><code>application/json</code></pre></p>                   |

### Response JSON object fields

| Field name       | Data type    | Description                     | Included |
|---------------|---------|-----------------------------|:---------:|
| `access_token`  | string  | The deleted Clova access token.                               | Always      |
| `client_id`     | string  | The client ID.                                       | Always      |
| `expires_in`    | number  | The validity of the deleted Clova access token (in seconds).              | Always      |

### Status codes

| Status code       | Description                                  |
|---------------|--------------------------------------|
| 200 OK                    | The request has been processed successfully.                                                                                                                       |
| 400 Bad Request           | Required parameters such as `client_id` are missing or invalid parameters have been used.                                        |
| 401 Unauthorized          | Invalid client credentials (`client_id` or `client_secret`) or user information (`device_id` or `model_id`) are received as parameters. |
| 500 Internal Server Error | Failed to delete the access token due to an internal server error.                                                                                             |

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
* [Client credentials](/Develop/Guides/Interact_with_CIC.md#ClientAuthInfo)
* [Requesting a Clova access token](#RequestClovaAccessToken)
