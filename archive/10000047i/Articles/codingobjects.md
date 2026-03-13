---
title: "Encoding and Decoding Objects"
book: "Archives and Serializations Programming Guide"
chapterId: "20000948"
date: "2012-07-17"
description: "Explains how to put Cocoa objects into and remove them from a representation suitable for archiving."
identifier: "//apple_ref/doc/uid/10000047i"
source: apple-archive
---

# Encoding and Decoding Objects

> **Archives and Serializations Programming Guide**
> Last updated: 2012-07-17

[Next](codingctypes.html)[Previous](creating.html)
        
        
        [](../index.html)
        
        # Encoding and Decoding Objects
To support encoding and decoding of instances, a class must adopt the `NSCoding`  [protocol](../../../../General/Conceptual/DevPedia-CocoaCore/Protocol.html#//apple_ref/doc/uid/TP40008195-CH45) and implement its methods. This protocol declares two methods that are sent to the objects being encoded or decoded.

In keeping with object-oriented design principles, an object being encoded or decoded is responsible for encoding and decoding its state. A coder instructs the object to do so by invoking `encodeWithCoder:` or `initWithCoder:`. The `encodeWithCoder:` method instructs the object to encode its state with the provided coder; an object can receive this method any number of times. The `initWithCoder:` message instructs the object to initialize itself from data in the provided coder; as such, it replaces any other initialization method and is sent only once per object.

## Encoding an Object
When an object receives an `encodeWithCoder:` message, it should encode all of its vital state (often represented by its properties or instance variables), after forwarding the message to its superclass if its superclass also conforms to the `NSCoding` protocol. An object does not have to encode all of its state. Some values may not be important to reestablish and others may be derivable from related state upon decoding. Other state should be encoded only under certain conditions (for example, with `encodeConditionalObject:`, as described in [Conditional Objects](archives.html#//apple_ref/doc/uid/20000946-142208)).

You use the coding methods, such as `encodeObject:forKey:`, to encode id’s, scalars, C arrays, structures, strings, and pointers to any of these types. See the `[NSKeyedArchiver](https://developer.apple.com/documentation/foundation/nskeyedarchiver)` class specification for a list of methods. For example, suppose you created a Person class that has properties for first name, last name, and height; you might implement `encodeWithCoder:` as follows

- (void)encodeWithCoder:(NSCoder *)coder {
```
    [coder encodeObject:self.firstName forKey:ASCPersonFirstName];
```

```
    [coder encodeObject:self.lastName forKey:ASCPersonLastName];
```

```
    [coder encodeFloat:self.height forKey:ASCPersonHeight];
```

```
}
```
This example assumes that the superclass of Person does not adopt the `NSCoding` protocol. If the superclass of your class does adopt `NSCoding`, you should invoke the superclass’s `encodeWithCoder:` method before invoking any of the other encoding methods:

- (void)encodeWithCoder:(NSCoder *)coder {
```
    [super encodeWithCoder:coder];
```

```
    // Implementation continues.
```
The `@encode()` compiler directive generates an Objective-C type code from a type expression that can be used as the first argument of `encodeValueOfObjCType:at:`. See “Type Encodings” in *[The Objective-C Programming Language](../../ObjectiveC/Introduction/introObjectiveC.html#//apple_ref/doc/uid/TP30001163)* for more information.

## Decoding an Object
In the implementation of an `initWithCoder:` method, the object should first invoke its superclass’s designated initializer to initialize inherited state, and then it should decode and initialize its state. The keys can be decoded in any order. Person’s implementation of `initWithCoder:` might look like this:

- (id)initWithCoder:(NSCoder *)coder {
```
    self = [super init];
```

```
    if (self) {
```

```
        _firstName = [coder decodeObjectForKey:ASCPersonFirstName];
```

```
        _lastName = [coder decodeObjectForKey:ASCPersonLastName];
```

```
        _height = [coder decodeFloatForKey:ASCPersonHeight];
```

```
    }
```

```
    return self;
```

```
}
```
If the superclass adopts the `NSCoding` protocol, you start by assigning of the return value of `initWithCoder:` to `self`:

- (id)initWithCoder:(NSCoder *)coder {
```
    self = [super initWithCoder:coder];
```

```
    if (self) {
```

```
        // Implementation continues.
```
This is done in the subclass because the superclass, in its implementation of `initWithCoder:`, may decide to return an object other than itself.

## Performance Considerations
The less you encode for an object, the less you have to decode, and both writing and reading archives becomes faster. Stop writing out items which are no longer pertinent to the class (but which you may have had to continue writing under non-keyed coding).

Encoding and decoding booleans is faster and cheaper than encoding and decoding individual 1-bit bit fields as integers. However, encoding many bit fields into a single integer value can be cheaper than encoding them individually, but that can also complicate compatibility efforts later (see [Structures and Bit Fields](codingctypes.html#//apple_ref/doc/uid/20001294-96941)).

Don’t read keys you don’t need. You aren’t required to read all the information from the archive for a particular object, as you were with non-keyed coding, and it is much cheaper not to. However, unread data still contributes to the size of an archive, so stop writing it out, too, if you don’t need to read it.

It is faster to just decode a value for a key than to check if it exists and then decode it if it exists. Invoke `containsValueForKey:` only when you need to distinguish the default return value (due to non-existence) from a real value that happens to be the same as the default.

It is typically more valuable for decoding to be fast than for encoding to be fast. If there is some trade-off you can make that improves decoding performance at the cost of lower encoding performance, the trade-off is usually reasonable to make.

Avoid using “$” as a prefix for your keys. The keyed archiver and unarchiver use keys prefixed with “$” for internal values. Although they test for and mangle user-defined keys that have a “$” prefix, this overhead makes archiving slower.

## Making Substitutions During Coding
During encoding or decoding, a coder object invokes methods that allow the object being coded to substitute a replacement class or instance for itself. This allows archives to be shared among implementations with different class hierarchies or simply different versions of a class. Class clusters, for example, take advantage of this feature. This feature also allows classes that should maintain unique instances to enforce this policy on decoding. For example, there need only be a single `NSFont` instance for a given typeface and size.

Substitution methods are declared by `NSObject`, and come in two flavors: generic and specialized. These are the generic methods:

Method

Typical use

`classForCoder`

Allows an object, before being encoded, to substitute a class other than its own. For example, the private subclasses of a class cluster substitute the name of their public superclass when being archived.

`replacementObjectForCoder:`

Allows an object, before being encoded, to substitute another instance in its place.

`awakeAfterUsingCoder:`

Allows an object, after being decoded, to substitute another object for itself. For example, an object that represents a font might, upon being decoded, return an existing object having the same font description as itself. In this way, redundant objects can be eliminated.

The specialized substitution methods are analogous to `classForCoder` and `replacementObjectForCoder:`, but they are designed for (and invoked by) a specific, concrete coder subclass. For example, `classForArchiver` and `replacementObjectForPortCoder:` are used by `NSArchiver` and `NSPortCoder`, respectively. By implementing these specialized methods, your class can base its coding behavior on the specific coder class being used. For more information on these methods, see their method descriptions in the `NSObject` class specification.

In addition to the methods just discussed, `NSKeyedArchiver` and `NSKeyedUnarchiver` allow a delegate object to perform a final substitution before encoding and after decoding objects. The delegate for an `NSKeyedArchiver` object can implement `archiver:willEncodeObject:` and the delegate for an `NSKeyedUnarchiver` object can implement `unarchiver:didDecodeObject:` to perform the substitutions.

## Restricting Coder Support
In some cases, a class may implement the `NSCoding` protocol, but not support one or more coder types. For example, the classes `NSDistantObject`, `NSInvocation`, `NSPort`, and their subclasses adopt `NSCoding` only for use by `NSPortCoder` within the distributed objects system; they cannot be encoded into an archive. In these cases, a class can test whether the coder is of a particular type and raise an exception if it isn’t supported. If the restriction is just to limit a class to sequential or keyed archives, you can send the message `allowsKeyedCoding` to the coder; otherwise, you can test the class identity of the coder as shown in the following sample.

- (void)encodeWithCoder:(NSCoder *)coder {
```
    if ([coder isKindOfClass:[NSKeyedArchiver class]]) {
```

```
        // encode object
```

```
    }
```

```
    else {
```

```
        [NSException raise:NSInvalidArchiveOperationException
```

```
                    format:@"Only supports NSKeyedArchiver coders"];
```

```
    }
```

```
}
```
In other situations, a class may inherit `NSCoding` from a superclass, but the subclass may not want to support coding. For example, `NSWindow` inherits `NSCoding` from `NSResponder`, but it does not support coding. In these cases, the class should override the `initWithCoder:` and `encodeWithCoder:` methods so that they raise exceptions if they are ever invoked.

        
            [Next](codingctypes.html)[Previous](creating.html)
        
         Copyright &#x00a9; 2002, 2012 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2012-07-17