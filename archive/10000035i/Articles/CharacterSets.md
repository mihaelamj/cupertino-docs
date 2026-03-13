---
title: "Character Sets"
book: "String Programming Guide"
chapterId: "20000146"
date: "2014-02-11"
description: "Explains how to create, search, concatenate, and draw strings in Cocoa."
identifier: "//apple_ref/doc/uid/10000035i"
source: apple-archive
---

# Character Sets

> **String Programming Guide**
> Last updated: 2014-02-11

[Next](Scanners.html)[Previous](stringsClusters.html)
        
        
        [](../index.html)
        
        # Character Sets
An `NSCharacterSet` object represents a set of Unicode characters. `NSString` and `NSScanner` objects use `NSCharacterSet` objects to group characters together for searching operations, so that they can find any of a particular set of characters during a search.

## Character Set Basics

A character set object represents a set of Unicode characters. Character sets are represented by instances of a [class cluster](../../../../General/Conceptual/DevPedia-CocoaCore/ClassCluster.html#//apple_ref/doc/uid/TP40008195-CH7). The cluster’s two public classes, `NSCharacterSet` and `NSMutableCharacterSet`, declare the programmatic interface for [immutable](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectMutability.html#//apple_ref/doc/uid/TP40008195-CH42) and mutable character sets, respectively. An immutable character set is defined when it is created and subsequently cannot be changed. A mutable character set can be changed after it’s created.

A character set object doesn’t perform any tasks; it simply holds a set of character values to limit operations on strings. The `NSString` and `NSScanner` classes define methods that take `NSCharacterSet` objects as arguments to find any of several characters. For example, this code excerpt finds the range of the first uppercase letter in `myString:`.

```
NSString *myString = @"some text in an NSString...";
```

```
NSCharacterSet *characterSet = [NSCharacterSet uppercaseLetterCharacterSet];
```

```
NSRange letterRange = [myString rangeOfCharacterFromSet:characterSet];
```

After this fragment executes, `letterRange.location` is equal to the index of the first “N” in “NSString” after `rangeOfCharacterFromSet:` is invoked. If the first letter of the string were “S”, then `letterRange.location` would be `0`.

## Creating Character Sets
`NSCharacterSet` defines class methods that return commonly used character sets, such as letters (uppercase or lowercase), decimal digits, whitespace, and so on. These “standard” character sets are always immutable, even if created by sending a message to `NSMutableCharacterSet`. See [Standard Character Sets and Unicode Definitions](#//apple_ref/doc/uid/20000153-74241) for more information on standard character sets.

You can use a standard character set as a starting point for building a custom set by making a mutable copy of it and changing that. (You can also start from scratch by creating a mutable character set with `alloc` and `init` and adding characters to it.) For example, this fragment creates a character set containing letters, digits, and basic punctuation:

 
```
NSMutableCharacterSet *workingSet = [[NSCharacterSet alphanumericCharacterSet] mutableCopy];
```

```
[workingSet addCharactersInString:@";:,."];
```

```
NSCharacterSet *finalCharacterSet = [workingSet copy];
```
To define a custom character set using Unicode code points, use code similar to the following fragment (which creates a character set including the form feed and line separator characters):

```
UniChar chars[] = {0x000C, 0x2028};
```

```
NSString *string = [[NSString alloc] initWithCharacters:chars
```

```
                            length:sizeof(chars) / sizeof(UniChar)];
```

```
NSCharacterSet *characterSet = [NSCharacterSet characterSetWithCharactersInString:string];
```
## Performance considerations
Because character sets often participate in performance-critical code, you should be aware of the aspects of their use that can affect the performance of your application. Mutable character sets are generally much more expensive than immutable character sets. They consume more memory and are costly to invert (an operation often performed in scanning a string). Because of this, you should follow these guidelines:

Create as few mutable character sets as possible.

Cache character sets (in a global dictionary, perhaps) instead of continually recreating them.

When creating a custom set that doesn’t need to change after creation, make an immutable copy of the final character set for actual use, and dispose of the working mutable character set. Alternatively, create a character set file as described in [Creating a character set file](#//apple_ref/doc/uid/20000146-SW3) and store it in your application’s main bundle.

Similarly, avoid [archiving](../../../../General/Conceptual/DevPedia-CocoaCore/Archiving.html#//apple_ref/doc/uid/TP40008195-CH1) character set objects; store them in character set files instead. Archiving can result in a character set being duplicated in different archive files, resulting in wasted disk space and duplicates in memory for each separate archive read.

## Creating a character set file
If your application frequently uses a custom character set, you should save its definition in a resource file and load that instead of explicitly adding individual characters each time you need to create the set. You can save a character set by getting its bitmap representation (an `NSData` object) and saving that object to a file:

NSData *charSetRep = [finalCharacterSet bitmapRepresentation];
```
NSURL *dataURL = <#URL for character set#>;
```

```
NSError *error;
```

```
BOOL result = [charSetRep writeToURL:dataURL options:NSDataWritingAtomic error:&error];
```
By convention, character set filenames use the extension `.bitmap`. If you intend for others to use your character set files, you should follow this convention. To read a character set file with a `.bitmap` extension, simply use the `characterSetWithContentsOfFile:` method.

## Standard Character Sets and Unicode Definitions
The standard character sets, such as that returned by `letterCharacterSet`, are formally defined in terms of the normative and informative categories established by the Unicode standard, such as Uppercase Letter, Combining Mark, and so on. The formal definition of a standard character set is in most cases given as one or more of the categories defined in the standard. For example, the set returned by `lowercaseLetterCharacterSet` include all characters in normative category Lowercase Letters, while the set returned by `letterCharacterSet` includes the characters in all of the Letter categories.

Note that the definitions of the categories themselves may change with new versions of the Unicode standard. You can download the files that define category membership from [http://www.unicode.org/](http://www.unicode.org/).

        
            [Next](Scanners.html)[Previous](stringsClusters.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11