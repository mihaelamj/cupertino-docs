---
title: "Creating Property Lists Programmatically"
book: "Property List Programming Guide"
chapterId: "10000048i-CH5"
date: "2010-03-24"
description: "Explains how to use structured, textual representations of data in Cocoa."
identifier: "//apple_ref/doc/uid/10000048i"
source: apple-archive
---

# Creating Property Lists Programmatically

> **Property List Programming Guide**
> Last updated: 2010-03-24

[Next](../UnderstandXMLPlist/UnderstandXMLPlist.html)[Previous](../AboutPropertyLists/AboutPropertyLists.html)
        
        
        [](../index.html)
        
        # Creating Property Lists Programmatically
You can create a graph of property-list objects by nesting property-list objects of various types in arrays and dictionaries. The following sections give details on doing this programmatically. 

## Creating a Property List in Objective-C
You can create a property list in Objective-C if all of the objects in the aggregate derive from the `[NSDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSDictionary)`, `[NSArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSArray)`, `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)`, `[NSDate](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateClassCluster/Description.html#//apple_ref/occ/cl/NSDate)`, `[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)`, or `[NSNumber](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumber/Description.html#//apple_ref/occ/cl/NSNumber)` class. The code in Listing 3-1 creates a property list consisting of an `NSDictionary` object (the root object) that contains two dictionaries, each containing a string, a date, and an array of numbers.

**Listing 3-1**  Creating a property list programmatically (Objective-C)

        NSMutableDictionary *rootObj = [NSMutableDictionary dictionaryWithCapacity:2];
```
        NSDictionary *innerDict;
```

```
        NSString *name;
```

```
        NSDate *dob;
```

```
        NSArray *scores;
```

```
 
```

```
        scores = [NSArray arrayWithObjects:[NSNumber numberWithInt:6],
```

```
            [NSNumber numberWithFloat:4.6], [NSNumber numberWithLong:6.0000034], nil];
```

```
        name = @"George Washington";
```

```
        dob = [NSDate dateWithString:@"1732-02-17 04:32:00 +0300"];
```

```
        innerDict = [NSDictionary dictionaryWithObjects:
```

```
            [NSArray arrayWithObjects: name, dob, scores, nil]
```

```
            forKeys:[NSArray arrayWithObjects:@"Name", @"DOB", @"Scores"]];
```

```
        [rootObj setObject:innerDict forKey:@"Washington"];
```

```
 
```

```
        scores = [NSArray arrayWithObjects:[NSNumber numberWithInt:8],
```

```
            [NSNumber numberWithFloat:4.9],
```

```
            [NSNumber numberWithLong:9.003433], nil];
```

```
        name = @"Abraham Lincoln";
```

```
        dob = [NSDate dateWithString:@"1809-02-12 13:18:00 +0400"];
```

```
        innerDict = [NSDictionary dictionaryWithObjects:
```

```
            [NSArray arrayWithObjects: name, dob, scores, nil]
```

```
            forKeys:[NSArray arrayWithObjects:@"Name", @"DOB", @"Scores"]];
```

```
        [rootObj setObject:innerDict forKey:@"Lincoln"];
```

```
 
```

```
        id plist = [NSPropertyListSerialization dataFromPropertyList:(id)rootObj
```

```
            format:NSPropertyListXMLFormat_v1_0 errorDescription:&error];
```

