---
title: "Creating and Modifying Custom Managed Objects"
book: "Core Data Programming Guide"
chapterId: "TP40001075-CH16"
date: "2017-03-27"
description: "Explains how to manage objects using the Core Data framework."
identifier: "//apple_ref/doc/uid/TP40001075"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Creating and Modifying Custom Managed Objects

> **Core Data Programming Guide**
> Last updated: 2017-03-27

## Creating and Modifying Custom Managed Objects

  
  	
  		
  As discussed previously, managed objects are instances of the `[NSManagedObject](https://developer.apple.com/documentation/coredata/nsmanagedobject)` class, or of a subclass of `NSManagedObject`, that represent instances of an entity. `NSManagedObject` is a generic class that implements all the basic behavior required of a managed object. You can create custom subclasses of `NSManagedObject`, although this is often not required. If you do not need any custom logic for a given entity, you do not need to create a custom class for that entity. You implement a custom class to, for example, provide custom accessor or validation methods, use nonstandard attributes, specify dependent keys, calculate derived values, and implement any other custom logic.

		 

  
  
  ### Creating Custom Managed Object Subclasses

  
  In an Objective-C managed object subclass, you can declare the properties for modeled attributes in the interface file, but you don’t declare instance variables:

  
  
      
        

            @interface MyManagedObject : NSManagedObject

             

            @property (nonatomic, strong) NSString *title;

            @property (nonatomic, strong) NSDate *date;

             

            @end

        

      
  

  Notice that the properties are declared as `nonatomic` and `strong`. For performance reasons, Core Data typically does not copy object values, even if the value class adopts the `[NSCopying](../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCopying/Description.html#//apple_ref/occ/intf/NSCopying)` [protocol](../../../General/Conceptual/DevPedia-CocoaCore/Protocol.html#//apple_ref/doc/uid/TP40008195-CH45).

  In the Objective-C implementation file, you specify the properties as `dynamic`:

  
  
      
        

            @implementation MyManagedObject

            @dynamic title;

            @dynamic date;

            @end

        

      
  

  In Swift, you declare the properties using the `@NSManaged` keyword:

  
  
      
        

            - `class MyManagedObject: NSManagedObject {`

            - `    @NSManaged var title: String?`

            - `    @NSManaged var date: NSDate?`

            - `}`

        

      
  

  Core Data dynamically generates efficient public and primitive get and set attribute accessor methods and relationship accessor methods for properties that are defined in the entity of a managed object’s corresponding managed object model. Therefore, you typically don’t need to write custom accessor methods for modeled properties.

  

  
  ### Guidelines for Overriding Methods

  
  `NSManagedObject` itself customizes many features of `NSObject` so that managed objects can be properly integrated into the Core Data infrastructure. Core Data relies on `NSManagedObject`’s implementation of the following methods, which you should therefore not override: 

  
  `primitiveValueForKey:`

  `setPrimitiveValue:forKey:`

  `isEqual:`

  `hash`

  `superclass`

  `class`

  `self`

  `zone`

  `isProxy`

  `isKindOfClass:`

  `isMemberOfClass:`

  `conformsToProtocol:`

  `respondsToSelector:`

  `managedObjectContext`

  `entity`

  `objectID`

  `isInserted`

  `isUpdated`

  `isDeleted`

  `isFault`

  You are discouraged from overriding `[initWithEntity:insertIntoManagedObjectContext:](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506357-init)` and `description`. If `description` fires a fault during a debugging operation, the results may be unpredictable. You should typically not override the key-value coding methods such as `[valueForKey:](../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/EOF/EOControl/Classes/NSObjectAdditions/Description.html#//apple_ref/occ/instm/NSObject/valueForKey:)` and `[setValue:forKeyPath:](https://developer.apple.com/documentation/objectivec/nsobject/1418139-setvalue)`.

  In addition, before overriding `[awakeFromInsert](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506548-awakefrominsert)`, `[awakeFromFetch](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506424-awakefromfetch)`, and validation methods such as `[validateForUpdate:](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506998-validateforupdate)`, invoke their superclass implementation. Be careful when overriding accessor methods because you could negatively impact performance.

  

  
  ### Defining Properties and Data Storage

  
  In some respects, a managed object acts like a dictionary—it is a generic container object that efficiently provides storage for the properties defined by its associated `NSEntityDescription` object. `NSManagedObject` supports a range of common types for attribute values, including string, date, and number (see `[NSAttributeDescription](https://developer.apple.com/documentation/coredata/nsattributedescription)` for full details). Therefore, you typically do not need to define instance variables in subclasses. However, if you need to implement nonstandard attributes or preserve time zones, you may need to do so. In addition, there are some performance considerations that can be mitigated in a subclass if you use large binary data objects—see [Binary Large Data Objects (BLOBs)](Performance.html#//apple_ref/doc/uid/TP40001075-CH25-SW11).

  
  
  ### Using Nonstandard Attributes

  
  By default, `NSManagedObject` stores its properties as objects in an internal structure, and in general Core Data is more efficient working with storage under its own control than with using custom instance variables.

  Sometimes you need to use types that are not supported directly, such as colors and C structures. For example, in a graphics application you might want to define a Rectangle entity that has attributes `color` and `bounds`, which are instances of `NSColor` and `NSRect` structures respectively. This situation requires you to create a subclass of `NSManagedObject`.

  

  
  ### Dates, Times, and Preserving Time Zones

  
  `NSManagedObject` represents date attributes with `[NSDate](../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateClassCluster/Description.html#//apple_ref/occ/cl/NSDate)` objects, and stores times internally as an `[NSTimeInterval](../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/tdef/NSTimeInterval)` value that is based on GMT. Time zones are not explicitly stored—always represent a Core Data date attribute in GMT, so that searches are normalized in the database. If you need to preserve the time zone information, store a time zone attribute in your model, which may require you to create a subclass of `NSManagedObject`. 

  

  
  ### Customizing Initialization and Deallocation

  
  Core Data controls the life cycle of managed objects. With faulting and undo, you cannot make the same assumptions about the life cycle of a managed object that you do with a standard Objective-C object—managed objects can be instantiated, destroyed, and resurrected by the framework as it requires. 

  When a managed object is created, it is initialized with the default values given for its entity in the managed object model. In many cases the default values set in the model are sufficient. Sometimes, however, you may wish to perform additional initialization—perhaps using dynamic values (such as the current date and time) that cannot be represented in the model.

  In a typical Objective-C class, you usually override the designated initializer (often the `init` method). In a subclass of `NSManagedObject`, there are three different ways you can customize initialization—by overriding `[initWithEntity:insertIntoManagedObjectContext:](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506357-init)`, `awakeFromInsert`, or `awakeFromFetch`. Do not override `init`. It is also recommended that you do not override `initWithEntity:insertIntoManagedObjectContext:`, as state changes made in this method may not be properly integrated with undo and redo. The two other methods, `awakeFromInsert` and `awakeFromFetch`, allow you to differentiate between two different situations:

  
  `[awakeFromInsert](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506548-awakefrominsert)` is invoked only once in the lifetime of an object—when it is first created.

  `awakeFromInsert` is invoked immediately after you invoke `[initWithEntity:insertIntoManagedObjectContext:](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506357-init)` or `[insertNewObjectForEntityForName:inManagedObjectContext:](https://developer.apple.com/documentation/coredata/nsentitydescription/1425093-insertnewobject)`. You can use `awakeFromInsert` to initialize special default property values, such as the creation date of an object, as illustrated in the following example.

  
  
      
          Objective-C

        

            - (void)awakeFromInsert

            {

                [super awakeFromInsert];

                [self setCreationDate:[NSDate date]];

            }

        

      
      
          Swift

        

            - `override func awakeFromInsert() {`

            - `    super.awakeFromInsert()`

            - `    creationDate = NSDate()`

            - `}`

        

      
  

  `[awakeFromFetch](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506424-awakefromfetch)` is invoked when an object is reinitialized from a persistent store (during a fetch).

  You can override `awakeFromFetch` to, for example, establish transient values and other caches. Change processing is explicitly disabled in `awakeFromFetch` so that you can conveniently use public set accessor methods without dirtying the object or its context. This disabling of change processing does mean, however, that you should not manipulate relationships because changes will not be properly propagated to the destination object or objects. Instead of overriding `awakeFromFetch`, you can override `awakeFromInsert` or employ any of the run loop-related methods such as `[performSelector:withObject:afterDelay:](../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/instm/NSObject/performSelector:withObject:afterDelay:)`. 

  Avoid overriding `[dealloc](../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/instm/NSObject/dealloc)` to clear transient properties and other variables. Instead, override `didTurnIntoFault`. `didTurnIntoFault` is invoked automatically by Core Data when an object is turned into a fault and immediately prior to actual deallocation. You might turn a managed object into a fault specifically to reduce memory overhead (see [Reducing Memory Overhead](Performance.html#//apple_ref/doc/uid/TP40001075-CH25-SW10)), so it is important to ensure that you properly perform cleanup operations in `didTurnIntoFault`. 

  

  
  ### Xcode Generated Subclasses

  
  Starting with Xcode 8, iOS 10, and macOS 10.12, Xcode can automatically generate `NSManagedObject` subclasses or extensions/categories from the Core Data Model.

  To enable this feature in an existing project, first ensure that the data model is configured correctly:

  
  Select the Core Data Model file, and open the File inspector.

  Confirm that the Tools Version is set to Xcode 8.0 or later.

  Confirm that the Code Generation is set to the language you are currently using.

  After the data model is configured, you can then configure each entity:

  
  Select the entity you want to configure.

  Open the Data Model inspector.

  
  
  

  Set the code generator to either None, Class Definition, or Category/Extension.

  After the data model is configured, Xcode regenerates the subclasses or categories/extensions whenever the related entity has changed in the data model.

  
  
> 
    Note
    
    	The generated source code is not included in your project and is intended to be a part of the build process. You will not see the files in your project’s source list but the files can be reviewed in the build directory. These files can be regenerated often so there is no value in editing them manually.
    	
    
  

  If you wish to add additional convenience methods or business logic to your NSManagedObject subclasses, you can create a category (in Objective-C) or an extension (in Swift) and place the additional logic there.

  

  	
 	
    		[Fetching Objects](FetchingObjects.html#//apple_ref/doc/uid/TP40001075-CH6-SW1)

  			[Connecting the Model to Views](nsfetchedresultscontroller.html#//apple_ref/doc/uid/TP40001075-CH8-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2017-03-27](RevisionHistory.html#//apple_ref/doc/uid/TP40003204-SW1)