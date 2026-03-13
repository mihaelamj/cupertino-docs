---
title: "Word and Line Calculations in Attributed Strings"
book: "Attributed String Programming Guide"
chapterId: "20000165"
date: "2014-02-11"
description: "Explains how to use attributed strings, which manage attributes of character strings or individual characters."
identifier: "//apple_ref/doc/uid/10000036i"
source: apple-archive
---

# Word and Line Calculations in Attributed Strings

> **Attributed String Programming Guide**
> Last updated: 2014-02-11

[Next](../Articles/standardAttributes.html)[Previous](../Articles/HTMLFilesandAttributedStrings.html)
        
        
        [](../index.html)
        
        # Word and Line Calculations in Attributed Strings

The Application Kit’s extensions to `NSAttributedString` support the typical behavior of text editors in selecting a word on a double-click with the `[doubleClickAtIndex:](https://developer.apple.com/documentation/foundation/nsattributedstring/1534748-doubleclickatindex)` method, and finds word breaks with `[nextWordFromIndex:forward:](https://developer.apple.com/documentation/foundation/nsattributedstring/1535305-nextwordfromindex)`. It also calculates line breaks with the `[lineBreakBeforeIndex:withinRange:](https://developer.apple.com/documentation/foundation/nsattributedstring/1526887-linebreak)` method.

        
            [Next](../Articles/standardAttributes.html)[Previous](../Articles/HTMLFilesandAttributedStrings.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11