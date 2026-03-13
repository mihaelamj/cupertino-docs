---
title: "Notification Queues"
book: "Notification Programming Topics"
chapterId: "20000217"
date: "2009-08-18"
description: "Explains how to send and receive information about events in Cocoa programs."
identifier: "//apple_ref/doc/uid/10000043i"
source: apple-archive
---

# Notification Queues

> **Notification Programming Topics**
> Last updated: 2009-08-18

[Next](Registering.html)[Previous](NotificationCenters.html)
        
        
        [](../index.html)
        
        # Notification Queues

`NSNotificationQueue` objects, or simply, *notification queues*, act as buffers for notification centers (instances of `NSNotificationCenter`). The `NSNotificationQueue` class contributes two important features to the Foundation Kit’s notification mechanism: the coalescing of notifications and asynchronous posting. 

## Notification Queue Basics
Using the `NSNotificationCenter`’s `[postNotification:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNotificationCenter/Description.html#//apple_ref/occ/instm/NSNotificationCenter/postNotification:)` method and its variants, you can post a notification to a notification center. However, the invocation of the method is synchronous: before the posting object can resume its thread of execution, it must wait until the notification center dispatches the notification to all observers and returns. A notification queue, on the other hand, maintains notifications (instances of `NSNotification`) generally in a First In First Out (FIFO) order. When a notification rises to the front of the queue, the queue posts it to the notification center, which in turn dispatches the notification to all objects registered as observers.

Every thread has a default notification queue, which is associated with the default notification center for the process. You can [create](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39) your own notification queues and have multiple queues per center and thread.

## Posting Notifications Asynchronously

With `NSNotificationQueue`’s `[enqueueNotification:postingStyle:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNotificationQueue/Description.html#//apple_ref/occ/instm/NSNotificationQueue/enqueueNotification:postingStyle:)` and `[enqueueNotification:postingStyle:coalesceMask:forModes:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNotificationQueue/Description.html#//apple_ref/occ/instm/NSNotificationQueue/enqueueNotification:postingStyle:coalesceMask:forModes:)` methods, you can post a notification asynchronously to the current thread by putting it in a queue. These methods immediately return to the invoking object after putting the notification in the queue.

