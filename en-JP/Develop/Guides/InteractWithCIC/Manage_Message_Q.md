## Managing message queues {#ManageMessageQ}

Clients constantly send and receive messages to and from CIC. Directives, the messages clients receive from CIC, have the following characteristics.

* Multiple directives and additional information can come within an HTTP response.
* The [dialogue IDs](/Develop/Guides/Implement_Client_Features.md#ManageDialogueIDAndHandleTasks) of received directives may differ.
* The order in which the message parts with additional information are received is not guaranteed.

Due to these characteristics, you must use a queue data structure to collect and remove directives in order. This is called a "message queue."

<div class="danger">
  <p><strong>Caution!</strong></p>
  <p>Directives that do not contain a dialogue ID must be handled immediately upon receipt.</p>
</div>

You must develop the code so that the directives piled into each message queue are handled one at a time. Here, the message responding to the latest request of the client user must be handled. The user request can be identified with a [dialogue ID](/Develop/Guides/Implement_Client_Features.md#ManageDialogueIDAndHandleTasks) and the client must save dialogue ID on the latest request for future use. If your message queue is keeping any directive with a dialogue ID of a canceled dialogue, remove the directive from the queue.

Determine the following as you wish or depending on the UX design.
* The number of message queues and each queue size
* Priority over message queues

For example, if a UX for performing seamless actions, even during user speech input or client speech output such as playing music ([AudioPlayer](/Develop/References/CICInterface/AudioPlayer.md)) is provided, it is necessary to handle the directive of the related message queue separately. However, there are a number of Clova services that send a single directive simultaneously. Using a message queue for such services may not be required.
