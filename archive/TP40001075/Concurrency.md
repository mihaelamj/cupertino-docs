---
title: "Concurrency"
book: "Core Data Programming Guide"
chapterId: "TP40001075-CH24"
date: "2017-03-27"
description: "Explains how to manage objects using the Core Data framework."
identifier: "//apple_ref/doc/uid/TP40001075"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Concurrency

> **Core Data Programming Guide**
> Last updated: 2017-03-27

## Concurrency

  
  	
  		
  Concurrency is the ability to work with the data on more than one queue at the same time. If you choose to use concurrency with Core Data, you also need to consider the application environment. For the most part, AppKit and UIKit are not thread-safe. In macOS in particular, Cocoa bindings and controllers are not threadsafe—if you are using these technologies, multithreading may be complex.

		 

  
  
  ### Core Data, Multithreading, and the Main Thread

  
  In Core Data, the managed object context can be used with two concurrency patterns, defined by `[NSMainQueueConcurrencyType](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontextconcurrencytype/nsmainqueueconcurrencytype)` and `[NSPrivateQueueConcurrencyType](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontextconcurrencytype/nsprivatequeueconcurrencytype)`. 

  `[NSMainQueueConcurrencyType](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontextconcurrencytype/nsmainqueueconcurrencytype)` is specifically for use with your application interface and can only be used on the main queue of an application.

  The `[NSPrivateQueueConcurrencyType](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontextconcurrencytype/nsprivatequeueconcurrencytype)` configuration creates its own queue upon initialization and can be used only on that queue. Because the queue is private and internal to the `[NSManagedObjectContext](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext)` instance, it can only be accessed through the `[performBlock:](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506578-performblock)` and the `[performBlockAndWait:](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506364-performblockandwait)` methods.

  In both cases, the initialization of the `NSManagedObjectContext` instance is the same:

  
  
      
          Objective-C

        

            NSManagedObjectContext *moc = [[NSManagedObjectContext alloc] initWithConcurrencyType:<#type#>];

        

      
      
          Swift

        

            - `let moc = NSManagedObjectContext(concurrencyType:<#type#>)`

        

      
  

  The parameter being passed in as part of the initialization determines what type of `NSManagedObjectContext` is returned.

  When you are using an `[NSPersistentContainer](https://developer.apple.com/documentation/coredata/nspersistentcontainer)`, the viewContext property is configured as a `[NSMainQueueConcurrencyType](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontextconcurrencytype/nsmainqueueconcurrencytype)` context and the contexts associated with `[performBackgroundTask:](https://developer.apple.com/documentation/coredata/nspersistentcontainer/1640564-performbackgroundtask)` and `[newBackgroundContext](https://developer.apple.com/documentation/coredata/nspersistentcontainer/1640581-newbackgroundcontext)` are configured as `[NSPrivateQueueConcurrencyType](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontextconcurrencytype/nsprivatequeueconcurrencytype)`.

  

  
  ### Using a Private Queue to Support Concurrency

  
  In general, avoid doing data processing on the main queue that is not user-related. Data processing can be CPU-intensive, and if it is performed on the main queue, it can result in unresponsiveness in the user interface. If your application will be processing data, such as importing data into Core Data from JSON, create a private queue context and perform the import on the private context. The following example shows how to do this:

  
  
      
          Objective-C

        

            NSArray *jsonArray = …; //JSON data to be imported into Core Data

            NSManagedObjectContext *moc = …; //Our primary context on the main queue

             

            NSManagedObjectContext *private = [[NSManagedObjectContext alloc] initWithConcurrencyType:NSPrivateQueueConcurrencyType];

            [private setParentContext:moc];

             

            [private performBlock:^{

                for (NSDictionary *jsonObject in jsonArray) {

                    NSManagedObject *mo = …; //Managed object that matches the incoming JSON structure

                    //update MO with data from the dictionary

                }

                NSError *error = nil;

                if (![private save:&error]) {

                    NSLog(@"Error saving context: %@\n%@", [error localizedDescription], [error userInfo]);

                    abort();

                }

                [moc performBlockAndWait:^{

                    NSError *error = nil;

                    if (![moc save:&error]) {

                        NSLog(@"Error saving context: %@\n%@", [error localizedDescription], [error userInfo]);

                        abort();

                    }

                }];

            }];

        

      
      
          Swift

        

            - `let jsonArray = … //JSON data to be imported into Core Data`

            - `let moc = … //Our primary context on the main queue`

            - ` `

            - `let privateMOC = NSManagedObjectContext(concurrencyType: .PrivateQueueConcurrencyType)`

            - `privateMOC.parentContext = moc`

            - ` `

            - `privateMOC.performBlock {`

            - `    for jsonObject in jsonArray {`

            - `        let mo = … //Managed object that matches the incoming JSON structure`

            - `        //update MO with data from the dictionary`

            - `    }`

            - `    do {`

            - `        try privateMOC.save()`

            - `        moc.performBlockAndWait {`

            - `            do {`

            - `                try moc.save()`

            - `            } catch {`

            - `                fatalError("Failure to save context: \(error)")`

            - `            }`

            - `        }`

            - `    } catch {`

            - `        fatalError("Failure to save context: \(error)")`

            - `    }`

            - `}`

        

      
  

  In this example an array of data has been originally received as a JSON payload. You then create a new `[NSManagedObjectContext](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext)` that is defined as a private queue. The new context is set as a child of the main queue context that runs the application. From there you call `[performBlock:](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506578-performblock)` and do the actual `[NSManagedObject](https://developer.apple.com/documentation/coredata/nsmanagedobject)` creation inside of the block that is passed to `[performBlock:](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506578-performblock)`. After all of the data has been consumed and turned into `[NSManagedObject](https://developer.apple.com/documentation/coredata/nsmanagedobject)` instances, you call save on the private context, which moves all of the changes into the main queue context without blocking the main queue.

  This example can be further simplified when using an `[NSPersistentContainer](https://developer.apple.com/documentation/coredata/nspersistentcontainer)`:

  
  
      
          Objective-C

        

            NSArray *jsonArray = …;

            NSPersistentContainer *container = self.persistentContainer;

            [container performBackgroundTask:^(NSManagedObjectContext *context) {

                for (NSDictionary *jsonObject in jsonArray) {

                    AAAEmployeeMO *mo = [[AAAEmployeeMO alloc] initWithContext:context];

                    [mo populateFromJSON:jsonObject];

                }

                NSError *error = nil;

                if (![context save:&error]) {

                    NSLog(@"Failure to save context: %@\n%@", [error localizedDescription], [error userInfo]);

                    abort();

                }

            }];

        

      
      
          Swift

        

            - `let jsonArray = …`

            - `let container = self.persistentContainer`

            - `container.performBackgroundTask() { (context) in`

            - `    for jsonObject in jsonArray {`

            - `        let mo = EmployeeMO(context: context)`

            - `        mo.populateFromJSON(jsonObject)`

            - `    }`

            - `    do {`

            - `        try context.save()`

            - `    } catch {`

            - `        fatalError("Failure to save context: \(error)")`

            - `    }`

            - `}`

        

      
  

  

  
  ### Passing References Between Queues

  
  `[NSManagedObject](https://developer.apple.com/documentation/coredata/nsmanagedobject)` instances are not intended to be passed between queues. Doing so can result in corruption of the data and termination of the application. When it is necessary to hand off a managed object reference from one queue to another, it must be done through `[NSManagedObjectID](https://developer.apple.com/documentation/coredata/nsmanagedobjectid)` instances.

  You retrieve the managed object ID of a managed object by calling the `[objectID](https://developer.apple.com/documentation/coredata/nsmanagedobject/1506848-objectid)` method on the `[NSManagedObject](https://developer.apple.com/documentation/coredata/nsmanagedobject)` instance. 

  

  	
 	
    		[Persistent Store Types and Behaviors](PersistentStoreFeatures.html#//apple_ref/doc/uid/TP40001075-CH23-SW1)

  			[Performance](Performance.html#//apple_ref/doc/uid/TP40001075-CH25-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2017-03-27](RevisionHistory.html#//apple_ref/doc/uid/TP40003204-SW1)