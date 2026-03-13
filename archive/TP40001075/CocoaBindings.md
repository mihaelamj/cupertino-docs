---
title: "Part III: Integrating Core Data with OS X"
book: "Core Data Programming Guide"
chapterId: "TP40001075-CH12"
date: "2017-03-27"
description: "Explains how to manage objects using the Core Data framework."
identifier: "//apple_ref/doc/uid/TP40001075"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Part III: Integrating Core Data with OS X

> **Core Data Programming Guide**
> Last updated: 2017-03-27

## Using Core Data with Cocoa Bindings

  
  	
  		
  
  
> 
    Note
    
    	Cocoa bindings are not available in iOS.
    	
    
  

  The Core Data framework focuses on the object model, while Cocoa bindings focus on the user interface. Cocoa bindings make it easy to keep the user interface properly synchronized and provide integration between the model, controller, and the user interface (the view in MVC). The Core Data framework is designed to interoperate seamlessly with and enhance the utility of Cocoa bindings. For more information, see *[Cocoa Bindings Reference](../../Reference/CocoaBindingsRef/CocoaBindingsRef.html#//apple_ref/doc/uid/10000189i)*.

  In general, Cocoa bindings work in exactly the same way with Core Data managed objects as they do with other Cocoa model objects. Core Data uses the  same predicate objects and sort descriptors that you would use against data that is not managed by Core Data—for example, to present data in a table view. These similarities between Core Data and Cocoa bindings give you a consistent API set to use throughout your application.

  Core Data and Cocoa bindings have a few differences in configuration and operation that are discussed in this chapter. Also see [Troubleshooting Core Data](TroubleshootingCoreData.html#//apple_ref/doc/uid/TP40001075-CH26-SW1) for details about these issues that may cause interaction problems between Cocoa bindings and Core Data:

  
  [Cannot access contents of an object controller after a nib is loaded](TroubleshootingCoreData.html#//apple_ref/doc/uid/TP40001075-CH26-SW11)

  [Table view or outline view contents not kept up-to-date when bound to an NSArrayController or NSTreeController object](TroubleshootingCoreData.html#//apple_ref/doc/uid/TP40001075-CH26-SW14)

  With these exceptions, everything that is discussed and described in *[Cocoa Bindings Programming Topics](../CocoaBindings/CocoaBindings.html#//apple_ref/doc/uid/10000167i)* applies equally to Core Data-based applications. Use the same techniques for configuring and debugging bindings, regardless of whether you are using Core Data.

		 

  
  
  ### Core Data Additions to Controllers

  
  Core Data adds features to Cocoa bindings primarily in the configuration of controller objects such as `[NSObjectController](https://developer.apple.com/documentation/appkit/nsobjectcontroller)` and `[NSArrayController](https://developer.apple.com/documentation/appkit/nsarraycontroller)`. Core Data adds the following features to those classes:

  
  A reference to a managed object context that is used for all fetches, insertions, and deletions. If a controller's content is a managed object or collection of managed objects, you must either bind or set the managed object context for the controller.

  An entity name that is used instead of the content object class to create new objects.

  A reference to a fetch predicate that constrains what is fetched to set the content if the content is not set directly.

  A content binding option (Deletes Objects On Remove) in Interface Builder that—if the content is bound to a relationship—specifies whether objects removed from the controller are deleted in addition to being removed from the relationship.

  

  
  ### Automatically Prepares Content Flag

  
  If the `[automaticallyPreparesContent](https://developer.apple.com/documentation/appkit/nsobjectcontroller/1534767-automaticallypreparescontent)` flag (see, for example, `setAutomaticallyPreparesContent:`) is set (for example, in Interface Builder) for a controller, the controller's initial content is fetched from its managed object context using the controller's current fetch predicate. If the flag is not set, the data is not retrieved from the managed object context until `[prepareContent](https://developer.apple.com/documentation/appkit/nsobjectcontroller/1534218-preparecontent)` is called. The `AutomaticallyPreparesContent` flag is available on both the `[NSObjectController](https://developer.apple.com/documentation/appkit/nsobjectcontroller)` and the `[NSArrayController](https://developer.apple.com/documentation/appkit/nsarraycontroller)`, regardless of whether Core Data is used. 

  It is important to note that the controller's fetch is executed as a delayed operation performed after the controller’s managed object context is set (by nib loading). Therefore, the fetch happens after `[awakeFromNib](https://developer.apple.com/documentation/objectivec/nsobject/1402907-awakefromnib)` and `[windowControllerDidLoadNib:](https://developer.apple.com/documentation/appkit/nsdocument/1515221-windowcontrollerdidloadnib)` are called. The order of operations can create a problem if you want to perform an operation with the contents of an object controller in either of these methods, because the controller's content is `nil`. You can work around this by executing the fetch manually with `[fetchWithRequest:merge:error:](https://developer.apple.com/documentation/appkit/nsobjectcontroller/1531782-fetch)`. You pass `nil` as the fetch request argument to use the default request, as illustrated in the following code fragment:

  
  
      
          Objective-C

        

            - (void)windowControllerDidLoadNib:(NSWindowController *)windowController

            {

                [super windowControllerDidLoadNib:windowController];

             

                NSArrayController *arrayController = [self arrayController];

                NSError *error = nil;

                BOOL ok = [arrayController fetchWithRequest:nil merge:NO error:&error];

                if (ok == NO) {

                    NSLog(@"Failed to perform fetch: %@", error);

                    abort();

                }

            }

        

      
      
          Swift

        

            - `override func windowControllerDidLoadNib(windowController: NSWindowController) {`

            - `    super.windowControllerDidLoadNib(windowController)`

            - `    `

            - `    do {`

            - `        try arrayController.fetch(with: nil, merge: false)`

            - `    } catch {`

            - `        fatalError("Failed to perform fetch: \(error)")`

            - `    }`

            - `}`

        

      
  

  

  
  ### Entity Inheritance

  
  If you specify a parent entity in Core Data as the entity for a Core Data fetch request, the fetch returns matching instances of the entity and subentities (see [Fetching Objects](FetchingObjects.html#//apple_ref/doc/uid/TP40001075-CH6-SW1)). As a corollary, if you specify a parent entity as the entity for a controller that is backed by Core Data, it fetches matching instances of the entity and any subentities. If you specify an abstract parent entity, the Core Data backed controller fetches matching instances of concrete subentities.

  

  
  ### Filter Predicate for a to-Many Relationship

  
  Sometimes you may want to set up a filter predicate for a search field that lets a user filter the contents of an array controller based on the destination of a to-many relationship. If you want to search a to-many relationship, you need to use an `ANY` or `ALL` operator in the predicate. For example, if you want to fetch departments in which at least one of the employees has the first name Matthew, you use an `ANY` operator as shown in the following example:

  
  
      
          Objective-C

        

            NSPredicate *predicate = [NSPredicate predicateWithFormat:

                @"ANY employees.firstName like 'Matthew'"];

        

      
      
          Swift

        

            - `let predicate = NSPredicate(format: "ANY employees.firstName like 'Matthew'", argumentArray: nil)`

        

      
  

  You use the same syntax in a search field's predicate binding:

  
  
      
        

            - `ANY employees.firstName like $value`

        

      
  

  Things are more complex, however, if you want to match a prefix or suffix or both—for example, if you want to look for departments in which at least one of the employees has the first name Matt, Matthew, Mattie, or any other name beginning with Matt. Fundamentally you simply need to add wildcard matching:

  
  
      
          Objective-C

        

            NSPredicate *predicate = [NSPredicate predicateWithFormat:

                @"ANY employees.firstName like 'Matt*'"];

        

      
      
          Swift

        

            - `let predicate = NSPredicate(format: "ANY employees.firstName like 'Matt*'", argumentArray: nil)`

        

      
  

  You *cannot*, though, use the same syntax within a search field's predicate binding:

  
  
      
        

            - `// does not work`

            - `ANY employees.firstName like '$value*'`

        

      
  

  The reasons for this are described in *[Predicate Programming Guide](../Predicates/AdditionalChapters/Introduction.html#//apple_ref/doc/uid/TP40001789)*—putting quotes in the predicate format prevents the variable substitution from happening. Instead, within a search field’s predicate binding, you must substitute any wildcards first, as illustrated in this example:

  
  
      
          Objective-C

        

            NSString *value = @"Matt";

            NSString *wildcardedString = [NSString stringWithFormat:@"%@*", value];

            [[NSPredicate predicateWithFormat:@"ANY employees.firstName like %@", wildcardedString];

        

      
      
          Swift

        

            - `let value = "Matt"`

            - `let wildcardedString = "\(value)*"`

            - `let predicate = NSPredicate(format: "ANY employees.firstName like %@", wildcardedString)`

        

      
  

  By implication, therefore, you must write some code to support the substitution of wildcards due to a limitation in the combination of Interface Builder and wildcard substitution variables in `[NSPredicate](https://developer.apple.com/documentation/foundation/nspredicate)`.

  
  
> 
    Note
    
    	You may find that search field predicate bindings filter results inconsistently with wildcard characters. This is due to a bug in `[NSArrayController](https://developer.apple.com/documentation/appkit/nsarraycontroller)`. The workaround is to create a subclass of `[NSArrayController](https://developer.apple.com/documentation/appkit/nsarraycontroller)` and override `[arrangeObjects:](https://developer.apple.com/documentation/appkit/nsarraycontroller/1533881-arrangeobjects)` to simply invoke `super`’s implementation.

    	
    
  

  

  	
 	
    		[Integrating Core Data and Storyboards](CoreDataandStoryboards.html#//apple_ref/doc/uid/TP40001075-CH10-SW1)

  			[Managed Objects and References](MO_Lifecycle.html#//apple_ref/doc/uid/TP40001075-CH31-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2017-03-27](RevisionHistory.html#//apple_ref/doc/uid/TP40003204-SW1)