---
title: "Words, Paragraphs, and Line Breaks"
book: "String Programming Guide"
chapterId: "TP40005016"
date: "2014-02-11"
description: "Explains how to create, search, concatenate, and draw strings in Cocoa."
identifier: "//apple_ref/doc/uid/10000035i"
source: apple-archive
---

# Words, Paragraphs, and Line Breaks

> **String Programming Guide**
> Last updated: 2014-02-11

[Next](stringsClusters.html)[Previous](SearchingStrings.html)
        
        
        [](../index.html)
        
        # Words, Paragraphs, and Line Breaks
This article describes how word and paragraph boundaries are defined, how line breaks are represented, and how you can separate a string by paragraph.

## Word Boundaries
The text system determines word boundaries in a language-specific manner according to [Unicode Standard Annex #29](http://www.unicode.org/reports/tr29/#Word_Boundaries) with additional customization for locale as described in that document. On OS X, Cocoa presents APIs related to word boundaries, such as the `NSAttributedString` methods `[doubleClickAtIndex:](https://developer.apple.com/documentation/foundation/nsattributedstring/1534748-doubleclickatindex)` and `[nextWordFromIndex:forward:](https://developer.apple.com/documentation/foundation/nsattributedstring/1535305-nextwordfromindex)`, but you cannot modify the way the word-boundary algorithms themselves work.

## Line and Paragraph Separator Characters
There are a number of ways in which a line or paragraph break can be represented. Historically, `\n`, `\r`, and `\r\n` have been used. Unicode defines an unambiguous paragraph separator, `U+2029` (for which Cocoa provides the constant `NSParagraphSeparatorCharacter`), and an unambiguous line separator, `U+2028` (for which Cocoa provides the constant `NSLineSeparatorCharacter`).

In the Cocoa text system, the `NSParagraphSeparatorCharacter` is treated consistently as a paragraph break, and `NSLineSeparatorCharacter` is treated consistently as a line break that is not a paragraph break—that is, a line break within a paragraph. However, in other contexts, there are few guarantees as to how these characters will be treated. POSIX-level software, for example, often recognizes only `\n` as a break. Some older Macintosh software recognizes only `\r`, and some Windows software recognizes only `\r\n`. Often there is no distinction between line and paragraph breaks.

Which line or paragraph break character you should use depends on how your data may be used and on what platforms. The Cocoa text system recognizes `\n`, `\r`, or `\r\n` all as paragraph breaks—equivalent to `NSParagraphSeparatorCharacter`. When it inserts paragraph breaks, for example with `insertNewline:`, it uses `\n`. Ordinarily `NSLineSeparatorCharacter` is used only for breaks that are specifically line breaks and not paragraph breaks, for example in `insertLineBreak:`, or for representing HTML `<br>` elements.

If your breaks are specifically intended as line breaks and not paragraph breaks, then you should typically use `NSLineSeparatorCharacter`. Otherwise, you may use `\n`, `\r`, or `\r\n` depending on what other software is likely to process your text. The default choice for Cocoa is usually `\n`.

## Separating a String “by Paragraph”
 A common approach to separating a string “by paragraph” is simply to use:

NSArray *arr = [myString componentsSeparatedByString:@"\n"];This, however, ignores the fact that there are a number of other ways in which a paragraph or line break may be represented in a string—`\r`, `\r\n`, or Unicode separators. 

Instead you can use methods—such as `[enumerateSubstringsInRange:options:usingBlock:](https://developer.apple.com/documentation/foundation/nsstring/1416774-enumeratesubstringsinrange)` and `[enumerateLinesUsingBlock:](https://developer.apple.com/documentation/foundation/nsstring/1408459-enumeratelinesusingblock)`—that take into account the variety of possible line terminations, as illustrated in the following example.

```
NSString *string = /* assume this exists */;
```

```
NSRange range = NSMakeRange(0, string.length);
```

```
[string enumerateSubstringsInRange:range
```

```
                           options:NSStringEnumerationByParagraphs
```

```
                        usingBlock:^(NSString * _Nullable paragraph, NSRange paragraphRange, NSRange enclosingRange, BOOL * _Nonnull stop) {             // ... }];
```

        
            [Next](stringsClusters.html)[Previous](SearchingStrings.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11