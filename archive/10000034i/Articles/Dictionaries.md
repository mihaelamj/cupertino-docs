---
title: "Dictionaries: Collections of Keys and Values"
book: "Collections Programming Topics"
chapterId: "20000134"
date: "2010-09-01"
description: "Explains how to group objects in arrays, sets, or dictionaries in Cocoa."
identifier: "//apple_ref/doc/uid/10000034i"
source: apple-archive
---

# Dictionaries: Collections of Keys and Values

> **Collections Programming Topics**
> Last updated: 2010-09-01

[Next](Sets.html)[Previous](Arrays.html)
        
        
        [](../index.html)
        
        # Dictionaries: Collections of Keys and Values
Dictionaries manage pairs of keys and values. A key-value pair within a dictionary is called an entry. Each entry consists of one object that represents the key, and a second object which is that key’s value. Within a dictionary, the keys are unique—that is, no two keys in a single dictionary are equal (as determined by `isEqual:`). A key can be any object that adopts the `NSCopying` protocol and implements the `hash` and `isEqual:` methods. Figure 1 shows a dictionary which contains information about a hypothetical person. As shown, a value  contained in the dictionary can be any object, even another collection.

**Figure 1**  Example dictionary## Dictionary Fundamentals
An `NSDictionary` object manages an immutable dictionary—that is, after you create the dictionary, you cannot add, remove or replace keys and values. You can, however, modify individual values themselves (if they support modification), but the keys must not be modified. The mutability of the collection does not affect the mutability of the objects inside the collection. You should use an immutable dictionary if the dictionary rarely changes, or changes wholesale.

