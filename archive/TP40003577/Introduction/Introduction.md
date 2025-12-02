---
title: "Introduction"
book: "Core Audio Overview"
framework: "CoreAudio"
chapterId: "TP40003577-CH1"
date: "2017-10-30"
description: "Provides an overview of Core Audio and its programming interfaces. "
identifier: "//apple_ref/doc/uid/TP40003577"
platforms: "OS X,iOS"
source: apple-archive
---

# Introduction

> **Core Audio Overview**
> Last updated: 2017-10-30

[Next](../WhatisCoreAudio/WhatisCoreAudio.html)
        
        
        [](../index.html)
        
        # Introduction
Core Audio provides software interfaces for implementing audio features in applications you create for iOS and OS X. Under the hood, it handles all aspects of audio on each of these platforms. In iOS, Core Audio capabilities include recording, playback, sound effects, positioning, format conversion, and file stream parsing, as well as:

A built-in equalizer and mixer that you can use in your applications

Automatic access to audio input and output hardware

APIs to let you manage the audio aspects of your application in the context of a device that can take phone calls

Optimizations to extend battery life without impacting audio quality

On the Mac, Core Audio encompasses recording, editing, playback, compression and decompression, MIDI, signal processing, file stream parsing, and audio synthesis. You can use it to write standalone applications or modular effects and codec plug-ins that work with existing products.

Core Audio combines C and Objective-C programming interfaces with tight system integration, resulting in a flexible programming environment that maintains low latency through the signal chain.

> **Note:** Core Audio does not provide direct support for audio digital rights management (DRM). If you need DRM support for audio files, you must implement it yourself.

*Core Audio Overview* is for all developers interested in creating audio software. Before reading this document you should have basic knowledge of general audio, digital audio, and MIDI terminology. You will also do well to have some familiarity with object-oriented programming concepts and with Apple’s development environment, Xcode. If you are developing for iOS-based devices, you should be familiar with Cocoa Touch development as introduced in *[Start Developing iOS Apps Today (Retired)](../../../../../referencelibrary/GettingStarted/RoadMapiOS-Legacy/index.html#//apple_ref/doc/uid/TP40011343)*.

## Organization of This Document
This document is organized into the following chapters:

[What Is Core Audio?](../WhatisCoreAudio/WhatisCoreAudio.html#//apple_ref/doc/uid/TP40003577-CH3-SW1) describes the features of Core Audio and what you can use it for.

[Core Audio Essentials](../CoreAudioEssentials/CoreAudioEssentials.html#//apple_ref/doc/uid/TP40003577-CH10-SW1) describes the architecture of Core Audio, introduces you to its programming patterns and idioms, and shows you the basics of how to use it in your applications.

[Common Tasks in OS X](../ARoadmaptoCommonTasks/ARoadmaptoCommonTasks.html#//apple_ref/doc/uid/TP40003577-CH6-SW1) outlines how you can use Core Audio to accomplish several audio tasks in OS X.

This document also contains four appendixes:

[Core Audio Frameworks](../CoreAudioFrameworks/CoreAudioFrameworks.html#//apple_ref/doc/uid/TP40003577-CH9-SW1) lists the frameworks and headers that define Core Audio.

[Core Audio Services](../WhatsinCoreAudio/WhatsinCoreAudio.html#//apple_ref/doc/uid/TP40003577-CH4-SW4) provides an alternate view of Core Audio, listing the services available in iOS, OS X, and on both platforms.

[System-Supplied Audio Units in OS X](../SystemAudioUnits/SystemAudioUnits.html#//apple_ref/doc/uid/TP40003577-CH8-SW2) lists the audio units that ship in OS X v10.5.

[Supported Audio File and Data Formats in OS X](../SupportedAudioFormatsMacOSX/SupportedAudioFormatsMacOSX.html#//apple_ref/doc/uid/TP40003577-CH7-SW1) lists the audio file and data formats that Core Audio supports in OS X v10.5.

## See Also
For more detailed information about audio and Core Audio, see the following resources:

*[AVAudioPlayer Class Reference](https://developer.apple.com/documentation/avfoundation/avaudioplayer)*, which describes a simple Objective-C interface for audio playback in iOS applications.

*[Audio Session Programming Guide](../../../../Audio/Conceptual/AudioSessionProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007875)*, which explains how to specify important aspects of audio behavior for iOS applications.

*[Audio Queue Services Programming Guide](../../AudioQueueProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40005343)*, which explains how to implement recording and playback in your application.

*[Core Audio Data Types Reference](https://developer.apple.com/documentation/coreaudio/core_audio_data_types)*, which describes the data types used throughout Core Audio.

*[Audio File Stream Services Reference](https://developer.apple.com/documentation/audiotoolbox/audio_file_stream_services)*, which describes the interfaces you use for working with streamed audio.

*[Audio Unit Programming Guide](../../AudioUnitProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40003278)*, which contains detailed information about creating audio units for OS X.

*[Core Audio Glossary](../../../Reference/CoreAudioGlossary/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004453)*, which defines terms used throughout the Core Audio documentation suite.

*[Apple Core Audio Format Specification 1.0](../../../Reference/CAFSpec/CAF_intro/CAF_intro.html#//apple_ref/doc/uid/TP40001862)*, which describes Apple’s universal audio container format, the Core Audio File (CAF) format.

The Core Audio mailing list: [http://lists.apple.com/mailman/listinfo/coreaudio-api](http://lists.apple.com/mailman/listinfo/coreaudio-api)

The OS X audio developer site: [http://developer.apple.com/audio/](http://developer.apple.com/audio/)

The Core Audio SDK (software development kit), available at  [http://developer.apple.com/sdk/](http://developer.apple.com/sdk/)

        
            [Next](../WhatisCoreAudio/WhatisCoreAudio.html)
        
         Copyright &#x00a9; 2017 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2017-10-30