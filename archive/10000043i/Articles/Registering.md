---
title: "Registering for a Notification"
book: "Notification Programming Topics"
chapterId: "20000723"
date: "2009-08-18"
description: "Explains how to send and receive information about events in Cocoa programs."
identifier: "//apple_ref/doc/uid/10000043i"
source: apple-archive
---

# Registering for a Notification

> **Notification Programming Topics**
> Last updated: 2009-08-18

[Next](Posting.html)[Previous](NotificationQueues.html)
        
        
        [](../index.html)
        
        # Registering for a Notification

You can register for notifications from within your own application or other applications. See [Registering for Local Notifications](#//apple_ref/doc/uid/20000723-98481) for the former and [Registering for Distributed Notifications](#//apple_ref/doc/uid/20000723-98486) for the latter. To unregister for a notification, which must be done when your object is [deallocated](../../../../General/Conceptual/DevPedia-CocoaCore/MemoryManagement.html#//apple_ref/doc/uid/TP40008195-CH27), see [Unregistering an Observer](#//apple_ref/doc/uid/20000723-98846).

## Registering for Local Notifications

You register an object  to receive a notification by invoking the notification center method `addObserver:selector:name:object:`, specifying the observer, the [message](../../../../General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59) the notification center should send to the observer, the name of the notification it wants to receive, and about which object. You don’t need to specify both the name and the object. If you specify only an object, the observer will receive all notifications containing that object. If you specify only a notification name, the observer will receive that notification every time it’s posted, regardless of the object associated with it.

It is possible for an observer to register to receive more than one message for the same notification. In such a case, the observer will receive all messages it is registered to receive for the notification, but the order in which it receives them cannot be determined.

If you later decide an observer no longer needs to receive notifications (for example, if the observer is being deallocated), you can remove the observer from the notification center’s list of observers with the methods `removeObserver:` or `removeObserver:name:object:`.

Normally, you register objects with the process’s default notification center. You obtain the default object using the `defaultCenter` class  [method](../../../../General/Conceptual/DevPedia-CocoaCore/ClassMethod.html#//apple_ref/doc/uid/TP40008195-CH8).

As an example of using the notification center to receive notifications, suppose you want to perform an operation any time a window becomes main (for example, if you’re implementing a [controller](../../../../General/Conceptual/DevPedia-CocoaCore/ControllerObject.html#//apple_ref/doc/uid/TP40008195-CH11) for an inspector panel). You would register your client object as an observer as shown in the following example:

```
[[NSNotificationCenter defaultCenter] addObserver:self
```

```
    selector:@selector(aWindowBecameMain:)
```

```
    name:NSWindowDidBecomeMainNotification object:nil];
```

By passing `nil` as the object to observe, the client object (`self`) is notified when any object posts a `[NSWindowDidBecomeMainNotification](https://developer.apple.com/documentation/appkit/nswindow/1419448-didbecomemainnotification)` notification.

When window becomes main, it posts an `NSWindowDidBecomeMainNotification` to the notification center. The notification center notifies all observers who are interested in the notification by invoking the method they specified in the [selector](../../../../General/Conceptual/DevPedia-CocoaCore/Selector.html#//apple_ref/doc/uid/TP40008195-CH48) argument of `addObserver:selector:name:object:`. In the case of our example observer, the selector is `aWindowBecameMain:`. The `aWindowBecameMain:` method might have the following implementation:

```
- (void)aWindowBecameMain:(NSNotification *)notification {
```

```
 
```

```
    NSWindow *theWindow = [notification object];
```

```
    MyDocument = (MyDocument *)[[theWindow windowController] document];
```

```
 
```

```
    // Retrieve information about the document and update the panel.
```

```
}
```

The `NSWindow` objects don’t need to know anything about your inspector panel.

## Registering for Distributed Notifications

An object registers itself to receive a notification by sending the `addObserver:selector:name:object:suspensionBehavior:` method to an `NSDistributedNotificationCenter` object, specifying the [message](../../../../General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59) the notification should send, the name of the notification it wants to receive, the identifying string to match (the *object* argument), and the behavior to follow if notification delivery is suspended.

Because the posting object and the observer may be in different processes, notifications can’t contain pointers to arbitrary objects. Therefore, the `NSDistributedNotificationCenter` class requires notifications to use an `NSString` object as the *object* argument. Notification matching is done based on this string, rather than an object pointer. You should check the documentation of the object posting the notification to see what it uses as its identifying string.

When a process is no longer interested in receiving notifications immediately, it may suspend notification delivery. This is often done when the application is hidden, or is put into the background. (The `NSApplication` object automatically suspends delivery when the application is not active.) The *suspensionBehavior* argument in the `addObserver` method identifies how arriving notifications should be handled while delivery is suspended. There are four different types of suspension behavior, each useful in different circumstances.

Suspension Behavior

Description

`NSNotificationSuspensionBehaviorDrop`

The server does not queue any notifications with this name and object until it receives the `setSuspended:NO` message.

`NSNotificationSuspensionBehaviorCoalesce`

The server queues only the last notification of the specified name and object; earlier notifications are dropped. In cover methods for which suspension behavior is not an explicit argument, `NSNotificationSuspensionBehaviorCoalesce` is the default.

`NSNotificationSuspensionBehaviorHold`

The server holds all matching notifications until the queue has been filled (queue size determined by the server) at which point the server may flush queued notifications.

`NSNotificationSuspensionBehaviorDeliverImmediately`

The server delivers notifications matching this registration irrespective of whether it has received the `setSuspended:YES` message. When a notification with this suspension behavior is matched, it has the effect of first flushing any queued notifications. The effect is as if the server received `setSuspended:NO` while the application is suspended, followed by the notification in question being delivered, followed by a transition back to the previous suspended or unsuspended state.

You suspend notifications by sending `setSuspended:YES` to the distributed notification center. While notifications are suspended, the notification server handles notifications destined for the process that suspended notification delivery according to the suspension behavior specified by the observers when they registered to receive notifications. When the process resumes notification delivery, all queued notifications are delivered immediately. In applications using Application Kit, the `NSApplication` object automatically suspends notification delivery when the application is not active.

Note that a notification destined for an observer that registered with `NSNotificationSuspensionBehaviorDeliverImmediately`, automatically flushes the queue as it is delivered, causing all queued notifications to be delivered at that time as well.

The suspended state can be overridden by the poster of a notification. If the notification is urgent, such as a warning of a server being shut down, the poster can force the notification to be delivered immediately to all observers by posting the notification with the `NSDistributedNotificationCenter postNotificationName:object:userInfo:deliverImmediately:` method with the *deliverImmediately* argument `YES`.

## Unregistering an Observer

Before an object that is observing notifications is [deallocated](../../../../General/Conceptual/DevPedia-CocoaCore/MemoryManagement.html#//apple_ref/doc/uid/TP40008195-CH27), it must tell the notification center to stop sending it notifications. Otherwise, the next notification gets sent to a nonexistent object and the program crashes. You can send the following message to completely remove an object as an observer of local notifications, regardless of how many objects and notifications for which it registered itself:

```
[[NSNotificationCenter defaultCenter] removeObserver:self];
```

For observers of distributed notifications send:

```
[[NSDistributedNotificationCenter defaultCenter] removeObserver:self];
```

Use the more specific `removeObserver...` methods that specify the notification name and observed object to selectively unregister an object for particular notifications.

        
            [Next](Posting.html)[Previous](NotificationQueues.html)
        
         Copyright &#x00a9; 2009 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2009-08-18