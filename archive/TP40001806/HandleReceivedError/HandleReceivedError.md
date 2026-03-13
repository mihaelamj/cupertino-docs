---
title: "Handling Received Errors"
book: "Error Handling Programming Guide"
chapterId: "TP40001806-CH205"
date: "2011-01-07"
description: "Describes NSError objects, related Application Kit support for error handling, and how to use these features in your code."
identifier: "//apple_ref/doc/uid/TP40001806"
source: apple-archive
---

# Handling Received Errors

> **Error Handling Programming Guide**
> Last updated: 2011-01-07

[Next](../RecoverFromErrors/RecoverFromErrors.html)[Previous](../ErrorRespondRecover/ErrorRespondRecover.html)
        
        
        [](../index.html)
        
        # Handling Received Errors

> **Note:** The error-presentation and error-responder architecture described in this chapter is available only on the Mac.

When you send a `presentError:` or `presentError:modalForWindow:delegate:didPresentSelector:contextInfo:` message to certain eligible objects, the message travels up a sequence of objects in an application called the error-responder chain (see [The Error-Responder Chain](../ErrorRespondRecover/ErrorRespondRecover.html#//apple_ref/doc/uid/TP40001806-CH203-CJBHABAA)). The default implementation for most objects in this chain is to send the `willPresentError:` method to `self` before sending the `presentError:` message to the next object. The `willPresentError:` message gives instances of custom subclasses an opportunity to look at the error object being passed up the chain and possibly customize it. When the error object reaches the end of the chain, the global application object, NSApp, displays an error alert to users; but before NSApp displays the error alert, it invokes the method `application:willPresentError:`, giving its delegate the same opportunity.

The following sections discuss strategies for implementing the `willPresentError:` and `application:willPresentError:` methods.

## Passing Errors Up the Error-Responder Chain
If you have a subclass of `[NSDocument](https://developer.apple.com/documentation/appkit/nsdocument)`, `[NSDocumentController](https://developer.apple.com/documentation/appkit/nsdocumentcontroller)`, `[NSWindowController](https://developer.apple.com/documentation/appkit/nswindowcontroller)`, `[NSWindow](https://developer.apple.com/documentation/appkit/nswindow)`, `[NSPanel](https://developer.apple.com/documentation/appkit/nspanel)`, or any view class, you can override the `willPresentError:` method to customize the presentation of errors. This might be something you want an instance of your subclass to do if it knows more about the context of a particular error than other objects in the application. Generally, an implementation of `willPresentError:` examines the passed-in `[NSError](https://developer.apple.com/documentation/foundation/nserror)` object and if, for example, its localized description is insufficient, or if the subclass knows how to recover from the error, it creates a new `NSError` object and returns it. In most cases, the customized error object keeps some information from the passed-in object.

An implementation of the `willPresentError:` method should always use the error domain and error code as the basis for deciding whether to return a customized error object. Do not base the decision on the strings in the user info dictionary for these can be localized and may vary between invocations. If your implementation decides not to customize the error, don’t return the passed-in object directly; instead, send the `willPresentError:` message to `super`. Listing 4-1 illustrates some of these strategies.

**Listing 4-1**  Handling an error passed up the error-responder chain

- (NSError *)willPresentError:(NSError *)error {
```
 
```

```
    if ([[error domain] isEqualToString:NSCocoaErrorDomain]) {
```

```
        switch([error code]) {
```

```
            case NSFileLockingError:
```

```
            case NSFileReadNoSuchFileError:
```

```
            { // Private method of custom subclass.
```

```
                return [self customizeError:error];
```

```
            }
```

```
            default:
```

```
                return [super willPresentError:error];
```

```
        }
```

```
    }
```

```
    return [super willPresentError:error];
```

```
}
```
You don’t have to make a subclass in order to customize an `NSError` object for presentation. Instead, your application [delegate](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14) can implement the `[application:willPresentError:](https://developer.apple.com/documentation/appkit/nsapplicationdelegate/1428721-application)` method. The same observations and guidelines given for `willPresentError:` above apply to the implementation of `application:willPresentError:`, except that you can return the original error object directly if you decide not to customize it.

## Customizing an Error Object
In the `willPresentError:` example in [Listing 4-1](#//apple_ref/doc/uid/TP40001806-CH205-BAJCGDGF), a private method is invoked to customize the error object. This is done to clarify the structure of the implementation. But if the customizing code was in-line, it might look some like the `willPresentError:` implementation in Listing 4-2. This code checks if the passed-in object has a failure reason and, if it does, it creates a more application-specific error description, appending the failure reason. Then it creates a new `[NSError](https://developer.apple.com/documentation/foundation/nserror)` object with this different description.

**Listing 4-2**  Customizing an NSError object

- (NSError *)willPresentError:(NSError *)error {
```
 
```

```
    if ([[error domain] isEqualToString:NSCocoaErrorDomain]) {
```

```
        switch([error code]) {
```

```
            case NSFileLockingError:
```

```
            case NSFileReadNoSuchFileError:
```

```
            {
```

```
                NSString *locFailure = [error localizedFailureReason];
```

```
                if (locFailure) {
```

```
                    NSMutableDictionary *newUserInfo = [NSMutableDictionary
```

```
                        dictionaryWithCapacity:[[[error userInfo] allKeys] count]];
```

```
                    [newUserInfo setDictionary:[error userInfo]];
```

```
                    NSString *errorDesc = [NSString stringWithFormat:
```

```
                        NSLocalizedString(@”MyGreatApp cannot open the file. %@”, @””),
```

```
                        locFailure];
```

```
                    [newUserInfo setObject:errorDesc
```

```
                        forKey:NSLocalizedDescriptionKey];
```

```
                    NSError *newError = [NSError errorWithDomain:[error domain]
```

```
                        code:[error code] userInfo:newUserInfo];
```

```
                    return newError;
```

```
                }
```

```
                else {
```

```
                    return [super willPresentError:error];
```

```
                }
```

```
            }
```

```
            default:
```

```
                return [super willPresentError:error];
```

```
        }
```

```
    }
```

```
    return [super willPresentError:error];
```

```
}
```
	In this example, the original error object is essentially cloned to make the new one. The new error object contains a more specific error description and appends the failure reason to it. 

As noted in [Passing Errors Up the Error-Responder Chain](#//apple_ref/doc/uid/TP40001806-CH205-BAJGIAJJ), there is no difference in implementing `willPresentError:` and the delegate method `application:willPresentError:`, except that in the latter method you can return the passed-in error object directly if you do not customize it.

> **Note:** For another example of error-object customization, see [Listing 5-1](../RecoverFromErrors/RecoverFromErrors.html#//apple_ref/doc/uid/TP40001806-CH206-BCIEDAIJ) in [Recovering From Errors](../RecoverFromErrors/RecoverFromErrors.html#//apple_ref/doc/uid/TP40001806-CH206-BCIDEGGF). In this case, the original object is changed to include recovery options, a recovery suggestion, and a recovery-attempter object.

        
            [Next](../RecoverFromErrors/RecoverFromErrors.html)[Previous](../ErrorRespondRecover/ErrorRespondRecover.html)
        
         Copyright &#x00a9; 2005, 2011 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2011-01-07