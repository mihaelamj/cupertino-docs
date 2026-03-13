---
title: "Validating Properties"
book: "Key-Value Coding Programming Guide"
chapterId: "10000107i-CH18"
date: "2016-10-27"
description: "Conceptual information about how to access a Cocoa object&#39;s values using keys."
identifier: "//apple_ref/doc/uid/10000107i"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Validating Properties

> **Key-Value Coding Programming Guide**
> Last updated: 2016-10-27

## Validating Properties

  
  	
  		
  The key-value coding protocol defines methods to support property validation. Just as you use key-based accessors to read and write properties of a key-value coding compliant object, you can also validate properties by key (or key path). When you call the `[validateValue:forKey:error:](https://developer.apple.com/documentation/objectivec/nsobject/1416754-validatevalue)` (or the `[validateValue:forKeyPath:error:](https://developer.apple.com/documentation/objectivec/nsobject/1416245-validatevalue)`) method, the default implementation of the protocol searches the object receiving the validation message (or the one at the end of the key path) for a method whose name matches the pattern `validate<Key>:error:`. If the object has no such method, the validation succeeds by default, and the default implementation returns `YES``true`. When a property-specific validation method exists, the default implementation returns the result of calling that method instead.

  
  
> 
    Note
    
    	You typically use the validation described here only in Objective-C. In Swift, property validation is more idiomatically handled by relying on compiler support for optionals and strong type checking, while using the built-in `willSet` and `didSet` property observers to test any run-time API contracts, as described in the Property Observers section of *The Swift Programming Language (Swift 3)*.
    	
    
  

  Because property-specific validation methods receive the value and error parameters by reference, validation has three possible outcomes:

  

  
  The validation method deems the value object valid and returns `YES``true` without altering the value or the error.

  The validation method deems the value object invalid, but chooses not to alter it. In this case, the method returns `NO``false` and sets the error reference (if provided by the caller) to an `NSError` object that indicates the reason for failure.

  The validation method deems the value object invalid, but creates a new, valid one as a replacement. In this case, the method returns `YES``true` while leaving the error object untouched. Before returning, the method modifies the value reference to point at the new value object. When it makes a modification, the method always creates a new object, rather than modifying the old one, even if the value object is mutable.

  Listing 6-1 shows an example of how to call validation for a name string.

  
    **Listing 6-1**Validation of the name property
  
      
        

            Person* person = [[Person alloc] init];

            NSError* error;

            NSString* name = @"John";

            if (![person validateValue:&name forKey:@"name" error:&error]) {

                NSLog(@"%@",error);

            }

        

      
  

		 

  
  
  ### Automatic Validation

  
  In general, neither the key-value coding protocol nor its default implementation define any mechanism to perform validation automatically. Instead, you make use of the validation methods when appropriate for your app.

  Certain other Cocoa technologies do perform validation automatically in some circumstances. For example, Core Data automatically performs validation when the managed object context is saved (see *[Core Data Programming Guide](../CoreData/index.html#//apple_ref/doc/uid/TP40001075)*). Also, in macOS, Cocoa bindings allow you to specify that validation should occur automatically (see *[Cocoa Bindings Programming Topics](../CocoaBindings/CocoaBindings.html#//apple_ref/doc/uid/10000167i)* for more information).

  

  	
 	
    		[Representing Non-Object Values](DataTypes.html#//apple_ref/doc/uid/20002171-BAJEAIEE)

  			[Accessor Search Patterns](SearchImplementation.html#//apple_ref/doc/uid/20000955-CJBBBFFA)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2016-10-27](RevisionHistory.html#//apple_ref/doc/uid/20001601-CJBGIAGF)