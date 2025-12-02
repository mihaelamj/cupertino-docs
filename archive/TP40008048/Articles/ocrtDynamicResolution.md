---
title: "Dynamic Method Resolution"
book: "Objective-C Runtime Programming Guide"
chapterId: "TP40008048-CH102"
date: "2009-10-19"
description: "Describes the Objective-C 2.0 runtime support library."
identifier: "//apple_ref/doc/uid/TP40008048"
source: apple-archive
---

# Dynamic Method Resolution

> **Objective-C Runtime Programming Guide**
> Last updated: 2009-10-19

[Next](ocrtForwarding.html)[Previous](ocrtHowMessagingWorks.html)
        
        
        [](../index.html)
        
        # Dynamic Method Resolution
This chapter describes how you can provide an implementation of a method dynamically.

## Dynamic Method Resolution
There are situations where you might want to provide an implementation of a method dynamically. For example, the Objective-C declared properties feature (see [Declared Properties](../../ObjectiveC/Chapters/ocProperties.html#//apple_ref/doc/uid/TP30001163-CH17) in *[The Objective-C Programming Language](../../ObjectiveC/Introduction/introObjectiveC.html#//apple_ref/doc/uid/TP30001163)*) includes the `@dynamic` directive:

@dynamic propertyName;which tells the compiler that the methods associated with the property will be provided dynamically.

You can implement the methods `[resolveInstanceMethod:](https://developer.apple.com/documentation/objectivec/nsobject/1418500-resolveinstancemethod)` and `[resolveClassMethod:](https://developer.apple.com/documentation/objectivec/nsobject/1418889-resolveclassmethod)` to dynamically provide an implementation for a given selector for an instance and class method respectively.

An Objective-C method is simply a C function that take at least two arguments—`self` and `_cmd`. You can add a function to a class as a method using the function `[class_addMethod](https://developer.apple.com/documentation/objectivec/1418901-class_addmethod)`. Therefore, given the following function:

void dynamicMethodIMP(id self, SEL _cmd) {
```
    // implementation ....
```

```
}
```
you can dynamically add it to a class as a method (called `resolveThisMethodDynamically`) using `resolveInstanceMethod:` like this:

@implementation MyClass
```
+ (BOOL)resolveInstanceMethod:(SEL)aSEL
```

```
{
```

```
    if (aSEL == @selector(resolveThisMethodDynamically)) {
```

```
          class_addMethod([self class], aSEL, (IMP) dynamicMethodIMP, "v@:");
```

```
          return YES;
```

```
    }
```

```
    return [super resolveInstanceMethod:aSEL];
```

```
}
```

```
@end
```
Forwarding methods (as described in [Message Forwarding](ocrtForwarding.html#//apple_ref/doc/uid/TP40008048-CH105-SW1)) and dynamic method resolution are, largely, orthogonal. A class has the opportunity to dynamically resolve a method before the forwarding mechanism kicks in. If `[respondsToSelector:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intfm/NSObject/respondsToSelector:)` or `[instancesRespondToSelector:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/clm/NSObject/instancesRespondToSelector:)` is invoked, the dynamic method resolver is given the opportunity to provide an `IMP` for the selector first. If you implement `[resolveInstanceMethod:](https://developer.apple.com/documentation/objectivec/nsobject/1418500-resolveinstancemethod)` but want particular selectors to actually be forwarded via the forwarding mechanism, you return `NO` for those selectors.

## Dynamic Loading
An Objective-C program can load and link new classes and categories while it’s running. The new code is incorporated into the program and treated identically to classes and categories loaded at the start.

Dynamic loading can be used to do a lot of different things. For example, the various modules in the System Preferences application are dynamically loaded.

In the Cocoa environment, dynamic loading is commonly used to allow applications to be customized. Others can write modules that your program loads at runtime—much as Interface Builder loads custom palettes and the OS X System Preferences application loads custom preference modules. The loadable modules extend what your application can do. They contribute to it in ways that you permit but could not have anticipated or defined yourself. You provide the framework, but others provide the code.

Although there is a runtime function that performs dynamic loading of Objective-C modules in Mach-O files (`objc_loadModules`, defined in `objc/objc-load.h`), Cocoa’s `NSBundle` class provides a significantly more convenient interface for dynamic loading—one that’s object-oriented and integrated with related services. See the `[NSBundle](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/cl/NSBundle)` class specification in the Foundation framework reference for information on the `NSBundle` class and its use. See *OS X ABI Mach-O File Format Reference*** for information on Mach-O files.

        
            [Next](ocrtForwarding.html)[Previous](ocrtHowMessagingWorks.html)
        
         Copyright &#x00a9; 2009 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2009-10-19