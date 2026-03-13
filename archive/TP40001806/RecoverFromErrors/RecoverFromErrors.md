---
title: "Recovering From Errors"
book: "Error Handling Programming Guide"
chapterId: "TP40001806-CH206"
date: "2011-01-07"
description: "Describes NSError objects, related Application Kit support for error handling, and how to use these features in your code."
identifier: "//apple_ref/doc/uid/TP40001806"
source: apple-archive
---

# Recovering From Errors

> **Error Handling Programming Guide**
> Last updated: 2011-01-07

[Next](../RevisionHistory/RevisionHistory.html)[Previous](../HandleReceivedError/HandleReceivedError.html)
        
        
        [](../index.html)
        
        # Recovering From Errors

> **Note:** The error-presentation, error-responder, and error-recovery architectures described in this chapter are available only on the Mac.

As described in [The Recovery Attempter](../ErrorObjectsDomains/ErrorObjectsDomains.html#//apple_ref/doc/uid/TP40001806-CH202-BBCJHJIG), an `[NSError](https://developer.apple.com/documentation/foundation/nserror)` object can have a designated recovery attempter, an object that attempts to recover from the error if the user requests it. The error object holds a reference to the recovery attempter in its user info [dictionary](../../../../General/Conceptual/DevPedia-CocoaCore/Collection.html#//apple_ref/doc/uid/TP40008195-CH10), so if the error object is passed around within an application, the recovery attempter stays with it. The user info dictionary must also contain recovery options, an array of [localized](../../../../General/Conceptual/DevPedia-CocoaCore/Internationalization.html#//apple_ref/doc/uid/TP40008195-CH23) strings for button titles, one or more of which requests recovery. When the error is presented in an alert and the user selects the recovery option, a message is sent to the recovery attempter, requesting it to do its job.

Ideally, the recovery attempter should be an independent object that knows something about the conditions of an error and how best to circumvent those conditions. An application could even have an object whose role is to recover from errors of various kinds. A recovery attempter must implement at least one of the two methods of the `NSErrorRecoveryAttempting` informal [protocol](../../../../General/Conceptual/DevPedia-CocoaCore/Protocol.html#//apple_ref/doc/uid/TP40008195-CH45): `attemptRecoveryFromError:optionIndex:delegate:didRecoverSelector:contextInfo:` or `attemptRecoveryFromError:optionIndex:`. It implements the former method for error alerts presented document-modally, and the latter method for application-modal alerts.

You must also prepare an error object so that error recovery can take place. To do this, add three items to the user info dictionary of the error object:

The recovery-attempter object under the key `NSRecoveryAttempterErrorKey`

The recovery options, an array of localized strings, under the key `NSLocalizedRecoveryOptionsErrorKey`

A recovery-suggestion string, also localized, under the key `NSLocalizedRecoverySuggestionErrorKey`. Although this property is not strictly required, an error alert that offers recovery as an option should display this string, as stipulated by the human-interface guidelines. And if the string refers to particular button titles, it should use the same titles in the recovery-options array.

For guidelines about error alerts on OS X, see *OS X Human Interface Guidelines* (section on dialogs in “UI Element Guidelines: Windows”).

Listing 5-1 illustrates a case involving the `[NSXMLDocument](https://developer.apple.com/documentation/foundation/xmldocument)` class. In this example, an `[NSDocument](https://developer.apple.com/documentation/appkit/nsdocument)` object attempts to create an internal tree representing an XML document using the `initWithContentsOfURL:options:error:` method of `NSXMLDocument`. If the attempt fails, the usual cause is that the source XML is malformed—for example, there is a missing end tag for an element, or an attribute value is not quoted. If the source XML is “tidied” first to fix the structural problems, it may be possible to create the XML tree. 

In the example in Listing 5-1 if the invocation of `initWithContentsOfURL:options:error:` returns an error object by reference, the document object customizes the error object, adding (among other things) a recovery-attempter object, localized recovery options, and a localized recovery suggestion to its user info dictionary. Then it sends `presentError:modalForWindow:delegate:didPresentSelector:contextInfo:` to `self`.

**Listing 5-1**  Preparing for error recovery

- (BOOL)readFromURL:(NSURL *)furl ofType:(NSString *)type error:(NSError **)anError {
```
    NSError *err;
```

```
 
```

```
    // xmlDoc is an NSXMLDocument instance variable.
```

```
    if (xmlDoc != nil) {
```

```
        xmlDoc = nil;
```

```
    }
```

```
 
```

```
    xmlDoc = [[NSXMLDocument alloc] initWithContentsOfURL:furl
```

```
            options:NSXMLNodeOptionsNone error:&err];
```

```
 
```

```
    if (xmlDoc == nil && err) {
```

```
        NSString *newDesc = [[err localizedDescription] stringByAppendingString:
```

```
            ([err localizedFailureReason] ? [err localizedFailureReason] : @"")];
```

```
 
```

```
        NSDictionary *newDict = @{ NSLocalizedDescriptionKey : newDesc,
```

```
            NSURLErrorKey : furl,
```

```
            NSRecoveryAttempterErrorKey : self,
```

```
            NSLocalizedRecoverySuggestionErrorKey :
```

```
                NSLocalizedString(@"Would you like to tidy the XML and try again?", @""),
```

```
            NSLocalizedRecoveryOptionsErrorKey :
```

```
                @[NSLocalizedString(@"Try Again", @""), NSLocalizedString(@"Cancel", @"")] };
```

```
 
```

```
        NSError *newError = [[NSError alloc] initWithDomain:[err domain]
```

```
            code:[err code] userInfo:newDict];
```

```
        [self presentError:newError modalForWindow:[self windowForSheet]
```

```
            delegate:self
```

```
            didPresentSelector:@selector(didPresentErrorWithRecovery:contextInfo:)
```

```
            contextInfo:nil];
```

```
    }
```

```
// ...
```
	Note that the document object also adds to the user info dictionary the URL identifying the source of XML. The recovery attempter will use this URL when it attempts to create a tree representing the XML.

The error object is passed up the error-responder chain and `NSApp` displays it. When the user clicks any button of the error alert, `NSApp` checks to see if the error object has both a recovery attempter and recovery options. If both of these conditions are true, it invokes the method implemented by the recovery attempter that corresponds to the mode of the alert (that is, document-modal or application-modal). 

Listing 5-2 shows how the recovery attempter for the XML document implements the `attemptRecoveryFromError:optionIndex:delegate:didRecoverSelector:contextInfo:` method.

**Listing 5-2**  Recovering from the error and informing the modal delegate

- (void)attemptRecoveryFromError:(NSError *)error
```
                     optionIndex:(unsigned int)recoveryOptionIndex
```

```
                        delegate:(id)delegate
```

```
              didRecoverSelector:(SEL)didRecoverSelector
```

```
                     contextInfo:(void *)contextInfo {
```

```
 
```

```
    BOOL success = NO;
```

```
    NSError *err;
```

```
    NSInvocation *invoke = [NSInvocation invocationWithMethodSignature:
```

```
                               [delegate methodSignatureForSelector:didRecoverSelector]];
```

```
    [invoke setSelector:didRecoverSelector];
```

```
 
```

```
    if (recoveryOptionIndex == 0) { // Recovery requested.
```

```
        xmlDoc = [[NSXMLDocument alloc] initWithContentsOfURL:[[error userInfo]
```

```
                objectForKey:NSURLErrorKey] options:NSXMLDocumentTidyXML error:&err];
```

```
        if (xmlDoc != nil) {
```

```
            success = YES;
```

```
        }
```

```
    }
```

```
    [invoke setArgument:(void *)&success atIndex:2];
```

```
    if (err)
```

```
        [invoke setArgument:&err atIndex:3];
```

```
    [invoke invokeWithTarget:delegate];
```

```
}
```
	The key part of the above example is the test the recovery attempter makes to determine if the user clicked the “Try Again” button: it checks the value of `recoveryOptionIndex`. If the user did click this button, the recovery attempter invokes the `initWithContentsOfURL:options:error:` method again, this time with the `NSXMLDocumentTidyXML` option. Then it creates and invokes an `[NSInvocation](https://developer.apple.com/documentation/foundation/nsinvocation)` object, thereby sending the required message to the modal [delegate](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14) of the error alert. The invocation object includes the two parameters required by the delegate’s [selector](../../../../General/Conceptual/DevPedia-CocoaCore/Selector.html#//apple_ref/doc/uid/TP40008195-CH48): a Boolean indicating whether the recovery attempt succeeded and a “context info” parameter which, in this case, contains any error object returned from the recovery attempt.

> **Note:** The example in Listing 5-2 shows the use of `NSInvocation` to send a message. However, if you have a reference to the modal delegate and know the name of the method it implements, you can send the message directly.

When the modal delegate receives the message from the recovery attempter, as in Listing 5-3, it can respond appropriately.

**Listing 5-3**  Modal delegate responding to recovery attempter

```
- (void)didPresentErrorWithRecovery:(BOOL)didRecover
```

```
            contextInfo:(void *)contextInfo {
```

```
 
```

```
    NSError *theError = (NSError *)contextInfo;
```

```
    if (didRecover) {
```

```
        [tableView reloadData];
```

```
    } else if (theError && [theError isKindOfClass:[NSError class]]) {
```

```
        [NSAlert alertWithError:theError];
```

```
    }
```

```
}
```

        
            [Next](../RevisionHistory/RevisionHistory.html)[Previous](../HandleReceivedError/HandleReceivedError.html)
        
         Copyright &#x00a9; 2005, 2011 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2011-01-07