---
title: "Appendix D: Supported Audio File and Data Formats in OS X"
book: "Core Audio Overview"
chapterId: "TP40003577-CH7"
date: "2017-10-30"
description: "Provides an overview of Core Audio and its programming interfaces. "
identifier: "//apple_ref/doc/uid/TP40003577"
source: apple-archive
---

# Appendix D: Supported Audio File and Data Formats in OS X

> **Core Audio Overview**
> Last updated: 2017-10-30

[Next](../RevisionHistory.html)[Previous](../SystemAudioUnits/SystemAudioUnits.html)
        
        
        [](../index.html)
        
        # Supported Audio File and Data Formats in OS X
This appendix describes the audio data and file formats supported in Core Audio in OS X v10.5.

Each audio file type lists the data formats supported for that type. That is, a converter exists to convert data from the particular file format to any of the listed data formats. Some data formats (such as AC3) cannot be converted to a linear PCM format and therefore cannot be handled by standard audio units. 

A Core Audio Format (CAF) file can contain audio data of any format. Any application that supports the CAF file format can write audio data to the file or extract the data it contains. However, the ability to encode or decode the audio data contained within it is dependent on the audio codecs available on the system. 

**Table D-1**   Allowable data formats for each file format. File Format

Data Formats

AAC (`.aac`, `.adts`)

`'aac '`

AC3 (`.ac3`)

`'ac-3'` 

AIFC (`.aif`, `.aiff`,`.aifc`)

`BEI8`,  `BEI16`,  `BEI24`,  `BEI32`,  `BEF32`,  `BEF64`,  `'ulaw'`,  `'alaw'`,  `'MAC3'`,  `'MAC6'`,  `'ima4' `,  `'QDMC'`,  `'QDM2'`,  `'Qclp'`,  `'agsm'`

AIFF (`.aiff`)

`BEI8`,  `BEI16`,  `BEI24`,  `BEI32`

Apple Core Audio Format (`.caf`)

`'.mp3'`, `'MAC3'`, `'MAC6'`, `'QDM2'`, `'QDMC'`,  `'Qclp'`, `'Qclq'`, `'aac '`, `'agsm'`, `'alac'`, `'alaw'`, `'drms'`, `'dvi '`, `'ima4'`, `'lpc '`, `BEI8`,  `BEI16`,  `BEI24`,  `BEI32`,  `BEF32`,  `BEF64`,   `LEI16`,  `LEI24`,  `LEI32`,  `LEF32`,  `LEF64`, `'ms\x00\x02'`, `'ms\x00\x11'`, `'ms\x001'`, `'ms\x00U'`, `'ms \x00'`,  `'samr'`, `'ulaw'`  

MPEG Layer 3 (`.mp3`)

`'.mp3'`

MPEG 4 Audio (`.mp4`)

`'aac '`

MPEG 4 Audio (`.m4a`)

`'aac '`, ` alac'`

NeXT/Sun Audio (`.snd`, `.au`)

`BEI8`,  `BEI16`,  `BEI24`,  `BEI32`,  `BEF32`,  `BEF64`, `'ulaw'`

Sound Designer II (`.sd2`)

`BEI8`,  `BEI16`,  `BEI24`,  `BEI32`

WAVE (`.wav`)

`LEUI8`,  `LEI16`,  `LEI24`,  `LEI32`,  `LEF32`,  `LEF64`, `'ulaw'`, `'alaw'`

Key for linear PCM formats. For example, BEF32 = Big Endian linear PCM 32 bit floating point. 

**Table D-2**  Key for linear PCM formats`LE`

Little Endian

`BE`

Big Endian

`F`

Floating point

`I`

Integer

`UI`

Unsigned integer

`8/16/24/32/64`

Number of bits

Core Audio includes a number of audio codecs that translate audio data to and from Linear PCM. Codecs for the following audio data type are available in OS X v10.4. Audio applications may install additional encoders and decoders. 

Audio data type

Encode from linear PCM?

Decode to linear PCM?

MPEG Layer 3 (`'.mp3'`)

No

Yes

MACE 3:1 (`'MAC3'`)

Yes

Yes

MACE 6:1 (`'MAC6'`)

Yes

Yes

QDesign Music 2 (`'QDM2'`)

Yes

Yes

QDesign (`'QDMC'`)

No

Yes

Qualcomm PureVoice (`'Qclp'`)

Yes

Yes

Qualcomm QCELP (`'qclq'`)

No

Yes

AAC (`'aac ')`

Yes

Yes

Apple Lossless (`'alac'`)

Yes

Yes

Apple GSM 10:1 (`'agsm'`)

No

Yes

ALaw 2:1 `'alaw'`)

Yes

Yes

Apple DRM Audio Decoder (`'drms'`)

No

Yes

AC-3

No

No

DVI 4:1 (`'dvi '`)

No

Yes

Apple IMA 4:1 (`'ima4'`)

Yes

Yes

LPC 23:1 (`'lpc '`)

No

Yes

Microsoft ADPCM

No

Yes

DVI ADPCM

Yes

Yes

GSM610

No

Yes

AMR Narrowband (`'samr'`)

Yes

Yes

µLaw 2:1 (`'ulaw'`)

Yes

Yes

        
            [Next](../RevisionHistory.html)[Previous](../SystemAudioUnits/SystemAudioUnits.html)
        
         Copyright &#x00a9; 2017 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2017-10-30