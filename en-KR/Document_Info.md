# README
This document provides design guidelines and developer guide/API reference for CIC platforms in order to develop the Clova client. The intended audiences of this document are client developers using CIC to develop electronic devices and apps that link with the Clova services.

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
      <th style="width:12%">Release date</th><th style="width:88%">History</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2019-08-19</td>
      <td>
        <ul>
          <li>Added the noticeText field to the <a href="/Develop/References/ContentTemplates/CardList.md">CardList</a> template for a legal notice that informs users about content modifications due to service restrictions when providing contents on a device with a screen</li>
          <li>Moved up a level of the table of contents in the guide for implementing client features, the CIC API interface, and context information references</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-08-05</td>
      <td>
        <ul>
          <li>Separated the Design guidelines for client devices document into <a href="/Design/Client_State_And_Event.md">Client states and events</a>, <a href="/Design/Button.md">Buttons</a>, <a href="/Design/Light.md">Lights</a>, <a href="/Design/Audio.md">Sounds</a>, <a href="/Design/Screen.md">Screens</a>, and <a href="/Design/Clova_Inside.md">Clova inside</a> pages</li>
          <li>Added the container formats supported by Clova in the details of <a href="/Design/Audio.md#SupportedAudioFormat">Supported audio formats</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-07-31</td>
      <td>
        <ul>
          <li>Added target and target.namespace fields in <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectNextCommand">ExpectNextCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectPauseCommand">ExpectPauseCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectPreviousCommand">ExpectPreviousCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectResumeCommand">ExpectResumeCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectStopCommand">ExpectStopCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#Pause">Pause</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#Resume">Resume</a>, and <a href="/Develop/References/MessageInterfaces/PlaybackController.md#Stop">Stop</a> directive messages of the <a href="/Develop/References/MessageInterfaces/PlaybackController.md">PlaybackController</a> namespace to specify the control target of media playback</li>
          <li>Modified the output of sound effect for entering the attending state to optional instead of mandatory in the <a href="/Design/Audio.md#SoundEffect">sound effect item of Design guidelines</a></li>
          <li>Added the details related to <a href="/Design/Clova_Inside.md">Clova inside</a> in the Design guidelines</li>
          <li>Separated as Clova client guide from Clova developer guide.</li>
          <li>Corrected some errors in the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-07-02</td>
      <td>
        <ul>
          <li>Changed the implementation status of the <a href="/Design/Light.md#LightEffect">lighting effect</a> related to timeouts to "Optional" in the lighting effect description in the Design guidelines</li>
          <li>Updated the redirect_uri description for the <a href="/Develop/References/Clova_Auth_API.md#RequestAuthorizationCode">Requesting an authorization code(/authorize)</a> of the <a href="/Develop/References/Clova_Auth_API.md">Clova auth API</a></li>
          <li>Added source and source.namespace fields to <a href="/Develop/References/MessageInterfaces/PlaybackController.md#NextCommandIssued">NextCommandIssued</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#PauseCommandIssued">PauseCommandIssued</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#PreviousCommandIssued">PreviousCommandIssued</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ResumeCommandIssued">ResumeCommandIssued</a>, and <a href="/Develop/References/MessageInterfaces/PlaybackController.md#StopCommandIssued">StopCommandIssued</a> event messages of the <a href="/Develop/References/MessageInterfaces/PlaybackController.md">PlaybackController</a> namespace to specify the control target of media playback</li>
          <li>Revised some misprints of examples and adjusted the level of note box</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-06-10</td>
      <td>
        <ul>
          Added examples and logo updates in the <a href="/Design/Screen.md#GreenDotVUI">Green Dot VUI</a> descriptions of the <li>Design guidelines</li>
          <li>Added the deviceId field to the <a href="/Develop/References/MessageInterfaces/DeviceControl.md#RequestStateSynchronization">RequestStateSynchronization</a> event message to share the state of a specific device</li>
          <li>Modified the deviceState field of the <a href="/Develop/References/MessageInterfaces/DeviceControl.md#SynchronizeState">SynchronizeState</a> event message as conditional to express the state of offline or the client device not registered</li>
          <li>Modified model_id to All in the <a href="/Develop/References/Clova_Auth_API.md">Clova auth API</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-05-27</td>
      <td>
        <ul>
          <li>Removed the Voice Agent description from the Design guidelines and added the <a href="/Design/Screen.md#GreenDotVUI">Green Dot VUI</a> description</li>
          <li>Revised the description to upload all state information that can be sent without sending random information selectively when Clova client is sending <a href="/Develop/References/Context_Objects.md">context information</a> to CIC</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-05-20</td>
      <td>
        <ul>
          <li>Added <a href="/Develop/Guides/Handle_Audio_Playback.md#ManageStreamInfo">Managing audio information</a> section to the <a href="/Develop/Guides/Handle_Audio_Playback.md">Handling audio playback</a> guide</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-05-04</td>
      <td>
        <ul>
          <li>Changed the description of <a href="/Develop/References/MessageInterfaces/PlaybackController.md#PlayCommandIssued">PlaybackController.PlayCommandIssued</a> and <a href="/Develop/References/MessageInterfaces/PlaybackController.md#SetRepeatModeCommandIssued">PlaybackController.SetRepeatModeCommandIssued</a> event messages that were not described correctly</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-04-19</td>
      <td>
        <ul>
          Added CIC API namespaces for each audio content type to enable classification of audio contents in <a href="/Design/Audio.md#AudioInterruptionRule">Rules for basic audio playback</a> of the <li>Design guidelines</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-04-08</td>
      <td>
        <ul>
          <li>Added description on the "error" parameter in the "redirect_uri" field of the response among the <a href="/Develop/References/Clova_Auth_API.md#RequestAuthorizationCode">Requesting an authorization code</a> description of the <a href="/Develop/References/Clova_Auth_API.md">CIC auth API</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-03-25</td>
      <td>
        <ul>
          <li>Added the contentType field to the <a href="/Develop/References/MessageInterfaces/SpeechSynthesizer.md#Speak">SpeechSynthesizer.Speak</a> directive message of the CIC API to provide the HLS audio</li>
          <li>Corrected some errors and examples in the link</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-03-13</td>
      <td>
        <ul>
          <li>Modified the order of the details of some reference documents</li>
          <li>Reflected internal and external feedback on the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-02-26</td>
      <td>
        <ul>
          <li>Separated the contents of Implementing client features into different pages</li>
          <li>Revised the term URL to URI for those without the UI components</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-01-25</td>
      <td>
        <ul>
          <li>Improved some descriptions in the <a href="/Develop/Guides/Interact_with_CIC.md">Interacting with CIC</a> document</li>
          <li>Revised some styles of the diagram and adjusted the table column interval</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-01-07</td>
      <td>
        <ul>
          <li>Unified the style of some UML diagrams used in the document</li>
          <li>Corrected some notation errors in the document history</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2019-01-04</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/Guides/Handle_Device_Control.md">Handling client action control</a> guide</li>
          <li>Added the <a href="/Develop/Guides/Handle_Bluetooth_Control.md">Handling client Bluetooth control</a> guide</li>
          <li>Added the <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#PlaybackQueueCleared">AudioPlayer.PlaybackQueueCleared</a> event message to report the playback queue initialization action to the <a href="/Develop/References/MessageInterfaces/AudioPlayer.md">AudioPlayer API</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-12-24</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/Guides/Handle_Device_Control.md">Handling client action control</a> guide</li>
          <li>Added the <a href="/Develop/Guides/Handle_Bluetooth_Control.md">Handling client Bluetooth control</a> guide</li>
          <li>Added the nowTemperatureImageCode field and the nowTemperatureImageUrl field related to the current weather information in the <a href="/Develop/References/ContentTemplates/TodayWeather.md">TodayWeather</a> template</li>
          <li>Corrected documentation errors in the <a href="/Develop/References/ContentTemplates/TodayWeather.md">TodayWeather</a> template</li>
          <li>Corrected the notation error in the note type of some sequence diagrams</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-11-30</td>
      <td>
        <ul>
          <li>Separated the description of the dialogue model into <a href="/Develop/CIC_Overview.md#IndirectDialogue">Indirect dialogue structure</a> and <a href="/Develop/Guides/Manage_Dialogue_ID_And_Handle_Tasks.md">Handling tasks and managing dialogue IDs,</a>, and supplemented the content</li>
          <li>Added a value field to the <a href="/Develop/References/MessageInterfaces/DeviceControl.md#Decrease">DeviceControl.Decrease</a> and <a href="/Develop/References/MessageInterfaces/DeviceControl.md#Increase">DeviceControl.Increase</a> directive messages to support adjustment of the screen brightness or volume of a device by a specific level</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-11-16</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/Guides/Handle_Settings.md">Handling settings</a> guide</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-11-09</td>
      <td>
        <ul>
          Added a guide for <a href="//Develop/Guides/Handle_Audio_Playback.md">Handling audio playback</a> regarding audio playback and playback control<li> (and included the following contents:)
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
          <li>Added the <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtDelete">DeviceControl.BtDelete</a> and <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtRescan">DeviceControl.BtRescan</a> directive messages to rescan or remove a Bluetooth device in the <a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> namespace</li>
          <li>Added the <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtPlay">DeviceControl.BtPlay</a> directive message to play music through a Bluetooth device in the <a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> namespace</li>
          <li>Added fields to the <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtConnect">DeviceControl.BtConnect</a> and <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtDisconnect">DeviceControl.BtDisconnect</a> directive messages of the <a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> namespace to enable connection or disconnection of a specific device or a device with a specific role</li>
          <li>Added connecting, pairing, playerinfo, and scanning fields to <a href="/Develop/References/Context_Objects.md#BluetoothInfoObject">BluetoothInfoObject</a> of the <a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a> context object to add Bluetooth-related state information of the client</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-10-05</td>
      <td>
        <ul>
          <li>Revised the structure and examples of <a href="/Develop/References/CIC_API.md#Error">CIC error messages</a> according to the actual implementation</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-09-21</td>
      <td>
        <ul>
          <li>Added the format field to the payload of <a href="/Develop/References/MessageInterfaces/AudioPlayer.md">AudioPlayer</a> <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#AudioStreamInfoObject">AudioStreamInfoObject</a> to specify the MIME type of content</li>
          <li>Added SubscribeCommandIssued and UnsubscribeCommandIssued event messages, and UpdateLike and UpdateSubscribe directive messages to the <a href="/Develop/References/MessageInterfaces/TemplateRuntime.md">TemplateRuntime</a> namespace to handle Like and Subscribe features when playing media content</li>
          <li>Added the button information that must be displayed when playing media content or the type of UI control to the <a href="/Develop/References/MessageInterfaces/TemplateRuntime.md#RenderPlayerInfo">TemplateRuntime.RenderPlayerInfo</a> directive message</li>
          <li>Corrected some wrong code examples</li>
          <li>Corrected some incorrect links</li>
          <li>Changed some notations of the word Extension that is at the point of interaction with the end user, to Skill (Also updated the UI capture images)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-09-07</td>
      <td>
        <ul>
          <li>Corrected errors in links and mistakes in code examples in the <a href="/Develop/Guides/Handle_Alerts.md">Handling alerts</a> section</li>
          <li>Changed "yourdomain.com" used in examples to "example.com," which is the domain name for document preparation</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-08-29</td>
      <td>
        <ul>
          <li>Changed the RGB value of <a href="/Design/Light.md#LightColor">Light colors</a> in the Design guidelines</li>
          <li>Added Implementing client features</li>
          <li>Updated some fields in the <a href="/Develop/References/MessageInterfaces/AudioPlayer.md">AudioPlayer</a> and <a href="/Develop/References/MessageInterfaces/TemplateRuntime.md">TemplateRuntime</a> namespaces</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-08-24</td>
      <td>
        <ul>
          <li>Corrected an error in the example for the <a href="/Develop/References/MessageInterfaces/Alerts.md#StopAlert">Alerts.StopAlert</a> directive message</li>
          <li>Revised the description on the initiator.inputSource field of the <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a> directive message to avoid confusion</li>
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
          Updated the sound effect for entering the attending state among the <a href="/Design/Audio.md#SoundEffect">sound effects</a> specified in the <li>Design guidelines</li>
          <li>Added 423 Locked status code to the section on <a href="/Develop/References/Clova_Auth_API.md#RequestAuthorizationCode">Requesting an authorization code</a> under <a href="/Develop/References/Clova_Auth_API.md">CIC auth API</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-07-09</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/References/MessageInterfaces/Settings.md">Settings</a> namespace to update and synchronize the settings information of the client device</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-06-17</td>
      <td>
        <ul>
          <li>Added the isLive field to <a href="/Develop/References/MessageInterfaces/TemplateRuntime.md#RenderPlayerInfo">TemplateRuntime.RenderPlayerInfo</a> to distinguish real-time broadcasting content</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-05-21</td>
      <td>
        <ul>
          <li>Added the missing field (btlist[].role) to <a href="/Develop/References/Context_Objects.md#BluetoothInfoObject">BluetoothInfoObject</a> of the <a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a> context object</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-05-14</td>
      <td>
        <ul>
          <li>Moved the <a href="/Develop/References/MessageInterfaces/Clova.md#LaunchURI">LaunchURI</a> directive message from the DeviceControl namespace to the <a href="/Develop/References/MessageInterfaces/Clova.md">Clova</a> namespace</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-05-07</td>
      <td>
        <ul>
          <li>Added the LaunchURI directive message to the DeviceControl namespace</li>
          <li>Deprecated the <a href="/Develop/References/MessageInterfaces/DeviceControl.md#LaunchApp">LaunchApp</a> directive message and the <a href="/Develop/References/MessageInterfaces/DeviceControl.md#OpenScreen">OpenScreen</a> directive message of the <a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> namespace</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-04-16</td>
      <td>
        <ul>
          <li>Updated the description of the wakeWord field and audio data of the <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a> event</li>
          <li>Added the <a href="/Develop/References/MessageInterfaces/DeviceControl.md#Open">Open</a> directive message to the <a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> namespace</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-04-09</td>
      <td>
        <ul>
          <li>Updated the description and example image for the <a href="/Design/Screen.md#BootingScreen">bootscreen</a> in the Design guidelines for client devices</li>
                  </ul>
      </td>
    </tr>
    <tr>
      <td>2018-04-02</td>
      <td>
        <ul>
          <li>Added message specifications to the <a href="/Develop/References/MessageInterfaces/AudioPlayer.md">AudioPlayer</a> namespace and updated some fields
            <ul>
              <li>Added the <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#ExpectReportPlaybackState">AudioPlayer.ExpectReportPlaybackState</a> directive message, <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#ReportPlaybackState">AudioPlayer.ReportPlaybackState event message</a> {% if book.DocMeta.TargetReaderType == "Internal" %}, <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#RequestPlaybackState">AudioPlayer.RequestPlaybackState</a> event message, and <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#SynchronizePlaybackState">SynchronizePlaybackState directive message</a> {% endif %}</li>
              <li>Updated the payload field of the <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#Play">AudioPlayer.Play</a> directive message</li>
              <li>Added a mandatory token field value to the event fields with name formats ProgressReportXXX and PlayXXX</li>
            </ul>
          </li>
          <li>Added repeatMode to the <a href="/Develop/References/Context_Objects.md#PlaybackState">AudioPlayer.PlaybackState</a> context object</li>
          <li>Added a total of 12 message specifications to the <a href="/Develop/References/MessageInterfaces/PlaybackController.md">PlaybackController</a> namespace
            <ul>
              <li>Added the <a href="/Develop/References/MessageInterfaces/PlaybackController.md#PauseCommandIssued">PlaybackController.PauseCommandIssued</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#PlayCommandIssued">PlaybackController.PlayCommandIssued</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ResumeCommandIssued">PlaybackController.ResumeCommandIssued</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#SetRepeatModeCommandIssued">PlaybackController.SetRepeatModeCommandIssued</a>, and <a href="/Develop/References/MessageInterfaces/PlaybackController.md#StopCommandIssued">PlaybackController.StopCommandIssued</a> event messages</li>
              <li><a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectNextCommand">PlaybackController.ExpectNextCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectPauseCommand">PlaybackController.ExpectPauseCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectPlayCommand">PlaybackController.ExpectPlayCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectPreviousCommand">PlaybackController.ExpectPreviousCommand</a>,
