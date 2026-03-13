---
title: "Encoding and Decoding C Data Types"
book: "Archives and Serializations Programming Guide"
chapterId: "20001294"
date: "2012-07-17"
description: "Explains how to put Cocoa objects into and remove them from a representation suitable for archiving."
identifier: "//apple_ref/doc/uid/10000047i"
source: apple-archive
---

# Encoding and Decoding C Data Types

> **Archives and Serializations Programming Guide**
> Last updated: 2012-07-17

[Next](compatibility.html)[Previous](codingobjects.html)
        
        
        [](../index.html)
        
        # Encoding and Decoding C Data Types
`NSKeyedArchiver` and `NSKeyedUnarchiver` provide a number of methods for handling non-object data. Integers can be encoded with `encodeInt:forKey:`, `encodeInt32:forKey:`, or `encodeInt64:forKey:`. Likewise, real values can be encoded with `encodeFloat:forKey:` or `encodeDouble:forKey:`. Other methods encode booleans and byte arrays. The classes also provide several convenience methods for handling special data types used in Cocoa, such as `NSPoint`, `NSSize`, and `NSRect`.

`NSKeyedArchiver` and `NSKeyedUnarchiver` do not provide methods for encoding and decoding aggregate types, such as structures, arrays, and bit fields. The following sections provide suggestions on how to handle unsupported data types.

## Pointers
You can’t encode a pointer and get back something useful at decode time. You have to encode the information to which the pointer is pointing. This is true in non-keyed coding as well.

Pointers to C strings (`char *`) are a special case because they can be treated as a byte array which can be encoded using `encodeBytes:length:forKey:`. You can also wrap C strings with a temporary `NSString` object and archive the string. Reverse the process when decoding. Be sure to keep in mind the character set encoding of the string when creating the `NSString` object, and chose an appropriate creation method.

## Arrays of Simple Types
If you are encoding an array of bytes, you can just use the provided methods to do so.

For other arithmetic types, create an `NSData` object with the array. *Note that in this case, dealing with platform endianness issues is your responsibility.* Platform endianness can be handled in two general ways. The first technique is to convert the elements of the array (or rather, a temporary copy of the array) into a canonical endianness, either big or little, one at a time with the functions discussed in [Swapping Bytes](../../../../MacOSX/Conceptual/universal_binary/universal_binary_byte_swap/universal_binary_swap.html#//apple_ref/doc/uid/TP40002217-CH243) in *[Universal Binary Programming Guidelines, Second Edition](../../../../MacOSX/Conceptual/universal_binary/universal_binary_intro/universal_binary_intro.html#//apple_ref/doc/uid/TP40002217)* (see also “Byte Ordering” in *Foundation Functions Reference*) and give that result to the `NSData` as the buffer. (Or, you can write the bytes directly with `encodeBytes:length:forKey:`.) At decode time, you have to reverse the process, converting from the big or little endian canonical form to the current host representation. The other technique is to use the array as-is and record in a separate keyed value (perhaps a boolean) which endianness the host was when the archive was created. During decoding, read the endian key and compare it to the endianness of the current host and swap the values only if different.

Alternatively, you can archive each array element separately as their native type, perhaps by using key names inspired by the array syntax, like “theArray[0]”, “theArray[1]”, and so on. This is not a terribly efficient technique, but you can ignore endian issues.

## Arrays of Objects
The simplest thing to do for a C array of objects is to temporarily wrap the array in an `NSArray` object with `initWithObjects:count:`, encode the array object, then get rid of the object. Because objects contain other information that has to be encoded, you can’t just embed the array of pointers in an `NSData` object; each object must be individually archived. During decoding, use `getObjects:` on the retrieved array to get the objects back out into an allocated C array (of the correct size).

## Structures and Bit Fields
The best technique for archiving a structure or a collection of bit fields is to archive the fields independently and choose the appropriate type of encoding/decoding method for each. The key names can be composed from the structure field names if you wish, like “theStruct.order”, “theStruct.flags”, and so on. This creates a slight dependency on the names of the fields in the source code, which over time may get renamed, but the archiving keys cannot change if you want to maintain compatibility.

You should not wrap a structure with an `NSData` object and archive that. If the structure contains object or pointer fields, the data object isn’t going to archive them correctly. You also create a dependence on how the compiler decides to lay out the structure, which can change between versions of the compiler and may depend on other factors. A compiler is not constrained to organize a structure just as you’ve specified it in the source code—there may be arbitrary internal and invisible padding bytes between fields in the structure, for example, and the amount of these can change without notice and on different platforms. In addition, any fields that are multiple bytes in width aren’t going to get treated correctly with respect to endianness issues. You will cause yourself all sorts of compatibility trouble.

Likewise, bit fields should never be encoded by reading the raw bits of several bit fields as an integer and encoding the integer. (Encoding an integer that you construct manually from several bit fields, using bit shifts and OR operations, however, avoids most of the pitfalls that follow.) Although there are some requirements on compilers specified in the C standard, a compiler still has some freedom in how things are actually organized and which bits it chooses to store where, and what bits it may choose not to use (inter-field padding bits). The location of those bits could differ between compilers or change as a particular compiler evolves. On top of this, you also have to deal with endianness issues. The order of the bits within an integer could be different for the machine that encodes the archive and the machine that decodes it. Finally, by encoding the raw bits, you constrain future development of your class to use the same bit field sizes as the oldest archive you need to support. Otherwise, you have to be able to parse the old bit stream and initialize the new bit stream yourself, handling the compiler and platform issues appropriately.

As is the case for an object’s instance variables in general, it is not necessary to archive every field of a structure or bit field. You only need to encode and decode the fields required to preserve a structure’s state. Fields that are calculated or otherwise derived by other means should not be archived.

## More Complex Data Types
More complex data types, such as arrays of aggregates, can generally be handled using the techniques for simple data types and combining them with custom logic for your particular application.

        
            [Next](compatibility.html)[Previous](codingobjects.html)
        
         Copyright &#x00a9; 2002, 2012 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2012-07-17