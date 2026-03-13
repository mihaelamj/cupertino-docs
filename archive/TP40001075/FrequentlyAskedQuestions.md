---
title: "Frequently Asked Questions"
book: "Core Data Programming Guide"
chapterId: "TP40001075-CH29"
date: "2017-03-27"
description: "Explains how to manage objects using the Core Data framework."
identifier: "//apple_ref/doc/uid/TP40001075"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Frequently Asked Questions

> **Core Data Programming Guide**
> Last updated: 2017-03-27

## Frequently Asked Questions

  

  
  
  ### Where is a managed object context created?

  
  Where a managed object context comes from is entirely application-dependent. In a Cocoa document-based application using `NSPersistentDocument`, the persistent document typically creates the context and gives you access to it through the `[managedObjectContext](https://developer.apple.com/documentation/appkit/nspersistentdocument/1396162-managedobjectcontext)` method.

  In a single-window application, if you create your project using the standard project assistant, the application delegate (the instance of the `[NSApplicationDelegate](https://developer.apple.com/documentation/appkit/nsapplicationdelegate)` class) again creates the context and gives you access to it through the `managedObjectContext` method. In this case, however, the code to create the context (and the rest of the Core Data stack) is explicit. It is written for you automatically as part of the template.

  Do not use instances of subclasses of `[NSController](https://developer.apple.com/documentation/appkit/nscontroller)` directly to execute fetches. For example, do not create an instance of `[NSArrayController](https://developer.apple.com/documentation/appkit/nsarraycontroller)` specifically to execute a fetch. Controllers are for managing the interaction between your model objects and your application interface.  At the model object level, use a managed object context to perform the fetches directly.

  

  
  ### How do I initialize a store with default data?

  
  There are two considerations here: creating the data and ensuring the data is imported only once.

  There are several ways to create the data. 

  
  Create a separate persistent store that contains the default data and include the store as an application resource. When you want to use it, either copy the whole store to a suitable location, or copy the objects from the defaults store to an existing store.

  For small datasets, create the managed objects directly in code.

  Create a property list or another file-based representation of the data, and store it as an application resource. When you want to use it, open the file and parse the representation to create managed objects.

  
  
