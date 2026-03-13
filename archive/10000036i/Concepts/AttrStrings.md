---
title: "Attributed Strings"
book: "Attributed String Programming Guide"
chapterId: "20000160"
date: "2014-02-11"
description: "Explains how to use attributed strings, which manage attributes of character strings or individual characters."
identifier: "//apple_ref/doc/uid/10000036i"
source: apple-archive
---

# Attributed Strings

> **Attributed String Programming Guide**
> Last updated: 2014-02-11

[Next](../Tasks/CreatingAttributedStrings.html)[Previous](../AttributedStrings.html)
        
        
        [](../index.html)
        
        # Attributed Strings
Attributed string objects manage character strings and associated sets of attributes (for example, font and kerning) that apply to individual characters or ranges of characters in the string. The classes `NSAttributedString` and `NSMutableAttributedString` declare the programmatic interface for read-only attributed strings and modifiable attributed strings, respectively. The Foundation Kit defines the basic functionality, while additional Objective-C methods are defined in the Application Kit. The Application Kit also uses a subclass of `NSMutableAttributedString`, called `NSTextStorage`, to provide the storage for the extended text-handling system (see *[Text System Storage Layer Overview](../../TextStorageLayer/TextStorageLayer.html#//apple_ref/doc/uid/10000087i)*).

`NSAttributedString` and `NSMutableAttributedString` are toll-free bridged to their Core Foundation counterparts, CFAttributedString and CFMutableAttributedString respectively. This means that a Foundation attributed string is interchangeable in function or method calls with the corresponding bridged Core Foundation type. Therefore, in a method where you see an `NSMutableAttributedString *` parameter, you can pass in a variable of type `CFMutableAttributedStringRef`, and in a function where you see a `CFAttributedStringRef` parameter, you can pass in an instance of `NSAttributedString` (or `NSMutableAttributedString`).

`NSAttributedString` is not a subclass of `NSString`. It contains an `NSString` object to which it applies attributes. This protects users of attributed strings from ambiguities caused by the semantic differences between simple and attributed strings. For example, equality can’t be simply defined between an `NSString` and an attributed string. The attributed string classes adopt the `NSCopying` and `NSMutableCopying` protocols, making it convenient to convert an attributed string from one type to the other.

`NSAttributedString` and `NSMutableAttributedString` add a number of features to the basic content storage of `NSString`:

Association of arbitrary, programmer-defined attributes with ranges of characters.

Preservation of attribute-to-character mapping after changes (`NSMutableAttributedString`).

Support for RTF, including file attachments and graphics.

Drawing in `NSView` objects (note that the Application Kit adds drawing methods to `NSString` as well)

Linguistic unit (word) and line calculation.

An attributed string identifies attributes by name, storing their values as opaque `id`s in an `NSDictionary` object. For example, the text font is stored as an `NSFont` object under the name given by `NSFontAttributeName`. You can associate any object value, by any name, with a given range of characters in the attributed string. 

A mutable attributed string keeps track of the attribute mapping as characters are added to and deleted from it and as attributes are changed. It allows you to group batches of edits with the `beginEditing` and `endEditing` methods, and to consolidate changes to the attribute-to-character mapping with the `fix...` methods.

        
            [Next](../Tasks/CreatingAttributedStrings.html)[Previous](../AttributedStrings.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11