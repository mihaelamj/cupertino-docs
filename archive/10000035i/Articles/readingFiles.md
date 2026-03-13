---
title: "Reading Strings From and Writing Strings To Files and URLs"
book: "String Programming Guide"
chapterId: "TP40003459"
date: "2014-02-11"
description: "Explains how to create, search, concatenate, and draw strings in Cocoa."
identifier: "//apple_ref/doc/uid/10000035i"
source: apple-archive
---

# Reading Strings From and Writing Strings To Files and URLs

> **String Programming Guide**
> Last updated: 2014-02-11

[Next](SearchingStrings.html)[Previous](formatSpecifiers.html)
        
        
        [](../index.html)
        
        # Reading Strings From and Writing Strings To Files and URLs
Reading files or URLs using `NSString` is straightforward provided that you know what encoding the resource uses—if you don't know the encoding, reading a resource is more challenging. When you write to a file or URL, you must specify the encoding to use. (Where possible, you should use URLs because these are more efficient.) 

## Reading From Files and URLs
`NSString` provides a variety of methods to read data from files and URLs. In general, it is much easier to read data if you know its encoding. If you have plain text and no knowledge of the encoding, you are already in a difficult position. You should avoid placing yourself in this position if at all possible—anything that calls for the use of plain text files should specify the encoding (preferably UTF-8 or UTF-16+BOM).

### Reading data with a known encoding
To read from a file or URL for which you know the encoding, you use `[stringWithContentsOfFile:encoding:error:](https://developer.apple.com/documentation/foundation/nsstring/1497327-stringwithcontentsoffile)` or `[stringWithContentsOfURL:encoding:error:](https://developer.apple.com/documentation/foundation/nsstring/1497360-stringwithcontentsofurl)`, or the corresponding `init...` method, as illustrated in the following example.

NSURL *URL = ...;
```
NSError *error;
```

```
NSString *stringFromFileAtURL = [[NSString alloc]
```

```
                                      initWithContentsOfURL:URL
```

```
                                      encoding:NSUTF8StringEncoding
```

```
                                      error:&error];
```

```
if (stringFromFileAtURL == nil) {
```

```
    // an error occurred
```

```
    NSLog(@"Error reading file at %@\n%@",
```

```
              URL, [error localizedFailureReason]);
```

```
    // implementation continues ...
```
You can also [initialize](../../../../General/Conceptual/DevPedia-CocoaCore/Initialization.html#//apple_ref/doc/uid/TP40008195-CH21) a string using a data object, as illustrated in the following examples. Again, you must specify the correct encoding.

```
NSURL *URL = ...;
```

```
NSData *data = [NSData dataWithContentsOfURL:URL];
```

```
 
```

```
// Assuming data is in UTF8.
```

```
NSString *string = [NSString stringWithUTF8String:[data bytes]];
```

```
 
```

```
// if data is in another encoding, for example ISO-8859-1
```

```
NSString *string = [[NSString alloc]
```

```
            initWithData:data encoding: NSISOLatin1StringEncoding];
```
### Reading data with an unknown encoding
If you find yourself with text of unknown encoding, it is best to make sure that there is a mechanism for correcting the inevitable errors. For example, Apple's Mail and Safari applications have encoding menus, and TextEdit allows the user to reopen the file with an explicitly specified encoding. 

If you are forced to guess the encoding (and note that in the absence of explicit information, it is a *guess*):

Try `[stringWithContentsOfFile:usedEncoding:error:](https://developer.apple.com/documentation/foundation/nsstring/1497254-stringwithcontentsoffile)` or `[initWithContentsOfFile:usedEncoding:error:](https://developer.apple.com/documentation/foundation/nsstring/1418227-initwithcontentsoffile)` (or the URL-based equivalents).

These methods try to determine the encoding of the resource, and if successful return by reference the encoding used.

If (1) fails, try to read the resource by specifying UTF-8 as the encoding.

If (2) fails, try an appropriate legacy encoding.

"Appropriate" here depends a bit on circumstances; it might be the default C string encoding, it might be ISO or Windows Latin 1, or something else, depending on where your data are coming from.

Finally, you can try `NSAttributedString`'s loading methods from the Application Kit (such as `[initWithURL:options:documentAttributes:error:](https://developer.apple.com/documentation/foundation/nsattributedstring/1530490-initwithurl)`).

These methods attempt to load plain text files, and return the encoding used. They can be used on more-or-less arbitrary text documents, and are worth considering if your application has no special expertise in text. They might not be as appropriate for Foundation-level tools or documents that are not natural-language text.

## Writing to Files and URLs
Compared with reading data from a file or URL, writing is straightforward—`NSString` provides two convenient methods, `[writeToFile:atomically:encoding:error:](https://developer.apple.com/documentation/foundation/nsstring/1407654-write)` and `[writeToURL:atomically:encoding:error:](https://developer.apple.com/documentation/foundation/nsstring/1417341-write)`. You must specify the encoding that should be used, and choose whether to write the resource atomically or not. If you do not choose to write atomically, the string is written directly to the path you specify. If you choose to write it atomically, it is written first to an auxiliary file, and then the auxiliary file is renamed to the path. This option guarantees that the file, if it exists at all, won’t be corrupted even if the system should crash during writing. If you write to an URL, the atomicity option is ignored if the destination is not of a type that can be accessed atomically.

```
NSURL *URL = ...;
```

```
NSString *string = ...;
```

```
NSError *error;
```

```
BOOL ok = [string writeToURL:URL atomically:YES
```

```
                  encoding:NSUnicodeStringEncoding error:&error];
```

```
if (!ok) {
```

```
    // an error occurred
```

```
    NSLog(@"Error writing file at %@\n%@",
```

```
              path, [error localizedFailureReason]);
```

```
    // implementation continues ...
```
## Summary
This table summarizes the most common means of reading and writing string objects to and from files and URLs:

Source

Creation method

Extraction method

URL contents

`[stringWithContentsOfURL:encoding:error:](https://developer.apple.com/documentation/foundation/nsstring/1497360-stringwithcontentsofurl)`

`[stringWithContentsOfURL:usedEncoding:error:](https://developer.apple.com/documentation/foundation/nsstring/1497408-stringwithcontentsofurl)`

`[writeToURL:atomically:encoding:error:](https://developer.apple.com/documentation/foundation/nsstring/1417341-write)`

File contents

`[stringWithContentsOfFile:encoding:error:](https://developer.apple.com/documentation/foundation/nsstring/1497327-stringwithcontentsoffile)`

`[stringWithContentsOfFile:usedEncoding:error:](https://developer.apple.com/documentation/foundation/nsstring/1497254-stringwithcontentsoffile)`

`[writeToFile:atomically:encoding:error:](https://developer.apple.com/documentation/foundation/nsstring/1407654-write)`

        
            [Next](SearchingStrings.html)[Previous](formatSpecifiers.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11