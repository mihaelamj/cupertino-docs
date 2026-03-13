---
title: "Forward and Backward Compatibility for Keyed Archives"
book: "Archives and Serializations Programming Guide"
chapterId: "20001055"
date: "2012-07-17"
description: "Explains how to put Cocoa objects into and remove them from a representation suitable for archiving."
identifier: "//apple_ref/doc/uid/10000047i"
source: apple-archive
---

# Forward and Backward Compatibility for Keyed Archives

> **Archives and Serializations Programming Guide**
> Last updated: 2012-07-17

[Next](subclassing.html)[Previous](codingctypes.html)
        
        
        [](../index.html)
        
        # Forward and Backward Compatibility for Keyed Archives

Keyed archiving gives you plenty of flexibility to make your classes forward and backward compatible. The following sections describe some general tips on how you can implement compatibility and then some guidelines for maintaining compatibility with specific types of changes.

## Benefits of Keyed Archiving
The principal benefit of keyed coding is that it makes it easier to be backward and forward compatible. The ability to read keyed values from the archive in any order, ignore keys you don’t need, and add new keys without disrupting older versions of the class is the foundation for implementing backward and forward compatibility with keyed coding.

For maximum compatibility, you need to be able to do the following:

Read archives created by older versions of your class.

Create archives that can be read by older versions of your class.

Read archives created by future versions of your class.

Create archives that can be read by future versions of your class.

The first two items provide full backward compatibility: the old and current versions of the class can read each others archives. To achieve this capability, it is essential that you know what values were encoded by all the previous versions of your class that you need to support as well as how previous versions decode themselves. If you don’t have this information, you may be able to deduce some things from existing archives and the existing implementations of the `NSCoding` methods. 

The last two items provide full forward compatibility: the current and future versions of the class can read each others archives. To achieve this capability, you need to anticipate the types of changes you may make in the future and code your current `NSCoding` methods appropriately.

## General Tips on Maintaining Compatibility

To easily identify the version of the class being decoded, you can add some version info to the archive. This can be any type of information you want, not just an integer (such as the class version) as it was with non-keyed coding. You may just encode a “version” integer or string with some key or in some rare cases you may want a dictionary object full of goodies. Of course, adding some version information today presumes that you also have a plan for dealing with different versions in your `initWithCoder:` today. If not, changing the version info in the future will not do the present version of the class any good.

Remember to keep your `NSCoding` implementations synchronized. Whenever you change how you write out an objects’ state in the class’s `encodeWithCoder:` method, you need to update your `initWithCoder:` method to understand the new keys. Because information in a keyed archive can be encoded and decoded in any order, the two `NSCoding` methods don’t need to process keys in the same sequence. Use whatever sequences is most convenient for each method.

## Adding New Values to Keys

Some of the values a class encodes may have a particular set of possible values. For example, a button can be a checkbox, a radio button, a push button, and so on. In the future, your set of values may expand; you may create a button that has another type of behavior and need to have a new value for the button’s type.

To prepare for this change in future archives, you can test whether the decoded value for the key is one of the allowed values. If it is not, you can assign a default value to it. Then, the future version of the class can just assign the new value to the old key and the current class will behave reasonably well.

