# ディレクティブの索引

| 名前空間          | メッセージ       | 説明                                             |
|--------------------|----------------|-------------------------------------------------|
| Alerts             | [`DeleteAlert`](/Develop/References/CICInterface/Alerts.md#DeleteAlert)             | クライアントに対して、特定のアラームを削除するように指示します。 |
| Alerts             | [`SetAlert`](/Develop/References/CICInterface/Alerts.md#SetAlert)                   | クライアントに対して、アラームを追加するか、または特定のアラームを編集するように指示します。 |
| Alerts             | [`StopAlert`](/Develop/References/CICInterface/Alerts.md#StopAlert)                 | クライアントに対して、特定のアラームを停止するように指示します。  |
| Alerts             | [`SynchronizeAlert`](/Develop/References/CICInterface/Alerts.md#SynchronizeAlert)   | クライアントに対して、`payload`内にあるユーザーのアラームデータを同期するように指示します。  |
| AudioPlayer        | [`ClearQueue`](/Develop/References/CICInterface/AudioPlayer.md#ClearQueue)          | クライアントに対して、オーディオストリームの再生キューをクリアするように指示します。                              |
| AudioPlayer        | [`ExpectReportPlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#ExpectReportPlaybackState) | クライアントに対して、現在の再生状態をレポートするように指示します。クライアントはこのディレクティブを受信すると、[`AudioPlayer.ReportPlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#ReportPlaybackState)イベントをCICに送信する必要があります。 |
| AudioPlayer        | [`Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)                      | クライアントに対して、特定のオーディオストリームを再生するか、または再生キューに追加するように指示します。                          |
| AudioPlayer        | [`StreamDeliver`](/Develop/References/CICInterface/AudioPlayer.md#StreamDeliver)    | [`AudioPlayer.StreamRequested`](/Develop/References/CICInterface/AudioPlayer.md#StreamRequested)イベントに対する応答です。実際に再生できるオーディオストリームの情報を受信するために使用します。 |
| AudioPlayer        | [`SynchronizePlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#SynchronizePlaybackState) | クライアントのストリーム再生状態を同期するように指示します。[`AudioPlayer.RequestPlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#RequestPlaybackState)イベントを送信したクライアントは、`AudioPlayer.SynchronizePlaybackState`ディレクティブを受信します。 |
{% if book.DocMeta.TargetReaderType == "Internal" or book.DocMeta.TargetReaderType == "Uplus" -%}
| Clova              | [`ExpectLogin`](/Develop/References/CICInterface/Clova.md#ExpectLogin)              | クライアントに対して、ユーザーの{{ book.ServiceEnv.OrientedService }}アカウント認証（ログイン）を行うように指示します。          |
{% endif -%}
| Clova              | [`FinishExtension`](/Develop/References/CICInterface/Clova.md#FinishExtension)      | クライアントに対して、特定のExtensionを終了するように指示します。                                             |
| Clova              | [`HandleDelegatedEvent`](/Develop/References/CICInterface/Clova.md#HandleDelegatedEvent) | クライアントに対して、Clovaアプリから[委任されたユーザーのリクエストを処理する](/Develop/Guides/ImplementClientFeatures/Handle_Delegation.md)ように指示します。   |
| Clova              | [`Hello`](/Develop/References/CICInterface/Clova.md#Hello)                          | クライアントに対して、Downchannelが確立したことを通知します。                                       |
| Clova              | [`Help`](/Develop/References/CICInterface/Clova.md#Help)                            | クライアントに対して、あらかじめ用意されたヘルプを提供するように指示します。                                       |
| Clova              | [`LaunchURI`](/Develop/References/CICInterface/Clova.md#LaunchURI)     | クライアントに、URIで表現されるウェブサイトまたはアプリを開いたり、起動するように指示します。       |
| Clova              | [`RenderTemplate`](/Develop/References/CICInterface/Clova.md#RenderTemplate)        | クライアントに対して、テンプレートを表示するように指示します。                                                     |
| Clova              | [`RenderText`](/Develop/References/CICInterface/Clova.md#RenderText)                | クライアントに対して、テキストを表示するように指示します。                                                     |
| Clova              | [`StartExtension`](/Develop/References/CICInterface/Clova.md#StartExtension)        | クライアントに対して、特定のExtensionを起動するように指示します。                                             |
| DeviceControl      | [`BtConnect`](/Develop/References/CICInterface/DeviceControl.md#BtConnect)          | クライアントに、特定のBluetoothデバイスと接続するように指示します。                                       |
| DeviceControl      | [`BtConnectByPINCode`](/Develop/References/CICInterface/DeviceControl.md#BtConnectByPINCode) | クライアントに、PINコードを要求したBluetoothデバイスと接続するように指示します。                      |
| DeviceControl      | [`BtDelete`](/Develop/References/CICInterface/DeviceControl.md#BtDelete)            | クライアントに、Bluetoothペアリングリストから特定のデバイスを削除するように指示します。                        |
| DeviceControl      | [`BtDisconnect`](/Develop/References/CICInterface/DeviceControl.md#BtDisconnect)    | クライアントに、特定のBluetoothデバイスとの接続を解除するように指示します。                                       |
| DeviceControl      | [`BtPlay`](/Develop/References/CICInterface/DeviceControl.md#BtPlay)                | クライアントに、接続しているBluetoothデバイスでストリームを再生するように指示します。                          |
| DeviceControl      | [`BtRescan`](/Develop/References/CICInterface/DeviceControl.md#BtRescan)            | クライアントに、Bluetoothデバイスを再スキャンするように指示します。                               |
| DeviceControl      | [`BtStartPairing`](/Develop/References/CICInterface/DeviceControl.md#BtStartPairing) | クライアントに、Bluetoothペアリングを開始するように指示します。                                              |
| DeviceControl      | [`BtStopPairing`](/Develop/References/CICInterface/DeviceControl.md#BtStopPairing)   | クライアントに、Bluetoothペアリングを解除するように指示します。                                              |
| DeviceControl      | [`Decrease`](/Develop/References/CICInterface/DeviceControl.md#Decrease)             | クライアントに、スピーカーの音量または画面の明るさを、基本値だけ下げるように指示します。                            |
| DeviceControl      | [`ExpectReportState`](/Develop/References/CICInterface/DeviceControl.md#ExpectReportState) | クライアントに、デバイスの現在の状態をCICにレポートするように指示します。                                  |
| DeviceControl      | [`Increase`](/Develop/References/CICInterface/DeviceControl.md#Increase)             | クライアントに、スピーカーの音量または画面の明るさを、基本値だけ上げるように指示します。                            |
| DeviceControl      | [`LaunchApp`](/Develop/References/CICInterface/DeviceControl.md#LaunchApp)           | **（Deprecated）** クライアントに、特定のアプリを起動するように指示します。                                                    |
| DeviceControl      | [`Open`](/Develop/References/CICInterface/DeviceControl.md#Open)                     | クライアントに、特定の画面を表示するように指示します。  |
| DeviceControl      | [`OpenScreen`](/Develop/References/CICInterface/DeviceControl.md#OpenScreen)         | **（Deprecated）** クライアントに、設定画面を開くように指示します。                                                     |
| DeviceControl      | [`SetValue`](/Develop/References/CICInterface/DeviceControl.md#SetValue)            | クライアントに、スピーカーの音量または画面の明るさを、指定された値に設定するように指示します。                           |
| DeviceControl      | [`SynchronizeState`](/Develop/References/CICInterface/DeviceControl.md#SynchronizeState) | クライアントに、ユーザーのアカウントに登録されている他のクライアントデバイスの状態を更新するように指示します。           |
| DeviceControl      | [`TurnOff`](/Develop/References/CICInterface/DeviceControl.md#TurnOff)               | クライアントに、指定された機能やモードをオフにしたり、または無効にするように指示します。                                  |
| DeviceControl      | [`TurnOn`](/Develop/References/CICInterface/DeviceControl.md#TurnOn)                 | クライアントに、指定された機能をオンにしたり、有効にしたりするように指示します。                                          |
| Notifier           | [`ClearIndicator`](/Develop/References/CICInterface/Notifier.md#ClearIndicator)      | クライアントに、通知インジケーターをすべてクリアするように指示します。                                         |
| Notifier           | [`Notify`](/Develop/References/CICInterface/Notifier.md#Notify)                      | クライアントに、通知の内容をユーザーに伝えるように指示します。                                          |
| Notifier           | [`SetIndicator`](/Develop/References/CICInterface/Notifier.md#SetIndicator)          | クライアントに、未読の通知があることを表示するように指示します。                                  |
| PlaybackController | [`ExpectNextCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectNextCommand)         | ユーザーがクライアント上で「次」ボタン（Next）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.NextCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#NextCommandIssued)イベントをCICに送信するように指示します。  |
| PlaybackController | [`ExpectPauseCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectPauseCommand)       | ユーザーがクライアント上で「一時停止」ボタン（Pause）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.PauseCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#PauseCommandIssued)イベントをCICに送信するように指示します。  |
| PlaybackController | [`ExpectPlayCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectPlayCommand)         | ユーザーがクライアント上で「再生」ボタン（Play）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.PlayCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#PlayCommandIssued)イベントをCICに送信するように指示します。  |
| PlaybackController | [`ExpectPreviousCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectPreviousCommand) | ユーザーがクライアント上で「前」ボタン（Previous）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.PreviousCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#PreviousCommandIssued)イベントをCICに送信するように指示します。  |
| PlaybackController | [`ExpectResumeCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectResumeCommand)     | ユーザーがクライアント上で「再開」ボタン（Resume）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.ResumeCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#ResumeCommandIssued)イベントをCICに送信するように指示します。  |
| PlaybackController | [`ExpectStopCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectStopCommand)         | ユーザーがクライアント上で「停止」ボタン（Stop）を押して、その効果が発生したときのように、クライアントが[`PlaybackController.StopCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#StopCommandIssued)イベントをCICに送信するように指示します。  |
| PlaybackController | [`Mute`](/Develop/References/CICInterface/PlaybackController.md#Mute)               | クライアントに、スピーカーをミュートにするように指示します。                                                |
| PlaybackController | [`Next`](/Develop/References/CICInterface/PlaybackController.md#Next)               | クライアントに、再生キューに入っている次のオーディオストリームを再生するように指示します。                               |
| PlaybackController | [`Pause`](/Develop/References/CICInterface/PlaybackController.md#Pause)             | クライアントに、再生中のオーディオストリームを一時停止するように指示します。                                    |
| PlaybackController | [`Previous`](/Develop/References/CICInterface/PlaybackController.md#Previous)       | クライアントに、再生キューにある前のオーディオストリームを再生するように指示します。                              |
| PlaybackController | [`Replay`](/Develop/References/CICInterface/PlaybackController.md#Replay)           | クライアントに、オーディオストリームを最初から再度再生するように指示します。                                     |
| PlaybackController | [`Resume`](/Develop/References/CICInterface/PlaybackController.md#Resume)           | クライアントに、オーディオストリームの再生を再開するように指示します。                                            |
| PlaybackController | [`SetRepeatMode`](/Develop/References/CICInterface/PlaybackController.md#SetRepeatMode) | クライアントに、指定されたリピート再生モードに再生状態を変更するように指示します。                                |
| PlaybackController | [`Stop`](/Develop/References/CICInterface/PlaybackController.md#Stop)               | クライアントに、オーディオストリームの再生を停止するように指示します。                                            |
| PlaybackController | [`TurnOffRepeatMode`](/Develop/References/CICInterface/PlaybackController.md#TurnOffRepeatMode) | **（Deprecated）** クライアントに、1曲リピート再生モードを無効にするように指示します。                |
| PlaybackController | [`TurnOnRepeatMode`](/Develop/References/CICInterface/PlaybackController.md#TurnOnRepeatMode) | **（Deprecated）** クライアントに、1曲リピート再生モードを有効にするように指示します。                  |
| PlaybackController | [`Unmute`](/Develop/References/CICInterface/PlaybackController.md#Unmute)           | クライアントに、スピーカーのミュートを解除するように指示します。                                           |
| PlaybackController | [`VolumeDown`](/Develop/References/CICInterface/PlaybackController.md#VolumeDown)   | **（Deprecated）** クライアントに、スピーカーの音量を下げるように指示します。                                                   |
| PlaybackController | [`VolumeUp`](/Develop/References/CICInterface/PlaybackController.md#VolumeUp)       | **（Deprecated）** クライアントに、スピーカーの音量を上げるように指示します。                                                   |
| Settings           | [`ExpectReport`](/Develop/References/CICInterface/Settings.md#ExpectReport)                                                 | クライアントに対して、現在の設定をレポートするように指示します。クライアントはこのディレクティブを受信すると、[`Settings.Report`](/Develop/References/CICInterface/Settings.md#Report)イベントをCICに送信する必要があります。 |
| Settings           | [`Update`](/Develop/References/CICInterface/Settings.md#Update)                                                             | クライアントに対して、`payload`に指定されている値を設定値として適用するように指示します。  |
| SpeechRecognizer   | [`ExpectSpeech`](/Develop/References/CICInterface/SpeechRecognizer.md#ExpectSpeech) | クライアントに、ユーザーの音声の取得を待機するように指示します。                                            |
| SpeechRecognizer   | [`KeepRecording`](/Develop/References/CICInterface/SpeechRecognizer.md#KeepRecording) | クライアントに、ユーザーの音声の取得を続けるように指示します。                                                |
{% if book.DocMeta.TargetReaderType == "Internal" or book.DocMeta.TargetReaderType == "Uplus" -%}
| SpeechRecognizer   | [`ShowRecognizedText`](/Develop/References/CICInterface/SpeechRecognizer.md#ShowRecognizedText) | クライアントに、認識されたユーザーの音声をリアルタイムで送信します。                                |
{% endif -%}
| SpeechRecognizer   | [`StopCapture`](/Develop/References/CICInterface/SpeechRecognizer.md#StopCapture)   | クライアントに、ユーザーの音声の認識を停止するように指示します。                                            |
| SpeechSynthesizer  | [`Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak)                 | クライアントに、合成された音声をスピーカーで出力するように指示します。                                |
| System             | [`SynchronizeState`](/Develop/References/CICInterface/System.md#SynchronizeState) | クライアントに、`payload`のデータを同期するように指示します。                                   |
| TemplateRuntime    | [`ExpectRequestPlayerInfo`](/Develop/References/CICInterface/TemplateRuntime.md#ExpectRequestPlayerInfo)  | クライアントに、再生メタデータをリクエストするように指示します。クライアントはこのディレクティブを受信すると、[`TemplateRuntime.RequestPlayerInfo`](/Develop/References/CICInterface/TemplateRuntime.md#RequestPlayerInfo)イベントをCICに送信する必要があります。 |
| TemplateRuntime    | [`RenderPlayerInfo`](/Develop/References/CICInterface/TemplateRuntime.md#RenderPlayerInfo)                | CICから、メディアプレーヤーに表示する再生リスト、アルバムの画像、歌詞のような再生メタデータをクライアントに送信し、表示するように指示します。 |
| TemplateRuntime    | [`UpdateLike`](/Develop/References/CICInterface/TemplateRuntime.md#UpdateLike)                            | クライアントに、メディアプレーヤー上で、特定のコンテンツに対するユーザーの「いいね」（like）の有無によって「いいね」の表示を指定された値に更新するように指示します。  |
| TemplateRuntime    | [`UpdateSubscribe`](/Develop/References/CICInterface/TemplateRuntime.md#UpdateSubscribe)                  | クライアントに、メディアプレーヤー上で、特定のコンテンツに対するユーザーのチャンネル登録（subscribe）有無によって、チャンネル登録の表示を指定された値に更新するように指示します。  |
