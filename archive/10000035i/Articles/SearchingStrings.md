---
title: "Searching, Comparing, and Sorting Strings"
book: "String Programming Guide"
chapterId: "20000149"
date: "2014-02-11"
description: "Explains how to create, search, concatenate, and draw strings in Cocoa."
identifier: "//apple_ref/doc/uid/10000035i"
source: apple-archive
---

# Searching, Comparing, and Sorting Strings

> **String Programming Guide**
> Last updated: 2014-02-11

[Next](stringsParagraphBreaks.html)[Previous](readingFiles.html)
        
        
        [](../index.html)
        
        # Searching, Comparing, and Sorting Strings

The string classes provide methods for finding characters and substrings within strings and for [comparing](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectComparison.html#//apple_ref/doc/uid/TP40008195-CH37) one string to another. These methods conform to the Unicode standard for determining whether two character sequences are equivalent. The string classes provide comparison methods that handle composed character sequences properly, though you do have the option of specifying a literal search when efficiency is important and you can guarantee some canonical form for composed character sequences.

## Search and Comparison Methods

The search and comparison methods each come in several variants. The simplest version of each searches or compares entire strings. Other variants allow you to alter the way comparison of composed character sequences is performed and to specify a specific range of characters within a string to be searched or compared; you can also search and compare strings in the context of a given locale.

These are the basic search and comparison methods:

Search methods

Comparison methods

`[rangeOfString:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/rangeOfString:)`

`[compare:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/compare:)`

`[rangeOfString:options:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/rangeOfString:options:)`

`[compare:options:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/compare:options:)`

`[rangeOfString:options:range:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/rangeOfString:options:range:)`

`[compare:options:range:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/compare:options:range:)`

`[rangeOfString:options:range:locale:](https://developer.apple.com/documentation/foundation/nsstring/1417348-range)`

`[compare:options:range:locale:](https://developer.apple.com/documentation/foundation/nsstring/1414561-compare)`

`[rangeOfCharacterFromSet:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/rangeOfCharacterFromSet:)`

`[rangeOfCharacterFromSet:options:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/rangeOfCharacterFromSet:options:)`

`[rangeOfCharacterFromSet:options:range:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/rangeOfCharacterFromSet:options:range:)`

### Searching strings
You use the `rangeOfString:...` methods to search for a substring within the receiver. The `rangeOfCharacterFromSet:...` methods search for individual characters from a supplied set of characters. 

Substrings are found only if completely contained within the specified range. If you specify a range for a search or comparison method and don’t request `NSLiteralSearch` (see below), the range must not break composed character sequences on either end; if it does, you could get an incorrect result. (See the method description for `[rangeOfComposedCharacterSequenceAtIndex:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/rangeOfComposedCharacterSequenceAtIndex:)` for a code sample that adjusts a range to lie on character sequence boundaries.)

You can also scan a string object for numeric and string values using an instance of `NSScanner`. For more about scanners, see [Scanners](Scanners.html#//apple_ref/doc/uid/20000147-BCIEFGHC). Both the `NSString` and the `NSScanner` class clusters use the `NSCharacterSet` class cluster for search operations. For more about character sets, see [Character Sets](CharacterSets.html#//apple_ref/doc/uid/20000146-BAJBJHCG).

If you simply want to determine whether a string contains a given pattern, you can use a predicate:

BOOL match = [myPredicate evaluateWithObject:myString];For more about predicates, see *[Predicate Programming Guide](../../Predicates/AdditionalChapters/Introduction.html#//apple_ref/doc/uid/TP40001789)*.

### Comparing and sorting strings
The `compare:...` methods return the lexical ordering of the receiver and the supplied string. Several other methods allow you to determine whether two strings are equal or whether one is the prefix or suffix of another, but they don’t have variants that allow you to specify search options or ranges.

The simplest method you can use to compare strings is `[compare:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/compare:)`—this is the same as invoking `[compare:options:range:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/compare:options:range:)` with no options and the receiver’s full extent as the range. If you want to specify comparison options (`NSCaseInsensitiveSearch`, `NSLiteralSearch`, or `NSNumericSearch`) you can use `[compare:options:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/compare:options:)`; if you want to specify a locale you can use `[compare:options:range:locale:](https://developer.apple.com/documentation/foundation/nsstring/1414561-compare)`. `NSString` also provides various convenience methods to allow you to perform common comparisons without the need to specify ranges and options directly, for example `[caseInsensitiveCompare:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/caseInsensitiveCompare:)` and `[localizedCompare:](https://developer.apple.com/documentation/foundation/nsstring/1416999-localizedcompare)`.

> **Important:** For user-visible sorted lists, you should *always* use localized comparisons. Thus typically instead of `compare:` or `caseInsensitiveCompare:` you should use `localizedCompare:` or `localizedCaseInsensitiveCompare:`. 

If you want to compare strings to order them in the same way as they’re presented in Finder, you should use `[compare:options:range:locale:](https://developer.apple.com/documentation/foundation/nsstring/1414561-compare)` with the user’s locale and the following options: `NSCaseInsensitiveSearch`, `NSNumericSearch`, `NSWidthInsensitiveSearch`, and `NSForcedOrderingSearch`. For an example, see [Sorting strings like Finder](#//apple_ref/doc/uid/20000149-SW1).
## Search and Comparison Options
Several of the search and comparison methods take an “options” argument. This is a bit mask that adds further constraints to the operation. You create the mask by combining the following options (not all options are available for every method):

Search option

Effect

`[NSCaseInsensitiveSearch](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/econst/NSCaseInsensitiveSearch)`

Ignores case distinctions among characters.

`[NSLiteralSearch](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/econst/NSLiteralSearch)`

Performs a byte-for-byte comparison. Differing literal sequences (such as composed character sequences) that would otherwise be considered equivalent are considered not to match. Using this option can speed some operations dramatically.

`[NSBackwardsSearch](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/econst/NSBackwardsSearch)`

Performs searching from the end of the range toward the beginning.

`[NSAnchoredSearch](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/econst/NSAnchoredSearch)`

Performs searching only on characters at the beginning or, if `NSBackwardsSearch` is also specified, the end of the range. No match at the beginning or end means nothing is found, even if a matching sequence of characters occurs elsewhere in the string.

`[NSNumericSearch](https://developer.apple.com/documentation/foundation/nsstringcompareoptions/nsnumericsearch)`

When used with the `compare:options:` methods, groups of numbers are treated as a numeric value for the purpose of comparison. For example, `Filename9.txt` < `Filename20.txt` < `Filename100.txt`.

Search and comparison are currently performed as if the `NSLiteralSearch` option were specified. 

## Examples
### Case-Insensitive Search for Prefix and Suffix
`NSString` provides the methods `[hasPrefix:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/hasPrefix:)` and `[hasSuffix:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/hasSuffix:)` that you can use to find an *exact* match for a prefix or suffix. The following example illustrates how you can use `[rangeOfString:options:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/rangeOfString:options:)` with a combination of options to perform *case insensitive* searches.

```
NSString *searchString = @"age";
```

```
 
```

```
NSString *beginsTest = @"Agencies";
```

```
NSRange prefixRange = [beginsTest rangeOfString:searchString
```

```
    options:(NSAnchoredSearch | NSCaseInsensitiveSearch)];
```

```
 
```

```
// prefixRange = {0, 3}
```

```
 
```

```
NSString *endsTest = @"BRICOLAGE";
```

```
NSRange suffixRange = [endsTest rangeOfString:searchString
```

```
    options:(NSAnchoredSearch | NSCaseInsensitiveSearch | NSBackwardsSearch)];
```

```
 
```

```
// suffixRange = {6, 3}
```
### Comparing Strings
The following examples illustrate the use of various string comparison methods and associated options. The first shows the simplest comparison method.

NSString *string1 = @"string1";
```
NSString *string2 = @"string2";
```

```
NSComparisonResult result;
```

```
result = [string1 compare:string2];
```

```
// result = -1 (NSOrderedAscending)
```
You can compare strings numerically using the `NSNumericSearch` option:

NSString *string10 = @"string10";
```
NSString *string2 = @"string2";
```

```
NSComparisonResult result;
```

```
 
```

```
result = [string10 compare:string2];
```

```
// result = -1 (NSOrderedAscending)
```

```
 
```

```
result = [string10 compare:string2 options:NSNumericSearch];
```

```
// result = 1 (NSOrderedDescending)
```
You can use convenience methods (`[caseInsensitiveCompare:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/caseInsensitiveCompare:)` and `[localizedCaseInsensitiveCompare:](https://developer.apple.com/documentation/foundation/nsstring/1417333-localizedcaseinsensitivecompare)`) to perform case-insensitive comparisons:

```
NSString *string_a = @"Aardvark";
```

```
NSString *string_A = @"AARDVARK";
```

```
 
```

```
result = [string_a compare:string_A];
```

```
// result = 1 (NSOrderedDescending)
```

```
 
```

```
result = [string_a caseInsensitiveCompare:string_A];
```

```
// result = 0 (NSOrderedSame)
```

```
// equivalent to [string_a compare:string_A options:NSCaseInsensitiveSearch]
```
### Sorting strings like Finder
To sort strings the way Finder does in OS X v10.6 and later, use the `[localizedStandardCompare:](https://developer.apple.com/documentation/foundation/nsstring/1409742-localizedstandardcompare)` method. It should be used whenever file names or other strings are presented in lists and tables where Finder-like sorting is appropriate.  The exact behavior of this method is different under different localizations, so clients should not depend on the exact sorting order of the strings.

The following example shows another implementation of similar functionality, comparing strings to order them in the same way as they’re presented in Finder, and it also shows how to sort the array of strings. First, define a sorting function that includes the relevant comparison options (for efficiency, pass the user's locale as the context—this way it's only looked up once).

int finderSortWithLocale(id string1, id string2, void *locale)
```
{
```

```
    static NSStringCompareOptions comparisonOptions =
```

```
        NSCaseInsensitiveSearch | NSNumericSearch |
```

```
        NSWidthInsensitiveSearch | NSForcedOrderingSearch;
```

```
 
```

```
    NSRange string1Range = NSMakeRange(0, [string1 length]);
```

```
 
```

```
    return [string1 compare:string2
```

```
                    options:comparisonOptions
```

```
                    range:string1Range
```

```
                    locale:(NSLocale *)locale];
```

```
}
```
You pass the function as a parameter to `[sortedArrayUsingFunction:context:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSArray/sortedArrayUsingFunction:context:)` with the user’s current locale as the context:

```
NSArray *stringsArray = @[@"string 1",
```

```
                          @"String 21",
```

```
                          @"string 12",
```

```
                          @"String 11",
```

```
                          @"String 02"];
```

```
 
```

```
NSArray *sortedArray = [stringsArray sortedArrayUsingFunction:finderSortWithLocale
```

```
                                     context:[NSLocale currentLocale]];
```

```
 
```

```
// sortedArray contains { "string 1", "String 02", "String 11", "string 12", "String 21" }
```

        
            [Next](stringsParagraphBreaks.html)[Previous](readingFiles.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11