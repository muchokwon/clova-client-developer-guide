# Screens {#Screen}

The following UI components must be provided on the screen via the display device of the client.

* [Bootscreen](#BootingScreen)
* [Logo display](#DisplayingLogo)
* [Green Dot VUI](#GreenDotVUI)

## Bootscreen {#BootingScreen}

This is a screen displayed when device is turned on until booting is complete. Logos are usually displayed on the bootscreen. The Clova logo must stand alone and may not be displayed in combination with other logos. All logos must be displayed in identical size and ratio.

![](/Design/Assets/Images/Clova-Client-Partner_Logo_on_Loading_Screen.png) ![](/Design/Assets/Images/Clova-Client-Clova_Logo_on_Loading_Screen.png)

## Logo display {#DisplayingLogo}

This section explains how to place the Clova logo according to the screen layout.

* [Layout A](#LayoutA)
* [Layout B](#LayoutB)
* [Layout C](#LayoutC)

### Layout A {#LayoutA}

The UI screen covers the entire screen or a section on the lower part of the screen. The Clova logo is placed in the top left section.

![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_A-Bottom_Overlay.png) ![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_A-Full_Screen_Overlay.png)

* The Clova logo must be placed on the top left section.
* The Clova logo must not be transparent.

### Layout B {#LayoutB}

Similar to [layout A](#LayoutA), the UI screen covers the entire screen or a section on the lower part of the screen. The Clova logo is placed in the top right section.

![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_B-Bottom_Overlay.png) ![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_B-Full_Screen_Overlay.png)

* The Clova logo must be placed in the top right section.
* The Clova logo must not be transparent.

### Layout C {#LayoutC}

The screen only displays simple text format. The Clova logo is placed in the top section.

![](/Design/Assets/Images/Clova-Client-Logo_Display-Layout_C.png)

* The Clova logo must be placed in the top section.
* The source of the content must be stated at the bottom of the content.


## Green Dot VUI {#GreenDotVUI}

Green Dot VUI is both a voice search tool for Clova and a UI design that represents the identity of Clova. Green Dot VUI expresses the states related to Clova voice actions such as receipt of user audio input or Clova audio output.

A client device with a screen must be able to implement Green Dot VUI. This section provides explanation on what colors are used in Green Dot VUI, and how they must be expressed depending on the client state.

* [Green Dot VUI color](#GreenDotVUIColor)
* [Green Dot VUI rendering](#RenderGreenDotVUI)

### Green Dot VUI color {#GreenDotVUIColor}

Green Dot VUI must be expressed by putting three types of gradients in the following ring shapes.

![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Color.png)

The information of the 3 colors used in the above image are as follows:

| Color        | RGB value       | CMYK value     | Pantone value   |
|----------------|-------------|-------------|-------------|
| Greendot Green | <span style="color:#03EB64; font-size:150%; vertical-align:middle;">&#9724;</span> 3, 235, 100(#03EB64) | 60, 0, 75, 0   | 2270C |
| Greendot Mint  | <span style="color:#14E6BE; font-size:150%; vertical-align:middle;">&#9724;</span>20, 230, 190(#14E6BE) | 50, 0, 25, 0   | 3255C |
| Greendot Blue  | <span style="color:#1EC8EB; font-size:150%; vertical-align:middle;">&#9724;</span>30, 200, 235(#1EC8EB) | 70, 5,  0, 0   |  298C |


### Green Dot VUI motion {#GreenDotVUIMotions}

The UI expression of Green Dot VUI varies depending on the identified action. Below is a table which describes the actions of Green Dot VUI.

| Action        | Description                           | UI Motion |
|----------------|-------------------------------|---------------|
| Intro          | The motion displayed when the user has invoked a wake word or pressed a button.         | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Intro.gif)      |
| Waiting        | The motion displayed until the voice input of the user is actually entered.         | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Waiting.gif)    |
| Listening      | The motion displayed when the voice input of the user is actually recognized and entered. | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Listening.gif)    |
| Processing | The motion displayed when the user request is analyzed and handled.       | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Processing.gif) |
| Answering      | The motion displayed when a response on the user request is being output as TTS. This motion is classified into the following three sections:<ul><li>Entry section: Expresses that TTS has started (frames 1-13).</li><li>Repeat section: Expresses the TTS output (frames 14-29). This section must be played repeatedly until the response is completed.</li><li>End section: Expresses that the response has ended (frames 30-41).</li></ul>    | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Answering.gif)  |
| Complete       | The motion displayed when completing a response on the user request.            | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Complete.gif)  |
| Error          | A state where an error has occurred.                                       | ![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Error.gif)     |

<div class="note">
  <p><strong>Note!</strong></p>
  <p>The speed of showing the Green Dot VUI motion must be 30 fps.</p>
</div>

The Green Dot VUI motion must be expressed according to the user action or the change of the [client state](#ClientStateAndEvent). Below are the explanations of the rules on how Green Dot VUI must be expressed according to each state.

* If the client enters **Attending** mode after the user invokes a wake word or presses a button, play the **Intro** motion .
* After displaying the **Intro** action once, repeat the **Waiting** motion until the actual voice input of the user gets started through the microphone.
* Once the client enters **Listening** mode as the actual voice of the user starts to be entered, repeat the **Speaking** motion until the voice input of the user is completed.
* Once the client enters the **Processing & reporting** state after the user input is completed, repeat the **Processing** motion until the response is output or the result screen is shown.
* When outputting TTS, play the entry section of **Answering** once then repeat the repeat section until the response is completed. Play the end section once when TTS ends.
* If it is difficult to express by identifying the **Answering** action, repeat the repeat section only.
* Once the client enters the **Idle** state by performing all necessary tasks on the user request, play the **Complete** action once.

An example of Green Dot VUI motion is shown below.

![](/Design/Assets/Images/Clova-Client-Green_Dot_VUI_Example.gif)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Please contact the Clova partnership team if you want access to Green Dot VUI image assets.</p>
</div>

## Push to talk button {#PushToTalkButton}

Push to talk button is a UI design to activate [Green Dot VUI](#GreenDotVUI). Push to talk button is provided as a touch UI button on the client device's screen. When the user touches the UI button, Green Dot VUI must be activated.

This section provides explanation on what colors are used in Push to talk button, and how they must be expressed depending on the client state.

* [Push to talk button color](#PushToTalkButtonColor)
* [Identifying Push to talk button presentation](#PushToTalkButtonPresentation)  

### Push to talk button color {#PushToTalkButtonColor}

When the client device's microphone is activated, Push to talk button must be expressed by putting three types of gradients in the following shape.

![](/Design/Assets/Images/Clova-Client-Push_To_Talk_Button_Active.png)

The information of the 3 colors used in the above image are as follows:

| Color        | RGB value       | CMYK value     | Pantone value   |
|----------------|-------------|-------------|-------------|
| Greendot Green | <span style="color:#03EB64; font-size:150%; vertical-align:middle;">&#9724;</span> 3, 235, 100(#03EB64) | 60, 0, 75, 0   | 2270C |
| Greendot Mint  | <span style="color:#14E6BE; font-size:150%; vertical-align:middle;">&#9724;</span>20, 230, 190(#14E6BE) | 50, 0, 25, 0   | 3255C |
| Greendot Blue  | <span style="color:#1EC8EB; font-size:150%; vertical-align:middle;">&#9724;</span>30, 200, 235(#1EC8EB) | 70, 5, 0, 0   |  298C |

When the microphone is deactivated or in the mute on state, Push to talk button must be expressed by grayed out.

![](/Design/Assets/Images/Clova-Client-Push_To_Talk_Button_Mute.png)

The information of the color in the above image are as follows:

| RGB value       | Opacity    |
|-------------|-------------|
| <span style="color:#999999; font-size:150%; vertical-align:middle;">&#9724;</span>153, 153, 153(#999999)     | 25%         |


### Identifying Push to talk button presentation {#PushToTalkButtonPresentation}

The Push to talk button presentation must be expressed according to the user action or the change of the client state. Below are the explanations of the rules on how Push to talk button must be expressed according to each state.

* When the client device's microphone is activated:
  - If the client state is **Idle**, display the Push to talk button UI on the client device's screen.
  - If the user touches the Push to talk button, activate the Green Dot VUI.
  - After processing the user utterances and the Green Dot VUI motion finished, back to Push to talk button UI.

![](/Design/Assets/Images/Clova-Client-Push_To_Talk_Button_Presentation.gif)

* When the client device's microphone is deactivated or in the mute on state:
  - Push to talk button UI is displayed to be grayed out.

![](/Design/Assets/Images/Clova-Client-Push_To_Talk_Button_Mute_Presentation.png)

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Please contact the Clova partnership team if you want access to Push to talk button image assets.</p>
</div>
