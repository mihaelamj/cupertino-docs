---
title: "Introduction"
book: "Property List Programming Guide"
chapterId: "10000048"
date: "2010-03-24"
description: "Explains how to use structured, textual representations of data in Cocoa."
identifier: "//apple_ref/doc/uid/10000048i"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Introduction

> **Property List Programming Guide**
> Last updated: 2010-03-24

[Next](../QuickStartPlist/QuickStartPlist.html)
        
        
        [](../index.html)
        
        # Introduction to Property Lists

> **Important:** This document is no longer being updated. For the latest information about Apple SDKs, visit the [documentation website](https://developer.apple.com/documentation).

Property lists organize data into named values and lists of values using several object types. These types give you the means to produce data that is meaningfully structured, transportable, storable, and accessible, but still as efficient as possible. Property lists are frequently used by applications running on both OS X and iOS. The property-list programming interfaces for Cocoa and Core Foundation allow you to convert hierarchically structured combinations of these basic types of objects to and from standard XML. You can save the XML data to disk and later use it to reconstruct the original objects.

This document describes property lists and their various representations, and how to work with them using both certain Foundation classes of Cocoa and Property List Services of Core Foundation.

> **Note:** The user defaults system, which you programmatically access through the `[NSUserDefaults](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSUserDefaults/Description.html#//apple_ref/occ/cl/NSUserDefaults)` class, uses property lists to store objects representing user preferences. This limitation would seem to exclude many kinds of objects, such as `[NSColor](https://developer.apple.com/documentation/appkit/nscolor)` and `[NSFont](https://developer.apple.com/documentation/appkit/nsfont)` objects, from the user default system. But if objects conform to the `[NSCoding](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intf/NSCoding)` protocol they can be archived to `[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)` objects, which are property list–compatible objects. For information on how to do this, see “[Storing NSColor in User Defaults](../../DrawColor/Tasks/StoringNSColorInDefaults.html#//apple_ref/doc/uid/20001693)“; although this article focuses on `NSColor` objects, the procedure can be applied to any object that can be archived. 

## Organization of This Document
This document consists of the following chapters:

[Quick Start for Property Lists](../QuickStartPlist/QuickStartPlist.html#//apple_ref/doc/uid/10000048i-CH4-SW5) is a mini-tutorial on property lists, giving you a “hands-on” familiarity with XML property lists and the Objective-C serialization API.

[About Property Lists](../AboutPropertyLists/AboutPropertyLists.html#//apple_ref/doc/uid/10000048i-CH3-SW2) explains what property lists are and when you should use them.

[Creating Property Lists Programmatically](../CreatePropListProgram/CreatePropListProgram.html#//apple_ref/doc/uid/10000048i-CH5-SW1) shows how you can create hierarchically structured property lists using the APIs of Cocoa and Core Foundation.

[Understanding XML Property Lists](../UnderstandXMLPlist/UnderstandXMLPlist.html#//apple_ref/doc/uid/10000048i-CH6-SW1) describes the format of XML property lists.

[Serializing a Property List ](../SerializePlist/SerializePlist.html#//apple_ref/doc/uid/10000048i-CH7-SW1) discusses how to serialize and deserialize property lists between their runtime and static representations).

[Reading and Writing Property-List Data](../ReadWritePlistData/ReadWritePlistData.html#//apple_ref/doc/uid/10000048i-CH8-SW1) describes how to save property lists to files or URL resources and how to restore them later.

[Old-Style ASCII Property Lists](../OldStylePlists/OldStylePLists.html#//apple_ref/doc/uid/20001012-BBCBDBJE) is an appendix that describes the format of old-style (OpenStep) ASCII property lists.

        
            [Next](../QuickStartPlist/QuickStartPlist.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-03-24