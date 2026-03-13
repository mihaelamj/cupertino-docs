---
title: "Using Values"
book: "Number and Value Programming Topics"
chapterId: "20000174"
date: "2008-02-08"
description: "Explains how to use Cocoa object wrappers for primitive C data types."
identifier: "//apple_ref/doc/uid/10000038i"
source: apple-archive
---

# Using Values

> **Number and Value Programming Topics**
> Last updated: 2008-02-08

[Next](Numbers.html)[Previous](../NumbersandValues.html)
        
        
        [](../index.html)
        
        # Using Values

An `[NSValue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSValue/Description.html#//apple_ref/occ/cl/NSValue)` object is a simple container for a single C or Objective-C data item. It can hold any of the scalar types such as `int`, `float`, and `char`, as well as pointers, structures, and object `id`s. The purpose of this class is to allow items of such data types to be added to collection objects such as instances of `[NSArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSArray)` or `[NSSet](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSSetClassCluster/Description.html#//apple_ref/occ/cl/NSSet)`, which require their elements to be objects. `[NSValue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSValue/Description.html#//apple_ref/occ/cl/NSValue)` objects are always immutable.

To create an `[NSValue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSValue/Description.html#//apple_ref/occ/cl/NSValue)` object with a particular data item, you provide a pointer to the item along with a C string describing the item’s type in Objective-C type encoding. You get this string using the `@encode()` compiler directive, which returns the platform-specific encoding for the given type (see [Type Encodings](../../ObjCRuntimeGuide/Articles/ocrtTypeEncodings.html#//apple_ref/doc/uid/TP40008048-CH100) for more information about `@encode()` and a list of type codes). For example, this code excerpt creates *theValue* containing an `[NSRange](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/tdef/NSRange)`:

NSRange myRange = {4, 10};
```
NSValue *theValue = [NSValue valueWithBytes:&myRange objCType:@encode(NSRange)];
```
The following example illustrates encoding a custom C structure.

```
// assume ImaginaryNumber defined:
```

```
typedef struct {
```

```
    float real;
```

```
    float imaginary;
```

```
} ImaginaryNumber;
```

```
 
```

```
 
```

```
ImaginaryNumber miNumber;
```

```
miNumber.real = 1.1;
```

```
miNumber.imaginary = 1.41;
```

```
 
```

```
NSValue *miValue = [NSValue valueWithBytes: &miNumber
```

```
                            withObjCType:@encode(ImaginaryNumber)];
```

```
 
```

```
ImaginaryNumber miNumber2;
```

```
[miValue getValue:&miNumber2];
```

The type you specify must be of constant length. You cannot store C strings, variable-length arrays and structures, and other data types of indeterminate length in an `[NSValue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSValue/Description.html#//apple_ref/occ/cl/NSValue)`—you should use `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` or `[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)` objects for these types. You can store a pointer to variable-length item in an `[NSValue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSValue/Description.html#//apple_ref/occ/cl/NSValue)` object. The following code excerpt incorrectly attempts to place a C string directly into an `[NSValue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSValue/Description.html#//apple_ref/occ/cl/NSValue)` object:

```
/* INCORRECT! */
```

```
char *myCString = "This is a string.";
```

```
NSValue *theValue = [NSValue valueWithBytes:myCString withObjCType:@encode(char *)];
```

In this code excerpt the contents of *myCString* are interpreted as a pointer to a `char`, so the first four bytes contained in the string are treated as a pointer (the actual number of bytes used may vary with the hardware architecture). That is, the sequence “This” is interpreted as a pointer value, which is unlikely to be a legal address. The correct way to store such a data item is to use an `NSString` object (if you need to contain the characters in an object), or to pass the address of its pointer, not the pointer itself:

```
/* Correct. */
```

```
char *myCString = "This is a string.";
```

```
NSValue *theValue = [NSValue valueWithBytes:&myCString withObjCType:@encode(char **)];
```

Here the *address* of *myCString* is passed (*&myCString*), so the address of the first character of the string is stored in *theValue*.

> **Important:** The `[NSValue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSValue/Description.html#//apple_ref/occ/cl/NSValue)` object doesn’t copy the contents of the string, but the pointer itself. If you create an `[NSValue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSValue/Description.html#//apple_ref/occ/cl/NSValue)` object with an allocated data item, don’t free the data’s memory while the `[NSValue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSValue/Description.html#//apple_ref/occ/cl/NSValue)` object exists.

        
            [Next](Numbers.html)[Previous](../NumbersandValues.html)
        
         Copyright &#x00a9; 2008 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2008-02-08