---
title: "About Property Lists"
book: "Property List Programming Guide"
chapterId: "10000048i-CH3"
date: "2010-03-24"
description: "Explains how to use structured, textual representations of data in Cocoa."
identifier: "//apple_ref/doc/uid/10000048i"
source: apple-archive
---

# About Property Lists

> **Property List Programming Guide**
> Last updated: 2010-03-24

[Next](../CreatePropListProgram/CreatePropListProgram.html)[Previous](../QuickStartPlist/QuickStartPlist.html)
        
        
        [](../index.html)
        
        # About Property Lists
A property list is a structured data representation used by Cocoa and Core Foundation as a convenient way to store, organize, and access standard types of data. It is colloquially referred to as a “plist.” Property lists are used extensively by applications and other software on OS X and iOS. For example, the OS X Finder—through [bundles](../../../../General/Conceptual/DevPedia-CocoaCore/Bundle.html#//apple_ref/doc/uid/TP40008195-CH4)—uses property lists to store file and directory attributes. Applications on iOS use property lists in their Settings bundle to define the list of options displayed to users. This section explains what property lists are and when you should use them.

> **Note:** Although user and application preferences use property lists to structure and store data, you should not use the property list API of either Cocoa or Core Foundation to read and modify them. Instead, use the programming interfaces provided specifically for this purpose—see *[Preferences and Settings Programming Guide](../../UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)* and *[Preferences Programming Topics for Core Foundation](../../../../CoreFoundation/Conceptual/CFPreferences/CFPreferences.html#//apple_ref/doc/uid/10000129i)* for more information.

## What is a Property List?
Property lists are based on an abstraction for expressing simple hierarchies of data. The items of data in a property list are of a limited number of types. Some types are for primitive values and others are for containers of values. The primitive types are strings, numbers, binary data, dates, and Boolean values. The containers are arrays—indexed [collections](../../../../General/Conceptual/DevPedia-CocoaCore/Collection.html#//apple_ref/doc/uid/TP40008195-CH10) of values—and dictionaries—collections of values each identified by a key. The containers can contain other containers as well as the primitive types. Thus you might have an array of dictionaries, and each dictionary might contain other arrays and dictionaries, as well as the primitive types. A root property-list object is at the top of this hierarchy, and in almost all cases is a dictionary or an array. Note, however, that a root property-list object does not have to be a dictionary or array; for example, you could have a single string, number, or date, and that primitive value by itself can constitute a property list.

From the basic abstraction derives both a static representation of the property-list data and a runtime representation of the property list. The static representation of a property list, which is used for storage, can be either XML or binary data. (The binary version is a more compact form of the XML property list.) In XML, each type is represented by a certain element. The runtime representation of a property list is based on objects corresponding to the abstract types. The objects can be Cocoa or Core Foundation objects. Table 2-1 lists the types and their corresponding static and runtime representations.

**Table 2-1**  Property list types and their various representations Abstract type

XML element

Cocoa class

Core Foundation type

array

`<array>`

`[NSArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSArray)`

`CFArray` (`[CFArrayRef](https://developer.apple.com/documentation/corefoundation/cfarray)`)

dictionary

`<dict>`

`[NSDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSDictionary)`

`CFDictionary` (`[CFDictionaryRef](https://developer.apple.com/documentation/corefoundation/cfdictionaryref)`)

string

`<string>`

`[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)`

`CFString` (`[CFStringRef](https://developer.apple.com/documentation/corefoundation/cfstringref)`)

data

`<data>`

`[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)`

`CFData` (`[CFDataRef](https://developer.apple.com/documentation/corefoundation/cfdata)`)

date

`<date>`

`[NSDate](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateClassCluster/Description.html#//apple_ref/occ/cl/NSDate)`

`CFDate` (`[CFDateRef](https://developer.apple.com/documentation/corefoundation/cfdateref)`)

number - integer

`<integer>`

`[NSNumber](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumber/Description.html#//apple_ref/occ/cl/NSNumber)` (`[intValue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumber/Description.html#//apple_ref/occ/instm/NSNumber/intValue)`)

`CFNumber` (`[CFNumberRef](https://developer.apple.com/documentation/corefoundation/cfnumberref)`, integer value)

number - floating point

`<real>`

`[NSNumber](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumber/Description.html#//apple_ref/occ/cl/NSNumber)` (`[floatValue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumber/Description.html#//apple_ref/occ/instm/NSNumber/floatValue)`)

`CFNumber` (`[CFNumberRef](https://developer.apple.com/documentation/corefoundation/cfnumberref)`, floating-point value)

Boolean

`<true/>` or `<false/>`

`[NSNumber](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumber/Description.html#//apple_ref/occ/cl/NSNumber)` (`[boolValue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumber/Description.html#//apple_ref/occ/instm/NSNumber/boolValue)` == `YES` or `[boolValue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumber/Description.html#//apple_ref/occ/instm/NSNumber/boolValue)` == `NO`)

`CFBoolean` (`[CFBooleanRef](https://developer.apple.com/documentation/corefoundation/cfbooleanref)` ; `[kCFBooleanTrue](https://developer.apple.com/documentation/corefoundation/kcfbooleantrue)` or `[kCFBooleanFalse](https://developer.apple.com/documentation/corefoundation/kcfbooleanfalse)`)

By convention, each Cocoa and Core Foundation object listed in Table 2-1 is called a *property-list object*. Conceptually, you can think of “property list” as being an abstract superclass of all these classes. If you receive a property list object from some method or function, you know that it must be an instance of one of these types, but *a priori* you may not know which type. If a property-list object is a container (that is, an array or dictionary), all objects contained within it must also be property-list objects. If an array or dictionary contains objects that are not property-list objects, then you cannot save and restore the hierarchy of data using the various property-list methods and functions. And although `NSDictionary` and CFDictionary objects allow their keys to be objects of any type, if the keys are not string objects, the collections are not property-list objects. 

Because all these types can be automatically cast to and from their corresponding Cocoa types, you can use the Core Foundation property list API with Cocoa objects. In most cases, however, methods provided by the `[NSPropertyListSerialization](https://developer.apple.com/documentation/foundation/nspropertylistserialization)` class should provide enough flexibility.

## When to Use Property Lists
Many applications require a mechanism for storing information that will be needed at a later time. For situations where you need to store small amounts of persistent data—say less than a few hundred kilobytes—property lists offer a uniform and convenient means of organizing, storing, and accessing the data.

In some situations, the property-list architecture may prove insufficient. If you need a way to store large, complex graphs of objects, objects not supported by the property-list architecture, or objects whose mutability settings must be retained, use [archiving](../../../../General/Conceptual/DevPedia-CocoaCore/Archiving.html#//apple_ref/doc/uid/TP40008195-CH1). See *[Archives and Serializations Programming Guide](../../Archiving/Archiving.html#//apple_ref/doc/uid/10000047i)* for more information.

If you are looking for a way to implement user or application preferences, Cocoa provides a class specifically for this purpose. While the user defaults system does use property lists to store information, you do not have to access these plists directly. See *[Preferences and Settings Programming Guide](../../UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)* and *[Preferences Programming Topics for Core Foundation](../../../../CoreFoundation/Conceptual/CFPreferences/CFPreferences.html#//apple_ref/doc/uid/10000129i)* for more information.

Note that property lists should be used for data that consists primarily of strings and numbers. They are very inefficient when used with large blocks of binary data.

## Property List Representations
A property list can be stored in one of three different ways: in an XML representation, in a binary format, or in an “old-style” ASCII format inherited from OpenStep. You can serialize property lists in the XML and binary formats. The serialization API with the old-style format is read-only. 

XML property lists are more portable than the binary alternative and can be manually edited, but binary property lists are much more compact; as a result, they require less memory and can be read and written much faster than XML property lists. In general, if your property list is relatively small, the benefits of XML property lists outweigh the I/O speed and compactness that comes with binary property lists. If you have a large data set, binary property lists, [keyed archives](../../../../General/Conceptual/DevPedia-CocoaCore/Archiving.html#//apple_ref/doc/uid/TP40008195-CH1), or custom data formats are a better solution.

        
            [Next](../CreatePropListProgram/CreatePropListProgram.html)[Previous](../QuickStartPlist/QuickStartPlist.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-03-24