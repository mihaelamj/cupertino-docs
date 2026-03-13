---
title: "Naming Functions"
book: "Coding Guidelines for Cocoa"
chapterId: "20001283"
date: "2013-10-22"
description: "Provides naming guidelines for Cocoa API and design advice to framework developers."
identifier: "//apple_ref/doc/uid/10000146i"
source: apple-archive
---

# Naming Functions

> **Coding Guidelines for Cocoa**
> Last updated: 2013-10-22

[Next](NamingIvarsAndTypes.html)[Previous](NamingMethods.html)
        
        
        [](../index.html)
        
        # Naming Functions

[Objective-C](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectiveC.html#//apple_ref/doc/uid/TP40008195-CH43) allows you to express behavior through functions as well as methods. You should use functions rather than, say, [class methods](../../../../General/Conceptual/DevPedia-CocoaCore/ClassMethod.html#//apple_ref/doc/uid/TP40008195-CH8), when the underlying object is always a [singleton](../../../../General/Conceptual/DevPedia-CocoaCore/Singleton.html#//apple_ref/doc/uid/TP40008195-CH49) or when you are dealing with obviously functional subsystems.

Functions have some general naming rules that you should follow:

Function names are formed like method names, but with a couple exceptions:

They start with the same prefix that you use for classes and constants.

The first letter of the word after the prefix is capitalized.

Most function names start with verbs that describe the effect the function has:

```
NSHighlightRect
```

```
NSDeallocateObject
```

Functions that query properties have a further set of naming rules:

If the function returns the property of its first argument, omit the verb.

```
unsigned int NSEventMaskFromType(NSEventType type)
```

```
float NSHeight(NSRect aRect)
```

If the value is returned by reference, use “Get”.

```
const char *NSGetSizeAndAlignment(const char *typePtr, unsigned int *sizep, unsigned int *alignp)
```

If the value returned is a boolean, the function should begin with an inflected verb.

```
BOOL NSDecimalIsNotANumber(const NSDecimal *decimal)
```

        
            [Next](NamingIvarsAndTypes.html)[Previous](NamingMethods.html)
        
         Copyright &#x00a9; 2003, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-10-22