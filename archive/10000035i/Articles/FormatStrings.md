---
title: "Formatting String Objects"
book: "String Programming Guide"
chapterId: "20000943"
date: "2014-02-11"
description: "Explains how to create, search, concatenate, and draw strings in Cocoa."
identifier: "//apple_ref/doc/uid/10000035i"
source: apple-archive
---

# Formatting String Objects

> **String Programming Guide**
> Last updated: 2014-02-11

[Next](formatSpecifiers.html)[Previous](CreatingStrings.html)
        
        
        [](../index.html)
        
        # Formatting String Objects
This article describes how to create a string using a format string, how to use non-ASCII characters in a format string, and a common error that developers make when using `NSLog` or `NSLogv`. 

## Formatting Basics

`NSString` uses a format string whose syntax is similar to that used by other formatter objects. It supports the format characters defined for the ANSI C function `printf()`, plus `%@` for any object (see [String Format Specifiers](formatSpecifiers.html#//apple_ref/doc/uid/TP40004265-SW1) and the [IEEE printf specification](http://www.opengroup.org/onlinepubs/009695399/functions/printf.html)). If the object responds to `descriptionWithLocale:` messages, `NSString` sends such a message to retrieve the text representation. Otherwise, it sends a `description` message. Localizing String Resources describes how to work with and reorder variable arguments in localized strings.

In format strings, a ‘`%`’ character announces a placeholder for a value, with the characters that follow determining the kind of value expected and how to format it. For example, a format string of `"%d houses"` expects an integer value to be substituted for the format expression '`%d`'. `NSString` supports the format characters defined for the ANSI C function`printf()`, plus ‘`@`’ for any object. If the object responds to the `descriptionWithLocale:` message, `NSString` sends that message to retrieve the text representation, otherwise, it sends a `description` message.

Value formatting is affected by the user’s current locale, which is an `NSDictionary` object that specifies number, date, and other kinds of formats. `NSString` uses only the locale’s definition for the decimal separator (given by the key named `NSDecimalSeparator`). If you use a method that doesn’t specify a locale, the string assumes the default locale.

You can use `NSString`’s  `[stringWithFormat:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/clm/NSString/stringWithFormat:)` method and other related methods to create strings with `printf`-style format specifiers and argument lists, as described in [Creating and Converting String Objects](CreatingStrings.html#//apple_ref/doc/uid/20000148-CJBCJHHI). The examples below illustrate how you can create a string using a variety of format specifiers and arguments.

```
NSString *string1 = [NSString stringWithFormat:@"A string: %@, a float: %1.2f",
```

```
                                               @"string", 31415.9265];
```

```
// string1 is "A string: string, a float: 31415.93"
```

```
 
```

```
NSNumber *number = @1234;
```

```
NSDictionary *dictionary = @{@"date": [NSDate date]};
```

```
NSString *baseString = @"Base string.";
```

```
NSString *string2 = [baseString stringByAppendingFormat:
```

```
        @" A number: %@, a dictionary: %@", number, dictionary];
```

```
// string2 is "Base string. A number: 1234, a dictionary: {date = 2005-10-17 09:02:01 -0700; }"
```
## Strings and Non-ASCII Characters
You can include non-ASCII characters (including Unicode) in strings using methods such as `stringWithFormat:` and `stringWithUTF8String:`.

NSString *s = [NSString stringWithFormat:@"Long %C dash", 0x2014];Since `\xe2\x80\x94` is the 3-byte UTF-8 string for  `0x2014`, you could also write:

```
NSString *s = [NSString stringWithUTF8String:"Long \xe2\x80\x94   dash"];
```
## NSLog and NSLogv
The utility functions `NSLog()` and `NSLogv()` use the `NSString` string formatting services to log error messages. Note that as a consequence of this, you should take care when specifying the argument for these functions. A common mistake is to specify a string that includes formatting characters, as shown in the following example.

NSString *string = @"A contrived string %@";
```
NSLog(string);
```

```
// The application will probably crash here due to signal 10 (SIGBUS)
```
It is better (safer) to use a format string to output another string, as shown in the following example. 

```
NSString *string = @"A contrived string %@";
```

```
NSLog(@"%@", string);
```

```
// Output: A contrived string %@
```

        
            [Next](formatSpecifiers.html)[Previous](CreatingStrings.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11