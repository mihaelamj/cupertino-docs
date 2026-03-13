---
title: "Glossary"
book: "Threading Programming Guide"
chapterId: "10000057i-CH13"
date: "2014-07-15"
description: "Explains how to use threads in Cocoa applications."
identifier: "//apple_ref/doc/uid/10000057i"
source: apple-archive
---

# Glossary

> **Threading Programming Guide**
> Last updated: 2014-07-15

[Next](../RevisionHistory.html)[Previous](../ThreadSafetySummary/ThreadSafetySummary.html)
        
        
        [](../index.html)
        
        # Glossary

**application**A specific style of [program](#//apple_ref/doc/uid/10000057i-CH13-SW3) that displays a graphical interface to the user.

**condition**A construct used to synchronize access to a resource. A thread waiting on a condition is not allowed to proceed until another thread explicitly signals the condition. 

**critical section**A portion of code that must be executed by only one thread at a time.

**input source**A source of asynchronous events for a thread. Input sources can be port-based or manually triggered and must be attached to the thread’s run loop. 

**joinable thread**A thread whose resources are not reclaimed immediately upon termination. Joinable threads must be explicitly detached or be joined by another thread before the resources can be reclaimed. Joinable threads provide a return value to the thread that joins with them.

**main thread**A special type of [thread](#//apple_ref/doc/uid/10000057i-CH13-SW1) created when its owning process is created. When the main thread of a program exits, the process ends.  

**mutex**A lock that provides mutually exclusive access to a shared resource. A mutex lock can be held by only one thread at a time. Attempting to acquire a mutex held by a different thread puts the current thread to sleep until the lock is finally acquired.

**operation object**An instance of the `[NSOperation](https://developer.apple.com/documentation/foundation/nsoperation)` class. Operation objects wrap the code and data associated with a task into an executable unit.

**operation queue**An instance of the `[NSOperationQueue](https://developer.apple.com/documentation/foundation/operationqueue)` class. Operation queues manage the execution of operation objects.

**process**The runtime instance of an application or program. A process has its own virtual memory space and system resources (including port rights) that are independent of those assigned to other programs. A process always contains at least one thread (the main thread) and may contain any number of additional threads. 

**program**A combination of code and resources that can be run to perform some task. Programs need not have a graphical user interface, although graphical applications are also considered programs.

**recursive lock**A lock that can be locked multiple times by the same thread. 

**run loop**An event-processing loop, during which events are received and dispatched to appropriate handlers. 

**run loop mode**A collection of input sources, timer sources, and run loop observers associated with a particular name. When run in a specific “mode,” a run loop monitors only the sources and observers associated with that mode.

**run loop object**An instance of the `[NSRunLoop](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRunLoop/Description.html#//apple_ref/occ/cl/NSRunLoop)` class or `[CFRunLoopRef](https://developer.apple.com/documentation/corefoundation/cfrunloopref)` opaque type. These objects provide the interface for implementing an event-processing loop in a thread.

**run loop observer**A recipient of notifications during different phases of a run loop’s execution.

**semaphore**A protected variable that restricts access to a shared resource. Mutexes and conditions are both different types of semaphore.

**task**A quantity of work to be performed.

**thread**A flow of execution in a process. Each thread has its own stack space but otherwise shares memory with other threads in the same process. 

**timer source**A source of synchronous events for a thread. Timers generate one-shot or repeated events at a scheduled future time. 

        
            [Next](../RevisionHistory.html)[Previous](../ThreadSafetySummary/ThreadSafetySummary.html)
        
         Copyright &#x00a9; 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-07-15