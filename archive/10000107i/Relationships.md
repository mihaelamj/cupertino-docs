---
title: "Describing Property Relationships"
book: "Key-Value Coding Programming Guide"
chapterId: "20000956"
date: "2016-10-27"
description: "Conceptual information about how to access a Cocoa object&#39;s values using keys."
identifier: "//apple_ref/doc/uid/10000107i"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Describing Property Relationships

> **Key-Value Coding Programming Guide**
> Last updated: 2016-10-27

## Describing Property Relationships

  
  	
  		
  Class descriptions provide a method of describing the to-one and to-many properties in a class. Defining these relationships between class properties allows for more intelligent and flexible manipulation of these properties with key-value coding.

		 

  
  
  ### Class Descriptions

  
  `NSClassDescription` is a base class that provides the interface for obtaining meta-data about classes. A class description object records the available attributes of objects of a particular class and the relationships (one-to-one, one-to-many, and inverse) between objects of that class and other objects. For example the `attributeKeys` method returns the list of all attributes defined for a class; the methods `toManyRelationshipKeys` and `toOneRelationshipKeys` return arrays of keys that define to-many and to-one relationships; and `inverseRelationshipKey:` returns the name of the relationship pointing back to the receiver from the destination of the relationship for the provided key.

  `NSClassDescription` does not define methods for defining the relationships. Concrete subclasses must define these methods. Once created, you register a class description with the NSClassDescription `registerClassDescription:forClass:` class method.

  `NSScriptClassDescription` is the only concrete subclass of NSClassDescription provided in Cocoa. It encapsulates an application’s scripting information.

  

  	
 	
    		[Adding Validation](Validation.html#//apple_ref/doc/uid/20002173-CJBDBHCB)

  			[Designing for Performance](Performance.html#//apple_ref/doc/uid/20002175-CJBDBHCB)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2016-10-27](RevisionHistory.html#//apple_ref/doc/uid/20001601-CJBGIAGF)