---
title: "Predefined Exceptions"
book: "Exception Programming Topics"
chapterId: "20000057"
date: "2013-08-08"
description: "Explains how to raise and handle exceptions in Cocoa applications."
identifier: "//apple_ref/doc/uid/10000012i"
source: apple-archive
---

# Predefined Exceptions

> **Exception Programming Topics**
> Last updated: 2013-08-08

[Next](../Tasks/ControllingAppResponse.html)[Previous](UncaughtExceptions.html)
        
        
        [](../index.html)
        
        # Predefined Exceptions
Cocoa predefines a number of generic exception names to identify exceptions that you can handle in your own code or even raise and re-raise. You can also create and use custom exception names. The generic exception names are string constants defined in `NSException.h` and documented in *[Foundation Constants Reference](https://developer.apple.com/documentation/foundation/foundation_constants)*. These constants include the following:

`NSGenericException`

`NSRangeException`

`NSInvalidArgumentException`

`NSInternalInconsistencyException`

`NSObjectInaccessibleException`

`NSObjectNotAvailableException`

`NSDestinationInvalidException`

`NSPortTimeoutException`

`NSInvalidSendPortException`

`NSInvalidReceivePortException`

`NSPortSendException`

`NSPortReceiveException`

In addition to the generic exception names, some subsystems of Cocoa define their own exception names, such as `NSInconsistentArchiveException` and `NSFileHandleOperationException`. These are also documented in *[Foundation Constants Reference](https://developer.apple.com/documentation/foundation/foundation_constants)*.

You can identify caught exceptions in your exception handler by comparing the exception's name with these predefined names. Then you can either handle the exception or, if it isn't one you are interested in, re-raise it. Note that all predefined exceptions begin with the prefix "NS", so you should avoid using the same prefix when creating new exception names.

        
            [Next](../Tasks/ControllingAppResponse.html)[Previous](UncaughtExceptions.html)
        
         Copyright &#x00a9; 2002, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-08-08