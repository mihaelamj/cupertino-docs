---
title: "Creating and Saving Managed Objects"
book: "Core Data Programming Guide"
chapterId: "TP40001075-CH5"
date: "2017-03-27"
description: "Explains how to manage objects using the Core Data framework."
identifier: "//apple_ref/doc/uid/TP40001075"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Creating and Saving Managed Objects

> **Core Data Programming Guide**
> Last updated: 2017-03-27

## Creating and Saving Managed Objects

  
  	
  		
  After you have defined your managed object model and initialized the Core Data stack within your application, you are ready to start creating objects for data storage.

		 

  
  
  ### Creating Managed Objects

  
  An `[NSManagedObject](https://developer.apple.com/documentation/coredata/nsmanagedobject)` instance implements the basic behavior required of a Core Data model object. The `NSManagedObject` instance requires two elements: an entity description (an `[NSEntityDescription](https://developer.apple.com/documentation/coredata/nsentitydescription)` instance) and a managed object context (an `[NSManagedObjectContext](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext)` instance). The entity description includes the name of the entity that the object represents and its attributes and relationships. The managed object context represents a scratch pad where you create the managed objects. The context tracks changes to and relationships between objects.

  As shown in this example, the `NSEntityDescription` class has a class method that accepts a string for the name of the entity and a reference to the `NSManagedObjectContext` that the `NSManagedObject` instance will be associated with. The example defines the returning object as an `AAAEmployeeMO` object.

  
  
      
          Objective-C

        

            AAAEmployeeMO *employee = [NSEntityDescription insertNewObjectForEntityForName:@"Employee" inManagedObjectContext:[self managedObjectContext];

        

      
      
          Swift

        

            - `let employee = NSEntityDescription.insertNewObjectForEntityForName("Employee", inManagedObjectContext: managedObjectContext) as! EmployeeMO`

        

      
  

  

  
  ### Creating NSManagedObject Subclasses

  
  By default, Core Data returns `NSManagedObject` instances to your application. However, it is useful to define subclasses of `NSManagedObject` for each of the entities in your model. Speciflcally, when you create subclasses of `NSManagedObject`, you can define the properties that the entity can use for code completion, and you can add convenience methods to those subclasses.

  To create a subclass of `NSManagedObject`, in the Xcode Core Data model editor, select the entity, and in the Entity pane of the Data Model inspector, enter the name in the Class field. Then create the subclass in Xcode.

  
  
      
          Objective-C

        

            #import <CoreData/CoreData.h>

             

            @interface AAAEmployeeMO : NSManagedObject

             

            @property (nonatomic, strong) NSString *name;

             

            @end

             

            @implementation AAAEmployeeMO

             

            @dynamic name;

             

            @end

        

      
      
          Swift

        

            - `import UIKit`

            - `import CoreData`

            - `import Foundation`

            - ` `

            - `class EmployeeMO: NSManagedObject {`

            - `    `

            - `    @NSManaged var name: String?`

            - `    `

            - `}`

        

      
  

  The `@dynamic` tag informs the compiler that the variable will be resolved at runtime.

  After the subclass has been defined in your data model and added to your project, you can reference it directly in your application and improve the readability of your application code.

  

  
  ### Saving NSManagedObject Instances

  
  The creation of `NSManagedObject` instances does not guarantee their persistence. After you create an `NSManagedObject` instance in your managed object context, explicitly save that context to persist those changes to your persistent store.

  
  
      
          Objective-C

        

            NSError *error = nil;

            if ([[self managedObjectContext] save:&error] == NO) {

                NSAssert(NO, @"Error saving context: %@\n%@", [error localizedDescription], [error userInfo]);

            }

        

      
      
          Swift

        

            - `do {`

            - `    try managedObjectContext.save()`

            - `} catch {`

            - `    fatalError("Failure to save context: \(error)")`

            - `}`

        

      
  

  The call to save on the `NSManagedObjectContext` accepts a reference to an `[NSError](https://developer.apple.com/documentation/foundation/nserror)` variable and always returns either a success or a fail. If the save fails, it is important to display the error condition so that it can be corrected. The display of that error condition can be as simple as outputting the error to the console or as complicated as offering the error message to the user. If the save method returns a success, the error variable does not need to be consulted.

  

  	
 	
    		[Initializing the Core Data Stack](InitializingtheCoreDataStack.html#//apple_ref/doc/uid/TP40001075-CH4-SW1)

  			[Fetching Objects](FetchingObjects.html#//apple_ref/doc/uid/TP40001075-CH6-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2017-03-27](RevisionHistory.html#//apple_ref/doc/uid/TP40003204-SW1)