---
title: "Error Responders and Error Recovery"
book: "Error Handling Programming Guide"
chapterId: "TP40001806-CH203"
date: "2011-01-07"
description: "Describes NSError objects, related Application Kit support for error handling, and how to use these features in your code."
identifier: "//apple_ref/doc/uid/TP40001806"
source: apple-archive
---

# Error Responders and Error Recovery

> **Error Handling Programming Guide**
> Last updated: 2011-01-07

[Next](../HandleReceivedError/HandleReceivedError.html)[Previous](../CreateCustomizeNSError/CreateCustomizeNSError.html)
        
        
        [](../index.html)
        
        # Error Responders and Error Recovery
As [Why Have Error Objects?](../ErrorObjectsDomains/ErrorObjectsDomains.html#//apple_ref/doc/uid/TP40001806-CH202-BBCBECJD) points out, `[NSError](https://developer.apple.com/documentation/foundation/nserror)` objects bring considerable advantages to Cocoa programming. But the Cocoa frameworks also give `NSError` objects a prominent role to play in architectures for error presentation and error recovery. These architectures enhance the usefulness of error objects. They make it possible for Cocoa applications to present users with a richer and more customizable range of messages, and to attempt recovery from errors as well as informing users of them.

> **Note:** The error-presentation and error-recovery architectures described in this chapter are available only on the Mac.

## The Error-Responder Chain
The Application Kit, largely through the `[NSResponder](https://developer.apple.com/documentation/appkit/nsresponder)` class, defines a mechanism known as the responder chain by which events and action messages in an application are passed up the view hierarchy to windows and eventually to the application object. The Application Kit defines a similar chain of objects for error handling and presentation.

To initiate the journey of an `NSError` object up the error-responder chain, you can send one of two messages to any object in the chain:

`[presentError:](https://developer.apple.com/documentation/appkit/nsresponder/1531294-presenterror)` — for error messages displayed in application-modal alert dialogs

`[presentError:modalForWindow:delegate:didPresentSelector:contextInfo:](https://developer.apple.com/documentation/appkit/nsresponder/1534705-presenterror)` — for error messages displayed in document-modal alert sheets

Although these methods are declared by the `NSResponder` class, you may also send them to objects of the `[NSDocument](https://developer.apple.com/documentation/appkit/nsdocument)` and `[NSDocumentController](https://developer.apple.com/documentation/appkit/nsdocumentcontroller)` classes. (The `NSResponder` class is the superclass, of course, of the `NSView`, `NSWindow`, `NSApplication`, and `NSWindowController` classes.)

The default behavior of both the `presentError:...` methods—except for the `NSApplication` implementation—is to send `willPresentError:` to `self` before forwarding the `presentError:..` message to the next object in the chain. Subclasses can implement the `willPresentError:` method to inspect the passed-in `NSError` object and return a customized object. Subclasses might want to do this if they know more than their superclass about the conditions giving rise to the error or if they know best how to recover from it. 

For the purposes of illustration, assume that a view object well down the view hierarchy receives the `presentError:` message. As Figure 3-1 shows, it sends `willPresentError:` to `self` and then sends `presentError:` to its superview, passing it any modified `NSError` object. The superviews of the originating view do the same thing until finally the window’s content view sends the `presentError:` message to its window object.

**Figure 3-1**  The error-responder chain—part oneThe `presentError:` message proceeds up the chain of error responders in this fashion until it reaches the global application object, NSApp. As Figure 3-2 depicts, NSApp sends the `application:willPresentError:` message to its delegate, giving it the same opportunity as the subclass objects in the chain to inspect the error object and possibly modify it—but without the need for a custom subclass. When the delegate returns, NSApp displays the error as (in this case) an alert dialog.

**Figure 3-2**  The error-responder chain — part two
> **Important:** Overriding the `presentError:modalForWindow:delegate:didPresentSelector:contextInfo:` method or `presentError:` method is not recommended.

The exact sequence of objects in the error-responder chain varies according to the type of application. For document-based applications, the error-responder chain includes document objects, window controllers, and document controllers as well as views, windows, and NSApp (Figure 3-3).

**Figure 3-3**  Error-responder chain for document-based applicationsSome Cocoa applications are not document-based but still use one or more window controllers. Figure 3-4 shows the sequence of objects in this error-responder chain.

**Figure 3-4**  Error-responder chain for non-document applications with window controllersFinally, simple Cocoa applications—those that are not document-based and that don’t use window controllers—have an error-responder sequence as depicted in Figure 3-5.

**Figure 3-5**  Error-responder chain for simple (non-document) applications## Error Customization
As described in the preceding section, all along the error-responder chain custom subclasses of objects in the chain are given the opportunity to inspect and customize an `[NSError](https://developer.apple.com/documentation/foundation/nserror)` object if they implement the `willPresentError:` method. Near the end of the chain the application delegate has the same opportunity in `application:willPresentError:`. What kind of tests and customizations can take place in these methods?

In either method, you probably first want to determine what the error is. When doing this, test the `NSError` object’s domain and error code against the constants that are probably related to the error condition. Do not evaluate the description or recovery strings as these can vary, especially when they are localized. You might also narrow down the cause of the error by using domain-specific keys to extract various pieces of information from the user info dictionary.

> **Important:** You should always special-case test for the `NSUserCancelledError` error code (in the `NSCocoaErrorDomain`). This code indicates that the user cancelled the operation (for example, by pressing Command-period). In this case, you should not display any error dialog.

You might also want to find out if the error object has an underlying error; you can access this object from the user info dictionary with the `NSUnderlyingErrorKey ` key. If there is an underlying error, and this object has a failure reason in its user info dictionary, you can append this localized string to the error description to create a more informative description. 

If you decide that you know how to recover from the error, you can add an object to the user info dictionary as the recovery attempter. For a recovery attempter to be effective, it must satisfy the requirements summarized in [Error Recovery](#//apple_ref/doc/uid/TP40001806-CH203-BAJJHBED). 

If you are customizing a received `NSError` object to have a custom error domain and error code, you may choose to store the original error in the user info dictionary as an underlying error. Use the key `NSUnderlyingErrorKey` for this purpose (or override the `recoveryAttempter` method).

You cannot modify a received `NSError` object because the class provides no setter methods and the user info dictionary is immutable. When customizing an error, you must create a new `NSError` object, initializing with new data plus data from the old error object that you want to carry over. See [Using and Creating Error Objects](../CreateCustomizeNSError/CreateCustomizeNSError.html#//apple_ref/doc/uid/TP40001806-CH204-BAJIIGCC) for explicit instructions and examples.

## Error Recovery
A recovery attempter is an object designated to attempt, upon user request, a recovery from a specific error. For example, say that a program cannot save a file because it is locked. The recovery attempter could try to unlock it first before overwriting it.

The error recovery mechanism is similar to the [delegation](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14) design pattern in that a designated object —the recovery attempter—is asked to respond to a user action. An `[NSError](https://developer.apple.com/documentation/foundation/nserror)` object can encapsulate a recovery attempter and recovery options, which is an array of button titles to display in the error alert. Among the button titles is one requesting error recovery. When an error alert is displayed and the user clicks a button, the application sends a message to the recovery attempter, passing it the index of the button that was clicked. If the “recover” button was clicked, the recovery attempter tries to complete the operation in a way that avoids the error or fixes the condition that gives rise to it. Finally, the recovery attempter informs either the application object or the document-modal sheet delegate whether it was successful. 

There are three requirements for error recovery to occur as a result of a user choice:

The recovery-attempter object must implement one of the `NSErrorRecoveryAttempting` informal protocol methods: `attemptRecoveryFromError:optionIndex:delegate:didRecoverSelector:contextInfo: ` or `attemptRecoveryFromError:optionIndex:`, depending on whether the error alert is document-modall (sheet) or application-modal (dialog), respectively.

The `recoveryAttempter` method must return a suitable object. To ensure this, you can add the recovery attempter to the user info dictionary as the value of `NSRecoveryAttempterErrorKey `, or you can override the `recoveryAttempter` method.

The `localizedRecoveryOptions` must return an array of button titles (including the title of the button that requests error recovery). To ensure this, you can add the array to the user info dictionary as the value of `NSLocalizedRecoveryOptionsErrorKey `, or you can override the `localizedRecoveryOptions` method.

For the complete procedure for error recovery, including sample code, see [Recovering From Errors](../RecoverFromErrors/RecoverFromErrors.html#//apple_ref/doc/uid/TP40001806-CH206-BCIDEGGF).

        
            [Next](../HandleReceivedError/HandleReceivedError.html)[Previous](../CreateCustomizeNSError/CreateCustomizeNSError.html)
        
         Copyright &#x00a9; 2005, 2011 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2011-01-07