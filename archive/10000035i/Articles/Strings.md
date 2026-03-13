---
title: "Strings"
book: "String Programming Guide"
chapterId: "20000145"
date: "2014-02-11"
description: "Explains how to create, search, concatenate, and draw strings in Cocoa."
identifier: "//apple_ref/doc/uid/10000035i"
source: apple-archive
---

# Strings

> **String Programming Guide**
> Last updated: 2014-02-11

[Next](CreatingStrings.html)[Previous](../introStrings.html)
        
        
        [](../index.html)
        
        # Strings

String objects represent character strings in Cocoa and Cocoa Touch [frameworks](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56). Representing strings as [objects](../../../../General/Conceptual/DevPedia-CocoaCore/ValueObject.html#//apple_ref/doc/uid/TP40008195-CH51) allows you to use strings wherever you use other objects. It also provides the benefits of encapsulation, so that string objects can use whatever encoding and storage is needed for efficiency while simply appearing as arrays of characters.

A string object is implemented as an array of Unicode characters (in other words, a text string). An [immutable](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectMutability.html#//apple_ref/doc/uid/TP40008195-CH42) string is a text string that is defined when it is created and subsequently cannot be changed. To create and manage an immutable string, use the `NSString` class. To construct and manage a string that can be changed after it has been created, use `NSMutableString`.

The objects you create using `NSString` and `NSMutableString` are referred to as string objects (or, when no confusion will result, merely as strings). The term *C string* refers to the standard C `char *` type.

A string object presents itself as an array of Unicode characters. You can determine how many characters it contains with the `[length](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/length)` method and can retrieve a specific character with the `[characterAtIndex:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/characterAtIndex:)` method. These two “primitive” methods provide basic access to a string object. Most use of strings, however, is at a higher level, with the strings being treated as single entities: You compare strings against one another, search them for substrings, combine them into new strings, and so on. If you need to access string objects character-by-character, you must understand the Unicode character encoding—specifically, issues related to composed character sequences. For details see:

*The Unicode Standard, Version 4.0*. The Unicode Consortium. Boston: Addison-Wesley, 2003. ISBN 0-321-18578-1.

The Unicode Consortium web site: [http://www.unicode.org/](http://www.unicode.org/).

        
            [Next](CreatingStrings.html)[Previous](../introStrings.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11