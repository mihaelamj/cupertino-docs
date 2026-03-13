---
title: "Standard Attributes"
book: "Attributed String Programming Guide"
chapterId: "TP40004903"
date: "2014-02-11"
description: "Explains how to use attributed strings, which manage attributes of character strings or individual characters."
identifier: "//apple_ref/doc/uid/10000036i"
source: apple-archive
---

# Standard Attributes

> **Attributed String Programming Guide**
> Last updated: 2014-02-11

[Next](../RevisionHistory.html)[Previous](../Tasks/WordCalcAttrStrings.html)
        
        
        [](../index.html)
        
        # Standard Attributes
The identifiers listed in Table 1 are global `NSString` constants containing the attribute names. The value class is the class of the value corresponding to that attribute.

**Table 1**  Table of standard attributesAttribute Identifier

Value Class

Default Value

`NSAttachmentAttributeName`

`NSTextAttachment`

none (no attachment)

`NSBackgroundColorAttributeName`

`NSColor`

none (no background)

`NSBaselineOffsetAttributeName`

`NSNumber`, as a float

0.0

`NSFontAttributeName`

`NSFont`

Helvetica 12-point

`NSForegroundColorAttributeName`

`NSColor`

black

`NSKernAttributeName`

`NSNumber`, as a float

0.0

`NSLigatureAttributeName`

`NSNumber`, as an int

1 (standard ligatures)

`NSLinkAttributeName`

`id`

none (no link)

`NSParagraphStyleAttributeName`

`NSParagraphStyle`

(as returned by `NSParagraphStyle`’s `[defaultParagraphStyle](https://developer.apple.com/documentation/appkit/nsparagraphstyle/1532681-default)` method)

`NSSuperscriptAttributeName`

`NSNumber`, as an int

0

`NSUnderlineStyleAttributeName`

`NSNumber`, as an int

none (no underline)

The natures of several attributes are not obvious from name alone:

The baseline offset attribute is a literal distance, in pixels, by which the characters should be shifted above the baseline (for positive offsets) or below (for negative offsets).

The kerning attribute indicates how much the following character should be shifted from its default offset as defined by the current character’s font; a positive kern indicates a shift farther along and a negative kern indicates a shift closer to the current character.

The ligature attribute determines what kinds of ligatures should be used when displaying the string. A value of 0 indicates that only ligatures essential for proper rendering of text should be used, 1 indicates that standard ligatures should be used, and 2 indicates that all available ligatures should be used. Which ligatures are standard depends on the script and possibly the font. Arabic text, for example, requires ligatures for many character sequences, but has a rich set of additional ligatures that combine characters. English text has no essential ligatures, and typically has only two standard ligatures, those for “fi” and “fl”—all others being considered more advanced or fancy.

The link attribute specifies an arbitrary object that is passed to the `NSTextView` method `clickedOnLink:atIndex:` when the user clicks in the text range associated with the `NSLinkAttributeName` attribute. The text view’s delegate object can implement `textView:clickedOnLink:atIndex:` or `textView:clickedOnLink:` to process the link object. Otherwise, the default implementation checks whether the link object is an `NSURL` object and, if so, opens it in the URL’s default application.

The superscript attribute indicates an abstract level for both super- and subscripts. The user of the attributed string can interpret this as desired, adjusting the baseline by the same or a different amount for each level, changing the font size, or both.

The underline attribute has only two values defined, `NSNoUnderlineStyle` and `NSSingleUnderlineStyle`, but these can be combined with `NSUnderlineByWordMask` and `NSUnderlineStrikethroughMask` to extend their behavior. By bitwise-ORing these values in different combinations, you can specify no underline, a single underline, a single strikethrough, both an underline and a strikethrough, and whether the line is drawn for whitespace or not.

        
            [Next](../RevisionHistory.html)[Previous](../Tasks/WordCalcAttrStrings.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11