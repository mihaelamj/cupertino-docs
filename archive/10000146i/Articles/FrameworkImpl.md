---
title: "Tips and Techniques for Framework Developers"
book: "Coding Guidelines for Cocoa"
chapterId: "20001286"
date: "2013-10-22"
description: "Provides naming guidelines for Cocoa API and design advice to framework developers."
identifier: "//apple_ref/doc/uid/10000146i"
source: apple-archive
---

# Tips and Techniques for Framework Developers

> **Coding Guidelines for Cocoa**
> Last updated: 2013-10-22

[Next](../RevisionHistory.html)[Previous](APIAbbreviations.html)
        
        
        [](../index.html)
        
        # Tips and Techniques for Framework Developers

Developers of [frameworks](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56) have to be more careful than other developers in how they write their code. Many client applications could link in their framework and, because of this wide exposure, any deficiencies in the framework might be magnified throughout a system. The following items discuss programming techniques you can adopt to ensure the efficiency and integrity of your framework.

> **Note:** Some of these techniques are not limited to frameworks. You can productively apply them in application development.

## Initialization

The following suggestions and recommendations cover framework initialization.

### Class Initialization

The `initialize`  [class method](../../../../General/Conceptual/DevPedia-CocoaCore/ClassMethod.html#//apple_ref/doc/uid/TP40008195-CH8) gives you a place to have some code executed once, lazily, before any other method of the class is invoked. It is typically used to set the version numbers of classes (see [Versioning and Compatibility](#//apple_ref/doc/uid/20001286-1001777)). 

The runtime sends `initialize` to each class in an inheritance chain, even if it hasn’t implemented it; thus it might invoke a class’s `initialize` method more than once (if, for example, a subclass hasn’t implemented it). Typically you want the initialization code to be executed only once. One way to ensure this happens is to use `dispatch_once()`:

+ (void)initialize {
```
    static dispatch_once_t onceToken = 0;
```

```
    dispatch_once(&onceToken, ^{
```

```
        // the initializing code
```

```
    }
```

```
}
```

```
 
```

> **Note:** Because the runtime sends initialize to every class, it's possible that `initialize` will be called in the context of a subclass—if the subclass doesn't implement `initialize`, then the invocation will fall through to the superclass. If you specifically need to perform initialization within the context of the relevant class, you can perform the following check rather than using `dispatch_once()`:

```
if (self == [NSFoo class]) {
```

```
    // the initializing code
```

```
}
```

You should never invoke the `initialize` method explicitly. If you need to trigger the initialization, invoke some harmless method, for example: 

```
[NSImage self];
```

### Designated Initializers

A designated initializer is an `init` method of a class that invokes an `init` method of the superclass. (Other initializers invoke the `init` methods defined by the class.) Every public class should have one or more designated initializers. As examples of designated initializers there is `NSView`’s `initWithFrame:` and `NSResponder`’s `init` method. Where `init` methods are not meant to be [overridden](../../../../General/Conceptual/DevPedia-CocoaCore/MethodOverriding.html#//apple_ref/doc/uid/TP40008195-CH57), as is the case with `NSString` and other abstract classes fronting [class clusters](../../../../General/Conceptual/DevPedia-CocoaCore/ClassCluster.html#//apple_ref/doc/uid/TP40008195-CH7), the subclass is expected to implement its own.

Designated initializers should be clearly identified because this information is important to those who want to subclass your class. A subclass can just override the designated initializer and all other initializers will work as designed.

When you implement a class of a framework, you often have to implement its [archiving](../../../../General/Conceptual/DevPedia-CocoaCore/Archiving.html#//apple_ref/doc/uid/TP40008195-CH1) methods as well: `initWithCoder:` and `encodeWithCoder:`. Be careful not to do things in the initialization code path that doesn’t happen when the object is unarchived. A good way to achieve this is to call a common routine from your designated initializers and `initWithCoder:` (which is a designated initializer itself) if your class implements archiving.

### Error Detection During Initialization

A well-designed initialization method should complete the following steps to ensure the proper detection and propagation of errors:

Reassign self by invoking `super`’s [designated initializer](../../../../General/Conceptual/DevPedia-CocoaCore/Initialization.html#//apple_ref/doc/uid/TP40008195-CH21).

Check the returned value for `nil`, which indicates that some error occurred in the superclass initialization.

If an error occurs while initializing the current class, release the object and return `nil`. 

[Listing 1](#//apple_ref/doc/uid/20001286-1004028-BAJBJCIA) illustrates how you might do this.

**Listing 1**  Error detection during initialization

```
- (id)init {
```

```
    self = [super init];  // Call a designated initializer here.
```

```
    if (self != nil) {
```

```
        // Initialize object  ...
```

```
        if (someError) {
```

```
            [self release];
```

```
            self = nil;
```

```
        }
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

## Versioning and Compatibility

When you add new classes or methods to your [framework](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56), it is not usually necessary to specify new version numbers for each new feature group. Developers typically perform (or should perform) [Objective-C](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectiveC.html#//apple_ref/doc/uid/TP40008195-CH43) runtime checks such as `respondsToSelector:` to determine if a feature is available on a given system. These runtime tests are the preferred and most dynamic way to check for new features.

However, you can employ several techniques to make sure each new version of your framework are properly marked and made as compatible as possible with earlier versions.

### Framework Version

When the presence of a new feature or bug fix isn’t easily detectable with runtime tests, you should provide developers with some way to check for the change. One way to achieve this is to store the exact version number of the framework and make this number accessible to developers:

Document the change (in a release note, for instance) under a version number.

Set the current version number of your framework and provide some way to make it globally accessible. You might store the version number in your framework’s information [property list](../../../../General/Conceptual/DevPedia-CocoaCore/PropertyList.html#//apple_ref/doc/uid/TP40008195-CH44) (`Info.plist`) and access it from there.

### Keyed Archiving

If the objects of your framework need to be written to [nib file](../../../../General/Conceptual/DevPedia-CocoaCore/NibFile.html#//apple_ref/doc/uid/TP40008195-CH34), they must be able to archive themselves. You also need to archive any documents that use the archiving mechanisms to store document data. 

You should consider the following issues about archiving:

If a key is missing in an archive, asking for its value will return `nil`, `NULL`, `NO`, 0, or 0.0, depending on the type being asked for. Test for this return value to reduce the data that you write out. In addition, you can find out whether a key was written to the archive.

Both the encode and decode methods can do things to ensure backwards compatibility. For instance, the encode method of a new version of a class might write new values using keys but can still write out older fields so that older versions of the class can still understand the object. In addition, decode methods might want to deal with missing values in some reasonable way to maintain some flexibility for future versions.

A recommended naming [convention](../../../../General/Conceptual/DevPedia-CocoaCore/CodingConventions.html#//apple_ref/doc/uid/TP40008195-CH53) for archive keys for framework classes is to begin with the prefix used for other API elements of the framework and then use the name of the instance variable. Just make sure that names cannot conflict with the names of any superclass or subclass.

If you have a utility function that writes out a basic data type (in other words, a value that isn’t an object), be sure to use a unique key. For example, an “archiveRect” routine that archives a rectangle should take a key argument and either use the given key or, if it writes out multiple values (for instance, four floats), it should append its own unique bits to the provided key.

Archiving bitfields as-is can be dangerous due to compiler and endianness dependencies. You should archive them only when, for performance reasons, a lot of bits need to be written out, many times. See [Bitfields](#//apple_ref/doc/uid/20001286-1005950) for a suggestion.

## Exceptions and Errors

Most [Cocoa](../../../../General/Conceptual/DevPedia-CocoaCore/Cocoa.html#//apple_ref/doc/uid/TP40008195-CH9) framework methods do not force developers to catch and handle [exceptions](../../../../General/Conceptual/DevPedia-CocoaCore/ExceptionHandling.html#//apple_ref/doc/uid/TP40008195-CH18). That is because exceptions are not raised as a normal part of execution, and are not typically used to communicate expected runtime or user errors. Examples of these errors include: 

File not found

No such user

Attempt to open a wrong type of document in an application

Error in converting a string to a specified encoding

However, Cocoa does raise exceptions to indicate programming or logic errors such as the following:

Array index out of bounds

Attempt to mutate immutable objects

Bad argument type

The expectation is that the developer will catch these kinds of errors during testing and address them before shipping the application; thus the application should not need to handle the exceptions at runtime. If an exception is raised and no part of the application catches it, the top-level default handler typically catches and reports the exception and execution then continues. Developers can choose to replace this default exception-catcher with one that gives more detail about what went wrong and offers the option to save data and quit the application. 

Errors are another area where Cocoa frameworks differ from some other software libraries. Cocoa methods generally do not return error codes. In cases where there is one reasonable or likely reason for an error, the methods rely on a simple test of a boolean or object (`nil`/non-`nil`) returned value; the reasons for a `NO` or `nil` returned value are documented. You should not use error codes to indicate programming errors to be handled at runtime, but instead raise exceptions or in some cases simply log the error without raising an exception. 

For instance, `NSDictionary`’s `objectForKey:` method either returns the found object or `nil` if it can’t find the object. `NSArray`’s `objectAtIndex:` method can never return `nil` (except for the overriding general language convention that any message to `nil` results in a `nil `return), because an `NSArray` object cannot store `nil` values, and by definition any out-of-bounds access is a programming error that should result in an exception. Many `init` methods return `nil` when the object cannot be initialized with the parameters supplied. 

In the small number of cases where a method has a valid need for multiple distinct error codes, it should specify them in a by-reference argument that returns either an error code, a localized error string, or some other information describing the error. For example, you might want to return the error as an `NSError` object; look at the `NSError.h` header file in Foundation for details. This argument might be in addition to a simpler `BOOL` or `nil` that is directly returned. The method should also observe the convention that all by-reference arguments are optional and thus allow the sender to pass `NULL` for the error-code argument if they do not wish to know about the error. 

## Framework Data

How you handle framework data has implications for performance, cross-platform compatibility, and other purposes. This section discusses techniques involving framework data. 

### Constant Data

For performance reasons, it is good to mark as constant as much framework data as possible because doing so reduces the size of the `__DATA` segment of the Mach-O binary. Global and static data that is not `const` ends up in the `__DATA` section of the `__DATA` segment. This kind of data takes up memory in every running instance of an application that uses the framework. Although an extra 500 bytes (for example) might not seem so bad, it might cause an increment in the number of pages required—an additional four kilobytes per application. 

You should mark any data that is constant as `const`. If there are no `char *` pointers in the block, this will cause the data to land in the `__TEXT` segment (which makes it truly constant); otherwise it will stay in the `__DATA` segment but will not be written on (unless prebinding is not done or is violated by having to slide the binary at load time). 

You should initialize static variables to ensure that they are merged into the `__data` section of the `__DATA` segment as opposed to the `__bss` section. If there is no obvious value to use for initialization, use 0, `NULL`, 0.0, or whatever is appropriate. 

### Bitfields

Using signed values for bitfields, especially one-bit bitfields, can result in undefined behavior if code assumes the value is a boolean. One-bit bitfields should always be unsigned. Because the only values that can be stored in such a bitfield are 0 and -1 (depending on the compiler implementation), comparing this bitfield to 1 is false. For example, if you come across something like this in your code: 

```
BOOL isAttachment:1;
```

```
int startTracking:1;
```

You should change the type to `unsigned int`. 

Another issue with bitfields is archiving. In general, you shouldn’t write bitfields to disk or archives in the form they are in, as the format might be different when they are read again on another architecture, or on another compiler.

### Memory Allocation

In framework code, the best course is to avoid allocating memory altogether, if you can help it. If you need a temporary buffer for some reason, it’s usually better to use the stack than to allocate a buffer. However, stack is limited in size (usually 512 kilobytes altogether), so the decision to use the stack depends on the function and the size of the buffer you need. Typically if the buffer size is 1000 bytes (or `MAXPATHLEN`) or less, using the stack is acceptable. 

One refinement is to start off using the stack, but switch to a `malloc`’ed buffer if the size requirements go beyond the stack buffer size. [Listing 2](#//apple_ref/doc/uid/20001286-1008754-BAJHEFCE) presents a code snippet that does just that: 

**Listing 2**  Allocation using both stack and malloc’ed buffer

```
#define STACKBUFSIZE (1000 / sizeof(YourElementType))
```

```
 YourElementType stackBuffer[STACKBUFSIZE];
```

```
 YourElementType *buf = stackBuffer;
```

```
 int capacity = STACKBUFSIZE;  // In terms of YourElementType
```

```
 int numElements = 0;  // In terms of YourElementType
```

```
 
```

```
while (1) {
```

```
    if (numElements > capacity) {  // Need more room
```

```
        int newCapacity = capacity * 2;  // Or whatever your growth algorithm is
```

```
        if (buf == stackBuffer) {  // Previously using stack; switch to allocated memory
```

```
            buf = malloc(newCapacity * sizeof(YourElementType));
```

```
            memmove(buf, stackBuffer, capacity * sizeof(YourElementType));
```

```
        } else {  // Was already using malloc; simply realloc
```

```
            buf = realloc(buf, newCapacity * sizeof(YourElementType));
```

```
        }
```

```
        capacity = newCapacity;
```

```
    }
```

```
    // ... use buf; increment numElements ...
```

```
  }
```

```
  // ...
```

```
  if (buf != stackBuffer) free(buf);
```
	
## Object Comparison

You should be aware of an important difference between the generic [object-comparison](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectComparison.html#//apple_ref/doc/uid/TP40008195-CH37) method `isEqual:` and the comparison methods that are associated with an object type, such as `isEqualToString:`. The `isEqual:` method allows you to pass arbitrary objects as arguments and returns `NO` if the objects aren’t of the same class. Methods such as `isEqualToString:` and `isEqualToArray:` usually assume the argument is of the specified type (which is that of the receiver). They therefore do not perform type-checking and consequently they are faster but not as safe. For values retrieved from external sources, such as an application’s information [property list](../../../../General/Conceptual/DevPedia-CocoaCore/PropertyList.html#//apple_ref/doc/uid/TP40008195-CH44) (`Info.plist`) or preferences, the use of `isEqual:` is preferred because it is safer; when the types are known, use `isEqualToString:` instead. 

A further point about `isEqual:` is its connection to the `hash` method. One basic invariant for objects that are put in a hash-based Cocoa [collection](../../../../General/Conceptual/DevPedia-CocoaCore/Collection.html#//apple_ref/doc/uid/TP40008195-CH10) such as an `NSDictionary` or `NSSet` is that if `[A isEqual:B] == YES`, then `[A hash] == [B hash]`. So if you override `isEqual:` in your class, you should also override `hash` to preserve this invariant. By default `isEqual:` looks for pointer equality of each object’s address, and `hash` returns a hash value based on each object’s address, so this invariant holds.

        
            [Next](../RevisionHistory.html)[Previous](APIAbbreviations.html)
        
         Copyright &#x00a9; 2003, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-10-22