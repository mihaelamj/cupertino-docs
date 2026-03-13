---
title: "Drawing Attributed Strings"
book: "Attributed String Programming Guide"
chapterId: "20000163"
date: "2014-02-11"
description: "Explains how to use attributed strings, which manage attributes of character strings or individual characters."
identifier: "//apple_ref/doc/uid/10000036i"
source: apple-archive
---

# Drawing Attributed Strings

> **Attributed String Programming Guide**
> Last updated: 2014-02-11

[Next](RTFAndAttrStrings.html)[Previous](ChangingAttrStrings.html)
        
        
        [](../index.html)
        
        # Drawing Attributed Strings

The Application Kit’s NSStringDrawing extensions let you draw an attributed string in a focused graphics context (typically an NSView) using a number of methods: `drawAtPoint:`, `drawInRect:`, and (with OS X v10.4 and later) `drawWithRect:options:`. These methods are designed for drawing small amounts of text or text that must be drawn rarely. They create and dispose of various supporting text objects every time you call them. To draw strings repeatedly, it is more efficient to use NSLayoutManager, as described in [Drawing Strings](../../TextLayout/Tasks/DrawingStrings.html#//apple_ref/doc/uid/20001808).

Note that the Application Kit defines drawing methods for NSString as well, allowing any string object to draw itself. These methods, `drawAtPoint:withAttributes:`, `drawInRect:withAttributes:`, and (with OS X v10.4 and later) `drawWithRect:options:attributes:`, are described in NSString Additions.

With OS X v10.4 and later, you can find out the rectangle required to lay out an attributed string using the method, `boundingRectWithSize:options:`. Again, there is an analogous method to determine the rectangle required to render an NSString object, given a set of attributes—`boundingRectWithSize:options:attributes:`.

        
            [Next](RTFAndAttrStrings.html)[Previous](ChangingAttrStrings.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11