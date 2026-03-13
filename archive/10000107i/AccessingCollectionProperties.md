---
title: "Accessing Collection Properties"
book: "Key-Value Coding Programming Guide"
chapterId: "10000107i-CH4"
date: "2016-10-27"
description: "Conceptual information about how to access a Cocoa object&#39;s values using keys."
identifier: "//apple_ref/doc/uid/10000107i"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Accessing Collection Properties

> **Key-Value Coding Programming Guide**
> Last updated: 2016-10-27

## Accessing Collection Properties

  
  	
  		
  Key-value coding compliant objects expose their to-many properties in the same way that they expose other properties. You can get or set a collection object just as you would any other object using `valueForKey:` and `setValue:forKey:` (or their key path equivalents). However, when you want to manipulate the content of these collections, it’s usually most efficient to use the mutable proxy methods defined by the protocol.

  The protocol defines three different proxy methods for collection object access, each with a key and a key path variant:

  
  `[mutableArrayValueForKey:](https://developer.apple.com/documentation/objectivec/nsobject/1416339-mutablearrayvalueforkey)` and `[mutableArrayValueForKeyPath:](https://developer.apple.com/documentation/objectivec/nsobject/1414937-mutablearrayvalueforkeypath)`

  These return a proxy object that behaves like an `NSMutableArray` object.

  `[mutableSetValueForKey:](https://developer.apple.com/documentation/objectivec/nsobject/1415105-mutablesetvalueforkey)` and `[mutableSetValueForKeyPath:](https://developer.apple.com/documentation/objectivec/nsobject/1408115-mutablesetvalue)`

  These return a proxy object that behaves like an `NSMutableSet` object.

  `[mutableOrderedSetValueForKey:](https://developer.apple.com/documentation/objectivec/nsobject/1415479-mutableorderedsetvalue)` and `[mutableOrderedSetValueForKeyPath:](https://developer.apple.com/documentation/objectivec/nsobject/1407188-mutableorderedsetvalue)`

  These return a proxy object that behaves like an `NSMutableOrderedSet` object. 

  When you operate on the proxy object, adding objects to, removing objects from, or replacing objects in it, the default implementation of the protocol modifies the underlying property accordingly. This is more efficient than obtaining a non-mutable collection object with `valueForKey:`, creating a modified one with altered content, and then storing it back to the object with a `setValue:forKey:` message. In many cases, it is also more efficient than working directly with a mutable property. These methods provide the additional benefit of maintaining key-value observing compliance for the objects held in the collection object (see *[Key-Value Observing Programming Guide](../KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i)* for details.

		 

  
  	
 	
    		[Accessing Object Properties](BasicPrinciples.html#//apple_ref/doc/uid/20002170-BAJEAIEE)

  			[Using Collection Operators](CollectionOperators.html#//apple_ref/doc/uid/20002176-BAJEAIEE)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2016-10-27](RevisionHistory.html#//apple_ref/doc/uid/20001601-CJBGIAGF)