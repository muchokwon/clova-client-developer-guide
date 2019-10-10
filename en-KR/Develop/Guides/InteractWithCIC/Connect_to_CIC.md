## Connecting with CIC {#ConnectToCIC}
To connect a client with CIC, complete the following steps.
* [Creating Clova access tokens](#CreateClovaAccessToken)
* [Creating connection](#CreateConnection)
* [Getting authorizations](#Authorization)
* [Managing connections](#ManageConnection)

### Creating Clova access tokens {#CreateClovaAccessToken}
To use Clova, users must authenticate their {{ book.ServiceEnv.TargetServiceForClientAuth }} account on a client, whether it is a device or an app. Clova access tokens with {{ book.ServiceEnv.TargetServiceForClientAuth }} credentials must be obtained from the Clova authorization server for the client to connect and make a request to CIC. For this process, you must use the [Clova auth API](/Develop/References/Clova_Auth_API.md).

The following diagram illustrates the process of a client obtaining a Clova access token. Note that the process differs for each client type. Clova defines the following two types of clients for providing Clova services:
* **Client without GUI**<br />
  Clients embedded in devices without a display, such as, speakers or home appliances. On this type of device, the client must be provided with a companion app, or it has to be linked to the Clova app because it can be inconvenient for users to input their credentials for account authentication, or making user inputs may not be available at all.
* **Client with GUI**<br />
  Clients embedded in devices with display, or software-type clients, such as the Clova app.

![](/Develop/Assets/Images/CIC_Authorization.svg)

Obtain a Clova access token by following the instructions provided below:

1. First of all, provide an interface for users to {{ book.ServiceEnv.TargetServiceForClientAuth }} authenticate the account (<a href="{{ book.ServiceEnv.LoginAPIofTargetService }}" target="_blank">{{ book.ServiceEnv.TargetServiceForClientAuth }}Log in using the ID</a>).<br />
  As voice biometric cannot be the sole method for account authentication, make sure to use the Clova app or the companion app for **clients without GUI**.
2. Obtain {{ "authorization code" if book.L10N.TargetCountryCode == "JP" else "access token" }} of the {{ book.ServiceEnv.TargetServiceForClientAuth }} account. When you obtain the {{ "authorization code" if book.L10N.TargetCountryCode == "JP" else "access token" }}, use the {{ book.ServiceEnv.TargetServiceForClientAuth }} account information entered by the user.
3. [Request an authorization code](/Develop/References/Clova_Auth_API.md#RequestAuthorizationCode).<br />
  When you request an authorization code, you must use the information such as {{ "authorization code" if book.L10N.TargetCountryCode == "JP" else "access token" }} of the {{ book.ServiceEnv.TargetServiceForClientAuth }} account obtained in step 2 and the [client credentials](#ClientAuthInfo). At this time, define the `device_id` field either with the client MAC address or with the UUID hash value you generated. The following is an example of requesting an authorization code.
  {% if book.L10N.TargetCountryCode == "KR" -%}
  ```bash
  $ curl -H "Authorization: Bearer Zc3d3QAR6zIxqceOpXoq"
      {{ book.ServiceEnv.AuthAPIBaseURI }}authorize \
      --data-urlencode "client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ" \
      --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
      --data-urlencode "model_id=test_model" \
      --data-urlencode "response_type=code" \
      --data-urlencode "state=FKjaJfMlakjdfTVbES5ccZ"
  ```
  {%- elif book.L10N.TargetCountryCode == "JP"  -%}
  ```bash
  $ curl -H "Authorization: Bearer Zc3d3QAR6zIxqceOpXoq"
      {{ book.ServiceEnv.AuthAPIBaseURI }}authorize \
      --data-urlencode "grant_type=uauth_auth_code_v2" \
      --data-urlencode "client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ" \
      --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
      --data-urlencode "model_id=test_model" \
      --data-urlencode "response_type=code" \
      --data-urlencode "state=FKjaJfMlakjdfTVbES5ccZ"
  ```
  {% endif %}
  As a response to the request, the client will receive a response containing the authorization code in the message body as shown below. The `code` field holds the authorization code.
  ```json
  {
    "code": "cnl__eCSTdsdlkjfweyuxXvnlA",
    "state": "FKjaJfMlakjdfTVbES5ccZ"
  }
  ```
4. If the client receives the `451 Unavailable For Legal Reasons` status code as a response to step 3, show the Terms and Conditions page to the user.<br />
  When providing the Terms and Conditions page, use the URI entered in the `redirect_uri` field in the body of the response message. Below is an example of the body of a response message received when the status code is `451 Unavailable For Legal Reasons`.
  ```json
  {
    "code": "4mrklvwoC_KNgDlvmslka",
    "redirect_uri": "https://example.net/clova/terms_3rd.html?code=4mrklvwoC_KNgDlvmslka&grant_type=code&state=FKjaJfMlakjdfTVbES5ccZ",
    "state": "FKjaJfMlakjdfTVbES5ccZ"
  }
  ```
  <div class="note">
    <p><strong>Note!</strong></p>
    <p>When the user submits the agreement, the client will receive a response containing the <code>302 Found</code> (URL Redirection) status code along with either one of the following URLs.</p>
    <ul>
      <li><code>clova://agreement-success</code>: The user has successfully agreed to the terms and conditions. The client is allowed to proceed onto the next step, which is creating the Clova access token.</li>
      <li><code>clova://agreement-failure?error=[reason]</code>: The user did not agree to the Terms and Conditions or the user agreement has failed due to a server error. We recommend you to take appropriate actions regarding the failure.</li>
    </ul>
  </div>
5. For clients without a GUI, send the authorization code to the actual client device.
6. [Request a Clova access token](/Develop/References/Clova_Auth_API.md#RequestClovaAccessToken).<br />
  When making a request, you must enter the obtained authorization code and [client credentials](#ClientAuthInfo) as the parameters. The following is an example of requesting a Clova access token.
  ```bash
  $ curl {{ book.ServiceEnv.AuthAPIBaseURI }}token?grant_type=authorization_code \
      --data-urlencode "client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ" \
      --data-urlencode "client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D" \
      --data-urlencode "code=cnl__eCSTdsdlkjfweyuxXvnlA" \
      --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
      --data-urlencode "model_id=test_model"
  ```
  As a response to the request, the Clova authentication server will return a Clova access token containing the information as shown below.
  ```json
  {
    "access_token": "XHapQasdfsdfFsdfasdflQQ7w",
    "expires_in": 332000,
    "refresh_token": "GW-Ipsdfasdfdfs3IbHFBA",
    "token_type": "Bearer"
  }
  ```

### Creating connection {#CreateConnection}
To establish an initial connection between a client and CIC, the first thing to do is [create a downchannel](/Develop/References/CIC_API.md#EstablishDownchannel). A downchannel is used for a client to receive directive messages from CIC. Directive messages sent through the downchannel are not responses to event messages but are cloud-initiated directives sent to the client in certain conditions or needs. For example, when a new push message arrives on a cloud queue, CIC will send a directive message through a downchannel to notify the user.

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

When the above connect request is successfully completed, CIC responds with the [`Clova.Hello`](/Develop/References/MessageInterfaces/Clova.md#Hello) directive message. This indicates that CIC is ready to send more directive message through the downchannel.

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
  <p>The client must maintain one open downchannel along with CIC at all times. When there is an open downchannel and an additional request to create a downchannel is made to <code>/v1/directives</code>, the existing downchannel will be closed.</p>
  <ul>
    <li>For more information on the user-agent field that should be included in the HTTP header, see <a href="#UserAgentString">User-agent string</a>.</li>
    <li>For more information on the authorization field that should be included in the HTTP header, see <a href="#Authorization">Getting authorizations</a>.</li>
  </ul>
</div>

<div class="note">
  <p><strong>Note!</strong></p>
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

The client must send the Clova access token for every new request (event message) sent.

![](/Develop/Assets/Images/CIC_Message_Interaction_Diagram.png)

### Managing connections {#ManageConnection}

Once a connection is established between a client and CIC, the connection must be managed following the instructions below:

* [Maintaining a downchannel](#KeepDownchannel)
* [Using ping-pong](#DoPingpong)
* [Refreshing an access token](#RefreshAccessToken)

#### Maintaining a downchannel {#KeepDownchannel}
When a downchannel is closed, either by request or network disconnection, [create a new downchannel](#CreateConnection) immediately to guarantee receiving directive messages from CIC.

#### Using ping-pong {#DoPingpong}

To check connectivity status with CIC, the client should send an HTTP/2 PING frame to CIC every minute. If an HTTP/2 PING ACK response is not received from CIC, the client must establish a new connection immediately and ensure a connection between a client and CIC all times. For more information on HTTP/2 PING frame, see [HTTP/2 PING payload format](https://http2.github.io/http2-spec/#rfc.figure.12).

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

<pre><code>$ curl {{ book.ServiceEnv.AuthAPIBaseURI }}token?grant_type=refresh_token \
       --data-urlencode "client_id=c2Rmc2Rmc2FkZ2FzZnNhZGZ" \
       --data-urlencode "client_secret=66qo65asdfasdfaA7JasdfasfOqwnOq1rOyfgeydtCDrvYasfasf%3D" \
       --data-urlencode "refresh_token=GW-Ipsdfasdfdfs3IbHFBA" \
       --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
       --data-urlencode "model_id=test_model"
</code></pre>
