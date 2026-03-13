---
title: "Using NSNull"
book: "Number and Value Programming Topics"
chapterId: "TP40005153"
date: "2008-02-08"
description: "Explains how to use Cocoa object wrappers for primitive C data types."
identifier: "//apple_ref/doc/uid/10000038i"
source: apple-archive
---

# Using NSNull

> **Number and Value Programming Topics**
> Last updated: 2008-02-08

[Next](../RevisionHistory.html)[Previous](DecimalNumbers.html)
        
        
        [](../index.html)
        
        # Using NSNull
The `NSNull` class defines a singleton object you use to represent null values in situations where `nil` is prohibited as a value (typically in a collection object such as an array or a dictionary).

NSNull *nullValue = [NSNull null];
```
NSArray *arrayWithNull = @[nullValue];
```

```
NSLog(@"arrayWithNull: %@", arrayWithNull);
```

```
// Output: "arrayWithNull: (<null>)"
```
It is important to appreciate that the `NSNull` instance is semantically different from `NO` or `false`—these both represent a logical value; the `NSNull` instance represents the absence of a value. The `NSNull` instance is semantically equivalent to `nil`, however it is also important to appreciate that it is not equal to `nil`. To test for a null object value, you must therefore make a direct object comparison.

```
id aValue = [arrayWithNull objectAtIndex:0];
```

```
if (aValue == nil) {
```

```
    NSLog(@"equals nil");
```

```
}
```

```
else if (aValue == [NSNull null]) {
```

```
    NSLog(@"equals NSNull instance");
```

```
    if ([aValue isEqual:nil]) {
```

```
        NSLog(@"isEqual:nil");
```

```
    }
```

```
}
```

```
// Output: "equals NSNull instance"
```

        
            [Next](../RevisionHistory.html)[Previous](DecimalNumbers.html)
        
         Copyright &#x00a9; 2008 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2008-02-08