> 
    Note
    
    	Do not use this technique in iOS, and only if absolutely necessary in OS X. Parsing a file to create a store incurs unnecessary overhead. It is much better to create a Core Data store yourself offline and use it directly in your application.
    	
    
  

  There are also several ways to ensure that the defaults are imported only once:

  
  If you are using iOS or creating an application for OS X that is not document-based, you can add a check on application launch to determine whether a file exists at the location you specify for the application’s store.  If it doesn't, you need to import the data.

  If you are creating a document-based application using `[NSPersistentDocument](https://developer.apple.com/documentation/appkit/nspersistentdocument)`, you initialize the defaults in `[initWithType:error:](https://developer.apple.com/documentation/appkit/nsdocument/1515159-initwithtype)`.

  If there is a possibility that the store (hence file) might be created but the data not imported, you can add a metadata flag to the store. You can check the metadata (using `[metadataForPersistentStoreWithURL:error:](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator/1468915-metadataforpersistentstorewithur)`) more efficiently than executing a fetch (and it does not require you to hard code any default data values). 

  

  
  ### How do I use my existing SQLite database with Core Data?

  
  You do not, unless you import your existing SQLite database into a Core Data store. Although Core Data supports SQLite as one of its persistent store types, the database format is private. You cannot create a SQLite database using the native SQLite API and use it directly with Core Data. If you have an existing SQLite database, you need to import it into a Core Data store. In addition, do not manipulate an existing Core Data-created SQLite store using the native SQLite API.

  

  
  ### I have a to-many relationship from Entity A to Entity B. How do I fetch the instances of Entity B related to a given instance of Entity A?

  
  You don’t. More specifically, there is no need to explicitly *fetch* the destination instances, you simply invoke the appropriate key-value coding or accessor method on the instance of Entity A. If the relationship is called widgets, then if you have implemented a custom class with a similarly named accessor method, you simply write:

  
  
      
          Objective-C

        

            NSSet *asWidgets = [instanceA widgets];

        

      
      
          Swift

        

            - `let asWidets = instanceA.widgets`

        

      
  

  Otherwise you use key-value coding:

  
  
      
          Objective-C

        

            NSMutableSet *asWidgets = [instanceA mutableSetValueForKey:@"widgets"];

        

      
      
          Swift

        

            - `let asWidgets = instanceA.mutableSetValueForKey("widgets")`

        

      
  

  

  
  ### How do I fetch objects in the same order I created them?

  
  Objects in a persistent store are unordered. Typically you should impose order at the controller or view layer, based on an attribute such as creation date. If there is order inherent in your data, you need to explicitly model that.

  

  
  ### How do I copy a managed object from one context to another?

  
  First, note that in a strict sense you are not copying the object. You are conceptually creating an additional reference to the same underlying data in the persistent store.

  To copy a managed object from one context to another, you can use the object’s object ID, as illustrated in the following example.

  
  
      
          Objective-C

        

            NSManagedObjectID *objectID = [managedObject objectID];

            NSManagedObject *copy = [context2 objectWithID:objectID];

        

      
      
          Swift

        

            - `let objectID = managedObject.objectID`

            - `let copy = context2.objectWithID(objectID)`

        

      
  

  

  
  ### I have a key whose value is dependent on values of attributes in a related entity—how do I ensure it is kept up to date as the attribute values are changed and as the relationship is manipulated?

  
  There are many situations in which the value of one property depends on that of one or more other attributes in another entity. If the value of one attribute changes, the value of the derived property should also be flagged for change. How you ensure that key-value observing notifications are posted for these dependent properties depends on which version of OS X you’re using and the cardinality of the relationship.

  
  
  ### macOS v10.5 and later for a to-one relationship

  
  If there is a to-one relationship to the related entity, to trigger notifications automatically, either override `[keyPathsForValuesAffectingValueForKey:](https://developer.apple.com/documentation/objectivec/nsobject/1414299-keypathsforvaluesaffectingvalue)` or implement a suitable method that follows the pattern `[keyPathsForValuesAffectingValueForKey:](https://developer.apple.com/documentation/objectivec/nsobject/1414299-keypathsforvaluesaffectingvalue)` defines for registering dependent keys.

  For example, you could override `[keyPathsForValuesAffectingValueForKey:](https://developer.apple.com/documentation/objectivec/nsobject/1414299-keypathsforvaluesaffectingvalue)` as shown in the following example:

  
  
      
          Objective-C

        

            + (NSSet *)keyPathsForValuesAffectingValueForKey:(NSString *)key {

                NSSet *keyPaths = [super keyPathsForValuesAffectingValueForKey:key];

                if ([key isEqualToString:@"fullNameAndDepartment"]) {

                    NSSet *affectingKeys = [NSSet setWithObjects:@"lastName", @"firstName", @"department.deptName", nil];

                    keyPaths = [keyPaths setByAddingObjectsFromSet:affectingKeys];

                }

                return keyPaths;

            }

        

      
      
          Swift

        

            - `class override public func keyPathsForValuesAffectingValueForKey(key: String) -> Set<String> {`

            - `    var keyPaths = super.keyPathsForValuesAffectingValueForKey(key)`

            - `    if key == "fullNameAndDepartment" {`

            - `        let affectingKeys: Set = ["lastName", "firstName", "department.deptName"]`

            - `        keyPaths = keyPaths.union(affectingKeys)`

            - `    }`

            - `    return keyPaths`

            - `}`

        

      
  

  Or, to achieve the same result, you could just implement `keyPathsForValuesAffectingFullNameAndDepartment`, as shown in the following example: 

  
  
      
          Objective-C

        

            + (NSSet *)keyPathsForValuesAffectingFullNameAndDepartment {

             

                return [NSSet setWithObjects:@"lastName", @"firstName", @"department.deptName", nil];

            }

        

      
      
          Swift

        

            - `class public func keyPathsForValuesAffectingFullNameAndDepartment() -> Set<String> {`

            - `    return ["lastName", "firstName", "department.deptName"]`

            - `}`

        

      
  

  

  
  ### To-many relationships

  
  The `[keyPathsForValuesAffectingValueForKey:](https://developer.apple.com/documentation/objectivec/nsobject/1414299-keypathsforvaluesaffectingvalue)` method does not allow keypaths that include a to-many relationship. For example, suppose you have a Department entity with a to-many relationship (`employees`) to an Employee, and Employee has a `salary` attribute. You might want the Department entity to have a `totalSalary` attribute that is dependent upon the salaries of all the Employees in the relationship. You *cannot* do this with, for example, `keyPathsForValuesAffectingTotalSalary` and returning `employees.salary` as a key.

  There are two possible solutions: 

  
  Use key-value observing to register the parent (in this example, Department) as an observer of the relevant attribute of all the children (Employees in this example). You must add and remove the parent as an observer as child objects are added to and removed from the relationship (see [Registering for Key-Value Observing](../KeyValueObserving/Articles/KVOBasics.html#//apple_ref/doc/uid/20002252)). In the `[observeValueForKeyPath:ofObject:change:context:](https://developer.apple.com/documentation/objectivec/nsobject/1416553-observevalueforkeypath)` method you update the dependent value in response to changes, as illustrated in the following code fragment:

  
  
      
          Objective-C

        

            - (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change context:(void *)context {

             

                if (context != totalSalaryContext) {

                    return [super observeValueForKeyPath:keyPath ofObject:object change:change context:context];

                }

                if ([keyPath isEqualToString:@"totalSalary"]) {

                    [self setTotalSalary:[self valueForKeyPath:@"employees.@sum.salary"]];

                }

                // Deal with other observations and/or invoke super...

            }

        

      
      
          Swift

        

            - `override public func observeValue(forKeyPath keyPath: String?, of object: Any?, change: [NSKeyValueChangeKey : Any]?, context: UnsafeMutableRawPointer?) {`

            - `    switch (keyPath, context) {`

            - `    case ("totalSalary"?, totalSalaryContext?):`

            - `        self.totalSalary = self.value(forKeyPath: "employees.@sum.salary") as! NSNumber?`

            - `    default:`

            - `        return super.observeValue(forKeyPath: keyPath, of: object, change: change, context: context)`

            - `    }`

            - `}`

        

      
  

  You can register the parent with the application's notification center as an observer of its managed object context. The parent should respond to relevant change notifications posted by the children in a manner similar to that for key-value observing.

  

  
  ### In the Xcode predicate builder, why don’t I see any properties for a fetched property predicate?

  
  If you want to create a predicate for a fetched property in the predicate builder in Xcode, but don’t see any properties, you probably have not set the destination entity for the fetched property.

  

  
  ### How efficient is Core Data?

  
  Throughout the development of Core Data, the engineering team compared the runtime performance of a generic Core Data application with that of a similar application developed without using Core Data. In general, the Core Data implementation performed better. There may nevertheless be opportunities for further optimization, and the team continues to pursue performance aggressively. For a discussion of how you can ensure you use Core Data as efficiently as possible, see [Performance](Performance.html#//apple_ref/doc/uid/TP40001075-CH25-SW1).

  

  
  ### macOS

  
  These questions are only relevant to using Core Data with OS X.

  
  
  ### How do I get the GUI to validate the data entered by the user?

  
  Core Data validates all managed objects when a managed object context is sent a `save:` call. In a Core Data document-based application, this call is sent when the user saves the document. To have the GUI validate the data as it is being entered, select the Validates Immediately option for a value binding in the Interface Builder Bindings inspector. If you establish the binding programmatically, you supply in the binding options dictionary a value of `YES``true` (as an `NSNumber` object) for the key `[NSValidatesImmediatelyBindingOption](https://developer.apple.com/documentation/appkit/nsbindingoption/1458045-validatesimmediately)`. See [Binding Options](../../Reference/CocoaBindingsRef/Concepts/BindingsOptions.html#//apple_ref/doc/uid/20002304).

  For details of how to write custom validation methods, see the subclassing notes for `[NSManagedObject](https://developer.apple.com/documentation/coredata/nsmanagedobject)`.

  

  
  ### When I remove objects from a detail table view managed by an array controller, why are they not removed from the object graph?

  
  If an array controller manages the collection of objects at the destination of a relationship, by default the remove method simply removes the current selection from the relationship. If you want removed objects to be deleted from the object graph, enable the Deletes Objects On Remove option for the `contentSet` binding in Interface Builder.

  

  
  ### How do I get undo/redo for free in my nondocument app?

  
  In a Core Data document-based application, the standard `NSDocument` undo manager is replaced by the undo manager of the document’s managed object context. In a nondocument application for OS X, your window’s delegate can supply the managed object context’s undo manager using the `[windowWillReturnUndoManager:](https://developer.apple.com/documentation/appkit/nswindowdelegate/1419745-windowwillreturnundomanager)` delegate method. If your window delegate has an accessor method for the managed object context (as is the case if you use the Core Data Application template), your implementation of `[windowWillReturnUndoManager:](https://developer.apple.com/documentation/appkit/nswindowdelegate/1419745-windowwillreturnundomanager)` might be as follows:

  
  
      
          Objective-C

        

            - (NSUndoManager *)windowWillReturnUndoManager:(NSWindow *)sender {

                return [[self managedObjectContext] undoManager];

            }

        

      
      
          Swift

        

            - `func windowWillReturnUndoManager(window: NSWindow) -> NSUndoManager? {`

            - `    return managedObjectContext!.undoManager`

            - `}`

        

      
  

  

  	
 	
    		[Troubleshooting Core Data](TroubleshootingCoreData.html#//apple_ref/doc/uid/TP40001075-CH26-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2017-03-27](RevisionHistory.html#//apple_ref/doc/uid/TP40003204-SW1)