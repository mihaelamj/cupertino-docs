---
title: "Using Specific Audio Units"
book: "Audio Unit Hosting Guide for iOS"
chapterId: "TP40009492-CH17"
date: "2010-09-01"
description: "Explains how to use system-supplied audio units."
identifier: "//apple_ref/doc/uid/TP40009492"
source: apple-archive
---

# Using Specific Audio Units

> **Audio Unit Hosting Guide for iOS**
> Last updated: 2010-09-01

[Next](../RevisionHistory.html)[Previous](../ConstructingAudioUnitApps/ConstructingAudioUnitApps.html)
        
        
        [](../index.html)
        
        # Using Specific Audio Units
Each iOS audio unit has certain things in common with all others and certain things unique to itself. Earlier chapters in this document described the common aspects, among them the need to find the audio unit at runtime, instantiate it, and ensure that its stream formats are set appropriately. This chapter explains the differences among the audio units and provides specifics on how to use them.

Later in the chapter, [Identifier Keys for Audio Units](#//apple_ref/doc/uid/TP40009492-CH17-SW14) lists the codes you need to locate the dynamically-linkable libraries for each audio unit at runtime.

## Using I/O Units

iOS provides three I/O (input/output) units. The vast majority of audio-unit applications use the Remote I/O unit, which connects to input and output audio hardware and provides low-latency access to individual incoming and outgoing audio sample values. For VoIP apps, the Voice-Processing I/O unit extends the Remote I/O unit by adding acoustic echo cancelation and other features. To send audio back to your application rather than to output audio hardware, use the Generic Output unit.

### Remote I/O Unit
The Remote I/O unit (subtype `[kAudioUnitSubType_RemoteIO](https://developer.apple.com/documentation/audiotoolbox/1619485-anonymous/kaudiounitsubtype_remoteio)`) connects to device hardware for input, output, or simultaneous input and output. Use it for playback, recording, or low-latency simultaneous input and output where echo cancelation is not needed.

The device’s audio hardware imposes its audio stream formats on the outward-facing sides of the Remote I/O unit, as described in [Understanding Where and How to Set Stream Formats](../AudioUnitHostingFundamentals/AudioUnitHostingFundamentals.html#//apple_ref/doc/uid/TP40009492-CH3-SW34). The audio unit provides format conversion between the hardware audio formats and your application audio format, doing so by way of an included Format Converter audio unit.

For sample code that shows how to use this audio unit, see the sample code project *[aurioTouch](../../../../../samplecode/aurioTouch/Introduction/Intro.html#//apple_ref/doc/uid/DTS40007770)*.

Table 3-1 provides usage details for this audio unit.

**Table 3-1**  Using the Remote I/O unitAudio unit feature

Details

Elements

One input element: element 1. One output element: element 0.

By default, the input element is disabled and the output element  is enabled. If you need to change this, refer to the description of the `[kAudioOutputUnitProperty_EnableIO](https://developer.apple.com/documentation/audiotoolbox/1534116-i_o_audio_unit_properties/kaudiooutputunitproperty_enableio)` property.

Recommended stream format attributes

`[kAudioFormatLinearPCM](https://developer.apple.com/documentation/coreaudio/1572096-audio_data_format_identifiers/kaudioformatlinearpcm)`

`[AudioUnitSampleType](https://developer.apple.com/documentation/coreaudio/audiounitsampletype)`

`[kAudioFormatFlagsAudioUnitCanonical](https://developer.apple.com/documentation/coreaudio/kaudioformatflagsaudiounitcanonical)`

Stream format notes

The outward-facing sides of the Remote I/O unit acquire their formats from the audio hardware as follows:

The input element (element 1) input scope gets its stream format from the currently-active audio input hardware.

The output element (element 0) output scope gets its stream format from the currently-active output audio hardware.

Set your application format on the output scope of the input element. The input element performs format conversion between its input and output scopes as needed. Use the hardware sample rate for your application stream format.

If the input scope of the output element is fed by an audio unit connection, it acquires its stream format from that connection. If, however, it is fed by a render callback function, set your application format on it.

Parameters

None in iOS.

Properties

See `I/O Audio Unit Properties`.

Property notes

You never need to set the `[kAudioUnitProperty_MaximumFramesPerSlice](https://developer.apple.com/documentation/audiotoolbox/1534199-generic_audio_unit_properties/kaudiounitproperty_maximumframesperslice)` property on this audio unit.

### Voice-Processing I/O Unit
The Voice-Processing I/O unit (subtype `[kAudioUnitSubType_VoiceProcessingIO](https://developer.apple.com/documentation/audiotoolbox/1584139-input_output_audio_unit_subtypes/kaudiounitsubtype_voiceprocessingio)`) has the characteristics of the Remote I/O unit and adds echo suppression for two-way duplex communication. It also adds automatic gain correction, adjustment of voice-processing quality, and muting. This is the correct I/O unit to use for VoIP (Voice over Internet Protocol) apps.

All of the considerations listed in [Table 3-1](#//apple_ref/doc/uid/TP40009492-CH17-SW10) apply as well to the Voice-Processing I/O unit. In addition, there are specific properties available for this audio unit, described in `Voice-Processing I/O Audio Unit Properties`.

### Generic Output Unit
Use this audio unit, of subtype `[kAudioUnitSubType_GenericOutput](https://developer.apple.com/documentation/audiotoolbox/1584139-input_output_audio_unit_subtypes/kaudiounitsubtype_genericoutput)`, when sending the output of an audio processing graph to your application rather than to the output audio hardware. You would typically use the Generic Output unit for offline audio processing. Just like the other I/O units, the Generic Output unit incorporates a Format Converter unit. This lets the Generic Output unit perform format conversion between the stream format used in an audio processing graph and the format you want.

You can also use a Generic Output unit as the final node in a subgraph that you place into a parent audio processing graph.

## Using Mixer Units
iOS provides two mixer units. In most cases, you should use the Multichannel Mixer unit, which provides mixing for any number of mono or stereo streams. If you need the features of the 3D Mixer unit, you should very likely be using OpenAL instead. OpenAL is built on top of the 3D Mixer unit, providing equivalent performance with a simpler API that is well suited for game app development.

### Multichannel Mixer Unit
The Multichannel Mixer unit (subtype `[kAudioUnitSubType_MultiChannelMixer](https://developer.apple.com/documentation/audiotoolbox/kaudiounitsubtype_multichannelmixer)`) takes any number of mono or stereo streams and combines them into a single stereo output. It controls audio gain for each input and for the output, and lets you turn each input on or off separately. Starting in iOS 4.0, the Multichannel Mixer supports stereo panning for each input. 

For sample code that shows how to use this audio unit, see the sample code project *Audio Mixer (MixerHost)*.

Table 3-2 provides usage details for this audio unit.

**Table 3-2**  Using the Multichannel Mixer unitAudio unit feature

Details

Elements

One or more input elements, each of which can be mono or stereo. One stereo output element.

Recommended stream format attributes

`[kAudioFormatLinearPCM](https://developer.apple.com/documentation/coreaudio/1572096-audio_data_format_identifiers/kaudioformatlinearpcm)`

`[AudioUnitSampleType](https://developer.apple.com/documentation/coreaudio/audiounitsampletype)`

`[kAudioFormatFlagsAudioUnitCanonical](https://developer.apple.com/documentation/coreaudio/kaudioformatflagsaudiounitcanonical)`

Stream format notes

On the input scope, manage stream formats as follows:

If an input bus is fed by an audio unit connection, it acquires its stream format from that connection.

If an input bus is fed by a render callback function, set your complete application stream format on the bus. Use the same stream format as used for the data provided by the callback.

On the output scope, set just the application sample rate.

Parameters

See `Multichannel Mixer Unit Parameters`.

Properties

`[kAudioUnitProperty_MeteringMode](https://developer.apple.com/documentation/audiotoolbox/1534041-mixer_audio_unit_properties/kaudiounitproperty_meteringmode)`.

Property notes

By default, the `[kAudioUnitProperty_MaximumFramesPerSlice](https://developer.apple.com/documentation/audiotoolbox/1534199-generic_audio_unit_properties/kaudiounitproperty_maximumframesperslice)` property is set to a value of 1024, which is not sufficient when the screen locks and the display sleeps. If your app plays audio with the screen locked, you must increase the value of this property unless audio input is active. Do as follows:

If audio input is active, you do not need to set a value for the `kAudioUnitProperty_MaximumFramesPerSlice` property.

If audio input is not active, set this property to a value of 4096.

### 3D Mixer Unit
The 3D Mixer unit (subtype `[kAudioUnitSubType_3DMixer](https://developer.apple.com/documentation/audiotoolbox/kaudiounitsubtype_3dmixer)`) controls stereo panning, playback tempo, and gain for each input, and controls other characteristics such as apparent distance to the listener. The output has an audio gain control. To get some idea of what this audio unit can do, consider that OpenAL in iOS is implemented using it.

In most cases, if you need the features of the 3D Mixer unit, your best option is to use OpenAL. For sample code that shows how to use OpenAL, see the sample code project *oalTouch*.

Table 3-3 provides usage details for this audio unit.

**Table 3-3**  Using the 3D Mixer unitAudio unit feature

Details

Elements

One or more input elements, each of which is mono. One stereo output element.

Recommended stream format attributes

`UInt16`

`[kAudioFormatFlagsCanonical](https://developer.apple.com/documentation/coreaudio/kaudioformatflagscanonical)`

Stream format notes

On the input scope, manage stream formats as follows:

If an input bus is fed by an audio unit connection, it acquires its stream format from that connection.

If an input bus is fed by a render callback function, set your complete application stream format on the bus. Use the same stream format as used for the data provided by the callback.

On the output scope, set just the application sample rate.

Parameters

See `3D Mixer Unit Parameters`.

Properties

See `3D Mixer Audio Unit Properties`. Note, however, that most of these properties are implemented only in the Mac OS X version of this audio unit.

Property notes

By default, the `[kAudioUnitProperty_MaximumFramesPerSlice](https://developer.apple.com/documentation/audiotoolbox/1534199-generic_audio_unit_properties/kaudiounitproperty_maximumframesperslice)` property is set to a value of 1024, which is not sufficient when the screen locks and the display sleeps. If your app plays audio with the screen locked, you must increase the value of this property unless audio input is active. Do as follows:

If audio input is active, you do not need to set a value for the `kAudioUnitProperty_MaximumFramesPerSlice` property.

If audio input is not active, set this property to a value of 4096.

## Using Effect Units
The iPod EQ unit (subtype `[kAudioUnitSubType_AUiPodEQ](https://developer.apple.com/documentation/audiotoolbox/kaudiounitsubtype_auipodeq)`) is the only effect unit provided in iOS 4. This is the same equalizer used by the built-in iPod app. To view the iPod app’s user interface for this audio unit, go to Settings > iPod > EQ. This audio unit offers a set of preset equalization curves such as Bass Booster, Pop, and Spoken Word.

You must supply your own user interface to the iPod EQ unit, as you must for any of the audio units. The *[Mixer iPodEQ AUGraph Test](../../../../../samplecode/iPhoneMixerEQGraphTest/Introduction/Intro.html#//apple_ref/doc/uid/DTS40009555)* sample code project demonstrates how to use the iPod EQ unit and shows one way to provide a user interface for it.

Table 3-4 provides usage details for this audio unit.

**Table 3-4**  Using the iPod EQ unitAudio unit feature

Details

Elements

One mono or stereo input element. One mono or stereo output element.

Recommended stream format attributes

`[kAudioFormatLinearPCM](https://developer.apple.com/documentation/coreaudio/1572096-audio_data_format_identifiers/kaudioformatlinearpcm)`

`[AudioUnitSampleType](https://developer.apple.com/documentation/coreaudio/audiounitsampletype)`

`[kAudioFormatFlagsAudioUnitCanonical](https://developer.apple.com/documentation/coreaudio/kaudioformatflagsaudiounitcanonical)`

Stream format notes

On the input scope, manage stream formats as follows:

If the input is fed by an audio unit connection, it acquires its stream format from that connection.

If the input is fed by a render callback function, set your complete application stream format on the bus. Use the same stream format as used for the data provided by the callback.

On the output scope, set the same full stream format that you used for the input.

Parameters

None.

Properties

 `[kAudioUnitProperty_FactoryPresets](https://developer.apple.com/documentation/audiotoolbox/1534199-generic_audio_unit_properties/kaudiounitproperty_factorypresets)` and `[kAudioUnitProperty_PresentPreset](https://developer.apple.com/documentation/audiotoolbox/kaudiounitproperty_presentpreset)`

Property notes

The iPod EQ unit provides a set of predefined tonal equalization curves as factory presets. Obtain the array of available EQ settings by accessing the audio unit’s `[kAudioUnitProperty_FactoryPresets](https://developer.apple.com/documentation/audiotoolbox/1534199-generic_audio_unit_properties/kaudiounitproperty_factorypresets)` property. You can then apply a setting by using it as the value for the `[kAudioUnitProperty_PresentPreset](https://developer.apple.com/documentation/audiotoolbox/kaudiounitproperty_presentpreset)` property.

By default, the `[kAudioUnitProperty_MaximumFramesPerSlice](https://developer.apple.com/documentation/audiotoolbox/1534199-generic_audio_unit_properties/kaudiounitproperty_maximumframesperslice)` property is set to a value of 1024, which is not sufficient when the screen locks and the display sleeps. If your app plays audio with the screen locked, you must increase the value of this property unless audio input is active. Do as follows:

If audio input is active, you do not need to set a value for the `kAudioUnitProperty_MaximumFramesPerSlice` property.

If audio input is not active, set this property to a value of 4096.

## Identifier Keys for Audio Units
This table provides the identifier keys you need to access the dynamically-linkable libraries for each iOS audio unit, along with brief descriptions of the audio units.

**Table 3-5**  Identifier keys for accessing the dynamically-linkable libraries for each iOS audio unitName and description

Identifier keys

Corresponding four-char codes

*Converter unit*

Supports audio format conversions to or from linear PCM.

`kAudioUnitType_FormatConverter`

`kAudioUnitSubType_AUConverter`

`kAudioUnitManufacturer_Apple`

`aufc`

`conv`

`appl`

*iPod Equalizer unit*

Provides the features of the iPod equalizer.

`kAudioUnitType_Effect`

`kAudioUnitSubType_AUiPodEQ`

`kAudioUnitManufacturer_Apple`

`aufx`

`ipeq`

`appl`

*3D Mixer unit*

Supports mixing multiple audio streams, output panning, sample rate conversion, and more.

`kAudioUnitType_Mixer`

`kAudioUnitSubType_AU3DMixerEmbedded`

`kAudioUnitManufacturer_Apple`

`aumx`

`3dem`

`appl`

*Multichannel Mixer unit*

Supports mixing multiple audio streams to a single stream.

`kAudioUnitType_Mixer`

`kAudioUnitSubType_MultiChannelMixer`

`kAudioUnitManufacturer_Apple`

`aumx`

`mcmx`

`appl`

*Generic Output unit*

Supports converting to and from linear PCM format; can be used to start and stop a graph.

`kAudioUnitType_Output`

`kAudioUnitSubType_GenericOutput`

`kAudioUnitManufacturer_Apple`

`auou`

`genr`

`appl`

*Remote I/O unit*

Connects to device hardware for input, output, or simultaneous input and output.

`kAudioUnitType_Output`

`kAudioUnitSubType_RemoteIO`

`kAudioUnitManufacturer_Apple`

`auou`

`rioc`

`appl`

*Voice Processing I/O unit*

Has the characteristics of the I/O unit and adds echo suppression for two-way communication.

`kAudioUnitType_Output`

`kAudioUnitSubType_VoiceProcessingIO`

`kAudioUnitManufacturer_Apple`

`auou`

`vpio`

`appl`

        
            [Next](../RevisionHistory.html)[Previous](../ConstructingAudioUnitApps/ConstructingAudioUnitApps.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-09-01