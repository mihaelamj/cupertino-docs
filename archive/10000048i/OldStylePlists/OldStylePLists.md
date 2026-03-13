---
title: "Appendix A: Old-Style ASCII Property Lists"
book: "Property List Programming Guide"
chapterId: "20001012"
date: "2010-03-24"
description: "Explains how to use structured, textual representations of data in Cocoa."
identifier: "//apple_ref/doc/uid/10000048i"
source: apple-archive
---

# Appendix A: Old-Style ASCII Property Lists

> **Property List Programming Guide**
> Last updated: 2010-03-24

[Next](../RevisionHistory.html)[Previous](../ReadWritePlistData/ReadWritePlistData.html)
        
        
        [](../index.html)
        
        # Old-Style ASCII Property Lists

The OpenStep frameworks, which Cocoa is derived from, used an ASCII format for storing property lists. These files store information equivalent to the information in XML property lists, and are still supported by Cocoa, but for reading only. Old-style plist support remains primarily for legacy reasons. You read old-style property lists by calling the `[propertyListFromData:mutabilityOption:format:errorDescription:](https://developer.apple.com/documentation/foundation/propertylistserialization/1411993-propertylistfromdata)` method of `[NSPropertyListSerialization](https://developer.apple.com/documentation/foundation/nspropertylistserialization)`.

ASCII property lists support the four primary property list data types: NSString, NSData, NSArray, and NSDictionary. The following sections describe the ASCII syntax for each of these types.

> **Important:** Cocoa allows you to read old-style ASCII property lists only. You may not write them.

## NSString

A string is enclosed in double quotation marks, for example:

```
"This is a string"
```

The quotation marks can be omitted if the string is composed strictly of alphanumeric characters and contains no white space (numbers are handled as strings in property lists). Though the property list format uses ASCII for strings, note that Cocoa uses Unicode. Since string encodings vary from region to region, this representation makes the format fragile. You may see strings containing unreadable sequences of ASCII characters; these are used to represent Unicode characters.

## NSData

Binary data is enclosed in angle brackets and encoded in hexadecimal ASCII. Spaces are ignored. For example:

```
<0fbd777 1c2735ae>
```

## NSArray

An array is enclosed in parentheses, with the elements separated by commas. For example:

```
("San Francisco", "New York", "Seoul", "London", "Seattle", "Shanghai")
```

The items don’t all have to be of the same type (for example, all strings)—but they normally are. Arrays can contain strings, binary data, other arrays, or dictionaries.

## NSDictionary

A dictionary is enclosed in curly braces, and contains a list of keys with their values. Each key-value pair ends with a semicolon. For example: 

```
{ user = wshakesp; birth = 1564; death = 1616; }
```

Note the omission of quotation marks for single-word alphanumeric strings. Values don’t all have to be the same type, because their types are usually defined by whatever program uses them. Dictionaries can contain strings, binary data, arrays, and other dictionaries.

Below is a sample of a more complex property list. The property list itself is a dictionary with keys “AnimalSmells,” “AnimalSounds,” and so on; each value is also a dictionary, with key-value pairs.

```
{
```

```
    AnimalSmells = { pig = piggish; lamb = lambish; worm = wormy; };
```

```
    AnimalSounds = { pig = oink; lamb = baa; worm = baa;
```

```
                    Lisa = "Why is the worm talking like a lamb?"; };
```

```
    AnimalColors = { pig = pink; lamb = black; worm = pink; };
```

```
}
```

        
            [Next](../RevisionHistory.html)[Previous](../ReadWritePlistData/ReadWritePlistData.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-03-24