<a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectResumeCommand">PlaybackController.ExpectResumeCommand</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#ExpectStopCommand">PlaybackController.ExpectStopCommand</a>, and <a href="/Develop/References/MessageInterfaces/PlaybackController.md#SetRepeatMode">PlaybackController.SetRepeatMode</a> directive messages</li>
              The <li><a href="/Develop/References/MessageInterfaces/PlaybackController.md#TurnOnRepeatMode">PlaybackController.TurnOnRepeatMode</a> and <a href="/Develop/References/MessageInterfaces/PlaybackController.md#TurnOffRepeatMode">PlaybackController.TurnOffRepeatMode</a> directive messages are scheduled to be removed</li>
            </ul>
          </li>
          <li>Added the <a href="/Develop/References/MessageInterfaces/TemplateRuntime.md">TemplateRuntime</a> namespace to separate the information for streaming media and playing metadata for displaying the play list</li>
          <li>Added the scanlist field to <a href="/Develop/References/Context_Objects.md#BluetoothInfoObject">BluetoothInfoObject</a> of <a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a></li>
          <li>Added the <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtConnectByPINCode">BtConnectByPINCode</a> directive message, <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtRequestForPINCode">BtRequestForPINCode</a> event message, and <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtRequestToCancelPinCodeInput">BtRequestToCancelPinCodeInput</a> event message to the <a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> namespace to connect with external Bluetooth devices that use PIN codes</li>
          <li>Added a payload to the <a href="/Develop/References/MessageInterfaces/DeviceControl.md#BtConnect">BtConnect</a> directive message of the <a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> namespace</li>
          <li>Updated some UIs on the Clova developer console</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-03-19</td>
      <td>
        <ul>
          <li>Added <a href="/Develop/References/Context_Objects.md#SoundOutputInfoObject">SoundOutputInfoObject</a> to <a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a></li>
          <li>Added the <a href="/Develop/References/MessageInterfaces/PlaybackController.md#CustomCommandIssued">CustomCommandIssued</a> event message, which can execute customized commands of a user, to the <a href="/Develop/References/MessageInterfaces/PlaybackController.md#CustomCommandIssued">PlaybackController</a> namespace</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-03-05</td>
      <td>
        <ul>
          <li>Revised the description of the initiator field of the <a  href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a> event message</li>
          <li>Modified the name of the hearing state to Listening in the client states specified in the Design guidelines</li>
          <li>Added Feedback type in the audio content type and description on rules in the <a href="/Design/Audio.md">Audio</a> section of the Design guidelines for client devices</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-02-26</td>
      <td>
        <ul>
          <li>Added the deviceUUID field to the initiator field of the <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a> event message</li>
          <li>Added the <a href="/Develop/References/MessageInterfaces/Alerts.md#RequestSynchronizeAlert">RequestSynchronizeAlert</a> event message and the <a href="/Develop/References/MessageInterfaces/Alerts.md#SynchronizeAlert">SynchronizeAlert</a> directive message related to alarm synchronization to the <a href="/Develop/References/MessageInterfaces/Alerts.md">Alerts</a> namespace</li>
          <li>Scheduled to remove some fields related to alarm synchronization from the System namespace</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-02-19</td>
      <td>
        <ul>
          <li>Added the initiator field to the <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a> event message to accurately identify the user invocation</li>
          <li>Added the label field to the <a href="/Develop/References/MessageInterfaces/Alerts.md#SetAlert">Alerts.SetAlert</a> directive message to check details of reminders and scheduled actions</li>
          <li>Added the label field to the <a href="/Develop/References/ContentTemplates/ActionTimer.md">ActionTimer</a>, <a href="/Develop/References/ContentTemplates/ActionTimerList.md">ActionTimerList</a>, <a href="/Develop/References/ContentTemplates/Reminder.md">Reminder</a>, and <a href="/Develop/References/ContentTemplates/ReminderList.md">ReminderList</a> templates to display the details of reminders and scheduled actions</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-02-05</td>
      <td>
        <ul>
          <li>Revised the description of the durationInMilliseconds field of <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#AudioStreamInfoObject">AudioStreamInfoObject</a></li>
          <li>Added new information such as a field related to the source on the following templates: <a href="/Develop/References/ContentTemplates/Atmosphere.md">Atmosphere</a>, <a href="/Develop/References/ContentTemplates/CardList.md">CardList</a>, <a href="/Develop/References/ContentTemplates/Humidity.md">Humidity</a>, <a href="/Develop/References/ContentTemplates/TodayWeather.md">TodayWeather</a>, <a href="/Develop/References/ContentTemplates/TomorrowWeather.md">TomorrowWeather</a>, <a href="/Develop/References/ContentTemplates/WeeklyWeather.md">WeeklyWeather</a>, and <a href="/Develop/References/ContentTemplates/WindSpeed.md">WindSpeed</a></li>
          <li>Corrected some errors in the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-29</td>
      <td>
        <ul>
          <li>Added <a href="/Design/Audio.md#SoundEffect">sound effects for reminders</a> in the Design guidelines</li>
          <li>Added the <a href="/Develop/References/MessageInterfaces/Notifier.md#Notify">Notifier.Notify</a> event message to the <a href="/Develop/References/MessageInterfaces/Notifier.md">Notifier</a> namespace and updated the payload field of the namespace</li>
          <li>Added <a href="/Develop/References/MessageInterfaces/SpeechSynthesizer.md#SpeechFinished">SpeechFinished</a>, <a href="/Develop/References/MessageInterfaces/SpeechSynthesizer.md#SpeechStarted">SpeechStarted</a>, and <a href="/Develop/References/MessageInterfaces/SpeechSynthesizer.md#SpeechStopped">SpeechStopped</a> event messages to the <a href="/Develop/References/Context_Objects.md#SpeechState">SpeechSynthesizer.SpeechState</a> and <a href="/Develop/References/MessageInterfaces/SpeechSynthesizer.md">SpeechSynthesizer</a> namespaces</li>
          <li>Added speechId and explicit fields to the <a href="/Develop/References/MessageInterfaces/TextRecognizer.md">TextRecognizer.Recognize</a> event message for multi-turn dialogues</li>
          <li>Reflected some UI updates on the Clova developer console</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-22</td>
      <td>
        <ul>
          <li>Added <a href="/Design/Audio.md#SupportedAudioFormat">Supported audio formats</a> to the Design guidelines</li>
          <li>Changed the image format of the UML diagram</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-15</td>
      <td>
        <ul>
          <li>Added descriptions on the guidelines for light and sound effects for alarms, reminders, and timers to the Design guidelines</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-08</td>
      <td>
        <ul>
          <li>Added a section on <a href="/Develop/Guides/Handle_Delegation.md">Handling delegated user requests</a> and added the <a href="/Develop/References/MessageInterfaces/Clova.md#HandleDelegatedEvent">Clova.HandleDelegatedEvent</a> directive message and the <a href="/Develop/References/MessageInterfaces/Clova.md#ProcessDelegatedEvent">Clova.ProcessDelegatedEvent</a> event message</li>
          <li>Added a description to include the <a href="/Develop/References/Context_Objects.md#PlaybackState">AudioPlayer.PlaybackState</a> context information in <a href="/Develop/References/MessageInterfaces/PlaybackController.md#NextCommandIssued">PlaybackController.NextCommandIssued</a> and <a href="/Develop/References/MessageInterfaces/PlaybackController.md#PreviousCommandIssued">PlaybackController.PreviousCommandIssued</a> event messages</li>
          <li>Supplemented the description of the interaction structure of the <a href="/Develop/References/MessageInterfaces/Alerts.md">Alerts</a> API</li>
          <li>Added the description of the <a href="/Develop/References/MessageInterfaces/DeviceControl.md#DeviceControlWorkFlow">interaction structure</a> of the <a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> API</li>
          <li>Corrected errors on some content templates and shared objects</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2018-01-02</td>
      <td>
        <ul>
          <li>Added error code 429 and its description in the Remarks of the section <a href="/Develop/References/CIC_API.md#EstablishDownchannel">Establishing a downchannel</a></li>
          <li>Removed route-finding templates (CarRoute, TransportationRoute) and the UI for route-finder was replaced with an ImageText template</li>
          <li>Corrected some errors and misprints in the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-12-18</td>
      <td>
        <ul>
          <li>Removed the ExpectSpeechTimedOut event message from the <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md">SpeechRecognizer</a> interface</li>
          <li>Removed the Clova.FreetalkState object from the <a href="/Develop/References/Context_Objects.md">context information</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-12-11</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#ClearQueue">ClearQueue</a> directive message to the <a href="/Develop/References/MessageInterfaces/AudioPlayer.md">AudioPlayer</a> interface</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-12-04</td>
      <td>
        <ul>
          <li>Added <a href="/Design/Audio.md#AudioInterruptionRule">audio interruption rules</a> to the Design guidelines</li>
          <li>Improved the images in the Design guidelines.</li>
          <li>Added the section on <a href="/Develop/Guides/Interact_with_CIC.md#UserAgentString">user-agent strings</a> for prerequisites before interacting with CIC</li>
          <li>Added a description of the 412 Precondition failed code in the <a href="/Develop/References/CIC_API.md#SendEvent">Sending event messages</a> section of the <a href="/Develop/References/CIC_API.md">CIC API</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-11-20</td>
      <td>
        <ul>
          <li>Added Design guidelines</li>
          <li>Added Type5 and Type6 to the subType value of the <a href="/Develop/References/ContentTemplates/CardList.md">CardList template</a> to display audio content and image thumbnails</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-11-13</td>
      <td>
        <ul>
          <li>Added UX details in the Remarks of directive messages for volume control (<a href="/Develop/References/MessageInterfaces/DeviceControl.md#Decrease">DeviceControl.Decrease</a>, <a href="/Develop/References/MessageInterfaces/DeviceControl.md#Increase">DeviceControl.Increase</a>, <a href="/Develop/References/MessageInterfaces/DeviceControl.md#SetValue">DeviceControl.SetValue</a>, <a href="/Develop/References/MessageInterfaces/PlaybackController.md#Mute">PlaybackController.Mute</a>, and <a href="/Develop/References/MessageInterfaces/PlaybackController.md#Unmute">PlaybackController.Unmute</a>)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-11-06</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#KeepRecording">SpeechRecognizer.KeepRecording</a> directive message</li>
          <li>Added <a href="/Develop/References/Context_Objects.md#Display">Device.Display</a> context information</li>
          <li>Added a token field to the <a href="/Develop/References/ContentTemplates/ActionTimer.md">ActionTimer</a>, <a href="/Develop/References/ContentTemplates/ActionTimerList.md">ActionTimerList</a>, <a href="/Develop/References/ContentTemplates/Alarm.md">Alarm</a>, <a href="/Develop/References/ContentTemplates/AlarmList.md">AlarmList</a>, <a href="/Develop/References/ContentTemplates/Memo.md">Memo</a>, <a href="/Develop/References/ContentTemplates/MemoList.md">MemoList</a>, <a href="/Develop/References/ContentTemplates/Reminder.md">Reminder</a>, <a href="/Develop/References/ContentTemplates/ReminderList.md">ReminderList</a>, <a href="/Develop/References/ContentTemplates/Schedule.md">Schedule</a>, <a href="/Develop/References/ContentTemplates/ScheduleList.md">ScheduleList</a>, <a href="/Develop/References/ContentTemplates/Timer.md">Timer</a>, and <a href="/Develop/References/ContentTemplates/TimerList.md">TimerList</a> templates</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-10-23</td>
      <td>
        <ul>
          <li>Added emotionCode and motionCode fields to the <a href="/Develop/References/ContentTemplates/Text.md">Text</a> template</li>
          <li>Changed assets[].url field contents of <a href="/Develop/References/MessageInterfaces/Alerts.md#SetAlert">Alerts.SetAlert</a> directive message</li>
          <li>Corrected an error in the example of the <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#StreamRequested">AudioPlayer.StreamRequested</a> event message</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-10-16</td>
      <td>
        <ul>
          <li>Added <a href="/Develop/References/MessageInterfaces/PlaybackController.md#Replay">Replay</a> directive message to the <a href="/Develop/References/MessageInterfaces/PlaybackController.md">PlaybackController</a> namespace</li>
          <li>Added supplementary description on alarm synchronization in the Overall process section</li>
          <li>Removed the content field from the <a href="/Develop/References/Context_Objects.md#AlertsState">Alert.AlertsState</a> of <a href="/Develop/References/Context_Objects.md#AlertInfoObject">AlertInfoObject</a> context information</li>
          <li>Revised some images and corrected errors in the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-10-02</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/References/MessageInterfaces/Alerts.md">Alerts</a> namespace and alarm related interface</li>
          <li>Added the <a href="/Develop/References/MessageInterfaces/System.md">System</a> namespace and alarm related interface</li>
          <li>Added the expectContentType field to the <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#ExpectSpeech">SpeechRecognizer.ExpectSpeech</a> directive message</li>
          <li>Added a warning field to the <a href="/Develop/References/Context_Objects.md#VolumeInfoObject">VolumeInfoObject</a> of <a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a></li>
          <li>Added templates: <a href="/Develop/References/ContentTemplates/ActionTimer.md">ActionTimer</a>, <a href="/Develop/References/ContentTemplates/ActionTimerList.md">ActionTimerList</a>, <a href="/Develop/References/ContentTemplates/Alarm.md">Alarm</a>, <a href="/Develop/References/ContentTemplates/AlarmList.md">AlarmList</a>, <a href="/Develop/References/ContentTemplates/Memo.md">Memo</a>, <a href="/Develop/References/ContentTemplates/MemoList.md">MemoList</a>, <a href="/Develop/References/ContentTemplates/Reminder.md">Reminder</a>, <a href="/Develop/References/ContentTemplates/ReminderList.md">ReminderList</a>, <a href="/Develop/References/ContentTemplates/Schedule.md">Schedule</a>, <a href="/Develop/References/ContentTemplates/ScheduleList.md">ScheduleList</a>, <a href="/Develop/References/ContentTemplates/Timer.md">Timer</a>, and <a href="/Develop/References/ContentTemplates/TimerList.md">TimerList</a></li>
          <li>Revised some code examples of the <a href="/Develop/References/ContentTemplates/ImageText.md">ImageText</a> template</li>
          <li>Revised some fields of the <a href="/Develop/References/ContentTemplates/Popup.md">Popup</a> template</li>
          <li>Added the Terms and Conditions of Service on <a href="/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken">Creating Clova access tokens</a> and <a href="/Develop/References/Clova_Auth_API.md#RequestAuthorizationCode">Requesting an authorization code</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-09-25</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/References/MessageInterfaces/PlaybackController.md#NextCommandIssued">PlaybackController.NextCommandIssued</a> and <a href="/Develop/References/MessageInterfaces/PlaybackController.md#PreviousCommandIssued">PlaybackController.PreviousCommandIssued</a> event messages for music playback control to the <a href="/Develop/References/MessageInterfaces/PlaybackController.md">PlaybackController API</a></li>
          <li>Added the expectSpeechId field to the <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#ExpectSpeech">SpeechRecognizer.ExpectSpeech</a> directive message, and added speechId and explicit fields to the <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a> event message, respectively</li>
          <li>Added the <a href="/Develop/References/ContentTemplates/Popup.md">Popup template</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-09-18</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/References/MessageInterfaces/DeviceControl.md#ExpectReportState">DeviceControl.ExpectReportState</a> directive message, <a href="/Develop/References/MessageInterfaces/DeviceControl.md#ReportState">DeviceControl.ReportState</a> event message, and <a href="/Develop/References/MessageInterfaces/DeviceControl.md#RequestStateSynchronization">DeviceControl.RequestStateSynchronization</a> event message to DeviceControl API and changed the name of the DeviceControl.UpdateDeviceState directive message to <a href="/Develop/References/MessageInterfaces/DeviceControl.md#SynchronizeState">DeviceControl.SynchronizeState</a></li>
          <li>Added the item3 field to the <a href="/Develop/References/ContentTemplates/Text.md">Text</a> template</li>
          <li>Added a source field in the <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#Play">AudioPlayer.Play</a> directive message</li>
          <li>Added the durationInMilliseconds field in <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#AudioStreamInfoObject">AudioStreamInfoObject</a> </li>
          <li>Added the Notifier namespace. Added <a href="/Develop/References/MessageInterfaces/Notifier.md#ClearIndicator">ClearIndicator</a> and <a href="/Develop/References/MessageInterfaces/Notifier.md#SetIndicator">SetIndicator</a> directive messages</li>
          <li>Added the <a href="/Develop/References/ContentTemplates/Atmosphere.md">Atmosphere</a> template</li>
          <li>Added an alert text on prohibited use for the bgClipURL field of weather templates due to license issues</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-09-11</td>
      <td>
        <ul>
          <li>Added the explicit field to the <a href="/Develop/References/MessageInterfaces/SpeechRecognizer.md#ExpectSpeech">SpeechRecognizer.ExpectSpeech</a> directive message</li>
          <li>Added <a href="/Develop/References/ContentTemplates/Common_Fields.md">common fields</a> specification to the <a href="/Develop/References/Content_Templates.md">Content</a> template</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-09-04</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/References/MessageInterfaces/Clova.md#Help">Clova.Help</a> directive message</li>
          <li>Added the <a href="/Develop/References/MessageInterfaces/DeviceControl.md#LaunchApp">DeviceControl.LaunchApp</a> directive message</li>
          <li>Added the TextRecognizer namespace and the <a href="/Develop/References/MessageInterfaces/TextRecognizer.md">TextRecognizer.Recognize</a> event message</li>
          <li>Rewrote the table of contents and descriptions of the <a href="/Develop/References/CIC_API.md">CIC API</a></li>
          <li>Updated content on CIC API: Added status codes for the request and response header, and applied the format of the REST API reference to the document</li>
          <li>Corrected some errors in the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-08-28</td>
      <td>
        <ul>
          <li>Added the specification on set-top box TV channels and on the power state to <a href="/Develop/References/Context_Objects.md#DeviceState">Device.DeviceState</a> and <a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl API</a></li>
          <li>Added and changed some target values in the <a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl API</a>: power, energysave, screenbrightness</li>
          <li>Changed the name SetPoint of the <a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl API</a> to <a href="/Develop/References/MessageInterfaces/DeviceControl.md#SetValue">SetValue</a></li>
          <li>Updated <a href="/Develop/References/Clova_Auth_API.md">Clova auth API</a>: Added request / response headers and status codes, and applied the format of the REST API reference to the document</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-08-21</td>
      <td>
        <ul>
          <li>Added the section on <a href="/Develop/Guides/Interact_with_CIC.md#ManageConnection">Renewing a Clova access token</a> and updated content on token APIs</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-08-14</td>
      <td>
        <ul>
          <li>Added description on the dialogue model</li>
          <li>Added <a href="/Develop/References/MessageInterfaces/DeviceControl.md">DeviceControl</a> API</li>
          <li>Added payload fields in <a href="/Develop/References/Context_Objects.md">Device.DeviceState</a>: airplane, battery, bluetooth, brightness, flashLight, gps, powerSavingMode, soundMode, volume, and wifi</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-08-04</td>
      <td>
        <ul>
          <li>Added the <a href="/Develop/References/MessageInterfaces/Clova.md#Hello">Clova.Hello</a> directive message</li>
          <li>Added the type field to the AudioItem object of the <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#Play">AudioPlayer.Play</a> directive message</li>
          <li>Added the urlPlayable field to the <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#AudioStreamInfoObject">AudioStreamInfoObject</a></li>
          <li>Added specification on <a href="/Develop/References/CIC_API.md#Error">CIC error messages</a></li>
          <li>Rewrote the contents of <a href="/Develop/References/CIC_API.md#MultipartMessage">multipart message</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-07-28</td>
      <td>
        <ul>
          <li>Removed PlayNext and Stop from <a href="/Develop/References/MessageInterfaces/AudioPlayer.md">AudioPlayer</a> (merged into <a href="/Develop/References/MessageInterfaces/PlaybackController.md">PlaybackController</a>)</li>
          <li>Changed the message names of <a href="/Develop/References/MessageInterfaces/PlaybackController.md">PlaybackController</a> (Mute, Next, Pause, Previous, Resume, Stop, Unmute, VolumeDown, and VolumeUp</li>
          <li>Added route-finding templates: CarRoute and TransportationRoute</li>
          <li>Added weather templates: <a href="/Develop/References/ContentTemplates/Humidity.md">Humidity</a>, <a href="/Develop/References/ContentTemplates/TodayWeather.md">TodayWeather</a>, <a href="/Develop/References/ContentTemplates/TomorrowWeather.md">TomorrowWeather</a>, <a href="/Develop/References/ContentTemplates/WeeklyWeather.md">WeeklyWeather</a>, and <a href="/Develop/References/ContentTemplates/WindSpeed.md">WindSpeed</a></li>
          </ul>
      </td>
    </tr>
    <tr>
      <td>2017-07-14</td>
      <td>
        <ul>
          <li>Added beginAtInMilliseconds field contents of <a href="/Develop/References/MessageInterfaces/AudioPlayer.md#AudioStreamInfoObject">AudioStreamInfoObject</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-06-19</td>
      <td>
        <ul>
          <li>Updated <a href="/Develop/Guides/Interact_with_CIC.md#ManageConnection">Managing connections</a> (for when HTTP PING frame is unavailable)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-06-08</td>
      <td>
        <ul>
          <li>Added the section on <a href="/Develop/Guides/Interact_with_CIC.md#ManageConnection">Managing connections</a> (HTTP PING) to <a href="/Develop/Guides/Interact_with_CIC.md">Interacting with CIC</a></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td>2017-05-29</td>
      <td>
        <ul>
          <li>Completed the CIC part of the document</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>
