---
title: "Scanners"
book: "String Programming Guide"
chapterId: "20000147"
date: "2014-02-11"
description: "Explains how to create, search, concatenate, and draw strings in Cocoa."
identifier: "//apple_ref/doc/uid/10000035i"
source: apple-archive
---

# Scanners

> **String Programming Guide**
> Last updated: 2014-02-11

[Next](ManipulatingPaths.html)[Previous](CharacterSets.html)
        
        
        [](../index.html)
        
        # Scanners

An `NSScanner` object scans the characters of an `NSString` object, typically interpreting the characters and converting them into number and string values. You assign the scanner’s string on creation, and the scanner progresses through the characters of that string from beginning to end as you request items. 

## Creating a Scanner

`NSScanner` is a [class cluster](../../../../General/Conceptual/DevPedia-CocoaCore/ClassCluster.html#//apple_ref/doc/uid/TP40008195-CH7) with a single public class, `[NSScanner](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSScannerClassCluster/Description.html#//apple_ref/occ/cl/NSScanner)`. Generally, you instantiate a scanner object by invoking the [class method](../../../../General/Conceptual/DevPedia-CocoaCore/ClassMethod.html#//apple_ref/doc/uid/TP40008195-CH8) `[scannerWithString:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSScannerClassCluster/Description.html#//apple_ref/occ/clm/NSScanner/scannerWithString:)` or `[localizedScannerWithString:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSScannerClassCluster/Description.html#//apple_ref/occ/clm/NSScanner/localizedScannerWithString:)`. Either method returns a scanner object initialized with the string you pass to it. The newly created scanner starts at the beginning of its string. You scan components using the `scan...` methods such as `[scanInt:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSScannerClassCluster/Description.html#//apple_ref/occ/instm/NSScanner/scanInt:)`, `[scanDouble:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSScannerClassCluster/Description.html#//apple_ref/occ/instm/NSScanner/scanDouble:)`, and `[scanString:intoString:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSScannerClassCluster/Description.html#//apple_ref/occ/instm/NSScanner/scanString:intoString:)`. If you are scanning multiple lines, you typically create a `while` loop that continues until the scanner is at the end of the string, as illustrated in the following code fragment: 

float aFloat;
```
NSScanner *theScanner = [NSScanner scannerWithString:aString];
```

```
while ([theScanner isAtEnd] == NO) {
```

```
 
```

```
    [theScanner scanFloat:&aFloat];
```

```
    // implementation continues...
```

```
}
```
You can configure a scanner to consider or ignore case using the `[setCaseSensitive:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSScannerClassCluster/Description.html#//apple_ref/occ/instm/NSScanner/setCaseSensitive:)` method. By default a scanner ignores case. 

## Using a Scanner
Scan operations start at the scan location and advance the scanner to just past the last character in the scanned value representation (if any). For example, after scanning an integer from the string “`137 small cases of bananas`”, a scanner’s location will be 3, indicating the space immediately after the number. Often you need to advance the scan location to skip characters in which you are not interested. You can change the implicit scan location with the `[setScanLocation:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSScannerClassCluster/Description.html#//apple_ref/occ/instm/NSScanner/setScanLocation:)` method to skip ahead a certain number of characters (you can also use the method to rescan a portion of the string after an error). Typically, however, you either want to skip characters from a particular character set, scan past a specific string, or scan up to a specific string.

You can configure a scanner to skip a set of characters with the `[setCharactersToBeSkipped:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSScannerClassCluster/Description.html#//apple_ref/occ/instm/NSScanner/setCharactersToBeSkipped:)` method. A scanner ignores characters to be skipped at the beginning of any scan operation. Once it finds a scannable character, however, it includes all characters matching the request. Scanners skip whitespace and newline characters by default. Note that case is always considered with regard to characters to be skipped. To skip all English vowels, for example, you must set the characters to be skipped to those in the string “AEIOUaeiou”.

If you want to read content from the current location up to a particular string, you can use `[scanUpToString:intoString:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSScannerClassCluster/Description.html#//apple_ref/occ/instm/NSScanner/scanUpToString:intoString:)` (you can pass `NULL` as the second argument if you simply want to skip the intervening characters). For example, given the following string:

137 small cases of bananasyou can find the type of container and number of containers using `[scanUpToString:intoString:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSScannerClassCluster/Description.html#//apple_ref/occ/instm/NSScanner/scanUpToString:intoString:)` as shown in the following example.

NSString *bananas = @"137 small cases of bananas";
```
NSString *separatorString = @" of";
```

```
 
```

```
NSScanner *aScanner = [NSScanner scannerWithString:bananas];
```

```
 
```

```
NSInteger anInteger;
```

```
[aScanner scanInteger:&anInteger];
```

```
NSString *container;
```

```
[aScanner scanUpToString:separatorString intoString:&container];
```
It is important to note that the search string (`separatorString`) is `" of"`. By default a scanner ignores whitespace, so the space character after the integer is ignored. Once the scanner begins to accumulate characters, however, all characters are added to the output string until the search string is reached. Thus if the search string is `"of"` (no space before), the first value of `container` is “small cases ” (includes the space following); if the search string is `" of"` (with a space before), the first value of `container` is “small cases” (no space following). 

After scanning up to a given string, the scan location is the beginning of that string. If you want to scan past that string, you must therefore first scan in the string you scanned up to. The following code fragment illustrates how to skip past the search string in the previous example and determine the type of product in the container. Note the use of `[substringFromIndex:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/substringFromIndex:)` to in effect scan up to the end of a string.

```
[aScanner scanString:separatorString intoString:NULL];
```

```
NSString *product;
```

```
product = [[aScanner string] substringFromIndex:[aScanner scanLocation]];
```

```
// could also use:
```

```
// product = [bananas substringFromIndex:[aScanner scanLocation]];
```
## Example

Suppose you have a string containing lines such as:

Product: Acme Potato Peeler; Cost: 0.98 73

Product: Chef Pierre Pasta Fork; Cost: 0.75 19

Product: Chef Pierre Colander; Cost: 1.27 2

The following example uses alternating scan operations to extract the product names and costs (costs are read as a `float` for simplicity’s sake), skipping the expected substrings “Product:” and “Cost:”, as well as the semicolon. Note that because a scanner skips whitespace and newlines by default, the loop does no special processing for them (in particular there is no need to do additional whitespace processing to retrieve the final integer).

```
NSString *string = @"Product: Acme Potato Peeler; Cost: 0.98 73\n\
```

```
Product: Chef Pierre Pasta Fork; Cost: 0.75 19\n\
```

```
Product: Chef Pierre Colander; Cost: 1.27 2\n";
```

```
 
```

```
NSCharacterSet *semicolonSet;
```

```
NSScanner *theScanner;
```

```
 
```

```
NSString *PRODUCT = @"Product:";
```

```
NSString *COST = @"Cost:";
```

```
 
```

```
NSString *productName;
```

```
float productCost;
```

```
NSInteger productSold;
```

```
 
```

```
semicolonSet = [NSCharacterSet characterSetWithCharactersInString:@";"];
```

```
theScanner = [NSScanner scannerWithString:string];
```

```
 
```

```
while ([theScanner isAtEnd] == NO)
```

```
{
```

```
    if ([theScanner scanString:PRODUCT intoString:NULL] &&
```

```
        [theScanner scanUpToCharactersFromSet:semicolonSet
```

```
            intoString:&productName] &&
```

```
        [theScanner scanString:@";" intoString:NULL] &&
```

```
        [theScanner scanString:COST intoString:NULL] &&
```

```
        [theScanner scanFloat:&productCost] &&
```

```
        [theScanner scanInteger:&productSold])
```

```
    {
```

```
        NSLog(@"Sales of %@: $%1.2f", productName, productCost * productSold);
```

```
    }
```

```
}
```
## Localization

A scanner bases some of its scanning behavior on a [locale](../../../../General/Conceptual/DevPedia-CocoaCore/Internationalization.html#//apple_ref/doc/uid/TP40008195-CH23), which specifies a language and conventions for value representations. `NSScanner` uses only the locale’s definition for the decimal separator (given by the key named `NSDecimalSeparator`). You can create a scanner with the user’s locale by using `[localizedScannerWithString:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSScannerClassCluster/Description.html#//apple_ref/occ/clm/NSScanner/localizedScannerWithString:)`, or set the locale explicitly using `[setLocale:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSScannerClassCluster/Description.html#//apple_ref/occ/instm/NSScanner/setLocale:)`. If you use a method that doesn’t specify a locale, the scanner assumes the default locale values.

        
            [Next](ManipulatingPaths.html)[Previous](CharacterSets.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11