---
title: "Faulting and Uniquing"
book: "Core Data Programming Guide"
chapterId: "TP40001075-CH18"
date: "2017-03-27"
description: "Explains how to manage objects using the Core Data framework."
identifier: "//apple_ref/doc/uid/TP40001075"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Faulting and Uniquing

> **Core Data Programming Guide**
> Last updated: 2017-03-27

## Faulting and Uniquing

  
  	
  		
  Faulting reduces your application’s memory usage by keeping placeholder objects (faults) in the persistent store. A related feature called uniquing ensures that, in a given managed object context, you never have more than one managed object to represent a given record.

		 

  
  
  ### Faulting Limits the Size of the Object Graph

  
  Managed objects typically represent data held in a persistent store. In some situations a managed object may be a fault—an object whose property values have not yet been loaded from the external data store. *Faulting* reduces the amount of memory your application consumes. A fault is a placeholder object that represents a managed object that has not yet been fully realized or a collection object that represents a relationship:

  
  A managed object fault is an instance of the appropriate class, but its persistent variables are not yet initialized. 

  A relationship fault is a subclass of the collection class that represents the relationship.

  Faulting allows Core Data to put boundaries on the object graph. Because a fault is not realized, a managed object fault consumes less memory, and managed objects related to a fault are not required to be represented in memory at all. 

  To illustrate, consider an application that allows a user to fetch and edit details about a single employee. The employee has a relationship to a manager and to a department, and these objects in turn have other relationships. If you retrieve just a single Employee object from a persistent store, its manager, department, and reports relationships are initially represented by faults. Figure 13-1 shows an employee’s department relationship represented by a fault. 

  
  **Figure 13-1**A department represented by a fault
  

  Although the fault is an instance of the Department class, it has not yet been realized—none of its persistent instance variables have yet been set. This means that not only does the department object consume less memory itself, but there’s no need to populate its employees relationship. If it were a requirement that the object graph be complete, then to edit a single attribute of a single employee, it would ultimately be necessary to create objects to represent the whole corporate structure.

  

  
  ### Firing Faults

  
  Fault handling is transparent—you do not have to execute a fetch to realize a fault. If at some stage a persistent property of a fault object is accessed, Core Data automatically retrieves the data for the object and initializes the object. This process is commonly referred to as firing the fault. If you access a property on the Department object — its name, for example — the fault fires and Core Data executes a fetch for you to retrieve all of the object's attributes. (See `[NSManagedObject](https://developer.apple.com/documentation/coredata/nsmanagedobject)` for a list of methods that do not cause faults to fire.)

  Core Data automatically fires faults when a persistent property (such as `firstName`) of a fault is accessed. However, firing faults individually can be inefficient, and there are better strategies for getting data from the persistent store (see [Decreasing Fault Overhead](Performance.html#//apple_ref/doc/uid/TP40001075-CH25-SW7)). To deal efficiently with faults and relationships, see [Fetching Managed Objects](Performance.html#//apple_ref/doc/uid/TP40001075-CH25-SW3) and [Preventing a Fault from Firing](Performance.html#//apple_ref/doc/uid/TP40001075-CH25-SW6).

  When a fault is fired, Core Data does not go back to the store *if* the data is available in its cache. With a cache hit, converting a fault into a realized managed object is very fast—it is basically the same as normal instantiation of a managed object. If the data is not available in the cache, Core Data automatically executes a fetch for the fault object; this results in a round trip to the persistent store to fetch the data, and again the data is cached in memory.

  Whether or not an object is a fault simply means whether or not a given managed object has all its persistent attributes populated and is ready to use. If you need to determine whether an object is a fault, call its `isFault` method without firing the fault (without accessing any relationships or attributes). If `isFault` returns `NO``false`, then the data must be in memory and therefore the object is not a fault. However, if `isFault` returns `YES``true`, it does *not* imply that the data is *not* in memory. The data may be in memory, or it may not, depending on many factors influencing caching.

  Although the standard `[description](../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/clm/NSObject/description)` method does not cause a fault to fire, if you implement a custom `description` method that accesses the object’s persistent properties, the fault will fire. You are strongly discouraged from overriding `description` in this way.

  There is no way to load individual attributes of a managed object on an as-needed basis and avoid realizing (retrieving all property values of) the entire object. For patterns to deal with large attributes, see [Binary Large Data Objects (BLOBs)](Performance.html#//apple_ref/doc/uid/TP40001075-CH25-SW11). 

  

  
  ### Turning Objects into Faults

  
  Turning a realized object into a fault can be useful in pruning the object graph, as well as ensuring that property values are current.  Turning a managed object into a fault releases unnecessary memory and sets its in-memory property values to `nil`. (See [Reducing Memory Overhead](Performance.html#//apple_ref/doc/uid/TP40001075-CH25-SW10) and Ensuring Data Is Up to Date.)

  You can turn a realized object into a fault with the `[refreshObject:mergeChanges:](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506224-refreshobject)` method. If you pass `NO``false` as the `mergeChanges` argument, you must be sure that there are no changes to that object’s relationships. If there are, and you then save the context, you will introduce referential integrity problems to the persistent store.

  When an object turns into a fault, its `[didTurnIntoFault](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506470-didturnintofault)` method is called. You may implement a custom `didTurnIntoFault` method to perform various housekeeping functions; for example, see Ensuring Data Is Up to Date.

  
  
> 
    Note
    
    	Core Data avoids the term *unfaulting* because it is confusing. There’s no “unfaulting” a virtual memory page fault. Page faults are triggered, caused, fired, or encountered. Of course, you can release memory back to the kernel in a variety of ways (using the function `vm_deallocate`, `munmap`, or `sbrk`). Core Data describes this as "turning an object into a fault."
    	
    
  

  

  
  ### Faults and KVO Notifications

  
  When Core Data turns an object into a fault, key-value observing (KVO) change notifications are sent to the object’s properties. If you are observing properties of an object that is turned into a fault and the fault is subsequently realized, you receive change notifications for properties whose values have not in fact changed.  See *[Key-Value Observing Programming Guide](../KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i)*.

  Although the values are not changing semantically from your perspective, the literal bytes in memory are changing as the object is materialized. The key-value observing mechanism requires Core Data to issue the notification whenever the values change according to the perspective of pointer comparison. KVO needs these notifications to track changes across key paths and dependent objects.

  

  
  ### Uniquing Ensures a Single Managed Object per Record per Context

  
  Core Data ensures that—in a given managed object context—an entry in a persistent store is associated with only one managed object. The technique is known as *uniquing*. Without uniquing, you might end up with a context maintaining more than one object to represent a given record.

  For example, consider the situation illustrated in Figure 13-2; two employees have been fetched into *a single managed object context*. Each has a relationship to a department, but the department is currently represented by a fault. 

  
  **Figure 13-2**Independent faults for a department object
  

  It would appear that each employee has a separate department, and if you called `department` on each employee—turning the Department faults into regular objects—you would have two separate Department objects in memory. However, if both employees belong to the same department (for example, Marketing), Core Data ensures that (in a given managed object context) only one object representing the Marketing department is ever created. If both employees belong to the same department, their department relationships would both therefore reference the same fault, as illustrated in Figure 13-3.

  
  **Figure 13-3**Uniqued fault for two employees working in the same department
  

  Without uniquing, if you fetched all the employees and called `department` on each—thereby firing the corresponding faults—a new Department object would be created every time. This would result in a number of objects, each representing the same department, that could contain different and conflicting data. When the context is saved, it would be impossible to determine the correct data to commit to the store.

  More generally, all the managed objects in a given context that refer to the Marketing Department object refer to the same instance—they have a single view of Marketing’s data—*even if the Marketing Department object is a fault*. 

  
  
> 
    Note
    
    	This discussion focuses on a single managed object context. Each managed object context represents a different view of the data. If the same employees are fetched into a second context, then they—and the corresponding Department object—are all represented by different objects in memory. The objects in different contexts *may* have different and conflicting data. It is precisely the role of the Core Data architecture to detect and resolve these conflicts at save time.
    	
    
  

  

  	
 	
    		[Creating Managed Object Relationships](HowManagedObjectsarerelated.html#//apple_ref/doc/uid/TP40001075-CH17-SW1)

  			[Object Validation](ObjectValidation.html#//apple_ref/doc/uid/TP40001075-CH20-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2017-03-27](RevisionHistory.html#//apple_ref/doc/uid/TP40003204-SW1)