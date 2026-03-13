---
title: "Introduction"
book: "Exception Programming Topics"
chapterId: "10000012"
date: "2013-08-08"
description: "Explains how to raise and handle exceptions in Cocoa applications."
identifier: "//apple_ref/doc/uid/10000012i"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Introduction

> **Exception Programming Topics**
> Last updated: 2013-08-08

[Next](Articles/ExceptionsAndCocoaFrameworks.html)
        
        
        [](index.html)
        
        # Introduction to Exception Programming Topics for Cocoa
This document discusses how to raise and handle exceptions: special conditions that interrupt the normal flow of program execution. The Objective-C directives and Foundation API for exceptions are available on iOS and OS X.

> **Important:** You should reserve the use of exceptions for programming or unexpected runtime errors such as out-of-bounds collection access, attempts to mutate immutable objects, sending an invalid message, and losing the connection to the window server. You usually take care of these sorts of errors with exceptions when an application is being created rather than at runtime.

If you have an existing body of code (such as third-party library) that uses exceptions to handle error conditions, you may use the code as-is in your Cocoa application. But you should ensure that any expected runtime exceptions do not escape from these subsystems and end up in the caller’s code. For example, a parsing library might use exceptions internally to indicate problems and enable a quick exit from a parsing state that could be deeply recursive; however, you should take care to catch such exceptions at the top level of the library and translate them into an appropriate return code or state.

Instead of exceptions, error objects (`[NSError](https://developer.apple.com/documentation/foundation/nserror)`) and the Cocoa error-delivery mechanism are the recommended way to communicate expected errors in Cocoa applications. For further information, see *[Error Handling Programming Guide](../ErrorHandlingCocoa/ErrorHandling/ErrorHandling.html#//apple_ref/doc/uid/TP40001806)*.

## Organization of This Document
This document contains the following articles:

[Exceptions and the Cocoa Frameworks](Articles/ExceptionsAndCocoaFrameworks.html#//apple_ref/doc/uid/TP40009045-SW1) describes `NSException` objects and their general use with the Cocoa frameworks.

[Handling Exceptions](Tasks/HandlingExceptions.html#//apple_ref/doc/uid/20000059-BBCHGJIJ) describes how to handle an exception using the compiler directives `@try`, `@catch`, and `@finally`.

[Throwing Exceptions](Tasks/RaisingExceptions.html#//apple_ref/doc/uid/20000058-BBCCFIBF) describes how to throw (raise) an exception.

[Nesting Exception Handlers](Tasks/NestingExceptionHandlers.html#//apple_ref/doc/uid/20000060-CJBBFDIF) describes how exception handlers can be nested.

[Predefined Exceptions](Concepts/PredefinedExceptions.html#//apple_ref/doc/uid/20000057-BCIGHECA) describes where to find exceptions defined by Cocoa.

[Uncaught Exceptions](Concepts/UncaughtExceptions.html#//apple_ref/doc/uid/20000056-BAJDDGGD) describes what happens to an exception not caught by an exception handler.

[Controlling a Program’s Response to Exceptions](Tasks/ControllingAppResponse.html#//apple_ref/doc/uid/20000473-BBCHGJIJ) describes how to use the Exception Handling framework for monitoring and controlling the behavior of Cocoa programs in response to various types of exceptions.

[Exceptions in 64-Bit Executables](Articles/Exceptions64Bit.html#//apple_ref/doc/uid/TP40009044-SW1) describes zero-cost `@try` blocks and C++ interoperability in 64-bit executables.

## See Also
For information on originating, handling, and recovering from expected runtime errors, see *[Error Handling Programming Guide](../ErrorHandlingCocoa/ErrorHandling/ErrorHandling.html#//apple_ref/doc/uid/TP40001806)*. Also see the related document,*[Assertions and Logging Programming Guide](../Assertions/Assertions.html#//apple_ref/doc/uid/10000014i)*, for information on the Foundation framework's support for making assertions and logging error information.

        
            [Next](Articles/ExceptionsAndCocoaFrameworks.html)
        
         Copyright &#x00a9; 2002, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-08-08