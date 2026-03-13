---
title: "Part I: Getting Started"
book: "Core Data Programming Guide"
chapterId: "TP40001075-CH2"
date: "2017-03-27"
description: "Explains how to manage objects using the Core Data framework."
identifier: "//apple_ref/doc/uid/TP40001075"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Part I: Getting Started

> **Core Data Programming Guide**
> Last updated: 2017-03-27

## What Is Core Data?

    

  
  	
  		
  Core Data is a framework that you use to manage the model layer objects in your application. It provides generalized and automated solutions to common tasks associated with object life cycle and object graph management, including persistence.

  Core Data typically decreases by 50 to 70 percent the amount of code you write to support the model layer. This is primarily due to the following built-in features that you do not have to implement, test, or optimize:

  
  Change tracking and built-in management of undo and redo beyond basic text editing.

  Maintenance of change propagation, including maintaining the consistency of relationships among objects.

  Lazy loading of objects, partially materialized futures (faulting), and copy-on-write data sharing to reduce overhead.

  Automatic validation of property values. Managed objects extend the standard key-value coding validation methods to ensure that individual values lie within acceptable ranges, so that combinations of values make sense.

  Schema migration tools that simplify schema changes and allow you to perform efficient in-place schema migration.

  Optional integration with the application’s controller layer to support user interface synchronization.

  Grouping, filtering, and organizing data in memory and in the user interface.

  Automatic support for storing objects in external data repositories.

  Sophisticated query compilation. Instead of writing SQL, you can create complex queries by associating an NSPredicate object with a fetch request.

  Version tracking and optimistic locking to support automatic multiwriter conflict resolution.

  Effective integration with the macOS and iOS tool chains.

  
  
> 
    Note
    
    	This document uses an employees database-style example for expediency and clarity. It represents a rich but easily understood problem domain. However, the Core Data framework is not restricted to database-style applications, nor is there an expectation of client-server behavior. The framework is equally as useful as the basis of a vector graphics application such as Sketch or a presentation application such as Keynote.
    	
    
  

		 

  
  	
 	
  			[Creating a Managed Object Model](KeyConcepts.html#//apple_ref/doc/uid/TP40001075-CH30-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2017-03-27](RevisionHistory.html#//apple_ref/doc/uid/TP40003204-SW1)