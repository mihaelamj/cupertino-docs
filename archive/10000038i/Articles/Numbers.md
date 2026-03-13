---
title: "Using Numbers"
book: "Number and Value Programming Topics"
chapterId: "20000175"
date: "2008-02-08"
description: "Explains how to use Cocoa object wrappers for primitive C data types."
identifier: "//apple_ref/doc/uid/10000038i"
source: apple-archive
---

# Using Numbers

> **Number and Value Programming Topics**
> Last updated: 2008-02-08

[Next](DecimalNumbers.html)[Previous](Values.html)
        
        
        [](../index.html)
        
        # Using Numbers

`NSNumber` is a subclass of `NSValue` that offers a value as any C scalar (numeric) type. It defines a set of methods specifically for creating number objects and accessing the value as a signed or unsigned `char`, `short int`, `int`, `NSInteger`, `long int`, `long long int`, `float`, or `double`, or as a `BOOL`.

NSInteger nine = 9;
```
float ten = 10.0;
```

```
 
```

```
NSNumber *nineFromInteger = [NSNumber alloc] initWithInteger:nine];
```

```
NSNumber *tenFromFloat = [NSNumber numberWithFloat:ten];
```
You can also create number object directly as literals using `@`:

NSNumber *nineFromInteger = @9;
```
NSNumber *tenFromFloat = @10.0;
```

```
NSNumber *nineteenFromExpression = @(nine + ten);
```
`NSNumber` defines a `compare:` method to determine the ordering of two `NSNumber` objects. :

```
NSComparisonResult comparison = [nineFromInteger compare:tenFromFloat];
```

```
// comparison = NSOrderedAscending
```

```
 
```

```
float aFloat = [nineFromInteger floatValue];
```

```
// aFloat = 9.0
```

```
BOOL ok = [tenFromFloat boolValue];
```

```
// ok = YES
```

An `NSNumber` object records the numeric type with which it is created, and uses the C rules for numeric conversion when comparing `NSNumber` objects of different numeric types and when returning values as C numeric types. See any standard C reference for information on type conversion. (If you ask a number for its `[objCType](https://developer.apple.com/documentation/foundation/nsnumber/1807278-objctype)`, however, the returned type does not necessarily match the method the receiver was created with.)

If you ask an `NSNumber` object for its value using a type that cannot hold the value, you get back an erroneous result—for example, if you ask for the `float` value of a number created with a `double` that is greater than `FLT_MAX`, or the `integer` value of a number created with a `float` that is greater than the maximum value of `NSInteger`.

```
NSNumber *bigNumber = @(FLT_MAX);
```

```
NSInteger badInteger = [bigNumber integerValue];
```

```
NSLog(@"bigNumber: %@; badInteger: %d", bigNumber, badInteger);
```

```
// output: "bigNumber: 3.402823e+38; badInteger: 0"
```

        
            [Next](DecimalNumbers.html)[Previous](Values.html)
        
         Copyright &#x00a9; 2008 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2008-02-08