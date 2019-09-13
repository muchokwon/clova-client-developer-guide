# Client states and events

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
