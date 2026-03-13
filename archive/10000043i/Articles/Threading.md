---
title: "Delivering Notifications To Particular Threads"
book: "Notification Programming Topics"
chapterId: "20001289"
date: "2009-08-18"
description: "Explains how to send and receive information about events in Cocoa programs."
identifier: "//apple_ref/doc/uid/10000043i"
source: apple-archive
---

# Delivering Notifications To Particular Threads

> **Notification Programming Topics**
> Last updated: 2009-08-18

[Next](../RevisionHistory.html)[Previous](Posting.html)
        
        
        [](../index.html)
        
        # Delivering Notifications To Particular Threads
Regular notification centers deliver notifications on the thread in which the notification was posted. Distributed notification centers deliver notifications on the main thread. At times, you may require notifications to be delivered on a particular thread that is determined by you instead of the notification center. For example, if an object running in a background thread is listening for notifications from the user interface, such as a window closing, you would like to receive the notifications in the background thread instead of the main thread. In these cases, you must capture the notifications as they are delivered on the default thread and redirect them to the appropriate thread.

One way to redirect notifications is to use a custom notification queue (not an `NSNotificationQueue` object) to hold any notifications that are received on incorrect threads and then process them on the correct thread. This technique works as follows. You register for a notification normally. When a notification arrives, you test whether the current thread is the thread that should handle the notification. If it is the wrong thread, you store the notification in a queue and then send a signal to the correct thread, indicating that a notification needs processing. The other thread receives the signal, removes the notification from the queue, and processes the notification.

To implement this technique, your observer object needs to have instance variables for the following values: a [mutable](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectMutability.html#//apple_ref/doc/uid/TP40008195-CH42) array to hold the notifications, a communication port for signaling the correct thread (a Mach port), a lock to prevent multithreading conflicts with the notification array, and a value that identifies the correct thread (an `NSThread` object). You also need methods to setup the variables, to process the notifications, and to receive the Mach messages. Here are the necessary definitions to add to the class of your observer object.

@interface MyThreadedClass: NSObject
```
/* Threaded notification support. */
```

```
@property NSMutableArray *notifications;
```

```
@property NSThread *notificationThread;
```

```
@property NSLock *notificationLock;
```

```
@property NSMachPort *notificationPort;
```

```
 
```

```
- (void) setUpThreadingSupport;
```

```
- (void) handleMachMessage:(void *)msg;
```

```
- (void) processNotification:(NSNotification *)notification;
```

```
@end
```
Before registering for any notifications, you need to [initialize](../../../../General/Conceptual/DevPedia-CocoaCore/Initialization.html#//apple_ref/doc/uid/TP40008195-CH21) the properties. The following method initializes the queue and lock objects, keeps a reference to the current thread object, and creates a Mach communication port, which it adds to the current thread’s run loop.

- (void) setUpThreadingSupport {
```
    if (self.notifications) {
```

```
        return;
```

```
    }
```

```
    self.notifications      = [[NSMutableArray alloc] init];
```

```
    self.notificationLock   = [[NSLock alloc] init];
```

```
    self.notificationThread = [NSThread currentThread];
```

```
 
```

```
    self.notificationPort = [[NSMachPort alloc] init];
```

```
    [self.notificationPort setDelegate:self];
```

```
    [[NSRunLoop currentRunLoop] addPort:self.notificationPort
```

```
            forMode:(NSString __bridge *)kCFRunLoopCommonModes];
```

```
}
```
After this method runs, any messages sent to `notificationPort` are received in the run loop of the thread that first ran this method. If the receiving thread’s run loop is not running when the Mach message arrives, the kernel holds on to the message until the next time the run loop is entered. The receiving thread’s run loop sends the incoming messages to the `handleMachMessage:` method of the port’s [delegate](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14).

In this implementation, no information is contained in the messages sent to `notificationPort`. Instead, the information passed between threads is contained in the notification array. When a Mach message arrives, the `handleMachMessage:` method ignores the contents of the message and just checks the  `notifications` array for any notifications that need processing. The notifications are removed from the array and forwarded to the real notification processing method. Because port messages may get dropped if too many are sent simultaneously, the `handleMachMessage:` method [iterates](../../../../General/Conceptual/DevPedia-CocoaCore/Enumeration.html#//apple_ref/doc/uid/TP40008195-CH17) over the array until it is empty. The method must acquire a lock when accessing the notification array to prevent conflicts between one thread adding notifications and another removing notifications from the array.

- (void) handleMachMessage:(void *)msg {
```
 
```

```
    [self.notificationLock lock];
```

```
 
```

```
    while ([self.notifications count]) {
```

```
        NSNotification *notification = [self.notifications objectAtIndex:0];
```

```
        [self.notifications removeObjectAtIndex:0];
```

```
        [self.notificationLock unlock];
```

```
        [self processNotification:notification];
```

```
        [self.notificationLock lock];
```

```
    };
```

```
 
```

```
    [self.notificationLock unlock];
```

```
}
```
When a notification is delivered to your object, the method that receives the notification must identify whether it is running in the correct thread or not. If it is the correct thread, the notification is processed normally. If it is the wrong thread, the notification is added to the queue and the notification port signaled.

- (void)processNotification:(NSNotification *)notification {
```
 
```

```
    if ([NSThread currentThread] != notificationThread) {
```

```
        // Forward the notification to the correct thread.
```

```
        [self.notificationLock lock];
```

```
        [self.notifications addObject:notification];
```

```
        [self.notificationLock unlock];
```

```
        [self.notificationPort sendBeforeDate:[NSDate date]
```

```
                components:nil
```

```
                from:nil
```

```
                reserved:0];
```

```
    }
```

```
    else {
```

```
        // Process the notification here;
```

```
    }
```

```
}
```
Finally, to register for a notification that you want delivered on the current thread, regardless of the thread in which it may be posted, you must initialize your object’s notification properties by invoking `setUpThreadingSupport` and then register for the notification normally, specifying the special notification processing method as the [selector](../../../../General/Conceptual/DevPedia-CocoaCore/Selector.html#//apple_ref/doc/uid/TP40008195-CH48).

[self setupThreadingSupport];
```
[[NSNotificationCenter defaultCenter]
```

```
        addObserver:self
```

```
        selector:@selector(processNotification:)
```

```
        name:@"NotificationName"
```

```
        object:nil];
```
This implementation is limited in several aspects. First, all threaded notifications processed by this object must pass through the same method  (`processNotification:`). Second, each object must provide its own implementation and communication port. A better, but more complex, implementation would generalize the behavior into either a subclass of `NSNotificationCenter` or a separate class that would have one notification queue for each thread and be able to deliver notifications to multiple observer objects and methods.

        
            [Next](../RevisionHistory.html)[Previous](Posting.html)
        
         Copyright &#x00a9; 2009 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2009-08-18