---
title: "Using Decimal Numbers"
book: "Number and Value Programming Topics"
chapterId: "20000176"
date: "2008-02-08"
description: "Explains how to use Cocoa object wrappers for primitive C data types."
identifier: "//apple_ref/doc/uid/10000038i"
source: apple-archive
---

# Using Decimal Numbers

> **Number and Value Programming Topics**
> Last updated: 2008-02-08

[Next](Null.html)[Previous](Numbers.html)
        
        
        [](../index.html)
        
        # Using Decimal Numbers

`[NSDecimalNumber](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDecimalNumber/Description.html#//apple_ref/occ/cl/NSDecimalNumber)` is an immutable subclass of `NSNumber` that provides an object-oriented wrapper for doing base-10 arithmetic. An instance can represent any number that can be expressed as `mantissa x 10 exponent` where *mantissa* is a decimal integer up to 38 digits long, and *exponent* is an integer between -128 and 127.

In the course of doing arithmetic, a method may produce calculation errors, such as division by zero. It may also meet circumstances where it has a choice of ways to round a number off. The way the method acts on such occasions is called its “behavior.”

Behavior is set by methods in the `[NSDecimalNumberBehaviors](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSDecNumberBehaviors/Description.html#//apple_ref/occ/intf/NSDecimalNumberBehaviors)` protocol. Every `NSDecimalNumber` argument called `behavior` requires an object that conforms to this protocol. For more on behaviors, see the specifications for the `[NSDecimalNumberBehaviors](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSDecNumberBehaviors/Description.html#//apple_ref/occ/intf/NSDecimalNumberBehaviors)` protocol and the `[NSDecimalNumberHandler](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDecimalNumberHandler/Description.html#//apple_ref/occ/cl/NSDecimalNumberHandler)` class. Also see the `[defaultBehavior](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDecimalNumber/Description.html#//apple_ref/occ/clm/NSDecimalNumber/defaultBehavior)` method description.

## C Interface to Decimal Numbers

You can access the arithmetic and rounding methods of `NSDecimalNumber` through group of C functions: 

`[NSDecimalAdd](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSDecimalAdd)``[NSDecimalCompact](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSDecimalCompact)``[NSDecimalCompare](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSDecimalCompare)``[NSDecimalCopy](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSDecimalCopy)``[NSDecimalDivide](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSDecimalDivide)``[NSDecimalIsNotANumber](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSDecimalIsNotANumber)``[NSDecimalMultiply](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSDecimalMultiply)``[NSDecimalMultiplyByPowerOf10](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSDecimalMultiplyByPowerOf10)``[NSDecimalNormalize](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSDecimalNormalize)``[NSDecimalPower](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSDecimalPower)``[NSDecimalRound](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSDecimalRound)``[NSDecimalString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSDecimalString)``[NSDecimalSubtract](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSDecimalSubtract)`You might consider the C interface if you don’t need to treat decimal numbers as objects—that is, if you don’t need to store them in an object-oriented collection like an instance of `NSArray` or `NSDictionary`. You might also consider the C interface if you need maximum efficiency. The C interface is faster and uses less memory than the `NSDecimalNumber` class.

If you need mutability, you can combine the two interfaces. Use functions from the C interface and convert their results to instances of `NSDecimalNumber`.

        
            [Next](Null.html)[Previous](Numbers.html)
        
         Copyright &#x00a9; 2008 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2008-02-08