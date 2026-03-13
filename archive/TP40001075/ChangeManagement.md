---
title: "Part V: Advanced Topics"
book: "Core Data Programming Guide"
chapterId: "TP40001075-CH22"
date: "2017-03-27"
description: "Explains how to manage objects using the Core Data framework."
identifier: "//apple_ref/doc/uid/TP40001075"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Part V: Advanced Topics

> **Core Data Programming Guide**
> Last updated: 2017-03-27

## Change Management

  
  	
  		
  If your application contains more than one managed object context and you allow objects to be modified in more than one context, you need to be able to reconcile the changes. This is a fairly common situation when an application is importing data from a network and the user can also edit that data.

		 

  
  
  ### Multiple Contexts in One Application

  
  The object graph associated with any given managed object context must be internally consistent. If you have multiple managed object contexts in the same application, however, it is possible that each may contain objects that represent the same records in the persistent store, but whose characteristics are mutually inconsistent. In an employee application, for example, you might have two separate windows that display the same set of employees, but distributed between different departments and with different managers, as shown in Figure 15-1. Note how the manager relationship has moved from Lau to Weiss. Managed object context 1 still represents what is on disk, but managed object context 2 has changed.

  
  **Figure 15-1**Managed object contexts with mutually inconsistent data values
  

  Ultimately there can be only one truth, and differences between these views must be detected and reconciled when data is saved. When one of the managed object contexts is saved, its changes are pushed through the persistent store coordinator to the persistent store. When the second managed object context is saved, conflicts are detected using a mechanism called optimistic locking. How the conflicts are resolved depends on how you have configured the context.

  

  
  ### Conflict Detection and Optimistic Locking

  
  When Core Data fetches an object from a persistent store, it takes a *snapshot* of its state. A snapshot is a dictionary of an object’s persistent properties—typically all its attributes and the global IDs of any objects to which it has a to-one relationship. Snapshots participate in optimistic locking. When the framework saves, it compares the values in each edited object’s snapshot with the then-current corresponding values in the persistent store. 

  
  If the values are the same, the store has not been changed since the object was fetched, so the save proceeds normally. As part of the save operation, the snapshots' values are updated to match the saved data. 

  If the values differ, the store has been changed since the object was fetched or last saved; this represents an optimistic locking failure. You must resolve the conflict.

  

  
  ### Choosing a Merge Policy

  
  You can get an optimistic locking failure if more than one Core Data stack references the same external data store regardless of whether you have multiple Core Data stacks in a single application or you have multiple applications. It is possible that the same conceptual managed object will be edited in two persistence stacks simultaneously. You may want to ensure that subsequent changes made by the second stack do not overwrite changes made by the first, but other behaviors may be appropriate. Choose a merge policy for the managed object context that is suitable for your situation. 

  The default behavior is defined by an `[NSErrorMergePolicy](https://developer.apple.com/documentation/coredata/nserrormergepolicy)` property. This policy causes a save to fail if there are any merge conflicts.  The save method returns an error with a `[userInfo](https://developer.apple.com/documentation/foundation/nserror/1411580-userinfo)` dictionary that contains the key `@"conflictList"`; the corresponding value is an array of conflict records. You use the array to tell the user the differences between the values the user is trying to save and those current in the store. In this scenario the user must fix the conflicts (by refetching objects so that the snapshots are updated). 

  Alternatively, you specify a different policy. The `NSErrorMergePolicy` is the only policy that generates an error. Other policies—`[NSMergeByPropertyStoreTrumpMergePolicy](https://developer.apple.com/documentation/coredata/nsmergebypropertystoretrumpmergepolicy)`, `[NSMergeByPropertyObjectTrumpMergePolicy](https://developer.apple.com/documentation/coredata/nsmergebypropertyobjecttrumpmergepolicy)`, and `[NSOverwriteMergePolicy](https://developer.apple.com/documentation/coredata/nsoverwritemergepolicy)`—allow the save to proceed by merging the state of the edited objects with the state of the objects in the store in different ways. The `[NSRollbackMergePolicy](https://developer.apple.com/documentation/coredata/nsrollbackmergepolicy)` discards in-memory state changes for objects in conflict and uses the persistent store’s version of the objects’ state.

  

  
  ### Automatic Snapshot Management

  
  An application that fetches hundreds of rows of data can build up a large cache of snapshots. Theoretically, if enough fetches are performed, a Core Data-based application can contain all the contents of a store in memory. Clearly, snapshots must be managed in order to prevent this situation.

  Responsibility for cleaning up snapshots rests with a mechanism called *snapshot reference counting*. This mechanism keeps track of the managed objects that are associated with a particular snapshot—that is, managed objects that contain data from a particular snapshot. When there are no remaining managed object instances associated with a particular snapshot (which Core Data determines by maintaining a list of strong references), Core Data automatically breaks the reference to the snapshot and it is removed from memory.

  

  
  ### Synchronizing Changes Between Contexts

  
  If you use more than one managed object context in an application, Core Data does not automatically notify one context of changes made to objects in another. In general, this is because a context is intended to be a scratch pad where you can make changes to objects in isolation, and you can discard the changes without affecting other contexts. If you do need to synchronize changes between contexts, how a change should be handled depends on the user-visible semantics you want in the second context, and on the state of the objects in the second context. 

  
  
  ### Registering with NSNotificationCenter

  
  Consider an application with two managed object contexts and a single persistent store coordinator. If a user deletes an object in the first context (`moc1`), you may need to inform the second context (`moc2`) that an object has been deleted. In all cases, `moc1` automatically posts an `[NSManagedObjectContextDidSaveNotification](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontextdidsavenotification)` [notification](../../../General/Conceptual/DevPedia-CocoaCore/Notification.html#//apple_ref/doc/uid/TP40008195-CH35) via the `[NSNotificationCenter](../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNotificationCenter/Description.html#//apple_ref/occ/cl/NSNotificationCenter)` that your application should register for and use as the trigger for whatever actions it needs to take. This notification contains information not only about deleted objects, but also about changed objects. You need to handle these changes because they may be the result of the delete. Most of these types of changes involve transient relationships or fetched properties.

  

  
  ### Choosing a Synchronization Strategy

  
  When deciding how you want to handle your delete  notification, consider:

  
  What other changes exist in the second context?

  Does the instance of the object that was deleted have changes in the second context?

  Can the changes made in the second context be undone?

  These concerns are somewhat orthogonal, and what actions you take to synchronize the contexts depend on the semantics of your application. The following three strategies are presented in order of increasing complexity.

  
  The object itself has been deleted in `moc1` but has not changed in `moc2`. In that situation you do not have to worry about undo, and you can just delete the object in `moc2`. The next time `moc2` saves, the framework will notice that you are trying to redelete an object, ignore the optimistic locking warning, and continue without error.

  If you do not care about the contents of `moc2`, you can simply reset it (using `[reset](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506807-reset)`) and refetch any data you need after the reset. This resets the undo stack as well, and the deleted object is now gone. The only issue here is determining what data to refetch. Do this before you reset by collecting the IDs (`[objectID](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506848-objectid)`) of the managed objects you still need and using those to reload after the reset has happened. You must exclude the deleted IDs, and it is best to create fetch requests with `IN` predicates to ensure faults are fulfilled for deleted IDs.

  If the object has changed in `moc2`, but you do not care about undo, your strategy depends on what it means for the semantics of your application. If the object that was deleted in `moc1` has changes in `moc2`, should it be deleted from `moc2` as well? Or should it be resurrected and the changes saved? What happens if the original deletion triggered a cascade delete for objects that have not been faulted into `moc2`? What if the object was deleted as part of a cascade delete?

  There are two workable options:

  
  Simply discard the changes by deleting the object in the `moc` that is receiving the notification.

  Alternatively, if the object is standalone, set the merge policy on the context to `[NSOverwriteMergePolicy](https://developer.apple.com/documentation/coredata/nsoverwritemergepolicy)`. This policy will cause the changes in the second context to overwrite the delete in the database.

  it will cause *all* changes in `moc2` to overwrite any changes made in `moc1`.

  The preceding solutions are least likely to leave your object graph in an unsustainable state as a result of something you missed. If you find your application hitting merge issues that you are not able to resolve, this generally indicates an issue with the application’s architecture.

  

  	
 	
    		[Object Validation](ObjectValidation.html#//apple_ref/doc/uid/TP40001075-CH20-SW1)

  			[Persistent Store Types and Behaviors](PersistentStoreFeatures.html#//apple_ref/doc/uid/TP40001075-CH23-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2017-03-27](RevisionHistory.html#//apple_ref/doc/uid/TP40003204-SW1)