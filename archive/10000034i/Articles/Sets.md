---
title: "Sets: Unordered Collections of Objects"
book: "Collections Programming Topics"
chapterId: "20000136"
date: "2010-09-01"
description: "Explains how to group objects in arrays, sets, or dictionaries in Cocoa."
identifier: "//apple_ref/doc/uid/10000034i"
source: apple-archive
---

# Sets: Unordered Collections of Objects

> **Collections Programming Topics**
> Last updated: 2010-09-01

[Next](index.html)[Previous](Dictionaries.html)
        
        
        [](../index.html)
        
        # Sets: Unordered Collections of Objects
A set is an unordered collection of objects, as shown in Figure 1. You can use sets as an alternative to arrays when the order of elements isn’t important and performance of testing whether an object is in the set is important. Even though arrays are ordered, testing them for membership is slower than testing sets.

**Figure 1**  Example set## Set Fundamentals
An `NSSet` object manages an immutable set of distinct objects—that is, after you [create](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39) the set, you cannot add, remove, or replace objects. You can, however, modify individual objects themselves (if they support modification). The mutability of the collection does not affect the mutability of the objects inside the collection. You should use an immutable set if the set rarely changes, or changes wholesale.

`NSMutableSet`, a subclass of `NSSet`, is a [mutable](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectMutability.html#//apple_ref/doc/uid/TP40008195-CH42) set of distinct objects, which allows the addition and deletion of entries at any time, automatically allocating memory as needed. You should use a mutable set if the set changes incrementally, or is very large—as large collections take more time to [initialize](../../../../General/Conceptual/DevPedia-CocoaCore/Initialization.html#//apple_ref/doc/uid/TP40008195-CH21).

`NSCountedSet`, a subclass of `NSMutableSet`, is a mutable set to which you can add a particular object more than once; in other words, the elements of the set aren’t necessarily distinct. A counted set is also known as a *bag*. The set keeps a counter associated with each distinct object inserted. `NSCountedSet` objects keep track of the number of times objects are inserted and require that objects be removed the same number of times to completely remove the object from the set. Thus, there is only one instance of an object in a counted set, even if the object has been added multiple times. The `[countForObject:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSCountedSet/Description.html#//apple_ref/occ/instm/NSCountedSet/countForObject:)` method returns the number of times the specified object has been added to this set.

The objects in a set must respond to the `NSObject` protocol methods `hash` and `isEqual:` (see `[NSObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/cl/NSObject)` for more information). If mutable objects are stored in a set, either the `hash` method of the objects shouldn’t depend on the internal state of the mutable objects or the mutable objects shouldn’t be modified while they’re in the set. For example, a mutable dictionary can be put in a set, but you must not change it while it is in there. (Note that it can be difficult to know whether or not a given object is in a collection).

`NSSet` provides a number of [initializer](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39) methods, such as `[setWithObjects:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSSetClassCluster/Description.html#//apple_ref/occ/clm/NSSet/setWithObjects:)` and `[initWithArray:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSSetClassCluster/Description.html#//apple_ref/occ/instm/NSSet/initWithArray:)`, that return an `NSSet` object containing the elements (if any) you pass in as arguments. Objects added to a set are not [copied](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCopying.html#//apple_ref/doc/uid/TP40008195-CH38) (unless you pass `YES` as the argument to `[initWithSet:copyItems:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSSetClassCluster/Description.html#//apple_ref/occ/instm/NSSet/initWithSet:copyItems:)`). Rather, a strong reference to the object is added to the set. For more information on copying and memory management, see [Copying Collections](Copying.html#//apple_ref/doc/uid/TP40010162-SW1).

Sets, excluding `NSCountedSet`, are the preferred collections if you want to be assured that no object is represented more than once, and there is no net effect for adding an object more than once.

> **Note:** If the objects in the set have a good hash function, accessing an element, setting an element, and removing an element all take constant time. With a poor hash function (one that causes frequent hash collisions), these operations take up to linear time. Classes such as `NSString` that are part of Foundation have a good hash function.

## Mutable Sets
You can create an `NSMutableSet` object using any of the initializers provided by `NSSet`. You can create an `NSMutableSet` object from an instance of `NSSet` (or vice versa) using `[setWithSet:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSSetClassCluster/Description.html#//apple_ref/occ/clm/NSSet/setWithSet:)` or `[initWithSet:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSSetClassCluster/Description.html#//apple_ref/occ/instm/NSSet/initWithSet:)`.

The `NSMutableSet` class provides methods for adding objects to a set: 

`addObject:` adds a single object to the set.

`addObjectsFromArray:` adds all objects from a specified array to the set. 

`unionSet:` adds all the objects from another set which are not already present.

The `NSMutableSet` class additionally provides these methods to remove objects from a set: 

`intersectSet:` removes all the objects which are not in another set.

`removeAllObjects` removes all the objects from the set.

`removeObject:` removes a particular object from the set.

`minusSet:` removes all the objects which are in another set.

Because `NSCountedSet` is a subclass of `NSMutableSet`, it inherits all of these methods. However, some of them behave slightly differently with `NSCountedSet`. For example:

`unionSet:` adds all the objects from another set, even if they are already present.

`intersectSet:` removes all the objects which are not in another set. If there are multiple instances of the same object in either set, the resulting set contains that object as many times as the set with fewer instances.

`minusSet:` removes all the objects which are in another set. If an object is present more than once in the counted set, it only removes one instance of it.

## Using Sets

The `NSSet` class provides methods for querying the elements of the set:

`allObjects` returns an array containing the objects in a set.

`anyObject` returns some object in the set. (The object is chosen at convenience *not* at random.)

`count` returns the number of objects currently in the set.

`member:` returns the object in the set that is equal to a specified object.

`intersectsSet:` tests whether two sets share at least one object.

`isEqualToSet:` tests whether two sets are equal.

`isSubsetOfSet:` tests whether all of the objects contained in the set are also present in another set.

The `NSSet` method `objectEnumerator` lets you traverse elements of the set one by one. And the`makeObjectsPerformSelector:` and `makeObjectsPerformSelector:withObject:` methods provide for sending [messages](../../../../General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59) to individual objects in the set. In most cases, fast enumeration should be used because it is faster and more flexible than using an `NSEnumerator` or the `makeObjectsPerformSelector:` method. For more on enumeration, see [Enumeration: Traversing a Collection’s Elements](Enumerators.html#//apple_ref/doc/uid/20000135-BBCFABCB).

## Hash Tables
The `NSHashTable` class is configured by default to hold objects much like `NSMutableSet` does. It also allows additional storage options that you can tailor for specific cases, such as when you need advanced [memory management](../../../../General/Conceptual/DevPedia-CocoaCore/MemoryManagement.html#//apple_ref/doc/uid/TP40008195-CH27) options, or when you want to hold a specific type of pointer. For example, the map table in Figure 2 is configured to hold weak references to its elements. You can also specify whether you want to copy objects entered into the  set.

**Figure 2**  Hash table object ownershipYou can use an `NSHashTable` object when you want an unordered collection of elements that uses weak references. For example, suppose you have a global hash table that contains some objects. Because global objects are never collected, none of its contents can be deallocated unless they are held weakly. Hash tables configured to hold objects weakly do not own their contents. If there are no strong references to objects within such a hash table, those objects are deallocated. For example, the hash table in [Figure 2](#//apple_ref/doc/uid/20000136-SW7) holds weak references to its contents. Objects A, C, and Z will be deallocated, but the rest of the objects remain.

To create a hash table, initialize it using the `[initWithOptions:capacity:](https://developer.apple.com/documentation/foundation/nshashtable/1411066-init)` method and the appropriate pointer functions options. Alternatively, you can initialize it using `[initWithPointerFunctions:capacity:](https://developer.apple.com/documentation/foundation/nshashtable/1416331-initwithpointerfunctions)` and appropriate instances of `[NSPointerFunctions](https://developer.apple.com/documentation/foundation/nspointerfunctions)`. For more information on the various pointer functions options, see [Pointer Function Options](pointer.html#//apple_ref/doc/uid/TP40010199-SW1).

The `NSHashTable` class also defines the `[hashTableWithWeakObjects](https://developer.apple.com/documentation/foundation/nshashtable/1591430-hashtablewithweakobjects)` convenience constructor for creating a hash table with weak references to its contents. It should only be used if you are storing objects.

> **Important:** Only the options listed in `[NSHashTableOptions](https://developer.apple.com/documentation/foundation/nshashtableoptions)` guarantee that the rest of the API will work correctly—including copying archiving and fast enumeration. While other `[NSPointerFunctions](https://developer.apple.com/documentation/foundation/nspointerfunctions)` options are used for certain configurations, such as to hold arbitrary pointers, not all combinations of the options are valid. With some combinations the hash table may not work correctly, or may not even be initialized correctly.

To configure a hash table to use arbitrary pointers, initialize it with both the `NSPointerFunctionsOpaqueMemory` and `NSPointerFunctionsOpaquePersonality` options. When using a hash table to contain arbitrary pointers, the C function API for `void *` pointers should be used. For more information, see Hash Tables. For example, you can add a pointer to an `int` value using the approach shown in Listing 1. 

**Listing 1**  Hash table configured for non-object pointers

NSHashTable *hashTable=[[NSHashTable alloc] initWithOptions:
```
    NSPointerFunctionsOpaqueMemory |NSPointerFunctionsOpaquePersonality
```

```
    capacity: 1];
```

```
 
```

```
NSHashInsert(hashTable, someIntPtr);
```
When configured to use arbitrary pointers, a hash table has the risks associated with using pointers. For example, if the pointers refer to stack-based data created in a function, those pointers are not valid outside of the function, even if the hash table is. Trying to access them will lead to undefined behavior.

        
            [Next](index.html)[Previous](Dictionaries.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-09-01