---
title: "Introduction"
book: "Error Handling Programming Guide"
chapterId: "TP40001806-CH201"
date: "2011-01-07"
description: "Describes NSError objects, related Application Kit support for error handling, and how to use these features in your code."
identifier: "//apple_ref/doc/uid/TP40001806"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Introduction

> **Error Handling Programming Guide**
> Last updated: 2011-01-07

[Next](../ErrorObjectsDomains/ErrorObjectsDomains.html)
        
        
        [](../index.html)
        
        # Introduction to Error Handling Programming Guide For Cocoa
Every program must deal with errors as they occur at runtime. The program, for example, might not be able to open a file, or perhaps it cannot parse an XML document. Often errors such as these require the program to inform the user about them. And perhaps the program can attempt to get around the problem causing the error.

[Cocoa](../../../../General/Conceptual/DevPedia-CocoaCore/Cocoa.html#//apple_ref/doc/uid/TP40008195-CH9) (and Cocoa Touch) offer developers programmatic tools for these tasks: the `[NSError](https://developer.apple.com/documentation/foundation/nserror)` class in Foundation and new methods and mechanisms in the Application Kit to support error handling in applications. An `NSError` object encapsulates information specific to an error, including the domain (subsystem) originating the error and the localized strings to present in an error alert. With an application there is also an architecture allowing the various objects in an application to refine the information in an error object and perhaps to recover from the error. 

> **Important:** The `NSError` class is available on both OS X and iOS. However, the error-responder and error-recovery APIs and mechanisms are available only in the Application Kit (OS X).

You should read this document to understand the `NSError` API and architecture and how to use them.

## Organization of This Document
*Error Handling Programming Guide for Cocoa* has the following articles:

[Error Objects, Domains, and Codes](../ErrorObjectsDomains/ErrorObjectsDomains.html#//apple_ref/doc/uid/TP40001806-CH202-CJBGAIBJ) describes the attributes of an `NSError` object, particularly its domain and error code, and discusses the possible contents of an error object’s “user info” dictionary, including localized message strings and underlying errors.

[Using and Creating Error Objects](../CreateCustomizeNSError/CreateCustomizeNSError.html#//apple_ref/doc/uid/TP40001806-CH204-BAJIIGCC) explains how to evaluate an error, how to display an error message using an `NSError` object, and how to implement methods that return an `NSError` object by reference.

[Error Responders and Error Recovery](../ErrorRespondRecover/ErrorRespondRecover.html#//apple_ref/doc/uid/TP40001806-CH203-BAJCBIFC) describes the Application Kit architecture for passing error objects up a chain of objects in an application, giving each object a chance to customize the error before it is presented. It also discusses the role of the recovery attempter, an object designated to attempt a recovery from an error if the user requests it.

[Handling Received Errors](../HandleReceivedError/HandleReceivedError.html#//apple_ref/doc/uid/TP40001806-CH205-BAJEHAJC) discusses how, in the chain of error-responder objects, you handle a received error and customize it.

[Recovering From Errors](../RecoverFromErrors/RecoverFromErrors.html#//apple_ref/doc/uid/TP40001806-CH206-BCIDEGGF) explains the procedure for attempting a user-requested recovery from an error.

The two chapters relevant to iOS are [Error Objects, Domains, and Codes](../ErrorObjectsDomains/ErrorObjectsDomains.html#//apple_ref/doc/uid/TP40001806-CH202-CJBGAIBJ) and [Using and Creating Error Objects](../CreateCustomizeNSError/CreateCustomizeNSError.html#//apple_ref/doc/uid/TP40001806-CH204-BAJIIGCC).

## See Also
“[The Document Architecture Supports Robust Error Handling](../../../../DataManagement/Conceptual/DocBasedAppProgrammingGuideForOSX/StandardBehaviors/StandardBehaviors.html#//apple_ref/doc/uid/TP40011179-CH5-SW32)" in *[Document-Based App Programming Guide for Mac](../../../../DataManagement/Conceptual/DocBasedAppProgrammingGuideForOSX/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011179)* offers valuable advice for subclasses that override methods with a by-reference `[NSError](https://developer.apple.com/documentation/foundation/nserror)` parameter.

Types of Dialogs and When to Use Them  offers advice on the form and content of alerts in OS X. *iOS Human Interface Guidelines* offers similar advice for alerts on iOS. You should consult these guidelines before composing your error messages. Also take a look at the following documents discussing areas of Cocoa programming related to error handling and the presentation of error messages:

*[Assertions and Logging Programming Guide](../../Assertions/Assertions.html#//apple_ref/doc/uid/10000014i)* (both platforms)

*[Dialogs and Special Panels](../../Dialog/Dialog.html#//apple_ref/doc/uid/10000071i)* (alerts on OS X)

*[Sheet Programming Topics](../../Sheets/Sheets.html#//apple_ref/doc/uid/10000002i)* (OS X)

*[UIAlertView Class Reference](https://developer.apple.com/documentation/uikit/uialertview)* (iOS)

*[Exception Programming Topics](../../Exceptions/Exceptions.html#//apple_ref/doc/uid/10000012i)* discusses how to raise and handle exceptions. [Exception Handling](../../ObjectiveC/Chapters/ocExceptionHandling.html#//apple_ref/doc/uid/TP30001163-CH13) in *[The Objective-C Programming Language](../../ObjectiveC/Introduction/introObjectiveC.html#//apple_ref/doc/uid/TP30001163)* describes the compiler directives `@try`, `@catch`, `@throw`, and `@finally`, which are used in exception handling. 

        
            [Next](../ErrorObjectsDomains/ErrorObjectsDomains.html)
        
         Copyright &#x00a9; 2005, 2011 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2011-01-07