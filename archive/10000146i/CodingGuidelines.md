---
title: "Introduction"
book: "Coding Guidelines for Cocoa"
chapterId: "10000146"
date: "2013-10-22"
description: "Provides naming guidelines for Cocoa API and design advice to framework developers."
identifier: "//apple_ref/doc/uid/10000146i"
platforms: "OS X,iOS,watchOS"
source: apple-archive
---

# Introduction

> **Coding Guidelines for Cocoa**
> Last updated: 2013-10-22

[Next](Articles/NamingBasics.html)
        
        
        [](index.html)
        
        # Introduction to Coding Guidelines for Cocoa

Developing a [Cocoa](../../../General/Conceptual/DevPedia-CocoaCore/Cocoa.html#//apple_ref/doc/uid/TP40008195-CH9) [framework](../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56), plug-in, or other executable with a public API requires some approaches and conventions that are different from those used in application development. The primary clients of your product are developers, and it is important that they are not mystified by your programmatic interface. This is where API naming conventions come in handy, for they help you to make your interfaces consistent and clear. There are also programming techniques that are special to—or of greater importance with—frameworks, such as versioning, binary compatibility, error-handling, and [memory management](../../../General/Conceptual/DevPedia-CocoaCore/MemoryManagement.html#//apple_ref/doc/uid/TP40008195-CH27). This topic includes information on both Cocoa naming conventions and recommended programming practices for frameworks.

## Organization of This Document

The articles contained in this topic fall into two general types. The first and larger group presents naming [conventions](../../../General/Conceptual/DevPedia-CocoaCore/CodingConventions.html#//apple_ref/doc/uid/TP40008195-CH53) for programmatic interfaces. These are the same conventions (with some minor exceptions) that Apple uses for its own Cocoa frameworks. These articles on naming conventions include the following:

[Code Naming Basics](Articles/NamingBasics.html#//apple_ref/doc/uid/20001281-BBCHBFAH)

[Naming Methods](Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF)

[Naming Functions](Articles/NamingFunctions.html#//apple_ref/doc/uid/20001283-BAJGGCAD)

[Naming Properties and Data Types](Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE)

[Acceptable Abbreviations and Acronyms](Articles/APIAbbreviations.html#//apple_ref/doc/uid/20001285-BCIHCGAE)

The second group (currently with a membership of one) discusses aspects of framework programming:

[Tips and Techniques for Framework Developers](Articles/FrameworkImpl.html#//apple_ref/doc/uid/20001286-BAJIBFHD)

        
            [Next](Articles/NamingBasics.html)
        
         Copyright &#x00a9; 2003, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-10-22