## Connecting with CIC {#ConnectToCIC}
To connect a client with CIC, complete the following steps.
* [Creating Clova access tokens](#CreateClovaAccessToken)
* [Creating connections](#CreateConnection)
* [Getting authorizations](#Authorization)
* [Managing connections](#ManageConnection)

### Creating Clova access tokens {#CreateClovaAccessToken}
To use Clova, users must authenticate their {{ book.ServiceEnv.TargetServiceForClientAuth }} account on a client, whether it is a device or an app. Clova access tokens with {{ book.ServiceEnv.TargetServiceForClientAuth }} credentials must be obtained from the Clova authorization server for the client to connect and make a request to CIC. For this process, you must use the [Clova auth API](/Develop/References/Clova_Auth_API.md).

The following diagram illustrates the process of a client obtaining a Clova access token. Note that the process differs for each client type. Clova defines the following two types of clients for providing Clova services:
* **Client without GUI**: Clients without a display embedded in devices like speakers or home appliances. Since users cannot enter their credentials on this type of device for account authentication and it can be frustrating, the client should provide a companion app or be linked to the Clova app.
* **Client with GUI**: Clients with a display embedded in devices like speakers or home appliances, or clients with an app, just like the Clova app.

![](/Develop/Assets/Images/CIC_Authorization.svg)

Obtain a Clova access token by following the instructions provided below:

<ol>
  <li>
    <p>The client provides an interface for a user to authenticate the {{ book.ServiceEnv.TargetServiceForClientAuth }} account (Log in using the <a href="{{ book.ServiceEnv.LoginAPIofTargetService }}" target="_blank">{{ book.ServiceEnv.TargetServiceForClientAuth }} ID</a>). As voice biometric cannot be the sole method of account authentication, make sure to use the Clova app or the companion app for <strong>clients without GUI</strong>. Client need to specify <code>{{ book.ServiceEnv.RedirectURIonLINEAuth }}?clova_client_id=[CLOVA_OAUTH_CLIENT_ID]</code>as <code>redirect_uri</code> parameter.</p>
  </li>
  <li>
    <p>Obtain the {{ "authorization code" if book.L10N.TargetCountryCode == "JP" else "access token" }} for the {{ book.ServiceEnv.TargetServiceForClientAuth }} account using the {{ book.ServiceEnv.TargetServiceForClientAuth }} account information entered by the user.</p>
  </li>
  <li>
    <p><a href="/Develop/References/Clova_Auth_API.html#RequestAuthorizationCode">Request an authorization code</a> using the obtained {{ "authorization code" if book.L10N.TargetCountryCode == "JP" else "access token" }} for the {{ book.ServiceEnv.TargetServiceForClientAuth }} account and <a href="#ClientAuthInfo">client credentials</a>. Define the <code>device_id</code> field either with the client MAC address or with the UUID hash value you generated. The following is an example of requesting an authorization code.</p>
    <pre><code>$ curl -H "Authorization: Bearer Zc3d3QAR6zIxqceOpXoq"
    {{ book.ServiceEnv.AuthServerBaseURL }}authorize \
    {% if book.L10N.TargetCountryCode == "JP"  -%}
    --data-urlencode "grant_type=uauth_auth_code_v2" \
    {% endif -%}
    --data-urlencode "client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ" \
    --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
    --data-urlencode "model_id=test_model" \
    --data-urlencode "response_type=code" \
    --data-urlencode "state=FKjaJfMlakjdfTVbES5ccZ"
</code></pre>
    <p>As a response to the request, the client will receive a response containing the authorization code in the message body as shown below. The <code>code</code> field holds the authorization code.</p>
    <pre><code>{
  "code": "cnl__eCSTdsdlkjfweyuxXvnlA",
  "state": "FKjaJfMlakjdfTVbES5ccZ"
}
</code></pre></li>
  <li>
    <p>If the client receives the <code>451 Unavailable For Legal Reasons</code> status code as a response to the <a href="/Develop/References/Clova_Auth_API.html#RequestAuthorizationCode"> request for an authorization code</a>, the client must display the Terms and Conditions page to the user using the URI in the <code>redirect_uri</code> field of the response message body. See the following example of a response message body for the status code, <code>451 Unavailable For Legal Reasons</code>.</p>
    <pre><code>{
  "code": "4mrklvwoC_KNgDlvmslka",
  "redirect_uri": "https://ssl.pstatic.example.net/static/clova/service/terms/place/terms_3rd.html?code=4mrklvwoC_KNgDlvmslka&grant_type=code&state=FKjaJfMlakjdfTVbES5ccZ",
  "state": "FKjaJfMlakjdfTVbES5ccZ"
}
</code></pre>
    <p>When the user submits the agreement, the client will receive a response containing the <code>302 Found</code>(URL redirection) status code along with either one of the following URLs.</p>
    <ul>
      <li><code>clova://agreement-success</code>: The user has successfully agreed to the terms and conditions. The client is allowed to proceed onto the next step, which is creating the Clova access token.</li>
      <li><code>clova://agreement-failure?error=[reason]</code>: The processing of the user agreement has failed due to user disagreement or a server error. We recommend you to take appropriate actions regarding the failure.</li>
    </ul>
    <p>If the client is a web application, client can add optional <code>redirect_uri</code> parameter when redirecting. In that case, client will receive the following callback on the URI for successful case and disagreement case, respectively.</p>
    <ul>
      <li><code>https://[redirect_uri]?code=[code]&state=[state]</code></li>
      <li><code>https://[redirect_uri]?code=[code]&state=[state]&error=[reason]</code></li>
    </ul>
  </li>
  <li>
    <p>For clients without a GUI, send the authorization code to the actual client device.</p>
  </li>
  <li>
    <p><a href="/Develop/References/Clova_Auth_API.html#RequestClovaAccessToken">Request for the Clova access token</a> using the obtained authorization code and <a href="#ClientAuthInfo">client credentials</a> as parameters. The following is an example of requesting a Clova access token.</p>
    <pre><code>$ curl {{ book.ServiceEnv.AuthServerBaseURL }}token?grant_type=authorization_code \
    --data-urlencode "client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ" \
    --data-urlencode "client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D" \
    --data-urlencode "code=cnl__eCSTdsdlkjfweyuxXvnlA" \
    --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
    --data-urlencode "model_id=test_model"
</code></pre>
    <p>As a response to the request, the Clova authentication server will return a Clova access token containing the information as shown below.</p>
    <pre><code>{
    "access_token": "XHapQasdfsdfFsdfasdflQQ7w",
    "expires_in": 332000,
    "refresh_token": "GW-Ipsdfasdfdfs3IbHFBA",
    "token_type": "Bearer"
}
</code></pre>
  </li>
</ol>

### Connecting with CIC {#CreateConnection}
To establish an initial connection between a client and CIC, the first thing to do is [create a downchannel](/Develop/References/CIC_API.md#EstablishDownchannel). A downchannel is used for a client to receive directives from CIC. Directives sent through the downchannel are not responses to events but are cloud-initiated directives by CIC when conditions are met or needs arise. For example, when a new push message arrives on a cloud queue, CIC will send a directive through a downchannel to notify the user.

To create a downchannel, send a `GET` request to `/v1/directives`. Once a downchannel is established, CIC keeps the connection open.

{% raw %}

```
:method: GET
:scheme: https
:path: /v1/directives
User-Agent: {{User-Agent_string}}
Authorization: Bearer {{ClovaAccessToken}}
```

{% endraw %}

When the above connect request is successfully completed, CIC responds with the [`Clova.Hello`](/Develop/References/CICInterface/Clova.md#Hello) directive. This indicates that CIC is ready to send more directives through the downchannel.

{% raw %}

```
{
    "directive": {
        "header": {
            "messageId": "2ca2ec70-c39d-4741-8a34-8aedd3b24760",
            "namespace": "Clova",
            "name": "Hello"
        },
        "payload": {}
    }
}
```

{% endraw %}

<div class="note">
  <p><strong>Note!</strong></p>
  <ul>
    <li>The client must maintain one open downchannel with CIC at all times. When there is an open downchannel and an additional request to create a downchannel is made to <code>/v1/directives</code>, the existing downchannel will be closed.</li>
    <li>For more information on the user-agent field that should be included in the HTTP header, see <a href="#UserAgentString">User-agent string</a>.</li>
    <li>For more information on the authorization field that should be included in the HTTP header, see <a href="#Authorization">Getting authorizations</a>.</li>
  </ul>
</div>

<div class="danger">
  <p><strong>Caution!</strong></p>
  <p>Make sure that event message are sent to CIC using the <a href="#CreateConnection">connection created when establishing the downchannel</a>.</p>
</div>

### Getting authorizations {#Authorization}
When the client sends a request to CIC, the request must include the [Clova access token](#CreateClovaAccessToken). Enter the type and value of the Clova access token, separated by a space, in the Authorization field of the header as shown below. For more information, see [CIC API reference](/Develop/References/CIC_API.md).

{% raw %}

```
:method: {{GET|POST}}
:scheme: https
:path: {{/v1/events|/v1/directives}}
User-Agent: {{User-Agent_string}}
Authorization: Bearer {{ClovaAccessToken}}
```

{% endraw %}

The client must send the Clova access token for every new request (event) sent.

![](/Develop/Assets/Images/CIC_Message_Interaction_Diagram.png)

### Managing connections {#ManageConnection}

Once a connection is established between a client and CIC, the connection must be managed following the instructions below:

* [Maintaining a downchannel](#KeepDownchannel)
* [Using ping-pong](#DoPingpong)
* [Refreshing an access token](#RefreshAccessToken)

#### Maintaining a downchannel {#KeepDownchannel}
When a downchannel is closed, either by request or network disconnection, [create a new downchannel](#CreateConnection) immediately to guarantee receiving directives from CIC.

#### Using ping-pong {#DoPingpong}

To check connectivity with CIC, the client should send an HTTP/2 PING frame to CIC every minute. If an HTTP/2 PING ACK response is not received from CIC, the client must establish a new connection immediately and ensure a connection between a client and CIC all times. For more information on HTTP/2 PING frame, see [HTTP/2 PING payload format](https://http2.github.io/http2-spec/#rfc.figure.12).

<div class="note">
  <p><strong>Note!</strong></p>
  <p>If the client cannot send an HTTP/2 PING frame, it must send a <code>GET</code> request to <code>/ping</code> every minute. Then the client will receive the "HTTP 204 No Content" response. If no response is received, the client must establish a new connection with CIC immediately.</p>
  <p>Here is an example of the <code>GET</code> request sent to <code>/ping</code>.</p>
  <pre><code>:method = GET
:scheme = https
:path = /ping
User-Agent: [User-Agent_string]
Authorization: Bearer [YOUR_ACCESS_TOKEN]
</code></pre>
</div>

#### Refreshing an access token {#RefreshAccessToken}

The client can get the expiration time of a Clova access token using the `expires_in` field of the obtained token. If the client receives an expired token or receives an "HTTP 401 Unauthorized" [error status code](/Develop/References/CIC_API.md#Error) for using the token, the access token must be renewed. As shown below, you can [refresh the Clova access token](/Develop/References/Clova_Auth_API.md#RefreshClovaAccessToken) by providing the `refresh_token` received with the [granted Clova access token](/Develop/References/Clova_Auth_API.md#RequestClovaAccessToken) and providing other required parameters.

<pre><code>$ curl {{ book.ServiceEnv.AuthServerBaseURL }}token?grant_type=refresh_token \
       --data-urlencode "client_id=c2Rmc2Rmc2FkZ2FzZnNhZGZ" \
       --data-urlencode "client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D" \
       --data-urlencode "refresh_token=GW-Ipsdfasdfdfs3IbHFBA" \
       --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
       --data-urlencode "model_id=test_model"
</code></pre>
