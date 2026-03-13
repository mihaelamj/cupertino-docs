---
title: "Index Sets: Storing Indexes into an Array"
book: "Collections Programming Topics"
chapterId: "TP40010165"
date: "2010-09-01"
description: "Explains how to group objects in arrays, sets, or dictionaries in Cocoa."
identifier: "//apple_ref/doc/uid/10000034i"
source: apple-archive
---

# Index Sets: Storing Indexes into an Array

> **Collections Programming Topics**
> Last updated: 2010-09-01

[Next](IndexPaths.html)[Previous](Sets.html)
        
        
        [](../index.html)
        
        # Index Sets: Storing Indexes into an Array
You use index sets to store indexes into some other data structure, such as an `NSArray` object. Each index in an index set can only appear once, which is why index sets are not suitable for storing arbitrary collections of integers. Because index sets (as in Figure 1) make use of ranges to store indexes, they are usually more efficient than storing a collection of integer values, such as in an array.

**Figure 1**  Index set and array interaction## Index Set Fundamentals
An `NSIndexSet` object manages an immutable set of indexes—that is, after you create the index set, you cannot add indexes to it or remove indexes from it.

An `NSMutableIndexSet` object manages a [mutable](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectMutability.html#//apple_ref/doc/uid/TP40008195-CH42) index set, which allows the addition and deletion of indexes at any time, automatically allocating memory as needed.

You can easily [create](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39) an instance of one type of index set from the other using the initializer `[initWithIndexSet:](https://developer.apple.com/documentation/foundation/nsindexset/1415602-initwithindexset)`. This is particularly useful if you want to create an immutable index set containing disjoint sets of indexes, which are typically created using mutable index sets. For example, if you have an `NSMutableIndexSet` object named `myIndexes`, which has had the indexes added to it, you can create an immutable copy as follows:

NSIndexSet *myImmutableIndexes=[[NSIndexSet alloc] initWithIndexSet: myIndexes];You can also initialize an index set from a single index or a range of indexes by using the `[initWithIndex:](https://developer.apple.com/documentation/foundation/nsindexset/1416501-init)` or `[initWithIndexesInRange:](https://developer.apple.com/documentation/foundation/nsindexset/1414013-initwithindexesinrange)` method.

## Mutable Index Sets
The methods of the `NSMutableIndexSet` class allow you to add or remove additional indexes or index ranges. You can, for example, store disjoint sets of indexes and modify preexisting sets of indexes as needed. Some of these methods are listed below:

`[addIndex:](https://developer.apple.com/documentation/foundation/nsmutableindexset/1410712-addindex)`

`[addIndexesInRange:](https://developer.apple.com/documentation/foundation/nsmutableindexset/1408251-addindexesinrange)`

`[removeIndex:](https://developer.apple.com/documentation/foundation/nsmutableindexset/1410650-remove)`

`[removeIndexesInRange:](https://developer.apple.com/documentation/foundation/nsmutableindexset/1415791-removeindexesinrange)`

If you have an empty `NSMutableIndexSet` object named `myDisjointIndexes`, you can fill it with the indexes: 1, 2, 5, 6, 7, and 10, as shown in Listing 1.

**Listing 1**  Adding indexes to a mutable index set

```
[myDisjointIndexes addIndexesInRange: NSMakeRange(1,2)];
```

```
[myDisjointIndexes addIndexesInRange: NSMakeRange(5,3)];
```

```
[myDisjointIndexes addIndex: 10];
```
## Iterating Through Index Sets
To access all of the objects indexed by an index set, it may be convenient to iterate sequentially through the index set. Iterating through the index set, rather than through the corresponding array, is more efficient, as it allows you to examine only the indexes that you are interested in. If you have an `NSArray` object named `anArray` and an `NSIndexSet` object named `anIndexSet`, you can iterate forward through an index set as shown in Listing 2.

**Listing 2**  Forward iteration through an index set

NSUInteger index=[anIndexSet firstIndex];
```
 
```

```
while(index != NSNotFound)
```

```
{
```

```
 
```

```
     NSLog(@" %@",[anArray objectAtIndex:index]);
```

```
     index=[anIndexSet indexGreaterThanIndex: index];
```

```
}
```
Sometimes it may be necessary to iterate backward through an index set, for example, when you want to selectively remove objects from indexes from an `NSMutableArray` object. You can iterate backward through an index set as shown in Listing 3.

**Listing 3**  Reverse iteration through an index set

NSUInteger index=[anIndexSet lastIndex];
```
 
```

```
while(index != NSNotFound)
```

```
{
```

```
 
```

```
     if([[aMutableArray objectAtIndex: index] isEqualToString:@"G"]){
```

```
          [aMutableArray removeObjectAtIndex:index];
```

```
     }
```

```
     index=[anIndexSet indexLessThanIndex: index];
```

```
}
```
The above approach should be used only if you want to selectively remove objects referred to by an index set. If you want to remove the objects at all the indexes in an index set, use `[removeObjectsAtIndexes:](https://developer.apple.com/documentation/foundation/nsmutablearray/1410154-removeobjects)` instead.

## Index Sets and Blocks
Index sets are especially powerful when used in conjunction with [blocks](../../../../General/Conceptual/DevPedia-CocoaCore/Block.html#//apple_ref/doc/uid/TP40008195-CH3). Blocks allow you to create index sets that designate the members of an array which pass some test. For example, if you have an unsorted array of numbers and you want to create an index set which holds indexes to all numbers less than 20, you use something similar to Listing 4.

**Listing 4**  Creating an index set from an array using a block

NSIndexSet *lessThan20=[someArray indexesOfObjectsPassingTest:^(id obj, NSUInteger index, BOOL *stop){
```
     if ([obj isLessThan:[NSNumber numberWithInt:20]]){
```

```
          return YES;
```

```
     }
```

```
     return NO;
```

```
}];
```
Index sets can also be used in block-based enumeration of an array. To enumerate only the indexes of the array contained in the index set, use the `[enumerateObjectsAtIndexes:options:usingBlock:](https://developer.apple.com/documentation/foundation/nsarray/1417577-enumerateobjectsatindexes)` method.

Alternatively, the index set itself can be enumerated using a block with the `[enumerateIndexesUsingBlock:](https://developer.apple.com/documentation/foundation/nsindexset/1411395-enumerateindexesusingblock)` method. For example, you can perform some task for each object whose index is in the set. You can even access objects from multiple arrays provided the index set is valid for the arrays used, as in Listing 5.

**Listing 5**  Enumerating an index set to access multiple arrays

```
[anIndexSet enumerateIndexesUsingBlock:^(NSUInteger idx, BOOL *stop){
```

```
     if([[firstArray objectAtIndex: idx] isEqual:[secondArray objectAtIndex: idx]]){
```

```
          NSLog(@"Objects at %i Equal",idx);
```

```
     }
```

```
}];
```

        
            [Next](IndexPaths.html)[Previous](Sets.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-09-01