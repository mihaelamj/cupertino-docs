---
title: "Serializing a Property List"
book: "Property List Programming Guide"
chapterId: "10000048i-CH7"
date: "2010-03-24"
description: "Explains how to use structured, textual representations of data in Cocoa."
identifier: "//apple_ref/doc/uid/10000048i"
source: apple-archive
---

# Serializing a Property List

> **Property List Programming Guide**
> Last updated: 2010-03-24

[Next](../ReadWritePlistData/ReadWritePlistData.html)[Previous](../UnderstandXMLPlist/UnderstandXMLPlist.html)
        
        
        [](../index.html)
        
        # Serializing a Property List 
Using the `[NSPropertyListSerialization](https://developer.apple.com/documentation/foundation/nspropertylistserialization)` class or Property List Services (Core Foundation), you can serialize property lists in their runtime (object) form to a static representation that can be stored in the file system; later you can deserialize that static representation back into the original property-list objects. Property-list serialization automatically takes account of endianness on different processor architectures—for example, you can correctly read on an Intel-based Macintosh a binary property list created on a PowerPC-based Macintosh. 

The property-list serialization APIs allow you to save graphs of property-list objects as binary data as well as XML property lists. See [Property List Representations](../AboutPropertyLists/AboutPropertyLists.html#//apple_ref/doc/uid/20001010-46719) for the relative advantages and disadvantages of XML and binary property lists.

## Saving and Restoring a Property List in Objective-C
The `[NSPropertyListSerialization](https://developer.apple.com/documentation/foundation/nspropertylistserialization)` class (available in OS X v10.2 and later) provides methods for saving and restoring property lists from the two major supported formats, XML and binary. To save a property list in XML format, call the `[dataFromPropertyList:format:errorDescription:](https://developer.apple.com/documentation/foundation/nspropertylistserialization/1416061-datafrompropertylist)` method, specifying `[NSPropertyListXMLFormat_v1_0](https://developer.apple.com/documentation/foundation/propertylistserialization/propertylistformat/xml)` as the second parameter; to save in binary format, specify `[NSPropertyListBinaryFormat_v1_0](https://developer.apple.com/documentation/foundation/propertylistserialization/propertylistformat/binary)` instead. 

Listing 5-1 saves an object graph of property-list objects as an XML property list in the application bundle.

**Listing 5-1**  Saving a property list as an XML property list (Objective-C)

id plist;       // Assume this property list exists.
```
NSString *path = [[NSBundle mainBundle] pathForResource:@"Data" ofType:@"plist"];
```

```
NSData *xmlData;
```

```
NSString *error;
```

```
 
```

```
xmlData = [NSPropertyListSerialization dataFromPropertyList:plist
```

```
                                       format:NSPropertyListXMLFormat_v1_0
```

```
                                       errorDescription:&error];
```

```
if(xmlData) {
```

```
    NSLog(@"No error creating XML data.");
```

```
    [xmlData writeToFile:path atomically:YES];
```

```
}
```

```
else {
```

```
    NSLog(error);
```

```
    [error release];
```

```
}
```

> **Note:** To avoid a memory leak, it is necessary to release the error-description string returned via indirection in the third parameter of `[dataFromPropertyList:format:errorDescription:](https://developer.apple.com/documentation/foundation/nspropertylistserialization/1416061-datafrompropertylist)` (as shown in Listing 5-1). This is a rare exception to the standard [memory-management](../../../../General/Conceptual/DevPedia-CocoaCore/MemoryManagement.html#//apple_ref/doc/uid/TP40008195-CH27) rules.

Because you cannot save a property list in the old-style (OpenStep) format, the only valid format parameters for this method are `[NSPropertyListXMLFormat_v1_0](https://developer.apple.com/documentation/foundation/propertylistserialization/propertylistformat/xml)` and `[NSPropertyListBinaryFormat_v1_0](https://developer.apple.com/documentation/foundation/propertylistserialization/propertylistformat/binary)`. The `[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)` object returned by `dataFromPropertyList:format:errorDescription:` encapsulates the XML or binary data. You can then call the `[writeToFile:atomically:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/instm/NSData/writeToFile:atomically:)` or `[writeToURL:atomically:](https://developer.apple.com/documentation/foundation/nsdata/1415134-writetourl)` method to store the data in the file system.

> **Note:** If the root property-list object is an `[NSDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSDictionary)` or `[NSArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSArray)` object (the typical case), you can serialize the property list as an XML property list *and* write it out to disk at the same time using the appropriate methods of those classes. See [Reading and Writing Property-List Data](../ReadWritePlistData/ReadWritePlistData.html#//apple_ref/doc/uid/10000048i-CH8-SW1) for details.

To restore a property list from a data object by deserializing it, call the `[propertyListFromData:mutabilityOption:format:errorDescription:](https://developer.apple.com/documentation/foundation/propertylistserialization/1411993-propertylistfromdata)` class method of the `NSPropertyListSerialization` class, passing in the data object. Listing 5-2 creates an immutable property list from the file at `path`:

**Listing 5-2**  Restoring a property list (Objective-C)

NSString *path = [[NSBundle mainBundle] pathForResource:@"Data" ofType:@"plist"];
```
NSData *plistData = [NSData dataWithContentsOfFile:path];
```

```
NSString *error;
```

```
NSPropertyListFormat format;
```

```
id plist;
```

```
 
```

```
plist = [NSPropertyListSerialization propertyListFromData:plistData
```

```
                                mutabilityOption:NSPropertyListImmutable
```

```
                                format:&format
```

```
                                errorDescription:&error];
```

```
if(!plist){
```

```
    NSLog(error);
```

```
    [error release];
```

```
}
```
The [mutability](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectMutability.html#//apple_ref/doc/uid/TP40008195-CH42)-option parameter of the deserialization method controls whether the deserialized property-list objects are created as mutable or immutable. You can specify that all objects are immutable, that only the container objects are mutable, or that all objects are mutable. Unless you have a specific reason to mutate the collections and other objects in a deserialized property list, use the immutable option. It is faster and uses less memory.

The last two parameters of `propertyListFromData:mutabilityOption:format:errorDescription:` are by-reference. On return from a call, the format parameter holds a constant indicating the on-disk format of the property list: `[NSPropertyListXMLFormat_v1_0](https://developer.apple.com/documentation/foundation/propertylistserialization/propertylistformat/xml)`,  `[NSPropertyListBinaryFormat_v1_0](https://developer.apple.com/documentation/foundation/propertylistserialization/propertylistformat/binary)`, or `[NSPropertyListOpenStepFormat](https://developer.apple.com/documentation/foundation/propertylistserialization/propertylistformat/openstep)`. You may pass in `NULL` if you are not interested in the format.

If the method call returns `nil`, the final parameter—the error-description string—states the reason the deserialization did not succeed.

> **Note:** In OS X v10.5 (and earlier versions of the operating system), it is necessary to release the error-description string in Listing 5-2.

## Saving and Restoring a Property List in Core Foundation
Property List Services of Core Foundation has serialization functions corresponding to the class methods of `[NSPropertyListSerialization](https://developer.apple.com/documentation/foundation/nspropertylistserialization)` described in Saving and Restoring a Property List in Objective-C. To create an XML property list from a property list object, call the `[CFPropertyListCreateXMLData](https://developer.apple.com/documentation/corefoundation/1429991-cfpropertylistcreatexmldata)` function. To restore a property list object from XML data, call the `[CFPropertyListCreateFromXMLData](https://developer.apple.com/documentation/corefoundation/1429995-cfpropertylistcreatefromxmldata)` function.

Listing 5-3 shows you how to create a complex property list, convert it to XML, write it to disk, and then re-create the original data structure using the saved XML. For more information about using CFDictionary objects see *[Collections Programming Topics for Core Foundation](../../../../CoreFoundation/Conceptual/CFCollections/CFCollections.html#//apple_ref/doc/uid/10000124i)*.

**Listing 5-3**  Saving and restoring property list data (Core Foundation)

#include <CoreFoundation/CoreFoundation.h>
```
 
```

```
#define kNumKids 2
```

```
#define kNumBytesInPic 10
```

```
 
```

```
CFDictionaryRef CreateMyDictionary( void );
```

```
CFPropertyListRef CreateMyPropertyListFromFile( CFURLRef fileURL );
```

```
void WriteMyPropertyListToFile( CFPropertyListRef propertyList,
```

```
            CFURLRef fileURL );
```

```
 
```

```
int main () {
```

```
   CFPropertyListRef propertyList;
```

```
   CFURLRef fileURL;
```

```
 
```

```
   // Construct a complex dictionary object;
```

```
   propertyList = CreateMyDictionary();
```

```
 
```

```
   // Create a URL that specifies the file we will create to
```

```
   // hold the XML data.
```

```
   fileURL = CFURLCreateWithFileSystemPath( kCFAllocatorDefault,
```

```
               CFSTR("test.txt"),       // file path name
```

```
               kCFURLPOSIXPathStyle,    // interpret as POSIX path
```

```
               false );                 // is it a directory?
```

```
 
```

```
   // Write the property list to the file.
```

```
   WriteMyPropertyListToFile( propertyList, fileURL );
```

```
   CFRelease(propertyList);
```

```
 
```

```
   // Recreate the property list from the file.
```

```
   propertyList = CreateMyPropertyListFromFile( fileURL );
```

```
 
```

```
   // Release any objects to which we have references.
```

```
   CFRelease(propertyList);
```

```
   CFRelease(fileURL);
```

```
   return 0;
```

```
}
```

```
 
```

```
CFDictionaryRef CreateMyDictionary( void ) {
```

```
   CFMutableDictionaryRef dict;
```

```
   CFNumberRef            num;
```

```
   CFArrayRef             array;
```

```
   CFDataRef              data;
```

```
 
```

```
   int                    yearOfBirth;
```

```
   CFStringRef            kidsNames[kNumKids];
```

```
 
```

```
   // Fake data to stand in for a picture of John Doe.
```

```
   const unsigned char pic[kNumBytesInPic] = {0x3c, 0x42, 0x81,
```

```
            0xa5, 0x81, 0xa5, 0x99, 0x81, 0x42, 0x3c};
```

```
 
```

```
   // Define some data.
```

```
   kidsNames[0] = CFSTR("John");
```

```
   kidsNames[1] = CFSTR("Kyra");
```

```
 
```

```
   yearOfBirth = 1965;
```

```
 
```

```
   // Create a dictionary that will hold the data.
```

```
   dict = CFDictionaryCreateMutable( kCFAllocatorDefault,
```

```
            0,
```

```
            &kCFTypeDictionaryKeyCallBacks,
```

```
            &kCFTypeDictionaryValueCallBacks );
```

```
 
```

```
   // Put the various items into the dictionary.
```

```
   // Because the values are retained as they are placed into the
```

```
   //  dictionary, we can release any allocated objects here.
```

```
 
```

```
   CFDictionarySetValue( dict, CFSTR("Name"), CFSTR("John Doe") );
```

```
 
```

```
   CFDictionarySetValue( dict,
```

```
            CFSTR("City of Birth"),
```

```
            CFSTR("Springfield") );
```

```
 
```

```
   num = CFNumberCreate( kCFAllocatorDefault,
```

```
            kCFNumberIntType,
```

```
            &yearOfBirth );
```

```
   CFDictionarySetValue( dict, CFSTR("Year Of Birth"), num );
```

```
   CFRelease( num );
```

```
 
```

```
   array = CFArrayCreate( kCFAllocatorDefault,
```

```
               (const void **)kidsNames,
```

```
               kNumKids,
```

```
               &kCFTypeArrayCallBacks );
```

```
   CFDictionarySetValue( dict, CFSTR("Kids Names"), array );
```

```
   CFRelease( array );
```

```
 
```

```
   array = CFArrayCreate( kCFAllocatorDefault,
```

```
               NULL,
```

```
               0,
```

```
               &kCFTypeArrayCallBacks );
```

```
   CFDictionarySetValue( dict, CFSTR("Pets Names"), array );
```

```
   CFRelease( array );
```

```
 
```

```
   data = CFDataCreate( kCFAllocatorDefault, pic, kNumBytesInPic );
```

```
   CFDictionarySetValue( dict, CFSTR("Picture"), data );
```

```
   CFRelease( data );
```

```
 
```

```
   return dict;
```

```
}
```

```
 
```

```
void WriteMyPropertyListToFile( CFPropertyListRef propertyList,
```

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
    } else {
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
For a discussion of the mutability-option parameter of `CFPropertyListCreateFromXMLData` see the discussion of the corresponding parameter of the `[propertyListFromData:mutabilityOption:format:errorDescription:](https://developer.apple.com/documentation/foundation/propertylistserialization/1411993-propertylistfromdata)` method in [Saving and Restoring a Property List in Objective-C](#//apple_ref/doc/uid/10000048i-CH7-SW4).

Listing 5-4 shows how the contents of `xmlData`, created in [Listing 5-1](#//apple_ref/doc/uid/10000048i-CH7-SW3), would look if printed to the screen.

**Listing 5-4**  XML file contents created by the sample program

```
<?xml version="1.0" encoding="UTF-8"?>
```

```
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN"
```

```
        "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
```

```
<plist version="1.0">
```

```
<dict>
```

```
    <key>Year Of Birth</key>
```

```
    <integer>1965</integer>
```

```
    <key>Pets Names</key>
```

```
    <array/>
```

```
    <key>Picture</key>
```

```
    <data>
```

```
        PEKBpYGlmYFCPA==
```

```
    </data>
```

```
    <key>City of Birth</key>
```

```
    <string>Springfield</string>
```

```
    <key>Name</key>
```

```
    <string>John Doe</string>
```

```
    <key>Kids Names</key>
```

```
    <array>
```

```
        <string>John</string>
```

```
        <string>Kyra</string>
```

```
    </array>
```

```
</dict>
```

```
</plist>
```

```
 
```
## Using Property List Services with Cocoa
Cocoa uses the Core Foundation property list API to read and write XML property lists. In some cases, you may wish to access the API directly in an Objective-C Cocoa application. For example, if you want to save an instance of a class other than `NSArray` or `NSDictionary` as the root object of an XML plist, currently the easiest way to do this is through Property List Services. This process is made simple because Cocoa objects can be cast to and from corresponding Core Foundation types. This conversion between Cocoa and Core Foundation object types is known as *toll-free bridging*.

To create an XML property list from a property list object, call the `[CFPropertyListCreateXMLData](https://developer.apple.com/documentation/corefoundation/1429991-cfpropertylistcreatexmldata)` function. This code fragment saves the property list `plist` into a file at `path`:

NSString *path = [NSString stringWithFormat:@"%@/MyData.plist", NSTemporaryDirectory()];
```
id plist;       // Assume this is a valid property list.
```

```
NSData *xmlData;
```

```
 
```

```
xmlData = (NSData *)CFPropertyListCreateXMLData(kCFAllocatorDefault,
```

```
                                               (CFPropertyListRef)plist);
```

```
[xmlData writeToFile:path atomically:YES];
```

```
[xmlData release];
```
To restore a property list object from XML data, call the `[CFPropertyListCreateFromXMLData](https://developer.apple.com/documentation/corefoundation/1429995-cfpropertylistcreatefromxmldata)` function. This code fragment restores property list from the XML plist file at `path` with mutable containers but immutable leaves:

```
NSString *path = [NSString stringWithFormat:@"%@/MyData.plist", NSTemporaryDirectory()];
```

```
NSString *errorString;
```

```
NSData *xmlData;
```

```
id plist;
```

```
 
```

```
xmlData = [NSData dataWithContentsOfFile:path];
```

```
plist = (id)CFPropertyListCreateFromXMLData(kCFAllocatorDefault,
```

```
                 (CFDataRef)xmlData, kCFPropertyListMutableContainers,
```

```
                 (CFStringRef *)&errorString);
```

        
            [Next](../ReadWritePlistData/ReadWritePlistData.html)[Previous](../UnderstandXMLPlist/UnderstandXMLPlist.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-03-24