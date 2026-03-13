---
title: "Creating and Extracting Archives"
book: "Archives and Serializations Programming Guide"
chapterId: "20000949"
date: "2012-07-17"
description: "Explains how to put Cocoa objects into and remove them from a representation suitable for archiving."
identifier: "//apple_ref/doc/uid/10000047i"
source: apple-archive
---

# Creating and Extracting Archives

> **Archives and Serializations Programming Guide**
> Last updated: 2012-07-17

[Next](codingobjects.html)[Previous](archives.html)
        
        
        [](../index.html)
        
        # Creating and Extracting Archives
This chapter describes how to create and extract an archive. To understand how to make your custom objects support archival, see [Encoding and Decoding Objects](codingobjects.html#//apple_ref/doc/uid/20000948-BCIHBJDE).

## Creating an Archive

The easiest way to create an archive of an object graph is to invoke a single [class method](../../../../General/Conceptual/DevPedia-CocoaCore/ClassMethod.html#//apple_ref/doc/uid/TP40008195-CH8)—either `archiveRootObject:toFile:` or `archivedDataWithRootObject:`—on the archiver class. These convenience methods create a temporary archiver object that encodes a single object graph; you need do no more. The following code fragment, for example, archives a custom object called *aPerson* directly to a file.

```
Person *aPerson = <#Get a Person#>;
```

```
NSString *archivePath = <Path for the archive#>;
```

```
BOOL success = [NSKeyedArchiver archiveRootObject:aPerson toFile:archivePath];
```

However, if you want to customize the archiving process (for example, by substituting certain classes for others), you must instead create an instance of the archiver yourself, configure it as desired, and send it an `encode` message explicitly. `NSCoder` itself defines no particular method for creating a coder; this typically varies with the subclass. `NSKeyedArchiver` defines `[initForWritingWithMutableData:](https://developer.apple.com/documentation/foundation/nskeyedarchiver/1409579-initforwritingwithmutabledata)`.

Once you have the configured coder object, to encode an object or data item, use any `encode` method for an `NSKeyedArchiver` coder. When finished encoding, you must invoke `finishEncoding` before accessing the archive data. The following sample code fragment archives a custom object called *aPerson* similar to the above code, but allows for customization.

```
Person *aPerson = <#Get a Person#>;
```

```
NSString *archivePath = <Path for the archive#>;
```

```
NSMutableData *data = [NSMutableData data];
```

```
NSKeyedArchiver *archiver = [[NSKeyedArchiver alloc] initForWritingWithMutableData:data];
```

```
[archiver encodeObject:aPerson forKey:ASCPersonKey];
```

```
[archiver finishEncoding];
```

```
 
```

```
NSURL *archiveURL = <URL for the archive#>;
```

```
BOOL result = [data writeToURL:archiveURL atomically:YES];
```

It is possible to create an archive that does not contain any objects. To archive other data types, invoke one of the type-specific methods, such as `[encodeInteger:forKey:](https://developer.apple.com/documentation/foundation/nscoder/1411551-encodeinteger)` or `encodeDouble:forKey:` directly for each data item to be archived, instead of using `encodeRootObject:`. 

## Decoding an Archive
The easiest way to decode an archive of an object is to invoke a single [class method](../../../../General/Conceptual/DevPedia-CocoaCore/ClassMethod.html#//apple_ref/doc/uid/TP40008195-CH8)—either `[unarchiveObjectWithFile:](https://developer.apple.com/documentation/foundation/nskeyedunarchiver/1417153-unarchiveobject)` or `[unarchiveObjectWithData:](https://developer.apple.com/documentation/foundation/nskeyedunarchiver/1413894-unarchiveobjectwithdata)`—on the unarchiver class. These convenience methods create a temporary unarchiver object that decodes and returns a single object graph; you need do no more. `NSKeyedUnarchiver` requires that the object graph in the archive was encoded with one of `NSKeyedArchiver`’s convenience class methods, such as `archiveRootObject:toFile:`. The following code fragment, for example, unarchives a custom object called *aPerson* directly from a file.

NSString *archivePath = <Path for the archive#>;
```
Person *aPerson = [NSKeyedUnarchiver unarchiveObjectWithFile:archivePath];
```
However, if you want to customize the unarchiving process (for example, by substituting certain classes for others), you typically create an instance of the unarchiver class yourself, configure it as desired, and send it a `decode`message explicitly. `NSCoder` itself defines no particular method for creating a coder; this typically varies with the subclass. `NSKeyedUnarchiver` defines `[initForReadingWithData:](https://developer.apple.com/documentation/foundation/nskeyedunarchiver/1410862-initforreadingwithdata)`.

Once you have the configured decoder object, to decode an object or data item, use the `decodeObjectForKey:` method. When finished decoding a keyed archive, you should invoke `finishDecoding` before releasing the unarchiver. The following sample code fragment unarchives a custom object called *myMap* similar to the above code sample, but allows for customization.

NSURL *archiveURL = <URL for the archive#>;
```
NSData *data = [NSData dataWithContentsOfURL:archiveURL];
```

```
 
```

```
NSKeyedUnarchiver *unarchiver = [[NSKeyedUnarchiver alloc] initForReadingWithData:data];
```

```
// Customize the unarchiver.
```

```
Person *aPerson = [unarchiver decodeObjectForKey:ASCPersonKey];
```

```
[unarchiver finishDecoding];
```
You can create an archive that does not contain any objects. To unarchive non-object data types, simply use the `decode...` method (such as `decodeIntForKey:` or `decodeDoubleForKey:`) corresponding to the original `encode...` method for each data item to be unarchived. 

To customize the unarchiving process for an archive previously created using `archiveRootObject:toFile:`, use `decodeObjectForKey:` and the `NSKeyedArchiveRootObjectKey` key to identify the root object in the archive:

```
id object = [unarchiver decodeObjectForKey:NSKeyedArchiveRootObjectKey];
```

        
            [Next](codingobjects.html)[Previous](archives.html)
        
         Copyright &#x00a9; 2002, 2012 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2012-07-17