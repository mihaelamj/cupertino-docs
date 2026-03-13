---
title: "Comparison of NSPredicate and Spotlight Query Strings"
book: "Predicate Programming Guide"
chapterId: "TP40002370"
date: "2014-09-17"
description: "Describes how to specify queries in Cocoa."
identifier: "//apple_ref/doc/uid/TP40001789"
source: apple-archive
---

# Comparison of NSPredicate and Spotlight Query Strings

> **Predicate Programming Guide**
> Last updated: 2014-09-17

[Next](pSyntax.html)[Previous](pUsing.html)
        
        
        [](../index.html)
        
        # Comparison of NSPredicate and Spotlight Query Strings
Both Spotlight and the `NSPredicate` class implement a query string syntax, and though they are similar, they differ in several respects. One query string can be converted into the other string form, as long as the common subset of functionality is used.

## Spotlight and NSPredicate
The `NSMetadataQuery` class, which is the Cocoa interface to Spotlight, uses `NSPredicate` in its API. Apart from this, there is no relationship between Spotlight and `NSPredicate`. The Spotlight query string syntax is similar to, but different from, the `NSPredicate` query string syntax. You can convert one query string into the other string form, as long as you use syntax that both APIs understand. Spotlight's query syntax is a subset of that of `NSPredicate`. For a complete description of the Spotlight query expression syntax, see [File Metadata Query Expression Syntax](../../../../Carbon/Conceptual/SpotlightQuery/Concepts/QueryFormat.html#//apple_ref/doc/uid/TP40001849), and for a complete description of the `NSPredicate` string syntax, see [Predicate Format String Syntax](pSyntax.html#//apple_ref/doc/uid/TP40001795-SW1).

Spotlight requires comparison clauses to take the form `"KEY operator VALUE"` and does not accept `"VALUE operator KEY"`. Moreover, the kinds of attribute Spotlight accepts for `VALUE` are more limited than those accepted by `NSPredicate`. As a partial consequence of this limitation, you do not always have to quote literal strings in Spotlight queries. You can omit the quotes when `VALUE` is a string and no special operators need to be applied to it. You cannot do this with an `NSPredicate` query string, as the result would be ambiguous.

The syntax for denoting case- and diacritic-insensitivity for queries in Spotlight is different from the `NSPredicate` version. In Spotlight, you append markers to the end of the comparison string (for example, `"myAttribute == 'foo'cd"`). In `NSPredicate` strings, you use the `like` operator and prefix the markers within "[]"s (for example, `"myAttribute like[cd] 'foo'"`). In both cases, `'cd'` means case-insensitive and diacritic-insensitive. Spotlight puts the modifiers on the value, `NSPredicate` puts the modifiers on the operator.

You cannot use an `MDQuery` operator as the `VALUE` of an `NSPredicate` object `"KEY operator VALUE"` string. For example, you write an “is-substring-of” expression in Spotlight like this: `"myAttribute = '*foo*'"`; in `NSPredicate` strings you use the `contains` operator, like this: `"myAttribute contains 'foo'"`. Spotlight takes glob-like expressions, `NSPredicate` uses a different operator.

If you use “`*`” as left-hand-side key in a comparison expression, in Spotlight it means “any key in the item” and can only be used with `==`.  You could only use this expression in an `NSPredicate` object in conjunction with an `NSMetadataQuery` object.  

## Creating a Predicate Format String From a Spotlight Search in Finder
You can create a predicate format string from a search in Finder. Perform a search, save it, then select the folder where you saved it and choose Show Info—the Info panel shows the query that is used by Spotlight. Note however, that there are slight differences between the `NSPredicate` format string and the one stored in Finder. The Finder string might look like the following example.

(((* = "FooBar*"wcd) || (kMDItemTextContent = "FooBar*"cd))
```
    && (kMDItemContentType != com.apple.mail.emlx)
```

```
    && (kMDItemContentType != public.vcard))
```
Typically all you have to do to convert the Spotlight query into a predicate format string is make sure the predicate does not start with `*` (this is not supported by `NSMetadataQuery` when parsing a predicate). In addition, when you want to use a wildcard, you should use `LIKE`, as shown in the following example.

```
((kMDItemTextContent LIKE[cd] "FooBar")
```

```
    && (kMDItemContentType != "com.apple.mail.emlx")
```

```
    && (kMDItemContentType != "public.vcard"))
```

        
            [Next](pSyntax.html)[Previous](pUsing.html)
        
         Copyright &#x00a9; 2005, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-09-17