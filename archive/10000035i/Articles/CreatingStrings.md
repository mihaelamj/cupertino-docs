---
title: "Creating and Converting String Objects"
book: "String Programming Guide"
chapterId: "20000148"
date: "2014-02-11"
description: "Explains how to create, search, concatenate, and draw strings in Cocoa."
identifier: "//apple_ref/doc/uid/10000035i"
source: apple-archive
---

# Creating and Converting String Objects

> **String Programming Guide**
> Last updated: 2014-02-11

[Next](FormatStrings.html)[Previous](Strings.html)
        
        
        [](../index.html)
        
        # Creating and Converting String Objects

`NSString` and its subclass `NSMutableString` provide several ways to create string objects, most based around the various character encodings it supports. Although string objects always present their own contents as Unicode characters, they can convert their contents to and from many other encodings, such as 7-bit ASCII, ISO Latin 1, EUC, and Shift-JIS. The `[availableStringEncodings](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/clm/NSString/availableStringEncodings)` class method returns the encodings supported. You can specify an encoding explicitly when converting a C string to or from a string object, or use the default C string encoding, which varies from platform to platform and is returned by the `defaultCStringEncoding` class method.

## Creating Strings

The simplest way to create a string object in source code is to use the [Objective-C](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectiveC.html#//apple_ref/doc/uid/TP40008195-CH43)  `@"..."` construct:

NSString *temp = @"Contrafibularity";Note, when creating a string constant in this fashion, you should use UTF-8 characters. You can put the actual Unicode data in the string for localized text, as in `NSString *hello = @"こんにちは";`. You can also insert `\u` or `\U` in the string for non-graphic characters.

Objective-C string constant is created at compile time and exists throughout your program’s execution. The compiler makes such object constants unique on a per-module basis, and they’re never deallocated. You can also send [messages](../../../../General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59) directly to a string constant as you do any other string:

```
BOOL same = [@"comparison" isEqualToString:myString];
```
### NSString from C Strings and Data

To [create](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39) an `NSString` object from a C string, you use methods such as `[initWithCString:encoding:](https://developer.apple.com/documentation/foundation/nsstring/1411950-init)`. You must correctly specify the character encoding of the C string. Similar methods allow you to create string objects from characters in a variety of encodings. The method `initWithData:encoding:` allows you to convert string data stored in an `NSData` object into an `NSString` object.

char *utf8String = /* Assume this exists. */ ;
```
NSString *stringFromUTFString = [[NSString alloc] initWithUTF8String:utf8String];
```

```
 
```

```
char *macOSRomanEncodedString = /* assume this exists */ ;
```

```
NSString *stringFromMORString =
```

```
            [[NSString alloc] initWithCString:macOSRomanEncodedString
```

```
                              encoding:NSMacOSRomanStringEncoding];
```

```
 
```

```
NSData *shiftJISData =  /* assume this exists */ ;
```

```
NSString *stringFromShiftJISData =
```

```
            [[NSString alloc] initWithData:shiftJISData
```

```
                              encoding:NSShiftJISStringEncoding];
```
The following example converts an `NSString` object containing a UTF-8 character to ASCII data then back to an `NSString` object.

unichar ellipsis = 0x2026;
```
NSString *theString = [NSString stringWithFormat:@"To be continued%C", ellipsis];
```

```
 
```

```
NSData *asciiData = [theString dataUsingEncoding:NSASCIIStringEncoding allowLossyConversion:YES];
```

```
 
```

```
NSString *asciiString = [[NSString alloc] initWithData:asciiData encoding:NSASCIIStringEncoding];
```

```
 
```

```
NSLog(@"Original: %@ (length %d)", theString, [theString length]);
```

```
NSLog(@"Converted: %@ (length %d)", asciiString, [asciiString length]);
```

```
 
```

```
// output:
```

```
// Original: To be continued… (length 16)
```

```
// Converted: To be continued... (length 18)
```

> **Note:** `NSString` is not intended as a data storage container for arbitrary sequences of bytes. For this type of functionality, refer to `[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)`.

### Variable Strings
To create a variable string, you typically use `[stringWithFormat:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/clm/NSString/stringWithFormat:)`: or `[initWithFormat:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/initWithFormat:)` (or for localized strings, `[localizedStringWithFormat:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/clm/NSString/localizedStringWithFormat:)`). These methods and their siblings use a format string as a template into which the values you provide (string and other objects, numerics values, and so on) are inserted. They and the supported format specifiers are described in [Formatting String Objects](FormatStrings.html#//apple_ref/doc/uid/20000943-CJBEAEHH).

You can build a string from existing string objects using the methods `[stringByAppendingString:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/stringByAppendingString:)` and `[stringByAppendingFormat:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/stringByAppendingFormat:)` to create a new string by adding one string after another, in the second case using a format string.

```
NSString *hString = @"Hello";
```

```
NSString *hwString = [hString stringByAppendingString:@", world!"];
```
### Strings to Present to the User
When creating strings to present to the user, you should consider the importance of [localizing](../../../../General/Conceptual/DevPedia-CocoaCore/Internationalization.html#//apple_ref/doc/uid/TP40008195-CH23) your application. In general, you should avoid creating user-visible strings directly in code. Instead you should use strings in your code as a key to a localization dictionary that will supply the user-visible string in the user's preferred language. Typically this involves using `[NSLocalizedString](https://developer.apple.com/documentation/foundation/nslocalizedstring)` and similar macros, as illustrated in the following example.

NSString *greeting = NSLocalizedStringFromTable
```
    (@"Hello", @"greeting to present in first launch panel", @"greetings");
```
For more about internationalizing your application, see *[Internationalization and Localization Guide](../../../../MacOSX/Conceptual/BPInternational/Introduction/Introduction.html#//apple_ref/doc/uid/10000171i)*. Localizing String Resources describes how to work with and reorder variable arguments in localized strings. 

## Combining and Extracting Strings
You can combine and extract strings in various ways. The simplest way to combine two strings is to append one to the other. The `[stringByAppendingString:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/stringByAppendingString:)` method returns a string object formed from the receiver and the given argument.

NSString *beginning = @"beginning";
```
NSString *alphaAndOmega = [beginning stringByAppendingString:@" and end"];
```

```
// alphaAndOmega is @"beginning and end"
```
You can also combine several strings according to a template with the `[initWithFormat:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/initWithFormat:)`, `[stringWithFormat:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/clm/NSString/stringWithFormat:)`, and `[stringByAppendingFormat:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/stringByAppendingFormat:)` methods; these are described in more detail in [Formatting String Objects](FormatStrings.html#//apple_ref/doc/uid/20000943-CJBEAEHH).

You can extract substrings from the beginning or end of a string to a particular index, or from a specific range, with the `[substringToIndex:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/substringToIndex:)`, `[substringFromIndex:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/substringFromIndex:)`, and `[substringWithRange:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/substringWithRange:)` methods. You can also split a string into substrings (based on a separator string) with the `[componentsSeparatedByString:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/componentsSeparatedByString:)` method. These methods are illustrated in the following examples—notice that the index of the index-based methods starts at `0`:

NSString *source = @"0123456789";
```
NSString *firstFour = [source substringToIndex:4];
```

```
// firstFour is @"0123"
```

```
 
```

```
NSString *allButFirstThree = [source substringFromIndex:3];
```

```
// allButFirstThree is @"3456789"
```

```
 
```

```
NSRange twoToSixRange = NSMakeRange(2, 4);
```

```
NSString *twoToSix = [source substringWithRange:twoToSixRange];
```

```
// twoToSix is @"2345"
```

```
 
```

```
NSArray *split = [source componentsSeparatedByString:@"45"];
```

```
// split contains { @"0123", @"6789" }
```
If you need to extract strings using pattern-matching rather than an index, you should use a scanner—see [Scanners](Scanners.html#//apple_ref/doc/uid/20000147-BCIEFGHC).

## Getting C Strings

To get a C string from a string object, you are recommended to use `UTF8String`. This returns a `const char *` using UTF8 string encoding.

const char *cString = [@"Hello, world" UTF8String];The C string you receive is owned by a temporary object, and will become invalid when automatic deallocation takes place. If you want to get a permanent C string, you must create a buffer and copy the contents of the `const char *` returned by the method.

Similar methods allow you to create string objects from characters in the Unicode encoding or an arbitrary encoding, and to extract data in these encodings. `initWithData:encoding:` and `dataUsingEncoding:` perform these conversions from and to `NSData` objects.

## Conversion Summary

This table summarizes the most common means of creating and converting string objects:

Source

Creation method

Extraction method

In code

`@"..."` compiler construct

N/A

UTF8 encoding

`[stringWithUTF8String:](https://developer.apple.com/documentation/foundation/nsstring/1497379-stringwithutf8string)`

`[UTF8String](https://developer.apple.com/documentation/foundation/nsstring/1411189-utf8string)`

Unicode encoding

`[stringWithCharacters:length:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/clm/NSString/stringWithCharacters:length:)`

`[getCharacters:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/getCharacters:)`

`[getCharacters:range:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/getCharacters:range:)`

Arbitrary encoding

`[initWithData:encoding:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/initWithData:encoding:)`

`[dataUsingEncoding:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/dataUsingEncoding:)`

Existing strings

`[stringByAppendingString:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/stringByAppendingString:)`

`[stringByAppendingFormat:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/stringByAppendingFormat:)`

N/A

Format string

`[localizedStringWithFormat:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/clm/NSString/localizedStringWithFormat:)`

`[initWithFormat:locale:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/initWithFormat:locale:)`

Use `NSScanner`

Localized strings

`[NSLocalizedString](https://developer.apple.com/documentation/foundation/nslocalizedstring)` and similar

N/A

        
            [Next](FormatStrings.html)[Previous](Strings.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11