> **Note:** When the thread where a notification is enqueued terminates before the notification queue posts the notification to its notification center, the notification is not posted. See [Delivering Notifications To Particular Threads](Threading.html#//apple_ref/doc/uid/20001289-CEGJFDFG) to learn how to post a notification to a different thread.

The notification queue is emptied and its notifications posted based on the posting style and run loop mode specified in the enqueuing method. The mode argument specifies the run loop mode in which the queue will be emptied. For example, if you specify `NSModalPanelRunLoopMode`, the notifications will be posted only when the run loop is in this mode. If the run loop is not currently in this mode, the notifications wait until the next time that mode is entered. See [Run Loop Modes](../../Multithreading/RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW12) for more information.

Posting to a notification queue can occur in one of three different styles: `NSPostASAP`, `NSPostWhenIdle`, and `NSPostNow`. These styles are described in the following sections. 

### Posting As Soon As Possible

Any notification queued with the `NSPostASAP` style is posted to the notification center when the current iteration of the run loop completes, assuming the current run loop mode matches the requested mode. (If the requested and current modes are different, the notification is posted when the requested mode is entered.) Because the run loop can make multiple callouts during each iteration, the notification may or may not get delivered as soon as the current callout exits and control returns to the run loop. Other callouts may take place first, such as a timer or source firing or other asynchronous notifications being delivered.

You typically use the `NSPostASAP` posting style for an expensive resource, such as the display server. When many clients draw on the window buffer during a callout from the run loop, it is expensive to flush the buffer to the display server after every draw operation. In this situation, each `draw...` method enqueues some notification such as “FlushTheServer” with coalescing on name and object specified and with a posting style of `NSPostASAP`. As a result, only one of those notifications is dispatched at the end of the run loop and the window buffer is flushed only once.

### Posting When Idle

A notification queued with the `NSPostWhenIdle` style is posted only when the run loop is in a wait state. In this state, there’s nothing in the run loop’s input channels, be it timers or other asynchronous events. A typical example of queuing with the `NSPostWhenIdle` style occurs when the user types text, and the program displays the size of the text in bytes somewhere. It would be very expensive (and not very useful) to update the text field size after each character the user types, especially if the user types quickly. In this case, the program queues a notification, such as “ChangeTheDisplayedSize,” with coalescing turned on and a posting style of `NSPostWhenIdle` after each character typed. When the user stops typing, the single “ChangeTheDisplayedSize” notification in the queue (due to coalescing) is posted when the run loop enters its wait state and the display is updated. Note that a run loop that is about to exit (which occurs when all of the input channels have expired) is not in a wait state and thus will not post a notification.

### Posting Immediately

A notification queued with `NSPostNow` is posted immediately after coalescing to the notification center. You queue a notification with `NSPostNow` (or post one with `[postNotification:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNotificationCenter/Description.html#//apple_ref/occ/instm/NSNotificationCenter/postNotification:)`) when you do not require asynchronous calling behavior. For many programming situations, synchronous behavior is not only allowable but desirable: You want the notification center to return after dispatching so you can be sure that observing objects have received and processed the notification. Of course, you should use `enqueueNotification... ` with `NSPostNow` rather than use `postNotification:` when there are similar notifications in the queue that you want to remove through coalescing.

## Coalescing Notifications
In some situations, you may want to post a notification if a given event occurs at least once, but you want to post no more than one notification even if the event occurs multiple times. For example, in an application that receives data in discrete packets, upon receipt of a packet you may wish to post a notification to signify that the data needs to be processed. If multiple packets arrive within a given time period, however, you do not want to post multiple notifications. Moreover, the object that posts these notifications may not have any way of knowing whether more packets are coming or not, whether the posting method is called in a loop or not.

In some situations it may be possible to simply set a Boolean flag (whether an instance variable of an object or a global variable) to denote that an event has occurred and to suppress posting of further notifications until the flag is cleared. If this is not possible, however, in this situation you cannot directly use `NSNotificationCenter` since its behavior is synchronous—notifications are posted before returning, thus there is no opportunity for "ignoring” duplicate notifications; moreover, an `NSNotificationCenter` instance has no way of knowing whether more notifications are coming or not.

Rather than posting a notification to a notification center, therefore, you can add the notification to an `NSNotificationQueue` instance specifying an appropriate option for *coalescing*. Coalescing is a process that removes from a queue notifications that are similar in some way to a notification that was queued earlier. You indicate the criteria for similarity by specifying one or more of the following constants in the third argument of the `[enqueueNotification:postingStyle:coalesceMask:forModes:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNotificationQueue/Description.html#//apple_ref/occ/instm/NSNotificationQueue/enqueueNotification:postingStyle:coalesceMask:forModes:)` method.

`[NSNotificationNoCoalescing](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/econst/NSNotificationNoCoalescing)``[NSNotificationCoalescingOnName](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/econst/NSNotificationCoalescingOnName)``[NSNotificationCoalescingOnSender](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/econst/NSNotificationCoalescingOnSender)`
You can perform a bitwise-OR operation with the `NSNotificationCoalescingOnName` and `NSNotificationCoalescingOnSender` constants to specify coalescing using both the notification name and notification object. The following example illustrates how you might use a queue to ensure that, within a given event loop cycle, all notifications named `MyNotificationName` are coalesced into a single notification. 

```
// MyNotificationName defined globally
```

```
NSString *MyNotificationName = @"MyNotification";
```

```
 
```

```
id object = <#The object associated with the notification#>;
```

```
NSNotification *myNotification =
```

```
        [NSNotification notificationWithName:MyNotificationName object:object]
```

```
[[NSNotificationQueue defaultQueue]
```

```
        enqueueNotification:myNotification
```

```
        postingStyle:NSPostWhenIdle
```

```
        coalesceMask:NSNotificationCoalescingOnName
```

```
        forModes:nil];
```

        
            [Next](Registering.html)[Previous](NotificationCenters.html)
        
         Copyright &#x00a9; 2009 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2009-08-18