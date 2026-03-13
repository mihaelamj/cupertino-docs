---
title: "Arrays: Ordered Collections"
book: "Collections Programming Topics"
chapterId: "20000132"
date: "2010-09-01"
description: "Explains how to group objects in arrays, sets, or dictionaries in Cocoa."
identifier: "//apple_ref/doc/uid/10000034i"
source: apple-archive
---

# Arrays: Ordered Collections

> **Collections Programming Topics**
> Last updated: 2010-09-01

[Next](Dictionaries.html)[Previous](../Collections.html)
        
        
        [](../index.html)
        
        # Arrays: Ordered Collections
Arrays are ordered collections of any sort of object. For example, the objects contained by the array in Figure 1 can be any combination of cat and dog objects, and if the array is mutable you can add more dog objects. The collection does not have to be homogeneous.

**Figure 1**  Example array## Array Fundamentals
An `NSArray` object manages an immutable array—that is, after you have created the array, you cannot add, remove, or replace objects. You can, however, modify individual elements themselves (if they support modification). The mutability of the collection does not affect the mutability of the objects inside the collection. You should use an immutable array if the array rarely changes, or changes wholesale.

An `NSMutableArray` object manages a [mutable](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectMutability.html#//apple_ref/doc/uid/TP40008195-CH42) array, which allows the addition and deletion of entries, allocating memory as needed. For example, given an `NSMutableArray` object that contains just a single dog object, you can add another dog, or a cat, or any other object. You can also, as with an `NSArray` object, change the dog’s name—and in general, anything that you can do with an `NSArray` object you can do with an `NSMutableArray` object. You should use a mutable array if the array changes incrementally or is very large—as large collections take more time to [initialize](../../../../General/Conceptual/DevPedia-CocoaCore/Initialization.html#//apple_ref/doc/uid/TP40008195-CH21).

You can easily [create](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39) an instance of one type of array from the other using the initializer `initWithArray:` or the convenience constructor `arrayWithArray:`. For example, if you have an instance of `NSArray`, `myArray`, you can create a mutable copy as follows:

```
NSMutableArray *myMutableArray = [NSMutableArray arrayWithArray:myArray];
```

In general, you instantiate an array by sending one of the `array...`  [messages](../../../../General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59) to either the `NSArray` or `NSMutableArray` class. The `array...` messages return an array containing the elements you pass in as arguments. And when you add an object to an `NSMutableArray` object, the object isn’t [copied](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCopying.html#//apple_ref/doc/uid/TP40008195-CH38), (unless you pass `YES` as the argument to `[initWithArray:copyItems:](https://developer.apple.com/documentation/foundation/nsarray/1408557-initwitharray)`). Rather, a strong reference to the object is added to the array. For more information on copying and memory management, see [Copying Collections](Copying.html#//apple_ref/doc/uid/TP40010162-SW1).

In `NSArray`, two main methods—`count` and `objectAtIndex:`—provide the basis for all other methods in its interface:

`[count](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSArray/count)` returns the number of elements in the array.

`[objectAtIndex:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSArray/objectAtIndex:)` gives you access to the array elements by index, with index values starting at 0.

> **Note:** Most operations on an array take constant time: accessing an element, adding or removing an element at either end, and replacing an element. Inserting an element into the middle of an array takes linear time.

## Mutable Arrays

In `NSMutableArray`, the main methods, listed below, provide the basis for its ability to add, replace, and remove elements:

`[addObject:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSMutableArray/addObject:)`

`[insertObject:atIndex:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSMutableArray/insertObject:atIndex:)`

`[removeLastObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSMutableArray/removeLastObject)`

`[removeObjectAtIndex:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSMutableArray/removeObjectAtIndex:)`

`[replaceObjectAtIndex:withObject:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSMutableArray/replaceObjectAtIndex:withObject:)`

If you do not need an object to be placed at a specific index or to be removed from the middle of the collection, you should use the `addObject:` and `removeLastObject` methods because it is faster to add and remove at the end of an array than in the middle.

The other methods in `NSMutableArray` provide convenient ways of inserting an object into a slot in the array and removing an object based on its identity or position in the array, as illustrated in Listing 1.

**Listing 1**  Adding to and removing from arrays

```
NSMutableArray *array = [NSMutableArray array];
```

```
[array addObject:[NSColor blackColor]];
```

```
[array insertObject:[NSColor redColor] atIndex:0];
```

```
[array insertObject:[NSColor blueColor] atIndex:1];
```

```
[array addObject:[NSColor whiteColor]];
```

```
[array removeObjectsInRange:(NSMakeRange(1, 2))];
```

```
// array now contains redColor and whiteColor
```
## Using Arrays
You can access objects in an array by index using the `[objectAtIndex:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSArray/objectAtIndex:)` method. For example, if you have an array of `NSString` objects, you can access the third string in the array as follows:

```
NSString *someString = [arrayOfStrings objectAtIndex:2];
```

The `NSArray` methods `objectEnumerator` and `reverseObjectEnumerator` grant sequential access to the elements of the array, differing only in the direction of travel through the elements. Similarly, the `NSArray` methods `makeObjectsPerformSelector:` and `makeObjectsPerformSelector:withObject:` let you send messages to all objects in the array. In most cases, fast enumeration should be used as it is faster and more flexible than using an `NSEnumerator` or the `makeObjectsPerformSelector:` method. For more on enumeration, see [Enumeration: Traversing a Collection’s Elements](Enumerators.html#//apple_ref/doc/uid/20000135-BBCFABCB).

You can extract a subset of the array (`subarrayWithRange:`) or concatenate the elements of an array of `NSString` objects into a single string (`componentsJoinedByString:`). In addition, you can compare two arrays using the `isEqualToArray:` and `firstObjectCommonWithArray:` methods. Finally, you can create a new array that contains the objects in an existing array and one or more additional objects with `arrayByAddingObject:` or `arrayByAddingObjectsFromArray:`.

There are two principal methods you can use to determine whether an object is present in an array, `[indexOfObject:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSArray/indexOfObject:)` and`[indexOfObjectIdenticalTo:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSArray/indexOfObjectIdenticalTo:)`. There are also two variants, `[indexOfObject:inRange:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSArray/indexOfObject:inRange:)` and `[indexOfObjectIdenticalTo:inRange:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSArray/indexOfObjectIdenticalTo:inRange:)` that you can use to search a range within an array. The `indexOfObject:` methods test for equality by sending elements in the array an `isEqual:` message; the `indexOfObjectIdenticalTo:` methods test for equality using pointer comparison. The difference is illustrated in Listing 2.

**Listing 2**  Searching for an object in an array

```
NSString *yes0 = @"yes";
```

```
NSString *yes1 = @"YES";
```

```
NSString *yes2 = [NSString stringWithFormat:@"%@", yes1];
```

```
 
```

```
NSArray *yesArray = [NSArray arrayWithObjects:yes0, yes1, yes2, nil];
```

```
 
```

```
NSUInteger index;
```

```
 
```

```
index = [yesArray indexOfObject:yes2];
```

```
// index is 1
```

```
 
```

```
index = [yesArray indexOfObjectIdenticalTo:yes2];
```

```
// index is 2
```
## Sorting Arrays
You may need to sort an array based on some criteria. For instance, you may need to place a number of user-created strings into alphabetic order, or you may need to place numbers into increasing or decreasing order. Figure 2 shows an array sorted by last name then first name. Cocoa provides convenient ways to sort the contents of an array, such as sort descriptors, [blocks](../../../../General/Conceptual/DevPedia-CocoaCore/Block.html#//apple_ref/doc/uid/TP40008195-CH3), and [selectors](../../../../General/Conceptual/DevPedia-CocoaCore/Selector.html#//apple_ref/doc/uid/TP40008195-CH48).

**Figure 2**  Sorting arrays### Sorting with Sort Descriptors
Sort descriptors (instances of `[NSSortDescriptor](https://developer.apple.com/documentation/foundation/nssortdescriptor)`) provide a convenient and abstract way to describe a sort ordering. Sort descriptors provide several useful features. You can easily perform most sort operations with minimal custom code. You can also use sort descriptors in conjunction with Cocoa bindings to sort the contents of, for example, a table view. You can also use them with Core Data to order the results of a fetch request.

If you use the methods `[sortedArrayUsingDescriptors:](https://developer.apple.com/documentation/foundation/nsarray/1415069-sortedarrayusingdescriptors)` or `[sortUsingDescriptors:](https://developer.apple.com/documentation/foundation/nsmutablearray/1410745-sortusingdescriptors)`, sort descriptors provide an easy way to sort a collection of objects using a number of their properties. Given an array of dictionaries (custom objects work in the same way), you can sort its contents by last name then first name. Listing 3 shows how to create that array and then sort with descriptors. ([Figure 2](#//apple_ref/doc/uid/20000132-SW12) shows an illustration of this example.)

**Listing 3**  Creating and sorting an array of dictionaries

//First create the array of dictionaries
```
NSString *last = @"lastName";
```

```
NSString *first = @"firstName";
```

```
 
```

```
NSMutableArray *array = [NSMutableArray array];
```

```
NSArray *sortedArray;
```

```
 
```

```
NSDictionary *dict;
```

```
dict = [NSDictionary dictionaryWithObjectsAndKeys:
```

```
                     @"Jo", first, @"Smith", last, nil];
```

```
[array addObject:dict];
```

```
 
```

```
dict = [NSDictionary dictionaryWithObjectsAndKeys:
```

```
                     @"Joe", first, @"Smith", last, nil];
```

```
[array addObject:dict];
```

```
 
```

```
dict = [NSDictionary dictionaryWithObjectsAndKeys:
```

```
                     @"Joe", first, @"Smythe", last, nil];
```

```
[array addObject:dict];
```

```
 
```

```
dict = [NSDictionary dictionaryWithObjectsAndKeys:
```

```
                     @"Joanne", first, @"Smith", last, nil];
```

```
[array addObject:dict];
```

```
 
```

```
dict = [NSDictionary dictionaryWithObjectsAndKeys:
```

```
                     @"Robert", first, @"Jones", last, nil];
```

```
[array addObject:dict];
```

```
 
```

```
//Next we sort the contents of the array by last name then first name
```

```
 
```

```
// The results are likely to be shown to a user
```

```
// Note the use of the localizedStandardCompare: selector
```

```
NSSortDescriptor *lastDescriptor =
```

```
    [[NSSortDescriptor alloc] initWithKey:last
```

```
                               ascending:YES
```

```
                               selector:@selector(localizedStandardCompare:)];
```

```
NSSortDescriptor *firstDescriptor =
```

```
    [[NSSortDescriptor alloc] initWithKey:first
```

```
                               ascending:YES
```

```
                               selector:@selector(localizedStandardCompare:)];
```

```
 
```

```
NSArray *descriptors = [NSArray arrayWithObjects:lastDescriptor, firstDescriptor, nil];
```

```
sortedArray = [array sortedArrayUsingDescriptors:descriptors];
```
It is conceptually and programmatically easy to change the sort ordering and to arrange by first name then last name, as shown in Listing 4.

**Listing 4**  Sorting by first name, last name

NSSortDescriptor *lastDescriptor =
```
    [[NSSortDescriptor alloc] initWithKey:last
```

```
                               ascending:NO
```

```
                               selector:@selector(localizedStandardCompare:)];
```

```
NSSortDescriptor *firstDescriptor =
```

```
    [[NSSortDescriptor alloc] initWithKey:first
```

```
                               ascending:NO
```

```
                               selector:@selector(localizedStandardCompare:)];
```

```
NSArray *descriptors = [NSArray arrayWithObjects:firstDescriptor, lastDescriptor, nil];
```

```
sortedArray = [array sortedArrayUsingDescriptors:descriptors];
```
In particular, it is straightforward to create the sort descriptors from user input.

By contrast, Listing 5 illustrates the first sorting using a function. This approach is considerably less flexible.

**Listing 5**  Sorting with a function is less flexible

```
NSInteger lastNameFirstNameSort(id person1, id person2, void *reverse)
```

```
{
```

```
    NSString *name1 = [person1 valueForKey:last];
```

```
    NSString *name2 = [person2 valueForKey:last];
```

```
 
```

```
    NSComparisonResult comparison = [name1 localizedStandardCompare:name2];
```

```
    if (comparison == NSOrderedSame) {
```

```
 
```

```
        name1 = [person1 valueForKey:first];
```

```
        name2 = [person2 valueForKey:first];
```

```
        comparison = [name1 localizedStandardCompare:name2];
```

```
    }
```

```
 
```

```
    if (*(BOOL *)reverse == YES) {
```

```
        return 0 - comparison;
```

```
    }
```

```
    return comparison;
```

```
}
```

```
 
```

```
BOOL reverseSort = YES;
```

```
sortedArray = [array sortedArrayUsingFunction:lastNameFirstNameSort
```

```
        context:&reverseSort];
```
### Sorting with Blocks
You can use [blocks](../../../../General/Conceptual/DevPedia-CocoaCore/Block.html#//apple_ref/doc/uid/TP40008195-CH3) to help sort an array based on custom criteria. The `[sortedArrayUsingComparator:](https://developer.apple.com/documentation/foundation/nsarray/1411195-sortedarray)` method of `NSArray` sorts the array into a new array, using the block to compare the objects. `NSMutableArray`'s `[sortUsingComparator:](https://developer.apple.com/documentation/foundation/nsmutablearray/1414904-sortusingcomparator)` sorts the array in place, using the block to compare the objects. Listing 6 illustrates sorting with a block.

**Listing 6**  Blocks ease custom sorting of arrays

```
NSArray *sortedArray = [array sortedArrayUsingComparator: ^(id obj1, id obj2) {
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
### Sorting with Functions and Selectors
Listing 7 illustrates the use of the methods `[sortedArrayUsingSelector:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSArray/sortedArrayUsingSelector:)`,`[sortedArrayUsingFunction:context:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSArray/sortedArrayUsingFunction:context:)`, and `[sortedArrayUsingFunction:context:hint:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSArray/sortedArrayUsingFunction:context:hint:)`. The most complex of these methods is `[sortedArrayUsingFunction:context:hint:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSArray/sortedArrayUsingFunction:context:hint:)`. The hinted sort is most efficient when you have a large array (N entries) that you sort once and then change only slightly (P additions and deletions, where P is much smaller than N). You can reuse the work you did in the original sort by conceptually doing a merge sort between the N “old” items and the P “new” items. To obtain an appropriate hint, you use `[sortedArrayHint](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSArray/sortedArrayHint)` when the original array has been sorted, and keep hold of it until you need it (when you want to re-sort the array after it has been modified).

**Listing 7**  Sorting using selectors and functions

NSInteger alphabeticSort(id string1, id string2, void *reverse)
```
{
```

```
    if (*(BOOL *)reverse == YES) {
```

```
        return [string2 localizedStandardCompare:string1];
```

```
    }
```

```
    return [string1 localizedStandardCompare:string2];
```

```
}
```

```
 
```

```
NSMutableArray *anArray =
```

```
    [NSMutableArray arrayWithObjects:@"aa", @"ab", @"ac", @"ad", @"ae", @"af", @"ag",
```

```
        @"ah", @"ai", @"aj", @"ak", @"al", @"am", @"an", @"ao", @"ap", @"aq", @"ar", @"as", @"at",
```

```
        @"au", @"av", @"aw", @"ax", @"ay", @"az", @"ba", @"bb", @"bc", @"bd", @"bf", @"bg", @"bh",
```

```
        @"bi", @"bj", @"bk", @"bl", @"bm", @"bn", @"bo", @"bp", @"bq", @"br", @"bs", @"bt", @"bu",
```

```
        @"bv", @"bw", @"bx", @"by", @"bz", @"ca", @"cb", @"cc", @"cd", @"ce", @"cf", @"cg", @"ch",
```

```
        @"ci", @"cj", @"ck", @"cl", @"cm", @"cn", @"co", @"cp", @"cq", @"cr", @"cs", @"ct", @"cu",
```

```
        @"cv", @"cw", @"cx", @"cy", @"cz", nil];
```

```
// note: anArray is sorted
```

```
NSData *sortedArrayHint = [anArray sortedArrayHint];
```

```
 
```

```
[anArray insertObject:@"be" atIndex:5];
```

```
 
```

```
NSArray *sortedArray;
```

```
 
```

```
// sort using a selector
```

```
sortedArray =
```

```
        [anArray sortedArrayUsingSelector:@selector(localizedStandardCompare:)];
```

```
 
```

```
// sort using a function
```

```
BOOL reverseSort = NO;
```

```
sortedArray =
```

```
        [anArray sortedArrayUsingFunction:alphabeticSort context:&reverseSort];
```

```
 
```

```
// sort with a hint
```

```
sortedArray =
```

```
        [anArray sortedArrayUsingFunction:alphabeticSort
```

```
                                  context:&reverseSort
```

```
                                     hint:sortedArrayHint];
```

> **Important:** If you sort an array of strings that is to be shown to the end user, you should always use a localized comparison. In other languages or contexts, the correct order of a sorted array can be different.

For a general overview of the issues related to sorting, see [Collation Introduction](http://icu-project.org/userguide/Collate_Intro.html).

## Filtering Arrays
The `NSArray` and `NSMutableArray` classes provide methods to filter array contents. `NSArray` provides `[filteredArrayUsingPredicate:](https://developer.apple.com/documentation/foundation/nsarray/1411033-filtered)`, which returns a new array containing objects in the receiver that match the specified predicate. `NSMutableArray` adds `[filterUsingPredicate:](https://developer.apple.com/documentation/foundation/nsmutablearray/1412085-filter)`, which evaluates the receiver’s content against the specified predicate and leaves only objects that match. These methods are illustrated in Listing 8. For more about predicates, see *[Predicate Programming Guide](../../Predicates/AdditionalChapters/Introduction.html#//apple_ref/doc/uid/TP40001789)*.

**Listing 8**  Filtering arrays with predicates

NSMutableArray *array =
```
    [NSMutableArray arrayWithObjects:@"Bill", @"Ben", @"Chris", @"Melissa", nil];
```

```
 
```

```
NSPredicate *bPredicate =
```

```
    [NSPredicate predicateWithFormat:@"SELF beginswith[c] 'b'"];
```

```
NSArray *beginWithB =
```

```
    [array filteredArrayUsingPredicate:bPredicate];
```

```
// beginWithB contains { @"Bill", @"Ben" }.
```

```
 
```

```
NSPredicate *sPredicate =
```

```
    [NSPredicate predicateWithFormat:@"SELF contains[c] 's'"];
```

```
[array filterUsingPredicate:sPredicate];
```

```
// array now contains { @"Chris", @"Melissa" }
```
You can also filter an array using an `NSIndexSet` object. `NSArray` provides `[objectsAtIndexes:](https://developer.apple.com/documentation/foundation/nsarray/1411296-objectsatindexes)`, which returns a new array containing the objects at the indexes in the provided index set. `NSMutableArray` adds  `[removeObjectsAtIndexes:](https://developer.apple.com/documentation/foundation/nsmutablearray/1410154-removeobjects)`, which allows you to filter the array in place using an index set. For more information on index sets, see [Index Sets: Storing Indexes into an Array](index.html#//apple_ref/doc/uid/TP40010165-SW1).

## Pointer Arrays
The `NSPointerArray` class is configured by default to hold objects much as `NSMutableArray` does, except that it can hold `nil` values and that the `[count](https://developer.apple.com/documentation/foundation/nspointerarray/1418453-count)` method reflects those `nil` values. It also allows additional storage options that you can tailor for specific cases, such as when you need advanced [memory management](../../../../General/Conceptual/DevPedia-CocoaCore/MemoryManagement.html#//apple_ref/doc/uid/TP40008195-CH27) options or when you want to hold a specific type of pointer. For example, the pointer array in Figure 3 is configured to hold weak references to its contents. You can also specify whether you want to copy objects entered into the array.

**Figure 3**  Pointer array object ownershipYou can use an `NSPointerArray` object when you want an ordered collection that uses weak references. For example, suppose you have a global array that contains some objects. Because global objects are never collected, none of its contents can be deallocated unless they are held weakly. Pointer arrays configured to hold objects weakly do not own their contents. If there are no strong references to objects within such a pointer array, those objects can be deallocated. For example, the pointer array in [Figure 3](#//apple_ref/doc/uid/20000132-SW11) holds weak references to its contents. Object D and Object E will deallocated.

To create a pointer array , create or initialize it using `[pointerArrayWithOptions:](https://developer.apple.com/documentation/foundation/nspointerarray/1564845-pointerarraywithoptions)` or `[initWithOptions:](https://developer.apple.com/documentation/foundation/nspointerarray/1408229-init)` and the appropriate `[NSPointerFunctionsOptions](https://developer.apple.com/documentation/foundation/nspointerfunctions/options)` options. Alternatively you can initialize it using `[initWithPointerFunctions:](https://developer.apple.com/documentation/foundation/nspointerarray/1416727-init)` and appropriate instances of `[NSPointerFunctions](https://developer.apple.com/documentation/foundation/nspointerfunctions)`. For more information on the various pointer functions options, see [Pointer Function Options](pointer.html#//apple_ref/doc/uid/TP40010199-SW1).

The `NSPointerArray` class also defines a number of convenience constructors for creating a pointer array with strong or weak references to its contents. For example, `[pointerArrayWithWeakObjects](https://developer.apple.com/documentation/foundation/nspointerarray/1564846-pointerarraywithweakobjects)` creates a pointer array that holds weak references to its contents. These convenience constructors should only be used if you are storing objects.

To configure a pointer array to use arbitrary pointers, you can initialize it with both the `NSPointerFunctionsOpaqueMemory` and `NSPointerFunctionsOpaquePersonality` options. For example, you can add a pointer to an `int` value using the approach shown in Listing 9.

**Listing 9**  Pointer array configured for non-object pointers

NSPointerFunctionsOptions options=(NSPointerFunctionsOpaqueMemory |
```
     NSPointerFunctionsOpaquePersonality);
```

```
 
```

```
NSPointerArray *ptrArray=[NSPointerArray pointerArrayWithOptions: options];
```

```
 
```

```
[ptrArray addPointer: someIntPtr];
```
You can then access an integer as show below.

NSLog(@" Index 0 contains: %i", *(int *) [ptrArray pointerAtIndex: 0] );When configured to use arbitrary pointers, a pointer array has the risks associated with using pointers. For example, if the pointers refer to stack–based data created in a function, those pointers are not valid outside of the function, even if the pointer array is. Trying to access them will lead to undefined behavior.

        
            [Next](Dictionaries.html)[Previous](../Collections.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-09-01