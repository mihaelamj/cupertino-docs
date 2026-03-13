---
title: "Runtime Versions and Platforms"
book: "Objective-C Runtime Programming Guide"
chapterId: "TP40008048-CH106"
date: "2009-10-19"
description: "Describes the Objective-C 2.0 runtime support library."
identifier: "//apple_ref/doc/uid/TP40008048"
source: apple-archive
---

# Runtime Versions and Platforms

> **Objective-C Runtime Programming Guide**
> Last updated: 2009-10-19

[Next](ocrtInteracting.html)[Previous](../Introduction/Introduction.html)
        
        
        [](../index.html)
        
        # Runtime Versions and Platforms
There are different versions of the Objective-C runtime on different platforms.

## Legacy and Modern Versions
There are two versions of the Objective-C runtime—“modern” and “legacy”. The modern version was introduced with Objective-C 2.0 and includes a number of new features. The programming interface for the legacy version of the runtime is described in *Objective-C 1 Runtime Reference*; the programming interface for the modern version of the runtime is described in *[Objective-C Runtime Reference](https://developer.apple.com/documentation/objectivec/objective_c_runtime)*.

The most notable new feature is that instance variables in the modern runtime are “non-fragile”: 

In the legacy runtime, if you change the layout of instance variables in a class, you must recompile classes that inherit from it.

In the modern runtime,  if you change the layout of instance variables in a class, you do not have to recompile classes that inherit from it.

In addition, the modern runtime supports instance variable synthesis for declared properties (see [Declared Properties](../../ObjectiveC/Chapters/ocProperties.html#//apple_ref/doc/uid/TP30001163-CH17) in *[The Objective-C Programming Language](../../ObjectiveC/Introduction/introObjectiveC.html#//apple_ref/doc/uid/TP30001163)*).

## Platforms
iPhone applications and 64-bit programs on OS X v10.5 and later use the modern version of the runtime.

Other programs (32-bit programs on OS X desktop) use the legacy version of the runtime.

        
            [Next](ocrtInteracting.html)[Previous](../Introduction/Introduction.html)
        
         Copyright &#x00a9; 2009 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2009-10-19