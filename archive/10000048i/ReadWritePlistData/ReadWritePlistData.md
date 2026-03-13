---
title: "Reading and Writing Property-List Data"
book: "Property List Programming Guide"
chapterId: "10000048i-CH8"
date: "2010-03-24"
description: "Explains how to use structured, textual representations of data in Cocoa."
identifier: "//apple_ref/doc/uid/10000048i"
source: apple-archive
---

# Reading and Writing Property-List Data

> **Property List Programming Guide**
> Last updated: 2010-03-24

[Next](../OldStylePlists/OldStylePLists.html)[Previous](../SerializePlist/SerializePlist.html)
        
        
        [](../index.html)
        
        # Reading and Writing Property-List Data
## Using Objective-C Methods to Read and Write Property-List Data
You have two major ways to write property-list data to the file system:

If the root object of the property list is an `[NSDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSDictionary)` or `[NSArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSArray)` object—which is almost always the case—you can invoke the `writeToFile:atomically:` or `writeToURL:atomically:` methods of those classes, passing in the root object. These methods save the graph of property-list objects as an XML property list before writing that out as a file or URL resource.

To read the property-list data back into your program, [initialize](../../../../General/Conceptual/DevPedia-CocoaCore/Initialization.html#//apple_ref/doc/uid/TP40008195-CH21) an allocated collection object by calling the `initWithContentsOfFile:` and `initWithContentsOfURL:` methods or the corresponding class [factory methods](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39) (for example, `[dictionaryWithContentsOfURL:](https://developer.apple.com/documentation/foundation/nsdictionary/1574185-dictionarywithcontentsofurl)`).

> **Note:** To function properly, these methods require that *all* objects contained by the `NSDictionary` or `NSArray` root object be property-list objects.

You can serialize the property-list objects to an `[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)` object using the `[dataFromPropertyList:format:errorDescription:](https://developer.apple.com/documentation/foundation/nspropertylistserialization/1416061-datafrompropertylist)` [class method](../../../../General/Conceptual/DevPedia-CocoaCore/ClassMethod.html#//apple_ref/doc/uid/TP40008195-CH8) and then save that object by calling the `[writeToFile:atomically:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/instm/NSData/writeToFile:atomically:)` or `[writeToURL:atomically:](https://developer.apple.com/documentation/foundation/nsdata/1415134-writetourl)` methods of the `NSData` class.

To read the property-list data back into your program, first initialize an allocated `NSData` object by invoking `[initWithContentsOfFile:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/instm/NSData/initWithContentsOfFile:)` or `[initWithContentsOfURL:](https://developer.apple.com/documentation/foundation/nsdata/1413892-init)` or call a corresponding class factory method such as `[dataWithContentsOfFile:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/clm/NSData/dataWithContentsOfFile:)`. Then call the `[propertyListFromData:mutabilityOption:format:errorDescription:](https://developer.apple.com/documentation/foundation/propertylistserialization/1411993-propertylistfromdata)` class method of `NSPropertyListSerialization`, passing in the data object.

> **Note:** The code examples in [Read in the Property List](../QuickStartPlist/QuickStartPlist.html#//apple_ref/doc/uid/10000048i-CH4-SW6) and [Write Out the Property List](../QuickStartPlist/QuickStartPlist.html#//apple_ref/doc/uid/10000048i-CH4-SW7) of Quick Start for Property Lists illustrate the second approach.

The first approach is simpler—it requires only one method invocation instead of two—but the second approach has its advantages. It allows you to convert the runtime property list to binary format as well as an XML property list. When you convert a static representation of a property list back into a graph of objects, it also lets you specify with more flexibility whether those objects are [mutable or immutable](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectMutability.html#//apple_ref/doc/uid/TP40008195-CH42). 

To expand on this last point, consider this example. You have an XML property list whose root object is an `[NSArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSArray)` object containing a number of `[NSDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSDictionary)` objects. If you load that property list with this call:

NSArray * a = [NSArray arrayWithContentsOfFile:xmlFile];`a` is an immutable array with immutable dictionaries in each element. Each key and each value in each dictionary are also immutable.

If you load the property list with this call:

NSMutableArray * ma = [NSMutableArray arrayWithContentsOfFile:xmlFile];`ma` is a mutable array with immutable dictionaries in each element. Each key and each value in each dictionary are immutable.

If you need finer-grained control over the mutability of the objects in a property list, use the `[propertyListFromData:mutabilityOption:format:errorDescription:](https://developer.apple.com/documentation/foundation/propertylistserialization/1411993-propertylistfromdata)` class method, whose second parameter permits you to specify the mutability of objects at various levels of the aggregate property list. You could specify that all objects are immutable (`[NSPropertyListImmutable](https://developer.apple.com/documentation/foundation/nspropertylistmutabilityoptions/nspropertylistimmutable)`), that only the container (array and dictionary) objects are mutable (`[NSPropertyListMutableContainers](https://developer.apple.com/documentation/foundation/nspropertylistmutabilityoptions/nspropertylistmutablecontainers)`), or that all objects are mutable (`[NSPropertyListMutableContainersAndLeaves](https://developer.apple.com/documentation/foundation/propertylistserialization/mutabilityoptions/1411313-mutablecontainersandleaves)`).

For example, you could write code like this:

NSMutableArray *dma = (NSMutableArray *)[NSPropertyListSerialization
```
                        propertyListFromData:plistData
```

```
                        mutabilityOption:NSPropertyListMutableContainersAndLeaves
```

```
                        format:&format
```

```
                        errorDescription:&error];
```
This call produces a mutable array with mutable dictionaries in each element. Each key and each value in each dictionary are themselves also mutable.

## Using Core Foundation Functions to Read and Write Property-List Data
To write out XML property lists using Property List Services (Core Foundation), call the function the `[CFURLWriteDataAndPropertiesToResource](https://developer.apple.com/documentation/corefoundation/1420723-cfurlwritedataandpropertiestores)` function, passing the CFData object created through calling `[CFPropertyListCreateXMLData](https://developer.apple.com/documentation/corefoundation/1429991-cfpropertylistcreatexmldata)`. To read an XML property list from the file system or URL resource, call the function `[CFURLCreateDataAndPropertiesFromResource](https://developer.apple.com/documentation/corefoundation/1420742-cfurlcreatedataandpropertiesfrom)`. Then convert the created CFData object to a graph of property-list objects by calling the `[CFPropertyListCreateFromXMLData](https://developer.apple.com/documentation/corefoundation/1429995-cfpropertylistcreatefromxmldata)` function.

Listing 6-1 includes a fragment of the larger code example in [Saving and Restoring a Property List in Core Foundation](../SerializePlist/SerializePlist.html#//apple_ref/doc/uid/10000048i-CH7-SW5) that illustrates the use of these functions. 

**Listing 6-1**  Writing and reading property lists using Core Foundation functions

void WriteMyPropertyListToFile( CFPropertyListRef propertyList,
```
            CFURLRef fileURL ) {
```

```
   CFDataRef xmlData;
```

```
   Boolean status;
```

```
   SInt32 errorCode;
```

```
 
```

```
   // Convert the property list into XML data.
```

```
   xmlData = CFPropertyListCreateXMLData( kCFAllocatorDefault, propertyList );
```

```
 
```

```
   // Write the XML data to the file.
```

```
   status = CFURLWriteDataAndPropertiesToResource (
```

```
               fileURL,                  // URL to use
```

```
               xmlData,                  // data to write
```

```
               NULL,
```

```
               &errorCode);
```

```
 
```

```
   CFRelease(xmlData);
```

```
}
```

```
 
```

```
CFPropertyListRef CreateMyPropertyListFromFile( CFURLRef fileURL ) {
```

```
   CFPropertyListRef propertyList;
```

```
   CFStringRef       errorString;
```

```
   CFDataRef         resourceData;
```

```
   Boolean           status;
```

```
   SInt32            errorCode;
```

```
 
```

```
   // Read the XML file.
```

```
   status = CFURLCreateDataAndPropertiesFromResource(
```

```
               kCFAllocatorDefault,
```

```
               fileURL,
```

```
               &resourceData,            // place to put file data
```

```
               NULL,
```

```
               NULL,
```

```
               &errorCode);
```

```
 
```

```
   // Reconstitute the dictionary using the XML data.
```

```
   propertyList = CFPropertyListCreateFromXMLData( kCFAllocatorDefault,
```

```
               resourceData,
```

```
               kCFPropertyListImmutable,
```

```
               &errorString);
```

```
 
```

```
   if (resourceData) {
```

```
        CFRelease( resourceData );
```

```
    else {
```

```
        CFRelease( errorString );
```

```
    }
```

```
   return propertyList;
```

```
}
```
You may also write and read property lists to the file system using the functions `[CFPropertyListWriteToStream](https://developer.apple.com/documentation/corefoundation/1430031-cfpropertylistwritetostream)` and `[CFPropertyListCreateFromStream](https://developer.apple.com/documentation/corefoundation/1429993-cfpropertylistcreatefromstream)`. These functions require that you open and configure the read and write streams yourself.

        
            [Next](../OldStylePlists/OldStylePLists.html)[Previous](../SerializePlist/SerializePlist.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-03-24