> **Note:** The `[NSPropertyListSerialization](https://developer.apple.com/documentation/foundation/nspropertylistserialization)` class method shown in this example serializes the property-list objects into an XML property list. It is described more fully in [Serializing a Property List ](../SerializePlist/SerializePlist.html#//apple_ref/doc/uid/10000048i-CH7-SW1).

The XML output of the code in Listing 3-1 is shown in Listing 3-2.

**Listing 3-2**  XML property list produced as output

```
<?xml version="1.0" encoding="UTF-8"?>
```

```
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
```

```
<plist version="1.0">
```

```
<dict>
```

```
    <key>Lincoln</key>
```

```
    <dict>
```

```
        <key>DOB</key>
```

```
        <date>1809-02-12T09:18:00Z</date>
```

```
        <key>Name</key>
```

```
        <string>Abraham Lincoln</string>
```

```
        <key>Scores</key>
```

```
        <array>
```

```
            <integer>8</integer>
```

```
            <real>4.9000000953674316</real>
```

```
            <integer>9</integer>
```

```
        </array>
```

```
    </dict>
```

```
    <key>Washington</key>
```

```
    <dict>
```

```
        <key>DOB</key>
```

```
        <date>1732-02-17T01:32:00Z</date>
```

```
        <key>Name</key>
```

```
        <string>George Washington</string>
```

```
        <key>Scores</key>
```

```
        <array>
```

```
            <integer>6</integer>
```

```
            <real>4.5999999046325684</real>
```

```
            <integer>6</integer>
```

```
        </array>
```

```
    </dict>
```

```
</dict>
```

```
</plist>
```
## Creating a Property List in Core Foundation
The examples in this section demonstrate how to create and work with property lists using Core Foundation functions. The error checking code has been removed for clarity. In practice, it is *vital* that you check for errors because passing bad parameters into Core Foundation routines can cause your application to crash.

Listing 3-3 shows you how to create a very simple property list—an array of CFString objects.

**Listing 3-3**  Creating a simple property list from an array

#include <CoreFoundation/CoreFoundation.h>
```
#define kNumFamilyMembers 5
```

```
 
```

```
void main () {
```

```
    CFStringRef names[kNumFamilyMembers];
```

```
    CFArrayRef  array;
```

```
    CFDataRef   xmlData;
```

```
 
```

```
    // Define the family members.
```

```
    names[0] = CFSTR("Marge");
```

```
    names[1] = CFSTR("Homer");
```

```
    names[2] = CFSTR("Bart");
```

```
    names[3] = CFSTR("Lisa");
```

```
    names[4] = CFSTR("Maggie");
```

```
 
```

```
    // Create a property list using the string array of names.
```

```
    array = CFArrayCreate( kCFAllocatorDefault,
```

```
                (const void **)names,
```

```
                kNumFamilyMembers,
```

```
                &kCFTypeArrayCallBacks );
```

```
 
```

```
    // Convert the plist into XML data.
```

```
    xmlData = CFPropertyListCreateXMLData( kCFAllocatorDefault, array );
```

```
 
```

```
    // Clean up CF types.
```

```
    CFRelease( array );
```

```
    CFRelease( xmlData );
```

```
}
```

> **Note:** The `[CFPropertyListCreateXMLData](https://developer.apple.com/documentation/corefoundation/1429991-cfpropertylistcreatexmldata)` function is discussed in [Serializing a Property List ](../SerializePlist/SerializePlist.html#//apple_ref/doc/uid/10000048i-CH7-SW1).

Listing 3-4 shows how the contents of `xmlData`, created in Listing 3-3, would look if printed to the screen.

**Listing 3-4**  XML created by the sample program

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
<array>
```

```
    <string>Marge</string>
```

```
    <string>Homer</string>
```

```
    <string>Bart</string>
```

```
    <string>Lisa</string>
```

```
    <string>Maggie</string>
```

```
</array>
```

```
</plist>
```

```
 
```
### About Numbers and Property Lists in Core Foundation
You cannot use C numeric data values directly in Core Foundation property lists. Core Foundation provides the function  `[CFNumberCreate](https://developer.apple.com/documentation/corefoundation/1542182-cfnumbercreate)` to convert C numerical values into CFNumber objects, the form that is required to use numbers in property lists.

A CFNumber object serves simply as a wrapper for C numeric values. Core Foundation includes functions to create a CFNumber, obtain its value, and compare two CFNumber objects. Note that CFNumber objects are immutable with respect to value, but type information may not be maintained. You can get information about a CFNumber object’s type, but this is the type the CFNumber object used to store your value and *may not* be the same type as the original C data.

When comparing CFNumber objects, conversion and comparison follow human expectations and not C promotion and comparison rules. Negative zero compares less than positive zero. Positive infinity compares greater than everything except itself, to which it compares equal. Negative infinity compares less than everything except itself, to which it compares equal. Unlike standard practice, if both numbers are NaNs, then they compare equal; if only one of the numbers is a NaN, then the NaN compares greater than the other number if it is negative, and smaller than the other number if it is positive.

Listing 3-5 shows how to create a CFNumber object from a 16-bit integer and then get information about the CFNumber object.

**Listing 3-5**  Creating a CFNumber object from an integer

Int16               sint16val = 276;
```
CFNumberRef         aCFNumber;
```

```
CFNumberType        type;
```

```
Int32               size;
```

```
Boolean             status;
```

```
 
```

```
// Make a CFNumber from a 16-bit integer.
```

```
aCFNumber = CFNumberCreate(kCFAllocatorDefault,
```

```
                           kCFNumberSInt16Type,
```

```
                           &sint16val);
```

```
 
```

```
// Find out what type is being used by this CFNumber.
```

```
type = CFNumberGetType(aCFNumber);
```

```
 
```

```
// Now find out the size in bytes.
```

```
size = CFNumberGetByteSize(aCFNumber);
```

```
 
```

```
// Get the value back from the CFNumber.
```

```
status = CFNumberGetValue(aCFNumber,
```

```
                          kCFNumberSInt16Type,
```

```
                          &sint16val);
```
Listing 3-6 creates another CFNumber object and compares it with the one created in Listing 3-5.

**Listing 3-6**  Comparing two CFNumber objects

```
CFNumberRef         anotherCFNumber;
```

```
CFComparisonResult  result;
```

```
 
```

```
// Make a new CFNumber.
```

```
sint16val = 382;
```

```
anotherCFNumber = CFNumberCreate(kCFAllocatorDefault,
```

```
                        kCFNumberSInt16Type,
```

```
                        &sint16val);
```

```
 
```

```
// Compare two CFNumber objects.
```

```
result = CFNumberCompare(aCFNumber, anotherCFNumber, NULL);
```

```
 
```

```
switch (result) {
```

```
    case kCFCompareLessThan:
```

```
        printf("aCFNumber is less than anotherCFNumber.\n");
```

```
        break;
```

```
    case kCFCompareEqualTo:
```

```
        printf("aCFNumber is equal to anotherCFNumber.\n");
```

```
        break;
```

```
    case kCFCompareGreaterThan:
```

```
        printf("aCFNumber is greater than anotherCFNumber.\n");
```

```
        break;
```

```
}
```

        
            [Next](../UnderstandXMLPlist/UnderstandXMLPlist.html)[Previous](../AboutPropertyLists/AboutPropertyLists.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-03-24