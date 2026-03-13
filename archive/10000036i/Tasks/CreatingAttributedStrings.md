---
title: "Creating Attributed Strings in Cocoa"
book: "Attributed String Programming Guide"
chapterId: "20000714"
date: "2014-02-11"
description: "Explains how to use attributed strings, which manage attributes of character strings or individual characters."
identifier: "//apple_ref/doc/uid/10000036i"
source: apple-archive
---

# Creating Attributed Strings in Cocoa

> **Attributed String Programming Guide**
> Last updated: 2014-02-11

[Next](AccessingAttrs.html)[Previous](../Concepts/AttrStrings.html)
        
        
        [](../index.html)
        
        # Creating Attributed Strings in Cocoa

You create an `NSAttributedString` object in a number of different ways:

You can create a new string with the `initWithString:`, `initWithString:attributes:`, or `initWithAttributedString:` method. These methods initialize an attributed string with data you provide, as illustrated in the following example:

NSFont *font = [NSFont fontWithName:@"Palatino-Roman" size:14.0];
```
NSDictionary *attrsDictionary =
```

```
        [NSDictionary dictionaryWithObject:font
```

```
                                    forKey:NSFontAttributeName];
```

```
NSAttributedString *attrString =
```

```
    [[NSAttributedString alloc] initWithString:@"strigil"
```

```
            attributes:attrsDictionary];
```
For a list of attributes provided by the Application Kit framework see the Constants section in NSAttributedString Application Kit Additions Reference. 

The attribute values assigned to an attributed string become the property of that string, and should not be modified “behind the attributed string” by other objects. Doing so can render inconsistent the attributed string’s internal state. Always use `NSMutableAttributedString`’s `[setAttributes:range:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAttributedStrngClstr/Description.html#//apple_ref/occ/instm/NSMutableAttributedString/setAttributes:range:)` and related methods to change attribute values. See [Changing an Attributed String](ChangingAttrStrings.html#//apple_ref/doc/uid/20000162-BBCBGCDG) for more details.

 You can create an attributed string from rich text (RTF) or rich text with attachments (RTFD) data using the initialization methods, `initWithRTF:documentAttributes:`, `initWithRTFD:documentAttributes:`, and `initWithRTFDFileWrapper:documentAttributes:`, as illustrated in the following example:

NSData *rtfData = ...;  // assume rtfData is an NSData object containing valid RTF data
```
NSDictionary *docAttributes;
```

```
NSSize paperSize;
```

```
 
```

```
NSAttributedString *attrString;
```

```
 
```

```
if ((attrString = [[NSAttributedString alloc]
```

```
        initWithRTF: rtfData documentAttributes: &docAttributes])) {
```

```
 
```

```
    NSValue *value = [docAttrs objectForKey:@"PaperSize"];
```

```
    paperSize = [value sizeValue];
```

```
    // implementation continues...
```
You can create an attributed string from HTML data using the initialization methods `initWithHTML:documentAttributes:` and `initWithHTML:baseURL:documentAttributes:`. The methods return text attributes defined by the HTML as the attributes of the string. They return document-level attributes defined by the HTML, such as paper and margin sizes, by reference to an `NSDictionary` object, as described in [RTF Files and Attributed Strings](RTFAndAttrStrings.html#//apple_ref/doc/uid/20000164-BBCBJIBC). The methods translate HTML as well as possible into structures of the Cocoa text system, but the Application Kit does not provide complete, true rendering of arbitrary HTML.

**Multicore considerations:** Since OS X v10.4, `NSAttributedString` has used WebKit for all import  (but not for export) of HTML documents. Because WebKit document loading is not thread safe, this has not been safe to use on background threads. For applications linked on OS X v10.5 and later, if `NSAttributedString` imports HTML documents on any but the main thread, the use of WebKit is transferred to the main thread via `[performSelectorOnMainThread:withObject:waitUntilDone:](https://developer.apple.com/documentation/objectivec/nsobject/1414900-performselector)`. This makes the operation thread safe, but it requires that the main thread be executing the run loop in one of the common modes. This behavior can be overridden by setting the value of the standard user default `NSRunWebKitOnAppKitThread` to either `YES` (to obtain the new behavior regardless of linkage) or `NO` (to obtain the old behavior regardless of linkage).

        
            [Next](AccessingAttrs.html)[Previous](../Concepts/AttrStrings.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11