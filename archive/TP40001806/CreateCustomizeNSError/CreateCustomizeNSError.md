---
title: "Using and Creating Error Objects"
book: "Error Handling Programming Guide"
chapterId: "TP40001806-CH204"
date: "2011-01-07"
description: "Describes NSError objects, related Application Kit support for error handling, and how to use these features in your code."
identifier: "//apple_ref/doc/uid/TP40001806"
source: apple-archive
---

# Using and Creating Error Objects

> **Error Handling Programming Guide**
> Last updated: 2011-01-07

[Next](../ErrorRespondRecover/ErrorRespondRecover.html)[Previous](../ErrorObjectsDomains/ErrorObjectsDomains.html)
        
        
        [](../index.html)
        
        # Using and Creating Error Objects
The following sections describe how to deal with `NSError` objects returned from [framework](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56) methods, how to display error messages using error objects, how to create error objects, and how to implement methods that return error objects by reference. 

## Handling Error Objects Returned From Methods
Many methods of the [Cocoa and Cocoa Touch](../../../../General/Conceptual/DevPedia-CocoaCore/Cocoa.html#//apple_ref/doc/uid/TP40008195-CH9) classes include as their last parameter a direct or indirect reference to an `[NSError](https://developer.apple.com/documentation/foundation/nserror)` object. In some Foundation and UIKit methods you find `NSError` objects as arguments of [delegation](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14) methods. The following declaration is from the UIKit framework’s `[UIWebViewDelegate](https://developer.apple.com/documentation/uikit/uiwebviewdelegate)` protocol; a delegate would implement a method such as this to find out if an operation failed:

- (void)webView:(UIWebView *)webView didFailLoadWithError:(NSError *)error;Some methods of the Cocoa frameworks that you call include an indirect reference to an `NSError` object; these methods typically perform an operation such as creating a document, writing a file, or loading a URL. For example, the following method declaration is from the `[NSDocument](https://developer.apple.com/documentation/appkit/nsdocument)` class header file:

- (BOOL)writeToURL:(NSURL *)absoluteURL
```
    ofType:(NSString *)typeName
```

```
    error:(NSError **)outError;
```
If a method such as this encounters an error in its implementation, it directly returns `NO` to indicate failure and indirectly returns (if the client code requests it) an `NSError` object in the last parameter to describe the error.

If you want to evaluate the error, declare an `NSError` object variable before calling a method such as `writeToURL:ofType:error:`. When you invoke the method, pass in a pointer to this variable. (If you are not interested in the error, just pass `NULL`.) If the method directly returns `nil` or `NO`, inspect the `NSError` object to determine the cause of the error or simply display an error alert. Listing 2-1 illustrates this approach.

**Listing 2-1**  Handling an `NSError` object returned from a AppKit method

NSError *theError;
```
BOOL success = [myDoc writeToURL:[self docURL] ofType:@"html" error:&theError];
```

```
 
```

```
if (success == NO) {
```

```
    // Maybe try to determine cause of error and recover first.
```

```
    NSAlert *theAlert = [NSAlert alertWithError:theError];
```

```
    [theAlert runModal]; // Ignore return value.
```

```
}
```

> **Important:** Success or failure is indicated by the return value of the method. Although Cocoa methods that indirectly return error objects in the Cocoa error domain are guaranteed to return such objects if the method indicates failure by directly returning `nil` or `NO`, you should always check that the return value is `nil` or `NO` before attempting to do anything with the `NSError` object.

This code in Listing 2-1 uses the returned `NSError` to display an error alert to the user immediately. (`[UIAlertView](https://developer.apple.com/documentation/uikit/uialertview)`, the UIKit class corresponding to `[NSAlert](https://developer.apple.com/documentation/appkit/nsalert)`, has no equivalent method for `[alertWithError:](https://developer.apple.com/documentation/appkit/nsalert/1531823-init)`.) Error objects in the Cocoa domain are always [localized](../../../../General/Conceptual/DevPedia-CocoaCore/Internationalization.html#//apple_ref/doc/uid/TP40008195-CH23) and ready to present to users, so they can often be presented without further evaluation. 

Instead of merely displaying an error message based on an `NSError` object, you could examine the object to determine if you can do something else. For example, you might be able to perform the operation again in a slightly different way that circumvents the error; if you do this, however, you should first request the user’s permission to perform the modified operation.

When evaluating an `NSError` object, always use the object’s domain and error code as the bases of tests and not the strings describing the error or how to recover from it. Strings are typically localized and are thus likely to vary. With a few exceptions (such as the `[NSURLErrorDomain](https://developer.apple.com/documentation/foundation/nsurlerrordomain)` domain), pre-existing errors returned from Cocoa framework methods are always in the `NSCocoaErrorDomain` domain; however, because there are exceptions you might want to test whether the top-level error belongs to that domain. Error objects returned from Cocoa methods can often contain underlying error objects representing errors returned by lower subsystems, such as the BSD layer (`NSPOSIXErrorDomain`). 

Of course, to make a successful evaluation of an error, you have to anticipate the errors that might be returned from a method invocation. And you should ensure that your code deals adequately with new errors that might be returned in the future.

> **Important:** You should always special-case test for the `NSUserCancelledError` error code (in the `NSCocoaErrorDomain`). This code indicates that the user cancelled the operation (for example, by pressing Command-period). In this case, you should not display any error dialog.

### Error-Handling Alternatives in OS X
If you are developing a Mac app, there are many other things you can do upon receiving an `NSError` object:

If you know how to recover from the error, but require the user’s approval, you could create a new version of the error object that adds a recovery attempter to it (see [Recovering From Errors](../RecoverFromErrors/RecoverFromErrors.html#//apple_ref/doc/uid/TP40001806-CH206-BCIDEGGF)). 

Send the error object up the error-responder chain so that other objects in the application can add to it or try to recover from the error.

Before doing this, you might be able supplement the information in the error from the current programming context and then create a new error object that contains this enriched information.

If you use the returned `NSError` object as the basis of a new error object, either by adding a recovery attempter or supplementary information, you can either:

Display the message immediately.

 Pass the error on to the next error responder.

For more on customizing errors passed up the error-responder chain, see [Handling Received Errors](../HandleReceivedError/HandleReceivedError.html#//apple_ref/doc/uid/TP40001806-CH205-BAJEHAJC).

## Displaying Information From Error Objects
There are several different ways to display the information in `NSError` objects. You could extract the localized description (or failure reason), recovery suggestion, and the recovery options from the error object and use them to initialize the tiles and message text of an `[NSAlert](https://developer.apple.com/documentation/appkit/nsalert)`, `[UIAlertView](https://developer.apple.com/documentation/uikit/uialertview)`, or `[UIActionSheet](https://developer.apple.com/documentation/uikit/uiactionsheet)` object (or an OS X modal document sheet). This universal approach gives you a large degree of control over the content and presentation of the error alert.

For example, the code in Listing 2-2 composes the message text of an `UIAlertView` object from the localized description and failure reason taken from the passed-in `NSError` object. 

**Listing 2-2**  Displaying an alert composed mostly from error-object attributes

- (void)webView:(UIWebView *)webView didFailLoadWithError:(NSError *)error {
```
 
```

```
    NSString *titleString = @"Error Loading Page";
```

```
    NSString *messageString = [error localizedDescription];
```

```
    NSString *moreString = [error localizedFailureReason] ?
```

```
                        [error localizedFailureReason] :
```

```
                        NSLocalizedString(@"Try typing the URL again.", nil);
```

```
    messageString = [NSString stringWithFormat:@"%@. %@", messageString, moreString];
```

```
 
```

```
    UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:titleString
```

```
        message:messageString delegate:self
```

```
        cancelButtonTitle:@"Cancel" otherButtonTitles:nil];
```

```
    [alertView show];
```

```
}
```
However, you don’t have to use the [localized](../../../../General/Conceptual/DevPedia-CocoaCore/Internationalization.html#//apple_ref/doc/uid/TP40008195-CH23) error strings made available by the frameworks. For example, if you think they’re not descriptive enough or want to supplement them with context-specific information, you can identify the error by its domain and code and then substitute your own string values. Take the delegate method used in the prior example; the error object passed to the delegate in `[webView:didFailLoadWithError:](https://developer.apple.com/documentation/uikit/uiwebviewdelegate/1617970-webview)` is almost always of the `[NSURLErrorDomain](https://developer.apple.com/documentation/foundation/nsurlerrordomain)` domain. You could then find out which code of this domain is associated with the error and substitute your own string for the strings contained in the error object. (`NSURLErrorDomain` and its codes are declared in `NSURLError.h`.) Listing 2-3 gives an example of this.

**Listing 2-3**  Assigning custom message strings based on error domain and code

```
- (void)webView:(UIWebView *)webView didFailLoadWithError:(NSError *)error {
```

```
 
```

```
    NSString *errorMsg;
```

```
 
```

```
    if ([[error domain] isEqualToString:NSURLErrorDomain]) {
```

```
        switch ([error code]) {
```

```
            case NSURLErrorCannotFindHost:
```

```
                errorMsg = NSLocalizedString(@"Cannot find specified host. Retype URL.", nil);
```

```
                break;
```

```
            case NSURLErrorCannotConnectToHost:
```

```
                errorMsg = NSLocalizedString(@"Cannot connect to specified host. Server may be down.", nil);
```

```
                break;
```

```
            case NSURLErrorNotConnectedToInternet:
```

```
                errorMsg = NSLocalizedString(@"Cannot connect to the internet. Service may not be available.", nil);
```

```
                break;
```

```
            default:
```

```
                errorMsg = [error localizedDescription];
```

```
                break;
```

```
        }
```

```
    } else {
```

```
        errorMsg = [error localizedDescription];
```

```
    }
```

```
 
```

```
    UIAlertView *av = [[UIAlertView alloc] initWithTitle:
```

```
        NSLocalizedString(@"Error Loading Page", nil)
```

```
        message:errorMsg delegate:self
```

```
        cancelButtonTitle:@"Cancel" otherButtonTitles:nil];
```

```
    [av show];
```

```
}
```
### Propagating Errors for Display by the Application Object (OS X)
The Application Kit provides a few shortcuts for displaying error alerts. The `presentError:` and the `[presentError:modalForWindow:delegate:didPresentSelector:contextInfo:](https://developer.apple.com/documentation/appkit/nsresponder/1534705-presenterror)` methods permit you to originate an error alert that is eventually displayed by the global application object, `NSApp`; the former method requests an application-modal alert and the latter a document-modal alert. You must send either of these present-error messages to an object in the error-responder chain (see [The Error-Responder Chain](../ErrorRespondRecover/ErrorRespondRecover.html#//apple_ref/doc/uid/TP40001806-CH203-CJBHABAA)): a view object, a window object, an `[NSDocument](https://developer.apple.com/documentation/appkit/nsdocument)` object, an `[NSWindowController](https://developer.apple.com/documentation/appkit/nswindowcontroller)` object, an `[NSDocumentController](https://developer.apple.com/documentation/appkit/nsdocumentcontroller)` object, or `NSApp`. (If you send the message to a view, it should ideally be a view object associated in some way with the condition that produced the error.) Listing 2-4 illustrates how you might invoke the document-modal `presentError:modalForWindow:delegate:didPresentSelector:contextInfo:` method.

**Listing 2-4**  Displaying a document-modal error alert

NSError *theError;
```
NSData *theData = [doc dataOfType:@”xml” error:&theError];
```

```
if (!theData && theError)
```

```
    [anyView presentError:theError
```

```
            modalForWindow:[doc windowForSheet]
```

```
            delegate:self
```

```
            didPresentSelector:
```

```
                @selector(didPresentErrorWithRecovery:contextInfo:)
```

```
            contextInfo:nil];
```
After the user dismisses the alert, `NSApp` invokes a method (identified in the `didPresentSelector:` keyword) implemented by the modal delegate. As Listing 2-5 shows, the modal delegate in this method checks whether the recovery-attempter object (if any) managed to recover from the error and responds accordingly.

**Listing 2-5**  Modal delegate handling the user response

- (void)didPresentErrorWithRecovery:(BOOL)recover contextInfo:(void *)info {
```
    if (recover == NO) { // Recovery did not succeed, or no recovery attempted.
```

```
        // Proceed accordingly.
```

```
    }
```

```
}
```
For more on the recovery-attempter object, see [Recovering From Errors](../RecoverFromErrors/RecoverFromErrors.html#//apple_ref/doc/uid/TP40001806-CH206-BCIDEGGF).

Sometimes you might not want to send an error object up the error-responder chain to be displayed by `NSApp`. You would rather show an error alert to the user immediately, and not have to construct it yourself. The `NSAlert` class provides the `alertWithError: ` method for this purpose.

**Listing 2-6**  Directly displaying an error alert dialog

NSAlert *theAlert = [NSAlert alertWithError:theError];
```
NSInteger button = [theAlert runModal];
```

```
if (button != NSAlertFirstButtonReturn) {
```

```
    // handle
```

```
}
```

> **Note:** The `[presentError:](https://developer.apple.com/documentation/appkit/nsresponder/1531294-presenterror)` and `[presentError:modalForWindow:delegate:didPresentSelector:contextInfo:](https://developer.apple.com/documentation/appkit/nsresponder/1534705-presenterror)` methods silently ignore `[NSUserCancelledError](https://developer.apple.com/documentation/foundation/1448136-nserror_codes/nsusercancellederror)` errors in the `[NSCocoaErrorDomain](https://developer.apple.com/documentation/foundation/nscocoaerrordomain)` domain.

## Creating and Returning NSError Objects
You can declare and implement your own methods that indirectly return an `NSError` object. Methods that are good candidates for `NSError` parameters are those that open and read files, load resources, parse formatted text, and so on. In general, these methods should not indicate an error through the existence of an `NSError` object. Instead, they should return `NO` or `nil` from the method to indicate that an error occurred. Return the `NSError` object to describe the error. 

If you are going to return an `NSError` object by reference in an implementation of such a method, you must create the `NSError` object. You create an error object either by allocating it and then initializing it with the `initWithDomain:code:userInfo:` method of `NSError` or by using the class factory method `errorWithDomain:code:userInfo:`. As the keywords of both methods indicate, you must supply the initializer with a domain (string constant), an error code (a signed integer), and a “user info” dictionary containing descriptive and supporting information. (See [Error Objects, Domains, and Codes](../ErrorObjectsDomains/ErrorObjectsDomains.html#//apple_ref/doc/uid/TP40001806-CH202-CJBGAIBJ) for full descriptions of these data items.) You should ensure that all strings in the user info dictionary are localized.

> **Important:** You should *only* modify the `NSError` parameter if there is an error in your method and your method directly returns `NO`. Always test to see if the parameter is non-`NULL` before assigning an object to it. And never assign `nil` to an error parameter.

Listing 2-7 is an example of a method that, for the purpose of illustration, calls the POSIX-layer `open` function to open a file. If this function returns an error, the method creates an `NSError` object of the `NSPOSIXErrorDomain` that is used as the underlying error of a custom error domain returned to the caller. 

**Listing 2-7**  Implementing a method that returns an `NSError` object

- (NSString *)fooFromPath:(NSString *)path error:(NSError **)anError {
```
 
```

```
    const char *fileRep = [path fileSystemRepresentation];
```

```
    int fd = open(fileRep, O_RDWR|O_NONBLOCK, 0);
```

```
 
```

```
    if (fd == -1) {
```

```
 
```

```
        if (anError != NULL) {
```

```
            NSString *description;
```

```
            NSDictionary *uDict;
```

```
            int errCode;
```

```
 
```

```
            if (errno == ENOENT) {
```

```
                description = NSLocalizedString(@"No file or directory at requested location", @"");
```

```
                errCode = MyCustomNoFileError;
```

```
            } else if (errno == EIO) {
```

```
                // Continue for each possible POSIX error...
```

```
            }
```

```
 
```

```
            // Create the underlying error.
```

```
            NSError *underlyingError = [[NSError alloc] initWithDomain:NSPOSIXErrorDomain
```

```
                code:errno userInfo:nil];
```

```
            // Create and return the custom domain error.
```

```
            NSDictionary *errorDictionary = @{ NSLocalizedDescriptionKey : description,
```

```
                NSUnderlyingErrorKey : underlyingError, NSFilePathErrorKey : path };
```

```
 
```

```
            *anError = [[NSError alloc] initWithDomain:MyCustomErrorDomain
```

```
                    code:errCode userInfo:errorDictionary];
```

```
        }
```

```
        return nil;
```

```
    }
```

```
    // ...
```
	In this example, the returned error object includes in its user info dictionary the path that caused the error.

As the example in Listing 2-7 shows, you can use errors that originate from underlying subsystems as the basis for error objects that you return to callers. You can use raised exceptions that your code handles in the same way. An `[NSException](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/cl/NSException)` object is compatible with an `NSError` object in that its attributes are a name, a reason, and a user info dictionary. You can easily transfer information in the exception object over to the error object.

### A Note on Errors and Exceptions
It is important to keep in mind the difference between error objects and [exception](../../../../General/Conceptual/DevPedia-CocoaCore/ExceptionHandling.html#//apple_ref/doc/uid/TP40008195-CH18) objects in Cocoa and Cocoa Touch, and when to use one or the other in your code. They serve different purposes and should not be confused.

Exceptions (represented by `[NSException](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/cl/NSException)` objects) are for programming errors, such as an array index that is out of bounds or an invalid method argument. User-level errors (represented by `NSError` objects) are for runtime errors, such as when a file cannot be found or a string in a certain encoding cannot be read. Conditions giving rise to exceptions are due to programming errors; you should deal with these errors before you ship a product. Runtime errors can always occur, and you should communicate these (via `NSError` objects) to the user in as much detail as is required. 

Although exceptions should ideally be taken care of before deployment, a shipped application can still experience exceptions as a result of some truly exceptional situation such as “out of memory” or “boot volume not available.” It is best to allow the highest level of the application—the global application object itself—to deal with these situations.

        
            [Next](../ErrorRespondRecover/ErrorRespondRecover.html)[Previous](../ErrorObjectsDomains/ErrorObjectsDomains.html)
        
         Copyright &#x00a9; 2005, 2011 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2011-01-07