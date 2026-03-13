---
title: "Enumeration: Traversing a Collection’s Elements"
book: "Collections Programming Topics"
chapterId: "20000135"
date: "2010-09-01"
description: "Explains how to group objects in arrays, sets, or dictionaries in Cocoa."
identifier: "//apple_ref/doc/uid/10000034i"
source: apple-archive
---

# Enumeration: Traversing a Collection’s Elements

> **Collections Programming Topics**
> Last updated: 2010-09-01

[Next](pointer.html)[Previous](Copying.html)
        
        
        [](../index.html)
        
        # Enumeration: Traversing a Collection’s Elements
Cocoa defines three main ways to [enumerate](../../../../General/Conceptual/DevPedia-CocoaCore/Enumeration.html#//apple_ref/doc/uid/TP40008195-CH17) the contents of a collection. These include fast enumeration and block-based enumeration. There is also the `NSEnumerator` class, though it has generally been superseded by fast enumeration.

## Fast Enumeration
Fast enumeration is the preferred method of enumerating the contents of a collection because it provides the following benefits:

The enumeration is more efficient than using `NSEnumerator` directly.

The syntax is concise.

The enumerator raises an exception if you modify the collection while enumerating.

You can perform multiple enumerations concurrently.

The behavior for fast enumeration varies slightly based on the type of collection. Arrays and sets enumerate their contents, and dictionaries enumerate their keys. `NSIndexSet` and `NSIndexPath` do not support fast enumeration. You can use fast enumeration with collection objects, as shown in Listing 1.

**Listing 1**  Using fast enumeration with a dictionary

for (NSString *element in someArray) {
```
     NSLog(@"element: %@", element);
```

```
}
```

```
 
```

```
NSString *key;
```

```
for (key in someDictionary){
```

```
     NSLog(@"Key: %@, Value %@", key, [someDictionary objectForKey: key]);
```

```
}
```
For more information on fast enumeration, see [Fast Enumeration](../../ObjectiveC/Chapters/ocFastEnumeration.html#//apple_ref/doc/uid/TP30001163-CH18) in *[The Objective-C Programming Language](../../ObjectiveC/Introduction/introObjectiveC.html#//apple_ref/doc/uid/TP30001163)*.

## Using Block-Based Enumeration
`NSArray`, `NSDictionary`, and `NSSet` allow enumeration of their contents using [blocks](../../../../General/Conceptual/DevPedia-CocoaCore/Block.html#//apple_ref/doc/uid/TP40008195-CH3). To enumerate with a block, invoke the appropriate method and specify the block to use. Listing 2 demonstrates block-based enumeration for an `NSArray` object.

**Listing 2**  Block-based enumeration of an array

NSArray *anArray = [NSArray arrayWithObjects:@"A", @"B", @"D", @"M", nil];
```
NSString *string = @"c";
```

```
 
```

```
[anArray enumerateObjectsUsingBlock:^(id obj, NSUInteger index, BOOL *stop){
```

```
     if ([obj localizedStandardCompare:string] == NSOrderedSame) {
```

```
          NSLog(@"Object Found: %@ at index: %i",obj, index);
```

```
          *stop = YES;
```

```
     }
```

```
} ];
```
For an `NSSet` object, you can use similar code, as shown in Listing 3.

**Listing 3**  Block-based enumeration of a set

NSSet *aSet = [NSSet setWithObjects: @"X", @"Y", @"Z", @"Pi", nil];
```
NSString *aString = @"z";
```

```
 
```

```
[aSet enumerateObjectsUsingBlock:^(id obj, BOOL *stop){
```

```
     if ([obj localizedStandardCompare:aString]==NSOrderedSame) {
```

```
          NSLog(@"Object Found: %@", obj);
```

```
          *stop = YES;
```

```
     }
```

```
} ];
```
For `NSArray` enumeration, the *index* parameter is useful for concurrent enumeration. Without this parameter, the only way to access the index would be to use the `[indexOfObject:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSArray/indexOfObject:)` method, which is inefficient. The *stop* parameter is important for performance, because it allows the enumeration to stop early based on some condition determined within the block. The block-based enumeration methods for the other collections are slightly different in name and in block signature. See the respective class references for the method definitions.

## Using an Enumerator

`[NSEnumerator](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSEnumerator/Description.html#//apple_ref/occ/cl/NSEnumerator)` is a simple abstract class whose subclasses enumerate collections of other objects. Collection objects—such as arrays, sets, and dictionaries—provide special `NSEnumerator` objects with which to enumerate their contents. You send `nextObject` repeatedly to a newly created `NSEnumerator` object to have it return the next object in the original collection. When the collection is exhausted, it returns `nil`. You can’t “reset” an enumerator after it’s exhausted its collection. To enumerate a collection again, you must create a new enumerator.

Collection classes such as `NSArray`, `NSSet`, and `NSDictionary` include methods that return an enumerator appropriate to the type of collection. For instance, `NSArray` has two methods that return an `NSEnumerator` object: `objectEnumerator` and `reverseObjectEnumerator`. The `NSDictionary` class also has two methods that return an `NSEnumerator` object: `keyEnumerator` and `objectEnumerator`. These methods let you enumerate the contents of an `NSDictionary` object by key or by value, respectively.

In Objective-C, an `NSEnumerator` object retains the collection over which it’s enumerating (unless it is implemented differently by a custom subclass).

It is not safe to remove, replace, or add to a mutable collection’s elements while enumerating through it. If you need to modify a collection during enumeration, you can either make a copy of the collection and enumerate using the copy or collect the information you require during the enumeration and apply the changes afterwards. The second pattern is illustrated in Listing 4.

**Listing 4**  Enumerating a dictionary and removing objects

```
NSMutableDictionary *myMutableDictionary = <#Get a mutable dictionary#> ;
```

```
NSMutableArray *keysToDeleteArray =
```

```
    [NSMutableArray arrayWithCapacity:[myMutableDictionary count]];
```

```
NSString *aKey;
```

```
NSEnumerator *keyEnumerator = [myMutableDictionary keyEnumerator];
```

```
while (aKey = [keyEnumerator nextObject])
```

```
{
```

```
    if ( /* test criteria for key or value */ ) {
```

```
        [keysToDeleteArray addObject:aKey];
```

```
    }
```

```
}
```

```
[myMutableDictionary removeObjectsForKeys:keysToDeleteArray];
```

        
            [Next](pointer.html)[Previous](Copying.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-09-01