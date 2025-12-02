---
title: "Appendix A: Core Audio Frameworks"
book: "Core Audio Overview"
framework: "CoreAudio"
chapterId: "TP40003577-CH9"
date: "2017-10-30"
description: "Provides an overview of Core Audio and its programming interfaces. "
identifier: "//apple_ref/doc/uid/TP40003577"
source: apple-archive
---

# Appendix A: Core Audio Frameworks

> **Core Audio Overview**
> Last updated: 2017-10-30

[Next](../WhatsinCoreAudio/WhatsinCoreAudio.html)[Previous](../ARoadmaptoCommonTasks/ARoadmaptoCommonTasks.html)
        
        
        [](../index.html)
        
        # Core Audio Frameworks
Core Audio consists of a number of separate frameworks, which you can find in `/System/Library/Frameworks`. These frameworks are not grouped under an umbrella framework, so finding particular headers can sometimes be tricky. This appendix describes each of the Core Audio frameworks and their associated header files. 

## Frameworks Available in iOS and OS X
The frameworks listed in this section are available in iOS 2.0 and OS X v10.5.

### AudioToolbox.framework
The Audio Toolbox framework contains the APIs that provide application-level services. The Audio Toolbox framework includes these header files:

`AudioConverter.h`: Audio Converter API. Defines the interface used to create and use audio converters. 

`AudioFile.h`: Defines an interface for reading and writing audio data in files.

`AudioFileStream.h`: Defines an interface for parsing audio file streams.

`AudioFormat.h`: Defines the interface used to assign and read audio format metadata in audio files. 

`AudioQueue.h`: Defines an interface for playing and recording audio.

`AudioServices.h`: Defines three interfaces. System Sound Services lets you play short sounds and alerts. Audio Hardware Services provides a lightweight interface for interacting with audio hardware. Audio Session Services lets iPhone and iPod touch applications manage audio sessions.

`AudioToolbox.h`: Top-level include file for the Audio Toolbox framework. 

`AUGraph.h`: Defines the interface used to create and use audio processing graphs.

`ExtendedAudioFile.h`: Defines the interface used to translate audio data from files directly into linear PCM, and vice versa. 

In OS X you have these additional header files:

`AudioFileComponents.h`: Defines the interface for Audio File Component Manager components. You use an audio file component to implement reading and writing a custom file format. 

`AudioUnitUtilities.h`: Utility functions for interacting with audio units. Includes audio unit parameter conversion functions, and audio unit event functions to create listener objects, which invoke a callback when specified audio unit parameters have changed.  

