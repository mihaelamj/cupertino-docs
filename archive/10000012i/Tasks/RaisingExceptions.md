---
title: "Throwing Exceptions"
book: "Exception Programming Topics"
chapterId: "20000058"
date: "2013-08-08"
description: "Explains how to raise and handle exceptions in Cocoa applications."
identifier: "//apple_ref/doc/uid/10000012i"
source: apple-archive
---

# Throwing Exceptions

> **Exception Programming Topics**
> Last updated: 2013-08-08

[Next](NestingExceptionHandlers.html)[Previous](HandlingExceptions.html)
        
        
        [](../index.html)
        
        # Throwing Exceptions
Once your program detects an exception, it must propagate the exception to code that handles it. This code is called the exception handler. This entire process of propagating an exception is referred to as "throwing an exception” (or "raising an exception" ). You throw (or raise) an exception by instantiating an `[NSException](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/cl/NSException)` object and then doing one of two things with it:

Using it as the argument of a `@throw` compiler directive

Sending it a `[raise](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/instm/NSException/raise)` message

The following example shows how you throw an exception using the `@throw` directive (the `raise` alternative is commented out):

NSException* myException = [NSException
```
        exceptionWithName:@"FileNotFoundException"
```

```
        reason:@"File Not Found on System"
```

```
        userInfo:nil];
```

```
@throw myException;
```

```
// [myException raise]; /* equivalent to above directive */
```
An important difference between `@throw` and `raise` is that the latter can be sent only to an `NSException` object whereas `@throw` can take other types of objects as its argument (such as string objects). Cocoa applications should `@throw` only `NSException` objects.

Typically you throw or raise an exception inside an exception-handling domain, which is a block of code marked off by the `@try` compiler directive.

See [Handling Exceptions](HandlingExceptions.html#//apple_ref/doc/uid/20000059-SW1) for details.

Within exception handling domains you can re-propagate exceptions caught by local exception handlers to higher-level handlers either by sending the `NSException` object another `raise` message or by using it with another `@throw` directive. Note that in  `@catch` exception-handling blocks you can rethrow the exception without explicitly specifying the exception object, as in the following example:

@try {
```
    NSException *e = [NSException
```

```
        exceptionWithName:@"FileNotFoundException"
```

```
        reason:@"File Not Found on System"
```

```
        userInfo:nil];
```

```
    @throw e;
```

```
}
```

```
@catch(NSException *e) {
```

```
    @throw; // rethrows e implicitly
```

```
}
```
There is a subtle aspect of behavior involving re-thrown exceptions. The `@finally` block associated with the local `@catch` exception handler is executed before the `@throw` causes the next-higher exception handler to be invoked. In a sense, the `@finally` block is executed as an early side effect of the `@throw` statement. This behavior has implications for [memory management](../../../../General/Conceptual/DevPedia-CocoaCore/MemoryManagement.html#//apple_ref/doc/uid/TP40008195-CH27) (see [Exception Handling and Memory Management ](HandlingExceptions.html#//apple_ref/doc/uid/20000059-SW7)). 

        
            [Next](NestingExceptionHandlers.html)[Previous](HandlingExceptions.html)
        
         Copyright &#x00a9; 2002, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-08-08