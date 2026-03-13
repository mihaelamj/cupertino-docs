---
title: "Designing for Performance"
book: "Key-Value Coding Programming Guide"
chapterId: "20002175"
date: "2016-10-27"
description: "Conceptual information about how to access a Cocoa object&#39;s values using keys."
identifier: "//apple_ref/doc/uid/10000107i"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Designing for Performance

> **Key-Value Coding Programming Guide**
> Last updated: 2016-10-27

## Designing for Performance

  
  	
  		
  Key-value coding is efficient, especially when you rely on the default implementation to do most of the work, but it does add a level of indirection that is slightly slower than direct method invocations. Use key-value coding only when you can benefit from the flexibility that it provides, or to allow your objects to participate in the Cocoa technologies that depend on it.

		 

  
  
  ### Overriding Key-Value Coding Methods

  
  Typically, you make your objects key-value coding compliant by ensuring that they inherit from `NSObject`, and then providing the property-specific accessors and related methods as described throughout this book. You rarely need to override the default implementations of the key-value coded accessors, such as `valueForKey:` and `setValue:forKey:`, or the key-based validation methods like `validateValue:forKey:`. Because these implementations cache information about the runtime environment to increase efficiency, if you do override them to introduce custom logic, make sure that you invoke the default implementation in the superclass before returning. 

  

  
  ### Optimizing To-Many Relationships

  
  When you implement to-many relationships, the indexed form of the accessors provides significant performance gains in many cases, especially for mutable collections. See [Accessing Collection Properties](AccessingCollectionProperties.html#//apple_ref/doc/uid/10000107i-CH4-SW1) and [Defining Collection Methods](DefiningCollectionMethods.html#//apple_ref/doc/uid/10000107i-CH17-SW1) for further information.

  

  	
 	
    		[Describing Property Relationships](Relationships.html#//apple_ref/doc/uid/20000956-BBCDDGCC)

  			[Compliance Checklist](Compliant.html#//apple_ref/doc/uid/20002172-BAJEAIEE)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2016-10-27](RevisionHistory.html#//apple_ref/doc/uid/20001601-CJBGIAGF)