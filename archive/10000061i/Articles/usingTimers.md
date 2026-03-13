---
title: "Using Timers"
book: "Timer Programming Topics"
chapterId: "20000807"
date: "2009-07-14"
description: "Explains how to use timers for scheduling automatic, repeating message invocations in Cocoa."
identifier: "//apple_ref/doc/uid/10000061i"
source: apple-archive
---

# Using Timers

> **Timer Programming Topics**
> Last updated: 2009-07-14

[Next](../RevisionHistory.html)[Previous](timerConcepts.html)
        
        
        [](../index.html)
        
        # Using Timers

There are several aspects to using a timer. When you create a timer, you must configure it so that it knows what message to send to what object when it fires. You must then associate it with a run loop so that it will fire—some of the creation methods do this for you automatically. Finally, if you create a repeating timer, you must invalidate it when you want it to stop firing.

## Creating and Scheduling a Timer
There are, broadly speaking, three ways to create a timer: 

Scheduling a timer with the current run loop;

Creating a timer that you later register with a run loop;

Initializing a timer with a given fire date.

In all cases, you have to configure the timer to tell it what message it should send to what object when it fires, and whether it should repeat. With some methods, you may also provide a user info dictionary. You can put whatever you want into this dictionary that may be useful in the method that the timer invokes when it fires.

