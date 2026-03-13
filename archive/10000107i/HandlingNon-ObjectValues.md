---
title: "Handling Non-Object Values"
book: "Key-Value Coding Programming Guide"
chapterId: "10000107i-CH5"
date: "2016-10-27"
description: "Conceptual information about how to access a Cocoa object&#39;s values using keys."
identifier: "//apple_ref/doc/uid/10000107i"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Handling Non-Object Values

> **Key-Value Coding Programming Guide**
> Last updated: 2016-10-27

## Handling Non-Object Values

  
  	
  		
  Typically, your key-value coding compliant object relies on the default implementation of key-value coding to automatically wrap and unwrap non-object properties, as described in [Representing Non-Object Values](DataTypes.html#//apple_ref/doc/uid/20002171-BAJEAIEE). However, you can override the default behavior. The most common reason to do so is to handle attempts to store a `nil` value on non-object properties.

  
  
> 
    Note
    
    	Because all properties in Swift are objects, this section only applies to Objective-C properties.
    	
    
  

  If your key-value coding compliant object receives a `setValue:forKey:` message with `nil` passed as the value for a non-object property, the default implementation has no appropriate, generalized course of action. It therefore sends itself a `setNilValueForKey:` message, which you can override. The default implementation of `setNilValueForKey:` raises an `[NSInvalidArgumentException](../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/data/NSInvalidArgumentException)` exception, but you can provide an appropriate, implementation-specific behavior.

  For example, the code in Listing 10-1 responds to an attempt to set a person’s age to a `nil` value by instead setting the age to 0, which is more appropriate for a floating point value. Notice that the override method calls upon its object’s superclass for any keys that it does not explicitly handle.

  
    **Listing 10-1**Example implementation of setNilValueForKey:
  
      
        

            - (void)setNilValueForKey:(NSString *)key

            {

                if ([key isEqualToString:@"age"]) {

                    [self setValue:@(0) forKey:@”age”];

                } else {

                    [super setNilValueForKey:key];

                }

            }

        

      
  

  
  
> 
    Note
    
    	For backward compatibility, when an object has overridden the deprecated `unableToSetNilForKey:` method, `setValue:forKey:` invokes that method instead of `setNilValueForKey:`.
    	
    
  

		 

  
  	
 	
    		[Defining Collection Methods](DefiningCollectionMethods.html#//apple_ref/doc/uid/10000107i-CH17-SW1)

  			[Adding Validation](Validation.html#//apple_ref/doc/uid/20002173-CJBDBHCB)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2016-10-27](RevisionHistory.html#//apple_ref/doc/uid/20001601-CJBGIAGF)