---
title: "Accessing Attributes"
book: "Attributed String Programming Guide"
chapterId: "20000161"
date: "2014-02-11"
description: "Explains how to use attributed strings, which manage attributes of character strings or individual characters."
identifier: "//apple_ref/doc/uid/10000036i"
source: apple-archive
---

# Accessing Attributes

> **Attributed String Programming Guide**
> Last updated: 2014-02-11

[Next](ChangingAttrStrings.html)[Previous](CreatingAttributedStrings.html)
        
        
        [](../index.html)
        
        # Accessing Attributes
An attributed string identifies attributes by name, storing a value under the attribute name in an `NSDictionary` object, which is in turn associated with an `NSRange` that indicates the characters to which the dictionary’s attributes apply. You can assign any attribute name-value pair you wish to a range of characters, in addition to the standard attributes.

## Retrieving Attribute Values
With an immutable attributed string, you assign all attributes when you create the string. In Java, you use the constructors. In Objective-C, you use methods such as `initWithString:attributes:`, which explicitly take an `NSDictionary` object of name-value pairs, or `initWithString:`, which assigns no attributes. And the Application Kit’s extensions to `NSAttributedString` adds methods that take an RTF file or an HTML file. See [Changing an Attributed String](ChangingAttrStrings.html#//apple_ref/doc/uid/20000162-BBCBGCDG) for information on assigning attributes with a mutable attributed string.

To retrieve attribute values from either type of attributed string, use any of these methods:

`attributesAtIndex:effectiveRange:`

`attributesAtIndex:longestEffectiveRange:inRange:`

`attribute:atIndex:effectiveRange:`

`attribute:atIndex:longestEffectiveRange:inRange:`

`fontAttributesInRange:`

`rulerAttributesInRange:`

The first two methods return all attributes at a given index, the `attribute:...` methods return the value of a single named attribute. The Application Kit’s extensions to `NSAttributedString` add `fontAttributesInRange:` and `rulerAttributesInRange:`, which return attributes defined to apply only to characters or to whole paragraphs, respectively.

The first four methods also return by reference the effective range and the longest effective range of the attributes. These ranges allow you to determine the extent of attributes. Conceptually, each character in an attributed string has its own collection of attributes; however, it’s often useful to know when the attributes and values are the same over a series of characters. This allows a routine to progress through an attributed string in chunks larger than a single character. In retrieving the effective range, an attributed string simply looks up information in its attribute mapping, essentially the dictionary of attributes that apply at the index requested. In retrieving the longest effective range, the attributed string continues checking characters past this basic range as long as the attribute values are the same. This extra comparison increases the execution time for these methods but guarantees a precise maximal range for the attributes requested.

## Effective and Maximal Ranges
Methods that return an effective range by reference are not guaranteed to return the maximal range to which the attribute(s) apply; they are merely guaranteed to return some range over which they apply. In practice they will return whatever range is readily available from the attributed string's internal storage mechanisms, which may depend on the implementation and on the precise history of modifications to the attributed string.

Methods that return a longest effective range by reference, on the other hand, are guaranteed to return the longest range containing the specified index to which the attribute(s) in question apply (constrained by the value of the argument passed in for `inRange:`). For efficiency, it is important that the `inRange:` argument should be as small as appropriate for the range of interest to the client.

When you iterate over an attributed string by attribute ranges, either sort of method may be appropriate depending on the situation. If there is some processing to be done for each range, and you know that the full range for a given attribute is going to have to be handled eventually, it may be more efficient to use the longest-effective-range variant, so as not to have to handle the range in pieces. However, you should use the longest-effective-range methods with caution, because the longest effective range could be quite long—potentially the entire length of the document, if the `inRange:` argument is not constrained. 

The Objective-C code fragment below progresses through an attributed string in chunks based on the effective range. The fictitious analyzer object here counts the number of characters in each font. The while loop progresses as long as the effective range retrieved does not include the end of the attributed string, retrieving the font in effect just past the latest retrieved range. For each font attribute retrieved, the analyzer tallies the number of characters in the effective range. In this example, it is possible that consecutive invocations of `attribute:atIndex:effectiveRange:` will return the same value.

NSAttributedString *attrStr;
```
unsigned int length;
```

```
NSRange effectiveRange;
```

```
id attributeValue;
```

```
 
```

```
length = [attrStr length];
```

```
effectiveRange = NSMakeRange(0, 0);
```

```
 
```

```
while (NSMaxRange(effectiveRange) < length) {
```

```
    attributeValue = [attrStr attribute:NSFontAttributeName
```

```
        atIndex:NSMaxRange(effectiveRange) effectiveRange:&effectiveRange];
```

```
    [analyzer tallyCharacterRange:effectiveRange font:attributeValue];
```

```
}
```
In contrast, the next Objective-C code fragment progresses through the attributed string according to the maximum effective range for each font. In this case, the analyzer counts font changes, which may not be represented by merely retrieving effective ranges. In this case the while loop is predicated on the length of the limiting range, which begins as the entire length of the attributed string and is whittled down as the loop progresses. After the analyzer records the font change, the limit range is adjusted to account for the longest effective range retrieved.

NSAttributedString *attrStr;
```
NSRange limitRange;
```

```
NSRange effectiveRange;
```

```
id attributeValue;
```

```
 
```

```
limitRange = NSMakeRange(0, [attrStr length]);
```

```
 
```

```
while (limitRange.length > 0) {
```

```
    attributeValue = [attrStr attribute:NSFontAttributeName
```

```
        atIndex:limitRange.location longestEffectiveRange:&effectiveRange
```

```
        inRange:limitRange];
```

```
    [analyzer recordFontChange:attributeValue];
```

```
    limitRange = NSMakeRange(NSMaxRange(effectiveRange),
```

```
        NSMaxRange(limitRange) - NSMaxRange(effectiveRange));
```

```
}
```
Note that the second code fragment is more complex. Because of this, and because `attribute:atIndex:longestEffectiveRange:inRange:` is somewhat slower than `attribute:atIndex:effectiveRange:`, you should typically use it only when absolutely necessary for the work you’re performing. In most cases working by effective range is enough.

        
            [Next](ChangingAttrStrings.html)[Previous](CreatingAttributedStrings.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11