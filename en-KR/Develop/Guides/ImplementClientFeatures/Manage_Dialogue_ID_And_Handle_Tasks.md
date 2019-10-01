# Handling tasks and managing dialogue IDs

To solve the issues with the [indirect dialogue structure](/Develop/CIC_Overview.md#IndirectDialogue), the concept of **dialogue ID** is used. The client must perform the following processes regarding dialogue IDs:

* [Creating a dialogue ID](#CreatingDialogueID)
* [Handling directive messages by dialogue ID](#HandleDirectivesByDialogueID)

## Creating a dialogue ID {#CreatingDialogueID}

To identify individual user requests, a **dialogue ID** is created every time a user starts speaking to Clova. The client must remember the dialogue ID of the last user request sent to CIC. The client must also call this the **latest dialogue ID**. **Latest dialogue ID** refers to the dialogue ID contained in the [SpeechRecognizer.Recognize](/Develop/References/CICInterface/SpeechRecognizer.md#Recognize) event message or the [TextRecognizer.Recognize](/Develop/References/CICInterface/TextRecognizer.md#Recognize) event message and is the last one sent from the client to CIC. The client must save this last dialogue ID for future use and must renew it every time a user request is sent to CIC.

The following actions are required by the client regarding the dialogue IDs:

![](/Develop/Assets/Images/CIC_Dialogue_ID_Creation.svg)

<ol>
  <li><strong>Create a new dialogue ID</strong> (UUID format recommended) every time a user initiates a dialogue.</li>
  <li>Send the user request to CIC using the <a href="/Develop/References/CICInterface/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a> event message. (For text requests, use the <a href="/Develop/References/CICInterface/TextRecognizer.md#Recognize">TextRecognizer.Recognize</a> event message.)
    <ul>
      <li>Here, include the newly created dialogue ID in <code>dialogRequestId</code> of the <a href="/Develop/References/CIC_API.md#Event">event message header</a>.</li>
    </ul>
  </li>
  <li>After sending the event message, the created dialogue ID must be saved as the <strong>latest dialogue ID</strong>.</li>
</ol>

<div class="note">
<p><strong>Note!</strong></p>
<p>The latest dialogue ID <strong> must be updated after the process of the <a href="/Develop/References/CICInterface/SpeechRecognizer.md#Recognize">SpeechRecognizer.Recognize</a> event message or the <a href="/Develop/References/CICInterface/TextRecognizer.md#Recognize">TextRecognizer.Recognize</a> event message </strong> is complete. The client will not be able to respond to the sudden change of request by the user if it does not remember or renew the latest dialogue ID. For more information, see <a href="/Develop/CIC_Overview.md#IndirectDialogue">Indirect dialogue structure</a>.</p>
</div>

Once the latest dialogue ID is updated, the client must perform the following in order to [handle the directive messages containing the dialogue ID](#HandleDirectivesByDialogueID):

* If the details containing old dialogue IDs are provided to the user, this must be stopped by referring to the [Rules for basic audio playback](/Design/Design_Guideline_For_Client_Hardware.md#AudioInterruptionRule) or [Audio playback rules for user utterances](/Design/Design_Guideline_For_Client_Hardware.md#AudioInterruptionRuleForUserUtterance).
* All directive messages containing old dialogue IDs must be discarded from the [message queue](/Develop/Guides/Interact_with_CIC.md#ManageMessageQ).

## Handling directive messages by dialogue ID {#HandleDirectivesByDialogueID}

In general, CIC sends a directive message to the client as a response to the user request and embeds the [dialogue ID created by the client](#CreatingDialogueID) in the directive message. In short, dialogue IDs help you identify whether the response from Clova corresponds to the latest user request. The client must handle the received directive messages according to the dialogue ID, as shown below.

![](/Develop/Assets/Images/CIC_Handle_Directives_By_Dialogue_ID.svg)

First, the client must check whether the directive messages received from CIC contains a dialogue ID in the [header of the directive message](/Develop/References/CIC_API.md#Directive). If the received directive message contains a dialogue ID, compare the ID to the **latest dialogue ID** and handle as follows, depending on the result:

* **If the dialogue IDs match**, add the received message to the [message queue](/Develop/Guides/Interact_with_CIC.md#ManageMessageQ) and provide details to the user in order.
* **If the dialogue IDs do not match**, discard the received directive. message

If **the directive does not contain a dialogue ID**, the client must **immediately** handle the received directive. Directives without dialogue IDs are usually sent via the [downchannel](/Develop/References/CIC_API.md#EstablishDownchannel) and require a background service operation which does not interfere with the dialogue.
