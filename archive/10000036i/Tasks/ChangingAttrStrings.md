---
title: "Changing an Attributed String"
book: "Attributed String Programming Guide"
chapterId: "20000162"
date: "2014-02-11"
description: "Explains how to use attributed strings, which manage attributes of character strings or individual characters."
identifier: "//apple_ref/doc/uid/10000036i"
source: apple-archive
---

# Changing an Attributed String

> **Attributed String Programming Guide**
> Last updated: 2014-02-11

[Next](DrawingAttrStrings.html)[Previous](AccessingAttrs.html)
        
        
        [](../index.html)
        
        # Changing an Attributed String

`NSMutableAttributedString` declares a number of methods for changing both characters and attributes. You must take care not to modify attribute values after they have been passed to an attributed string. You may also need to repair inconsistencies that can be introduced if you modify an attributed string.

## Modifying Attributes

`NSMutableAttributedString` declares a number of methods for changing both characters and attributes, such as the primitive `replaceCharactersInRange:withString:` and `setAttributes:range:`, or the more convenient methods `addAttribute:value:range:`, `applyFontTraits:range:`, and so on.

The following example illustrates how to specify a link attribute for a selected range in an attributed string, underline the text, and color it blue. Note that *you can define whatever value you want for the link attribute, it is up to you to interpret the value when the link is selected*—see [Accessing Attributes](AccessingAttrs.html#//apple_ref/doc/uid/20000161-BAJBBCBI)—typically, however, you use either a string or a URL. For an explanation of the role of `beginEditing` and `endEditing` (shown in the sample), see [Fixing Inconsistencies](#//apple_ref/doc/uid/20000162-176048).

```
NSMutableAttributedString *string; // assume string exists
```

```
NSRange selectedRange; // assume this is set
```

```
 
```

```
NSURL *linkURL = [NSURL URLWithString:@"http://www.apple.com/"];
```

```
 
```

```
[string beginEditing];
```

```
[string addAttribute:NSLinkAttributeName
```

```
               value:linkURL
```

```
               range:selectedRange];
```

```
 
```

```
[string addAttribute:NSForegroundColorAttributeName
```

```
               value:[NSColor blueColor]
```

```
               range:selectedRange];
```

```
 
```

```
[string addAttribute:NSUnderlineStyleAttributeName
```

```
               value:[NSNumber numberWithInt:NSSingleUnderlineStyle]
```

```
               range:selectedRange];
```

```
[string endEditing];
```

Attribute values assigned to an attributed string become the property of that string, and should not be modified “behind the attributed string” by other objects. Doing so can render inconsistent the attributed string’s internal state. There are two main reasons for this:

How an attribute value propagates through an attributed string is not predictable. If you change the value, you might be editing more of the attributed string than you thought. In fact the value could have been copied to the undo stack, or to a totally different document, and so on.

Attributed strings do caching and uniquing of attributes, which assumes attribute values do not change. The assumption is that `isEqual:` and `hash` on attribute values will not change once the attribute value has been set.

If you must change attribute values, and are sure that the change will apply to the correct range, there are two strategies you can adopt:

Use an attribute value whose `isEqual:` and `hash` do not depend on the values you are modifying.

Use indirection: use the attribute value as a lookup key into a table where the actual value can be changed. For instance, this might be the appropriate approach for having a “stylesheet”-like attribute.

## Fixing Inconsistencies

All of the methods for changing a mutable attributed string properly update the mapping between characters and attributes, but after a change some inconsistencies can develop. Here are some examples of attribute consistency requirements:

Paragraph styles must apply to entire paragraphs.

Scripts may only be assigned fonts that support them. For example, Kanji and Arabic characters can’t be assigned the Times-Roman font, and must be reassigned fonts that support these scripts.

Deleting attachment characters from the string requires the corresponding attachment objects to be released. Similarly, removing attachment objects requires the corresponding attachment characters to be removed from the string.

A code editing application that displays all language keywords in boldface can automatically assign this attribute as the user changes the font or edits the text.

The Application Kit’s extensions to `NSMutableAttributedString` define methods to fix these inconsistencies as changes are made. This allows the attributes to be cleaned up at a low level, hiding potential problems from higher levels and providing for very clean update of display as attributes change. There are four methods for fixing attributes and two to group editing changes:

`fixAttributesInRange:`

`fixAttachmentAttributeInRange:`

`fixFontAttributeInRange:`

`fixParagraphStyleAttributeInRange:`

`beginEditing`

`endEditing`

The first method, `fixAttributesInRange:`, invokes the other three `fix...` methods to clean up deleted attachment references, font attributes, and paragraph attributes, respectively. The individual method descriptions explain what cleanup entails for each case.

`NSMutableAttributedString` provides `beginEditing` and `endEditing` methods for subclasses of `NSMutableAttributedString` to override. These methods allow instances of a subclass to record or buffer groups of changes and clean themselves up on receiving an `endEditing` message. The `endEditing` method also allows the receiver to notify any observers that it has been changed. `NSTextStorage`’s implementation of `endEditing`, for example, fixes changed attributes and then notifies its layout managers that they need to re-lay and redisplay their text. The default implementations do nothing.

        
            [Next](DrawingAttrStrings.html)[Previous](AccessingAttrs.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11