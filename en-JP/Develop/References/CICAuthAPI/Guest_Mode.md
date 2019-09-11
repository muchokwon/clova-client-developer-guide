{% if book.DocMeta.TargetReaderType == "Internal" or book.DocMeta.TargetReaderType == "Uplus" %}
### Remarks {#GuestMode}
To provide Clova services in guest mode without authenticating the {{ book.ServiceEnv.TargetServiceForClientAuth }} account, complete the following steps:

1. Skip step 1 and step 2 in the instructions for [creating Clova access tokens](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken).
2. Apply the following changes when [requesting for authorization code](#RequestAuthorizationCode) in step 3:
  * Skip specifying the `Authorization` header field for the request.
  * Add `request_vu` as a query parameter and set the value to `Y`.

<div class="note">
  <p><strong>Note!</strong></p>
  The default value of the <p><code>request_vu</code> parameter is <code>N</code>. The Clova policy is to authenticate the {{ book.ServiceEnv.TargetServiceForClientAuth }} account of a user for use.</p>
</div>

You can obtain a guest code (an authorization code for guest mode) by following the instruction provided above. To obtain a Clova access token for guests, continue following the steps provided in the [Create Clova access token](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken) section with the guest code.

The following code is an example of requesting a guest code.

<pre><code>$ curl {{ book.ServiceEnv.AuthServerBaseURL }}authorize \
       --data-urlencode "request_vu=Y" \
       --data-urlencode "client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ" \
       --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
       --data-urlencode "model_id=test_model" \
       --data-urlencode "response_type=code" \
       --data-urlencode "state=FKjaJfMlakjdfTVbES5ccZ"
</code></pre>
{% endif %}
