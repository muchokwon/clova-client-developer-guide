## Handling delegated user requests {#HandleDelegation}

A user can delegate another client device of the user, that is not the Clova app, to receive the handling results of requests made while using the Clova app. For example, when the user asks the Clova app to play a song of a specific artist, the user can also designate the song to be played on the Clova Friends speaker and not on the smartphone. We refer to this as the Clova app having delegated the user request to another client device.

Once the Clova app delegates handling of user request, the delegated client can receive directives through the downchannel at any time. Below is an explanation of the interaction structure for delegating a user request. **You must implement the action of the delegated client in the explanation for your client.**

![](/Develop/Assets/Images/CIC_Handle_Event_Delegation.svg)

<ol>
  <li>The Clova app requests delegation to another client device when sending the user request to CIC.</li>
  <li>
    <p>CIC sends the <a href="/Develop/References/CICInterface/Clova.html#HandleDelegatedEvent"><code>Clova.HandleDelegatedEvent</code></a> directive to the delegated client device to handle the request through a <a href="/Develop/Guides/Interact_with_CIC.md#CreateConnection">downchannel</a>.<p>
    <pre><code>{
  "directive": {
    "header": {
      "messageId": "b17df741-2b5b-4db4-a608-85ecb1307b33",
      "namespace": "Clova",
      "name": "HandleDelegatedEvent"
    },
    "payload": {
      "delegationId": "99e86204-710a-4e94-b949-a763e78348a7"
    }
  }
}</code></pre>
  </li>
  <li>
    <p>The client device must send the <a href="/Develop/References/CICInterface/Clova.html#ProcessDelegatedEvent"><code>Clova.ProcessDelegatedEvent</code></a> event in order to receive the handling result of the delegated request from CIC. In this process, you must use the <code>delegationId</code> field value received from step 2 in the <code>payload</code> field without any alteration.</p>
    <pre><code>{
  "context": [
    ...
  ],
  "event": {
    "header": {
      "namespace": "Clova",
      "name": "ProcessDelegatedEvent",
      "messageId": "b120c3e0-e6b9-4a3d-96de-71539e5f6214"
    },
    "payload": {
      "delegationId": "99e86204-710a-4e94-b949-a763e78348a7"
    }
  }
}</code></pre>
  </li>
  <li>CIC returns the handling result of the delegated user request to the client as a response to the <a href="/Develop/References/CICInterface/Clova.html#ProcessDelegatedEvent"><code>Clova.ProcessDelegatedEvent</code></a> event.</li>
  <li>The client can handle the directive received as a response just like handling a normal <a href="/Develop/Guides/Interact_with_CIC.html#HandleDirective">directive</a>.</li>
</ol>
