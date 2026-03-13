---
title: "Copying Collections"
book: "Collections Programming Topics"
chapterId: "TP40010162"
date: "2010-09-01"
description: "Explains how to group objects in arrays, sets, or dictionaries in Cocoa."
identifier: "//apple_ref/doc/uid/10000034i"
source: apple-archive
---

# Copying Collections

> **Collections Programming Topics**
> Last updated: 2010-09-01

[Next](Enumerators.html)[Previous](IndexPaths.html)
        
        
        [](../index.html)
        
        # Copying Collections
There are two kinds of [object copying](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCopying.html#//apple_ref/doc/uid/TP40008195-CH38): shallow copies and deep copies. The normal copy is a shallow copy that produces a new collection that shares ownership of the objects with the original. Deep copies create new objects from the originals and add those to the new collection. This difference is illustrated by Figure 1.

**Figure 1**  Shallow copies and deep copies## Shallow Copies
There are a number of ways to make a shallow copy of a collection. When you create a shallow copy, the objects in the original collection are sent a `retain` message and the pointers are copied to the new collection. Listing 1 shows some of the ways to create a new collection using a shallow copy.

**Listing 1**  Making a shallow copy

NSArray *shallowCopyArray = [someArray copyWithZone:nil];
```
 
```

```
NSDictionary *shallowCopyDict = [[NSDictionary alloc] initWithDictionary:someDictionary copyItems:NO];
```
These techniques are not restricted to the collections shown. For example, you can copy a set with the `[copyWithZone:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCopying/Description.html#//apple_ref/occ/intfm/NSCopying/copyWithZone:)` method—or the `[mutableCopyWithZone:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSMutableCopying/Description.html#//apple_ref/occ/intfm/NSMutableCopying/mutableCopyWithZone:)` method—or an array with `[initWithArray:copyItems:](https://developer.apple.com/documentation/foundation/nsarray/1408557-initwitharray)` method.

## Deep Copies
There are two ways to make deep copies of a collection. You can use the collection’s equivalent of `initWithArray:copyItems:` with `YES` as the second parameter. If you create a deep copy of a collection in this way, each object in the collection is sent a `copyWithZone:` message. If the objects in the collection have adopted the `[NSCopying](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCopying/Description.html#//apple_ref/occ/intf/NSCopying)` protocol, the objects are deeply copied to the new collection, which is then the sole owner of the copied objects. If the objects do not adopt the `NSCopying` protocol, attempting to copy them in such a way results in a runtime error. However, `copyWithZone:` produces a shallow copy. This kind of copy is only capable of producing a one-level-deep copy. If you only need a one-level-deep copy, you can explicitly call for one as in Listing 2.

**Listing 2**  Making a deep copy

NSArray *deepCopyArray=[[NSArray alloc] initWithArray:someArray copyItems:YES];This technique applies to the other collections as well. Use the collection’s equivalent of `initWithArray:copyItems:` with `YES` as the second parameter.

If you need a true deep copy, such as when you have an array of arrays, you can archive and then unarchive the collection, provided the contents all conform to the `[NSCoding](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intf/NSCoding)` protocol. An example of this technique is shown in Listing 3.

**Listing 3**  A true deep copy

```
NSArray* trueDeepCopyArray = [NSKeyedUnarchiver unarchiveObjectWithData:
```

```
          [NSKeyedArchiver archivedDataWithRootObject:oldArray]];
```
## Copying and Mutability
When you copy a collection, the [mutability](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectMutability.html#//apple_ref/doc/uid/TP40008195-CH42) of that collection or the objects it contains can be affected. Each method of copying has slightly different effects on the mutability of the objects in a collection of arbitrary depth:

`copyWithZone:` makes the surface level immutable. All deeper levels have the mutability they previously had.

`initWithArray:copyItems:` with `NO` as the second parameter gives the surface level the mutability of the class it is allocated as. All deeper levels have the mutability they previously had.

`initWithArray:copyItems:` with `YES` as the second parameter gives the surface level the mutability of the class it is allocated as. The next level is immutable, and all deeper levels have the mutability they previously had.

Archiving and unarchiving the collection leaves the mutability of all levels as it was before.

        
            [Next](Enumerators.html)[Previous](IndexPaths.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-09-01