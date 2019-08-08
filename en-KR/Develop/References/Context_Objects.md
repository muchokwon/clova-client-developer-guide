# Context

A context is a state of a client and is specified in context objects. Context objects are included in the [events](/Develop/References/CIC_API.md#Event) sent to CIC. Context objects contain the device state at the time of a user voice request, and **must show all state information of the client that can be written as context.** The available context objects are as follows:

* [`Alerts.AlertsState`](#AlertsState)
* [`AudioPlayer.PlaybackState`](#PlaybackState)
* [`Clova.Location`](#Location)
* [`Clova.SavedPlace`](#SavedPlace)
* [`Device.DeviceState`](#DeviceState)
* [`Device.Display`](#Display)
* [`Speaker.VolumeState`](#VolumeState)
* [`SpeechSynthesizer.SpeechState`](#SpeechState)

{% include "/Develop/References/ContextObjects/AlertsState.md" %}

{% include "/Develop/References/ContextObjects/PlaybackState.md" %}

{% include "/Develop/References/ContextObjects/Location.md" %}

{% include "/Develop/References/ContextObjects/SavedPlace.md" %}

{% include "/Develop/References/ContextObjects/DeviceState.md" %}

{% include "/Develop/References/ContextObjects/Display.md" %}

{% include "/Develop/References/ContextObjects/VolumeState.md" %}

{% include "/Develop/References/ContextObjects/SpeechState.md" %}
