---
title: "Introduction"
book: "Audio Unit Hosting Guide for iOS"
framework: "CoreAudio"
chapterId: "TP40009492-CH1"
date: "2010-09-01"
description: "Explains how to use system-supplied audio units."
identifier: "//apple_ref/doc/uid/TP40009492"
platforms: "iOS,tvOS"
source: apple-archive
---

# Introduction

> **Audio Unit Hosting Guide for iOS**
> Last updated: 2010-09-01

[Next](../AudioUnitHostingFundamentals/AudioUnitHostingFundamentals.html)
        
        
        [](../index.html)
        
        # About Audio Unit Hosting

> **Important:** This document is no longer being updated. For the latest information about Apple SDKs, visit the [documentation website](https://developer.apple.com/documentation).

iOS provides audio processing plug-ins that support mixing, equalization, format conversion, and realtime input/output for recording, playback, offline rendering, and live conversation such as for VoIP (Voice over Internet Protocol). You can dynamically load and use—that is, *host*—these powerful and flexible plug-ins, known as *audio units*, from your iOS application.

Audio units usually do their work in the context of an enclosing object called an *audio processing graph*, as shown in the figure. In this example, your app sends audio to the first audio units in the graph by way of one or more callback functions and exercises individual control over each audio unit. The output of the I/O unit—the last audio unit in this or any audio processing graph—connects directly to the output hardware.

## At a Glance
Because audio units constitute the lowest programming layer in the iOS audio stack, to use them effectively requires deeper understanding than you need for other iOS audio technologies. Unless you require realtime playback of synthesized sounds, low-latency I/O (input and output), or specific audio unit features, look first at the Media Player, AV Foundation, OpenAL, or Audio Toolbox frameworks. These higher-level technologies employ audio units on your behalf and provide important additional features, as described in *[Multimedia Programming Guide](../../../../AudioVideo/Conceptual/MultimediaPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009767)*.

### Audio Units Provide Fast, Modular Audio Processing
The two greatest advantages of using audio units directly are:

Excellent responsiveness. Because you have access to a realtime priority thread in an audio unit render callback function, your audio code is as close as possible to the metal. Synthetic musical instruments and realtime simultaneous voice I/O benefit the most from using audio units directly.

Dynamic reconfiguration. The audio processing graph API, built around the `[AUGraph](https://developer.apple.com/documentation/audiotoolbox/augraph)` opaque type, lets you dynamically assemble, reconfigure, and rearrange complex audio processing chains in a thread-safe manner, all while processing audio. This is the only audio API in iOS offering this capability.

An audio unit’s life cycle proceeds as follows:

At runtime, obtain a reference to the dynamically-linkable library that defines an audio unit you want to use.

Instantiate the audio unit.

Configure the audio unit as required for its type and to accomodate the intent of your app.

Initialize the audio unit to prepare it to handle audio.

Start audio flow.

Control the audio unit.

When finished, deallocate the audio unit.

Audio units provide highly useful individual features such as stereo panning, mixing, volume control, and audio level metering. Hosting audio units lets you add such features to your app. To reap these benefits, however, you must gain facility with a set of fundamental concepts including audio data stream formats, render callback functions, and audio unit architecture.

> **Relevant Chapter:**  [Audio Unit Hosting Fundamentals](../AudioUnitHostingFundamentals/AudioUnitHostingFundamentals.html#//apple_ref/doc/uid/TP40009492-CH3-SW11)

### Choosing a Design Pattern and Constructing Your App

An audio unit hosting design pattern provides a flexible blueprint to customize for the specifics of your app. Each pattern indicates:

How to configure the I/O unit. I/O units have two independent elements, one that accepts audio from the input hardware, one that sends audio to the output hardware. Each design pattern indicates which element or elements you should enable.

Where, within the audio processing graph, you must specify audio data stream formats. You must correctly specify formats to support audio flow.

Where to establish audio unit connections and where to attach your render callback functions. An audio unit connection is a formal construct that propagates a stream format from an output of one audio unit to an input of another audio unit. A render callback lets you feed audio into a graph or manipulate audio at the individual sample level within a graph.

No matter which design pattern you choose, the steps for constructing an audio unit hosting app are basically the same:

Configure your application audio session to ensure your app works correctly in the context of the system and device hardware.

Construct an audio processing graph. This multistep process makes use of everything you learned in [Audio Unit Hosting Fundamentals](../AudioUnitHostingFundamentals/AudioUnitHostingFundamentals.html#//apple_ref/doc/uid/TP40009492-CH3-SW11).

Provide a user interface for controlling the graph’s audio units.

Become familiar with these steps so you can apply them to your own projects.

> **Relevant Chapter:** [Constructing Audio Unit Apps](../ConstructingAudioUnitApps/ConstructingAudioUnitApps.html#//apple_ref/doc/uid/TP40009492-CH16-SW1)

### Get the Most Out of Each Audio Unit
Most of this document teaches you that all iOS audio units share important, common attributes. These attributes include, for example, the need for your app to specify and load the audio unit at runtime, and then to correctly specify its audio stream formats.

At the same time, each audio unit has certain unique features and requirements, ranging from the correct audio sample data type to use, to required configuration for correct behavior. Understand the usage details and specific capabilities of each audio unit so you know, for example, when to use the 3D Mixer unit and when to instead use the Multichannel Mixer.

> **Relevant Chapter:** [Using Specific Audio Units](../UsingSpecificAudioUnits/UsingSpecificAudioUnits.html#//apple_ref/doc/uid/TP40009492-CH17-SW1)

 
 
 ## How to Use This Document

If you prefer to begin with a hands-on introduction to audio unit hosting in iOS, download one of the sample apps available in the iOS Dev Center, such as *Audio Mixer (MixerHost)*. Come back to this document to answer questions you may have and to learn more.

If you want a solid conceptual grounding before starting your project, read [Audio Unit Hosting Fundamentals](../AudioUnitHostingFundamentals/AudioUnitHostingFundamentals.html#//apple_ref/doc/uid/TP40009492-CH3-SW11) first. This chapter explains the concepts behind the APIs. Continue with [Constructing Audio Unit Apps](../ConstructingAudioUnitApps/ConstructingAudioUnitApps.html#//apple_ref/doc/uid/TP40009492-CH16-SW1) to learn about picking a design pattern for your project and the workflow for building your app.

If you have some experience with audio units and just want the specifics for a given type, you can start with [Using Specific Audio Units](../UsingSpecificAudioUnits/UsingSpecificAudioUnits.html#//apple_ref/doc/uid/TP40009492-CH17-SW1).

## Prerequisites
Before reading this document, it’s a good idea to read the section [A Little About Digital Audio and Linear PCM](../../CoreAudioOverview/WhatisCoreAudio/WhatisCoreAudio.html#//apple_ref/doc/uid/TP40003577-CH3-SW11) in *[Core Audio Overview](../../CoreAudioOverview/Introduction/Introduction.html#//apple_ref/doc/uid/TP40003577)*. Also, review *[Core Audio Glossary](../../../Reference/CoreAudioGlossary/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004453)* for terms you may not already be familiar with. To check if your audio needs might be met by a higher-level technology, review [Using Audio](../../../../AudioVideo/Conceptual/MultimediaPG/UsingAudio/UsingAudio.html#//apple_ref/doc/uid/TP40009767-CH2) in *[Multimedia Programming Guide](../../../../AudioVideo/Conceptual/MultimediaPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009767)*.

## See Also
Essential reference documentation for building an audio unit hosting app includes the following:

*[Audio Unit Properties Reference](https://developer.apple.com/documentation/audiounit/audio_unit_properties)* describes the properties you can use to configure each type of audio unit.

*[Audio Unit Parameters Reference](https://developer.apple.com/documentation/audiounit/audio_unit_parameters)* describes the parameters you can use to control each type of audio unit.

*[Audio Unit Component Services Reference](https://developer.apple.com/documentation/audiounit/audio_unit_component_services)* describes the API for accessing audio unit parameters and properties, and describes the various audio unit callback functions.

*[Audio Component Services Reference](https://developer.apple.com/documentation/audiounit/audio_component_services)* describes the API for accessing audio units at runtime and for managing audio unit instances.

*[Audio Unit Processing Graph Services Reference](https://developer.apple.com/documentation/audiotoolbox/audio_unit_processing_graph_services)* describes the API for constructing and manipulating audio processing graphs, which are dynamically reconfigurable audio processing chains.

*[Core Audio Data Types Reference](https://developer.apple.com/documentation/coreaudio/core_audio_data_types)* describes the data structures and types you need for hosting audio units.

        
            [Next](../AudioUnitHostingFundamentals/AudioUnitHostingFundamentals.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-09-01