---
title: "Index Paths: Storing a Path Through Nested Arrays"
book: "Collections Programming Topics"
chapterId: "TP40010167"
date: "2010-09-01"
description: "Explains how to group objects in arrays, sets, or dictionaries in Cocoa."
identifier: "//apple_ref/doc/uid/10000034i"
source: apple-archive
---

# Index Paths: Storing a Path Through Nested Arrays

> **Collections Programming Topics**
> Last updated: 2010-09-01

[Next](Copying.html)[Previous](index.html)
        
        
        [](../index.html)
        
        # Index Paths: Storing a Path Through Nested Arrays
Index paths store the path through a nested set of arrays and are used to retrieve an object in a more complicated collection hierarchy, such as a tree. Figure 1, for example, shows a nested set of arrays which represents the hierarchy of a hypothetical company.

**Figure 1**  Nested arrays and index paths## Index Path Fundamentals
If you consider the hierarchy of the hypothetical company shown in [Figure 1](#//apple_ref/doc/uid/TP40010167-SW4), the root array consists of a single entry for the CEO. The array below that consists of the various vice presidents. Below each vice president is an array of directors, and so on. If you want to store the position of a particular employee on the Europe marketing team, for instance, a simple index is not enough. Instead a path through the nested arrays is necessary. In this case, Bill T. can be represented by the index path 0.0.1.1.2.

You can create an index path using from a single index or from a C-style array of `NSUInteger` values. Listing 1 shows how to create the index path to Bill T.

**Listing 1**  Creating an index path from an array

NSUInteger arrayLength = 5;
```
NSUInteger integerArray[] = {0,0,1,1,2};
```

```
NSIndexPath *aPath = [[NSIndexPath alloc] initWithIndexes:integerArray length:arrayLength];
```
You can also create an index path automatically from many of the more complex hierarchy collection classes. See the `[indexPath](https://developer.apple.com/documentation/appkit/nstreenode/1532255-indexpath)` method of the `[NSTreeNode](https://developer.apple.com/documentation/appkit/nstreenode)` class for an example.

## Using Index Paths
`NSIndexPath` provides methods for querying the elements in the path. For example, `[indexAtPosition:](https://developer.apple.com/documentation/foundation/nsindexpath/1414179-index)` returns the index stored at the given position in the index path. You can also create new index paths by adding a new index or removing the last index. A few classes use index paths extensively to manage their contents. `NSTreeController` is one such example. For more information on `NSTreeController` and index paths, see *[Cocoa Bindings Programming Topics](../../CocoaBindings/CocoaBindings.html#//apple_ref/doc/uid/10000167i)*.

In iOS, `UITableView` and its [delegate](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14) and data source use index paths to manage much of their content and to handle user interaction. To assist with this, UIKit  adds programming interfaces to `NSIndexPath` to incorporate the rows and sections of a table view more fully into index paths. For more information, see *NSIndexPath UIKit Additions*. For instance, index paths are used to designate user selections using the `[tableView:didSelectRowAtIndexPath:](https://developer.apple.com/documentation/uikit/uitableviewdelegate/1614877-tableview)` delegate method.

        
            [Next](Copying.html)[Previous](index.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-09-01