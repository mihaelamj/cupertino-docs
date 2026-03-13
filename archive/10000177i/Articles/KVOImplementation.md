---
title: "Key-Value Observing Implementation Details"
book: "Key-Value Observing Programming Guide"
chapterId: "20002307"
date: "2016-09-13"
description: "Explains the Cocoa key-value observing protocol."
identifier: "//apple_ref/doc/uid/10000177i"
source: apple-archive
---

# Key-Value Observing Implementation Details

> **Key-Value Observing Programming Guide**
> Last updated: 2016-09-13

[Next](../RevisionHistory.html)[Previous](KVODependentKeys.html)
        
        
        [](../index.html)
        
        # Key-Value Observing Implementation Details 

Automatic key-value observing is implemented using a technique called *isa-swizzling*. 

The `isa` pointer, as the name suggests, points to the object's class which maintains a dispatch table. This dispatch table essentially contains pointers to the methods the class implements, among other data. 

When an observer is registered for an attribute of an object the isa pointer of the observed object is modified, pointing to an intermediate class rather than at the true class. As a result the value of the isa pointer does not necessarily reflect the actual class of the instance. 

You should never rely on the `isa` pointer to determine class membership. Instead, you should use the `[class](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intfm/NSObject/class)` method to determine the class of an object instance.

        
            [Next](../RevisionHistory.html)[Previous](KVODependentKeys.html)
        
         Copyright &#x00a9; 2003, 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-09-13