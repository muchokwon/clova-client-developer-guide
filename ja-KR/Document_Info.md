# このドキュメントについて
このドキュメントは、Clovaクライアント開発をサポートするために、クライアントデバイスのデザインガイドラインとCICプラットフォームについての開発ガイドおよびAPIリファレンスを提供します。対象となる読者は、CICを使用してClovaサービスと連携するデバイスやアプリを開発するクライアント開発者です。

<div class="note">
  <p><strong>メモ</strong></p>
  <p>Clovaは、現在も開発が進められています。このドキュメントの内容は随時変更される可能性があります。</p>
</div>

## お問い合わせ
ドキュメントの内容に関するお問い合わせは、指定のClova提携担当者、または<a href="{{ book.ServiceEnv.DeveloperCenterForumURI }}" target="_blank">{{ book.ServiceEnv.DeveloperCenterName }}フォーラム</a>までお願いします。

## ドキュメントの変更履歴

このドキュメントの変更履歴は、以下のとおりです。

<table>
  <thead>
    <tr>
      <th style="width:12%">リリース日</th><th style="width:88%">変更履歴</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2019/07/31</td>
      <td>
        <ul>
          <li>メディア再生の制御対象を指定するために、<a href="/Develop/References/CICInterface/PlaybackController.md">PlaybackController</a>名前空間の<a href="/Develop/References/CICInterface/PlaybackController.md#ExpectNextCommand">ExpectNextCommand</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#ExpectPauseCommand">ExpectPauseCommand</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#ExpectPreviousCommand">ExpectPreviousCommand</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#ExpectResumeCommand">ExpectResumeCommand</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#ExpectStopCommand">ExpectStopCommand</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#Pause">Pause</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#Resume">Resume</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#Stop">Stop</a>ディレクティブに、、targetとtarget.namespaceフィールドを追加</li>
          <li><a href="/Design/Design_Guideline_For_Client_Hardware.md#SoundEffect">デザインガイドラインの効果音</a>で、Attending状態遷移時の効果音の再生を、必須ではなく任意に修正</li>
          <li><a href="/Design/Design_Guideline_For_Client_Hardware.md">デザインガイドライン</a>ドキュメントに<a href="/Design/Design_Guideline_For_Client_Hardware.md#ClovaInside">Clovaインサイド</a>に関する内容を追加</li>
          <li>ClovaクライアントデベロッパーガイドからClovaクライアントガイドに分割</li>
          <li>一部のドキュメントの誤りを訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/07/02</td>
      <td>
        <ul>
          <li><a href="/Design/Design_Guideline_For_Client_Hardware.md">クライアントデバイスのデザインガイドライン</a>の<a href="/Design/Design_Guideline_For_Client_Hardware.md#LightEffect">照明効果</a>の説明で、タイムアウト（timeout）に関連する照明効果の実装を任意に変更</li>
          <li><a href="/Develop/References/Clova_Auth_API.md">Clova認証APIのリファレンス</a>で、<a href="/Develop/References/Clova_Auth_API.md#RequestAuthorizationCode">認可コードをリクエストする(/authorize)</a>のredirect_uriの説明を更新</li>
          <li>メディア再生の制御対象を指定するために、<a href="/Develop/References/CICInterface/PlaybackController.md">PlaybackController</a>名前空間の<a href="/Develop/References/CICInterface/PlaybackController.md#NextCommandIssued">NextCommandIssued</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#PauseCommandIssued">PauseCommandIssued</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#PreviousCommandIssued">PreviousCommandIssued</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#ResumeCommandIssued">ResumeCommandIssued</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#StopCommandIssued">StopCommandIssued</a>イベントに、sourceとsource.namespaceフィールドを追加</li>
          <li>一部のサンプルの誤字・脱字を修正およびメモボックスのレベルを調整</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/06/10</td>
      <td>
        <ul>
          <li><a href="/Design/Design_Guideline_For_Client_Hardware.md">クライアントデバイスのデザインガイドライン</a>の<a href="/Design/Design_Guideline_For_Client_Hardware.md#GreenDotVUI">Green Dot VUI</a>の説明にサンプルを追加し、ロゴを更新</li>
          <li>特定のデバイスの状態を受信するために、<a href="/Develop/References/CICInterface/DeviceControl.md#RequestStateSynchronization">RequestStateSynchronization</a>イベントにdeviceIdフィールドを追加</li>
          <li>クライアントデバイスが登録されていないか、またはオフライン状態にあることを示すために、<a href="/Develop/References/CICInterface/DeviceControl.md#SynchronizeState">SynchronizeState</a>イベントのdeviceStateフィールドを条件付きに修正</li>
          <li><a href="/Develop/References/Clova_Auth_API.md">Clova認証APIのリファレンス</a>で、model_idをすべての項目に修正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/05/27</td>
      <td>
        <ul>
          <li><a href="/Design/Design_Guideline_For_Client_Hardware.md">クライアントデバイスのデザインガイドライン</a>のVoice Agentの説明を削除し、<a href="/Design/Design_Guideline_For_Client_Hardware.md#GreenDotVUI">Green Dot VUI</a>の説明を追加</li>
          <li>ClovaクライアントがCICに<a href="/Develop/References/Context_Objects.md">コンテキスト</a>を送信するとき、任意の情報を選択して送信するのではなく、送信できるすべての状態情報を送信するように説明を修正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/05/20</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md#ManageStreamInfo">ストリームの情報を管理する</a>セクションを<a href="/Develop/Guides/ImplementClientFeatures/Handle_Audio_Playback.md">オーディオ再生を処理する</a>ガイドに追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/05/04</td>
      <td>
        <ul>
          <li>記述に誤りがあった<a href="/Develop/References/CICInterface/PlaybackController.md#PlayCommandIssued">PlaybackController.PlayCommandIssued</a>と<a href="/Develop/References/CICInterface/PlaybackController.md#SetRepeatModeCommandIssued">PlaybackController.SetRepeatModeCommandIssued</a>イベントの説明を訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/04/19</td>
      <td>
        <ul>
          <li><a href="/Design/Design_Guideline_For_Client_Hardware.md">クライアントデバイスのデザインガイドライン</a>の<a href="/Design/Design_Guideline_For_Client_Hardware.md#AudioInterruptionRule">オーディオ再生の基本ルール</a>に、オーディオコンテンツを区別できるようにするため、オーディオコンテンツのタイプに関連するCIC APIの名前空間を追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/04/08</td>
      <td>
        <ul>
          <li><a href="/Develop/References/Clova_Auth_API.md">CIC認証API</a>の<a href="/Develop/References/Clova_Auth_API.md#RequestAuthorizationCode">認可コードをリクエストする</a>の説明に、レスポンスの「redirect_uri」フィールドに「error」パラメータの説明を追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/03/25</td>
      <td>
        <ul>
          <li>HLSストリームを提供するために、contentTypeフィールドをCIC APIの<a href="/Develop/References/CICInterface/SpeechSynthesizer.md#Speak">SpeechSynthesizer.Speak</a>ディレクティブに追加</li>
          <li>一部のリンクの誤りおよびサンプルを訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/03/13</td>
      <td>
        <ul>
          <li>一部のリファレンスドキュメントの内容の順序を修正</li>
          <li>内部・外部からのフィードバックをドキュメントに反映</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/02/26</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Implement_Client_Features.md">クライアントの機能を実装する</a>セクションの内容をそれぞれのページに分割</li>
          <li>UI要素を削除した個所に対し、URLの表現をURIに変更</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/01/25</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Interact_with_CIC.md">CICと連携する</a>ドキュメントの説明を一部改善</li>
          <li>ダイアグラムの一部のスタイルを修正し、表の列の間隔を調整</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/01/07</td>
      <td>
        <ul>
          <li>ドキュメントで使用されている一部のUMLダイアグラムのスタイルを統一</li>
          <li>ドキュメントの変更履歴のうち、一部の表記の誤りを訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019/01/04</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Implement_Client_Features.md#HandleDeviceControl">クライアントの動作制御を処理する</a>ガイドドキュメントを追加</li>
          <li><a href="/Develop/Guides/Implement_Client_Features.md#HandleBluetoothControl">クライアントのBluetooth制御を処理する</a>ガイドドキュメントを追加</li>
          <li><a href="/Develop/References/CICInterface/AudioPlayer.md">AudioPlayer API</a>に、再生キューのクリア動作をレポートするための<a href="/Develop/References/CICInterface/AudioPlayer.md#PlaybackQueueCleared">AudioPlayer.PlaybackQueueCleared</a>イベントを追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/12/24</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Implement_Client_Features.md#HandleDeviceControl">クライアントの動作制御を処理する</a>ガイドドキュメントを追加</li>
          <li><a href="/Develop/Guides/Implement_Client_Features.md#HandleBluetoothControl">クライアントのBluetooth制御を処理する</a>ガイドドキュメントを追加</li>
          <li><a href="/Develop/References/ContentTemplates/TodayWeather.md">TodayWeather</a>テンプレートに、現在の天気に関するnowTemperatureImageCodeフィールドとnowTemperatureImageUrlフィールドを追加</li>
          <li><a href="/Develop/References/ContentTemplates/TodayWeather.md">TodayWeather</a>テンプレートで、ドキュメントの誤りを訂正</li>
          <li>一部のシーケンスダイアグラムで、正しく表記されてないノード型を訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/11/30</td>
      <td>
        <ul>
          <li>ダイアログモデルの説明を<a href="/Develop/CIC_Overview.md#IndirectDialogue">間接ダイアログ</a>と<a href="/Develop/Guides/Implement_Client_Features.md#ManageDialogueIDAndHandleTasks">ダイアログIDを管理し、作業を処理する</a>に分け、内容を補充</li>
          <li><a href="/Develop/References/CICInterface/DeviceControl.md#Decrease">DeviceControl.Decrease</a>と<a href="/Develop/References/CICInterface/DeviceControl.md#Increase">DeviceControl.Increase</a>ディレクティブにvalueフィールドを追加し、デバイスの画面の明るさや音量を特定のレベルだけ調整することに対応</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/11/16</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Implement_Client_Features.md">クライアントの機能を実装する</a>に、設定情報に関する<a href="/Develop/Guides/Implement_Client_Features.md#HandleSettings">設定情報を処理する</a>ガイドを追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/11/09</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Implement_Client_Features.md">クライアントの機能を実装する</a>にオーディオ再生および再生のコントロールに関連する<a href="/Develop/Guides/Implement_Client_Features.md#HandleAudioPlayback">オーディオ再生を処理する</a>ガイドを追加（以下の内容を含む）
            <ul>
              <li><a href="/Develop/Guides/Implement_Client_Features.md#PlayAudioStream">オーディオを再生する</a></li>
              <li><a href="/Develop/Guides/Implement_Client_Features.md#ReportAudioPlaybackProgress">オーディオ再生の進行状況をレポートする</a></li>
              <li><a href="/Develop/Guides/Implement_Client_Features.md#ControlAudioPlayback">オーディオ再生をコントロールする</a></li>
              <li><a href="/Develop/Guides/Implement_Client_Features.md#ShareAudioPlaybackState">オーディオ再生状態を共有する</a></li>
            </ul>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/10/20</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/DeviceControl.md">DeviceControl</a>名前空間に、Bluetoothデバイスを再スキャンしたり、Bluetoothデバイスを削除する<a href="/Develop/References/CICInterface/DeviceControl.md#BtDelete">DeviceControl.BtDelete</a>と<a href="/Develop/References/CICInterface/DeviceControl.md#BtRescan">DeviceControl.BtRescan</a>ディレクティブを追加</li>
          <li><a href="/Develop/References/CICInterface/DeviceControl.md">DeviceControl</a>名前空間に、Bluetoothデバイスで音楽を再生するように指示する<a href="/Develop/References/CICInterface/DeviceControl.md#BtPlay">DeviceControl.BtPlay</a>ディレクティブを追加</li>
          <li><a href="/Develop/References/CICInterface/DeviceControl.md">DeviceControl</a>名前空間の<a href="/Develop/References/CICInterface/DeviceControl.md#BtConnect">DeviceControl.BtConnect</a>と<a href="/Develop/References/CICInterface/DeviceControl.md#BtDisconnect">DeviceControl.BtDisconnect</a>ディレクティブにフィールドを追加し、特定の役割を持つデバイスまたは特定のデバイスと接続したり、接続解除したりする機能を追加</li>
          <li><a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a>コンテキストオブジェクトの<a href="/Develop/References/Context_Objects.md#BluetoothInfoObject">BluetoothInfoObject</a>にconnecting、pairing、playerinfo、scanningフィールドを追加することで、クライアントのBluetooth関連状態情報を追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/10/05</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CIC_API.md#Error">CICエラーメッセージ</a>の構造およびサンプルを実際の実装に合わせて訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/09/21</td>
      <td>
        <ul>
          <li>コンテンツのMIMEタイプを表すために、<a href="/Develop/References/CICInterface/AudioPlayer.md">AudioPlayer</a> <a href="/Develop/References/CICInterface/AudioPlayer.md#AudioStreamInfoObject">AudioStreamInfoObject</a>のpayloadにformatフィールドを追加</li>
          <li>コンテンツを再生するときに、「いいね」およびチャンネル登録を処理するために、<a href="/Develop/References/CICInterface/TemplateRuntime.md">TemplateRuntime</a>名前空間にSubscribeCommandIssued、UnsubscribeCommandIssuedイベントとUpdateLike、UpdateSubscribeディレクティブを追加</li>
          <li>コンテンツを再生するときに表示するボタンやコントロールUIの種類を<a href="/Develop/References/CICInterface/TemplateRuntime.md#RenderPlayerInfo">TemplateRuntime.RenderPlayerInfo</a>ディレクティブに追加</li>
          <li>誤りがあった一部のサンプールコードを訂正</li>
          <li>誤りがあった一部のリンクを訂正</li>
          <li>ユーザータッチポイントにある一部のExtensionの表記をスキルに変更（UI画像を一緒に更新）</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/09/07</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Implement_Client_Features.md#HandleAlerts">アラームを処理する</a>セクションのリンクの誤り、サンプルコードの表記の誤りを訂正</li>
          <li>サンプルの説明で、「yourdomain.com」になっているサンプルをドキュメント作成用のドメイン名である「example.com」に変更</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/08/29</td>
      <td>
        <ul>
          <li><a href="/Design/Design_Guideline_For_Client_Hardware.md">クライアントデバイスのデザインガイドライン</a>で<a href="/Design/Design_Guideline_For_Client_Hardware.md#LightColor">ライトの色</a>のRGB値を変更</li>
          <li><a href="/Develop/Guides/Implement_Client_Features.md">クライアントの機能を実装する</a>セクションを追加</li>
          <li><a href="/Develop/References/CICInterface/AudioPlayer.md">AudioPlayer</a>、<a href="/Develop/References/CICInterface/TemplateRuntime.md">TemplateRuntime</a>名前空間で一部のフィールドを更新</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/08/24</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/Alerts.md#StopAlert">Alerts.StopAlert</a>ディレクティブのサンプルで誤りを訂正</li>
          <li>表記による混同を避けるために、<a href="/Develop/References/CICInterface/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a>ディレクティブのinitiator.inputSourceフィールドの説明を修正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/08/09</td>
      <td>
        <ul>
          <li>ダイアログモデルの説明を補充</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/07/23</td>
      <td>
        <ul>
          <li><a href="/Design/Design_Guideline_For_Client_Hardware.md">クライアントデバイスのデザインガイドライン</a>の<a href="/Design/Design_Guideline_For_Client_Hardware.md#SoundEffect">効果音</a>のうち、Attending状態遷移時の効果音を更新</li>
          <li><a href="/Develop/References/Clova_Auth_API.md">CIC認証API</a>の<a href="/Develop/References/Clova_Auth_API.md#RequestAuthorizationCode">認可コードをリクエストする</a>の説明に423 Lockedステータスコードを追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/07/09</td>
      <td>
        <ul>
          <li>クライアントデバイスの設定を更新および同期するために、<a href="/Develop/References/CICInterface/Settings.md">Settings</a>名前空間を追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/06/17</td>
      <td>
        <ul>
          <li>リアルタイム配信コンテンツを区別するために、<a href="/Develop/References/CICInterface/TemplateRuntime.md#RenderPlayerInfo">TemplateRuntime.RenderPlayerInfo</a>にisLiveフィールドを追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/05/21</td>
      <td>
        <ul>
          <li><a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a>コンテキストオブジェクトの<a href="/Develop/References/Context_Objects.md#BluetoothInfoObject">BluetoothInfoObject</a>に抜け落ちのフィールド（btlist[].role）を追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/05/14</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/Clova.md#LaunchURI">LaunchURI</a>ディレクティブをDeviceControl名前空間から<a href="/Develop/References/CICInterface/Clova.md">Clova</a>名前空間に移動</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/05/07</td>
      <td>
        <ul>
          <li>DeviceControl名前空間にLaunchURIディレクティブを追加</li>
          <li><a href="/Develop/References/CICInterface/DeviceControl.md">DeviceControl</a>名前空間の<a href="/Develop/References/CICInterface/DeviceControl.md#LaunchApp">LaunchApp</a>ディレクティブと<a href="/Develop/References/CICInterface/DeviceControl.md#OpenScreen">OpenScreen</a>ディレクティブのサポートを中止（Deprecated）</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/04/16</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a>イベントのwakeWordフィールドの説明およびAudio dataの説明を更新</li>
          <li><a href="/Develop/References/CICInterface/DeviceControl.md">DeviceControl</a>名前空間に<a href="/Develop/References/CICInterface/DeviceControl.md#Open">Open</a>ディレクティブを追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/04/09</td>
      <td>
        <ul>
          <li>クライアントデバイスのデザインガイドラインで、<a href="Design/Design_Guideline_For_Client_Hardware.md#BootingScreen">起動画面</a>の説明およびサンプル画像を更新</li>
                  </ul>
      </td>
    </tr>
    <tr>
      <td>2018/04/02</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/AudioPlayer.md">AudioPlayer</a>名前空間にメッセージ仕様を追加および一部のフィールドを更新
            <ul>
              <li><a href="/Develop/References/CICInterface/AudioPlayer.md#ExpectReportPlaybackState">AudioPlayer.ExpectReportPlaybackState</a>ディレクティブ、<a href="/Develop/References/CICInterface/AudioPlayer.md#ReportPlaybackState">AudioPlayer.ReportPlaybackStateイベント</a>{% if book.DocMeta.TargetReaderType == "Internal" %}、<a href="/Develop/References/CICInterface/AudioPlayer.md#RequestPlaybackState">AudioPlayer.RequestPlaybackState</a>イベント、<a href="/Develop/References/CICInterface/AudioPlayer.md#SynchronizePlaybackState">SynchronizePlaybackStateディレクティブ</a>{% endif %}を追加</li>
              <li><a href="/Develop/References/CICInterface/AudioPlayer.md#Play">AudioPlayer.Play</a>ディレクティブのpayloadフィールドの内容を更新</li>
              <li>ProgressReportXXX、PlayXXXの形式の名前を持っているイベントフィールドにtokenフィールド値を必須として追加</li>
            </ul>
          </li>
          <li><a href="/Develop/References/Context_Objects.md#PlaybackState">AudioPlayer.PlaybackState</a>コンテキストオブジェクトにrepeatModeを追加</li>
          <li><a href="/Develop/References/CICInterface/PlaybackController.md">PlaybackController</a>名前空間に、全部で12件のメッセージ仕様を追加
            <ul>
              <li><a href="/Develop/References/CICInterface/PlaybackController.md#PauseCommandIssued">PlaybackController.PauseCommandIssued</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#PlayCommandIssued">PlaybackController.PlayCommandIssued</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#ResumeCommandIssued">PlaybackController.ResumeCommandIssued</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#SetRepeatModeCommandIssued">PlaybackController.SetRepeatModeCommandIssued</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#StopCommandIssued">PlaybackController.StopCommandIssued</a>イベントを追加</li>
              <li><a href="/Develop/References/CICInterface/PlaybackController.md#ExpectNextCommand">PlaybackController.ExpectNextCommand</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#ExpectPauseCommand">PlaybackController.ExpectPauseCommand</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#ExpectPlayCommand">PlaybackController.ExpectPlayCommand</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#ExpectPreviousCommand">PlaybackController.ExpectPreviousCommand</a>、
