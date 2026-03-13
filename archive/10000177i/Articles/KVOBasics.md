---
title: "Registering for Key-Value Observing"
book: "Key-Value Observing Programming Guide"
chapterId: "20002252"
date: "2016-09-13"
description: "Explains the Cocoa key-value observing protocol."
identifier: "//apple_ref/doc/uid/10000177i"
source: apple-archive
---

# Registering for Key-Value Observing

> **Key-Value Observing Programming Guide**
> Last updated: 2016-09-13

[Next](KVOCompliance.html)[Previous](../KeyValueObserving.html)
        
        
        [](../index.html)
        
        # Registering for Key-Value Observing
You must perform the following steps to enable an object to receive key-value observing notifications for a KVO-compliant property:

Register the observer with the observed object using the method `[addObserver:forKeyPath:options:context:](https://developer.apple.com/documentation/objectivec/nsobject/1412787-addobserver)`.

Implement `[observeValueForKeyPath:ofObject:change:context:](https://developer.apple.com/documentation/objectivec/nsobject/1416553-observevalueforkeypath)` inside the observer to accept change notification messages.

Unregister the observer using the method `[removeObserver:forKeyPath:](https://developer.apple.com/documentation/objectivec/nsobject/1408054-removeobserver)` when it no longer should receive messages. At a minimum, invoke this method before the observer is released from memory.

