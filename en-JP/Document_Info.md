# README
This document provides Design guidelines and developer guide/API reference for CIC platforms in order to develop the Clova client. The intended audiences of this document are client developers using CIC to develop electronic devices and apps that link with the Clova services.

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Clova is under continuous development. Thus, the information in this document is subject to change at any time.</p>
</div>

## Contacts
For any inquiries about this document, contact the Clova partnership team or put your query in our <a href="{{ book.ServiceEnv.DeveloperCenterForumURI }}" target="_blank">{{ book.ServiceEnv.DeveloperCenterName }}forum</a>.

## Document revision history

The revision history of this document is as follows:

<table>
  <thead>
    <tr>
      <th style="width:15%">Release date</th><th style="width:75%">History</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2019-07-17</td>
      <td>
        <ul>
          <li>Added <a href="/Develop/Guides/Restrictions_On_Line_Music.md">Restrictions on LINE MUSIC</a> to the Development guide</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-07-03</td>
      <td>
        <ul>
          <li>Revised the <a href="/Develop/Guides/Interact_with_CIC.html#ClientAuthInfo">Client credentials</a> guide</li>
          <li>Revised the <a href="/Develop/Guides/Interact_with_CIC.html#CreateClovaAccessToken">Creating Clova access tokens</a> guide</li>
          <li>Revised the <a href="/Develop/References/Clova_Auth_API.md">Clova auth APreference</a> guide</li>
          <li>Revised some notation errors in the document history</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-01-07</td>
      <td>
        <ul>
          <li>Unified the style of some UML diagrams used in the document</li>
          <li>Revised some notation errors in the document history</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-01-04</td>
      <td>
        <ul>
          <li>Added the <a href=/Develop/Guides/Handle_Device_Control.md>Handling client action control</a> guide</li>
          <li>Added the <a href=/Develop/Guides/Handle_Bluetooth_Control.md>Handling client Bluetooth control</a> guide</li>
          <li>Added the <a href="/Develop/References/CICInterface/AudioPlayer.md#PlaybackQueueCleared">AudioPlayer.PlaybackQueueCleared</a> event to report the playback queue initialization action to the <a href="/Develop/References/CICInterface/AudioPlayer.md">AudioPlayer API</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-12-24</td>
      <td>
        <ul>
          <li>Added the <a href=/Develop/Guides/Handle_Device_Control.md>Handling client action control</a> guide</li>
          <li>Added the <a href=/Develop/Guides/Handle_Bluetooth_Control.md>Handling client Bluetooth control</a> guide</li>
          <li>Added the nowTemperatureImageCode field and the nowTemperatureImageUrl field related to the current weather information in the <a href="/Develop/References/ContentTemplates/TodayWeather.md">TodayWeather</a> template</li>
          <li>Revised documentation errors in the <a href="/Develop/References/ContentTemplates/TodayWeather.md">TodayWeather</a> template</li>
          <li>Added the link of the added document in <a href="/README.md">Before getting started</a></li>
          <li>Revised the notation error in the note type of some sequence diagrams</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-11-30</td>
      <td>
        <ul>
          <li>Separated the description of the dialogue model into <a href="/Develop/CIC_Overview.md#IndirectDialogue">Indirect dialogue structure</a> and <a href="/Develop/Guides/Manage_Dialogue_ID_And_Handle_Tasks.md">Handling tasks and managing dialogue IDs,</a>, and supplemented the content</li>
          <li>Added a value field to the <a href="/Develop/References/CICInterface/DeviceControl.md#Decrease">DeviceControl.Decrease</a> and <a href="/Develop/References/CICInterface/DeviceControl.md#Increase">DeviceControl.Increase</a> directives to support adjustment of the screen brightness or volume of a device by a specifiamount</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-11-16</td>
      <td>
        <ul>
          <li>Added a guide for <a href="/Develop/Guides/Handle_Settings.md">Handling settings</a> related to the settings information in the Development guide</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-11-09</td>
      <td>
        <ul>
          <li>Added the guide for <a href="/Develop/Guides/Handle_Audio_Playback.md">Handling audio playback</a> related to audio playback and playback control in the Development guide (and includes the following content)
            <ul>
              <li><a href="/Develop/Guides/Handle_Audio_Playback.md#PlayAudioStream">Playing audio stream</a></li>
              <li><a href="/Develop/Guides/Handle_Audio_Playback.md#ReportAudioPlaybackProgress">Reporting audio playback progress</a></li>
              <li><a href="/Develop/Guides/Handle_Audio_Playback.md#ControlAudioPlayback">Controlling audio playback</a></li>
              <li><a href="/Develop/Guides/Handle_Audio_Playback.md#ShareAudioPlaybackState">Sharing audio playback state</a></li>
            </ul>
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-10-20</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/References/CICInterface/DeviceControl.html#BtDelete">DeviceControl.BtDelete</a> and <a href="/Develop/References/CICInterface/DeviceControl.html#BtRescan">DeviceControl.BtRescan</a> directives to rescan or remove a Bluetooth device in the <a href="/Develop/References/CICInterface/DeviceControl.html">DeviceControl</a> namespace</li>
          <li>Added the <a href="/Develop/References/CICInterface/DeviceControl.html#BtPlay">DeviceControl.BtPlay</a> directive to play musithrough a Bluetooth device in the <a href="/Develop/References/CICInterface/DeviceControl.html">DeviceControl</a> namespace</li>
          <li>Added fields to the <a href="/Develop/References/CICInterface/DeviceControl.html#BtConnect">DeviceControl.BtConnect</a> and <a href="/Develop/References/CICInterface/DeviceControl.html#BtDisconnect">DeviceControl.BtDisconnect</a> directives of the <a href="/Develop/References/CICInterface/DeviceControl.html">DeviceControl</a> namespace to enable connection or disconnection of a specifidevice or a device with a specifirole</li>
          <li>Added connecting, pairing, playerinfo, and scanning fields to <a href="/Develop/References/Context_Objects.html#BluetoothInfoObject">BluetoothInfoObject</a> of the <a href="/Develop/References/Context_Objects.html#DeviceState">Device.DeviceState</a> context object to add Bluetooth-related state information of the client</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-10-05</td>
      <td>
        <ul>
          <li>Emended the structure and examples of <a href="/Develop/References/CIC_API.html#Error">CIerror messages</a> according to the actual implementation</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-09-21</td>
      <td>
        <ul>
          <li>Added the format field to the payload of <a href="/Develop/References/CICInterface/AudioPlayer.html">AudioPlayer</a> <a href="/Develop/References/CICInterface/AudioPlayer.html#AudioStreamInfoObject">AudioStreamInfoObject</a> to specify the MIME type of content</li>
          <li>Added SubscribeCommandIssued and UnsubscribeCommandIssued events, and UpdateLike and UpdateSubscribe directives to the <a href="/Develop/References/CICInterface/TemplateRuntime.html">TemplateRuntime</a> namespace to handle Like and Subscribe features when playing media content</li>
          <li>Added the button information that must be displayed when playing media content or the type of Ucontrol to the <a href="/Develop/References/CICInterface/TemplateRuntime.html#RenderPlayerInfo">TemplateRuntime.RenderPlayerInfo</a> directive</li>
          <li>Revised some wrong code examples</li>
          <li>Revised some incorrect links</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-09-07</td>
      <td>
        <ul>
          <li>Revised errors in links and mistakes in code examples in the <a href="/Develop/Guides/Handle_Alerts.md">Handling alerts</a> section</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-08-29</td>
      <td>
        <ul>
          <li>Changed the RGB value of the <a href="/Design/Light.md#LightColor">light colors</a> in the Design guidelines</li>
          <li>Updated some fields in the <a href="/Develop/References/CICInterface/AudioPlayer.html">AudioPlayer</a> and <a href="/Develop/References/CICInterface/TemplateRuntime.html">TemplateRuntime</a> namespaces</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-08-24</td>
      <td>
        <ul>
          <li>Revised an error in the example for the <a href="/Develop/References/CICInterface/Alerts.html#StopAlert">Alerts.StopAlert</a> directive</li>
          <li>Revised the description on the initiator.inputSource field of the <a href="/Develop/References/CICInterface/SpeechRecognizer.html#Recognize">SpeechRecognizer.Recognize</a> directive to avoid confusion</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-08-09</td>
      <td>
        <ul>
          <li>Supplemented the description of the dialogue model</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-07-23</td>
      <td>
        <ul>
          <li>Updated the sound effect of entering the attending state among the <a href="/Design/Audio.md#SoundEffect">sound effects</a> of the Design guidelines</li>
          <li>Added 423 Locked status code to the section on <a href="/Develop/References/Clova_Auth_API.html#RequestAuthorizationCode">Requesting an authorization code</a> under <a href="/Develop/References/Clova_Auth_API.html">CIauth API</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-07-09</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/References/CICInterface/Settings.html">Settings</a> namespace to update and synchronize the settings information of the client device</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-06-17</td>
      <td>
        <ul>
          <li>Added the isLive field to <a href="/Develop/References/CICInterface/TemplateRuntime.html#RenderPlayerInfo">TemplateRuntime.RenderPlayerInfo</a> to distinguish real-time broadcasting content</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-05-21</td>
      <td>
        <ul>
          <li>Added the missing field (btlist[].role) in <a href="/Develop/References/Context_Objects.html#BluetoothInfoObject">BluetoothInfoObject</a> of the <a href="/Develop/References/Context_Objects.html#DeviceState">Device.DeviceState</a> context object</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-05-14</td>
      <td>
        <ul>
          <li>Moved the <a href="/Develop/References/CICInterface/Clova.html#LaunchURI">LaunchURI</a> directive from the DeviceControl namespace to the <a href="/Develop/References/CICInterface/Clova.html">Clova</a> namespace</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-05-07</td>
      <td>
        <ul>
          <li>Added the LaunchURdirective to the DeviceControl namespace</li>
          <li>Deprecated the <a href="/Develop/References/CICInterface/DeviceControl.html#LaunchApp">LaunchApp</a> directive and the <a href="/Develop/References/CICInterface/DeviceControl.html#OpenScreen">OpenScreen</a> directive of the <a href="/Develop/References/CICInterface/DeviceControl.html">DeviceControl</a> namespace</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-04-16</td>
      <td>
        <ul>
          <li>Updated the description of the wakeWord field and audio data of the <a href="/Develop/References/CICInterface/SpeechRecognizer.html#Recognize">SpeechRecognizer.Recognize</a> event</li>
          <li>Added the <a href="/Develop/References/CICInterface/DeviceControl.html#Open">Open</a> directive to the <a href="/Develop/References/CICInterface/DeviceControl.html">DeviceControl</a> namespace</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-04-09</td>
      <td>
        <ul>
          <li>Updated the description and example image for the <a href="Design/Screen.md#BootingScreen">bootscreen</a> in the Design guidelines</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-04-02</td>
      <td>
        <ul>
          <li>Added message specifications to the <a href="/Develop/References/CICInterface/AudioPlayer.html">AudioPlayer</a> namespace and updated some fields
            <ul>
              <li>Added the <a href="/Develop/References/CICInterface/AudioPlayer.html#ExpectReportPlaybackState">AudioPlayer.ExpectReportPlaybackState</a> directive, <a href="/Develop/References/CICInterface/AudioPlayer.html#ReportPlaybackState">AudioPlayer.ReportPlaybackState event</a> {% if book.DocMeta.TargetReaderType == "Internal" %}, <a href="/Develop/References/CICInterface/AudioPlayer.html#RequestPlaybackState">AudioPlayer.RequestPlaybackState</a> event, and <a href="/Develop/References/CICInterface/AudioPlayer.html#SynchronizePlaybackState">SynchronizePlaybackState directive</a> {% endif %}</li>
              <li>Updated the payload field of the <a href="/Develop/References/CICInterface/AudioPlayer.html#Play">AudioPlayer.Play</a> directive</li>
              <li>Added a mandatory token field value to the event fields with name formats ProgressReportXXX and PlayXXX</li>
            </ul>
          </li>
          <li>Added repeatMode to the <a href="/Develop/References/Context_Objects.html#PlaybackState">AudioPlayer.PlaybackState</a> context object</li>
          <li>Added a total of 12 message specifications to the <a href="/Develop/References/CICInterface/PlaybackController.html">PlaybackController</a> namespace
            <ul>
              <li>Added the <a href="/Develop/References/CICInterface/PlaybackController.html#PauseCommandIssued">PlaybackController.PauseCommandIssued</a>, <a href="/Develop/References/CICInterface/PlaybackController.html#PlayCommandIssued">PlaybackController.PlayCommandIssued</a>, <a href="/Develop/References/CICInterface/PlaybackController.html#ResumeCommandIssued">PlaybackController.ResumeCommandIssued</a>, <a href="/Develop/References/CICInterface/PlaybackController.html#SetRepeatModeCommandIssued">PlaybackController.SetRepeatModeCommandIssued</a>, and <a href="/Develop/References/CICInterface/PlaybackController.html#StopCommandIssued">PlaybackController.StopCommandIssued</a> events</li>
              <li><a href="/Develop/References/CICInterface/PlaybackController.html#ExpectNextCommand">PlaybackController.ExpectNextCommand</a>, <a href="/Develop/References/CICInterface/PlaybackController.html#ExpectPauseCommand">PlaybackController.ExpectPauseCommand</a>, <a href="/Develop/References/CICInterface/PlaybackController.html#ExpectPlayCommand">PlaybackController.ExpectPlayCommand</a>, <a href="/Develop/References/CICInterface/PlaybackController.html#ExpectPreviousCommand">PlaybackController.ExpectPreviousCommand</a>, <a href="/Develop/References/CICInterface/PlaybackController.html#ExpectResumeCommand">PlaybackController.ExpectResumeCommand</a>, <a href="/Develop/References/CICInterface/PlaybackController.html#ExpectStopCommand">PlaybackController.ExpectStopCommand</a>, and <a href="/Develop/References/CICInterface/PlaybackController.html#SetRepeatMode">PlaybackController.SetRepeatMode</a> directives</li> The
              <li><a href="/Develop/References/CICInterface/PlaybackController.html#TurnOnRepeatMode">PlaybackController.TurnOnRepeatMode</a> and <a href="/Develop/References/CICInterface/PlaybackController.html#TurnOffRepeatMode">PlaybackController.TurnOffRepeatMode</a> directives are scheduled to be removed</li>
            </ul>
          </li>
          <li>Added the <a href="/Develop/References/CICInterface/TemplateRuntime.html">TemplateRuntime</a> namespace to separate the information for streaming media and metadata for displaying the play list</li>
          <li>Added the scanlist field to <a href="/Develop/References/Context_Objects.html#BluetoothInfoObject">BluetoothInfoObject</a> of <a href="/Develop/References/Context_Objects.html#DeviceState">Device.DeviceState</a></li>
          <li>Added the <a href="/Develop/References/CICInterface/DeviceControl.html#BtConnectByPINCode">BtConnectByPINCode</a> directive, <a href="/Develop/References/CICInterface/DeviceControl.html#BtRequestForPINCode">BtRequestForPINCode</a> event, and <a href="/Develop/References/CICInterface/DeviceControl.html#BtRequestToCancelPinCodeInput">BtRequestToCancelPinCodeInput</a> event to the <a href="/Develop/References/CICInterface/DeviceControl.html">DeviceControl</a> namespace to connect with third-party Bluetooth devices that use PIN codes</li>
          <li>Added a payload to the <a href="/Develop/References/CICInterface/DeviceControl.html#BtConnect">BtConnect</a> directive of the <a href="/Develop/References/CICInterface/DeviceControl.html">DeviceControl</a> namespace</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-03-19</td>
      <td>
        <ul>
          <li>Added <a href="/Develop/References/Context_Objects.html#SoundOutputInfoObject">SoundOutputInfoObject</a> to <a href="/Develop/References/Context_Objects.html#DeviceState">Device.DeviceState</a></li>
          <li>Added the <a href="/Develop/References/CICInterface/PlaybackController.html#CustomCommandIssued">CustomCommandIssued</a> event, which can execute customized commands of a user, to the <a href="/Develop/References/CICInterface/PlaybackController.html#CustomCommandIssued">PlaybackController</a> namespace</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-03-05</td>
      <td>
        <ul>
          <li>Modified the description of the initiator field of the <a href="/Develop/References/CICInterface/SpeechRecognizer.html#Recognize">SpeechRecognizer.Recognize</a> event</li>
          <li>Changed the name of the hearing state to listening state in the client states of the <a href="/Design/Screen.md#GreenDotVUI">Green Dot VUI</a></li>
          <li>Added Feedback type in the audio content type and explanation on rules in the <a href="/Design/Audio.md">Audio</a> section of the Design guidelines</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-02-26</td>
      <td>
        <ul>
          <li>Added the deviceUUID field to the initiator field of the <a href="/Develop/References/CICInterface/SpeechRecognizer.html#Recognize">SpeechRecognizer.Recognize</a> event</li>
          <li>Added the <a href="/Develop/References/CICInterface/Alerts.html#RequestSynchronizeAlert">RequestSynchronizeAlert</a> event and the <a href="/Develop/References/CICInterface/Alerts.html#SynchronizeAlert">SynchronizeAlert</a> directive related to alarm synchronization to the <a href="/Develop/References/CICInterface/Alerts.html">Alerts</a> namespace</li>
          <li>Scheduled to remove some fields related to alarm synchronization from the System namespace</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-02-19</td>
      <td>
        <ul>
          <li>Added the initiator field to the <a href="/Develop/References/CICInterface/SpeechRecognizer.html#Recognize">SpeechRecognizer.Recognize</a> event to accurately identify the user invocation</li>
          <li>Added the label field to the <a href="/Develop/References/CICInterface/Alerts.html#SetAlert">Alerts.SetAlert</a> directive to check details of reminders and scheduled actions</li>
          <li>Added the label field to the <a href="/Develop/References/ContentTemplates/ActionTimer.html">ActionTimer</a>, <a href="/Develop/References/ContentTemplates/ActionTimerList.html">ActionTimerList</a>, <a href="/Develop/References/ContentTemplates/Reminder.html">Reminder</a>, and <a href="/Develop/References/ContentTemplates/ReminderList.html">ReminderList</a> templates to display the details of reminders and scheduled actions</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-02-05</td>
      <td>
        <ul>
          <li>Modified the description of the durationInMilliseconds field of <a href="/Develop/References/CICInterface/AudioPlayer.html#AudioStreamInfoObject">AudioStreamInfoObject</a></li>
          <li>Added new information such as a field to state the source on the following templates: <a href="/Develop/References/ContentTemplates/Atmosphere.html">Atmosphere</a>, <a href="/Develop/References/ContentTemplates/CardList.html">CardList</a>, <a href="/Develop/References/ContentTemplates/Humidity.html">Humidity</a>, <a href="/Develop/References/ContentTemplates/TodayWeather.html">TodayWeather</a>, <a href="/Develop/References/ContentTemplates/TomorrowWeather.html">TomorrowWeather</a>, <a href="/Develop/References/ContentTemplates/WeeklyWeather.html">WeeklyWeather</a>, and <a href="/Develop/References/ContentTemplates/WindSpeed.html">WindSpeed</a></li>
          <li>Emended some errors in the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-29</td>
      <td>
        <ul>
          <li>Added <a href="/Design/Audio.md#SoundEffect">sound effects for reminders</a> in the Design guidelines</li>
          <li>Added the <a href="/Develop/References/CICInterface/Notifier.html#Notify">Notifier.Notify</a> event to the <a href="/Develop/References/CICInterface/Notifier.html">Notifier</a> namespace and updated the payload field of the namespace</li>
          <li>Added <a href="/Develop/References/CICInterface/SpeechSynthesizer.html#SpeechFinished">SpeechFinished</a>, <a href="/Develop/References/CICInterface/SpeechSynthesizer.html#SpeechStarted">SpeechStarted</a>, and <a href="/Develop/References/CICInterface/SpeechSynthesizer.html#SpeechStopped">SpeechStopped</a> event messages to the <a href="/Develop/References/Context_Objects.html#SpeechState">SpeechSynthesizer.SpeechState</a> and <a href="/Develop/References/CICInterface/SpeechSynthesizer.html">SpeechSynthesizer</a> namespaces</li>
          <li>Added speechId and explicit fields to the <a href="/Develop/References/CICInterface/TextRecognizer.html">TextRecognizer.Recognize</a> event for multi-turn dialogues</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-22</td>
      <td>
        <ul>
          <li>Added a section on supported audio compression formats in the <a href="/Design/Audio.html#SupportedAudioCompressionFormat">Design guidelines for client device</a> and <a href="{{ book.DocMeta.ClovaCustomExtensionDeveloperGuideBaseURI}}/Design/Design_Guideline_For_Extension.html#SupportedAudioCompressionFormat">Design guidelines for extensions</a></li>
          <li>Changed the image format of the UML diagram</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-15</td>
      <td>
        <ul>
          <li>Added guideline descriptions on <a href="/Design/Light.md">light</a> and <a href="/Design/Audio.html#SoundEffect">sound effects</a> for alarms, reminders, and timers to the Design guidelines</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-08</td>
      <td>
        <ul>
          <li>Added a section on <a href="/Develop/Guides/Handle_Delegation.html#HandleDelegation">Handling delegated user requests</a> and added the <a href="/Develop/References/CICInterface/Clova.html#HandleDelegatedEvent">Clova.HandleDelegatedEvent</a> directive and the <a href="/Develop/References/CICInterface/Clova.html#ProcessDelegatedEvent">Clova.ProcessDelegatedEvent</a> event</li>
          <li>Added a description to include the <a href="/Develop/References/Context_Objects.html#PlaybackState">AudioPlayer.PlaybackState</a> context information in <a href="/Develop/References/CICInterface/PlaybackController.html#NextCommandIssued">PlaybackController.NextCommandIssued</a> and <a href="/Develop/References/CICInterface/PlaybackController.html#PreviousCommandIssued">PlaybackController.PreviousCommandIssued</a> event messages</li>
          <li>Revised the description of the interaction structure of the <a href="/Develop/References/CICInterface/Alerts.html">Alerts</a> API</li>
          <li>Added the description of the <a href="/Develop/References/CICInterface/DeviceControl.html#DeviceControlWorkFlow">interaction structure</a> of the <a href="/Develop/References/CICInterface/DeviceControl.html">DeviceControl</a> API</li>
          <li>Emended errors on some content templates and shared objects</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-02</td>
      <td>
        <ul>
          <li>Added error code 429 and its description in the Remarks of the section <a href="/Develop/References/CIC_API.html#EstablishDownchannel">Establishing a downchannel</a></li>
          <li>Removed route-finding templates (CarRoute, TransportationRoute) and the Ufor route-finder was replaced with an ImageText template</li>
          <li>Emended some errors and typos in the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-12-18</td>
      <td>
        <ul>
          <li>Removed the ExpectSpeechTimedOut event from the <a href="/Develop/References/CICInterface/SpeechRecognizer.html">SpeechRecognizer</a> interface</li>
          <li>Removed the Clova.FreetalkState object from the <a href="/Develop/References/Context_Objects.html">context information</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-12-11</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/References/CICInterface/AudioPlayer.html#ClearQueue">ClearQueue</a> directive to the <a href="/Develop/References/CICInterface/AudioPlayer.html">AudioPlayer</a> interface</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-12-04</td>
      <td>
        <ul>
          <li>Added <a href="/Design/Audio.html#AudioInterruptionRule">audio interruption rules</a> to the Design guidelines</li>
          <li>Improved the images in Design guidelines</li>
          <li>Added the section on <a href="/Develop/Guides/Interact_with_CIC.html#UserAgentString">user-agent strings</a> for prerequisites before interacting with CIC</li>
          <li>Added the description of the 412 Precondition failed code in the <a href="/Develop/References/CIC_API.html#SendEvent">Sending event messages</a> section of the <a href="/Develop/References/CIC_API.html">CIAPreference</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-11-20</td>
      <td>
        <ul>
          <li>Added the Design guidelines section</a></li>
          <li>Added Type5 and Type6 to the subType value of the <a href="/Develop/References/ContentTemplates/CardList.html">CardList template</a> to display audio content and image thumbnails</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-11-13</td>
      <td>
        <ul>
          <li>Added UX details in the Remarks of directive messages for volume control (<a href="/Develop/References/CICInterface/DeviceControl.html#Decrease">DeviceControl.Decrease</a>, <a href="/Develop/References/CICInterface/DeviceControl.html#Increase">DeviceControl.Increase</a>, <a href="/Develop/References/CICInterface/DeviceControl.html#SetValue">DeviceControl.SetValue</a>, <a href="/Develop/References/CICInterface/PlaybackController.html#Mute">PlaybackController.Mute</a>, and <a href="/Develop/References/CICInterface/PlaybackController.html#Unmute">PlaybackController.Unmute</a>)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-11-06</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/References/CICInterface/SpeechRecognizer.html#KeepRecording">SpeechRecognizer.KeepRecording</a> directive</li>
          <li>Added <a href="/Develop/References/Context_Objects.html#Display">Device.Display</a> context information</li>
          <li>Added a token field to the <a href="/Develop/References/ContentTemplates/ActionTimer.html">ActionTimer</a>, <a href="/Develop/References/ContentTemplates/ActionTimerList.html">ActionTimerList</a>, <a href="/Develop/References/ContentTemplates/Alarm.html">Alarm</a>, <a href="/Develop/References/ContentTemplates/AlarmList.html">AlarmList</a>, <a href="/Develop/References/ContentTemplates/Memo.html">Memo</a>, <a href="/Develop/References/ContentTemplates/MemoList.html">MemoList</a>, <a href="/Develop/References/ContentTemplates/Reminder.html">Reminder</a>, <a href="/Develop/References/ContentTemplates/ReminderList.html">ReminderList</a>, <a href="/Develop/References/ContentTemplates/Schedule.html">Schedule</a>, <a href="/Develop/References/ContentTemplates/ScheduleList.html">ScheduleList</a>, <a href="/Develop/References/ContentTemplates/Timer.html">Timer</a>, and <a href="/Develop/References/ContentTemplates/TimerList.html">TimerList</a> templates</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-10-23</td>
      <td>
        <ul>
          <li>Added emotionCode and motionCode fields to the <a href="/Develop/References/ContentTemplates/Text.html">Text</a> template</li>
          <li>Changed assets[].url field contents of <a href="/Develop/References/CICInterface/Alerts.html#SetAlert">Alerts.SetAlert</a> directive message</li>
          <li>Revised an error in the example of the <a href="/Develop/References/CICInterface/AudioPlayer.html#StreamRequested">AudioPlayer.StreamRequested</a> event</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-10-16</td>
      <td>
        <ul>
          <li>Added <a href="/Develop/References/CICInterface/PlaybackController.html#Replay">Replay</a> directive message to the <a href="/Develop/References/CICInterface/PlaybackController.html">PlaybackController</a> namespace</li>
          <li>Added supplementary information on alarm synchronization in the Overall process section</li>
          <li>Removed the content field from the <a href="/Develop/References/Context_Objects.html#AlertsState">Alert.AlertsState</a> of <a href="/Develop/References/Context_Objects.html#AlertInfoObject">AlertInfoObject</a> context information</li>
          <li>Modified images and emended errors in the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-10-02</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/References/CICInterface/Alerts.html">Alerts</a> namespace and alarm related interface</li>
          <li>Added the <a href="/Develop/References/CICInterface/System.html">System</a> namespace and alarm related interface</li>
          <li>Added the expectContentType field to the <a href="/Develop/References/CICInterface/SpeechRecognizer.html#ExpectSpeech">SpeechRecognizer.ExpectSpeech</a> directive</li>
          <li>Added a warning field to the <a href="/Develop/References/Context_Objects.html#VolumeInfoObject">VolumeInfoObject</a> of <a href="/Develop/References/Context_Objects.html#DeviceState">Device.DeviceState</a></li>
          <li>Added templates: <a href="/Develop/References/ContentTemplates/ActionTimer.html">ActionTimer</a>, <a href="/Develop/References/ContentTemplates/ActionTimerList.html">ActionTimerList</a>, <a href="/Develop/References/ContentTemplates/Alarm.html">Alarm</a>, <a href="/Develop/References/ContentTemplates/AlarmList.html">AlarmList</a>, <a href="/Develop/References/ContentTemplates/Memo.html">Memo</a>, <a href="/Develop/References/ContentTemplates/MemoList.html">MemoList</a>, <a href="/Develop/References/ContentTemplates/Reminder.html">Reminder</a>, <a href="/Develop/References/ContentTemplates/ReminderList.html">ReminderList</a>, <a href="/Develop/References/ContentTemplates/Schedule.html">Schedule</a>, <a href="/Develop/References/ContentTemplates/ScheduleList.html">ScheduleList</a>, <a href="/Develop/References/ContentTemplates/Timer.html">Timer</a>, and <a href="/Develop/References/ContentTemplates/TimerList.html">TimerList</a></li>
          <li>Modified some code examples of the <a href="/Develop/References/ContentTemplates/ImageText.html">ImageText</a> template</li>
          <li>Modified some fields of the <a href="/Develop/References/ContentTemplates/Popup.html">Popup</a> template</li>
          <li>Added the Terms and Conditions of Service on <a href="/Develop/Guides/Interact_with_CIC.html#CreateClovaAccessToken">Creating Clova access tokens</a> and <a href="/Develop/References/Clova_Auth_API.html#RequestAuthorizationCode">Requesting an authorization code</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-09-25</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/References/CICInterface/PlaybackController.html#NextCommandIssued">PlaybackController.NextCommandIssued</a> and <a href="/Develop/References/CICInterface/PlaybackController.html#PreviousCommandIssued">PlaybackController.PreviousCommandIssued</a> events for musiplayback control to the <a href="/Develop/References/CICInterface/PlaybackController.html">PlaybackController API</a></li>
          <li>Added the expectSpeechId field to the <a href="/Develop/References/CICInterface/SpeechRecognizer.html#ExpectSpeech">SpeechRecognizer.ExpectSpeech</a> directive, and added speechId and explicit fields to the <a href="/Develop/References/CICInterface/SpeechRecognizer.html#Recognize">SpeechRecognizer.Recognize</a> event</li>
          <li>Added the <a href="/Develop/References/ContentTemplates/Popup.html">Popup template</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-09-18</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/References/CICInterface/DeviceControl.html#ExpectReportState">DeviceControl.ExpectReportState</a> directive, <a href="/Develop/References/CICInterface/DeviceControl.html#ReportState">DeviceControl.ReportState</a> event, and <a href="/Develop/References/CICInterface/DeviceControl.html#RequestStateSynchronization">DeviceControl.RequestStateSynchronization</a> event to DeviceControl APand changed the name of the DeviceControl.UpdateDeviceState directive to <a href="/Develop/References/CICInterface/DeviceControl.html#SynchronizeState">DeviceControl.SynchronizeState</a></li>
          <li>Added the item3 field to the <a href="/Develop/References/ContentTemplates/Text.html">Text</a> template</li>
          <li>Added a source field in the <a href="/Develop/References/CICInterface/AudioPlayer.html#Play">AudioPlayer.Play</a> directive message</li>
          <li>Added the durationInMilliseconds field in <a href="/Develop/References/CICInterface/AudioPlayer.html#AudioStreamInfoObject">AudioStreamInfoObject</a> </li>
          <li>Added the Notifier namespace. Added <a href="/Develop/References/CICInterface/Notifier.html#ClearIndicator">ClearIndicator</a> and <a href="/Develop/References/CICInterface/Notifier.html#SetIndicator">SetIndicator</a> directive messages</li>
          <li>Added the <a href="/Develop/References/ContentTemplates/Atmosphere.html">Atmosphere</a> template</li>
          <li>Added a guide text on prohibited use for the bgClipURL field of weather templates due to license issues</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-09-11</td>
      <td>
        <ul>
          <li>Added the explicit field to the <a href="/Develop/References/CICInterface/SpeechRecognizer.html#ExpectSpeech">SpeechRecognizer.ExpectSpeech</a> directive message</li>
          <li>Added <a href="/Develop/References/ContentTemplates/Common_Fields.html">common fields</a> specification to the <a href="/Develop/References/Content_Templates.html">Content</a> template</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-09-04</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/References/CICInterface/Clova.html#Help">Clova.Help</a> directive</li>
          <li>Added the <a href="/Develop/References/CICInterface/DeviceControl.html#LaunchApp">DeviceControl.LaunchApp</a> directive message</li>
          <li>Added the TextRecognizer namespace and the <a href="/Develop/References/CICInterface/TextRecognizer.html">TextRecognizer.Recognize</a> event</li>
          <li>Updated content on CIAPI: Added status codes for the request and response header, and applied the format of the REST APreference to the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-08-28</td>
      <td>
        <ul>
          <li>Added the set-top box specification on TV channels and specifications for the power state to <a href="/Develop/References/Context_Objects.html#DeviceState">Device.DeviceState</a> and <a href="/Develop/References/CICInterface/DeviceControl.html">DeviceControl API</a></li>
          <li>Added and changed some target values in the <a href="/Develop/References/CICInterface/DeviceControl.html">DeviceControl API</a>: power, energysave, screenbrightness</li>
          <li>Changed the name SetPoint of the <a href="/Develop/References/CICInterface/DeviceControl.html">DeviceControl API</a> to <a href="/Develop/References/CICInterface/DeviceControl.html#SetValue">SetValue</a></li>
          <li>Updated <a href="/Develop/References/Clova_Auth_API.html">Clova auth API</a>: Added request and response headers, and status codes. Applied the format of the REST APreference to the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-08-21</td>
      <td>
        <ul>
          <li>Added the section on <a href="/Develop/Guides/Interact_with_CIC.html#ManageConnection">Renewing a Clova access token</a> and updated content on token APIs</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-08-14</td>
      <td>
        <ul>
          <li>Added description on the dialogue model</li>
          <li>Added <a href="/Develop/References/CICInterface/DeviceControl.html">DeviceControl</a> API</li>
          <li>Added payload fields in <a href="/Develop/References/Context_Objects.html">Device.DeviceState</a>: airplane, battery, bluetooth, brightness, flashLight, gps, powerSavingMode, soundMode, volume, and wifi</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-08-04</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/References/CICInterface/Clova.html#Hello">Clova.Hello</a> directive message</li>
          <li>Added the type field to the AudioItem object of the <a href="/Develop/References/CICInterface/AudioPlayer.html#Play">AudioPlayer.Play</a> directive message</li>
          <li>Added the urlPlayable field to the <a href="/Develop/References/CICInterface/AudioPlayer.html#AudioStreamInfoObject">AudioStreamInfoObject</a></li>
          <li>Added specification on <a href="/Develop/References/CIC_API.html#Error">CIerror messages</a></li>
          <li>Rewrote the contents of <a href="/Develop/References/CIC_API.html#MultipartMessage">multipart message</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-07-28</td>
      <td>
        <ul>
          <li>Removed PlayNext and Stop from <a href="/Develop/References/CICInterface/AudioPlayer.html">AudioPlayer</a> (merged into <a href="/Develop/References/CICInterface/PlaybackController.html">PlaybackController</a>)</li>
          <li>Changed the message names of <a href="/Develop/References/CICInterface/PlaybackController.html">PlaybackController</a> (Mute, Next, Pause, Previous, Resume, Stop, Unmute, VolumeDown, and VolumeUp</li>
          <li>Added route-finding templates: CarRoute and TransportationRoute</li>
          <li>Added weather templates: <a href="/Develop/References/ContentTemplates/Humidity.html">Humidity</a>, <a href="/Develop/References/ContentTemplates/TodayWeather.html">TodayWeather</a>, <a href="/Develop/References/ContentTemplates/TomorrowWeather.html">TomorrowWeather</a>, <a href="/Develop/References/ContentTemplates/WeeklyWeather.html">WeeklyWeather</a>, and <a href="/Develop/References/ContentTemplates/WindSpeed.html">WindSpeed</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-07-14</td>
      <td>
        <ul>
          <li>Added beginAtInMilliseconds field contents of <a href="/Develop/References/CICInterface/AudioPlayer.html#AudioStreamInfoObject">AudioStreamInfoObject</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-07-07</td>
      <td>
        <ul>
          <li>Added the <a href="/Glossary.html">Glossary</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-07-03</td>
      <td>
        <ul>
          <li>Applied changes according to the document review results</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-06-19</td>
      <td>
        <ul>
          <li>Updated <a href="/Develop/Guides/Interact_with_CIC.html#ManageConnection">Managing connections</a> (for when HTTP PING frame is unavailable)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-06-08</td>
      <td>
        <ul>
          <li>Added the section on <a href="/Develop/Guides/Interact_with_CIC.html#ManageConnection">Managing connections</a> (HTTP PING) to <a href="/Develop/Guides/Interact_with_CIC.html">Interacting with CIC</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-05-29</td>
      <td>
        <ul>
          <li>Completed the first draft of the CIdocument</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>
