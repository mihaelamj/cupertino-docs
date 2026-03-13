---
title: "Part VI: Troubleshooting and FAQs"
book: "Core Data Programming Guide"
chapterId: "TP40001075-CH26"
date: "2017-03-27"
description: "Explains how to manage objects using the Core Data framework."
identifier: "//apple_ref/doc/uid/TP40001075"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Part VI: Troubleshooting and FAQs

> **Core Data Programming Guide**
> Last updated: 2017-03-27

## Troubleshooting Core Data

  
  	
  		
  Core Data builds on functionality provided by other parts of Cocoa. When diagnosing a problem with an application that uses Core Data, take care to distinguish between problems that are specific to Core Data and those that stern from an error with another framework or that are architecture-related. Poor performance, for example, may not be a Core Data problem, but instead due to a failure to observe standard Cocoa techniques of memory management or resource conservation. If a user interface does not update properly, this may be due to an error in how you have configured Cocoa bindings.

		 

  
  
  ### Object Life Cycle Problems

  
  
  
  ### Merge errors

  
  *Problem:* You see the error message, "`Could not merge changes`."

  *Cause:* Two different managed object contexts tried to change the same data. This is also known as an optimistic locking failure.

  *Remedy:* Either set a merge policy on the context, or manually (programmatically) resolve the failure. You can retrieve the currently committed values for an object using `[committedValuesForKeys:](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506771-committedvaluesforkeys)`, and you can use `refreshObject:mergeChanges:` to refault the object, so that when it is next accessed, its data values are retrieved from its persistent store.

  

  
  ### Assigning a managed object to a different store

  
  *Problem:* You see an exception that looks similar to this example.

  
  
      
        

            - `<NSInvalidArgumentException> [<MyMO 0x3036b0>_assignObject:toPersistentStore:]:`

            - `Can’t reassign an object to a different store once it has been saved.`

        

      
  

  *Cause:* The object you are trying to assign to a store has already been assigned and saved to a different store.

  *Remedy:* To move an object from one store to another, you must create a new instance, copy the information from the old object, save it to the appropriate store, and then delete the old instance.

  

  
  ### Fault cannot be fulfilled

  
  *Problem:* You see the error message, `NSObjectInaccessibleException`; "Core Data could not fulfill a fault."

  *Cause:* The object that Core Data is trying to realize has been deleted from the persistent store.

  *Remedy:* Discard this object by removing all references to it.

  *Details:* This problem can occur in at least two situations:

  First situation:

  
  You started with a strong reference to a managed object from another object in your application.

  You deleted the managed object through the managed object context.

  You saved changes on the object context. 

  At this point, the deleted object has been turned into a fault. It isn’t destroyed because doing so would violate the rules of memory management.

  Core Data will try to realize the faulted managed object but will fail to do so because the object has been deleted from the store. That is, there is no longer an object with the same global ID in the store. 

  Second situation:

  
  You deleted an object from a managed object context.

  The deletion failed to break all relationships from other objects to the deleted object.

  You saved changes.

  At this point, if you try to fire the fault of a relationship from another object to the deleted object, it may fail, depending on the configuration of the relationship, which affects how the relationship is stored. 

  The delete rules for relationships affect relationships only from the source object to other objects (including inverses). Without potentially fetching large numbers of objects, possibly without reason, there is no way for Core Data to efficiently clean up the relationships to the object.

  Keep in mind that a Core Data object graph is directional. That is, a relationship has a source and a destination. Following a source to a destination does not necessarily mean that there is an inverse relationship. So, in that sense, you need to ensure that you are properly maintaining the object graph across deletes.

  Core Data uses inverse relationships to maintain referential integrity within the data model. If no inverse relationship exists and an object is deleted, you will be required to clean up that relationship manually.

  In practice, a well-designed object graph does not require much manual post-deletion clean-up. Most object graphs have entry points that in effect act as a root node for navigating the graph, and most insertion and deletion events are rooted at those nodes just like fetches. This means that delete rules take care of most of the work for you. Similarly, because smart groups and other loosely coupled relationships are generally best implemented with fetched properties, various ancillary collections of entry points into the object graph generally do not need to be maintained across deletes, because fetched relationships have no notion of permanence when it comes to objects found through the fetched relationship.

  

  
  ### Managed object invalidated

  
  *Problem:* You see an exception that looks similar to this example:

  
  
      
        

            - `<NSObjectInaccessibleException> [<MyMO 0x3036b0>_assignObject:toPersistentStore:]:`

            - `The NSManagedObject with ID:#### has been invalidated.`

        

      
  

  *Cause:* Either you have removed the store for the fault you are attempting to fire, or the managed object's context has been sent a `[reset](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506807-reset)`.

  *Remedy:* Discard this object by removing all references to it. If you add the store again, you can try to fetch the object again.

  

  
  ### Class is not key-value coding-compliant

  
  *Problem:* You see an exception that looks similar to the following example.

  
  
      
        

            - `<NSUnknownKeyException> [<MyMO 0x3036b0> valueForUndefinedKey:]:`

            - `this class is not key value coding-compliant for the key randomKey.`

        

      
  

  *Cause:* Either you used an incorrect key, or you initialized your managed object with `init` instead of `[initWithEntity:insertIntoManagedObjectContext:](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506357-init)`.

  *Remedy:* Use a valid key (check the spelling and case carefully—also review the rules for key-value coding compliance in *[Key-Value Coding Programming Guide](../KeyValueCoding/index.html#//apple_ref/doc/uid/10000107i)*). Ensure that you use the designated initializer for `NSManagedObject` (see `[initWithEntity:insertIntoManagedObjectContext:](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506357-init)`).

  

  
  ### Entity class does not respond to invocations of custom methods

  
  *Problem:* You define an entity that uses a custom subclass of `NSManagedObject`, then in code you create an instance of the entity and invoke a custom method, as illustrated in this code fragment:

  
  
      
          Objective-C

        

            NSManagedObject *entityInstance = [NSEntityDescription insertNewObjectForEntityForName:@"MyEntity" inManagedObjectContext:moc];

            [entityInstance setAttribute: newValue];

        

      
      
          Swift

        

            - `let entityInstance = NSEntityDescription.insertNewObjectForEntityForName("MyEntity", inManagedObjectContext: moc)`

            - `entityInstance.attribute = newValue`

        

      
  

  You get a runtime error like this:

  
  
      
        

            - `"2005-05-05 15:44:51.233 MyApp[1234] ***`

            - `    -[NSManagedObject setNameOfEntity:]: selector not recognized [self = 0x30e340]`

        

      
  

  *Cause:* In the model, you misspelled the name of the custom class for the entity.

  *Remedy:* Ensure that the spelling of the custom class name in the model matches the spelling of the custom class you implement.

  

  
  ### Custom accessor methods are not invoked, and key dependencies are not obeyed

  
  *Problem:* You define a custom subclass of `[NSManagedObject](https://developer.apple.com/documentation/coredata/nsmanagedobject)` for a particular entity and implement custom accessors methods (and perhaps dependent keys). At runtime, the accessor methods are not called, and the dependent key is not updated.

  *Cause:* In the model, you did not specify the custom class for the entity.

  *Remedy:* Ensure that the model specifies the custom class name for the entity rather than `[NSManagedObject](https://developer.apple.com/documentation/coredata/nsmanagedobject)`.

  

  
  ### Problems with Fetching

  
  
  
  ### SQLite store does not work with sorting

  
  *Problem:* You create a sort descriptor that uses a comparison method defined by [NSString](https://developer.apple.com/documentation/foundation/nsstring), such as the following:

  
  
      
          Objective-C

        

            NSSortDescriptor *mySortDescriptor = [[NSSortDescriptor alloc] initWithKey:@"lastName" ascending:YES selector:@selector(localizedCaseInsensitiveCompare:)];

        

      
      
          Swift

        

            - `let mySortDescriptor = NSSortDescriptor(key: "lastName", ascending: true, selector: #(localizedCaseInsensitiveCompare:))`

        

      
  

  You then either use this descriptor with a fetch request or as one of an array controller's sort descriptors. At runtime, you see an error message that looks similar to the following:

  
  
      
        

            NSRunLoop ignoring exception 'unsupported NSSortDescriptor selector:

                    localizedCaseInsensitiveCompare:' that raised during posting of

                    delayed perform with target 3e2e42 and selector 'invokeWithTarget:'

        

      
  

  *Cause:* Exactly how a fetch request is executed depends on the store—see [Fetching Objects](FetchingObjects.html#//apple_ref/doc/uid/TP40001075-CH6-SW1).

  *Remedy:* If you are executing the fetch directly, do not use Cocoa-based sort operators—instead, sort the returned array in memory. If you are using an array controller, you may need to subclass `[NSArrayController](https://developer.apple.com/documentation/appkit/nsarraycontroller)`, so that it will not pass the sort descriptors to the database but will instead do the sorting after your data has been fetched.

  

  
  ### Problems with Saving

  
  
  
  ### Cannot save documents because entity is null

  
  *Problem:* You have a Core Data document-based application that is unable to save. When you try to save the document you get an exception:

  
  
      
        

            - `Exception raised during posting of notification.  Ignored.  exception: Cannot perform operation since entity with name 'Wxyz' cannot be found`

        

      
  

  *Cause:* This error is emitted by an instance of `NSObjectController` (or one of its subclasses) that is set in Entity mode but can’t access the entity description in the managed object model associated with the entity name specified in Interface Builder. In short, you have a controller in entity mode with an invalid entity name. 

  *Remedy:* Select in turn each of your controllers in Interface Builder, and press Command-1 to show the inspector.  For each controller, ensure you have a valid entity name in the Entity name field at the top of the inspector.

  

  
  ### Exception generated in retainedDataForObjectID:withContext

  
  *Problem:* You add an object to a context. When you try to save the document you get an error that looks like this:

  
  
      
        

            [date] My App[2529:4b03] cannot find data for a temporary oid: 0x60797a0 <<x-coredata:///MyClass/t8BB18D3A-0495-4BBE-840F-AF0D92E549FA195>x-coredata:///MyClass/t8BB18D3A-0495-4BBE-840F-AF0D92E549FA195>

        

      
  

  This exception is in `-[NSSQLCore retainedDataForObjectID:withContext:]`, and the backtrace looks like this:

  
  
      
        

            - `#1    0x9599a6ac in -[NSSQLCore retainedDataForObjectID:withContext:]`

            - `#2    0x95990238 in -[NSPersistentStoreCoordinator(_NSInternalMethods) _conflictsWithRowCacheForObject:andStore:]`

            - `#3    0x95990548 in -[NSPersistentStoreCoordinator(_NSInternalMethods) _checkRequestForStore:originalRequest:andOptimisticLocking:]`

            - `#4    0x9594e8f0 in -[NSPersistentStoreCoordinator(_NSInternalMethods) executeRequest:withContext:]`

            - `#5    0x959617ec in -[NSManagedObjectContext save:]`

        

      
  

  The call to `_conflictsWithRowCacheForObject:` is comparing the object you're trying to save with its last cached version from the database. Basically, it's checking to see if any other code (thread, process, or just a different managed object context) changed this object without your knowledge.

  Core Data does not do this check on newly inserted objects because they could not have existed in any other scope.   They haven't been written to the database yet.

  *Cause:* You may have forced a newly inserted object to lose its inserted status and then changed or deleted it. This could happen if you passed a temporary object ID to `[objectWithID:](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506197-object)`. You may have passed an inserted object to another managed object context. 

  *Remedy:* There are a number of possible remedies, depending on the root cause:

  
  Do not pass an inserted (not yet saved) object to another context. Only objects that have been saved can be passed between contexts.

  Do not invoke `refreshObject:` on a newly-inserted object. 

  Do not make a relationship to an object that you never insert into the context.

  Ensure that you use the designated initializer for instances of `NSManagedObject`.

  Before you save (frame #5 in the stack trace), make sure that the context’s `[updatedObjects](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506985-updatedobjects)` and `[deletedObjects](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506699-deletedobjects)` sets should only have members whose object ID returns `NO``false` from `[isTemporaryID](https://developer.apple.com/documentation/coredata/nsmanagedobjectid/1391691-temporaryid)`. 

  

  
  ### Debugging Fetching

  
  Use the user default `com.apple.CoreData.SQLDebug` setting to log to `stderr` the actual SQL statement sent to SQLite. (Note that user default names are case sensitive.) For example, you can pass the following as an argument to the application:

  
  
      
        

            - `-com.apple.CoreData.SQLDebug 1`

        

      
  

  Higher levels of debug numbers produce more information, although using higher numbers is likely to be of diminishing utility.

  The information the output provides can be useful when debugging performance problems—in particular it may tell you when Core Data is performing a large number of small fetches (such as when firing faults individually). Like file I/O, executing many small fetches is expensive compared to executing a single large fetch. For examples of how to correct this situation, see [Preventing a Fault from Firing](Performance.html#//apple_ref/doc/uid/TP40001075-CH25-SW6).

  
  
> 
    Important
    
    Using com.apple.CoreData.SQLDebug for reverse engineering to facilitate direct access to the SQLite file is *not* supported. It is exclusively a debugging tool.
    
    
  As this is for debugging, the exact format of the logging is subject to change without notice. You should not, for example, pipe the output into an analysis tool with the expectation that it will work on all OS versions.

  

  

  
  ### Managed Object Models

  
  
  
  ### Your application generates the message "+entityForName: could not locate an NSManagedObjectModel"

  
  *Problem:* The error states clearly the issue—the entity description cannot find a managed object model from which to access the entity information.

  *Cause:* The model may not be included in your application resources. You may be trying to access the model before it has been loaded. The reference to the context may be `nil`. 

  *Remedy:* Be sure that the model is included in your application resources and that the corresponding project target option in Xcode is selected. 

  The class method you invoked requires an entity name and a managed object context, and it is through the context that the entity gets the model. Basically, the core data stack looks like:

      managed object context ---> persistent store coordinator ---> managed object model

  If the managed object model cannot be found, make sure of the following:

  
  The managed object context is not `nil`.

  If you are setting the reference to the context in a `.nib` file, make sure the appropriate outlet or binding is set correctly.

  If you are managing your own Core Data stack, make sure that the managed object context has an associated coordinator (`[setPersistentStoreCoordinator:](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506618-persistentstorecoordinator)`) after allocating.

  The persistent store coordinator has a valid model.

  

  
  ### Bindings Integration

  
  Many problems relating to bindings are not specific to Core Data and are discussed in [Troubleshooting Cocoa Bindings](../CocoaBindings/Concepts/Troubleshooting.html#//apple_ref/doc/uid/TP40002148). This section describes some additional problems that could be caused by the interaction of Core Data and bindings.

  
  
  ### Cannot access contents of an object controller after a nib is loaded

  
  *Problem:* You want to perform an operation with the contents of an object controller (an instance of `[NSObjectController](https://developer.apple.com/documentation/appkit/nsobjectcontroller)`, `[NSArrayController](https://developer.apple.com/documentation/appkit/nsarraycontroller)`, or `[NSTreeController](https://developer.apple.com/documentation/appkit/nstreecontroller)`) after a `.nib` file has been loaded, but the controller's content is `nil`. 

  *Cause:* The controller's fetch is executed as a delayed operation performed after its managed object context is set (by nib loading)—the fetch therefore happens after `[awakeFromNib](https://developer.apple.com/documentation/objectivec/nsobject/1402907-awakefromnib)` and `[windowControllerDidLoadNib:](https://developer.apple.com/documentation/appkit/nsdocument/1515221-windowcontrollerdidloadnib)`. 

  *Remedy:* Execute the fetch manually with `[fetchWithRequest:merge:error:](https://developer.apple.com/documentation/appkit/nsobjectcontroller/1531782-fetch)`. See [Using Core Data with Cocoa Bindings](CocoaBindings.html#//apple_ref/doc/uid/TP40001075-CH12-SW1).

  

  
  ### Cannot create new objects with array controller

  
  *Problem:* You cannot create new objects using an `NSArrayController`. For example, when you click the button assigned to the `add:` action, you get an error similar to the following: 

  
  
      
        

            - `2005-05-05 12:00:)).000 MyApp[1234] *** NSRunLoop`

            - `ignoring exception 'Failed to create new object' that raised`

            - `during posting of delayed perform with target 123456`

            - `and selector 'invokeWithTarget:'`

        

      
  

  *Cause:* In your managed object model, you may have specified a custom class for the entity, but you have not implemented the class.

  *Remedy:* Implement the custom class, or specify that the entity is represented by `NSManagedObject`.

  

  
  ### A table view bound to an array controller doesn't display the contents of a relationship

  
  *Problem:* You want to display the contents of a relationship in a table view bound to an array controller, but nothing is displayed and you get an error similar to the following:

  
  
      
        

            - `2005-05-27 14:13:39.077 MyApp[1234] *** NSRunLoop ignoring exception`

            - `'Cannot create NSArray from object <_NSFaultingMutableSet: 0x3818f0> ()`

            - `of class _NSFaultingMutableSet - consider using contentSet`

            - `binding instead of contentArray binding' that raised during posting of`

            - `delayed perform with target 385350 and selector 'invokeWithTarget:'`

        

      
  

  *Cause:* You bound the controller's `contentArray` binding to a relationship. Relationships are represented by sets.

  *Remedy:* Bind the controller's `contentSet` binding to the relationship. 

  

  
  ### A new object is not added to the relationship of the object currently selected in a table view

  
  *Problem:* You have a table view that displays a collection of instances of an entity. The entity has a relationship to another entity, instances of which are displayed in a second table view. Each table view is managed by an array controller. When you add new instances of the second entity, they are not added to the relationship of the currently selected instance of the first.

  *Cause:* The two array controllers are not related. There is nothing to tell the second array controller about the first.

  *Remedy:* Bind the second array controller's `contentSet` binding to the key path that specifies the relationship of the selection in the first array controller. For example, if the first array controller manages the Department entity, and the second array controller manages the Employee entity, then the `contentSet` binding of the second array controller should be `[Department Controller].selection.employees`.

  

  
  ### Table view or outline view contents not kept up-to-date when bound to an NSArrayController or NSTreeController object

  
  *Problem:* You have a table view or outline view that displays a collection of instances of an entity. As new instances of the entity are added and removed, the table view is not kept in sync.

  *Cause:* If the controller's content is an array that you manage yourself, then it is possible you are not modifying the array in a way that is KVO-compliant.

  If the controller's content is fetched automatically, then you have probably not set the controller to "Automatically prepare content."

  Alternatively, the controller may not be properly configured.

  *Remedy:* If the controller's content is a collection that you manage yourself, then ensure you modify the collection in a way that is KVO-compliant. See [Troubleshooting Cocoa Bindings](../CocoaBindings/Concepts/Troubleshooting.html#//apple_ref/doc/uid/TP40002148).

  If the controller's content is fetched automatically, set the "Automatically prepares content" switch for the controller in the Attributes inspector in Interface Builder (see also `[automaticallyPreparesContent](https://developer.apple.com/documentation/appkit/nsobjectcontroller/1534767-automaticallypreparescontent)`). Doing so means that the controller will track inserts into and deletions from its managed object context for its entity.

  Also check to see that the controller is properly configured (for example, that you have set the entity correctly).

  

  	
 	
    		[Performance](Performance.html#//apple_ref/doc/uid/TP40001075-CH25-SW1)

  			[Frequently Asked Questions](FrequentlyAskedQuestions.html#//apple_ref/doc/uid/TP40001075-CH29-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2017-03-27](RevisionHistory.html#//apple_ref/doc/uid/TP40003204-SW1)