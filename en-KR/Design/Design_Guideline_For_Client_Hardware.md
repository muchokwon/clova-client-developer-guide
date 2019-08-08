<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

# Design guidelines for client devices

A consistent UI and UX must be provided to users of Clova client devices for a familiar, convenient experience when using the product. The design guidelines on the following items are provided for designing the client devices that access Clova.

* [Client states and events](#ClientStateAndEvent)
* [Buttons](#Button)
* [Lights](#Light)
* [Audio](#Audio)
* [Screen](#Screen)
* [Green Dot VUI](#GreenDotVUI)
* [Clova inside](#ClovaInside)


<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>Any guidelines or specifications not mentioned in this document can be implemented according to the manufacturer needs or policies. However, if you are uncertain and need help to decide, contact the partnership team.</p>
</div>

## Client states and events {#ClientStateAndEvent}

You should design and implement the client devices for easy user recognition and operation such as for user voice inputs, Clova voice outputs, microphone states, or error occurrences (e.g., an error in the Clova service or network). For this, you need to understand the states of the client device and the actions and flow between states. The following diagram shows the client state cycle.

![](/Design/Assets/Images/Clova-Client-State_Diagram.png)

The description on the client states are as follows:

| State                | Description                                                            |
|------------------------|--------------------------------------------------------------------|
| Idle                   | A state where the client is not performing any tasks.                             |
| Attending              | A state where the client is waiting for the user voice input.                        |
| Listening              | A state where the client is receiving the user voice input.                            |
| Processing & reporting | A state where Clova is processing the user voice request or Clova voice is being output through the speakers. |
| Error                  | A state where a system error has occurred.                                                |
| Mute on                | A state where the microphone is set to mute.                                               |

The above states can be expressed using [Lights](#Light), [Sound effects](#SoundEffect), or [Screens](#Screen). And the transition between states can be initiated or executed by the user voice, [Buttons](#Button) controls, or other environmental factors.

The following events can occur in the client and must also be expressed using sounds or lights.

| Event name               | Description                                                           |
|------------------------|--------------------------------------------------------------------|
| Alarm             | Sounds at a specified date and time.                                           |
| Reminder       | Displays or reads out content input by the user at a specified date and time.         |
| Timer            | Sounds after a specified time has elapsed.                                        |

## Buttons {#Button}

A client device must provide buttons to allow the user to control the device directly instead of speaking. This section describes the buttons that can be provided by the client and the guidelines for implementation.

* [Button types](#Buttons)
* [Button guidelines](#ButtonGuideline)

### Button types {#Buttons}

A client device must provide the following buttons:

| Button name                        | Description                                                     | Required |
|----------------------------|-------------------------------------------------------------|:---------:|
| Power             | Turns the device on or off.                                        | Required      |
| Mic mute    | Enables or disables the microphone.                                | Required      |
| Volume up       | Increases the volume of the speaker.                                         | Required      |
| Volume down   | Decreases the volume of the speaker.                                          | Required      |
| Play/Pause | Plays or pauses the music. Or, pauses a task in progress.         | Required      |
| Wake up   | Switches to the attending state, a mode to receive the user voice input. An example of this action is a user saying "Clova." | Optional      |
| Wi-Fi          | Connects or disconnects Wi-Fi.                                 | Optional      |
| Bluetooth     | Pairs with, connects to, or disconnects from the Bluetooth device.                              | Optional      |
| Reset          | Resets the device.                                              | Optional      |

### Button guidelines {#ButtonGuideline}

The following guidelines must be followed when providing buttons.

* The power and the mic mute buttons are recommended to be provided as physical buttons.
* Buttons other than the power and the mic mute buttons can be provided in various forms according to the manufacturer policy, such as a physical or touch UI buttons.
* The most commonly used buttons are recommended to be placed on the front or the top of the client device for easy operation.
* If you are providing a touch UI button, make sure that the defined action is executed upon the touch release gesture.
* If there are no predefined combinations of buttons, only the function of the first recognized button must be executed at a simultaneous input of multiple buttons.
* If there is new button input while a task is already in progress from a previous input, the feedback effect on the previous input must be stopped to provide the feedback effect on the latest input.
* It is permitted to provide the play and pause button as GUIs for devices supporting UI screens.
* The buttons may be provided via the following methods:
  - An exclusive button for one type of function (e.g., a Bluetooth button)
  - Long-press gesture on a button to provide a function (e.g., reset by long-pressing the Power button)
  - Combination of multiple buttons to provide a function (e.g., reset by pressing the power and play buttons at the same time)

## LED {#Light}

A client device must provide LED to indicate the [Client states and events](#ClientStateAndEvent) or the feedback on user request. This section describes the lights that can be provided by the client and the guidelines for implementation.

* [Light colors](#LightColor)
* [Light effects](#LightEffect)
* [Light guidelines](#LightGuideline)

### Light colors {#LightColor}

The client must use the following colors of light:

| Light color     | RGB value                | Description                                   | Required |
|-------------|----------------------|---------------------------------------|:--------:|
| Green       | <span style="color:#05D686; font-size:150%; vertical-align:middle;">&#9724;</span> 5, 214, 134(#05D686)   | Receipt of user audio input                                  | Required  |
| Yellow Green | <span style="color:#96FF00; font-size:150%; vertical-align:middle;">&#9724;</span> 150, 255, 0(#96FF00)    | Clova notification                             | Required  |
| Red         | <span style="color:#FF0000; font-size:150%; vertical-align:middle;">&#9724;</span> 255, 0, 0(#FF0000)      | Errors including mic mute, network connection error, or not enough battery     | Required  |
| Warm White   | <span style="color:#F0E6E6; font-size:150%; vertical-align:middle;">&#9724;</span> 240, 230, 230(#F0E6E6)  | Clova voice outputs and receipt of alarm/reminder/timer events through speaker                             | Required  |

The following image shows the implementation of light colors on Wave:

<table style="width:600px;">
  <thead>
    <tr>
      <th style="width:150px;">Green</th>
      <th style="width:150px;">Yellow Green</th>
      <th style="width:150px;">Red</th>
      <th style="width:150px;">Warm White</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><img src="Assets/Images/Clova-Client-Light-Wave_Green.png" alt=""></td>
      <td><img src="Assets/Images/Clova-Client-Light-Wave_Yellow_Green.png" alt=""></td>
      <td><img src="Assets/Images/Clova-Client-Light-Wave_Red.png" alt=""></td>
      <td><img src="Assets/Images/Clova-Client-Light-Wave_Warm_White.png" alt=""></td>
    </tr>
  </tbody>
</table>

### Light effects {#LightEffect}

Light effects are used for the purpose of delivering more details on the meaning or state in addition to the meaning delivered by the [Light colors](#LightColor).

The table below shows the light effects that must be expressed when implemented on a client device along with its descriptions and examples.

| Light effect                            | Description                                      | Example                                                               |
|------------------------------------|------------------------------------------|-------------------------------------------------------------------|
| Lights up                     | Switches directly to the light on state without any other effects.   | ![](/Design/Assets/Images/Clova-Client-Light-Wave_Sustain.gif)              |
| Repeat pulse         | Slowly brightens and dims the intensity of a light repeatedly. | ![](/Design/Assets/Images/Clova-Client-Light-Wave_Repeat_Blink_Slowly.gif)  |
| Fade out                 | Slowly dims the intensity of light and eventually turns off. | ![](/Design/Assets/Images/Clova-Client-Light-Wave_Fade_Out.gif)             |
| Repeat splash          | Repeats the sideways rippling effect. | ![](/Design/Assets/Images/Clova-Client-Light-Wave_Splash.gif)         |

The table below shows how to express the client device [states and events](#ClientStateAndEvent) using light.

| State or event               | Light effect                | Required |
|----------------------------|----------------------------|:---------:|
| Attending & listening state     | Turn on the Green light.              | Required     |
| End state                    | Slowly turn off the Warm White light.     | Required     |
| Error state                  | Slowly flicker the Red light repeatedly.       | Required     |
| Mute on state                | Turn on the Red light.                | Required     |
| Processing & reporting state | Slowly flicker the Warm White light repeatedly. | Required     |
| Disabling the mute on state            | Slowly turn off the Red light.           | Required     |
| Immediately after exceeding the wait time           | Slowly turn off the Green light.<div class="note"><p><strong>Note!</strong></p><p>Lighting effect on this event is scheduled to be removed from the implemented items if not mandatory anymore.</p></div>         | Optional     |
| Start alarm, reminder, or timer      | Repeat the rippling effect of the Warm White light.  | Optional     |

### Light guidelines {#LightGuideline}

The following guidelines must be followed when providing lights.
  - A person with eyesight of 0.7 (decimal visual acuity) must be able to distinguish the [Light colors](#LightColor) within 1 meter distance.
  - For light colors, avoid applying states or meanings other than those that are predefined.
  - The light colors must be applied so that the user can perceive the graphic RGB color and the actual light color as the same.
  - In addition to the essential [Light effects](#LightEffect), you can add other light colors or effects depending on your UX policies or for appropriate situations, such as bootup, speaker volume controls, charging status, and button feedback.
  - It is recommended that you do not express too many meanings or states with a single light color or effect.
  - For screenless devices, it is recommended to indicate the level of the speaker volume by methods, such as brightness of lights.
  - For battery powered portable devices, it is recommended to implement the light effect so the information on the charging status can be gained from the light.

## Sound {#Audio}

This section describes the guidelines for outputting audio content or sound effects from the client device.

* [Rules for basic audio playback](#AudioInterruptionRule)
* [Audio playback rules for user utterances](#AudioInterruptionRuleForUserUtterance)
* [Sound effects](#SoundEffect)
* [Supported audio compression formats](#SupportedAudioCompressionFormat)

### Rules for basic audio playback {#AudioInterruptionRule}

A client may have to play other audio content during audio playback. In this case, the client must play audio content according to the rules for audio playback. The rules for audio playback were written based on the types of audio content. Thus, an understanding of the types of audio content is required before understanding the audio playback rules. Audio content types are classified as shown below in the table and the client must classify the audio contents sent from Clova according to the CIC API namespace related to each audio content type.

| Audio content type | Description                                                  | Related CIC API namespace             |
|---------------|-------------------------------------------------------|----------------------------------|
| Alert         | Audio content such as alarm sounds, timer sounds, reminder sounds, reminder utterance, emergency warning sounds.             | [`Alerts`](/Develop/References/CICInterface/Alerts.md) |
| Content       | Audio content such as music, stories, news, and podcasts provided upon user request.                           | [`AudioPlayer`](/Develop/References/CICInterface/AudioPlayer.md) |
| Dialogue      | TTS audio content provided upon user request.                                                  | [`SpeechRecognizer`](/Develop/References/CICInterface/SpeechRecognizer.md), [`SpeechSynthesizer`](/Develop/References/CICInterface/SpeechSynthesizer.md) |
| Feedback      | Audio content such as reset sound, ring tone, and ringback tone.                              | None (Determined by the client itself) |
| Notification  | Audio content such as Beep sound, system state utterance (e.g., notification for low battery or Bluetooth disconnection), notification sound, and notification utterance.         | [`Notifier`](/Develop/References/CICInterface/Notifier.md) |

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Alert and notification types must handle sound effects and utterances as a single audio content. For example, a reminder must recognize the reminder sound and utterance as a single alert audio content. For a low battery notification, a system state utterance such as "Not enough battery," and the beep sound must be handled as a single notification audio content.</p>
</div>

The rules for audio playback are as follows:

* For physical buttons, the sound effects must be played instantly being output in the mixing method.
* Audio content must be played instantly. If an audio content is playing, it must be processed as background audio to play the new audio.
* However, if the [Content type](#AudioInterruptionRule) of the audio playing and the new audio is the same, follow the process below.
  - **Alert, Content, Dialogue, Feedback type**: Cancel the audio content playing and play the new audio content.
  - For **Notification type**: Continue playing the audio content and keep the new audio content in the playback queue. After finishing the current audio content, play the following audio content in the queue order.
* To cancel playing the audio content, you must first cancel the currently playing audio content.

Based on above rules, the following table shows how to process the playing audio content for each audio content type:

<table style="text-align:center">
  <thead>
    <tr>
      <th rowspan="2">Type of playing content</th><th colspan="5">Type of content to play</th><th rowspan="2">Sound effect of a physical button</th>
    </tr>
    <tr>
      <th>Alert</th><th>Content</th><th>Dialogue</th><th>Feedback</th><th>Notification</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alert</th>
      <td><span class="audioInterruptionRule cancelPlayback">Cancel play</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
      <td rowspan="5"><span class="audioInterruptionRule">Process mixing</span></td>
    </tr>
    <tr>
      <th>Content</th>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
      <td><span class="audioInterruptionRule cancelPlayback">Cancel play</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
    </tr>
    <tr>
      <th>Dialogue</th>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
      <td><span class="audioInterruptionRule cancelPlayback">Cancel play</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
    </tr>
    <tr>
      <th>Feedback</th>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
      <td><span class="audioInterruptionRule cancelPlayback">Cancel play</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
    </tr>
    <tr>
      <th>Notification</th>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
      <td><span class="audioInterruptionRule backgroundPlay">Process as background audio</span></td>
      <td><span class="audioInterruptionRule continuePlayback">Continue playing (in queue)</span></td>
    </tr>
  </tbody>
</table>

### Audio playback rules for user utterance {#AudioInterruptionRuleForUserUtterance}

If a user attempts voice input while the client is playing audio content, follow the rules below:

* If there is audio content being played, process it as background audio from the attending state to the processing and reporting state.
* If the waiting time for voice input has been exceeded or the user request processing has failed, the audio content processed as the background audio must be played in the original mode.
* If a different audio content has to be played depending on the processed result of the request, the audio content must be played in accordance with the [Rules for basic audio playback](#AudioInterruptionRule).
* The same rules apply for the listening and processing and reporting states gained from attempting multi-turn dialogues.

If there is a request to play a new audio content during the attending and listening state, process the request as follows:

* For playing audio content of **alert/dialogue/content** type, the audio content must be played after canceling the receiving of user voice input.
* For playing audio content of **notification** type or **feedback** type, the audio content must be played as the background audio.

### Sound effects {#SoundEffect}

A client must provide sound effects in addition to the [Lights](#Light) in order to convey the device status or feedback on a user request. This section explains the types of sound effects to be provided by the client to the user and the situations for providing it.

* [Types of sound effects](#SoundEffects)
* [Sound effect guidelines](#SoundEffectGuideline)

#### Sound effect type {#SoundEffects}

In order to express [states and events](#ClientStateAndEvent) of the client, the following sound effects must be provided:

| State or event              | Sample sound effect                     | Required |
|---------------------------|------------------------------|:---------:|
| Entering the attending state         | <audio title="Attending" controls><source src="./Assets/Sounds/Clova-Client-Soundeffect-Attending.wav" type="audio/wav" /></audio> | Optional     |
| Entering the error state             | <audio title="Error" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Error.wav" type="audio/wav" /></audio>         | Required     |
| Entering the mute on state           | <audio title="Mute on" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Mute_On.wav" type="audio/wav" /></audio>     | Required     |
| Disabling the mute on state           | <audio title="Mute off" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Mute_Off.wav" type="audio/wav" /></audio>   | Required     |
| Alarm (When an event occurs, repeat the sound effect.) | <audio title="Alarm" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Alarm.wav" type="audio/wav" /></audio>         | Required     |
| Reminder (When an event occurs, repeat the sound effect and the reminder details in TTS order.) | <audio title="Reminder" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Reminder.wav" type="audio/wav" /></audio>         | Required     |
| Timer (When an event occurs, repeat the sound effect.)      | <audio title="Timer" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Timer.wav" type="audio/wav" /></audio>         | Required     |

#### Sound effect guidelines {#SoundEffectGuideline}

The following guidelines must be followed when providing sound effects.

* It is recommended that the provided sound effects are used without any alteration.
* Other sound effects may be added depending on your UX policies or for appropriate situations.
* Users must be able to recognize each situation by the sound.
* The sound effects must be consistent with the light effects or screen states.
* If you are providing a sound effect for button feedback, the sound effect must be suitable to the property and feeling of a button.

### Supported audio compression formats {#SupportedAudioCompressionFormat}

<!-- Start of the shared content: SupportedAudioCompressionFormat -->

Since a client must play the audio content delivered by Clova, it must be able to play the audio compression formats supported by Clova.

Audio compression formats supported by Clova are as follows:

| Audio compression format                     | File extension | License cost |
|----------------------------------|:--------:|-----------|
| MPEG-1 or MPEG-2 Audio Layer III | .mp3     | Free       |

<div class="tip">
  <p><strong>Tip!</strong></p>
  <p>More audio compression formats may be additionally supported by Clova in the future.</p>
</div>

<!-- End of the shared content -->

## Screens {#Screen}

The following UI components must be provided on the screen via the display device of the client.

* [Bootscreen](#BootingScreen)
* [Logo display](#DisplayingLogo)
* [Green Dot VUI](#GreenDotVUI)

### Bootscreen {#BootingScreen}

This is a screen displayed when device is turned on until booting is complete. Logos are usually displayed on the bootscreen. The Clova logo must stand alone and may not be displayed in combination with other logos. All logos must be displayed in identical size and ratio.

![](/Design/Assets/Images/Clova-Client-Partner_Logo_on_Loading_Screen.png) ![](/Design/Assets/Images/Clova-Client-Clova_Logo_on_Loading_Screen.png)

### Logo display {#DisplayingLogo}

This section explains how to place the Clova logo according to the screen layout.

* [Layout A](#LayoutA)
* [Layout B](#LayoutB)
* [Layout C](#LayoutC)

#### Layout A {#LayoutA}

The UI screen covers the entire screen or a section on the lower part of the screen. The Clova logo is placed in the top left section.

![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_A-Bottom_Overlay.png) ![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_A-Full_Screen_Overlay.png)

* The Clova logo must be placed on the top left section.
* The Clova logo must not be transparent.

#### Layout B {#LayoutB}

Similar to [layout A](#LayoutA), the UI screen covers the entire screen or a section on the lower part of the screen. The Clova logo is placed in the top right section.

![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_B-Bottom_Overlay.png) ![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_B-Full_Screen_Overlay.png)

* The Clova logo must be placed in the top right section.
* The Clova logo must not be transparent.

#### Layout C {#LayoutC}

The screen only displays simple text format. The Clova logo is placed in the top section.

![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_C.png)

* The Clova logo must be placed in the top section.
* The source of the content must be stated at the bottom of the content.


### Green Dot VUI {#GreenDotVUI}

Green Dot VUI is the voice search tool of Clova and UI design which shows the identity. Green Dot VUI expresses the states related to Clova voice actions such as receipt of user audio input or Clova audio output.

A client device with a screen must be able to implement Green Dot VUI. This section provides explanation on what colors are used in Green Dot VUI, and how they must be expressed depending on the client state.

* [Green Dot VUI color](#GreenDotVUIColor)
* [Green Dot VUI rendering](#GreenDotVUIMotions)

#### Green Dot VUI color {#GreenDotVUIColor}

Green Dot VUI must be illustrated by putting three types of gradients in the following doughnut-shaped hook.

![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Color.png)

The information of the three colors used in the above image are as follows:

| Color        | RGB value       | CMYK value     | Pantone value   |
|----------------|-------------|-------------|-------------|
| Greendot Green | <span style="color:#03EB64; font-size:150%; vertical-align:middle;">&#9724;</span> 3, 235, 100(#03EB64) | 60, 0, 75, 0   | 2270C |
| Greendot Mint  | <span style="color:#14E6BE; font-size:150%; vertical-align:middle;">&#9724;</span>20, 230, 190(#14E6BE) | 50, 0, 25, 0   | 3255C |
| Greendot Blue  | <span style="color:#1EC8EB; font-size:150%; vertical-align:middle;">&#9724;</span>30, 200, 235(#1EC8EB) | 70, 5,  0, 0   |  298C |


#### Green Dot VUI motion {#GreenDotVUIMotions}

The UI expression of Green Dot VUI varies depending on the identified action. Below is a table which describes the actions of Green Dot VUI.

| Action        | Description                           | UI Motion |
|----------------|-------------------------------|---------------|
| Intro          | The motion displayed when the user has invoked a wake word or pressed a button.         | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Intro.gif)      |
| Waiting        | The motion displayed until the voice input of the user is actually entered.         | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Waiting.gif)    |
| Speaking      | The motion displayed when the voice input of the user is actually recognized and entered. | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Speaking.gif)    |
| Processing | The motion displayed when the user request is analyzed and handled.       | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Processing.gif) |
| Answering      | The motion displayed when a response on the user request is being output as TTS. This motion is classified into the following three sections:<ul><li>Entry section: Indicates that TTS has started (frames 1-13).</li><li>Repeat section: Expresses the TTS output (frames 14-29). This section must be played repeatedly until the response is completed.</li><li>End section: Indicates that the response has ended (frames 30-41).</li></ul>    | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Answering.gif)  |
| Complete       | The motion displayed when completing a response on the user request.            | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Complete.gif)  |
| Error          | A state where an error has occurred.                                       | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Error.gif)     |

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The speed of showing the Green Dot VUI motion must be 30 fps.</p>
</div>

The Green Dot VUI motion must be expressed according to the user action or the change of the [client state](#ClientStateAndEvent). Below are the explanations of the rules on how Green Dot VUI must be expressed according to each state.

* If the client enters the **Attending** mode after the user invokes a wake word or presses a button, play the **Intro** motion .
* After displaying the **Intro** action once, repeat the **Waiting** motion until the actual voice input of the user gets started through the microphone.
* Once the client enters the **Listening** mode as the actual voice of the user starts to be entered, repeat the **Speaking** motion until the voice input of the user is completed.
* Once the client enters the **Processing & reporting** state after the user input is completed, repeat the **Processing** motion until the response is output or the result screen is shown.
* When outputting TTS, play the entry section of **Answering** once then repeat the repeat section until the response is completed. Play the end section once when TTS ends.
* If it is difficult to express by identifying the **Answering** action, repeat the repeat section only.
* Once the client enters the **Idle** state by performing all necessary tasks on the user request, play the **Complete** action once.

An example of Green Dot VUI motion is shown below.

![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Example.gif)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>To get image data of the Green Dot VUI motion, please contact the Clova partnership team.</p>
</div>

## Clova inside {#ClovaInside}
Clova inside is a brand element to inform that Clova technology  is mounted on a specific product. Clova inside is to be properly indicated on the materials for expression such as printed media after identifying the characteristics of applied media and environment.

To this end, this section covers the following topics:

* [Clova inside badge](#ClovaInsideBadge)
* [Clova inside color](#ClovaInsideColor)
* [Clova inside background color](#ClovaInsideBackgroundColor)
* [Clova inside rules](#ClovaInsideRules)

### Clova inside badge {#ClovaInsideBadge}
There are two types of Clova inside badge as shown below:

| Basic Type                                                        | Text Type                                                      |
|:-----------------------------------------------------------:|:-----------------------------------------------------------:|
| ![](/Design/Assets/Images/Clova_Inside-Basic_Type_Badge.png)  | ![](/Design/Assets/Images/Clova_Inside-Text_Type_Badge.png)  |

There are rules on shape and size according to the type of Clova inside badge. First, the rules on the **basic type** badge are as shown below:
* Must have top, bottom, left and right margins with the length that has divided the badge height into three equal lengths.
* Minimum size of the badge is 6 millimeters if it is a printed material, 17 pixels if displayed on the screen, as a size based on the height.

![](/Design/Assets/Images/Clova_Inside-Basic_Type_Badge-Rules.png)

The rules on the **text type** badge are as shown below:

* Must have top, bottom, left and right margins with the same length as the badge height.
* Minimum size of the badge is 2.1 millimeters if it is a printed material, 6 pixels if displayed on the screen, as a size based on the height.

![](/Design/Assets/Images/Clova_Inside-Text_Type_Badge-Rules.png)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The provided image must be used as it is for the Clova inside badge without creating at random. To get the Clova inside image, please contact the Clova partnership team.</p>
</div>

### Clova inside color {#ClovaInsideColor}
Use one of the four colors for the Clova inside badge and the badge color as shown below:

![](/Design/Assets/Images/Clova_Inside-Color.png)

The information of the four colors used in the above image are as follows:

| Color        | RGB value       | CMYK value     | Pantone value   |
|----------------|-------------|-------------|-------------|
| Clova Green    | <span style="color:#05D686; font-size:150%; vertical-align:middle;">&#9724;</span>  5, 214, 134(#05D686) | 70,  0, 60,  0 | 3395C |
| Clova Black    | <span style="color:#1E1E1E; font-size:150%; vertical-align:middle;">&#9724;</span> 30,  30,  30(#1E1E1E) |  0,  0,  0, 90 | <!-- --> |
| Clova Grey     | <span style="color:#B1B1B1; font-size:150%; vertical-align:middle;">&#9724;</span>177, 177, 177(#B1B1B1) |  0,  0,  0, 50 | Coolgrey 5C |
| Clova White    | <span style="color:#FFFFFF; font-size:150%; vertical-align:middle;">&#9724;</span>255, 255, 255(#FFFFFF) |  0,  0,  0,  0 | <!-- --> |


### Clova inside background color {#ClovaInsideBackgroundColor}

When applying the [Clova inside badge](#ClovaInsideBadge), you must first determine the background color of the badge. You must apply the appropriate color according to the environment or the condition such as device or product package material for the background of Clova inside, and must select from the [Clova inside color](#ClovaInsideColor). For the Clova inside background color and badge color, you must use one of the combinations below:

![](/Design/Assets/Images/Clova_Inside-Background_Color-Combinations.png)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>When determining the color of badge and background, it is recommended to use the combination with the Clova Green color first. If spot color printing is not possible, use the black and white combination.</p>
</div>

### Clova inside rules {#ClovaInsideRules}

The rules that must be followed when applying Clova inside are as shown below:

<ul>
  <li>When expressing Clova inside, be sure to use the predetermined <a href="#ClovaInsideBadge">Clova inside badge</a>.</li>
  <li>Use the predetermined <a href="#ClovaInsideColor">color</a> and <a href="#ClovaInsideBackgroundColor">background color</a> combination for the Clova inside badge and the background color.
  <li>Be sure to comply with the spacing rules and the minimum size of the <a href="#ClovaInsideBadge">Clova inside badge</a>.</li>
  <li>It is recommended to use <strong>text type</strong> badge when applying to a device. Below is an example of applying <strong>text type</strong> Clova inside badge to a device:<br />
    <img src="/Design/Assets/Images/Clova_Inside-Device_Exmaple.png" />
  </li>
  <li>It is recommended to use <strong>basic type</strong> badge when applying to a product package. Below is an example of applying <strong>basic type</strong> Clova inside badge to a product package:<br />
    <img src="/Design/Assets/Images/Clova_Inside-Package_Example.png" />
  </li>
  <li>It is recommended to use combination with a description message as shown below when applying to a product package:
    <ul>
      <li>Below is a form that has combined badge and message horizontally:<br />
        <img src="/Design/Assets/Images/Clova_Inside-Horizontal_Signature_For_Package.png" />
      </li>
      <li>Below is a form that has combined badge and message vertically:<br />
        <img src="/Design/Assets/Images/Clova_Inside-Vertical_Signature_For_Package.png" />
      </li>
    </ul>
  </li>
</ul>
