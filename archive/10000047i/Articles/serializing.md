---
title: "Serializing Property Lists"
book: "Archives and Serializations Programming Guide"
chapterId: "20000952"
date: "2012-07-17"
description: "Explains how to put Cocoa objects into and remove them from a representation suitable for archiving."
identifier: "//apple_ref/doc/uid/10000047i"
source: apple-archive
---

# Serializing Property Lists

> **Archives and Serializations Programming Guide**
> Last updated: 2012-07-17

[Next](../RevisionHistory.html)[Previous](subclassing.html)
        
        
        [](../index.html)
        
        # Serializing Property Lists
Serialization converts Objective-C types to and from an architecture-independent byte stream. In contrast to archiving, basic serialization does not record the data type of the values nor the relationships between them; only the values themselves are recorded. It is your responsibility to deserialize the data in the proper order.

Property list serialization does not preserve the full class identity of the objects, only its general kind—a dictionary, an array, and so on. As a result, if a property list is serialized and then deserialized, the objects in the resulting property list might not be of the same class as the objects in the original property list. In particular, when a property list is serialized, the mutability of the container objects (`NSDictionary` and `NSArray` objects) is not preserved. When deserializing, though, you can choose to have all container objects created mutable or immutable.

Serialization also does not track the presence of objects referenced multiple times. Each reference to an object within the property list is serialized separately, resulting in multiple instances when deserialized.

Because serialization does not preserve class information or mutability, nor handles multiple references, coding (as implemented by `NSCoder` and its subclasses) is the preferred way to make object graphs persistent.

The `NSPropertyListSerialization` class provides the serialization methods that convert [property list](../../../../General/Conceptual/DevPedia-CocoaCore/PropertyList.html#//apple_ref/doc/uid/TP40008195-CH44) objects to and from either an XML or an optimized binary format. The `NSPropertyListSerialization` class object provides the interface to the serialization process; you don’t create instances of `NSPropertyListSerialization`.

The following code sample shows how to serialize a simple property list into an XML format.

NSDictionary *propertyList= @{ @"FirstNameKey" : @"Edmund",
```
                               @"LastNameKey" : @"Blackadder" };
```

```
NSString *errorStr;
```

```
NSData *dataRep = [NSPropertyListSerialization dataFromPropertyList:propertyList
```

```
                format:NSPropertyListXMLFormat_v1_0
```

```
                errorDescription:&errorStr];
```

```
if (!dataRep) {
```

```
    // Handle error
```

```
}
```
The following code sample converts the XML data from above back into an object graph.

```
NSDictionary *propertyList = [NSPropertyListSerialization propertyListFromData:dataRep
```

```
                mutabilityOption:NSPropertyListImmutable
```

```
                format:NULL
```

```
                errorDescription:&errorStr];
```

```
if (!propertyList) {
```

```
    // Handle error
```

```
}
```

        
            [Next](../RevisionHistory.html)[Previous](subclassing.html)
        
         Copyright &#x00a9; 2002, 2012 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2012-07-17