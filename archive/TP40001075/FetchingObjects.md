---
title: "Fetching Objects"
book: "Core Data Programming Guide"
chapterId: "TP40001075-CH6"
date: "2017-03-27"
description: "Explains how to manage objects using the Core Data framework."
identifier: "//apple_ref/doc/uid/TP40001075"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Fetching Objects

> **Core Data Programming Guide**
> Last updated: 2017-03-27

## Fetching Objects

  
  	
  		
  Now that data is stored in the Core Data persistent store, you will use an `[NSFetchRequest](https://developer.apple.com/documentation/coredata/nsfetchrequest)` to access that existing data. The fetching of objects from Core Data is one of the most powerful features of this framework.

		 

  
  
  ### Fetching NSManagedObject Instances

  
  In this example you start by constructing an `NSFetchRequest` that describes the data you want returned. This example does not add any requirements to that data other than the type of entity being returned. You then call `[executeFetchRequest:error:](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506672-fetch)` on the `[NSManagedObjectContext](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext)` and pass in the request along with a pointer to an error.

  
  
      
          Objective-C

        

            NSManagedObjectContext *moc = …;

            NSFetchRequest *request = [NSFetchRequest fetchRequestWithEntityName:@"Employee"];

             

            NSError *error = nil;

            NSArray *results = [moc executeFetchRequest:request error:&error];

            if (!results) {

                NSLog(@"Error fetching Employee objects: %@\n%@", [error localizedDescription], [error userInfo]);

                abort();

            }

        

      
      
          Swift

        

            - `let moc = …`

            - `let employeesFetch = NSFetchRequest(entityName: "Employee")`

            - ` `

            - `do {`

            - `    let fetchedEmployees = try moc.executeFetchRequest(employeesFetch) as! [EmployeeMO]`

            - `} catch {`

            - `    fatalError("Failed to fetch employees: \(error)")`

            - `}`

        

      
  

  The `[executeFetchRequest:error:](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext/1506672-fetch)` method has two possible results. It either returns an `[NSArray](../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSArray)` object with zero or more objects, or it returns `nil`. If `nil` is returned, you have received an error from Core Data and need to respond to it. If the array exists, you receive possible results for the request even though the `NSArray` may be empty. An empty `NSArray` indicates that there were no records found.

  

  
  ### Filtering Results

  
  The real flexibility in fetching objects comes in the complexity of the fetch request. To begin with, you can add an `[NSPredicate](https://developer.apple.com/documentation/foundation/nspredicate)` object to the fetch request to narrow the number of objects being returned. For example, if you only want Employee objects that have a firstName of Trevor, you add the predicate directly to `NSFetchRequest`:

  
  
      
          Objective-C

        

            NSString *firstName = @"Trevor";

            [fetchRequest setPredicate:[NSPredicate predicateWithFormat:@"firstName == %@", firstName]];

        

      
      
          Swift

        

            - `let firstName = "Trevor"`

            - `fetchRequest.predicate = NSPredicate(format: "firstName == %@", firstName)`

        

      
  

  In addition to narrowing the objects being returned, you can configure how those objects are returned. For example, you can instruct Core Data to return `[NSDictionary](../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSDictionary)` instances instead of fully formed `[NSManagedObject](https://developer.apple.com/documentation/coredata/nsmanagedobject)` instances. Further, you can configure the `NSFetchRequest` so that those `NSDictionary` instances only contain a subset of the properties available on the Employee entity.

  For more information about `[NSFetchRequest](https://developer.apple.com/documentation/coredata/nsfetchrequest)`, see the class documentation.

  

  	
 	
    		[Creating and Saving Managed Objects](CreatingObjects.html#//apple_ref/doc/uid/TP40001075-CH5-SW1)

  			[Creating and Modifying Custom Managed Objects](LifeofaManagedObject.html#//apple_ref/doc/uid/TP40001075-CH16-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2017-03-27](RevisionHistory.html#//apple_ref/doc/uid/TP40003204-SW1)