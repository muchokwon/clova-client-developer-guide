<!-- Note! This content includes shared parts. Therefore, when you update this file, you should beware of synchronization. -->

<!-- Start of the shared content: Glossary -->

# Terms and abbreviations

<div class="note">
  <p><strong>Note!</strong></p>
  <p>This page is updated on an ongoing basis.</p>
</div>

### CEK
The abbreviation of [Clova Extensions Kit](#CEK).

### CIC
The abbreviation of [Clova Interface Connect](#CIC).

### CIC API {#CICAPI}
The REST API that CIC provides to clients. Clients use CIC API to exchange information with Clova.

### Clova {#Clova}
[Clova](https://clova.ai) is an AI (artificial intelligence) platform developed and serviced by {{ book.ServiceEnv.OrientedService }}. Clova recognizes user speech or images, analyzes them, and provides information or services that users have requested. Third-party developers, by leveraging the Clova technologies, can make a device or home appliance that provides an AI service. They can also offer their content or services to users through Clova.

### Clova access token {#ClovaAccessToken}
A means used by Clova to authorize a client when the client tries to send [event messages](#Event) to [Clova Interface Connect](#CIC). For more information, see [Creating Clova access tokens](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken).

### Clova developer console {#ClovaDeveloperConsole}
A <a target="_blank" href="{{ book.ServiceEnv.DeveloperConsoleURI }}">[web tool]</a> that provides the following features to the [Clova extension](#ClovaExtension) developers or to client devices that interact with the Clova platform.
* Registering client device
* Providing client credentials

<div class="note">
  <p><strong>Note!</strong></p>
  <p>Clova developer console for Clova client is currently under development.</p>
</div>

### Clova extension {#ClovaExtension}
A web application that provides extended capabilities to Clova for a more diverse user experience, including the capability of controlling IoT appliances at home or third-party services such as music, shopping, and banking. It is commonly referred to as an "extension" and the Clova platform currently supports and provides two types of Clova extensions. To regular users, the extension is referred to as a "skill."
* [Custom extension](#CustomExtension)
* [Clova Home extension](#ClovaHomeExtension)

### Clova Extensions Kit (CEK) {#CEK}
A platform that provides tools and interfaces for development and deployment of a Clova extension. It supports communication between Clova and extensions.

### Clova Home extension {#ClovaHomeExtension}
An [extension](#ClovaExtension) that provides a service for controlling IoT appliances. For more information, see [Building a Clova Home extension]({{ book.DocMeta.ClovaHomeExtensionDeveloperGuideBaseURI }}/Develop/Guides/Build_Clova_Home_Extension.md).

### Clova Interface Connect (CIC) {#CIC}
A platform that serves as an interface between Clova and a client aiming to provide AI assistant services such as PC/mobile apps, mobile devices, or home appliances. For more information, see [CIC overview](/Develop/CIC_Overview.md).

### Clova app {#ClovaApp}

A Clova app developed by {{ book.ServiceEnv.OrientedService }} and deployed to the iOS or Android platform. These apps can send commands to Clova and also register and manage client devices.

### Clova auth API {#ClovaAuthAPI}
An API used by clients to obtain [Clova access tokens](#ClovaAccessToken). For more information, see [Clova auth API](/Develop/References/Clova_Auth_API.md).

### Content template {#ContentTemplate}
Standardized formats for displaying content returned from CIC. For more information, see [Content template](/Develop/References/Content_Templates.md).

### Context objects {#ContextObjects}
Objects that express the current [context information](#Context) of a client. For more information, see [Context](/Develop/References/Context_Objects.md).

### Custom extension {#CustomExtension}
An [extension](#ClovaExtension) that provides extended capabilities. A custom extension lets you provide third-party services such as music, shopping, or banking. For more information, see [Building a Clova custom extension]({{ book.DocMeta.ClovaCustomExtensionDeveloperGuideBaseURI }}/Develop/Guides/Build_Custom_Extension.md).

### Downchannel {#Downchannel}
An [HTTP/2](#HTTP2) stream through which clients receive directive messages from [CIC](#CIC). For more information, see [Connecting with CIC](/Develop/Guides/Interact_with_CIC.md#ConnectToCIC).

### Extension {#Extension}
Another name for a [Clova extension](#ClovaExtension)

### HTTP/2 {#HTTP2}
The second version of the HTTP protocol. HTTP/2 is based on [SPDY](https://en.wikipedia.org/wiki/SPDY) and developed by the IETF (Internet Engineering Task Force). HTTP/2 is the enhanced version of HTTP 1.1, which was standardized in RFC 2068 in 1997 and was presented as a proposed standard in December, 2014 and approved by IESG on February 17, 2015. HTTP/2 was released as [<a href="https://tools.ietf.org/html/rfc7540" target="_blank">RFC 7540</a>] in May 2015.

### OAuth 2.0
A public standard for delegation of access permission. The protocol allows internet users to grant account access right to other web services or applications. On the Clova platform, it is used for account linking when clients try to obtain [Clova access tokens](#ClovaAccessToken) or users try to use a certain extension. For more information, go to [https://tools.ietf.org/html/rfc6749](https://tools.ietf.org/html/rfc6749).

### Dialogue ID {#DialogID}
A dialogue ID is created every time a user initiates new speech input. It is included when clients send [Recognize](/Develop/References/MessageInterfaces/SpeechRecognizer.md#Recognize) [event messages](#Event) to [Clova Interface Connect](#CIC). Dialogue IDs are also included in [directive messages](#Directive) when responses are returned from a server side to map those responses to corresponding event messages. Clients check the dialogue ID included in a directive message and determine which directive message is responding to which event message. If the dialogue ID at the client side is different from that of the directive message, the directive message must be disregarded. For more information, see [Dialogue model](/Develop/Guides/Manage_Dialogue_ID_And_Handle_Tasks.md).

### Context {#Context}
Various states of a client. It is expressed with [context objects](#ContextObjects). For more information, see [Context](/Develop/References/Context_Objects.md).

### Message ID {#MessageID}
An identifier for distinguishing individual messages. All [event messages](#Event) and [directive messages](#Directive) have their own message IDs.

### Event message {#Event}
Messages that are sent from clients to [CIC](#CIC). Event messages are used to send user requests (speech input) or notify that a client state has changed.

### Directive message {#Directive}
Messages that are sent from [CIC](#CIC) to the client to control client actions. Directive messages are used when CIC returns responses to event messages from clients or when CIC sends information to clients under certain conditions.

### Client credentials {#ClientCredentialInfo}
Credentials obtained by registering a client in the [Clova developer console](#ClovaDeveloperConsole). Credentials are used to obtain [Clova access tokens](#ClovaAccessToken). For more information, see [Creating Clova access tokens](/Develop/Guides/Interact_with_CIC.md#CreateClovaAccessToken).

<!-- End of the shared content -->
