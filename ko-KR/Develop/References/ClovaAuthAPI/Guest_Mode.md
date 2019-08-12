### Remarks {#GuestMode}

사용자에게 {{ book.ServiceEnv.TargetServiceForClientAuth }} 계정 인증 없이 guest 모드 형태의 서비스를 제공하려면 다음 내용을 따릅니다.

1. [Clova access token 생성하기](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)에서 설명하고 있는 절차 설명 중 1 번과 2 번 단계 설명을 생략합니다.
2. 3 번 단계에서 [authorization code를 요청](#RequestAuthorizationCode)할 때 다음 내용을 적용합니다.
  * 요청 헤더 중 `Authorization` 필드를 입력하지 않습니다.
  * `request_vu`를 Query parameter로 추가하고 `Y`로 설정합니다.

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p><code>request_vu</code>의 기본 값은 <code>N</code>이며, {{ book.ServiceEnv.TargetServiceForClientAuth }} 계정을 인증하여 쓰는 것이 기본 방침입니다.</p>
</div>

위 내용을 수행하면 guest 모드 용 authorization code를 받게 됩니다. 이를 이용하여 [Clova access token 생성하기](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)에서 설명하고 있는 나머지 절차를 수행하면 guest용 Clova access token을 획득할 수 있습니다.

다음은 guest 모드 용 authorization code를 요청하는 예제입니다.

<pre><code>$ curl {{ book.ServiceEnv.AuthServerBaseURI }}authorize \
       --data-urlencode "request_vu=Y" \
       --data-urlencode "client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ" \
       --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
       --data-urlencode "model_id=test_model" \
       --data-urlencode "response_type=code" \
       --data-urlencode "state=FKjaJfMlakjdfTVbES5ccZ"
</code></pre>

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Guest 모드를 사용하려면 제휴 담당자에게 연락하시기 바랍니다.</p>
</div>