> **Important:** Not all classes are KVO-compliant for all properties. You can ensure your own classes are KVO-compliant by following the steps described in [KVO Compliance](KVOCompliance.html#//apple_ref/doc/uid/20002178-BAJEAIEE). Typically properties in Apple-supplied frameworks are only KVO-compliant if they are documented as such.

## Registering as an Observer
An observing object first registers itself with the observed object by sending an  `addObserver:forKeyPath:options:context:` [message](../../../../General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59), passing itself as the observer and the key path of the property to be observed. The observer additionally specifies an options parameter and a context pointer to manage aspects of the notifications.

### Options
The options parameter, specified as a bitwise `OR` of option constants, affects both the content of the change dictionary supplied in the notification, and the manner in which notifications are generated.

You opt to receive the value of the observed property from before the change by specifying option `NSKeyValueObservingOptionOld`. You request the new value of the property with option `NSKeyValueObservingOptionNew`. You receive both old and new values with the bitwise `OR` of these options.

You instruct the observed object to send an immediate change notification (before `addObserver:forKeyPath:options:context:` returns) with the option `NSKeyValueObservingOptionInitial`. You can use this additional, one-time notification to establish the initial value of a property in the observer.

You instruct the observed object to send a notification just prior to a property change (in addition to the usual notification just after the change) by including the option `NSKeyValueObservingOptionPrior`. The change dictionary represents a prechange notification by including the key `NSKeyValueChangeNotificationIsPriorKey` with the value of an `NSNumber` wrapping `YES`. That key is not otherwise present. You can use the prechange notification when the observer’s own KVO compliance requires it to invoke one of the -`willChange…` methods for one of its properties that depends on an observed property. The usual post-change notification comes too late to invoke `willChange…` in time.

### Context
The context pointer in the `addObserver:forKeyPath:options:context:` message contains arbitrary data that will be passed back to the observer in the corresponding change notifications. You may specify `NULL` and rely entirely on the key path string to determine the origin of a change notification, but this approach may cause problems for an object whose superclass is also observing the same key path for different reasons.

A safer and more extensible approach is to use the context to ensure notifications you receive are destined for your observer and not a superclass.

The address of a uniquely named static variable within your class makes a good context. Contexts chosen in a similar manner in the super- or subclass will be unlikely to overlap. You may choose a single context for the entire class and rely on the key path string in the notification message to determine what changed. Alternatively, you may create a distinct context for each observed key path, which bypasses the need for string comparisons entirely, resulting in more efficient notification parsing. Listing 1 shows example contexts for the `balance` and `interestRate` properties chosen this way.

**Listing 1**  Creating context pointers

static void *PersonAccountBalanceContext = &PersonAccountBalanceContext;
```
static void *PersonAccountInterestRateContext = &PersonAccountInterestRateContext;
```
The example in Listing 2 demonstrates how a Person instance registers itself as an observer for an `Account` instance’s `balance` and `interestRate` properties using the given context pointers.

**Listing 2**  Registering the inspector as an observer of the balance and interestRate properties

- (void)registerAsObserverForAccount:(Account*)account {
```
    [account addObserver:self
```

```
              forKeyPath:@"balance"
```

```
                 options:(NSKeyValueObservingOptionNew |
```

```
                          NSKeyValueObservingOptionOld)
```

```
                 context:PersonAccountBalanceContext];
```

```
 
```

```
    [account addObserver:self
```

```
              forKeyPath:@"interestRate"
```

```
                 options:(NSKeyValueObservingOptionNew |
```

```
                          NSKeyValueObservingOptionOld)
```

```
                  context:PersonAccountInterestRateContext];
```

```
}
```

> **Note:** The key-value observing `addObserver:forKeyPath:options:context:` method does not maintain strong references to the observing object, the observed objects, or the context. You should ensure that you maintain strong references to the observing, and observed, objects, and the context as necessary.

## Receiving Notification of a Change
When the value of an observed property of an object changes, the observer receives an `observeValueForKeyPath:ofObject:change:context:`    [message](../../../../General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59). All observers must implement this method.

The observing object provides the key path that triggered the notification, itself as the relevant object, a [dictionary](../../../../General/Conceptual/DevPedia-CocoaCore/Collection.html#//apple_ref/doc/uid/TP40008195-CH10) containing details about the change, and the context pointer that was provided when the observer was registered for this key path.

The change dictionary entry `NSKeyValueChangeKindKey` provides information about the type of change that occurred. If the value of the observed object has changed, the `NSKeyValueChangeKindKey` entry returns `NSKeyValueChangeSetting`. Depending on the options specified when the observer was registered, the `NSKeyValueChangeOldKey` and `NSKeyValueChangeNewKey` entries in the change dictionary contain the values of the property before, and after, the change. If the property is an object, the value is provided directly. If the property is a scalar or a C structure, the value is wrapped in an `[NSValue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSValue/Description.html#//apple_ref/occ/cl/NSValue)` object (as with [key-value coding](../../../../General/Conceptual/DevPedia-CocoaCore/KeyValueCoding.html#//apple_ref/doc/uid/TP40008195-CH25)).

If the observed property is a to-many relationship, the `NSKeyValueChangeKindKey` entry also indicates whether objects in the relationship were inserted, removed, or replaced by returning `NSKeyValueChangeInsertion`, `NSKeyValueChangeRemoval`, or `NSKeyValueChangeReplacement`, respectively.

The change dictionary entry for `NSKeyValueChangeIndexesKey` is an `NSIndexSet` object specifying the indexes in the relationship that changed. If `NSKeyValueObservingOptionNew` or `NSKeyValueObservingOptionOld` are specified as options when the observer is registered, the `NSKeyValueChangeOldKey` and `NSKeyValueChangeNewKey` entries in the change dictionary are arrays containing the values of the related objects before, and after, the change.

The example in Listing 3 shows the `observeValueForKeyPath:ofObject:change:context:` implementation for the `Person` observer that logs the old and new values of the properties `balance` and `interestRate`, as registered in [Listing 2](#//apple_ref/doc/uid/20002252-SW2).

**Listing 3**  Implementation of observeValueForKeyPath:ofObject:change:context:

- (void)observeValueForKeyPath:(NSString *)keyPath
```
                      ofObject:(id)object
```

```
                        change:(NSDictionary *)change
```

```
                       context:(void *)context {
```

```
 
```

```
    if (context == PersonAccountBalanceContext) {
```

```
        // Do something with the balance…
```

```
 
```

```
    } else if (context == PersonAccountInterestRateContext) {
```

```
        // Do something with the interest rate…
```

```
 
```

```
    } else {
```

```
        // Any unrecognized context must belong to super
```

```
        [super observeValueForKeyPath:keyPath
```

```
                             ofObject:object
```

```
                               change:change
```

```
                               context:context];
```

```
    }
```

```
}
```
If you specified a `NULL` context when registering an observer, you compare the notification’s key path against the key paths you are observing to determine what has changed. If you used a single context for all observed key paths, you first test that against the notification’s context, and finding a match, use key path string comparisons to determine what specifically has changed. If you have provided a unique context for each key path, as demonstrated here, a series of simple pointer comparisons tells you simultaneously whether or not the notification is for this observer, and if so, what key path has changed.

In any case, the observer should always call the superclass’s implementation of `observeValueForKeyPath:ofObject:change:context:` when it does not recognize the context (or in the simple case, any of the key paths), because this means a superclass has registered for notifications as well.

> **Note:** If a notification propagates to the top of the class hierarchy, `NSObject` throws an `NSInternalInconsistencyException` because this is a programming error: a subclass failed to consume a notification for which it registered.

## Removing an Object as an Observer
You remove a key-value observer by sending the observed object a `removeObserver:forKeyPath:context:` message, specifying the observing object, the key path, and the context. The example in Listing 4 shows `Person` removing itself as an observer of `balance` and `interestRate`.

**Listing 4**  Removing the inspector as an observer of balance and interestRate

- (void)unregisterAsObserverForAccount:(Account*)account {
```
    [account removeObserver:self
```

```
                 forKeyPath:@"balance"
```

```
                    context:PersonAccountBalanceContext];
```

```
 
```

```
    [account removeObserver:self
```

```
                 forKeyPath:@"interestRate"
```

```
                    context:PersonAccountInterestRateContext];
```

```
}
```
After receiving a `removeObserver:forKeyPath:context:` message, the observing object will no longer receive any `observeValueForKeyPath:ofObject:change:context:` messages for the specified key path and object.

When removing an observer, keep several points in mind:

Asking to be removed as an observer if not already registered as one results in an `NSRangeException`. You either call `removeObserver:forKeyPath:context:` exactly once for the corresponding call to `addObserver:forKeyPath:options:context:`, or if that is not feasible in your app, place the `removeObserver:forKeyPath:context:` call inside a try/catch block to process the potential exception.

An observer does not automatically remove itself when deallocated. The observed object continues to send notifications, oblivious to the state of the observer. However, a change notification, like any other message, sent to a released object, triggers a memory access exception. You therefore ensure that observers remove themselves before disappearing from memory.

The protocol offers no way to ask an object if it is an observer or being observed. Construct your code to avoid release related errors. A typical pattern is to register as an observer during the observer’s initialization (for example in `init` or `viewDidLoad`) and unregister during deallocation (usually in `dealloc`), ensuring properly paired and ordered add and remove messages, and that the observer is unregistered before it is freed from memory.

        
            [Next](KVOCompliance.html)[Previous](../KeyValueObserving.html)
        
         Copyright &#x00a9; 2003, 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-09-13