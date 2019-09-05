# Buttons

A client device must provide buttons to allow the user to control the device directly instead of speaking. This section describes the buttons that can be provided by the client and the guidelines for implementation.

* [Button types](#Buttons)
* [Button guidelines](#ButtonGuideline)

## Button types {#Buttons}

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

## Button guidelines {#ButtonGuideline}

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
