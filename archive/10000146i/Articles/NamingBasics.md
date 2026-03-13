---
title: "Code Naming Basics"
book: "Coding Guidelines for Cocoa"
chapterId: "20001281"
date: "2013-10-22"
description: "Provides naming guidelines for Cocoa API and design advice to framework developers."
identifier: "//apple_ref/doc/uid/10000146i"
source: apple-archive
---

# Code Naming Basics

> **Coding Guidelines for Cocoa**
> Last updated: 2013-10-22

[Next](NamingMethods.html)[Previous](../CodingGuidelines.html)
        
        
        [](../index.html)
        
        # Code Naming Basics

An often overlooked aspect of the design of object-oriented software libraries is the naming of classes, methods, functions, constants, and the other elements of a programmatic interface. This section discusses several of the naming [conventions](../../../../General/Conceptual/DevPedia-CocoaCore/CodingConventions.html#//apple_ref/doc/uid/TP40008195-CH53) common to most items of a [Cocoa](../../../../General/Conceptual/DevPedia-CocoaCore/Cocoa.html#//apple_ref/doc/uid/TP40008195-CH9) interface.

## General Principles

*Clarity*

It is good to be both clear and brief as possible, but clarity shouldn’t suffer because of brevity: 

Code

Commentary

`insertObject:atIndex:`

Good.

`insert:at:`

Not clear; what is being inserted? what does “at” signify?

`removeObjectAtIndex:`

Good.

`removeObject:`

Good, because it removes object referred to in argument.

`remove:`

Not clear; what is being removed?

In general, don’t abbreviate names of things. Spell them out, even if they’re long:

Code

Commentary

`destinationSelection`

Good.

`destSel`

Not clear.

`setBackgroundColor:`

Good.

`setBkgdColor:`

Not clear.

You may think an abbreviation is well-known, but it might not be, especially if the developer encountering your method or function name has a different cultural and linguistic background. 

However, a handful of abbreviations are truly common and have a long history of use. You can continue to use them; see [Acceptable Abbreviations and Acronyms](APIAbbreviations.html#//apple_ref/doc/uid/20001285-BCIHCGAE).

Avoid ambiguity in API names, such as method names that could be interpreted in more than one way.

Code

Commentary

`sendPort`

Does it send the port or return it?

`displayName`

Does it display a name or return the receiver’s title in the user interface?

*Consistency*

Try to use names consistently throughout the Cocoa programmatic interfaces. If you are unsure, browse the current header files or reference documentation for precedents.

Consistency is especially important when you have a class whose methods should take advantage of polymorphism. Methods that do the same thing in different classes should have the same name.

Code

Commentary

`- (NSInteger)tag`

Defined in `NSView`, `NSCell`, `NSControl`.

`- (void)setStringValue:(NSString *)`

Defined in a number of Cocoa classes.

See also [Method Arguments](NamingMethods.html#//apple_ref/doc/uid/20001282-1001865).

*No Self Reference*

Names shouldn’t be self-referential.

Code

Commentary

`NSString`

Okay.

`NSStringObject`

Self-referential.

Constants that are masks (and thus can be combined in bitwise operations) are an exception to this rule, as are constants for [notification](../../../../General/Conceptual/DevPedia-CocoaCore/Notification.html#//apple_ref/doc/uid/TP40008195-CH35) names.

Code

Commentary

`NSUnderlineByWordMask`

Okay.

`NSTableViewColumnDidMoveNotification`

Okay.

## Prefixes

Prefixes are an important part of names in programmatic interfaces. They differentiate functional areas of software. Usually this software comes packaged in a [framework](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56) or (as is the case of Foundation and Application Kit) in closely related frameworks. Prefixes protect against collisions between symbols defined by third-party developers and those defined by Apple (as well as between symbols in Apple’s own frameworks). 

A prefix has a prescribed format. It consists of two or three uppercase letters and does not use underscores or “sub prefixes.” Here are some examples:

Prefix

Cocoa Framework

NS

Foundation

NS

Application Kit

AB

Address Book

IB

Interface Builder

Use prefixes when naming classes, [protocols](../../../../General/Conceptual/DevPedia-CocoaCore/Protocol.html#//apple_ref/doc/uid/TP40008195-CH45), functions, constants, and `typedef` structures. Do *not* use prefixes when naming methods; methods exist in a name space created by the class that defines them. Also, don’t use prefixes for naming the fields of a structure.

## Typographic Conventions

Follow a few simple typographical conventions when naming API elements:

For names composed of multiple words, do not use punctuation marks as parts of names or as separators (underscores, dashes, and so on); instead, capitalize the first letter of each word and run the words together (for example, `runTheWordsTogether`)—this is known as camel-casing. However, note the following qualifications:

For method names, start with a lowercase letter and capitalize the first letter of embedded words. Don’t use prefixes.

fileExistsAtPath:isDirectory:An exception to this guideline is method names that start with a well-known acronym, for example, `TIFFRepresentation` (`NSImage`).

For names of functions and constants, use the same prefix as for related classes and capitalize the first letter of embedded words.

```
NSRunAlertPanel
```

```
NSCellDisabled
```

Avoid the use of the underscore character as a prefix meaning private in *method* names (using an underscore character as a prefix for an *instance variable* name is allowed). Apple reserves the use of this convention. Use by third parties could result in name-space collisions; they might unwittingly [override](../../../../General/Conceptual/DevPedia-CocoaCore/MethodOverriding.html#//apple_ref/doc/uid/TP40008195-CH57) an existing private method with one of their own, with disastrous consequences. See [Private Methods](NamingMethods.html#//apple_ref/doc/uid/20001282-1003829) for suggestions on conventions to follow for private API.

## Class and Protocol Names

The name of a class should contain a noun that clearly indicates what the class (or objects of the class) represent or do. The name should have an appropriate prefix (see [Prefixes](#//apple_ref/doc/uid/20001281-1002226)). The Foundation and application frameworks are full of examples; a few are `NSString`, `NSDate`, `NSScanner`, `NSApplication`, `UIApplication`, `NSButton`, and `UIButton`. 

[Protocols](../../../../General/Conceptual/DevPedia-CocoaCore/Protocol.html#//apple_ref/doc/uid/TP40008195-CH45) should be named according to how they group behaviors:

Most *protocols* group related methods that aren’t associated with any class in particular. This type of protocol should be named so that the protocol won’t be confused with a class. A common convention is to use a gerund (“...ing”) form:

Code

Commentary

`NSLocking`

Good.

`NSLock`

Poor (seems like a name for a class).

Some protocols group a number of unrelated methods (rather than create several separate small protocols). These protocols tend to be associated with a class that is the principal expression of the protocol. In these cases, the convention is to give the protocol the same name as the class.

An example of this sort of protocol is the `[NSObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intf/NSObject)` protocol. This protocol groups methods that you can use to query any object about its position in the class hierarchy, to make it invoke specific methods, and to increment or decrement its reference count. Because the `[NSObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/cl/NSObject)` class provides the primary expression of these methods, the protocol is named after the class.

## Header Files

How you name header files is important because the convention you use indicates what the file contains:

*Declaring an isolated class or protocol*. If a class or protocol isn’t part of a group, put its declaration in a separate file whose name is that of the declared class or protocol.

Header file

Declares

`NSLocale.h`

The `NSLocale` class.

*Declaring related classes and protocols*. For a group of related declarations (classes, categories, and protocols), put the declarations in a file that bears the name of the primary class, [category](../../../../General/Conceptual/DevPedia-CocoaCore/Category.html#//apple_ref/doc/uid/TP40008195-CH5), or [protocol](../../../../General/Conceptual/DevPedia-CocoaCore/Protocol.html#//apple_ref/doc/uid/TP40008195-CH45).

Header file

Declares

`NSString.h`

`NSString` and `NSMutableString` classes.

`NSLock.h`

`NSLocking` protocol and `NSLock`, `NSConditionLock`, and `NSRecursiveLock` classes.

*Including framework header files*. Each framework should have a header file, named after the framework, that includes all the public header files of the framework.

Header file

Framework

`Foundation.h`

`Foundation.framework`.

*Adding API to a class in another framework*. If you declare methods in one framework that are in a category on a class in another framework, append “Additions” to the name of the original class; an example is the `NSBundleAdditions.h` header file of the Application Kit.

*Related functions and data types*. If you have a group of related functions, constants, structures, and other data types, put them in an appropriately named header file such as `NSGraphics.h` (Application Kit).

        
            [Next](NamingMethods.html)[Previous](../CodingGuidelines.html)
        
         Copyright &#x00a9; 2003, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-10-22