---
title: "Pointer Function Options"
book: "Collections Programming Topics"
chapterId: "TP40010199"
date: "2010-09-01"
description: "Explains how to group objects in arrays, sets, or dictionaries in Cocoa."
identifier: "//apple_ref/doc/uid/10000034i"
source: apple-archive
---

# Pointer Function Options

> **Collections Programming Topics**
> Last updated: 2010-09-01

[Next](../RevisionHistory.html)[Previous](Enumerators.html)
        
        
        [](../index.html)
        
        # Pointer Function Options
The pointer collection classes (`NSPointerArray`, `NSMapTable`, and `NSHashTable`) allow you to further customize the collection to tailor it to your [memory](../../../../General/Conceptual/DevPedia-CocoaCore/MemoryManagement.html#//apple_ref/doc/uid/TP40008195-CH27) and storage needs. The options specified by `[NSPointerFunctionsOptions](https://developer.apple.com/documentation/foundation/nspointerfunctions/options)` provide a convenient interface for customizing how the collection manages the pointers it contains.

## Pointer Collection Fundamentals
Pointer collections are configured using options from three different categories: memory options, personality options, and copying behavior. Not all combinations of memory, personality, and copying options are valid.

Memory options specify the expected behavior for when items are added to the collection, removed from the collection, or copied. A few of the more common options include:

`NSPointerFunctionsStrongMemory`, which is used for a collection that holds strong references to its contents.

`NSPointerFunctionsZeroingWeakMemory`, which is used for a collection that holds weak references to its contents.

`NSPointerFunctionsOpaqueMemory`, which is used for cases when ownership of the contents is managed completely outside a collection. It is often used for collections that hold pointers to primitive types such as integers or C-strings.

Personality options specify the type of pointers stored in the collection, such as pointers to objects or pointers to other data types. They also specify what happens for hashing and equality tests. A few of the more common options include:

`NSPointerFunctionsObjectPersonality`, which is used for a collection that holds objects and uses `isEqual:` to determine [equality](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectComparison.html#//apple_ref/doc/uid/TP40008195-CH37).

`NSPointerFunctionsObjectPointerPersonality`, which is often used for a collection that holds objects and uses direct comparison to determine equality.

`NSPointerFunctionsOpaquePersonality`, which is often used for a collection that holds pointers to primitive types such as integers or C-strings.

Copy options specify whether the collection should [copy](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCopying.html#//apple_ref/doc/uid/TP40008195-CH38) the elements entered into the collection. If the `NSPointerFunctionsCopyIn` option is specified, the collection copies the elements entered; otherwise, it does not.

If you need greater customization than the `NSPointerFunctionsOptions` allow, you can use the `[NSPointerFunctions](https://developer.apple.com/documentation/foundation/nspointerfunctions)` class to define custom functions for operations like memory allocation, hashing, and equality testing. For example, if you have a collection of `struct`s, you would need to specify the size of the `struct`.

## Configuring Pointer Collections to Hold Objects
If you want to configure a pointer collection to hold objects, there are a few options. For objects, only two personality options make sense:

`NSPointerFunctionsObjectPersonality`, the default object option, uses the `isEqual:` method for determining equality. 

`NSPointerFunctionsObjectPointerPersonality`, also a viable option, uses pointer equality to determine equality.

You can also choose to use either strong references or zeroing weak references. If you choose to use strong references, you can also choose whether you want objects to be copied when they are added to the collection.

For example, if you want a collection to hold weak references to objects and use `isEqual:` to determine equality, you can specify the options as follows:

NSPointerFunctionsOptions collectionOptions = NSPointerFunctionsObjectPersonality
```
          | NSPointerFunctionsZeroingWeakMemory;
```
After specifying the options, `collectionOptions` can then be passed to a collection during initialization.

## Configuring Pointer Collections for Arbitrary Pointer Use
If you want to configure a pointer collection to hold arbitrary (non-object) pointers, you have some flexibility to configure the collection based on the type of pointer the collection will hold. For the most flexibility, you can select the `NSPointerFunctionsOpaquePersonality`, which allows you to hold pointers to most primitive types. You can also select one of the type-specific options:

`NSPointerFunctionsIntegerPersonality` holds integer pointers.

`NSPointerFunctionsStructPersonality` holds pointers to structs. If you specify this option you must set the `[sizeFunction](https://developer.apple.com/documentation/foundation/nspointerfunctions/1408045-sizefunction)` property of the `[NSPointerFunctions](https://developer.apple.com/documentation/foundation/nspointerfunctions)` object that you use.

`NSPointerFunctionsCStringPersonality` holds pointers to C-strings.

You should typically use `NSPointerFunctionsOpaqueMemory` when dealing with arbitrary pointers because it is compatible with all of the personality options. If you need to, you can use `NSPointerFunctionsMallocMemory` or `NSPointerFunctionsMachVirtualMemory` with the opaque, C-string, and struct personalities, although this is not typically recommended.

The only arbitrary pointer configurations that support copy-in behavior are the C-string and struct personalities when using either malloc or Mach virtual memory.

If you want a collection to hold arbitrary pointers using opaque memory, you can specify the options as follows:

NSPointerFunctionsOptions collectionOptions = NSPointerFunctionsOpaquePersonality
```
          | NSPointerFunctionsOpaqueMemory;
```
After specifying the options, `collectionOptions` can then be passed to a collection during initialization.

        
            [Next](../RevisionHistory.html)[Previous](Enumerators.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-09-01