If you are making this change and a previous version did not make allowances for the change or the allowances are insufficient or unacceptable, you probably have to create a whole new key for the new state (see [Adding New Keys](#//apple_ref/doc/uid/20001055-92185)) and make the old key obsolete (see [Removing or Retiring Keys](#//apple_ref/doc/uid/20001055-92825)).

## Adding New Keys

As a class evolves, you may need to add information to the class to describe its new features. For example, a button has a label and a style. Later you may allow the button to have a custom color. You need to create a new key in the archive to hold the color data.

Because you do not need to decode every value in a keyed archive, new keyed values are harmless to old versions of the class, as long as it is OK for them not to be initialized with such state. You can safely add as many new keys as necessary without affecting older versions; old versions automatically ignore those values.

When decoding older archives, you must be prepared to handle the absence of the new key. If appropriate, you can still attempt to decode the new key and just accept the default value for the missing key (`nil`, 0, `NSZeroPoint`, and so on). The coder’s default value may not be valid for every key, however. In that case, you should detect the default value and substitute a more reasonable default value of your own. If the new key is a replacement for an older key, the appropriate substitution should come from the old key, which may require mapping the old value to one of the allowed values for the new key. If you must distinguish between the default value for a missing key and the same value for an existing key, use the `NSCoder` method `containsValueForKey:`.

If the new key is replacing an older key, you need to properly handle the obsolete key (see [Removing or Retiring Keys](#//apple_ref/doc/uid/20001055-92825)).

## Removing or Retiring Keys

As a class evolves, some information may become obsolete or replaced by a newer implementation. 

Because you do not need to decode every value in a keyed archive, when decoding older archives, you can just ignore keys you no longer need. The decoding will be slightly faster, too.

When decoding future archives, you must be prepared to handle missing keys. If appropriate, you can simply accept the default decode value for the missing keys (`nil`, 0, `NSZeroPoint`, and so on). If the coder’s default value is not valid for a particular key, you should detect the default value and substitute a more reasonable default value of your own. If you must distinguish between the default value for a missing key and the same value for an existing key, use the `NSCoder` method `containsValueForKey:`. In this way, you give yourself the flexibility to stop encoding certain values later.

In cases where you need to abandon an old key for a newer one, but an old class cannot handle a missing key appropriately, you need to keep writing some value for the old key as well as the newer key. The value should be something the old class can understand and should probably be as close a simulation of the new state as possible. For example, consider a class that originally came in “vanilla”, “chocolate”, and “butter pecan” flavors and now has additional “double chocolate” and “caramel” flavors. To encode a value for the old key, you can map “double chocolate” to the value for “chocolate” in the old class, but you may have to map “caramel” to “vanilla”. Of course, you write the entire new set of values with the new key and your `initWithCoder:` method should prefer to use the new key if available.

In some cases it may also be useful to build in fallback handling. Fallback handling is useful when one of a set of possible keys for a value is encoded. The set of supported keys may evolve over time, with newer keys being preferred in future versions of your class. Fallback handling defines a fundamental key that must be readable forever, but is used only when no other recognized keys are present. Future versions can then write a value using both a new key and the fallback key. Older versions of the class will not see the new key, but can still read the value with the fallback key.

Consider as an example a class named Image that represents images. (This example does not necessarily reflect the actual behavior of any image class, like `NSImage`.) Suppose the Image class is able to encode its instances as an URL, JPEG, or GIF, depending on whichever is most convenient for the particular instance. An encoded Image object, therefore, contains only one of the following keys: `@"URL"`, `@"JPEG"`, `@"GIF"`. The `initWithCoder:` method checks for the keys in the order `@"URL"`, `@"JPEG"`, `@"GIF"`, and initializes itself with the first representation that it finds. In the future it might be that none of these are easy or convenient to archive (for example, taking whatever data the Image instance does have and converting it to JPEG might be fairly expensive).

An example of fallback handling in this case would be to allow for an additional key (or group of keys), like `@"rawdata"`, that is understood and used by Image’s `initWithCoder:` method if none of the other keys for this value (the image data) are present. The value of the `@"rawdata"` key might be defined, for example, to be an `NSData` object containing 32-bit RGBA pixels. There might also be auxiliary keys like `@"pixelshigh"` and `@"pixelswide"` that `initWithCoder:` would look for to get a minimal set of information needed to produce an Image instance from the archived information. In the future, the encoding process for an Image might write out the convenient information, whatever that is at that time, and would also have to write out the `@"rawdata"` and other keys to allow old decoders to read the object.

## Changing Bit Sizes of Values

In some situations, you may have code and archives that you use on 32- and 64-bit platforms (for example, you might have iOS and OS X versions of an application that share data via iCloud). In these cases, you need to take care to ensure that integer values are treated correctly.

Encoding what is a 32-bit integer as a 64-bit integer isn’t necessarily the best solution. The extra high-order zero bits you’re giving to the value as you give it to the archiver are wasted. On the other hand, it isn’t harmful either, and is easy to implement.

The generic `encodeInt:forKey:` and `decodeIntForKey:` methods read and write whatever the native `int` size is on the computer. On a 64-bit computer, `int` may be 64 bits wide (or it might not be; the C language is flexible in this regard). Therefore, it’s possible that values requiring more than 32 bits to represent may be written by an `encodeInt:forKey:` method. If such an archive is transported to a 32-bit computer, the `decodeIntForKey:` method may be unable to represent that integer in the `int` return value, and have to throw an `NSRangeException`.

Whether or not it is useful to attempt to handle this by always decoding such integers as 64-bit is debatable. If the integer is a “count” of something, for example, it may be physically impossible to have more than 2^32 of whatever it is on a 32-bit computer, so further attempting to unarchive the file is probably a waste of time, and an exception is reasonable. Alternatively, you might want to either catch the exception or perform your own bounds checking on a 64-bit decoded value and return `nil` from `initWithCoder:`. However, the caller of the `decodeObjectForKey:` method that is unpacking an instance of your class may not like the `nil` any more than the exception, and might end up raising an exception of its own that is less intelligible as to the cause of the problem than the range exception might have been.

        
            [Next](subclassing.html)[Previous](codingctypes.html)
        
         Copyright &#x00a9; 2002, 2012 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2012-07-17