`CAFFile.h`: Defines the Core Audio Format audio file format. See *[Apple Core Audio Format Specification 1.0](../../../Reference/CAFSpec/CAF_intro/CAF_intro.html#//apple_ref/doc/uid/TP40001862)* for more information.

`CoreAudioClock.h`: Lets you designate a timing source for synchronizing applications or devices.

`MusicPlayer.h`: Defines the interface used to manage and play event tracks in music sequences. 

`AUMIDIController.h`: Deprecated: Do not use. An interface to allow audio units to receive data from a designated MIDI source. Standard MIDI messages are translated into audio unit parameter values. This interface is superseded by functions in the Music Player API. 

`DefaultAudioOutput.h`: Deprecated: Do not use. Defines an older interface for accessing the default output unit (deprecated in OS X v10.3 and later).

### AudioUnit.framework
The Audio Unit framework contains the APIs used for managing plug-ins in Core Audio. Except as noted, the Audio Unit framework includes these header files:

`AUComponent.h`: Defines the audio unit types.

`AudioComponent.h`: (iOS only) Defines the interface for using audio components.

`AudioOutputUnit.h`: Defines the interface used to turn an output unit on or off.  

`AudioUnit.h`: Include file for the Audio Unit framework.

`AudioUnitParameters.h`: Predefined parameter constants used by Apple's audio units. Third parties can also use these constants for their own audio units.  

`AudioUnitProperties.h`: Predefined audio unit properties for common audio unit types as well as Apple's audio units. 

In OS X you have these additional header files:

`AUCocoaUIView.h`: Defines the protocol for a custom Cocoa view you can use to hold your audio unit's user interface. See also `CoreAudioKit.framework/AUGenericView.h`.

`AudioCodec.h`: Defines the interface used specifically for creating audio codec components..

`AudioUnitCarbonView.h`: Defines the interface for loading and interacting with a Carbon-based audio unit user interface. A Carbon interface is packaged as a Component Manager component and appears as an HIView.

`AUNTComponent.h`: Deprecated: Do not use. Defines the interface for older "v1" audio units. Deprecated in OS X v10.3 and later. Replaced by `AUComponent.h`.

`LogicAUProperties.h`: An interface for audio units that run in the Logic Node environment of the Logic Studio application. 

`MusicDevice.h`: An interface for creating instrument units (that is, software-based music synthesizers). 

### CoreAudio.framework
The Core Audio framework contains data types common to all Core Audio services, as well as lower-level APIs used to interact with hardware. In OS X, this framework contains the interfaces for Hardware Abstraction Layer (HAL) Services.

This framework includes this header file:

`CoreAudioTypes.h`: Defines data types used by all of Core Audio. 

In OS X you have these additional header files:

`AudioDriverPlugin.h`: Defines the interface used to communicate with an audio driver plug-in. 

`AudioHardware.h`: Defines the interface for interacting with audio device objects. An audio device object represents an external device in the hardware abstraction layer (HAL). 

`AudioHardwarePlugin.h`: Defines the CFPlugin interface required for a HAL plug-in. An instance of a plug-in appears as an audio device object in the HAL. 

`CoreAudio.h`: Top-level include file for the Core Audio framework. 

`HostTime.h`: Contains functions to obtain and convert the host's time base. 

### OpenAL.framework
The OpenAL framework provides an implementation of the the OpenAL specification. This framework includes these two header files:

al.h

alc.h

In iOS you have these additional header files:

`oalMacOSX_OALExtensions.h`

`oalStaticBufferExtension.h`

In OS X you have this additional header file:

`MacOSX_OALExtensions.h`

## Frameworks Available in iOS Only
The frameworks listed in this section are available in iOS only.

### AVFoundation.framework
The AVFoundation framework provides an Objective-C interface for playing back audio with the control needed by most applications. 

## Frameworks Available in OS X Only
The frameworks listed in this section are available in OS X only.

### CoreAudioKit.framework
The Core Audio Kit framework contains APIs used for creating a Cocoa user interface for an audio unit. 

`CoreAudioKit.h`: Top-level include file for the Core Audio Kit framework. 

`AUGenericView.h`: Defines a generic Cocoa view class for use with audio units.  This is the bare-bones user interface that is displayed if an audio unit doesn't create its own custom interface. 

`AUPannerView.h`: Defines and instantiates a generic view for use with panner audio units.

### CoreMIDI.framework
The Core MIDI framework contains all Core MIDI Services APIs used to implement MIDI support in applications.  

`CoreMIDI.h`: Top-level include file for the Core MIDI framework. 

`MIDIServices.h`: Defines the interface used to set up and configure an application to communicate with MIDI devices (through MIDI endpoints, notifications, and so on). 

`MIDISetup.h`: Defines the interface used to configure or customize the global state of the MIDI system (that is, available MIDI devices, MIDI endpoints, and so on). 

`MIDIThruConnection.h`: Defines functions to create MIDI play-through connections between MIDI sources and destinations. A MIDI thru connection allows you to daisy-chain MIDI devices, allowing input to one device to pass through to another device as well. 

### CoreMIDIServer.framework
The Core MIDI Server framework contains interfaces for MIDI drivers. 

`CoreMIDIServer.h`: Top-level include file for the Core MIDI Server framework. 

`MIDIDriver.h`: Defines the CFPlugin interface used by MIDI drivers to interact with the Core MIDI Server. 

        
            [Next](../WhatsinCoreAudio/WhatsinCoreAudio.html)[Previous](../ARoadmaptoCommonTasks/ARoadmaptoCommonTasks.html)
        
         Copyright &#x00a9; 2017 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2017-10-30