---
title: "Introduction"
book: "Threading Programming Guide"
chapterId: "10000057i-CH1"
date: "2014-07-15"
description: "Explains how to use threads in Cocoa applications."
identifier: "//apple_ref/doc/uid/10000057i"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Introduction

> **Threading Programming Guide**
> Last updated: 2014-07-15

[Next](../AboutThreads/AboutThreads.html)
        
        
        [](../index.html)
        
        # Introduction
Threads are one of several technologies that make it possible to execute multiple code paths concurrently inside a single application. Although newer technologies such as operation objects and Grand Central Dispatch (GCD) provide a more modern and efficient infrastructure for implementing concurrency, OS X and iOS also provide interfaces for creating and managing threads. 

This document provides an introduction to the thread packages available in OS X and shows you how to use them. This document also describes the relevant technologies provided to support threading and the synchronization of multithreaded code inside your application. 

> **Important:** If you are developing a new application, you are encouraged to investigate the alternative OS X technologies for implementing concurrency. This is especially true if you are not already familiar with the design techniques needed to implement a threaded application. These alternative technologies simplify the amount of work you have to do to implement concurrent paths of execution and offer much better performance than traditional threads. For information about these technologies, see *[Concurrency Programming Guide](../../../../General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)*.

## Organization of This Document
This document has the following chapters and appendixes:

[About Threaded Programming](../AboutThreads/AboutThreads.html#//apple_ref/doc/uid/10000057i-CH6-SW2) introduces the concept of threads and their role in application design. 

[Thread Management](../CreatingThreads/CreatingThreads.html#//apple_ref/doc/uid/10000057i-CH15-SW2) provides information about the threading technologies in OS X and how you use them. 

[Run Loops](../RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW1) provides information about how to manage event-processing loops in secondary threads. 

[Synchronization](../ThreadSafety/ThreadSafety.html#//apple_ref/doc/uid/10000057i-CH8-SW1) describes synchronization issues and the tools you use to prevent multiple threads from corrupting data or crashing your program. 

[Thread Safety Summary](../ThreadSafetySummary/ThreadSafetySummary.html#//apple_ref/doc/uid/10000057i-CH12-SW1) provides a high-level summary of the inherent thread safety of OS X and iOS and some of their key frameworks. 

## See Also
For information about the alternatives to threads, see *[Concurrency Programming Guide](../../../../General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)*. 

This document provides only a light coverage of the use of the POSIX threads API. For more information about the available POSIX thread routines, see the `[pthread](../../../../System/Conceptual/ManPages_iPhoneOS/man3/pthread.3.html#//apple_ref/doc/man/3/pthread)` man page. For a more in-depth explanation of POSIX threads and their usage, see *Programming with POSIX Threads* by David R. Butenhof. 

        
            [Next](../AboutThreads/AboutThreads.html)
        
         Copyright &#x00a9; 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-07-15