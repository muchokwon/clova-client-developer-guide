### 備考 {#GuestMode}

ユーザーに、{{ book.ServiceEnv.TargetServiceForClientAuth }}アカウント認証なしにゲストモードでサービスを提供するには、以下の手順に従います。

1. [Clovaアクセストークンを作成する](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)で説明されている手順のうち、ステップ1と2の説明を省略します。
2. ステップ3で[認可コードをリクエスト](#RequestAuthorizationCode)する際、次の内容を適用します。
  * リクエストヘッダーに`Authorization`フィールドを入力しません。
  * `request_vu`をクエリパラメーターとして追加し、`Y`に設定します。

<div class="tip">
  <p><strong>ヒント</strong></p>
  <p><code>request_vu</code>のデフォルト値は<code>N</code>で、{{ book.ServiceEnv.TargetServiceForClientAuth }}アカウントを認証してから使用するのが基本ポリシーです。</p>
</div>

上記の手順どおりにすると、ゲストモードのための認可コードを取得します。その認可コードを使用して、[Clovaアクセストークンを作成する](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken)で説明されている続きの手順に従うと、ゲストモードのためのClovaアクセストークンを取得できます。

次は、ゲストモードのための認可コードをリクエストするサンプルです。

<pre><code>$ curl {{ book.ServiceEnv.AuthServerBaseURI }}authorize \
       --data-urlencode "request_vu=Y" \
       --data-urlencode "client_id=c2Rmc2Rmc2FkZ2Fasdkjh234zZnNhZGZ" \
       --data-urlencode "device_id=aa123123d6-d900-48a1-b73b-aa6c156353206" \
       --data-urlencode "model_id=test_model" \
       --data-urlencode "response_type=code" \
       --data-urlencode "state=FKjaJfMlakjdfTVbES5ccZ"
</code></pre>

<div class="note">
  <p><strong>メモ</strong></p>
  <p>ゲストモードを使用するには、Clova事務局までお問い合わせください。</p>
</div>
