---
title: "Introduction"
book: "String Programming Guide"
chapterId: "10000035"
date: "2014-02-11"
description: "Explains how to create, search, concatenate, and draw strings in Cocoa."
identifier: "//apple_ref/doc/uid/10000035i"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Introduction

> **String Programming Guide**
> Last updated: 2014-02-11

[Next](Articles/Strings.html)
        
        
        [](index.html)
        
        # Introduction to String Programming Guide
*String Programming Guide* describes how to create, search, concatenate, and draw strings. It also describes character sets, which let you search a string for characters in a group, and scanners, which convert numbers to strings and vice versa.

## Who Should Read This Document
You should read this document if you need to work directly with strings or character sets.

## Organization of This Document
This document contains the following articles:

[Strings](Articles/Strings.html#//apple_ref/doc/uid/20000145-BBCCGGCC) describes the characteristics of string objects in Cocoa and Cocoa Touch.

[Creating and Converting String Objects](Articles/CreatingStrings.html#//apple_ref/doc/uid/20000148-CJBCJHHI) explains the ways in which `NSString` and its subclass `NSMutableString` create string objects and convert their contents to and from the various character encodings they support.

[Formatting String Objects](Articles/FormatStrings.html#//apple_ref/doc/uid/20000943-CJBEAEHH) describes how to format `NSString` objects.

[String Format Specifiers](Articles/formatSpecifiers.html#//apple_ref/doc/uid/TP40004265-SW1) describes `printf`-style format specifiers supported by `NSString`.

[Reading Strings From and Writing Strings To Files and URLs](Articles/readingFiles.html#//apple_ref/doc/uid/TP40003459-SW1) describes how to read strings from and write strings to files and URLs.

[Searching, Comparing, and Sorting Strings](Articles/SearchingStrings.html#//apple_ref/doc/uid/20000149-CJBBGBAI) describes methods for finding characters and substrings within strings and for comparing one string to another.

[Words, Paragraphs, and Line Breaks](Articles/stringsParagraphBreaks.html#//apple_ref/doc/uid/TP40005016-SW1) describes how words, paragraphs, and line breaks are determined.

[Characters and Grapheme Clusters](Articles/stringsClusters.html#//apple_ref/doc/uid/TP40008025-SW1) describes how you can break strings down into user-perceived characters.

[Character Sets](Articles/CharacterSets.html#//apple_ref/doc/uid/20000146-BAJBJHCG) explains how to use character set objects, and how to use `NSCharacterSet` methods to create standard and custom character sets.

[Scanners](Articles/Scanners.html#//apple_ref/doc/uid/20000147-BCIEFGHC) describes `NSScanner` objects, which interpret and convert the characters of an `NSString` object into number and string values.

[String Representations of File Paths](Articles/ManipulatingPaths.html#//apple_ref/doc/uid/20000152-BBCBIGHH) describes the `NSString` methods that manipulate strings as file-system paths.

[Drawing Strings](Articles/DrawingStrings.html#//apple_ref/doc/uid/20000151-BBCHAGDG) discusses the methods of the `NSString` class that support drawing directly in an `NSView` object.

## See Also
For more information, refer to the following documents:

*[Attributed String Programming Guide](../AttributedStrings/AttributedStrings.html#//apple_ref/doc/uid/10000036i)* is closely related to *String Programming Guide*. It provides information about `NSAttributedString` objects, which manage sets of attributes, such as font and kerning, that are associated with character strings or individual characters.

*[Data Formatting Guide](../DataFormatting/DataFormatting.html#//apple_ref/doc/uid/10000029i)* describes how to format data using objects that create, interpret, and validate text.

*[Internationalization and Localization Guide](../../../MacOSX/Conceptual/BPInternational/Introduction/Introduction.html#//apple_ref/doc/uid/10000171i)* provides information about localizing strings in your project, including information on how string formatting arguments can be ordered.

*[String Programming Guide for Core Foundation](../../../CoreFoundation/Conceptual/CFStrings/introCFStrings.html#//apple_ref/doc/uid/10000131i)* in Core Foundation, discusses the Core Foundation opaque type CFString, which is toll-free bridged with the `NSString` class.

        
            [Next](Articles/Strings.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11