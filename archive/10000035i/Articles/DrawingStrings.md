---
title: "Drawing Strings"
book: "String Programming Guide"
chapterId: "20000151"
date: "2014-02-11"
description: "Explains how to create, search, concatenate, and draw strings in Cocoa."
identifier: "//apple_ref/doc/uid/10000035i"
source: apple-archive
---

# Drawing Strings

> **String Programming Guide**
> Last updated: 2014-02-11

[Next](../RevisionHistory.html)[Previous](ManipulatingPaths.html)
        
        
        [](../index.html)
        
        # Drawing Strings

You can draw string objects directly in a focused `NSView` using methods such as `[drawAtPoint:withAttributes:](https://developer.apple.com/documentation/foundation/nsstring/1533109-drawatpoint)` (to draw a string with multiple attributes, such as multiple text fonts, you must use an `NSAttributedString` object). These methods are described briefly in [Text](../../CocoaDrawingGuide/Text/Text.html#//apple_ref/doc/uid/TP40003290-CH209) in *[Cocoa Drawing Guide](../../CocoaDrawingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40003290)*.

The simple methods, however, are designed for drawing small amounts of text or text that is only drawn rarely—they create and dispose of various supporting objects every time you call them. To draw strings repeatedly, it is more efficient to use `NSLayoutManager`, as described in [Drawing Strings](../../TextLayout/Tasks/DrawingStrings.html#//apple_ref/doc/uid/20001808) in *[Text Layout Programming Guide](../../TextLayout/TextLayout.html#//apple_ref/doc/uid/10000158i)*. For an overview of the Cocoa text system, of which `NSLayoutManager` is a part, see *[Cocoa Text Architecture Guide](../../../../TextFonts/Conceptual/CocoaTextArchitecture/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009459)*.

        
            [Next](../RevisionHistory.html)[Previous](ManipulatingPaths.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11