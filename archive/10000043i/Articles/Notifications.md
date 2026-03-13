---
title: "Notifications"
book: "Notification Programming Topics"
chapterId: "20000215"
date: "2009-08-18"
description: "Explains how to send and receive information about events in Cocoa programs."
identifier: "//apple_ref/doc/uid/10000043i"
source: apple-archive
---

# Notifications

> **Notification Programming Topics**
> Last updated: 2009-08-18

[Next](NotificationCenters.html)[Previous](../Introduction/introNotifications.html)
        
        
        [](../index.html)
        
        # Notifications

A *notification* encapsulates information about an event, such as a window gaining focus or a network connection closing. Objects that need to know about an event (for example, a file that needs to know when its window is about to be closed) register with the notification center that it wants to be notified when that event happens. When the event does happen, a notification is posted to the notification center, which immediately broadcasts the notification to all registered objects. Optionally, a notification is queued in a notification queue, which posts notifications to a notification center after it delays specified notifications and coalesces notifications that are similar according to some specified criteria you specify.

## Notifications and Their Rationale

The standard way to pass information between objects is [message](../../../../General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59) passing—one object invokes the method of another object. However, message passing requires that the object sending the message know who the receiver is and what messages it responds to. At times, this tight coupling of two objects is undesirable—most notably because it would join together two otherwise independent subsystems. For these cases, a broadcast model is introduced: An object posts a notification, which is dispatched to the appropriate observers through an `NSNotificationCenter` object, or simply notification center.

An `NSNotification` object (referred to as a notification) contains a name, an object, and an optional dictionary. The name is a tag identifying the notification. The object is any object that the poster of the notification wants to send to observers of that notification—typically the object that posted the notification itself. The dictionary may contain additional information about the event. 

> **Notification names:** Notification names are typically defined as constant string variables—for example, `[NSWindowDidBecomeMainNotification](https://developer.apple.com/documentation/appkit/nswindow/1419448-didbecomemainnotification)`. Usually the value of the string is similar to the variable name. You should not, however, use the string value in your code, you should always use the name of the variable—see [Registering for Local Notifications](Registering.html#//apple_ref/doc/uid/20000723-98481) for an example.

Many Cocoa [frameworks](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56) make extensive use of notifications to allow objects to react to events they are interested in. The notifications sent by each class are described in the class’s reference documentation, under the “Notifications” section.

Any object may post a notification. Other objects can register themselves with the notification center as observers to receive notifications when they are posted. The notification center takes care of broadcasting notifications to the registered observers, if any. The object posting the notification, the object included in the notification, and the observer of the notification may all be different objects or the same object. Objects that post notifications need not know anything about the observers. On the other hand, observers need to know at least the notification name and keys to the dictionary if provided.

## Notification and Delegation

Using the notification system is similar to using [delegation](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14) but has these differences:

Any number of objects may receive the notification, not just the delegate object. This precludes returning a value.

An object may receive any [message](../../../../General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59) you like from the notification center, not just the predefined delegate methods.

The object posting the notification does not even have to know the observer exists.

        
            [Next](NotificationCenters.html)[Previous](../Introduction/introNotifications.html)
        
         Copyright &#x00a9; 2009 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2009-08-18