---
title: "Introduction"
book: "Timer Programming Topics"
chapterId: "10000061"
date: "2009-07-14"
description: "Explains how to use timers for scheduling automatic, repeating message invocations in Cocoa."
identifier: "//apple_ref/doc/uid/10000061i"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Introduction

> **Timer Programming Topics**
> Last updated: 2009-07-14

[Next](Articles/timerConcepts.html)
        
        
        [](index.html)
        
        # Introduction to Timers

> **Important:** This document is no longer being updated. For the latest information about Apple SDKs, visit the [documentation website](https://developer.apple.com/documentation).

A timer provides a way to perform a delayed action or a periodic action. The timer waits until a certain time interval has elapsed and then fires, sending a specified message to a specified object. For example, you could create a timer that sends a message to a controller object, telling it to update a particular value after a certain time interval. 

## At a Glance
Timers work in conjunction with `NSRunLoop` objects. As a result, they don’t provide a real-time mechanism—their accuracy is limited.

For more about timers in general, see [Timers](Articles/timerConcepts.html#//apple_ref/doc/uid/20000806-BAJFBAIH).

There are several aspects to using a timer. When you create a timer, you must configure it so that it knows what message to send to what object when it fires. You must then associate it with a run loop so that it will fire—some of the creation methods do this for you automatically. Finally, if you create a repeating timer, you must invalidate it when you want it to stop firing.

To learn more about using timers, see [Using Timers](Articles/usingTimers.html#//apple_ref/doc/uid/20000807-CJBJCBDE).

        
            [Next](Articles/timerConcepts.html)
        
         Copyright &#x00a9; 2009 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2009-07-14