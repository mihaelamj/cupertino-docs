---
title: "Error Objects, Domains, and Codes"
book: "Error Handling Programming Guide"
chapterId: "TP40001806-CH202"
date: "2011-01-07"
description: "Describes NSError objects, related Application Kit support for error handling, and how to use these features in your code."
identifier: "//apple_ref/doc/uid/TP40001806"
source: apple-archive
---

# Error Objects, Domains, and Codes

> **Error Handling Programming Guide**
> Last updated: 2011-01-07

[Next](../CreateCustomizeNSError/CreateCustomizeNSError.html)[Previous](../ErrorHandling/ErrorHandling.html)
        
        
        [](../index.html)
        
        # Error Objects, Domains, and Codes
Cocoa programs use `[NSError](https://developer.apple.com/documentation/foundation/nserror)` objects to convey information about runtime errors that users need to be informed about. In most cases, a program displays this error information in a dialog or sheet. But it may also interpret the information and either ask the user to attempt to recover from the error or attempt to correct the error on its own. 

The core [attributes](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectModeling.html#//apple_ref/doc/uid/TP40008195-CH41) of an `NSError` object—or, simply, an error object—are an error domain, a domain-specific error code, and a “user info” [dictionary](../../../../General/Conceptual/DevPedia-CocoaCore/Collection.html#//apple_ref/doc/uid/TP40008195-CH10) containing objects related to the error, most significantly description and recovery strings. This chapter explains the reason for error objects, describes their attributes, and discusses how you use them in Cocoa code.

## Why Have Error Objects?
Because they are objects, instances of the `[NSError](https://developer.apple.com/documentation/foundation/nserror)` class have several advantages over simple error codes and error strings. They encapsulate several pieces of error information at once, including localized error strings of various kinds. `NSError` objects can also be [archived](../../../../General/Conceptual/DevPedia-CocoaCore/Archiving.html#//apple_ref/doc/uid/TP40008195-CH1) and [copied](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCopying.html#//apple_ref/doc/uid/TP40008195-CH38), and they can be passed around in an application and modified. And although `NSError` is not an abstract class (and thus can be used directly) you can extend the `NSError` class through subclassing.

Because of the notion of layered error domains, `NSError` objects can embed errors from underlying subsystems and thus provide more detailed and nuanced information about an error. Error objects also provide a mechanism for error recovery by holding a reference to an object designated as the recovery attempter for the error.

## Error Domains
For largely historical reasons, errors codes in OS X are segregated into domains. For example, Carbon error codes, which are typed as `OSStatus`, have their origin in versions of the Macintosh operating system predating OS X. On the other hand, POSIX error codes derive from the various POSIX-conforming “flavors” of UNIX, such as BSD. The Foundation framework declares in `NSError.h` the following string constants for the four major error domains:

`NSMachErrorDomain`

`NSPOSIXErrorDomain`

`NSOSStatusErrorDomain`

`NSCocoaErrorDomain`

The above sequence of domain constants indicates the general layering of the domains, with the Mach error domain at the lowest layer. You get the domain of an error by sending an `[NSError](https://developer.apple.com/documentation/foundation/nserror)` object a `[domain](https://developer.apple.com/documentation/foundation/nserror/1413924-domain)` message.

In addition to the four major domains, there are error domains that are specific to [frameworks](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56) or even to groups of classes or individual classes. For example, the Web Kit framework has its own domain for errors in its Objective-C implementation, `WebKitErrorDomain`. Within the Foundation framework, the URL classes have their own error domain (`NSURLErrorDomain`) as do the XML classes (`NSXMLParserErrorDomain`). The NSStream class itself defines two error domains, one for SSL errors and the other for SOCKS errors.

The Cocoa error domain (`NSCocoaErrorDomain`) includes all error codes for the Cocoa frameworks—except, of course, for error codes in class-specific domains of those frameworks. These frameworks include not only Foundation, UIKit, and Application Kit, but Core Data and potentially other Objective-C frameworks. (Error domains within the Cocoa frameworks that are separate from the Cocoa error domain were defined before the latter was introduced.)

Domains serve several useful purposes. They give Cocoa programs a way to identify the OS X subsystem that is detecting an error. They also help to prevent collisions between error codes from different subsystems with the same numeric value. In addition, domains allow for a causal relationship between error codes based on the layering of subsystems; for example, an error in the `NSOSStatusErrorDomain` may have an underlying error in the `NSMachErrorDomain`.

You can create your own error domains and error codes for use in your own frameworks, or even in your own applications. It is recommended that the string constant for the domain be of the form `com.`*company*`.`*framework_or_app*`.ErrorDomain`. 

## Error Codes
An error code identifies a particular error in a particular domain. It is a signed integer assigned as the value of a program symbol. You get the error code by sending an `[NSError](https://developer.apple.com/documentation/foundation/nserror)` object a `[code](https://developer.apple.com/documentation/foundation/nserror/1409165-code)` message. As listed in Table 1-1, error codes are declared and documented in one or more header files for each major domain.

**Table 1-1**  Header files for error codes in major domainsDomain

Header file

Mach

`/usr/include/mach/kern_return.h`

POSIX

`/usr/include/sys/errno.h`

Carbon (`OSStatus`)

`/System/Library/Frameworks/CoreServices.framework/Frameworks/CarbonCore.framework/Headers/MacErrors.h`

Cocoa

See Table 1-2.

Table 1-2 lists the frameworks and header files where error codes in the Cocoa domain are currently declared. 

**Table 1-2**  Header files declaring error codesFramework/Header

Description

`<Foundation/FoundationErrors.h>`

Generic Foundation error codes

`<AppKit/AppKitErrors.h>`

Generic Application Kit error codes

`<CoreData/CoreDataErrors.h>`

Core Data error codes

To give an idea of how you might test for and act upon errors, let’s say you want to test for underlying POSIX errors during an operation that writes to a file. (Underlying errors are explained in [Underlying Error](#//apple_ref/doc/uid/TP40001806-CH202-CJBDCIJJ).) If you consulted the POSIX error codes declared in `/usr/include/sys/errno.h`, you would see a list similar to Listing 1-1.

**Listing 1-1**  Part of the POSIX error-code declarations `(errno.h`)

#define EPERM       1       /* Operation not permitted */
```
#define ENOENT      2       /* No such file or directory */
```

```
#define ESRCH       3       /* No such process */
```

```
#define EINTR       4       /* Interrupted system call */
```

```
#define EIO         5       /* Input/output error */
```

```
#define ENXIO       6       /* Device not configured */
```

```
#define E2BIG       7       /* Argument list too long */
```

```
#define ENOEXEC     8       /* Exec format error */
```

```
#define EBADF       9       /* Bad file descriptor */
```

```
#define ECHILD      10      /* No child processes */
```

```
#define EDEADLK     11      /* Resource deadlock avoided */
```

```
                            /* 11 was EAGAIN */
```

```
#define ENOMEM      12      /* Cannot allocate memory */
```

```
#define EACCES      13      /* Permission denied */
```

```
#define EFAULT      14      /* Bad address *#H
```
You could choose the error conditions you want to test for and use them in code similar to that in Listing 1-2. 

**Listing 1-2**  Testing for particular error codes in a specific domain

// underError is underlying-error object of a Cocoa-domain error
```
if ( [[underError domain] isEqualToString:NSPOSIXErrorDomain] ) {
```

```
        switch([underError code]) {
```

```
            case EIO:
```

```
            {
```

```
                // handle POSIX I/O error
```

```
            }
```

```
            case EACCES:
```

```
            {
```

```
                // handle POSIX permissions error
```

```
            {
```

```
        // etc.
```

```
        }
```

```
}
```
You may declare you own error codes for use by your own applications or frameworks, but the error codes should belong to your own domain. You should never add error codes to an existing domain that you do not “own.”

## The User Info Dictionary
Every `[NSError](https://developer.apple.com/documentation/foundation/nserror)` object has a “user info” [dictionary](../../../../General/Conceptual/DevPedia-CocoaCore/Collection.html#//apple_ref/doc/uid/TP40008195-CH10) to hold error information beyond domain and code. You access this dictionary by sending a `[userInfo](https://developer.apple.com/documentation/foundation/nserror/1411580-userinfo)` message to an `NSError` object. The advantage of an `NSDictionary` object over another kind of container object is that it is flexible; it can even carry custom information about an error. But all user info dictionaries contain (or can contain) several predefined string and object values related to an error.

### Localized Error Information
An important role for `NSError` objects is to contain error information that programs can display in an alert dialog or sheet. This information is usually stored in the user info dictionary as strings in several categories: description, failure reason, recovery suggestion, and recovery options. (See [Figure 1-1](#//apple_ref/doc/uid/TP40001806-CH202-BBCHIDHG) for the placement of these strings on an alert.) When you create an `NSError` object, you should insert localized strings into the dictionary, unless you want to compute them lazily.

> **Note:** Don’t expect the user info dictionary of every error object to contain localized strings. A subclass of `NSError`, for example, could override `localizedDescription` to compose these strings on-the-fly from the error domain, code, and context instead of storing them.

You can usually access the localized information associated with an `NSError` object in one of two ways. You can send `objectForKey:` to the user info dictionary, specifying the appropriate key. Or you can send an equivalent message to the `NSError` object. However, you should send the message rather than use the dictionary key to access a localized string. The error object might not store the string in the dictionary, instead choosing to compose it dynamically. The dictionary is designed to be a fallback mechanism, not the sole repository of error strings. Use the dictionary keys instead to store your own strings in the user info dictionary.

The following summaries include both the dictionary key and the method used to access the localized string:

	Error descriptionThe main description of the error, appearing in a larger, bold type face. It often includes the failure reason. If no error description is present in the user info dictionary, `NSError` either constructs one from the error domain and code or (when the domain is well-known , such as `NSCocoaErrorDomain`), attempts to fetch a suitable string from a function or method within that domain. .

User info key: `NSLocalizedDescriptionKey`

Method: `localizedDescription` (never returns `nil`)

Failure reasonA brief sentence that explains the reason why the error occurred. It is typically part of the error description. Methods such as `presentError:` do not automatically display the failure reason because it is already included in the error description. The failure reason is for clients that only want to display the reason for the failure.

User info key: `NSLocalizedFailureReasonErrorKey`

Method: `localizedFailureReason` (can return `nil`)

**Note:** An example can help to clarify the relationship between error description and failure reason. An error object has an error description of “File could not be saved because the disk is full.” The accompanying failure reason is “The disk is full.”

Recovery suggestionA secondary sentence that ideally tells users what they can do to recover from the error. It appears beneath the error description in a lighter type face. If the recovery suggestion refers to the buttons of the error alert, it should use the same titles as specified for recovery options (`NSLocalizedRecoveryOptionsErrorKey`). You may use this string as a purely informative message, supplementing the error description and failure reason.

User info key: `NSLocalizedRecoverySuggestionErrorKey`

Method: `localizedRecoverySuggestion` (can return `nil`)

Recovery optionsAn array of titles (as strings) for the buttons of the error alert. By default, alert sheets and dialogs for error messages have only the “OK” button for dismissing the alert. The first string in the array is the title of the rightmost button, the next string is the title of the button just to the left of the first, and so on. Note that if a recovery attempter is specified for the error object, the recovery-options array should contain more than one string. The recovery attempter accesses the recovery options in order to interpret user choices.

User info key: `NSLocalizedRecoveryOptionsErrorKey`

Method: `localizedRecoveryOptions` (if returns `nil`, implies a single “OK button)

**Figure 1-1**  The localized strings of an NSError object
> **Note:** Beginning with OS X version 10.4, you can use the `alertWithError:`class method of `NSAlert` as a convenience for creating `NSAlert` objects to use when displaying alert dialogs or sheets. The method extracts the localized information from the passed-in `NSError` object for its message text, informative text, and button titles. You may also use the `presentError:` message to display error alerts.

To internationalize your error strings, create a `.strings` file for each localization and place the file in an appropriately named `.lproj` subdirectory of your bundle’s `Resources` directory. Then use one of the `NSLocalizedString` macros to add localized strings to the user info dictionary of an `NSError` object. For more on [internationalization](../../../../General/Conceptual/DevPedia-CocoaCore/Internationalization.html#//apple_ref/doc/uid/TP40008195-CH23) and string localization, see *[Internationalization and Localization Guide](../../../../MacOSX/Conceptual/BPInternational/Introduction/Introduction.html#//apple_ref/doc/uid/10000171i)*.

> **Note:** For many error objects in the Cocoa error domain, the localization is performed on demand; in these cases, the localized values are not stored in the user info dictionary.

### The Recovery Attempter
An `[NSError](https://developer.apple.com/documentation/foundation/nserror)` object’s user info dictionary can also contain a recovery attempter. A recovery attempter is an object that implements one or more methods of the `NSErrorRecoveryAttempting` [informal protocol](../../../../General/Conceptual/DevPedia-CocoaCore/Protocol.html#//apple_ref/doc/uid/TP40008195-CH45). In many cases, this is the same object that creates the `NSError` object, but it can be any other object that may know how to recover from a particular error.

If a recovery attempter has been specified for an `NSError` object, and multiple recovery options have also been specified, when the error alert is displayed and the user selects a recovery option, the recovery attempter is given a chance to recover from the error. You access the recovery attempter by sending `recoveryAttempter` to the `NSError` object. You can add a recovery attempter to the user info dictionary using the key `NSRecoveryAttempterErrorKey`.

For more on the recovery-attempter object and its role in error handling, see [Error Responders and Error Recovery](../ErrorRespondRecover/ErrorRespondRecover.html#//apple_ref/doc/uid/TP40001806-CH203-BAJCBIFC).

### Underlying Error
The user info dictionary can sometimes include another `[NSError](https://developer.apple.com/documentation/foundation/nserror)` object that represents an error in a subsystem underlying the error represented by the containing `NSError`. You can query this underlying error object to obtain more specific information about the cause of the error.

You access the underlying error object by using the `NSUnderlyingErrorKey` dictionary key.

### Domain-Specific Keys
Many of the various error domains specify keys for accessing particular items of information from the user info dictionary. This information supplements the other information in the error object. For example, the Cocoa domain defines the keys `NSStringEncodingErrorKey`, `NSURLErrorKey`, and `NSFilePathErrorKey`.

Check the header files or documentation of error domains to find out what domain-specific keys they declare.

        
            [Next](../CreateCustomizeNSError/CreateCustomizeNSError.html)[Previous](../ErrorHandling/ErrorHandling.html)
        
         Copyright &#x00a9; 2005, 2011 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2011-01-07