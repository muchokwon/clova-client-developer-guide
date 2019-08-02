# 지시 메시지 색인

| 네임스페이스          | 메시지 이름       | 설명                                             |
|--------------------|----------------|-------------------------------------------------|
| Alerts             | [`DeleteAlert`](/Develop/References/CICInterface/Alerts.md#DeleteAlert)             | 클라이언트에게 특정 알람을 삭제하도록 지시합니다. |
| Alerts             | [`SetAlert`](/Develop/References/CICInterface/Alerts.md#SetAlert)                   | 클라이언트에게 알람을 새로 추가하거나 특정 알람을 수정하도록 지시합니다. |
| Alerts             | [`StopAlert`](/Develop/References/CICInterface/Alerts.md#StopAlert)                 | 클라이언트에게 특정 알람을 중지하도록 지시합니다.  |
| Alerts             | [`SynchronizeAlert`](/Develop/References/CICInterface/Alerts.md#SynchronizeAlert)   | 클라이언트에게 `payload`에 있는 사용자의 알람 데이터를 동기화하도록 지시합니다.  |
| AudioPlayer        | [`ClearQueue`](/Develop/References/CICInterface/AudioPlayer.md#ClearQueue)          | 클라이언트에게 오디오 스트림 재생 대기열(queue)을 초기화하도록 지시합니다.                              |
| AudioPlayer        | [`ExpectReportPlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#ExpectReportPlaybackState) | 클라이언트에게 현재 재생 상태를 보고하도록 지시합니다. 클라이언트는 이 지시 메시지를 받으면 [`AudioPlayer.ReportPlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#ReportPlaybackState) 이벤트 메시지를 CIC로 전송해야 합니다. |
| AudioPlayer        | [`Play`](/Develop/References/CICInterface/AudioPlayer.md#Play)                      | 클라이언트에게 특정 오디오 스트림을 재생하거나 재생 대기열에 추가하도록 지시합니다.                          |
| AudioPlayer        | [`StreamDeliver`](/Develop/References/CICInterface/AudioPlayer.md#StreamDeliver)    | [`AudioPlayer.StreamRequested`](/Develop/References/CICInterface/AudioPlayer.md#StreamRequested) 이벤트 메시지의 응답이며, 실제 음악 재생이 가능한 오디오 스트림 정보를 수신해야 할 때 사용합니다. |
| AudioPlayer        | [`SynchronizePlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#SynchronizePlaybackState) | 클라이언트의 음원 재생 상태를 동기화하도록 지시합니다. [`AudioPlayer.RequestPlaybackState`](/Develop/References/CICInterface/AudioPlayer.md#RequestPlaybackState) 이벤트 메시지를 전송했던 클라이언트는 `AudioPlayer.SynchronizePlaybackState` 지시 메시지를 수신하게 됩니다. |
| Clova              | [`ExpectLogin`](/Develop/References/CICInterface/Clova.md#ExpectLogin)              | 클라이언트에게 사용자로부터 {{ book.ServiceEnv.OrientedService }} 계정 인증(login)을 받도록 지시합니다.          |
| Clova              | [`FinishExtension`](/Develop/References/CICInterface/Clova.md#FinishExtension)      | 클라이언트에게 특정 Extension을 종료하도록 지시합니다.                                             |
| Clova              | [`HandleDelegatedEvent`](/Develop/References/CICInterface/Clova.md#HandleDelegatedEvent) | 클라이언트에게 Clova 앱으로부터 [위임된 사용자의 요청을 처리](/Develop/Guides/ImplementClientFeatures/Handle_Delegation.md)하도록 지시합니다.   |
| Clova              | [`Hello`](/Develop/References/CICInterface/Clova.md#Hello)                          | 클라이언트에게 downchannel 연결 설정이 완료되었음을 알립니다.                                       |
| Clova              | [`Help`](/Develop/References/CICInterface/Clova.md#Help)                            | 클라이언트에게 미리 준비해둔 도움말 정보를 제공하도록 지시합니다.                                       |
| Clova              | [`LaunchURI`](/Develop/References/CICInterface/Clova.md#LaunchURI)     | 클라이언트에게 URI로 표현되는 사이트 혹은 앱을 열거나 실행하도록 지시합니다.       |
| Clova              | [`RenderTemplate`](/Develop/References/CICInterface/Clova.md#RenderTemplate)        | 클라이언트에게 템플릿을 표시하도록 지시합니다.                                                     |
| Clova              | [`RenderText`](/Develop/References/CICInterface/Clova.md#RenderText)                | 클라이언트에게 텍스트를 표시하도록 지시합니다.                                                     |
| Clova              | [`StartExtension`](/Develop/References/CICInterface/Clova.md#StartExtension)        | 클라이언트에게 특정 Extension을 시작하도록 지시합니다.                                             |
| DeviceControl      | [`BtConnect`](/Develop/References/CICInterface/DeviceControl.md#BtConnect)          | 클라이언트에게 특정 블루투스 기기와 연결을 설정하도록 지시합니다.                                       |
| DeviceControl      | [`BtConnectByPINCode`](/Develop/References/CICInterface/DeviceControl.md#BtConnectByPINCode) | 클라이언트에게 PIN 코드를 요청한 블루투스 기기와 연결하도록 지시합니다.                      |
| DeviceControl      | [`BtDelete`](/Develop/References/CICInterface/DeviceControl.md#BtDelete)            | 클라이언트에게 블루투스 페어링 목록에서 특정 기기를 제거하도록 지시합니다.                        |
| DeviceControl      | [`BtDisconnect`](/Develop/References/CICInterface/DeviceControl.md#BtDisconnect)    | 클라이언트에게 특정 블루투스 기기와 연결을 해제하도록 지시합니다.                                       |
| DeviceControl      | [`BtPlay`](/Develop/References/CICInterface/DeviceControl.md#BtPlay)                | 클라이언트에게 연결된 블루투스 기기를 통해 음악을 재생하도록 지시합니다.                          |
| DeviceControl      | [`BtRescan`](/Develop/References/CICInterface/DeviceControl.md#BtRescan)            | 클라이언트에게 블루투스 기기를 재탐지(rescan)하도록 지시합니다.                               |
| DeviceControl      | [`BtStartPairing`](/Develop/References/CICInterface/DeviceControl.md#BtStartPairing) | 클라이언트에게 블루투스 페어링을 시작하도록 지시합니다.                                              |
| DeviceControl      | [`BtStopPairing`](/Develop/References/CICInterface/DeviceControl.md#BtStopPairing)   | 클라이언트에게 블루투스 페어링을 중지하도록 지시합니다.                                              |
| DeviceControl      | [`Decrease`](/Develop/References/CICInterface/DeviceControl.md#Decrease)             | 클라이언트에게 스피커 볼륨 또는 화면 밝기를 기본 단위만큼 줄이도록 지시합니다.                            |
| DeviceControl      | [`ExpectReportState`](/Develop/References/CICInterface/DeviceControl.md#ExpectReportState) | 클라이언트에게 기기의 현재 상태를 CIC로 보고하도록 지시합니다.                                  |
| DeviceControl      | [`Increase`](/Develop/References/CICInterface/DeviceControl.md#Increase)             | 클라이언트에게 스피커 볼륨 또는 화면 밝기를 기본 단위만큼 높이도록 지시합니다.                            |
| DeviceControl      | [`LaunchApp`](/Develop/References/CICInterface/DeviceControl.md#LaunchApp)           | **(Deprecated)** 클라이언트에게 특정 앱을 실행하도록 지시합니다.                                                    |
| DeviceControl      | [`Open`](/Develop/References/CICInterface/DeviceControl.md#Open)                     | 클라이언트에게 특정 화면을 표시하도록 지시합니다.  |
| DeviceControl      | [`OpenScreen`](/Develop/References/CICInterface/DeviceControl.md#OpenScreen)         | **(Deprecated)** 클라이언트에게 설정 화면을 열도록 지시합니다.                                                     |
| DeviceControl      | [`SetValue`](/Develop/References/CICInterface/DeviceControl.md#SetValue)            | 클라이언트에게 스피커 볼륨 또는 화면 밝기를 지정한 값으로 설정하도록 지시합니다.                           |
| DeviceControl      | [`SynchronizeState`](/Develop/References/CICInterface/DeviceControl.md#SynchronizeState) | 클라이언트에게 사용자 계정에 등록된 또 다른 클라이언트 기기의 상태를 업데이트하도록 지시합니다.           |
| DeviceControl      | [`TurnOff`](/Develop/References/CICInterface/DeviceControl.md#TurnOff)               | 클라이언트에게 지정한 기능이나 모드를 끄거나 비활성화하도록 지시합니다.                                  |
| DeviceControl      | [`TurnOn`](/Develop/References/CICInterface/DeviceControl.md#TurnOn)                 | 클라이언트에게 지정한 기능을 켜거나 활성화하도록 지시합니다.                                          |
| Notifier           | [`ClearIndicator`](/Develop/References/CICInterface/Notifier.md#ClearIndicator)      | 클라이언트에게 알림을 나타내는 표시를 모두 끄도록 지시합니다.                                         |
| Notifier           | [`Notify`](/Develop/References/CICInterface/Notifier.md#Notify)                      | 클라이언트에게 알림 내용을 사용자에게 전달하도록 지시합니다.                                          |
| Notifier           | [`SetIndicator`](/Develop/References/CICInterface/Notifier.md#SetIndicator)          | 클라이언트에게 사용자가 읽지 않은 알림이 있음을 표시하도록 지시합니다.                                  |
| PlaybackController | [`ExpectNextCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectNextCommand)         | 사용자가 클라이언트 기기에서 다음 버튼(Next)을 누른 효과가 발생한 것처럼 클라이언트가 [`PlaybackController.NextCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#NextCommandIssued) 이벤트 메시지를 CIC로 보내도록 지시합니다.  |
| PlaybackController | [`ExpectPauseCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectPauseCommand)       | 사용자가 클라이언트 기기에서 일시 정지 버튼(Pause)을 누른 효과가 발생한 것처럼 클라이언트가 [`PlaybackController.PauseCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#PauseCommandIssued) 이벤트 메시지를 CIC로 보내도록 지시합니다.  |
| PlaybackController | [`ExpectPlayCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectPlayCommand)         | 사용자가 클라이언트 기기에서 재생 버튼(Play)을 누른 효과가 발생한 것처럼 클라이언트가 [`PlaybackController.PlayCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#PlayCommandIssued) 이벤트 메시지를 CIC로 보내도록 지시합니다.  |
| PlaybackController | [`ExpectPreviousCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectPreviousCommand) | 사용자가 클라이언트 기기에서 이전 버튼(Previous)을 누른 효과가 발생한 것처럼 클라이언트가 [`PlaybackController.PreviousCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#PreviousCommandIssued) 이벤트 메시지를 CIC로 보내도록 지시합니다.  |
| PlaybackController | [`ExpectResumeCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectResumeCommand)     | 사용자가 클라이언트 기기에서 재개 버튼(Resume)을 누른 효과가 발생한 것처럼 클라이언트가 [`PlaybackController.ResumeCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#ResumeCommandIssued) 이벤트 메시지를 CIC로 보내도록 지시합니다.  |
| PlaybackController | [`ExpectStopCommand`](/Develop/References/CICInterface/PlaybackController.md#ExpectStopCommand)         | 사용자가 클라이언트 기기에서 정지 버튼(Stop)을 누른 효과가 발생한 것처럼 클라이언트가 [`PlaybackController.StopCommandIssued`](/Develop/References/CICInterface/PlaybackController.md#StopCommandIssued) 이벤트 메시지를 CIC로 보내도록 지시합니다.  |
| PlaybackController | [`Mute`](/Develop/References/CICInterface/PlaybackController.md#Mute)               | 클라이언트에게 스피커 볼륨을 음소거하도록 지시합니다.                                                |
| PlaybackController | [`Next`](/Develop/References/CICInterface/PlaybackController.md#Next)               | 클라이언트에게 재생 대기열에 있는 다음 오디오 스트림 재생하도록 지시합니다.                               |
| PlaybackController | [`Pause`](/Develop/References/CICInterface/PlaybackController.md#Pause)             | 클라이언트에게 재생 중인 오디오 스트림을 일시 정지하도록 지시합니다.                                    |
| PlaybackController | [`Previous`](/Develop/References/CICInterface/PlaybackController.md#Previous)       | 클라이언트에게 재생 대기열에 있는 이전 오디오 스트림을 재생하도록 지시합니다.                              |
| PlaybackController | [`Replay`](/Develop/References/CICInterface/PlaybackController.md#Replay)           | 클라이언트에게 오디오 스트림을 처음부터 다시 재생하도록 지시합니다.                                     |
| PlaybackController | [`Resume`](/Develop/References/CICInterface/PlaybackController.md#Resume)           | 클라이언트에게 오디오 스트림 재생을 재개하도록 지시합니다.                                            |
| PlaybackController | [`SetRepeatMode`](/Develop/References/CICInterface/PlaybackController.md#SetRepeatMode) | 클라이언트에게 지정된 반복 모드로 재생 상태를 변경하도록 지시합니다.                                |
| PlaybackController | [`Stop`](/Develop/References/CICInterface/PlaybackController.md#Stop)               | 클라이언트에게 오디오 스트림 재생을 중지하도록 지시합니다.                                            |
| PlaybackController | [`TurnOffRepeatMode`](/Develop/References/CICInterface/PlaybackController.md#TurnOffRepeatMode) | **(Deprecated)** 클라이언트에게 한 곡 반복 재생 모드를 끄도록 지시합니다.                |
| PlaybackController | [`TurnOnRepeatMode`](/Develop/References/CICInterface/PlaybackController.md#TurnOnRepeatMode) | **(Deprecated)** 클라이언트에게 한 곡 반복 재생 모드를 켜도록 지시합니다.                  |
| PlaybackController | [`Unmute`](/Develop/References/CICInterface/PlaybackController.md#Unmute)           | 클라이언트에게 스피커 볼륨의 음소거를 해제하도록 지시합니다.                                           |
| PlaybackController | [`VolumeDown`](/Develop/References/CICInterface/PlaybackController.md#VolumeDown)   | **(Deprecated)** 클라이언트에게 스피커 볼륨을 낮추도록 지시합니다.                                                   |
| PlaybackController | [`VolumeUp`](/Develop/References/CICInterface/PlaybackController.md#VolumeUp)       | **(Deprecated)** 클라이언트에게 스피커 볼륨을 높이도록 지시합니다.                                                   |
| Settings           | [`ExpectReport`](/Develop/References/CICInterface/Settings.md#ExpectReport)                                                 | 클라이언트에게 현재의 설정 정보를 보고하도록 지시합니다. 클라이언트는 이 지시 메시지를 받으면 [`Settings.Report`](/Develop/References/CICInterface/Settings.md#Report) 이벤트 메시지를 CIC로 전송해야 합니다. |
| Settings           | [`Update`](/Develop/References/CICInterface/Settings.md#Update)                                                             | 클라이언트에게 `payload`에 저장된 값을 설정값으로 적용하도록 지시합니다.  |
| SpeechRecognizer   | [`ExpectSpeech`](/Develop/References/CICInterface/SpeechRecognizer.md#ExpectSpeech) | 클라이언트에게 사용자의 음성 입력을 대기하도록 지시합니다.                                            |
| SpeechRecognizer   | [`KeepRecording`](/Develop/References/CICInterface/SpeechRecognizer.md#KeepRecording) | 클라이언트에게 음성 입력을 계속 받도록 지시합니다.                                                |
| SpeechRecognizer   | [`ShowRecognizedText`](/Develop/References/CICInterface/SpeechRecognizer.md#ShowRecognizedText) | 클라이언트에게 인식된 사용자 음성을 실시간으로 전달합니다.                                |
| SpeechRecognizer   | [`StopCapture`](/Develop/References/CICInterface/SpeechRecognizer.md#StopCapture)   | 클라이언트에게 사용자의 음성 인식을 중지하도록 지시합니다.                                            |
| SpeechSynthesizer  | [`Speak`](/Develop/References/CICInterface/SpeechSynthesizer.md#Speak)                 | 클라이언트에게 합성된 TTS를 스피커로 출력하도록 지시합니다.                                |
| System             | [`SynchronizeState`](/Develop/References/CICInterface/System.md#SynchronizeState) | 클라이언트에게 `payload`에 있는 데이터를 동기화하도록 지시합니다.                                   |
| TemplateRuntime    | [`ExpectRequestPlayerInfo`](/Develop/References/CICInterface/TemplateRuntime.md#ExpectRequestPlayerInfo)  | 클라이언트에게 재생 메타 정보를 요청하도록 지시합니다. 클라이언트는 이 지시 메시지를 받으면 [`TemplateRuntime.RequestPlayerInfo`](/Develop/References/CICInterface/TemplateRuntime.md#RequestPlayerInfo) 이벤트 메시지를 CIC로 전송해야 합니다. |
| TemplateRuntime    | [`RenderPlayerInfo`](/Develop/References/CICInterface/TemplateRuntime.md#RenderPlayerInfo)                | CIC가 클라이언트에게 미디어 플레이어에 표시할 재생 목록, 앨범 이미지, 가사와 같은 재생 메타 정보를 전달하고 이를 표시하도록 지시합니다. |
| TemplateRuntime    | [`UpdateLike`](/Develop/References/CICInterface/TemplateRuntime.md#UpdateLike)                            | 클라이언트에게 미디어 플레이어에서 특정 미디어에 대한 사용자의 좋아요(like) 여부에 따라 좋아요 표시 상태를 지정된 값으로 업데이트하도록 지시합니다.  |
| TemplateRuntime    | [`UpdateSubscribe`](/Develop/References/CICInterface/TemplateRuntime.md#UpdateSubscribe)                  | 클라이언트에게 미디어 플레이어에서 특정 미디어에 사용자의 구독(subscribe) 여부에 따라 구독 표시 상태를 지정된 값으로 업데이트하도록 지시합니다.  |
