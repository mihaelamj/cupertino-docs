---
title: "Part IV: Managing the Object Life Cycle"
book: "Core Data Programming Guide"
chapterId: "TP40001075-CH31"
date: "2017-03-27"
description: "Explains how to manage objects using the Core Data framework."
identifier: "//apple_ref/doc/uid/TP40001075"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Part IV: Managing the Object Life Cycle

> **Core Data Programming Guide**
> Last updated: 2017-03-27

## Managed Objects and References

  
  	
  		
  References determine when managed objects are released from memory and also affect what causes them to be retained.

		 

  
  
  ### Weak References Between Managed Objects and the Context

  
  Managed objects know what managed object context they’re associated with, and managed object contexts know what managed objects they contain. *By default*, though, the references between a managed object and its context are weak. This means that in general you cannot rely on a context to ensure the longevity of a managed object instance, and you cannot rely on the existence of a managed object to ensure the longevity of a context. Put another way, just because you fetched an object doesn’t mean it will stay around. 

  The exception to this rule is that a managed object context maintains a strong reference to any changed (inserted, deleted, and updated) objects until the pending transaction is committed (with a `save:`) or discarded (with a `[reset](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506807-reset)` or `[rollback](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506942-rollback)`). Note that the undo manager may also keep strong references to changed objects—see [Change Management](ChangeManagement.html#//apple_ref/doc/uid/TP40001075-CH22-SW1).

  

  
  ### Creating a Strong Reference Between Managed Objects and the Context

  
  You can change a context’s default behavior so that it does keep strong references to its managed objects by setting the `[retainsRegisteredObjects](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506290-retainsregisteredobjects)` property to `true`. Doing so makes the life of the managed objects dependent on the life of the context. This dependence can be convenient if you are caching smaller data sets in memory. For example, suppose the context controls a temporary set of objects that may persist beyond a single event cycle, such as a sheet in an macOS application or a modal view in an iOS application. Making managed objects dependent on the context can also be useful if you are using multiple threads and passing data between them—for example, if you are performing a background fetch and passing object IDs to the main thread. The background thread needs to keep strong references to the objects it prefetched for the main thread until it knows the main thread has actually used the object IDs to fault local instances into the main thread. Otherwise the objects may fall out of memory and need to be retrieved again from disk.

  Use a separate container to keep strong references only to those managed objects you really need. You can use an array or a dictionary or an object controller (for example, an `[NSArrayController](https://developer.apple.com/documentation/appkit/nsarraycontroller)` instance) that has strong references to the objects it manages. The managed objects you don’t need will then be deallocated when possible, such as when relationships are cleared (see [Breaking Strong References Between Objects](#//apple_ref/doc/uid/TP40001075-CH31-SW9)). 

  If you have finished with a managed object context, or for some other reason you want to disconnect a context from its persistent store coordinator, do *not* set the context’s coordinator to `nil`.

  Instead, simply relinquish ownership of the context and allow it to be deallocated normally.

  
  
      
          Objective-C

        

            [self setMyManagedObjectContext:nil];

        

      
      
          Swift

        

            - `myManagedObjectContext = nil`

        

      
  

  

  
  ### Breaking Strong References Between Objects

  
  As opposed to the default behavior between managed objects and their managed object contexts, with relationships between managed objects, each object maintains a strong reference to the object or objects to which it is related. This relationship can cause strong reference cycles that in turn can cause objects to be held in memory long past their usefulness. To ensure that reference cycles are broken, when you are finished with an object, you can use the managed object context method `[refreshObject:mergeChanges:](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506224-refreshobject)` to turn the managed object into a fault. 

  You typically use `refreshObject:mergeChanges:` to refresh a managed object’s property values. If the `mergeChanges` flag is `YES``true`, the method merges the object’s property values with those of the object available in the persistent store coordinator. If the flag is `NO``false`, however, the method simply turns an object back into a fault without merging, which causes it to break strong references to related managed objects.

  Before a managed object can be deallocated, any strong references to it must be removed, including those from outside of Core Data. See also [Change Management](ChangeManagement.html#//apple_ref/doc/uid/TP40001075-CH22-SW1).

  

  
  ### Strong References in Change and Undo Management

  
  A context keeps strong references to managed objects that have pending changes (insertions, deletions, or updates)  until the context is sent a `save:`, `reset` , `rollback`, or `dealloc` call, or the context is sent the appropriate number of undos to undo the change. If the calls to undo cause all changes to be undone on a particular object, the object’s strong reference reverts back to a weak reference.

  *The undo manager associated with a context keeps strong references to any changed managed objects.* By default, the context’s undo manager keeps an unlimited undo and redo stack. To limit your application's memory footprint, make sure that you scrub (using `[removeAllActions](https://developer.apple.com/documentation/foundation/nsundomanager/1407442-removeallactions)`) the context’s undo stack when appropriate.

  If you do not intend to use Core Data’s undo functionality, you can reduce your application's resource requirements by setting the context’s undo manager to `nil`. This may be especially beneficial for background worker threads as well as for large import or batch operations.

  

  
  ### Ensuring Data Is Up to Date

  
  If two applications are using the same data store, or a single application has multiple persistence stacks, it is possible for managed objects in one managed object context or persistent object store to get out of sync with the contents of the repository. If this occurs, you need to refresh the data in the managed objects, and in particular the persistent object store (the snapshots), to ensure that the data values are current.

  
  
  ### Refreshing an Object

  
  Managed objects whose property values are populated from the persistent store (realized objects), as well as pending updated, inserted, or deleted objects, are never changed by a fetch operation without developer intervention. For example, consider a scenario in which you fetch some objects and modify them in one editing context; meanwhile, in another editing context you edit the same data and commit the changes. If in the first editing context you then execute a new fetch that returns the same objects, you do not see the newly committed data values—you see the existing objects in their current in-memory state.

  To refresh a managed object's property values, you use the managed object context method `[refreshObject:mergeChanges:](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506224-refreshobject)`. If the `mergeChanges` flag is `YES``true`, the method merges the object's property values with those of the object available in the persistent store coordinator. If the flag is set to `NO``false`, the method simply turns an object back into a fault without merging, which also causes strong references to other related managed objects to be broken. Thus you can use this method to trim the portion of your object graph that you want to hold in memory.

  An object’s staleness interval is the time that has to pass until the store refetches the snapshot. The staleness interval only affects firing faults; moveover, it is only relevant for incremental (that is, SQLite) stores. The other stores never refetch because the entire data set is kept in memory.

  

  
  ### Merging Changes with Transient Properties

  
  A transient property is a property on an object that is not persisted to disk. If you use `[refreshObject:mergeChanges:](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506224-refreshobject)` with the `mergeChanges` flag `YES``true`, then any transient properties are restored to their prerefresh value after `[awakeFromFetch](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506424-awakefromfetch)` is invoked. This means that, if you have a transient property with a value that depends on a property that is refreshed, the transient value may become out of sync.

  Consider an application in which you have a Person entity with attributes `firstName` and `lastName` and a *cached* transient derived property, `fullName`. (In practice it might be unlikely that a `fullName` attribute would be cached, but the example is easy to understand.) Suppose also that `fullName` is calculated and cached in a custom `[awakeFromFetch](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506424-awakefromfetch)` method.

  An Employee, currently named "Sarit Smith" in the persistent store, is edited in two managed object contexts:

  
  In context one, the corresponding instance's `firstName` is changed to "Fiona," which causes the cached `fullName` to be updated to "Fiona Smith," and the context is saved. 

  In the persistent store, the employee is now "Fiona Smith."

  In context two, the corresponding instance's `lastName` is changed to "Jones," which causes the cached `fullName` to be updated to "Sarit Jones."

  The object is then refreshed with the `mergeChanges` flag `YES``true`. The refresh fetches "Fiona Smith" from the store. The refresh updates the object as follows:

  
  `firstName` was *not* changed prior to the refresh; the refresh causes it to be updated to the new value from the persistent store, so it is now "Fiona."

  `lastName`*was* changed prior to the refresh; after the refresh, it is set back to its modified value—"Jones."

  The *transient* value, `fullName`, was also changed prior to the refresh. After the refresh, its value is restored to "Sarit Jones." However, to be correct, it should be "Fiona Jones."

  The example shows that, because prerefresh values are applied *after* `[awakeFromFetch](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506424-awakefromfetch)`, you cannot use `[awakeFromFetch](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506424-awakefromfetch)` to ensure that a transient value is properly updated following a refresh (or if you do, the value will subsequently be overwritten). In these circumstances, the best solution is to use an additional instance variable to note that a refresh has occurred and that the transient value should be recalculated.

  

  	
 	
    		[Using Core Data with Cocoa Bindings](CocoaBindings.html#//apple_ref/doc/uid/TP40001075-CH12-SW1)

  			[Creating Managed Object Relationships](HowManagedObjectsarerelated.html#//apple_ref/doc/uid/TP40001075-CH17-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2017-03-27](RevisionHistory.html#//apple_ref/doc/uid/TP40003204-SW1)