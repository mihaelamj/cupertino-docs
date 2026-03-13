---
title: "Introduction"
book: "Objective-C Runtime Programming Guide"
chapterId: "TP40008048-CH1"
date: "2009-10-19"
description: "Describes the Objective-C 2.0 runtime support library."
identifier: "//apple_ref/doc/uid/TP40008048"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Introduction

> **Objective-C Runtime Programming Guide**
> Last updated: 2009-10-19

[Next](../Articles/ocrtVersionsPlatforms.html)
        
        
        [](../index.html)
        
        # Introduction

> **Important:** This document is no longer being updated. For the latest information about Apple SDKs, visit the [documentation website](https://developer.apple.com/documentation).

The Objective-C language defers as many decisions as it can from compile time and link time to runtime. Whenever possible, it does things dynamically. This means that the language requires not just a compiler, but also a runtime system to execute the compiled code. The runtime system acts as a kind of operating system for the Objective-C language; it’s what makes the language work.

This document looks at the `NSObject` class and how Objective-C programs interact with the runtime system. In particular, it examines the paradigms for dynamically loading new classes at runtime, and forwarding messages to other objects. It also provides information about how you can find information about objects while your program is running.

You should read this document to gain an understanding of how the Objective-C runtime system works and how you can take advantage of it. Typically, though, there should be little reason for you to need to know and understand this material to write a Cocoa application.

## Organization of This Document
This document has the following chapters:

[Runtime Versions and Platforms](../Articles/ocrtVersionsPlatforms.html#//apple_ref/doc/uid/TP40008048-CH106-SW1)

[Interacting with the Runtime](../Articles/ocrtInteracting.html#//apple_ref/doc/uid/TP40008048-CH103-SW1)

[Messaging](../Articles/ocrtHowMessagingWorks.html#//apple_ref/doc/uid/TP40008048-CH104-SW1)

[Dynamic Method Resolution](../Articles/ocrtDynamicResolution.html#//apple_ref/doc/uid/TP40008048-CH102-SW1)

[Message Forwarding](../Articles/ocrtForwarding.html#//apple_ref/doc/uid/TP40008048-CH105-SW1)

[Type Encodings](../Articles/ocrtTypeEncodings.html#//apple_ref/doc/uid/TP40008048-CH100-SW1)

[Declared Properties](../Articles/ocrtPropertyIntrospection.html#//apple_ref/doc/uid/TP40008048-CH101-SW1)

## See Also
*[Objective-C Runtime Reference](https://developer.apple.com/documentation/objectivec/objective_c_runtime)* describes the data structures and functions of the Objective-C runtime support library. Your programs can use these interfaces to interact with the Objective-C runtime system. For example, you can add classes or methods, or obtain a list of all class definitions for loaded classes.

*[Programming with Objective-C](../../ProgrammingWithObjectiveC/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011210)* describes the Objective-C language.

*[Objective-C Release Notes](../../../../../releasenotes/Cocoa/RN-ObjectiveC/index.html#//apple_ref/doc/uid/TP40004309)* describes some of the changes in the Objective-C runtime in recent releases of OS X.

        
            [Next](../Articles/ocrtVersionsPlatforms.html)
        
         Copyright &#x00a9; 2009 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2009-10-19