---
title: "Introduction"
book: "Predicate Programming Guide"
chapterId: "TP40001798"
date: "2014-09-17"
description: "Describes how to specify queries in Cocoa."
identifier: "//apple_ref/doc/uid/TP40001789"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Introduction

> **Predicate Programming Guide**
> Last updated: 2014-09-17

[Next](../Articles/pCreating.html)
        
        
        [](../index.html)
        
        # Introduction
Predicates provide a general means of specifying queries in Cocoa. The predicate system is capable of handling a large number of domains, including Core Data and Spotlight. This document describes predicates in general, their use, their syntax, and their limitations.

## At a Glance
In Cocoa, a *predicate* is a logical statement that evaluates to a Boolean value (true or false). There are two types of predicate, known as *comparison* and *compound*:

A *comparison predicate* compares two *expressions* using an *operator*. The expressions are referred to as the left hand side and the right hand side of the predicate (with the operator in the middle). A comparison predicate returns the result of invoking the operator with the results of evaluating the expressions.

A *compound predicate* compares the results of evaluating two or more other predicates, or negates another predicate.

Cocoa supports a wide range of types of predicate, including the following:

Simple comparisons, such as `grade == 7` or `firstName like 'Mark'`

Case or diacritic insensitive lookups, such as `name contains[cd] 'citroen'`

Logical operations, such as `(firstName beginswith 'M') AND (lastName like 'Adderley')`

You can also create predicates for relationships—such as `group.name matches 'work.*'`, `ALL children.age > 12`, and `ANY children.age > 12`—and for operations such as `@sum.items.price < 1000`. 

Cocoa predicates provide a means of encoding queries in a manner that is independent of the store used to hold the data being searched. You use predicates to represent logical conditions used for constraining the set of objects retrieved by Spotlight and Core Data, and for in-memory filtering of objects.

You can use predicates with any class of object, but a class must be [key-value coding](../../../../General/Conceptual/DevPedia-CocoaCore/KeyValueCoding.html#//apple_ref/doc/uid/TP40008195-CH25) compliant for the keys you want to use in a predicate.

### Predicate Classes
Cocoa provides `[NSPredicate](https://developer.apple.com/documentation/foundation/nspredicate)` and its two subclasses, `[NSComparisonPredicate](https://developer.apple.com/documentation/foundation/nscomparisonpredicate)` and `[NSCompoundPredicate](https://developer.apple.com/documentation/foundation/nscompoundpredicate)`.

The `NSPredicate` class provides methods to evaluate a predicate and to create a predicate from a string (such as `firstName like 'Mark'`). When you create a predicate from a string, `NSPredicate` creates the appropriate predicate and expression instances for you. In some situations, you want to create comparison or compound predicates yourself, in which case you can use the `NSComparisonPredicate` and `NSCompoundPredicate` classes.

Predicate expressions in Cocoa are represented by instances of the `[NSExpression](https://developer.apple.com/documentation/foundation/nsexpression)` class. The simplest predicate expression represents a constant value. Frequently, though, you use expressions that retrieve the value for a key path of the object currently being evaluated in the predicate. You can also create an expression to represent the object currently being evaluated in the predicate, to serve as a placeholder for a variable, or to return the result of performing an operation on an array.

For more on creating predicates and expressions, see [Creating Predicates](../Articles/pCreating.html#//apple_ref/doc/uid/TP40001793-CJBDBHCB).

### Constraints and Limitations
If you use predicates with Core Data or Spotlight, take care that they work with the corresponding data store. There is no specific implementation language for predicate queries; a predicate query may be translated into SQL, XML, or another format, depending on the requirements of the backing store (if indeed there is one). 

The Cocoa predicate system is intended to support a useful range of operators, so provides neither the set union nor the set intersection of all operators supported by all backing stores. Therefore, not all possible predicate queries are supported by all backing stores, and not all operations supported by all backing stores can be expressed with `NSPredicate` and `NSExpression` objects. The back end might downgrade a predicate (for example it might make a case-sensitive comparison case-insensitive) or raise an exception if you try to use an unsupported operator. For example:

The `matches` operator uses `regex`, so is not supported by Core Data’s SQL store— although it does work with in-memory filtering.

The Core Data SQL store supports only one to-many operation per query; therefore in any predicate sent to the SQL store, there may be only one operator (and one instance of that operator) from `ALL`, `ANY`, and `IN`.

You cannot necessarily translate arbitrary SQL queries into predicates.

The `ANYKEY` operator can only be used with Spotlight.

Spotlight does not support relationships.

## How to Use This Document
The following articles explain the basics of predicates in Cocoa, explain how to create and use predicate objects, and define the predicate syntax:

[Creating Predicates](../Articles/pCreating.html#//apple_ref/doc/uid/TP40001793-CJBDBHCB) describes how to correctly instantiate predicates programmatically and how to retrieve them from a managed object model.

[Using Predicates](../Articles/pUsing.html#//apple_ref/doc/uid/TP40001794-CJBDBHCB) explains how to use predicates and discusses some issues related to performance.

[Comparison of NSPredicate and Spotlight Query Strings](../Articles/pSpotlightComparison.html#//apple_ref/doc/uid/TP40002370-SW1) compares NSPredicate and Spotlight queries.

[Predicate Format String Syntax](../Articles/pSyntax.html#//apple_ref/doc/uid/TP40001795-CJBDBHCB) describes the syntax of the predicate format string.

[BNF Definition of Cocoa Predicates](../Articles/pBNF.html#//apple_ref/doc/uid/TP40001796-CJBDBHCB) provides a definition of Cocoa predicates in Backus-Naur Form notation.

        
            [Next](../Articles/pCreating.html)
        
         Copyright &#x00a9; 2005, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-09-17