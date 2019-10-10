# Handling delegated user requests

A user can delegate another client device of the user, that is not the Clova app, to receive the handling results of requests made while using the Clova app. For example, when the user asks the Clova app to play a song of a specific artist, the user can also designate the song to be played on the Clova Friends speaker and not on the smartphone. We refer to this as the Clova app having delegated the user request to another client device.

Once the Clova app delegates handling of user request, the delegated client can receive directive messages through the downchannel at any time. Below is an explanation of the interaction structure for delegating a user request. **You must implement the action of the delegated client in the explanation for your client.**

![](/Develop/Assets/Images/CIC_Handle_Event_Delegation.svg)

1. The Clova app requests delegation to another client device when sending the user request to CIC.
2. CIC sends the [`Clova.HandleDelegatedEvent`](/Develop/References/MessageInterfaces/Clova.md#HandleDelegatedEvent) directive messages to the delegated client device to handle the request through a [downchannel](/Develop/Guides/Interact_with_CIC.md#CreateConnection).
  ```json
  {
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
  }
  ```
3. The client device must send the [`Clova.ProcessDelegatedEvent`](/Develop/References/MessageInterfaces/Clova.md#ProcessDelegatedEvent) event message in order to receive the handling result of the delegated request from CIC.<br />
  In this process, you must enter the `delegationId` field value received from step 2 in the `payload` field without any alteration.
  ```json
  {
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
  }
  ```
4. CIC returns the handling result of the delegated user request to the client as a response to the [`Clova.ProcessDelegatedEvent`](/Develop/References/MessageInterfaces/Clova.md#ProcessDelegatedEvent) event message.
5. The client can handle the directive message received as a response just like handling a normal [directive message](/Develop/Guides/Interact_with_CIC.md#HandleDirective).
