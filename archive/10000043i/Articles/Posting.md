---
title: "Posting a Notification"
book: "Notification Programming Topics"
chapterId: "20000724"
date: "2009-08-18"
description: "Explains how to send and receive information about events in Cocoa programs."
identifier: "//apple_ref/doc/uid/10000043i"
source: apple-archive
---

# Posting a Notification

> **Notification Programming Topics**
> Last updated: 2009-08-18

[Next](Threading.html)[Previous](Registering.html)
        
        
        [](../index.html)
        
        # Posting a Notification

You can post notifications within your own application or make them available to other applications. See [Posting Local Notifications](#//apple_ref/doc/uid/20000724-97609) for the former and [Posting Distributed Notifications](#//apple_ref/doc/uid/20000724-97476) for the latter.

## Posting Local Notifications

You can [create](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39) a notification object with `notificationWithName:object:` or `notificationWithName:object:userInfo:`. You then post the notification object to a notification center using the `postNotification:` instance method. `NSNotification` objects are [immutable](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectMutability.html#//apple_ref/doc/uid/TP40008195-CH42), so once created, they cannot be modified.

However, you normally don’t create your own notifications directly. The methods `postNotificationName:object:` and `postNotificationName:object:userInfo:` of the `NSNotificationCenter` class allow you to conveniently post a notification without creating it first.

In each case, you usually post the notification to the process’s default notification center. You obtain the default object using the `defaultCenter` [class method](../../../../General/Conceptual/DevPedia-CocoaCore/ClassMethod.html#//apple_ref/doc/uid/TP40008195-CH8).

As an example of using the notification center to post a notification, consider the example from [Registering for Local Notifications](Registering.html#//apple_ref/doc/uid/20000723-98481). You have a program that can perform a number of conversions on text (for instance, RTF to ASCII). The conversions are handled by a class of objects (`Converter`) that can be added or removed during program execution. Your program may have other objects that want to be notified when converters are added or removed, but the `Converter` objects do not need to know who these objects are or what they do. You thus declare two notifications, `"ConverterAdded"` and `"ConverterRemoved"`, which you post when the given event occurs.

When a user installs or removes a converter, it sends one of the following [messages](../../../../General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59) to the notification center:

```
[[NSNotificationCenter defaultCenter]
```

```
    postNotificationName:@"ConverterAdded" object:self];
```

or

```
[[NSNotificationCenter defaultCenter]
```

```
    postNotificationName:@"ConverterRemoved" object:self];
```

The notification center then identifies which objects (if any) are interested in these notifications and notifies them.

If there are other objects of interest to the observer (besides the notification name and observed object), place them in the notification’s optional dictionary or use `postNotificationName:object:userInfo:`.

## Posting Distributed Notifications

Posting distributed notifications is much the same as for posting local notifications. You can [create](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39) an `NSNotification` object manually and post with `postNotification:` or use one of the `NSDistributedNotificationCenter` convenience methods. The only differences are that the notification object must be a string object and the optional user-info dictionary can contain only [property list](../../../../General/Conceptual/DevPedia-CocoaCore/PropertyList.html#//apple_ref/doc/uid/TP40008195-CH44) objects, such as `NSString` and `NSNumber`.

An observer of a given notification may be in a suspended state and not processing notifications immediately. If an object posting a notification wants to ensure that all observers receive the notification immediately (for example, if the notification is a warning that a server is about to shut down), it can invoke `postNotificationName:object:userInfo:deliverImmediately:` with *deliverImmediately:YES*. The notification center delivers the notification as if the observers had registered with `NSNotificationSuspensionBehaviorDeliverImmediately` (further described in [Registering for Distributed Notifications](Registering.html#//apple_ref/doc/uid/20000723-98486)). Delivery is not guaranteed, however. For example, the process receiving the notifications may be too busy to process and accept queued notifications. In this case, the notification is dropped.

        
            [Next](Threading.html)[Previous](Registering.html)
        
         Copyright &#x00a9; 2009 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2009-08-18