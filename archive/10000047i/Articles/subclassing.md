---
title: "Subclassing NSCoder"
book: "Archives and Serializations Programming Guide"
chapterId: "20000951"
date: "2012-07-17"
description: "Explains how to put Cocoa objects into and remove them from a representation suitable for archiving."
identifier: "//apple_ref/doc/uid/10000047i"
source: apple-archive
---

# Subclassing NSCoder

> **Archives and Serializations Programming Guide**
> Last updated: 2012-07-17

[Next](serializing.html)[Previous](compatibility.html)
        
        
        [](../index.html)
        
        # Subclassing NSCoder

`NSCoder`’s interface is quite general and extensive, declaring methods to encode and decode objects and values with and without keys. Concrete subclasses are not required to properly implement all of `NSCoder`’s methods and may explicitly restrict themselves to certain types of operations. For example, `NSArchiver` does not implement the `decode...` methods, and `NSUnarchiver` does not implement the `encode...` methods. In addition, neither class implements the keyed coding methods for encoding and decoding keyed archives. Invoking a `decode` method on `NSArchiver` or an `encode` method on `NSUnarchiver` raises an `NSInvalidArgumentException`.

If you define a subclass of `NSCoder` that does not support keyed coding, at a minimum your subclass must override the following methods:

`encodeValueOfObjCType:at:`

`decodeValueOfObjCType:at:`

`encodeDataObject:`

`decodeDataObject`

`versionForClassName:`

If your subclass supports keyed coding, you must override the above methods as well as the `allowsKeyedCoding` method (to return `YES`) and all of the keyed coding methods defined by `NSCoder`. In both cases, if you are creating separate classes for encoding and decoding, you do not need to override the encode methods in the decoder class nor the decode methods in the encoder class.

Note that `encodeObject:` and `decodeObject` are not among the basic methods. They are defined abstractly to invoke `encodeValueOfObjCType:at:` or `decodeValueOfObjCType:at:` with an Objective-C type code of “@”. Your implementations of the latter two methods must handle this case, invoking the object’s `encodeWithCoder:` or `initWithCoder:` method and sending the proper substitution messages (as described in [Making Substitutions During Coding](codingobjects.html#//apple_ref/doc/uid/20000948-97072)) to the object before encoding it and after decoding it.

Your subclass may override other methods to provide specialized handling for certain situations. In particular, you can implement any of the following methods:

`encodeRootObject:`

`encodeConditionalObject:`

`encodeBycopyObject:`

`encodeByrefObject:`

See the individual method descriptions for more information on their required behavior. The default `NSCoder` implementations of these methods just invoke `encodeObject:`.

If you override `encodeConditionalObject:` to support conditional objects (see [Conditional Objects](archives.html#//apple_ref/doc/uid/20000946-142208)), be aware that the first unconditional encoding may occur after any number of conditional encoding requests, so your coder will not know which conditional objects to encode until all the other objects have been encoded.

With objects, the object being coded is fully responsible for coding itself. However, a few classes hand this responsibility back to the coder object, either for performance reasons or because proper support depends on more information than the object itself has. The notable classes in Foundation that do this are `NSData` and `NSPort`. `NSData`’s low-level nature makes optimization important. For this reason, an `NSData` object always asks its coder to handle its contents directly using the `encodeDataObject:` and `decodeDataObject` methods when it receives the `encodeWithCoder:` and `initWithCoder:` messages. Similarly, an `NSPort` object asks its coder to handle it using the `encodePortObject:` and `decodePortObject` methods (which only `NSPortCoder` implements). This is because an `NSPort` represents information kept in the operating system itself, which requires special handling for transmission to another process.

These special cases don’t affect users of coder objects, since the redirection is handled by the classes themselves in their `NSCoding` protocol methods. An implementor of a concrete coder subclass, however, must implement the appropriate custom methods to encode and decode `NSData` and (if relevant) `NSPort` objects itself.

        
            [Next](serializing.html)[Previous](compatibility.html)
        
         Copyright &#x00a9; 2002, 2012 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2012-07-17