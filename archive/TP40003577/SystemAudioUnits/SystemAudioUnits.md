---
title: "Appendix C: System-Supplied Audio Units in OS X"
book: "Core Audio Overview"
chapterId: "TP40003577-CH8"
date: "2017-10-30"
description: "Provides an overview of Core Audio and its programming interfaces. "
identifier: "//apple_ref/doc/uid/TP40003577"
source: apple-archive
---

# Appendix C: System-Supplied Audio Units in OS X

> **Core Audio Overview**
> Last updated: 2017-10-30

[Next](../SupportedAudioFormatsMacOSX/SupportedAudioFormatsMacOSX.html)[Previous](../WhatsinCoreAudio/WhatsinCoreAudio.html)
        
        
        [](../index.html)
        
        # System-Supplied Audio Units in OS X
The tables in this appendix list the audio units that ship with OS X v10.5, grouped by Component Manager type. The Component Manager manufacturer identifier for all these units is `kAudioUnitManufacturer_Apple`. 

**Table C-1**  System-supplied effect units (`kAudioUnitType_Effect`)Effect Units

Subtype

Description

AUBandpass

`kAudioUnitSubType_BandPassFilter`

A single-band bandpass filter.

AUDynamicsProcessor

`kAudioUnitSubType_DynamicsProcessor`

A dynamics processor that lets you set parameters such as headroom, the amount of compression, attack and release times, and so on. 

AUDelay

`kAudioUnitSubType_Delay`

A delay unit.

AUFilter

`kAudioUnitSubType_AUFilter`

A five-band filter, allowing for low and high frequency cutoffs as well as three bandpass filters. 

AUGraphicEQ

`kAudioUnitSubType_GraphicEQ`

A 10-band or 31-band graphic equalizer.

AUHiPass

`kAudioUnitSubType_HighPassFilter`

A high-pass filter with an adjustable resonance peak. 

AUHighShelfFilter

`kAudioUnitSubType_HighShelfFilter`

A filter that allows you to boost or cut high frequencies by a fixed amount. 

AUPeakLimiter

`kAudioUnitSubType_PeakLimiter`

A peak limiter.

AULowPass

`kAudioUnitSubType_LowPassFilter`

A low-pass filter with an adjustable resonance peak. 

AULowShelfFilter

`kAudioUnitSubType_LowShelfFilter`

A filter that allows you to boost or cut low frequencies by a fixed amount. 

AUMultibandCompressor

`kAudioUnitSubType_MultiBandCompressor`

A four-band compressor. 

AUMatrixReverb 

`kAudioUnitSubType_MatrixReverb`

A reverberation unit that allows you to specify spatial characteristics, such as size of room, material absorption characteristics, and so on.

AUNetSend

`kAudioUnitSubType_NetSend`

A unit that streams audio data over a network. Used in conjunction with the AUNetReceive generator audio unit. 

AUParametricEQ

`kAudioUnitSubType_ParametricEQ`

A parametric equalizer.

AUSampleDelay

`kAudioUnitSubType_SampleDelay`

A delay unit that allows you to set the delay by number of samples rather than by time.

AUPitch

`kAudioUnitSubType_Pitch`

An effect unit that lets you alter the pitch of the sound without changing the speed of playback.

**Table C-2**  System-supplied instrument unit (`kAudioUnitType_MusicDevice`)Instrument Unit

Subtype

Description

DLSMusicDevice

`kAudioUnitSubType_DLSSynth`

A virtual instrument unit that lets you play MIDI data using sound banks in the SoundFont or Downloadable Sounds (DLS) format. Sound banks must be stored in the `/Library/Audio/Sounds/Banks` folder of either your home or system directory. 

**Table C-3**  System-supplied mixer units (`kAudioUnitType_Mixer`)Mixer Units

Subtype

Description 

AUMixer3D

`kAudioUnitSubType_3DMixer`

A special mixing unit that can take several different signals and mix them so they appear to be positioned in a three-dimensional space. For details on using this unit, see [Technical Note TN2112: Using the 3DMixer Audio Unit](http://developer.apple.com/technotes/tn2004/tn2112.html).  

AUMatrixMixer

`kAudioUnitSubType_MatrixMixer`

A unit that mixes an arbitrary number of inputs to an arbitrary number of outputs.  

AUMixer

`kAudioUnitSubType_StereoMixer`

A unit that mixes an arbitrary number of mono or stereo inputs to a single stereo output. 

**Table C-4**  System-supplied converter units (`kAudioUnitType_FormatConverter`)Converter Unit

Subtype

Description

AUConverter

`kAudioUnitSubType_AUConverter`

A generic converter to handle data conversions within the linear PCM format. That is, it can handle sample rate conversions, integer to floating point conversions (and vice versa), interleaving, and so on. This audio unit is essentially a wrapper around an audio converter.

AUDeferredRenderer

`kAudioUnitSubType_DeferredRenderer`

An audio unit that obtains its input from one thread and sends its output to another; you can use this unit to divide your audio processing chain among multiple threads.  

AUMerger

`kAudioUnitSubType_Merger`

An unit that combines two separate audio inputs. 

AUSplitter

`kAudioUnitSubType_Splitter`

A unit that splits an audio input into two separate audio outputs. 

AUTimePitch

`kAudioUnitSubType_TimePitch`

A unit that lets you change the speed of playback without altering the pitch, or vice versa. 

AUVarispeed

`kAudioUnitSubType_Varispeed`

A unit that lets you change the speed of playback (and consequently the pitch as well). 

**Table C-5**  System-supplied output units (`kAudioUnitType_Output`)Output Unit

Subtype

Description

AudioDeviceOutput

`kAudioUnitSubType_HALOutput`

A unit that interfaces with an audio device using the hardware abstraction layer. Also called the AUHAL. Despite its name, the AudioDeviceOutput unit can also be configured to accept device input.  See [Interfacing with Hardware](../ARoadmaptoCommonTasks/ARoadmaptoCommonTasks.html#//apple_ref/doc/uid/TP40003577-CH6-SW12) for more details.  

DefaultOutputUnit

`kAudioUnitSubType_DefaultOutput`

An output unit that sends its input data to the user-designated default output (such as the computer's speaker). 

GenericOutput

`kAudioUnitSubType_GenericOutput`

A generic output unit that contains the signal format control and conversion features of an output unit, but doesn't interface with an output device.  Typically used for the output of an audio processing subgraph. See [Audio Processing Graphs](../CoreAudioEssentials/CoreAudioEssentials.html#//apple_ref/doc/uid/TP40003577-CH10-SW28).

SystemOutputUnit

`kAudioUnitSubType_SystemOutput`

An output unit that sends its input data to the standard system output. System output is the output designated for system sounds and effects, which the user can set in the Sound Effects tab of the Sound preference panel. 

**Table C-6**  System-supplied generator units (`kAudioUnitType_Generator)`Generator Unit

Subtype

Description

AUAudioFilePlayer

`kAudioUnitSubType_AudioFilePlayer`

A unit that obtains and plays audio data from a file. 

AUNetReceive

`kAudioUnitSubType_NetReceive`

A unit that receives streamed audio data from a network. Used in conjunction with the AUNetSend audio unit. 

AUScheduledSoundPlayer

`kAudioUnitSubType_ScheduledSoundPlayer`

A unit that plays audio data from one or more buffers in memory. 

        
            [Next](../SupportedAudioFormatsMacOSX/SupportedAudioFormatsMacOSX.html)[Previous](../WhatsinCoreAudio/WhatsinCoreAudio.html)
        
         Copyright &#x00a9; 2017 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2017-10-30