An `NSMutableDictionary` object manages a [mutable](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectMutability.html#//apple_ref/doc/uid/TP40008195-CH42) dictionary, which allows the addition and deletion of entries at any time, automatically allocating memory as needed. You should use a mutable dictionary if the dictionary changes incrementally, or is very large—as large collections take more time to [initialize](../../../../General/Conceptual/DevPedia-CocoaCore/Initialization.html#//apple_ref/doc/uid/TP40008195-CH21).

You can easily [create](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39) an instance of one type of dictionary from the other using the initializer `initWithDictionary:` or the convenience constructor `dictionaryWithDictionary:`.

In general, you instantiate a dictionary by sending one of the `dictionary...`[messages](../../../../General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59) to either the `NSDictionary` or `NSMutableDictionary` class. The `dictionary...` messages return a dictionary containing the keys and values you pass in as arguments. Objects added as values to a dictionary are not [copied](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCopying.html#//apple_ref/doc/uid/TP40008195-CH38) (unless you pass `YES` as the argument to `[initWithDictionary:copyItems:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/instm/NSDictionary/initWithDictionary:copyItems:)`). Rather, a strong reference to the object is added to the dictionary. For information on how a dictionary handles key objects, see [Using Custom Keys](#//apple_ref/doc/uid/20000134-SW8). For more information on copying and memory management, see [Copying Collections](Copying.html#//apple_ref/doc/uid/TP40010162-SW1).

Internally, a dictionary uses a hash table to organize its storage and to provide rapid access to a value given the corresponding key. However, the methods defined for dictionaries insulate you from the complexities of working with hash tables, hashing functions, or the hashed value of keys. The methods take keys directly, not in their hashed form.

> **Note:** If the key objects have a good hash function, accessing an element, setting an element, and removing an element all take constant time. With a poor hash function (one that causes frequent hash collisions), these operations take up to linear time. Classes such as `NSString` that are part of Foundation have a good hash function.

## Using Mutable Dictionaries

When removing an entry from a mutable dictionary, remember that the dictionary’s strong reference to the key and value objects that make up the entry are discarded. If there are no further strong references to the objects, they’re deallocated.

Adding objects to a mutable dictionary is relatively straightforward. To add a single key-value pair, or to replace the object for a particular key, use the `[setObject:forKey:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/instm/NSMutableDictionary/setObject:forKey:)` instance method, as shown in Listing 1.

**Listing 1**  Adding objects to a dictionary

NSString *last = @"lastName";
```
NSString *first = @"firstName";
```

```
 
```

```
NSMutableDictionary *dict = [NSMutableDictionary dictionaryWithObjectsAndKeys:
```

```
        @"Jo", first, @"Smith", last, nil];
```

```
NSString *middle = @"middleInitial";
```

```
 
```

```
[dict setObject:@"M" forKey:middle];
```
You can also add entries from another dictionary using the `[addEntriesFromDictionary:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/instm/NSMutableDictionary/addEntriesFromDictionary:)` instance method. If both dictionaries contain the same key, the receiver’s previous value object for that key is released and the new object takes its place. For example, after the code in Listing 2 executes, `dict` would have a value of “Jones” for the key “lastName”.

**Listing 2**  Adding entries from another dictionary

```
NSString *last = @"lastName";
```

```
NSString *first = @"firstName";
```

```
NSString *suffix = @"suffix";
```

```
NSString *title = @"title";
```

```
 
```

```
NSMutableDictionary *dict = [NSMutableDictionary dictionaryWithObjectsAndKeys:
```

```
    @"Jo", first, @"Smith", last, nil];
```

```
 
```

```
NSDictionary *newDict = [NSDictionary dictionaryWithObjectsAndKeys:
```

```
    @"Jones", last, @"Hon.", title, @"J.D.", suffix, nil];
```

```
 
```

```
[dict addEntriesFromDictionary: newDict];
```
## Sorting a Dictionary
`NSDictionary` provides the method `[keysSortedByValueUsingSelector:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/instm/NSDictionary/keysSortedByValueUsingSelector:)`, which returns an array of the dictionary’s keys in the order they would be in if the dictionary were sorted by its values, as illustrated in Listing 3.

**Listing 3**  Sorting dictionary keys by value

NSDictionary *dict = [NSDictionary dictionaryWithObjectsAndKeys:
```
    [NSNumber numberWithInt:63], @"Mathematics",
```

```
    [NSNumber numberWithInt:72], @"English",
```

```
    [NSNumber numberWithInt:55], @"History",
```

```
    [NSNumber numberWithInt:49], @"Geography",
```

```
    nil];
```

```
 
```

```
NSArray *sortedKeysArray =
```

```
    [dict keysSortedByValueUsingSelector:@selector(compare:)];
```

```
// sortedKeysArray contains: Geography, History, Mathematics, English
```
You can also use [blocks](../../../../General/Conceptual/DevPedia-CocoaCore/Block.html#//apple_ref/doc/uid/TP40008195-CH3) to easily sort a dictionary’s keys based on their corresponding values. The `[keysSortedByValueUsingComparator:](https://developer.apple.com/documentation/foundation/nsdictionary/1411105-keyssortedbyvalueusingcomparator)` method of `NSDictionary` allows you to use a block to compare the keys to be sorted into a new array. Listing 4 illustrates sorting with a block.

**Listing 4**  Blocks ease custom sorting of dictionaries

```
NSArray *blockSortedKeys = [dict keysSortedByValueUsingComparator: ^(id obj1, id obj2) {
```

```
 
```

```
     if ([obj1 integerValue] > [obj2 integerValue]) {
```

```
          return (NSComparisonResult)NSOrderedDescending;
```

```
     }
```

```
 
```

```
     if ([obj1 integerValue] < [obj2 integerValue]) {
```

```
          return (NSComparisonResult)NSOrderedAscending;
```

```
     }
```

```
     return (NSComparisonResult)NSOrderedSame;
```

```
}];
```
## Using Custom Keys
In most cases, Cocoa-provided objects such as `NSString` objects should be sufficient for use as keys. In some cases, however, it may be necessary to use custom objects as keys in a dictionary. When using custom objects as keys, there are some important points to keep in mind.

Keys must conform to the `[NSCopying](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCopying/Description.html#//apple_ref/occ/intf/NSCopying)` protocol. Methods that add entries to dictionaries—whether as part of initialization (for all dictionaries) or during modification (for [mutable](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectMutability.html#//apple_ref/doc/uid/TP40008195-CH42) dictionaries)— don’t add each key object to the dictionary directly. Instead, they copy each key argument and add the copy to the dictionary. After being copied into the dictionary, the dictionary-owned copies of the keys should not be modified.

Keys must implement the `hash` and `isEqual:` methods because a dictionary uses a hash table to organize its storage and to quickly access contained objects. In addition, performance in a dictionary is heavily dependent on the hash function used. With a bad hash function, the decrease in performance can be severe. For more information on the `hash` and `isEqual:` methods see `[NSObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intf/NSObject)`.

> **Important:** Because the dictionary copies each key, keys must conform to the `NSCopying` protocol. Bear this in mind when choosing what objects to use as keys. Although you can use any object that adopts the `NSCopying` protocol and implements the `hash` and `isEqual:` methods, it is typically bad design to use large objects, such as instances of `NSImage`, because doing so may incur performance penalties.

## Using Map Tables
The `NSMapTable` class is configured by default to hold objects much like `NSMutableDictionary`. It also allows additional storage options that you can tailor for specific cases, such as when you need advanced [memory management](../../../../General/Conceptual/DevPedia-CocoaCore/MemoryManagement.html#//apple_ref/doc/uid/TP40008195-CH27) options, or when you want to hold a specific type of pointer. For example, the map table in Figure 2 is configured to hold weak references to its value objects. You can also specify whether you want to copy objects entered into the array.

**Figure 2**  Map table object ownershipYou can use an `NSMapTable` object when you want a collection of key-value pairs that uses weak references. For example, suppose you have a global map table that contains some objects. Because global objects are never collected, none of its contents can be deallocated unless they are held weakly. Map tables configured to hold objects weakly do not own their contents. If there are no strong references to objects within such a map table, those objects are deallocated. For example, the map table in [Figure 2](#//apple_ref/doc/uid/20000134-SW2) holds weak references to its contents. Object D and Object E will be deallocated, but the rest of the objects remain.

To create a map table, create or initialize it using `[mapTableWithKeyOptions:valueOptions:](https://developer.apple.com/documentation/foundation/nsmaptable/1391414-init)` or `[initWithKeyOptions:valueOptions:capacity:](https://developer.apple.com/documentation/foundation/nsmaptable/1391382-init)` and the appropriate pointer functions options. Alternatively, you can initialize it using `[initWithKeyPointerFunctions:valuePointerFunctions:capacity:](https://developer.apple.com/documentation/foundation/nsmaptable/1391429-initwithkeypointerfunctions)` and appropriate instances of `[NSPointerFunctions](https://developer.apple.com/documentation/foundation/nspointerfunctions)`. For more information on the pointer functions options, see [Pointer Function Options](pointer.html#//apple_ref/doc/uid/TP40010199-SW1). 

The `NSMapTable` class also defines a number of convenience constructors for creating a map table with strong or weak references to its contents. For example, `[mapTableWithStrongToWeakObjects](https://developer.apple.com/documentation/foundation/nsmaptable/1391372-maptablewithstrongtoweakobjects)` creates a map table that holds strong references to its keys and weak references to its values. These convenience constructors should only be used if you are storing objects.

> **Important:** Only the options listed in `[NSMapTableOptions](https://developer.apple.com/documentation/foundation/nsmaptableoptions)` guarantee that the rest of the API will work correctly—including copying, archiving, and fast enumeration. While other `[NSPointerFunctions](https://developer.apple.com/documentation/foundation/nspointerfunctions)` options are used for certain configurations, such as to hold arbitrary pointers, not all combinations of the options are valid. With some combinations, the map table may not work correctly or may not even be initialized correctly.

To configure a map table to use arbitrary pointers, initialize it with both the `NSPointerFunctionsOpaqueMemory` and `NSPointerFunctionsOpaquePersonality` value options. Key and value options do not have to be the same. When using a map table to contain arbitrary pointers, the C function API for `void *` pointers should be used. For more information, see Managing Map Tables. For example, you can add a pointer to an `int` value using the approach shown in Listing 5. Note that the map table uses `NSString` objects as keys, and that keys are copied into the map table. 

**Listing 5**  Map table configured for non-object pointers

NSPointerFunctionsOptions keyOptions=NSPointerFunctionsStrongMemory |
```
     NSPointerFunctionsObjectPersonality | NSPointerFunctionsCopyIn;
```

```
NSPointerFunctionsOptions valueOptions=NSPointerFunctionsOpaqueMemory |
```

```
     NSPointerFunctionsOpaquePersonality;
```

```
 
```

```
NSMapTable *mapTable = [NSMapTable mapTableWithKeyOptions:keyOptions
```

```
        valueOptions:valueOptions];
```

```
 
```

```
NSString *key1 = @"Key1";
```

```
NSMapInsert(mapTable, key1, someIntPtr);
```
You can then access that integer by using the `[NSMapGet](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSMapGet)` function.

NSLog(@" Key1 contains: %i", *(int *) NSMapGet(mapTable, @"Key1"));When configured to use arbitrary pointers, a map table has the risks associated with using pointers. For example, if the pointers refer to stack-based data created in a function, those pointers are not valid outside of the function, even if the map table is. Trying to access them will lead to undefined behavior.

        
            [Next](Sets.html)[Previous](Arrays.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-09-01