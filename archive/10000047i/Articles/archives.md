---
title: "Archives"
book: "Archives and Serializations Programming Guide"
chapterId: "20000946"
date: "2012-07-17"
description: "Explains how to put Cocoa objects into and remove them from a representation suitable for archiving."
identifier: "//apple_ref/doc/uid/10000047i"
source: apple-archive
---

# Archives

> **Archives and Serializations Programming Guide**
> Last updated: 2012-07-17

[Next](creating.html)[Previous](objectgraphs.html)
        
        
        [](../index.html)
        
        # Archives
Archives provide a means to convert objects and values into an architecture-independent stream of bytes that preserves the identity of and the relationships between the objects and values.

Cocoa archives can hold Objective-C objects, scalars, arrays, structures, and strings. They do not hold types whose implementation varies across platforms, such as `union`, `void *`, function pointers, and long chains of pointers.

Archives store object type information along with the data, so an object decoded from a stream of bytes is normally of the same class as the object that was originally encoded into the stream. Exceptions to this rule are described in [Making Substitutions During Coding](codingobjects.html#//apple_ref/doc/uid/20000948-97072).

## Coders
Objects are written to and read from archives with coder objects. Coder objects are instances of concrete subclasses of the abstract class `NSCoder`. `NSCoder` declares an extensive interface for taking the information stored in an object and putting it into another format suitable for writing to a file, transmitting between processes or across a network, or performing other types of data exchange. `NSCoder` also declares the interface for reversing the process, taking the information stored in a byte stream and converting it back into an object. Subclasses implement the appropriate portions of this interface to support a specific archiving format. 

Coder objects read and write objects by sending one of two [messages](../../../../General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59) to the objects to be encoded or decoded. A coder sends `encodeWithCoder:` to objects when creating an archive and `initWithCoder:` when reading an archive. These messages are defined by the `NSCoding`  [protocol](../../../../General/Conceptual/DevPedia-CocoaCore/Protocol.html#//apple_ref/doc/uid/TP40008195-CH45). Only objects whose class conforms to the `NSCoding` protocol can be written to an archive. (The reference for each Cocoa class indicates whether the class adopts the `NSCoding` protocol.) When an object receives one of these messages, the object sends messages back to the coder to tell the coder which objects or values, usually instance variables, to read or write next. When encoding objects, the coder records in the archive the class identity of the objects (or type of Objective-C values) and their position in the hierarchy.

Object graphs, such as the one shown in [Figure 1](objectgraphs.html#//apple_ref/doc/uid/20001293-111742-BBCDBFCA), pose two problems for a coder: redundancy and constraint. To solve these problems, `NSCoder` introduces the concepts of root objects and conditional objects, which are described in the following sections.

### Root Object
An object graph is not necessarily a simple tree structure. Two objects can contain references to each other, for example, creating a cycle. If a coder follows every link and blindly encodes each object it encounters, this circular reference will generate an infinite loop in the coder. Also, a single object can be referenced by several other objects. The coder must be able to recognize and handle multiple and circular references so that it does not encode more than one copy of each object, but still regenerate all the references when decoding.

To solve this problem, `NSCoder` introduces the concept of a root object. The root object is the starting point of an object graph. To encode an object graph, you invoke the `NSCoder` method `encodeRootObject:`, passing in the first object to encode. Every object encoded within the context of this invocation is tracked. If the coder is asked to encode an object more than once, the coder encodes a reference to the first encoding instead of encoding the object again.

`NSCoder` does not implement support for root objects; `NSCoder`’s implementation of `encodeRootObject:` simply encodes the object by invoking `encodeObject:`. It is the responsibility of its concrete subclasses to keep track of multiple references to objects, thus preserving the structure of any object graphs.

### Conditional Objects
Another problem presented by object graphs is that it is not always appropriate to archive the entire graph. For example, when you encode an `NSView` object, the view can have many links to other objects: subviews, superviews, formatters, targets, windows, menus, and so on. If a view encoded all of its references to these objects, the entire application would get pulled in. Some objects are more important than others, though. A view’s subviews always should be archived, but not necessarily its superview. In this case, the superview is considered an extraneous part of the graph; a view can exist without its superview, but not its subviews. A view, however, needs to keep a reference to its superview, if the superview is also being encoded in the archive.

To solve this dilemma, `NSCoder` introduces the concept of a conditional object. A conditional object is an object that should be encoded only if it is being encoded unconditionally elsewhere in the object graph. A conditional object is encoded by invoking  `encodeConditionalObject:forKey:`. If all requests to encode an object are made with these conditional methods, the object is not encoded and references to it decode to `nil`. If the object is encoded elsewhere, all the conditional references decode to the single encoded object.

Typically, conditional objects are used to encode weak references to objects.

`NSCoder` does not implement support for conditional objects; `NSCoder`’s implementation of  `encodeConditionalObject:forKey:` simply encodes the object by invoking `encodeObject:forKey:`. It is the responsibility of its concrete subclasses to keep track of conditional objects and to not encode objects unless they are needed. 

## Keyed Archives
Keyed archives are created by `[NSKeyedArchiver](https://developer.apple.com/documentation/foundation/nskeyedarchiver)` objects and decoded by `[NSKeyedUnarchiver](https://developer.apple.com/documentation/foundation/nskeyedunarchiver)` objects. Keyed archives differ from sequential archives in that every value encoded in a keyed archive is given a name, or key. When decoding the archive, the values can be requested by name, allowing the values to be requested in any order or not at all. This freedom enables greater flexibility for making your classes forward and backward compatible.

The following sections describe how to use keyed archives.

### Naming Values
Values that an object encodes to a keyed archive can be individually named with an arbitrary string. Archives are hierarchical with each object defining a separate name space for its encoded values, similar to the object’s instance variables. Therefore, keys must be unique only within the scope of the current object being encoded. The keys used by object A to encode its instance variables do not conflict with the keys used by object B, even if A and B are instances of the same class. Within a single object, however, the keys used by a subclass can conflict with keys used in its superclasses.

Public classes, such as those in a [framework](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56), which can be subclassed, should add a prefix to the name string to avoid collisions with keys that may be used now or in the future by the subclasses of the class. A reasonable prefix is the full name of the class. Cocoa classes use the prefix “NS” in their keys, the same as the API prefix, and carefully makes sure that there are no collisions in the class hierarchy. Another possibility is to use the same string as the bundle identifier for the framework. 

You should avoid using “$” as a prefix for your keys. The keyed archiver and unarchiver use keys prefixed with “$” for internal values. Although they test for and mangle user-defined keys that have a “$” prefix, this overhead slows down archiving performance.

Subclasses also need to be somewhat aware of the prefix used by superclasses to avoid accidental collisions on key names. Subclasses of Cocoa classes should avoid unintentionally starting their key names with “NS”. For example, don’t name a key “NSString search options”.

### Return Values for Missing Keys
While decoding, if you request a keyed value that does not exist, the unarchiver returns a default value based on the return type of the decode method you invoked. The default values are the equivalent of zero for each data type: `nil` for objects, `NO` for booleans, `0.0` for reals, `NSZeroSize` for sizes, and so on. If you need to detect the absence of a keyed value, use the `NSKeyedUnarchiver` instance method `containsValueForKey:`, which returns `NO` if the supplied key is not present. For performance reasons, you should avoid explicitly testing for keys when the default values are sufficient.

### Type Coercions
`NSKeyedUnarchiver` supports limited type coercion. A value encoded as any type of integer, be it a standard `int` or an explicit 32-bit or 64-bit integer, can be decoded using any of the integer decode methods. Likewise, a value encoded as a `float` or `double` can be decoded as either a `float` or a `double` value. When decoding a `double` value as a `float`, though, the decoded value loses precision. If an encoded value is too large to fit within the coerced decoded type, the decoding method throws an `NSRangeException`. Further, when trying to coerce a value to an incompatible type, such as decoding an `int` as a `float`, the decoding method throws an `NSInvalidUnarchiveOperationException`.

### Class Versioning
Versioning of encoded data is not handled through class versioning with keyed coding as it is in sequential archives. In fact, no automatic versioning is done for a class; this allows a class to at least get a look at the encoded values without the unarchiver deciding on its own that the versions fatally mismatch. A class is free to decide to encode some type of version information with its other values if it wishes, and this information can be of any type or quantity.

### Root Objects
The `encodeObject:` and `encodeObject:forKey:` methods are able to track multiple references to objects in multiple object graphs. As many object graphs or values as desired may be encoded at the top level of a keyed archive.

`NSKeyedArchiver` implements `archiveRootObject:toFile:` and `archivedDataWithRootObject:` to produce archives with a single object graph. These archives, however, have to be unarchived using the `NSKeyedUnarchiver` methods `unarchiveObjectWithFile:` and `unarchiveObjectWithData:`.

### Delegates
`NSKeyedArchiver` and `NSKeyedUnarchiver` objects can have [delegate objects](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14). The delegates are notified as each object is encoded or decoded. You can use the delegates to perform substitutions, replacing one object for another, if desired.

### Non-Keyed Coding Methods
Keyed archivers do not have to provide names for every value encoded in the archive. The `NSKeyedArchiver` and `NSKeyedUnarchiver` classes implement the non-keyed encoding and decoding methods that they inherit from `NSCoder`. Use of the non-keyed methods within keyed coding is discouraged.

        
            [Next](creating.html)[Previous](objectgraphs.html)
        
         Copyright &#x00a9; 2002, 2012 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2012-07-17