---
title: "Uncaught Exceptions"
book: "Exception Programming Topics"
chapterId: "20000056"
date: "2013-08-08"
description: "Explains how to raise and handle exceptions in Cocoa applications."
identifier: "//apple_ref/doc/uid/10000012i"
source: apple-archive
---

# Uncaught Exceptions

> **Exception Programming Topics**
> Last updated: 2013-08-08

[Next](PredefinedExceptions.html)[Previous](../Tasks/NestingExceptionHandlers.html)
        
        
        [](../index.html)
        
        # Uncaught Exceptions
If an exception is not caught, it is intercepted by a function called the uncaught exception handler.  The uncaught exception handler always causes the program to exit but may perform some task before this happens.

The default uncaught exception handler logs a message to the console before it exits the program. On OS X, if the application was launched from the shell, the log messages are sent to the Terminal window. 

You can set a custom function as the uncaught exception handler using the `[NSSetUncaughtExceptionHandler](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSSetUncaughtExceptionHandler)` function; you can obtain the current uncaught exception handler with the  `[NSGetUncaughtExceptionHandler](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSGetUncaughtExceptionHandler)` function.

> **Note:** Exceptions on the main thread of a Cocoa application do not typically rise to the level of the uncaught exception handler because the [global](../../../../General/Conceptual/DevPedia-CocoaCore/Singleton.html#//apple_ref/doc/uid/TP40008195-CH49) application object catches all such exceptions.

        
            [Next](PredefinedExceptions.html)[Previous](../Tasks/NestingExceptionHandlers.html)
        
         Copyright &#x00a9; 2002, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-08-08