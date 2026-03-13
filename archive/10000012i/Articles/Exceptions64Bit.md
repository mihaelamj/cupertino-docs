---
title: "Exceptions in 64-Bit Executables"
book: "Exception Programming Topics"
chapterId: "TP40009044"
date: "2013-08-08"
description: "Explains how to raise and handle exceptions in Cocoa applications."
identifier: "//apple_ref/doc/uid/10000012i"
source: apple-archive
---

# Exceptions in 64-Bit Executables

> **Exception Programming Topics**
> Last updated: 2013-08-08

[Next](../RevisionHistory.html)[Previous](../Tasks/ControllingAppResponse.html)
        
        
        [](../index.html)
        
        # Exceptions in 64-Bit Executables
The Objective-C runtime has reimplemented the exception mechanism for 64-bit executables to provide zero-cost `@try` blocks and interoperability with C++ exceptions. 

## Zero-Cost @try Blocks
64-bit processes that enter a zero-cost `@try` block incur no performance penalty. This is unlike the mechanism for 32-bit processes, which calls `setjmp()` and performs additional “bookkeeping”. However, throwing an exception is much more expensive in 64-bit executables. For best performance in 64-bit, you should throw exceptions only when absolutely necessary.

## C++ Interoperability
In 64-bit processes, Objective-C exceptions (`[NSException](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/cl/NSException)`) and C++ exception are interoperable. Specifically, C++ destructors and Objective-C `@finally` blocks are honored when the exception mechanism unwinds an exception. In addition, default catch clauses—that is, `catch(...)` and `@catch(...)`—can catch and rethrow any exception

On the other hand, an Objective-C catch clause taking a dynamically typed exception object (`@catch(id exception)`) can catch any Objective-C exception, but cannot catch any C++ exceptions. So, for interoperability, use `@catch(...)` to catch every exception and `@throw;` to rethrow caught exceptions. In 32-bit, `@catch(...)` has the same effect as `@catch(id exception)`.

        
            [Next](../RevisionHistory.html)[Previous](../Tasks/ControllingAppResponse.html)
        
         Copyright &#x00a9; 2002, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-08-08