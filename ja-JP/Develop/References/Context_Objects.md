# コンテキスト

コンテキストは、クライアントのさまざまなステータスを示します。コンテキストオブジェクトで表され、CIC APIの[イベント](/Develop/References/CIC_API.md#Event)を送信する際に含まれます。コンテキストは、ユーザーが発話するタイミングのクライアントの状態を持っている必要があります。**作成できるクライアントのすべての状態をコンテキストとして表す必要があります。**コンテキストオブジェクトは、次のようなものがあります。

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
