---
title: "Understanding XML Property Lists"
book: "Property List Programming Guide"
chapterId: "10000048i-CH6"
date: "2010-03-24"
description: "Explains how to use structured, textual representations of data in Cocoa."
identifier: "//apple_ref/doc/uid/10000048i"
source: apple-archive
---

# Understanding XML Property Lists

> **Property List Programming Guide**
> Last updated: 2010-03-24

[Next](../SerializePlist/SerializePlist.html)[Previous](../CreatePropListProgram/CreatePropListProgram.html)
        
        
        [](../index.html)
        
        # Understanding XML Property Lists
The preferred way to store property lists on OS X and iOS is as an XML file called an *XML property list* or *XML plist*. These files have the advantages of being human-readable and in the standards-based XML format. The `[NSArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSArray)` and `[NSDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSDictionary)` classes both have methods for saving themselves as XML plists (for example, `[descriptionWithLocale:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/instm/NSArray/descriptionWithLocale:)` and `[writeToFile:atomically:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/instm/NSDictionary/writeToFile:atomically:)`), and also have methods to convert XML property lists back to objects in memory. CFPropertyList provides functions for converting property lists to and from their XML representation.

Core Foundation supports XML as the exclusive medium for the static representation of property lists on disk. Cocoa allows property lists to be stored on disk as XML property lists, in binary form, and as “old-style” property lists. The old-style property lists can only be read, not written; see [Old-Style ASCII Property Lists](../OldStylePlists/OldStylePLists.html#//apple_ref/doc/uid/20001012-BBCBDBJE) for further information.

Generally, there is little need to create or edit XML property yourself, but if you do, use Xcode’s built-in property list editor or the Property List Editor application (which is part of the tools package). You should not edit the XML data in a text editor unless you are very familiar with XML syntax and the format of property lists. Moreover, the elements in an XML property list could change in future releases, so keep that in mind.  

Even if you don’t edit XML property lists, it is useful to understand their structure for design and debugging purposes. Like every XML file, XML plists begin with standard header information, and contain one root object, wrapped with the `<plist>` document type tag. The `<plist>` object also contains exactly one object, denoted by one of the XML elements listed in [Table 2-1](../AboutPropertyLists/AboutPropertyLists.html#//apple_ref/doc/uid/10000048i-CH3-SW1).

Graphs of objects are created by nesting the XML elements listed in Table 2-1. When encoding dictionaries, the element `<key>` is used for dictionary keys, and one of the other property list tags is used for the key’s value. Here is an example of XML data generated from a property list:

<?xml version="1.0" encoding="UTF-8"?>
```
<!DOCTYPE plist SYSTEM "file://localhost/System/Library/DTDs/PropertyList.dtd">
```

```
<plist version="1.0">
```

```
<dict>
```

```
    <key>Author</key>
```

```
    <string>William Shakespeare</string>
```

```
    <key>Lines</key>
```

```
    <array>
```

```
        <string>It is a tale told by an idiot,</string>
```

```
        <string>Full of sound and fury, signifying nothing.</string>
```

```
    </array>
```

```
    <key>Birthdate</key>
```

```
    <integer>1564</integer>
```

```
</dict>
```

```
</plist>
```
Note that data bytes are base-64 encoded between the `<data>` and `</data>` tags.

        
            [Next](../SerializePlist/SerializePlist.html)[Previous](../CreatePropListProgram/CreatePropListProgram.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-03-24