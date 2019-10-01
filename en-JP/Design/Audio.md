<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->
# Sound {#Audio}

This section describes the guidelines for outputting audio content or sound effects from the client device.

* [Rules for basic audio playback](#AudioInterruptionRule)
* [Audio playback rules for user utterances](#AudioInterruptionRuleForUserUtterance)
* [Sound effects](#SoundEffect)
* [Supported audio compression formats](#SupportedAudioCompressionFormat)

## Rules for basic audio playback {#AudioInterruptionRule}

A client may have to play other audio content during audio playback. In this case, the client must play audio content according to the rules for audio playback. The rules for audio playback were written based on the types of audio content. Thus, an understanding of the types of audio content is required before understanding the audio playback rules. Audio content types are classified as shown below in the table and the client must classify the audio contents sent from Clova according to the CIC API namespace related to each audio content type.

| Audio content type | Description                                                  | Related CIC API namespace             |
|---------------|-------------------------------------------------------|----------------------------------|
| Alert         | Audio content such as alarm sounds, timer sounds, reminder sounds, reminder utterance, emergency warning sounds.             | [`Alerts`](/Develop/References/CICInterface/Alerts.md) |
| Content       | Audio content such as music, picture book and radio provided upon user request.                           | [`AudioPlayer`](/Develop/References/CICInterface/AudioPlayer.md) |
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

## Audio playback rules for user utterance {#AudioInterruptionRuleForUserUtterance}

If a user attempts voice input while the client is playing audio content, follow the rules below:

* If there is audio content being played, process it as background audio from the attending state to the processing and reporting state.
* If the waiting time for voice input has been exceeded or the user request processing has failed, the audio content processed as the background audio must be played in the original mode.
* If a different audio content has to be played depending on the processed result of the request, the audio content must be played in accordance with the [Rules for basic audio playback](#AudioInterruptionRule).
* The same rules apply for the listening and processing and reporting states gained from attempting multi-turn dialogues.

If there is a request to play a new audio content during the attending and listening state, process the request as follows:

* For playing audio content of **alert/dialogue/content** type, the audio content must be played after canceling the receiving of user voice input.
* For playing audio content of **notification** type or **feedback** type, the audio content must be played as the background audio.

## Sound effects {#SoundEffect}

A client must provide sound effects in addition to the [Lights](#Light) in order to convey the device status or feedback on a user request. This section explains the types of sound effects to be provided by the client to the user and the situations for providing it.

* [Types of sound effects](#SoundEffects)
* [Sound effect guidelines](#SoundEffectGuideline)

### Sound effect type {#SoundEffects}

In order to express [states and events](#ClientStateAndEvent) of the client, the following sound effects must be provided:

| State or event              | Sample sound effect                     | Required |
|---------------------------|------------------------------|:---------:|
| Entering the attending state         | <audio title="Attending" controls><source src="./Assets/Sounds/Clova-Client-Soundeffect-Attending.wav" type="audio/wav" /></audio> | Required     |
| Entering the error state             | <audio title="Error" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Error.wav" type="audio/wav" /></audio>         | Required     |
| Entering the mute on state           | <audio title="Mute on" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Mute_On.wav" type="audio/wav" /></audio>     | Required     |
| Disabling the mute on state           | <audio title="Mute off" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Mute_Off.wav" type="audio/wav" /></audio>   | Required     |
| Alarm (When an event occurs, repeat the sound effect.) | <audio title="Alarm" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Alarm.wav" type="audio/wav" /></audio>         | Required     |
| Reminder (When an event occurs, repeat the sound effect and the reminder details in TTS order.) | <audio title="Reminder" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Reminder.wav" type="audio/wav" /></audio>         | Required     |
| Timer (When an event occurs, repeat the sound effect.)      | <audio title="Timer" controls><source src="./Assets/Sounds/Clova-Client-SoundEffect-Timer.wav" type="audio/wav" /></audio>         | Required     |

### Sound effect guidelines {#SoundEffectGuideline}

The following guidelines must be followed when providing sound effects.

* It is recommended that the provided sound effects are used without any alteration.
* Other sound effects may be added depending on your UX policies or for appropriate situations.
* Users must be able to recognize each situation by the sound.
* The sound effects must be consistent with the light effects or screen states.
* If you are providing a sound effect for button feedback, the sound effect must be suitable to the property and feeling of a button.

## Supported audio compression formats {#SupportedAudioCompressionFormat}

Since a client must play the audio content delivered by Clova, it must be able to play the audio compression formats supported by Clova.

<!-- Start of the shared content: SupportedAudioFormat -->

Audio compression formats supported by Clova are as follows:

| Audio compression format                     | File extension | License cost |
|----------------------------------|:--------:|-----------|
| MPEG-1 or MPEG-2 Audio Layer III | .mp3     | Free       |

<div class="note">
  <p><strong>Note!</strong></p>
  <p>More audio compression formats may be supported by Clova in the future.</p>
</div>

<!-- End of the shared content -->
