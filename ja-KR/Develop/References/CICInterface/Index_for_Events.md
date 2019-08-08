# イベントの索引

| 名前空間         | メッセージ       | 説明                                             |
|-------------------|----------------|-------------------------------------------------|
| Alerts            | [`AlertStarted`](/Develop/References/CICInterface/Alerts.md#AlertStarted)                 | クライアントから、アラームが開始したことをCICにレポートします。 |
| Alerts            | [`AlertStopped`](/Develop/References/CICInterface/Alerts.md#AlertStopped)                 | クライアントから、アラームが停止したことをCICにレポートします。 |
| Alerts            | [`DeleteAlertFailed`](/Develop/References/CICInterface/Alerts.md#DeleteAlertFailed)       | クライアントから、クライアントに設定されている特定のアラームを削除できなかったことをCICにレポートします。 |
| Alerts            | [`DeleteAlertSucceeded`](/Develop/References/CICInterface/Alerts.md#DeleteAlertSucceeded) | クライアントから、クライアントに設定されている特定のアラームを正常に削除したことをCICにレポートします。 |
| Alerts            | [`RequestAlertStop`](/Develop/References/CICInterface/Alerts.md#RequestAlertStop)         | クライアントから、アクティブなアラームを停止するようにCICにリクエストします。  |
| Alerts            | [`RequestSynchronizeAlert`](/Develop/References/CICInterface/Alerts.md#RequestSynchronizeAlert) | クライアントから、Clovaのクラウドに保存されたユーザーのアラームデータを同期する必要がある場合、CICにこのイベントを送信します。 |
| Alerts            | [`SetAlertFailed`](/Develop/References/CICInterface/Alerts.md#SetAlertFailed)             | クライアントから、特定のアラームを追加または編集できなかったことをCICにレポートします。 |
| Alerts            | [`SetAlertSucceeded`](/Develop/References/CICInterface/Alerts.md#SetAlertSucceeded)       | クライアントから、特定のアラームを正常に追加または編集したことをCICにレポートします。 |
| AudioPlayer       | [`PlaybackQueueCleared`](/Develop/References/CICInterface/AudioPlayer.md#PlaybackQueueCleared) | クライアントがCICから[`AudioPlayer.ClearQueue`](/Develop/References/CICInterface/AudioPlayer.md#ClearQueue)ディレクティブを受信した場合、再生キューをクリアし、`PlaybackQueueCleared`イベントを送信する必要があります。   |
| AudioPlayer       | [`PlayFinished`](/Develop/References/CICInterface/AudioPlayer.md#PlayFinished) | クライアントがオーディオストリームの再生を終了するとき、そのオーディオストリームの情報をCICにレポートするために使用します。
        |
| AudioPlayer       | [`PlayPaused`](/Develop/References/CICInterface/AudioPlayer.md#PlayPaused)     | クライアントがオーディオストリームの再生を一時停止するとき、そのオーディオストリームの情報をCICにレポートするために使用します。
    |
| AudioPlayer       | [`PlayResumed`](/Develop/References/CICInterface/AudioPlayer.md#PlayResumed)   | クライアントがオーディオストリームの再生を再開するとき、そのオーディオストリームの情報をCICにレポートするために使用します。            |
| AudioPlayer       | [`PlayStarted`](/Develop/References/CICInterface/AudioPlayer.md#PlayStarted)   | クライアントがオーディオストリームの再生を開始するとき、そのオーディオストリームの情報をCICにレポートするために使用します。
       |
| AudioPlayer       | [`PlayStopped`](/Develop/References/CICInterface/AudioPlayer.md#PlayStopped)   | クライアントがオーディオストリームの再生を停止するとき、そのオーディオストリームの情報をCICにレポートするために使用します。       |
| AudioPlayer       | [`ProgressReportDelayPassed`](/Develop/References/CICInterface/AudioPlayer.md#ProgressReportPositionPassed) | オーディオストリームの再生が開始してから、指定された遅延期間が経過したタイミングの再生状態（[`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)）をCICにレポートするために使用します。オーディオストリームの遅延期間は、[`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)ディレクティブがクライアントに送信されるときに確認できます。 |
| AudioPlayer       | [`ProgressReportIntervalPassed`](/Develop/References/CICInterface/AudioPlayer.md#ProgressReportPositionPassed)| オーディオストリームの再生が開始してから、指定された間隔ごとの再生状態（[`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)）を、CICにレポートします。レポートする間隔は、[`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)ディレクティブがクライアントに送信されるときに確認できます。|
| AudioPlayer       | [`ProgressReportPositionPassed`](/Develop/References/CICInterface/AudioPlayer.md#ProgressReportPositionPassed) | オーディオストリームの再生が開始してから、指定されたタイミングに、そのときの再生状態（[`AudioPlayer.PlaybackState`](/Develop/References/Context_Objects.md#PlaybackState)）をCICにレポートします。レポートするタイミングは、[`AudioPlayer.Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)ディレクティブがクライアントに送信されるときに確認できます。|
| AudioPlayer       | [`ReportPlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#ReportPlaybackState) | クライアントから、現在のストリーム再生状態をCICにレポートします。クライアントがCICから[`AudioPlayer.ExpectReportPlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#ExpectReportPlaybackState)ディレクティブを受信した場合、`AudioPlayer.ReportPlaybackState`イベントをCICに送信する必要があります。  |
{% if book.DocMeta.TargetReaderType == "Internal" -%}
| AudioPlayer       | [`RequestPlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#RequestPlaybackState) | クライアントのストリーム再生状態をCICにリクエストします。CICは、`AudioPlayer.RequestPlaybackState`イベントを受信した場合、ユーザーアカウントに登録されているすべての、または特定のクライアントに[`AudioPlayer.ExpectReportPlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#ExpectReportPlaybackState)ディレクティブを送信します。  |
{% endif -%}
| AudioPlayer       | [`StreamRequested`](/Develop/References/CICInterface/AudioPlayer.md#StreamRequested) | オーディオストリームを再生するために、ストリーミングのURLなど、追加の情報をCICにリクエストするイベントです。 |
| Clova              | [`ProcessDelegatedEvent`](/Develop/References/CICInterface/Clova.md#ProcessDelegatedEvent)                          | クライアントが、[委任されたユーザーのリクエスト](/Develop/Guides/ImplementClientFeatures/Handle_Delegation.md)の結果をCICから受信するために使用します。  |
| DeviceControl     | [`ActionExecuted`](/Develop/References/CICInterface/DeviceControl.md#ActionExecuted) | クライアントから、デバイスを正常にコントロールしたことをレポートします。                               |
| DeviceControl     | [`ActionFailed`](/Develop/References/CICInterface/DeviceControl.md#ActionFailed) | クライアントから、デバイスをコントロールできないか、またはコントロールに失敗したことをレポートします。                   |
| DeviceControl     | [`BtRequestForPINCode`](/Develop/References/CICInterface/DeviceControl.md#BtRequestForPINCode) | クライアントは、BluetoothデバイスからPINコードを要求される場合、このイベントでCICにリクエストを送信する必要があります。       |
| DeviceControl     | [`BtRequestToCancelPinCodeInput`](/Develop/References/CICInterface/DeviceControl.md#BtRequestToCancelPinCodeInput) | クライアントは、CICに対してPINコード入力の要求をキャンセルする場合、このイベントを送信する必要があります。 |
| DeviceControl     | [`ReportState`](/Develop/References/CICInterface/DeviceControl.md#ReportState)   | クライアントは、デバイスの現在の状態をレポートするとき、このメッセージを使用する必要があります。                              |
| DeviceControl     | [`RequestStateSynchronization`](/Develop/References/CICInterface/DeviceControl.md#RequestStateSynchronization) | ユーザーのアカウントに登録されている他のクライアントデバイスの現在の状態を確認しようとするとき、このイベントをCICに送信します。  |
| PlaybackController | [`CustomCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#CustomCommandIssued)               | ユーザーがクライアントデバイスで任意の機能を設定したボタンを押したり、タップした場合、クライアントはこのイベントをCICに送信する必要があります。  |
| PlaybackController | [`NextCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#NextCommandIssued)                   | ユーザーがクライアントデバイスで「次」ボタン（Next）を押したり、CICから[`PlaybackController.ExpectNextCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectNextCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。 |
| PlaybackController | [`PauseCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#PauseCommandIssued)                 | ユーザーがクライアントデバイスで「一時停止」ボタン（Pause）を押したり、CICから[`PlaybackController.ExpectPauseCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectPauseCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。  |
| PlaybackController | [`PlayCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#PlayCommandIssued)                   | ユーザーがクライアントデバイスで「再生」ボタン（Play）を押したり、CICから[`PlaybackController.ExpectPlayCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectPlayCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。  |
| PlaybackController | [`PreviousCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#PreviousCommandIssued)           | ユーザーがクライアントデバイスで「前へ」ボタン（Previous）を押したり、CICから[`PlaybackController.ExpectPreviousCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectPreviousCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。 |
| PlaybackController | [`ResumeCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#ResumeCommandIssued)               | ユーザーがクライアントデバイスで「再開」ボタン（Resume）を押したり、CICから[`PlaybackController.ExpectResumeCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectResumeCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。  |
| PlaybackController | [`SetRepeatModeCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#SetRepeatModeCommandIssued) | ユーザーがクライアントデバイスで「リピート再生」ボタン（Repeat）を押した場合、クライアントはこのイベントをCICに送信する必要があります。  |
| PlaybackController | [`StopCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#StopCommandIssued)                   | ユーザーがクライアントデバイスで「再開」ボタン（Resume）を押したり、CICから[`PlaybackController.ExpectStopCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectStopCommand)ディレクティブを受信した場合、クライアントはこのイベントをCICに送信する必要があります。  |
| Settings          | [`Report`](/Develop/References/CICInterface/Settings.md#Report)                                                    | クライアントが現在の設定をCICにレポートします。クライアントがCICから[Settings.ExpectReport](/Develop/References/CICInterface/Settings.md#ExpectReport)ディレクティブを受信した場合、`Settings.Report`イベントをCICに送信する必要があります。  |
| SpeechRecognizer  | [`Recognize`](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize)  | 取得されるユーザーの音声を送信して、その音声を認識するようにCICにリクエストします。                                          |
| SpeechSynthesizer | [`Request`](/Develop/References/CICInterface/SpeechSynthesizer.md#Request)     | CICに、特定のテキストを音声に合成するようにリクエストします。                                             |
| SpeechSynthesizer | [`SpeechFinished`](/Develop/References/CICInterface/SpeechSynthesizer.md#SpeechFinished)   | クライアントから、合成音声の再生を完了したことをCICにレポートします。                                 |
| SpeechSynthesizer | [`SpeechStarted`](/Develop/References/CICInterface/SpeechSynthesizer.md#SpeechStarted)     | クライアントから、合成音声の再生を開始したことをCICにレポートします。                                 |
| SpeechSynthesizer | [`SpeechStopped`](/Develop/References/CICInterface/SpeechSynthesizer.md#SpeechStopped)     | クライアントから、合成音声の再生を停止したことをCICにレポートします。                                 |
| System          | [`RequestSynchronizeState`](/Develop/References/CICInterface/System.md#RequestSynchronizeState) | クライアントから、Clovaのクラウドに保存された共有データを同期する必要がある場合、CICにこのイベントを送信します。 |
| TemplateRuntime    | [`LikeCommandIssued`](/Develop/References/CICInterface/TemplateRuntime.md#LikeCommandIssued) | ユーザーがクライアントデバイスのメディアプレーヤーで、特定のメディアに対して「いいね」ボタン（Like）を押した場合、クライアントはこのイベントをCICに送信する必要があります。 |
| TemplateRuntime    | [`RequestPlayerInfo`](/Develop/References/CICInterface/TemplateRuntime.md#RequestPlayerInfo) | クライアントから、メディアプレーヤーに表示する再生リスト、アルバムの画像、歌詞のような再生メタデータをCICにリクエストします。 |
| TemplateRuntime    | [`SubscribeCommandIssued`](/Develop/References/CICInterface/TemplateRuntime.md#SubscribeCommandIssued) | ユーザーがクライアントデバイスのメディアプレーヤーでチャンネル登録ボタン（Subscribe）を押した場合、クライアントはこのイベントをCICに送信する必要があります。  |
| TemplateRuntime    | [`UnlikeCommandIssued`](/Develop/References/CICInterface/TemplateRuntime.md#UnlikeCommandIssued) | ユーザーがクライアントデバイスのメディアプレーヤーで、特定のメディアに対して「いいね」を取り消すボタン（Unlike）を押した場合、クライアントはこのイベントをCICに送信する必要があります。 |
| TemplateRuntime    | [`UnsubscribeCommandIssued`](/Develop/References/CICInterface/TemplateRuntime.md#UnsubscribeCommandIssued) | ユーザーがクライアントデバイスのメディアプレーヤーでチャンネル登録解除ボタン（Unsubscribe）を押した場合、クライアントはこのイベントをCICに送信する必要があります。 |
| TextRecognizer  | [`Recognize`](/Develop/References/CICInterface/TextRecognizer.md#Recognize)      | ユーザーから取得したテキストをCICに送信して、ユーザーの意図を認識するようにリクエストします。                           |