There are two ways to tell a timer what message it should send and the object to which it should send the message—by specifying each independently, or (in some cases) by using an instance of `NSInvocation`. If you specify the [selector](../../../../General/Conceptual/DevPedia-CocoaCore/Selector.html#//apple_ref/doc/uid/TP40008195-CH48) for the message directly, the name of the method does not matter but it must have the following signature:

- (void)targetMethod:(NSTimer*)theTimerIf you create an invocation object, you can specify whatever message you want. (For more about invocation objects, see [Using NSInvocation](../../DistrObjects/Tasks/invocations.html#//apple_ref/doc/uid/20000744) in *[Distributed Objects Programming Topics](../../DistrObjects/DistrObjects.html#//apple_ref/doc/uid/10000102i)*.)

### References to Timers and Object Lifetimes
Because the run loop maintains the timer, from the perspective of object lifetimes there’s typically no *need* to keep a reference to a timer *after you’ve scheduled it*. (Because the timer is passed as an argument when you specify its method as a selector, you can invalidate a repeating timer when appropriate within that method.) In many situations, however, you also want the option of invalidating the timer—perhaps even before it starts. In this case, you *do* need to keep a reference to the timer, so that you can stop it whenever appropriate. If you create an unscheduled timer (see [Unscheduled Timers](#//apple_ref/doc/uid/20000807-SW3)), then you must maintain a strong reference to the timer so that it is not deallocated before you use it.

A timer maintains a strong reference to its target. This means that as long as a timer remains valid, its target will not be deallocated. As a corollary, this means that it does not make sense for a timer’s target to try to invalidate the timer in its `dealloc` method—the `dealloc` method will not be invoked as long as the timer is valid. 

### Timer Examples
For the examples that follow, consider a timer controller object that declares methods to start and (in some cases) stop four timers configured in different ways. It has properties for two of the timers; a property to count how many times one of the timers has fired, and three timer-related methods (`targetMethod:`, `invocationMethod:`, and `countedTimerFireMethod:`). The controller also provides a method to supply a user info dictionary.

@interface TimerController : NSObject
```
 
```

```
// The repeating timer is a weak property.
```

```
@property (weak) NSTimer *repeatingTimer;
```

```
@property (strong) NSTimer *unregisteredTimer;
```

```
@property NSUInteger timerCount;
```

```
 
```

```
- (IBAction)startOneOffTimer:sender;
```

```
 
```

```
- (IBAction)startRepeatingTimer:sender;
```

```
- (IBAction)stopRepeatingTimer:sender;
```

```
 
```

```
- (IBAction)createUnregisteredTimer:sender;
```

```
- (IBAction)startUnregisteredTimer:sender;
```

```
- (IBAction)stopUnregisteredTimer:sender;
```

```
 
```

```
- (IBAction)startFireDateTimer:sender;
```

```
 
```

```
- (void)targetMethod:(NSTimer*)theTimer;
```

```
- (void)invocationMethod:(NSDate *)date;
```

```
- (void)countedTimerFireMethod:(NSTimer*)theTimer;
```

```
 
```

```
- (NSDictionary *)userInfo;
```

```
 
```

```
@end
```
The implementations of the user info method and two of the methods invoked by the timers might be as follows (`countedTimerFireMethod:` is described in [Stopping a Timer](#//apple_ref/doc/uid/20000807-SW5)):

```
- (NSDictionary *)userInfo {
```

```
    return @{ @"StartDate" : [NSDate date] };
```

```
}
```

```
 
```

```
- (void)targetMethod:(NSTimer*)theTimer {
```

```
    NSDate *startDate = [[theTimer userInfo] objectForKey:@"StartDate"];
```

```
    NSLog(@"Timer started on %@", startDate);
```

```
}
```

```
 
```

```
- (void)invocationMethod:(NSDate *)date {
```

```
    NSLog(@"Invocation for timer started on %@", date);
```

```
}
```
## Scheduled Timers
 The following two class methods automatically register the new timer with the current `NSRunLoop` object in the default mode (`[NSDefaultRunLoopMode](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/data/NSDefaultRunLoopMode)`):

`[scheduledTimerWithTimeInterval:invocation:repeats:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTimer/Description.html#//apple_ref/occ/clm/NSTimer/scheduledTimerWithTimeInterval:invocation:repeats:)`

`[scheduledTimerWithTimeInterval:target:selector:userInfo:repeats:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTimer/Description.html#//apple_ref/occ/clm/NSTimer/scheduledTimerWithTimeInterval:target:selector:userInfo:repeats:)`

The following example shows how you can schedule a one-off timer that uses a selector:

- (IBAction)startOneOffTimer:sender {
```
 
```

```
    [NSTimer scheduledTimerWithTimeInterval:2.0
```

```
             target:self
```

```
             selector:@selector(targetMethod:)
```

```
             userInfo:[self userInfo]
```

```
             repeats:NO];
```

```
}
```
The timer is automatically fired by the run loop after 2 seconds, and is then removed from the run loop.

The next example shows how you can schedule a repeating timer, that again uses a selector (invalidation is described in [Stopping a Timer](#//apple_ref/doc/uid/20000807-SW5)):

- (IBAction)startRepeatingTimer:sender {
```
 
```

```
    // Cancel a preexisting timer.
```

```
    [self.repeatingTimer invalidate];
```

```
 
```

```
    NSTimer *timer = [NSTimer scheduledTimerWithTimeInterval:0.5
```

```
                              target:self selector:@selector(targetMethod:)
```

```
                              userInfo:[self userInfo] repeats:YES];
```

```
    self.repeatingTimer = timer;
```

```
}
```
If you create a repeating timer, you usually need to save a reference to it so that you can stop the timer at a later stage (see [Initializing a Timer with a Fire Date](#//apple_ref/doc/uid/20000807-SW4) for an example of when this is not the case).

## Unscheduled Timers

The following methods create timers that you may schedule at a later time by sending the message `[addTimer:forMode:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRunLoop/Description.html#//apple_ref/occ/instm/NSRunLoop/addTimer:forMode:)` to an `NSRunLoop` object.

`[timerWithTimeInterval:invocation:repeats:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTimer/Description.html#//apple_ref/occ/clm/NSTimer/timerWithTimeInterval:invocation:repeats:)`

 `[timerWithTimeInterval:target:selector:userInfo:repeats:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTimer/Description.html#//apple_ref/occ/clm/NSTimer/timerWithTimeInterval:target:selector:userInfo:repeats:)`

The following example shows how you can create a timer that uses an invocation object in one method, and then, in another method, start the timer by adding it to a run loop:

```
- (IBAction)createUnregisteredTimer:sender {
```

```
 
```

```
    NSMethodSignature *methodSignature = [self methodSignatureForSelector:@selector(invocationMethod:)];
```

```
    NSInvocation *invocation = [NSInvocation invocationWithMethodSignature:methodSignature];
```

```
    [invocation setTarget:self];
```

```
    [invocation setSelector:@selector(invocationMethod:)];
```

```
    NSDate *startDate = [NSDate date];
```

```
    [invocation setArgument:&startDate atIndex:2];
```

```
 
```

```
    NSTimer *timer = [NSTimer timerWithTimeInterval:0.5 invocation:invocation repeats:YES];
```

```
    self.unregisteredTimer = timer;
```

```
}
```

```
 
```

```
- (IBAction)startUnregisteredTimer:sender {
```

```
 
```

```
    if (self.unregisteredTimer != nil) {
```

```
        NSRunLoop *runLoop = [NSRunLoop currentRunLoop];
```

```
        [runLoop addTimer:self.unregisteredTimer forMode:NSDefaultRunLoopMode];
```

```
    }
```

```
}
```
## Initializing a Timer with a Fire Date

You can allocate an `NSTimer` object yourself and send it an `[initWithFireDate:interval:target:selector:userInfo:repeats:](https://developer.apple.com/documentation/foundation/nstimer/1415700-initwithfiredate)` message. This allows you to specify an initial fire date independently of the repeat interval. Once you’ve created a timer, the only property you can modify is its firing date (using `setFireDate:`). All other parameters are immutable after creating the timer. To cause the timer to start firing, you must add it to a run loop.

The following example shows how you can create a timer with a given start time (in this case, one second in the future), and then start the timer by adding it to a run loop:

- (IBAction)startFireDateTimer:sender {
```
 
```

```
    NSDate *fireDate = [NSDate dateWithTimeIntervalSinceNow:1.0];
```

```
    NSTimer *timer = [[NSTimer alloc] initWithFireDate:fireDate
```

```
                                      interval:0.5
```

```
                                      target:self
```

```
                                      selector:@selector(countedTimerFireMethod:)
```

```
                                      userInfo:[self userInfo]
```

```
                                      repeats:YES];
```

```
 
```

```
    self.timerCount = 1;
```

```
    NSRunLoop *runLoop = [NSRunLoop currentRunLoop];
```

```
    [runLoop addTimer:timer forMode:NSDefaultRunLoopMode];
```

```
}
```
In this example, although the timer is configured to repeat, it will be stopped after it has fired three times by the `countedTimerFireMethod:` that it invokes—see [Stopping a Timer](#//apple_ref/doc/uid/20000807-SW5).

## Stopping a Timer
If you create a non-repeating timer, there is no need to take any further action. It automatically stops itself after it fires. For example, there is no need to stop the timer created in the [Initializing a Timer with a Fire Date](#//apple_ref/doc/uid/20000807-SW4). If you create a repeating timer, however, you stop it by sending it an `invalidate` message. You can also send a non-repeating timer an `invalidate` message before it fires to prevent it from firing. 

The following examples show the stop methods for the timers created in the previous examples:

- (IBAction)stopRepeatingTimer:sender {
```
    [self.repeatingTimer invalidate];
```

```
    self.repeatingTimer = nil;
```

```
}
```

```
 
```

```
- (IBAction)stopUnregisteredTimer:sender {
```

```
    [self.unregisteredTimer invalidate];
```

```
    self.unregisteredTimer = nil;
```

```
}
```
You can also invalidate a timer from the method it invokes. For example, the method invoked by the timer shown in [Initializing a Timer with a Fire Date](#//apple_ref/doc/uid/20000807-SW4) might look like this:

- (void)countedTimerFireMethod:(NSTimer*)theTimer {
```
 
```

```
    NSDate *startDate = [[theTimer userInfo] objectForKey:@"StartDate"];
```

```
    NSLog(@"Timer started on %@; fire count %d", startDate, self.timerCount);
```

```
 
```

```
    self.timerCount++;
```

```
    if (self.timerCount > 3) {
```

```
        [theTimer invalidate];
```

```
    }
```

```
}
```
This will invalidate the timer after it has fired three times. Because the timer is passed as an argument to the method it invokes, there may be no need to maintain the timer as a variable. Typically, however, you might nevertheless keep a reference to the timer in case you want the option of stopping it earlier.

        
            [Next](../RevisionHistory.html)[Previous](timerConcepts.html)
        
         Copyright &#x00a9; 2009 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2009-07-14