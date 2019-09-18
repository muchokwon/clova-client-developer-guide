# CIC overview
Learn what Clova Interface Connect (hereinafter referred to as "CIC") is and how it works. This document will help you understand how CIC operates and includes CIC guides and references.

## What is CIC? {#WhatisCIC}
CIC is a platform that serves as an interface between Clova and a client providing AI assistant services on computers, mobile apps, mobile devices, or home appliances. CIC provides the [CIC API](/Develop/References/CIC_API.md), with which clients can send user requests to Clova and Clova can return responses to clients.

![](/Develop/Assets/Images/CIC_Interaction_Structure.png)

## CIC interaction structure {#CICInteractionStructure}
Clients use the CIC API to send user requests to CIC and also to receive responses from CIC. All communications with CIC is made over the [HTTP/2 protocol](https://tools.ietf.org/html/rfc7540). The [CIC API](/Develop/References/CIC_API.md) provides interfaces for speech recognition, playing synthesized speech and music, calendar management, setting alarms/timers, and many other features.

Through the CIC API, various communications are made between clients and CIC in the form of messages. The message type depends on who the message sender is as follows.

* [Event](/Develop/References/CIC_API.md#Event): Messages that clients send to CIC. Send events to CIC to pass user voice requests (speech input) to CIC and to notify CIC when your client states have changed.

* [Directive](/Develop/References/CIC_API.md#Directive): Messages that CIC sends to clients to control client behavior. For example, a directive can instruct an app to display certain information or play synthesized speech. Directives are sent to clients when:
    * Responding to user requests. After recognizing user voice request (user intent), CIC returns directives to instruct clients to perform certain actions relevant to the user intent.
    * Certain conditions require CIC to send directives to clients without any preceding user requests.

The following sequence diagram shows how messages are sent back and forth between CIC and a client.

![](/Develop/Assets/Images/CIC_Interaction_Example_in_Sequence_Diagram.svg)


* [Dialogue ID and client behavior](#DialogIDandClientOP)

### Indirect dialogue structure {#IndirectDialogue}
Users can have a series of dialogues with Clova. In general, users make requests to Clova to get information or to perform certain actions. Clova returns requested information or performs the requested action. Such dialogues between users and Clova are relayed by clients and CIC.

![](/Develop/Assets/Images/CIC_Structure_Of_Indirect_Dialogue.png)

Dialogues between users and Clova usually proceed as follows:

1. A user starts to speak to Clova.
2. The client records the user speech and sends the recording to CIC.
3. CIC receives the user request from the client and sends the result of Clova handling the request back to the client.
4. CIC returns result to the client. The client delivers the result to the user through synthesized speech or display.

Having CIC and a client in between, dialogues between users and Clova are indirect and have the following limitations:

* Delivering requests and receiving responses take longer than direct dialogues.
* Response is not immediate when new requests are made or new dialogues are initiated by users.

Suppose that a user asks Clova, "How's the weather today?" and then immediately proceeds to make the request, "Play some upbeat music." before Clova responds or while Clova is in the process of responding to the previous request. In this case, the user probably no longer wants a response to "How's the weather today?"

Had the dialogue been direct, you would simply ignore the weather information returned. However, since dialogues are relayed by a client to Clova, the client must recognize the dialogue status and take appropriate actions to provide what user wants. For this, the client must use a dialogue ID when [creating dialogue IDs and handling tasks](/Develop/Guides/Manage_Dialogue_ID_And_Handle_Tasks.md).
