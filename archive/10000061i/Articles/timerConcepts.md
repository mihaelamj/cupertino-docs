---
title: "Timers"
book: "Timer Programming Topics"
chapterId: "20000806"
date: "2009-07-14"
description: "Explains how to use timers for scheduling automatic, repeating message invocations in Cocoa."
identifier: "//apple_ref/doc/uid/10000061i"
source: apple-archive
---

# Timers

> **Timer Programming Topics**
> Last updated: 2009-07-14

[Next](usingTimers.html)[Previous](../Timers.html)
        
        
        [](../index.html)
        
        # Timers
Timers work in conjunction with `NSRunLoop` objects. As a result, they don’t provide a real-time mechanism—their accuracy is limited. If you simply want to send a message at some point in the future, you can do so without using a timer. 

## Timers and Run Loops
Timers are represented by `NSTimer` objects. They work in conjunction with `NSRunLoop` objects. `NSRunLoop` objects control loops that wait for input, and they use timers to help determine the maximum amount of time they should wait. When the timer’s time limit has elapsed, the run loop fires the timer (causing its message to be sent), then checks for new input. 

The run loop mode in which you register the timer must be running for the timer to fire. For applications built using the Application Kit or UIKit, the application object runs the main thread’s run loop for you. On secondary threads, however, you have to run the run loop yourself—see *[Run Loops](../../InputControl/InputControl.html#//apple_ref/doc/uid/10000062i)* for details.

Each run loop timer can be registered in only one run loop at a time, although it can be added to multiple run loop modes within that run loop.

## Timing Accuracy
A timer is not a real-time mechanism; it fires only when one of the run loop modes to which the timer has been added is running and able to check if the timer’s firing time has passed. Because of the various input sources a typical run loop manages, the effective resolution of the time interval for a timer is limited to on the order of 50-100 milliseconds. If a timer’s firing time occurs while the run loop is in a mode that is not monitoring the timer or during a long callout, the timer does not fire until the next time the run loop checks the timer. Therefore, the actual time at which the timer fires potentially can be a significant period of time after the scheduled firing time.

A repeating timer reschedules itself based on the scheduled firing time, not the actual firing time. For example, if a timer is scheduled to fire at a particular time and every 5 seconds after that, the scheduled firing time will always fall on the original 5 second time intervals, even if the actual firing time gets delayed. If the firing time is delayed so far that it passes one or more of the scheduled firing times, the timer is fired only once for that time period; the timer is then rescheduled, after firing, for the next scheduled firing time in the future.

## Alternatives to Timers
If you simply want to send a message at some point in the future, you can do so without using a timer. You can use `[performSelector:withObject:afterDelay:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/instm/NSObject/performSelector:withObject:afterDelay:)` and related methods to invoke a method directly on another object. Some variants, such as `[performSelectorOnMainThread:withObject:waitUntilDone:](https://developer.apple.com/documentation/objectivec/nsobject/1414900-performselector)`, allow you to invoke the method on a particular thread. You can also cancel a delayed message send using `[cancelPreviousPerformRequestsWithTarget:](https://developer.apple.com/documentation/objectivec/nsobject/1417611-cancelpreviousperformrequests)` and related methods.

        
            [Next](usingTimers.html)[Previous](../Timers.html)
        
         Copyright &#x00a9; 2009 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2009-07-14