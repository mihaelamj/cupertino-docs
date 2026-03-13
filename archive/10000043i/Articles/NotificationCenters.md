---
title: "Notification Centers"
book: "Notification Programming Topics"
chapterId: "20000216"
date: "2009-08-18"
description: "Explains how to send and receive information about events in Cocoa programs."
identifier: "//apple_ref/doc/uid/10000043i"
source: apple-archive
---

# Notification Centers

> **Notification Programming Topics**
> Last updated: 2009-08-18

[Next](NotificationQueues.html)[Previous](Notifications.html)
        
        
        [](../index.html)
        
        # Notification Centers

A *notification center* manages the sending and receiving of notifications. It notifies all observers of notifications meeting specific criteria. The notification information is encapsulated in `NSNotification` objects. Client objects register themselves with the notification center as observers of specific notifications posted by other objects. When an event occurs, an object posts an appropriate notification to the notification center. (See [Posting a Notification](Posting.html#//apple_ref/doc/uid/20000724-CEGJFDFG) for more on posting notifications.) The notification center dispatches a [message](../../../../General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59) to each registered observer, passing the notification as the sole argument. It is possible for the posting object and the observing object to be the same.

[Cocoa](../../../../General/Conceptual/DevPedia-CocoaCore/Cocoa.html#//apple_ref/doc/uid/TP40008195-CH9) includes two types of notification centers:

The `NSNotificationCenter` class manages notifications within a single process.

The `NSDistributedNotificationCenter` class manages notifications across multiple processes on a single computer.

## NSNotificationCenter

Each process has a default notification center that you access with the `NSNotificationCenter +defaultCenter` [class method](../../../../General/Conceptual/DevPedia-CocoaCore/ClassMethod.html#//apple_ref/doc/uid/TP40008195-CH8). This notification center handles notifications within a single process. For communication between processes on the same machine, use a distributed notification center (see [NSDistributedNotificationCenter](#//apple_ref/doc/uid/20000216-111391)).

A notification center delivers notifications to observers synchronously. In other words, when posting a notification, control does not return to the poster until all observers have received and processed the notification. To send notifications asynchronously use a notification queue, which is described in [Notification Queues](NotificationQueues.html#//apple_ref/doc/uid/20000217-CJBCECJC).

In a multithreaded application, notifications are always delivered in the thread in which the notification was posted, which may not be the same thread in which an observer registered itself.

## NSDistributedNotificationCenter

Each process has a default distributed notification center that you access with the `NSDistributedNotificationCenter +defaultCenter` [class method](../../../../General/Conceptual/DevPedia-CocoaCore/ClassMethod.html#//apple_ref/doc/uid/TP40008195-CH8). This distributed notification center handles notifications that can be sent between processes on a single machine. For communication between processes on different machines, use distributed objects (see *[Distributed Objects Programming Topics](../../DistrObjects/DistrObjects.html#//apple_ref/doc/uid/10000102i)*).

Posting a distributed notification is an expensive operation. The notification gets sent to a systemwide server that then distributes it to all the processes that have objects registered for distributed notifications. The latency between posting the notification and the notification’s arrival in another process is unbounded. In fact, if too many notifications are being posted and the server’s queue fills up, notifications can be dropped.

Distributed notifications are delivered via a process’s run loop. A process must be running a run loop in one of the “common” modes, such as `NSDefaultRunLoopMode`, to receive a distributed notification. If the receiving process is multithreaded, do not depend on the notification arriving on the main thread. The notification is usually delivered to the main thread’s run loop, but other threads could also receive the notification.

Whereas a regular notification center allows any object to be observed, a distributed notification center is restricted to observing a string object. Because the posting object and the observer may be in different processes, notifications cannot contain pointers to arbitrary objects. Therefore, a distributed notification center requires notifications to use a string as the notification object. Notification matching is done based on this string, rather than an object pointer.

        
            [Next](NotificationQueues.html)[Previous](Notifications.html)
        
         Copyright &#x00a9; 2009 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2009-08-18