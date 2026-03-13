---
title: "Exceptions and the Cocoa Frameworks"
book: "Exception Programming Topics"
chapterId: "TP40009045"
date: "2013-08-08"
description: "Explains how to raise and handle exceptions in Cocoa applications."
identifier: "//apple_ref/doc/uid/10000012i"
source: apple-archive
---

# Exceptions and the Cocoa Frameworks

> **Exception Programming Topics**
> Last updated: 2013-08-08

[Next](../Tasks/HandlingExceptions.html)[Previous](../Exceptions.html)
        
        
        [](../index.html)
        
        # Exceptions and the Cocoa Frameworks
Exceptions in Cocoa are represented by objects of the `[NSException](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/cl/NSException)` class, which is part of the Foundation framework. The methods of this class allow you to create exception objects, raise (throw) exceptions with them, and get the call return addresses related to an exception. The attributes of an `NSException` object are the following:

A `[name](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/instm/NSException/name)` — a short string that is used to uniquely identify the exception. The name is required.

A `[reason](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/instm/NSException/reason)` — a longer string that contains a “human-readable” reason for the exception. The reason is required.

An optional dictionary (`[userInfo](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/instm/NSException/userInfo)`) used to supply application-specific data to the exception handler. For example, if the return value of a method causes an exception to be raised, you could pass the return value to the exception handler through `userInfo`.

You may extract the information in an exception object and, if appropriate, present to the user in an alert dialog, perhaps using an `[NSError](https://developer.apple.com/documentation/foundation/nserror)` object. See [Handling Exceptions](../Tasks/HandlingExceptions.html#//apple_ref/doc/uid/20000059-SW1) for information on this subject.

The Cocoa frameworks require that all exceptions be instances of `NSException` or its subclasses. Do not throw objects of other types.

The Cocoa frameworks are generally not exception-safe. The general pattern is that exceptions are reserved for programmer error only, and the program catching such an exception should quit soon afterwards. 

        
            [Next](../Tasks/HandlingExceptions.html)[Previous](../Exceptions.html)
        
         Copyright &#x00a9; 2002, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-08-08