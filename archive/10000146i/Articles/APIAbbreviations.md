---
title: "Acceptable Abbreviations and Acronyms"
book: "Coding Guidelines for Cocoa"
chapterId: "20001285"
date: "2013-10-22"
description: "Provides naming guidelines for Cocoa API and design advice to framework developers."
identifier: "//apple_ref/doc/uid/10000146i"
source: apple-archive
---

# Acceptable Abbreviations and Acronyms

> **Coding Guidelines for Cocoa**
> Last updated: 2013-10-22

[Next](FrameworkImpl.html)[Previous](NamingIvarsAndTypes.html)
        
        
        [](../index.html)
        
        # Acceptable Abbreviations and Acronyms

In general, you shouldn’t abbreviate names when you design your programmatic interface (see [General Principles](NamingBasics.html#//apple_ref/doc/uid/20001281-1001751)). However, the abbreviations listed below are either well established or have been used in the past, and so you may continue to use them. There are a couple of additional things to note about abbreviations: 

Abbreviations that duplicate forms long used in the standard C library—for example, “`alloc`” and “`getc`”—are permitted.

You may use abbreviations more freely in argument names (for example, “`imageRep`”, “`col`” (for “column”), “`obj`”, and “`otherWin`”).

Abbreviation

Meaning and comments

alloc

Allocate.

alt

Alternate.

app

Application. For example, NSApp the global application object. However, “application” is spelled out in [delegate](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14) methods, notifications, and so on.

calc

Calculate.

dealloc

Deallocate.

func

Function.

horiz

Horizontal.

info

Information.

init

Initialize (for methods that [initialize](../../../../General/Conceptual/DevPedia-CocoaCore/Initialization.html#//apple_ref/doc/uid/TP40008195-CH21) new objects).

int

Integer (in the context of a C `int`—for an `[NSInteger](https://developer.apple.com/documentation/objectivec/nsinteger)` value, use `integer`).

max

Maximum.

min

Minimum.

msg

[Message](../../../../General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59).

nib

Interface Builder archive.

pboard

Pasteboard (but only in constants).

rect

Rectangle.

Rep

Representation (used in class name such as `NSBitmapImageRep`).

temp

Temporary.

vert

Vertical.

You may use abbreviations and acronyms that are common in the computer industry in place of the words they represent. Here are some of the better-known acronyms:

ASCII

PDF

XML

HTML

URL

RTF

HTTP

TIFF

JPG

PNG

GIF

LZW

ROM

RGB

CMYK

MIDI

FTP

        
            [Next](FrameworkImpl.html)[Previous](NamingIvarsAndTypes.html)
        
         Copyright &#x00a9; 2003, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-10-22