<a href="/Develop/References/CICInterface/PlaybackController.md#ExpectResumeCommand">PlaybackController.ExpectResumeCommand</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#ExpectStopCommand">PlaybackController.ExpectStopCommand</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#SetRepeatMode">PlaybackController.SetRepeatMode</a>ディレクティブを追加</li>
              <li><a href="/Develop/References/CICInterface/PlaybackController.md#TurnOnRepeatMode">PlaybackController.TurnOnRepeatMode</a>ディレクティブと<a href="/Develop/References/CICInterface/PlaybackController.md#TurnOffRepeatMode">PlaybackController.TurnOffRepeatMode</a>は削除予定</li>
            </ul>
          </li>
          <li>メディアストリームのための情報と、プレイリストを表示するための再生メタデータを分離するため、<a href="/Develop/References/CICInterface/TemplateRuntime.md">TemplateRuntime</a>名前空間を追加</li>
          <li><a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a>の<a href="/Develop/References/Context_Objects.md#BluetoothInfoObject">BluetoothInfoObject</a>にscanlistフィールドを追加</li>
          <li>PINコードを使用する外部のBluetoothデバイスと接続できるように、<a href="/Develop/References/CICInterface/DeviceControl.md">DeviceControl</a>名前空間に<a href="/Develop/References/CICInterface/DeviceControl.md#BtConnectByPINCode">BtConnectByPINCode</a>ディレクティブと<a href="/Develop/References/CICInterface/DeviceControl.md#BtRequestForPINCode">BtRequestForPINCode</a>、<a href="/Develop/References/CICInterface/DeviceControl.md#BtRequestToCancelPinCodeInput">BtRequestToCancelPinCodeInput</a>イベントを追加</li>
          <li><a href="/Develop/References/CICInterface/DeviceControl.md">DeviceControl</a>名前空間の<a href="/Develop/References/CICInterface/DeviceControl.md#BtConnect">BtConnect</a>ディレクティブにpayloadを追加</li>
          <li>Clova Developer Centerの一部のUIを更新</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/03/19</td>
      <td>
        <ul>
          <li><a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a>に<a href="/Develop/References/Context_Objects.md#SoundOutputInfoObject">SoundOutputInfoObject</a>を追加</li>
          <li>ユーザーが設定した任意のコマンドを実行する<a href="/Develop/References/CICInterface/PlaybackController.md#CustomCommandIssued">CustomCommandIssued</a>イベントを<a href="/Develop/References/CICInterface/PlaybackController.md#CustomCommandIssued">PlaybackController</a>名前空間に追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/03/05</td>
      <td>
        <ul>
          <li><a  href="/Develop/References/CICInterface/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a>イベントのinitiatorフィールドの説明を修正</li>
          <li><a href="/Design/Design_Guideline_For_Client_Hardware.md">クライアントデバイスのデザインガイドライン</a>で、クライアントのステータスのうち、Hearingステータスの名前をListeningに修正</li>
          <li>クライアントデバイスのデザインガイドラインの<a href="/Design/Design_Guideline_For_Client_Hardware.md#Audio">音声</a>で、オーディオコンテンツのタイプにFeedbackタイプを追加し、関連ルールを説明に追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/02/26</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a>イベントのinitiatorフィールドにdeviceUUIDフィールドを追加</li>
          <li>アラームの同期に関連する<a href="/Develop/References/CICInterface/Alerts.md#RequestSynchronizeAlert">RequestSynchronizeAlert</a>イベントと<a href="/Develop/References/CICInterface/Alerts.md#SynchronizeAlert">SynchronizeAlert</a>ディレクティブを<a href="/Develop/References/CICInterface/Alerts.md">Alerts</a>名前空間に追加</li>
          <li>System名前空間から、アラームの同期に関連する一部のフィールドを削除する予定</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/02/19</td>
      <td>
        <ul>
          <li>ユーザーの呼び出しを正確に認識するために、<a href="/Develop/References/CICInterface/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a>イベントにinitiatorフィールドを追加</li>
          <li>リマインダーおよびアクション予約の内容を確認するために、<a href="/Develop/References/CICInterface/Alerts.md#SetAlert">Alerts.SetAlert</a>ディレクティブにlabelフィールドを追加</li>
          <li>リマインダーおよびアクション予約の内容を表示するために、<a href="/Develop/References/ContentTemplates/ActionTimer.md">ActionTimer</a>、<a href="/Develop/References/ContentTemplates/ActionTimerList.md">ActionTimerList</a>、<a href="/Develop/References/ContentTemplates/Reminder.md">Reminder</a>、<a href="/Develop/References/ContentTemplates/ReminderList.md">ReminderList</a>テンプレートにlabelフィールドを追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/02/05</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/AudioPlayer.md#AudioStreamInfoObject">AudioStreamInfoObject</a>のdurationInMillisecondsフィールドの説明を修正</li>
          <li><a href="/Develop/References/ContentTemplates/Atmosphere.md">Atmosphere</a>、<a href="/Develop/References/ContentTemplates/CardList.md">CardList</a>、<a href="/Develop/References/ContentTemplates/Humidity.md">Humidity</a>、<a href="/Develop/References/ContentTemplates/TodayWeather.md">TodayWeather</a>、<a href="/Develop/References/ContentTemplates/TomorrowWeather.md">TomorrowWeather</a>、<a href="/Develop/References/ContentTemplates/WeeklyWeather.md">WeeklyWeather</a>、<a href="/Develop/References/ContentTemplates/WindSpeed.md">WindSpeed</a>テンプレートにコンテンツの提供元に関連するフィールドなどの内容を追加</li>
          <li>一部のドキュメントの誤りを訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/01/29</td>
      <td>
        <ul>
          <li><a href="/Design/Design_Guideline_For_Client_Hardware.md">クライアントデバイスのデザインガイドライン</a>に<a href="/Design/Design_Guideline_For_Client_Hardware.md#SoundEffect">リマインダーの効果音</a>を追加</li>
          <li><a href="/Develop/References/CICInterface/Notifier.md">Notifier</a>名前空間に<a href="/Develop/References/CICInterface/Notifier.md#Notify">Notifier.Notify</a>イベントを追加およびその名前空間で使用されるメッセージのpayloadフィールドを更新</li>
          <li><a href="/Develop/References/Context_Objects.md#SpeechState">SpeechSynthesizer.SpeechState</a>および<a href="/Develop/References/CICInterface/SpeechSynthesizer.md">SpeechSynthesizer</a>名前空間に<a href="/Develop/References/CICInterface/SpeechSynthesizer.md#SpeechFinished">SpeechFinished</a>、<a href="/Develop/References/CICInterface/SpeechSynthesizer.md#SpeechStarted">SpeechStarted</a>、<a href="/Develop/References/CICInterface/SpeechSynthesizer.md#SpeechStopped">SpeechStopped</a>イベントを追加</li>
          <li>マルチターン対話のために、<a href="/Develop/References/CICInterface/TextRecognizer.md">TextRecognizer.Recognize</a>イベントにspeechId、explicitフィールドを追加</li>
          <li>Clova Developer Centerの一部のUIを更新</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/01/22</td>
      <td>
        <ul>
          <li>プラットフォームでサポートされるオーディオ圧縮形式を<a href="/Design/Design_Guideline_For_Client_Hardware.md#SupportedAudioCompressionFormat">クライアントデバイスのデザインガイドラインおよび</a>Extensionのデザインガイドラインに追加</li>
          <li>UMLダイアグラムの画像形式を変更</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/01/15</td>
      <td>
        <ul>
          <li><a href="/Design/Design_Guideline_For_Client_Hardware.md">クライアントデバイスのデザインガイドライン</a>にアラーム、リマインダー、タイマーの照明効果および効果音のガイドラインの説明を追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/01/08</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Implement_Client_Features.md#HandleDelegation">委任されたユーザーのリクエストを処理する</a>セクションを追加および<a href="/Develop/References/CICInterface/Clova.md#HandleDelegatedEvent">Clova.HandleDelegatedEvent</a>ディレクティブと<a href="/Develop/References/CICInterface/Clova.md#ProcessDelegatedEvent">Clova.ProcessDelegatedEvent</a>イベントを追加</li>
          <li><a href="/Develop/References/CICInterface/PlaybackController.md#NextCommandIssued">PlaybackController.NextCommandIssued</a>と<a href="/Develop/References/CICInterface/PlaybackController.md#PreviousCommandIssued">PlaybackController.PreviousCommandIssued</a>イベントに<a href="/Develop/References/Context_Objects.md#PlaybackState">AudioPlayer.PlaybackState</a>コンテキストを含めるように説明を追加</li>
          <li><a href="/Develop/References/CICInterface/Alerts.md">Alerts</a> APIの仕組みの説明を改善</li>
          <li><a href="/Develop/References/CICInterface/DeviceControl.md">DeviceControl</a> APIの<a href="/Develop/References/CICInterface/DeviceControl.md#DeviceControlWorkFlow">仕組み</a>の説明を追加</li>
          <li>一部のコンテンツテンプレートおよび共有オブジェクトのエラー訂正内容を修正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018/01/02</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CIC_API.md#EstablishDownchannel">Downchannelを構成する</a>セクションに、429エラーコードおよび関連説明を備考に追加</li>
          <li>ルート検索のテンプレート（CarRoute、TransportationRoute）を削除、ルート検索のUI表現をImageTextテンプレートで代替</li>
          <li>一部のドキュメントの誤りと誤字を訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/12/18</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/SpeechRecognizer.md">SpeechRecognizer</a>インターフェースからExpectSpeechTimedOutイベントを削除</li>
          <li><a href="/Develop/References/Context_Objects.md">コンテキスト</a>からClova.FreetalkStateオブジェクトを削除</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/12/11</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/AudioPlayer.md">AudioPlayer</a>インターフェースに<a href="/Develop/References/CICInterface/AudioPlayer.md#ClearQueue">ClearQueue</a>ディレクティブを追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/12/04</td>
      <td>
        <ul>
          <li><a href="/Design/Design_Guideline_For_Client_Hardware.md#AudioInterruptionRule">オーディオ再生のルール（audio interruption rule）</a>を<a href="/Design/Design_Guideline_For_Client_Hardware.md">クライアントデバイスのデザインガイドライン</a>に追加</li>
          <li><a href="/Design/Design_Guideline_For_Client_Hardware.md">クライアントデバイスのデザインガイドライン</a>の画像を改善</li>
          <li>CICと連携するための事前準備に<a href="/Develop/Guides/Interact_with_CIC.md#UserAgentString">User-Agent string</a>を追加</li>
          <li><a href="/Develop/References/CIC_API.md">CIC APIのリファレンス</a>の<a href="/Develop/References/CIC_API.md#SendEvent">イベントを送信する</a>セクションに、412 Precondition Failedステータスコードの説明を追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/11/20</td>
      <td>
        <ul>
          <li><a href="/Design/Design_Guideline_For_Client_Hardware.md">クライアントデバイスのデザインガイドライン</a>を追加</li>
          <li>オーディオコンテンツおよび画像のサムネイル表示のために、<a href="/Develop/References/ContentTemplates/CardList.md">CardListテンプレート</a>のsubType値にType5とType6を追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/11/13</td>
      <td>
        <ul>
          <li>音量のコントロールに関連するディレクティブ（<a href="/Develop/References/CICInterface/DeviceControl.md#Decrease">DeviceControl.Decrease</a>、<a href="/Develop/References/CICInterface/DeviceControl.md#Increase">DeviceControl.Increase</a>、<a href="/Develop/References/CICInterface/DeviceControl.md#SetValue">DeviceControl.SetValue</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#Mute">PlaybackController.Mute</a>、<a href="/Develop/References/CICInterface/PlaybackController.md#Unmute">PlaybackController.Unmute</a>）の備考にUX関連内容を追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/11/06</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/SpeechRecognizer.md#KeepRecording">SpeechRecognizer.KeepRecording</a>ディレクティブを追加</li>
          <li><a href="/Develop/References/Context_Objects.md#Display">Device.Display</a>コンテキストを追加</li>
          <li><a href="/Develop/References/ContentTemplates/ActionTimer.md">ActionTimer</a>、<a href="/Develop/References/ContentTemplates/ActionTimerList.md">ActionTimerList</a>、<a href="/Develop/References/ContentTemplates/Alarm.md">Alarm</a>、<a href="/Develop/References/ContentTemplates/AlarmList.md">AlarmList</a>、<a href="/Develop/References/ContentTemplates/Memo.md">Memo</a>、<a href="/Develop/References/ContentTemplates/MemoList.md">MemoList</a>、<a href="/Develop/References/ContentTemplates/Reminder.md">Reminder</a>、<a href="/Develop/References/ContentTemplates/ReminderList.md">ReminderList</a>、<a href="/Develop/References/ContentTemplates/Schedule.md">Schedule</a>、<a href="/Develop/References/ContentTemplates/ScheduleList.md">ScheduleList</a>、<a href="/Develop/References/ContentTemplates/Timer.md">Timer</a>、<a href="/Develop/References/ContentTemplates/TimerList.md">TimerList</a>テンプレートにtokenフィールドを追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/10/23</td>
      <td>
        <ul>
          <li><a href="/Develop/References/ContentTemplates/Text.md">Text</a>テンプレートにemotionCodeフィールドとmotionCodeフィールドを追加</li>
          <li><a href="/Develop/References/CICInterface/Alerts.md#SetAlert">Alerts.SetAlert</a>ディレクティブのassets[].urlフィールドの内容を変更</li>
          <li><a href="/Develop/References/CICInterface/AudioPlayer.md#StreamRequested">AudioPlayer.StreamRequested</a>イベントのサンプルの誤りを訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/10/16</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/PlaybackController.md">PlaybackController</a>名前空間に<a href="/Develop/References/CICInterface/PlaybackController.md#Replay">Replay</a>ディレクティブを追加</li>
          <li>アラームの同期の補足説明をアラームの仕組みセクションに追加</li>
          <li>contentフィールドを<a href="/Develop/References/Context_Objects.md#AlertsState">Alert.AlertsState</a>コンテキストの<a href="/Develop/References/Context_Objects.md#AlertInfoObject">AlertInfoObject</a>から削除</li>
          <li>一部のドキュメントで画像を修正およびドキュメントの誤りを訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/10/02</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/Alerts.md">Alerts</a>名前空間およびアラーム関連インターフェースを追加</li>
          <li><a href="/Develop/References/CICInterface/System.md">System</a>名前空間およびアラーム関連インターフェースを追加</li>
          <li><a href="/Develop/References/CICInterface/SpeechRecognizer.md#ExpectSpeech">SpeechRecognizer.ExpectSpeech</a>ディレクティブにexpectContentTypeフィールドを追加</li>
          <li><a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a>の<a href="/Develop/References/Context_Objects.md#VolumeInfoObject">VolumeInfoObject</a>にwarningフィールドを追加</li>
          <li><a href="/Develop/References/ContentTemplates/ActionTimer.md">ActionTimer</a>、<a href="/Develop/References/ContentTemplates/ActionTimerList.md">ActionTimerList</a>、<a href="/Develop/References/ContentTemplates/Alarm.md">Alarm</a>、<a href="/Develop/References/ContentTemplates/AlarmList.md">AlarmList</a>、<a href="/Develop/References/ContentTemplates/Memo.md">Memo</a>、<a href="/Develop/References/ContentTemplates/MemoList.md">MemoList</a>、<a href="/Develop/References/ContentTemplates/Reminder.md">Reminder</a>、<a href="/Develop/References/ContentTemplates/ReminderList.md">ReminderList</a>、<a href="/Develop/References/ContentTemplates/Schedule.md">Schedule</a>、<a href="/Develop/References/ContentTemplates/ScheduleList.md">ScheduleList</a>、<a href="/Develop/References/ContentTemplates/Timer.md">Timer</a>、<a href="/Develop/References/ContentTemplates/TimerList.md">TimerList</a>テンプレートを追加</li>
          <li><a href="/Develop/References/ContentTemplates/ImageText.md">ImageText</a>テンプレートの一部のサンプルコードを修正</li>
          <li><a href="/Develop/References/ContentTemplates/Popup.md">Popupテンプレート</a>の一部のフィールドを修正</li>
          <li><a href="/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken">Clovaアクセストークンを作成する</a>と<a href="/Develop/References/Clova_Auth_API.md#RequestAuthorizationCode">認可コードをリクエストする</a>にサービスの利用規約に関する内容を追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/09/25</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/PlaybackController.md">PlaybackController API</a>にオーディオ再生をコントロールするための<a href="/Develop/References/CICInterface/PlaybackController.md#NextCommandIssued">PlaybackController.NextCommandIssued</a>イベントと<a href="/Develop/References/CICInterface/PlaybackController.md#PreviousCommandIssued">PlaybackController.PreviousCommandIssued</a>イベントを追加</li>
          <li><a href="/Develop/References/CICInterface/SpeechRecognizer.md#ExpectSpeech">SpeechRecognizer.ExpectSpeech</a>ディレクティブにexpectSpeechIdフィールドを、<a href="/Develop/References/CICInterface/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a>イベントにspeechIdとexplicitフィールドを追加</li>
          <li><a href="/Develop/References/ContentTemplates/Popup.md">Popupテンプレート</a>を追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/09/18</td>
      <td>
        <ul>
          <li>DeviceControl APIに<a href="/Develop/References/CICInterface/DeviceControl.md#ExpectReportState">DeviceControl.ExpectReportState</a>ディレクティブ、<a href="/Develop/References/CICInterface/DeviceControl.md#ReportState">DeviceControl.ReportState</a>イベント、<a href="/Develop/References/CICInterface/DeviceControl.md#RequestStateSynchronization">DeviceControl.RequestStateSynchronization</a>イベントを追加およびDeviceControl.UpdateDeviceStateディレクティブを<a href="/Develop/References/CICInterface/DeviceControl.md#SynchronizeState">DeviceControl.SynchronizeState</a>に名前を変更</li>
          <li><a href="/Develop/References/ContentTemplates/Text.md">Text</a>テンプレートにitem3フィールドを追加</li>
          <li><a href="/Develop/References/CICInterface/AudioPlayer.md#Play">AudioPlayer.Play</a>ディレクティブに、コンテンツの提供元に関するsourceフィールドを追加</li>
          <li><a href="/Develop/References/CICInterface/AudioPlayer.md#AudioStreamInfoObject">AudioStreamInfoObject</a>のdurationInMillisecondsフィールドを追加</li>
          <li>Notifier名前空間および<a href="/Develop/References/CICInterface/Notifier.md#ClearIndicator">ClearIndicator</a>、<a href="/Develop/References/CICInterface/Notifier.md#SetIndicator">SetIndicator</a>ディレクティブを追加</li>
          <li><a href="/Develop/References/ContentTemplates/Atmosphere.md">空気情報（Atmosphere）テンプレート</a>を追加</li>
          <li>ライセンスの問題により、天気テンプレートのbgClipURLフィールドを使用できないという内容を追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/09/11</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/SpeechRecognizer.md#ExpectSpeech">SpeechRecognizer.ExpectSpeech</a>ディレクティブにexplicitフィールドを追加</li>
          <li><a href="/Develop/References/Content_Templates.md">コンテンツテンプレート</a>に<a href="/Develop/References/ContentTemplates/Common_Fields.md">共通フィールド</a>の仕様を追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/09/04</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/Clova.md#Help">Clova.Help</a>ディレクティブを追加</li>
          <li><a href="/Develop/References/CICInterface/DeviceControl.md#LaunchApp">DeviceControl.LaunchApp</a>ディレクティブを追加</li>
          <li>TextRecognizer名前空間および<a href="/Develop/References/CICInterface/TextRecognizer.md">TextRecognizer.Recognize</a>イベントを追加</li>
          <li><a href="/Develop/References/CIC_API.md">CIC API</a>の目次と説明を再作成</li>
          <li>CIC APIの内容を更新：リクエスト/レスポンスヘッダーのステータスコードを追加、REST APIのリファレンスドキュメント形式を適用</li>
          <li>一部のドキュメントの誤りを訂正</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/08/28</td>
      <td>
        <ul>
          <li>セットトップボックスのためのテレビチャンネル情報の仕様と電源状態情報の仕様を<a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a>と<a href="/Develop/References/CICInterface/DeviceControl.md">DeviceControl API</a>に追加</li>
          <li><a href="/Develop/References/CICInterface/DeviceControl.md">DeviceControl API</a>でtargetに使用される値を一部追加および変更：power、energysave、screenbrightness</li>
          <li><a href="/Develop/References/CICInterface/DeviceControl.md">DeviceControl API</a>のSetPointを<a href="/Develop/References/CICInterface/DeviceControl.md#SetValue">SetValue</a>に名前を変更</li>
          <li><a href="/Develop/References/Clova_Auth_API.md">Clova認証API</a>の内容を更新 - リクエスト/レスポンスヘッダーとステータスコードを追加、REST APIのリファレンスドキュメント形式を適用</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/08/21</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Interact_with_CIC.md#ManageConnection">アクセストークンを更新する</a>セクションを追加および/token APIの内容を更新</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/08/14</td>
      <td>
        <ul>
          <li>ダイアログモデルの説明を追加</li>
          <li><a href="/Develop/References/CICInterface/DeviceControl.md">DeviceControl</a> APIを追加</li>
          <li><a href="/Develop/References/Context_Objects.md">Device.DeviceState</a>のpayloadフィールドを追加：airplane、battery、bluetooth、brightness、flashLight、gps、powerSavingMode、soundMode、volume、wifi</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/08/04</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/Clova.md#Hello">Clova.Hello</a>ディレクティブを追加</li>
          <li><a href="/Develop/References/CICInterface/AudioPlayer.md#Play">AudioPlayer.Play</a>ディレクティブのAudioItemオブジェクトにtypeフィールドを追加</li>
          <li><a href="/Develop/References/CICInterface/AudioPlayer.md#AudioStreamInfoObject">AudioStreamInfoObject</a>オブジェクトにurlPlayableフィールドを追加</li>
          <li><a href="/Develop/References/CIC_API.md#Error">CICのエラーメッセージ</a>の仕様を追加</li>
          <li><a href="/Develop/References/CIC_API.md#MultipartMessage">マルチパートメッセージ</a>の内容を再作成</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/07/28</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/AudioPlayer.md">AudioPlayer</a>のPlayNext、Stopを削除（<a href="/Develop/References/CICInterface/PlaybackController.md">PlaybackController</a>に統合）</li>
          <li><a href="/Develop/References/CICInterface/PlaybackController.md">PlaybackController</a>のメッセージ名を変更（Mute、Next、Pause、Previous、Resume、Stop、Unmute、VolumeDown、VolumeUp）</li>
          <li>ルート検索テンプレートを追加：CarRoute、TransportationRoute</li>
          <li>天気テンプレートを追加：<a href="/Develop/References/ContentTemplates/Humidity.md">Humidity</a>、<a href="/Develop/References/ContentTemplates/TodayWeather.md">TodayWeather</a>、<a href="/Develop/References/ContentTemplates/TomorrowWeather.md">TomorrowWeather</a>、<a href="/Develop/References/ContentTemplates/WeeklyWeather.md">WeeklyWeather</a>、<a href="/Develop/References/ContentTemplates/WindSpeed.md">WindSpeed</a></li>
          </ul>
      </td>
    </tr>
    <tr>
      <td>2017/07/14</td>
      <td>
        <ul>
          <li><a href="/Develop/References/CICInterface/AudioPlayer.md#AudioStreamInfoObject">AudioStreamInfoObject</a>オブジェクトのbeginAtInMillisecondsフィールドを追加</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/06/19</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Interact_with_CIC.md#ManageConnection">接続を管理する</a>セクションの内容を更新（HTTP Pingフレームを使用できない場合）</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/06/08</td>
      <td>
        <ul>
          <li><a href="/Develop/Guides/Interact_with_CIC.md">CICと連携する</a>に<a href="/Develop/Guides/Interact_with_CIC.md#ManageConnection">接続を管理する</a>を追加（HTTP Ping）</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017/05/29</td>
      <td>
        <ul>
          <li>CICのドキュメントを作成</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>
