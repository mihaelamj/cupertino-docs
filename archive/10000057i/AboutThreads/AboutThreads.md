---
title: "About Threaded Programming"
book: "Threading Programming Guide"
chapterId: "10000057i-CH6"
date: "2014-07-15"
description: "Explains how to use threads in Cocoa applications."
identifier: "//apple_ref/doc/uid/10000057i"
source: apple-archive
---

# About Threaded Programming

> **Threading Programming Guide**
> Last updated: 2014-07-15

[Next](../CreatingThreads/CreatingThreads.html)[Previous](../Introduction/Introduction.html)
        
        
        [](../index.html)
        
        # About Threaded Programming
For many years, maximum computer performance was limited largely by the speed of a single microprocessor at the heart of the computer. As the speed of individual processors started reaching their practical limits, however, chip makers switched to multicore designs, giving the computer the opportunity to perform multiple tasks simultaneously. And although OS X takes advantage of these cores whenever it can to perform system-related tasks, your own applications can also take advantage of them through threads. 

## What Are Threads?
Threads are a relatively lightweight way to implement multiple paths of execution inside of an application. At the system level, programs run side by side, with the system doling out execution time to each program based on its needs and the needs of other programs. Inside each program, however, exists one or more threads of execution, which can be used to perform different tasks simultaneously or in a nearly simultaneous manner. The system itself actually manages these threads of execution, scheduling them to run on the available cores and preemptively interrupting them as needed to allow other threads to run.

From a technical standpoint, a thread is a combination of the kernel-level and application-level data structures needed to manage the execution of code. The kernel-level structures coordinate the dispatching of events to the thread and the preemptive scheduling of the thread on one of the available cores. The application-level structures include the call stack for storing function calls and the structures the application needs to manage and manipulate the thread’s attributes and state.

In a non-concurrent application, there is only one thread of execution. That thread starts and ends with your application’s `main` routine and branches one-by-one to different methods or functions to implement the application’s overall behavior. By contrast, an application that supports concurrency starts with one thread and adds more as needed to create additional execution paths. Each new path has its own custom start routine that runs independently of the code in the application’s `main` routine. Having multiple threads in an application provides two very important potential advantages: 

Multiple threads can improve an application’s perceived responsiveness.

Multiple threads can improve an application’s real-time performance on multicore systems.

If your application has only one thread, that one thread must do everything. It must respond to events, update your application’s windows, and perform all of the computations needed to implement your application’s behavior. The problem with having just one thread is that it can only do one thing at a time. So what happens when one of your computations takes a long time to finish? While your code is busy computing the values it needs, your application stops responding to user events and updating its windows. If this behavior continues long enough, a user might think your application is hung and try to forcibly quit it. If you moved your custom computations onto a separate thread, however, your application’s main thread would be free to respond to user interactions in a more timely manner. 

With multicore computers common these days, threads provide a way to increase performance in some types of applications. Threads that perform different tasks can do so simultaneously on different processor cores, making it possible for an application to increase the amount of work it does in a given amount of time. 

Of course, threads are not a panacea for fixing an application’s performance problems. Along with the benefits offered by threads come the potential problems. Having multiple paths of execution in an application can add a considerable amount of complexity to your code. Each thread has to coordinate its actions with other threads to prevent it from corrupting the application’s state information. Because threads in a single application share the same memory space, they have access to all of the same data structures. If two threads try to manipulate the same data structure at the same time, one thread might overwrite another’s changes in a way that corrupts the resulting data structure. Even with proper protections in place, you still have to watch out for compiler optimizations that introduce subtle (and not so subtle) bugs into your code. 

## Threading Terminology
Before getting too far into discussions about threads and their supporting technologies, it is necessary to define some basic terminology. 

If you are familiar with UNIX systems, you may find that the term “task” is used differently by this document. On UNIX systems, the term “task” is used at times to refer to a running process.

This document adopts the following terminology:

The term *thread* is used to refer to a separate path of execution for code.

The term *process* is used to refer to a running executable, which can encompass multiple threads.

The term *task* is used to refer to the abstract concept of work that needs to be performed.

## Alternatives to Threads
One problem with creating threads yourself is that they add uncertainty to your code. Threads are a relatively low-level and complicated way to support concurrency in your application. If you do not fully understand the implications of your design choices, you could easily encounter synchronization or timing issues, the severity of which can range from subtle behavioral changes to the crashing of your application and the corruption of the user’s data.

Another factor to consider is whether you need threads or concurrency at all. Threads solve the specific problem of how to execute multiple code paths concurrently inside the same process. There may be cases, though, where the amount of work you are doing does not warrant concurrency. Threads introduce a tremendous amount of overhead to your process, both in terms of memory consumption and CPU time. You may discover that this overhead is too great for the intended task, or that other options are easier to implement.

Table 1-1 lists some of the alternatives to threads. This table includes both replacement technologies for threads (such as operation objects and GCD) and alternatives that are geared towards efficiently using the single thread you already have. 

**Table 1-1**  Alternative technologies to threadsTechnology

Description

Operation objects

Introduced in OS X v10.5, an operation object is a wrapper for a task that would normally be executed on a secondary thread. This wrapper hides the thread management aspects of performing the task, leaving you free to focus on the task itself. You typically use these objects in conjunction with an operation queue object, which actually manages the execution of the operation objects on one or more threads. 

For more information on how to use operation objects, see *[Concurrency Programming Guide](../../../../General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)*.

Grand Central Dispatch (GCD)

Introduced in Mac OS x v10.6, Grand Central Dispatch is another alternative to threads that lets you focus on the tasks you need to perform rather than on thread management. With GCD, you define the task you want to perform and add it to a work queue, which handles the scheduling of your task on an appropriate thread. Work queues take into account the number of available cores and the current load to execute your tasks more efficiently than you could do yourself using threads. 

For information on how to use GCD and work queues, see *[Concurrency Programming Guide](../../../../General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)*

Idle-time notifications

For tasks that are relatively short and very low priority, idle time notifications let you perform the task at a time when your application is not as busy. Cocoa provides support for idle-time notifications using the `[NSNotificationQueue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNotificationQueue/Description.html#//apple_ref/occ/cl/NSNotificationQueue)` object. To request an idle-time notification, post a notification to the default `NSNotificationQueue` object using the `[NSPostWhenIdle](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/econst/NSPostWhenIdle)` option. The queue delays the delivery of your notification object until the run loop becomes idle. For more information, see *[Notification Programming Topics](../../Notifications/Introduction/introNotifications.html#//apple_ref/doc/uid/10000043i)*. 

Asynchronous functions

The system interfaces include many asynchronous functions that provide automatic concurrency for you. These APIs may use system daemons and processes or create custom threads to perform their task and return the results to you. (The actual implementation is irrelevant because it is separated from your code.) As you design your application, look for functions that offer asynchronous behavior and consider using them instead of using the equivalent synchronous function on a custom thread.

Timers

You can use timers on your application’s main thread to perform periodic tasks that are too trivial to require a thread, but which still require servicing at regular intervals. For information on timers, see [Timer Sources](../RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW21). 

Separate processes

Although more heavyweight than threads, creating a separate process might be useful in cases where the task is only tangentially related to your application. You might use a process if a task requires a significant amount of memory or must be executed using root privileges. For example, you might use a 64-bit server process to compute a large data set while your 32-bit application displays the results to the user. 

> **Warning:** When launching separate processes using the `fork` function, you must always follow a call to `fork` with a call to `exec` or a similar function. Applications that depend on the Core Foundation, Cocoa, or Core Data frameworks (either explicitly or implicitly) must make a subsequent call to an `exec` function or those frameworks may behave improperly.

## Threading Support
If you have existing code that uses threads, OS X and iOS provide several technologies for creating threads in your applications. In addition, both systems also provide support for managing and synchronizing the work that needs to be done on those threads. The following sections describe some of the key technologies that you need to be aware of when working with threads in OS X and iOS.

### Threading Packages
Although the underlying implementation mechanism for threads is Mach threads, you rarely (if ever) work with threads at the Mach level. Instead, you usually use the more convenient POSIX API or one of its derivatives. The Mach implementation does provide the basic features of all threads, however, including the preemptive execution model and the ability to schedule threads so they are independent of each other. 

Listing 2-2 lists the threading technologies you can use in your applications. 

**Table 1-2**  Thread technologiesTechnology

Description

Cocoa threads 

Cocoa implements threads using the `[NSThread](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/cl/NSThread)` class. Cocoa also provides methods on `[NSObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/cl/NSObject)` for spawning new threads and executing code on already-running threads. For more information, see [Using NSThread](../CreatingThreads/CreatingThreads.html#//apple_ref/doc/uid/10000057i-CH15-SW11) and [Using NSObject to Spawn a Thread](../CreatingThreads/CreatingThreads.html#//apple_ref/doc/uid/10000057i-CH15-SW13).

POSIX threads

POSIX threads provide a C-based interface for creating threads. If you are not writing a Cocoa application, this is the best choice for creating threads. The POSIX interface is relatively simple to use and offers ample flexibility for configuring your threads. For more information, see [Using POSIX Threads](../CreatingThreads/CreatingThreads.html#//apple_ref/doc/uid/10000057i-CH15-SW12)

Multiprocessing Services

Multiprocessing Services is a legacy C-based interface used by applications transitioning from older versions of Mac OS. This technology is available in OS X only and should be avoided for any new development. Instead, you should use the `NSThread` class or POSIX threads. If you need more information on this technology, see *[Multiprocessing Services Programming Guide](../../../../Carbon/Conceptual/Multitasking_MultiproServ/01introduction/introduction.html#//apple_ref/doc/uid/TP40000853)*. 

At the application level, all threads behave in essentially the same way as on other platforms. After starting a thread, the thread runs in one of three main states: running, ready, or blocked. If a thread is not currently running, it is either blocked and waiting for input or it is ready to run but not scheduled to do so yet. The thread continues moving back and forth among these states until it finally exits and moves to the terminated state.

When you create a new thread, you must specify an entry-point function (or an entry-point method in the case of Cocoa threads) for that thread. This entry-point function constitutes the code you want to run on the thread. When the function returns, or when you terminate the thread explicitly, the thread stops permanently and is reclaimed by the system. Because threads are relatively expensive to create in terms of memory and time, it is therefore recommended that your entry point function do a significant amount of work or set up a run loop to allow for recurring work to be performed. 

For more information about the available threading technologies and how to use them, see [Thread Management](../CreatingThreads/CreatingThreads.html#//apple_ref/doc/uid/10000057i-CH15-SW2).

### Run Loops
A run loop is a piece of infrastructure used to manage events arriving asynchronously on a thread. A run loop works by monitoring one or more event sources for the thread. As events arrive, the system wakes up the thread and dispatches the events to the run loop, which then dispatches them to the handlers you specify. If no events are present and ready to be handled, the run loop puts the thread to sleep. 

You are not required to use a run loop with any threads you create but doing so can provide a better experience for the user. Run loops make it possible to create long-lived threads that use a minimal amount of resources. Because a run loop puts its thread to sleep when there is nothing to do, it eliminates the need for polling, which wastes CPU cycles and prevents the processor itself from sleeping and saving power. 

To configure a run loop, all you have to do is launch your thread, get a reference to the run loop object, install your event handlers, and tell the run loop to run. The infrastructure provided by OS X handles the configuration of the main thread’s run loop for you automatically. If you plan to create long-lived secondary threads, however, you must configure the run loop for those threads yourself.

Details about run loops and examples of how to use them are provided in [Run Loops](../RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW1). 

### Synchronization Tools
One of the hazards of threaded programming is resource contention among multiple threads. If multiple threads try to use or modify the same resource at the same time, problems can occur. One way to alleviate the problem is to eliminate the shared resource altogether and make sure each thread has its own distinct set of resources on which to operate. When maintaining completely separate resources is not an option though, you may have to synchronize access to the resource using locks, conditions, atomic operations, and other techniques. 

Locks provide a brute force form of protection for code that can be executed by only one thread at a time. The most common type of lock is mutual exclusion lock, also known as a *mutex*. When a thread tries to acquire a mutex that is currently held by another thread, it blocks until the lock is released by the other thread. Several system frameworks provide support for mutex locks, although they are all based on the same underlying technology. In addition, Cocoa provides several variants of the mutex lock to support different types of behavior, such as recursion. For more information about the available types of locks, see [Locks](../ThreadSafety/ThreadSafety.html#//apple_ref/doc/uid/10000057i-CH8-126320). 

In addition to locks, the system provides support for conditions, which ensure the proper sequencing of tasks within your application. A condition acts as a gatekeeper, blocking a given thread until the condition it represents becomes true. When that happens, the condition releases the thread and allows it to continue. The POSIX layer and Foundation framework both provide direct support for conditions. (If you use operation objects, you can configure dependencies among your operation objects to sequence the execution of tasks, which is very similar to the behavior offered by conditions.)

Although locks and conditions are very common in concurrent design, atomic operations are another way to protect and synchronize access to data. Atomic operations offer a lightweight alternative to locks in situations where you can perform mathematical or logical operations on scalar data types. Atomic operations use special hardware instructions to ensure that modifications to a variable are completed before other threads have a chance to access it.

For more information about the available synchronization tools, see [Synchronization Tools](../ThreadSafety/ThreadSafety.html#//apple_ref/doc/uid/10000057i-CH8-124887).  

### Inter-thread Communication
Although a good design minimizes the amount of required communication, at some point, communication between threads becomes necessary. (A thread’s job is to do work for your application, but if the results of that job are never used, what good is it?) Threads may need to process new job requests or report their progress to your application’s main thread. In these situations, you need a way to get information from one thread to another. Fortunately, the fact that threads share the same process space means you have lots of options for communication.

There are many ways to communicate between threads, each with its own advantages and disadvantages. Configuring Thread-Local Storage lists the most common communication mechanisms you can use in OS X. (With the exception of message queues and Cocoa distributed objects, these technologies are also available in iOS.) The techniques in this table are listed in order of increasing complexity.

**Table 1-3**  Communication mechanismsMechanism

Description

Direct messaging

Cocoa applications support the ability to perform selectors directly on other threads. This capability means that one thread can essentially execute a method on any other thread. Because they are executed in the context of the target thread, messages sent this way are automatically serialized on that thread. For information about input sources, see [Cocoa Perform Selector Sources](../RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW44). 

Global variables, shared memory, and objects

Another simple way to communicate information between two threads is to use a global variable, shared object, or shared block of memory. Although shared variables are fast and simple, they are also more fragile than direct messaging. Shared variables must be carefully protected with locks or other synchronization mechanisms to ensure the correctness of your code. Failure to do so could lead to race conditions, corrupted data, or crashes. 

Conditions

Conditions are a synchronization tool that you can use to control when a thread executes a particular portion of code. You can think of conditions as gate keepers, letting a thread run only when the stated condition is met. For information on how to use conditions, see [Using Conditions](../ThreadSafety/ThreadSafety.html#//apple_ref/doc/uid/10000057i-CH8-SW4). 

Run loop sources

A custom run loop source is one that you set up to receive application-specific messages on a thread. Because they are event driven, run loop sources put your thread to sleep automatically when there is nothing to do, which improves your thread’s efficiency. For information about run loops and run loop sources, see [Run Loops](../RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW1). 

Ports and sockets

Port-based communication is a more elaborate way to communication between two threads, but it is also a very reliable technique. More importantly, ports and sockets can be used to communicate with external entities, such as other processes and services. For efficiency, ports are implemented using run loop sources, so your thread sleeps when there is no data waiting on the port. For information about run loops and about port-based input sources, see [Run Loops](../RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW1). 

Message queues

The legacy Multiprocessing Services defines a first-in, first-out (FIFO) queue abstraction for managing incoming and outgoing data. Although message queues are simple and convenient, they are not as efficient as some other communications techniques. For more information about how to use message queues, see *[Multiprocessing Services Programming Guide](../../../../Carbon/Conceptual/Multitasking_MultiproServ/01introduction/introduction.html#//apple_ref/doc/uid/TP40000853)*.

Cocoa distributed objects

Distributed objects is a Cocoa technology that provides a high-level implementation of port-based communications. Although it is possible to use this technology for inter-thread communication, doing so is highly discouraged because of the amount of overhead it incurs. Distributed objects is much more suitable for communicating with other processes, where the overhead of going between processes is already high. For more information, see *[Distributed Objects Programming Topics](../../DistrObjects/DistrObjects.html#//apple_ref/doc/uid/10000102i)*. 

## Design Tips
The following sections offer guidelines to help you implement threads in a way that ensures the correctness of your code. Some of these guidelines also offer tips for achieving better performance with your own threaded code. As with any performance tips, you should always gather relevant performance statistics before, during, and after you make changes to your code. 

### Avoid Creating Threads Explicitly
Writing thread-creation code manually is tedious and potentially error-prone and you should avoid it whenever possible. OS X and iOS provide implicit support for concurrency through other APIs. Rather than create a thread yourself, consider using asynchronous APIs, GCD, or operation objects to do the work. These technologies do the thread-related work behind the scenes for you and are guaranteed to do it correctly. In addition, technologies such as GCD and operation objects are designed to manage threads much more efficiently than your own code ever could by adjusting the number of active threads based on the current system load. For more information about GCD and operation objects, see *[Concurrency Programming Guide](../../../../General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)*.  

### Keep Your Threads Reasonably Busy
If you decide to create and manage threads manually, remember that threads consume precious system resources. You should do your best to make sure that any tasks you assign to threads are reasonably long-lived and productive. At the same time, you should not be afraid to terminate threads that are spending most of their time idle. Threads use a nontrivial amount of memory, some of it wired, so releasing an idle thread not only helps reduce your application’s memory footprint, it also frees up more physical memory for other system processes to use.

> **Important:** Before you start terminating idle threads, you should always record a set of baseline measurements of your applications current performance. After trying your changes, take additional measurements to verify that the changes are actually improving performance, rather than hurting it.

### Avoid Shared Data Structures
The simplest and easiest way to avoid thread-related resource conflicts is to give each thread in your program its own copy of whatever data it needs. Parallel code works best when you minimize the communication and resource contention among your threads. 

Creating a multithreaded application is hard. Even if you are very careful and lock shared data structures at all the right junctures in your code, your code may still be semantically unsafe. For example, your code could run into problems if it expected shared data structures to be modified in a specific order. Changing your code to a transaction-based model to compensate could subsequently negate the performance advantage of having multiple threads. Eliminating the resource contention in the first place often results in a simpler design with excellent performance. 

### Threads and Your User Interface
If your application has a graphical user interface, it is recommended that you receive user-related events and initiate interface updates from your application’s main thread. This approach helps avoid synchronization issues associated with handling user events and drawing window content. Some frameworks, such as Cocoa, generally require this behavior, but even for those that do not, keeping this behavior on the main thread has the advantage of simplifying the logic for managing your user interface.

There are a few notable exceptions where it is advantageous to perform graphical operations from other threads. For example, you can use secondary threads to create and process images and perform other image-related calculations. Using secondary threads for these operations can greatly increase performance. If you are not sure about a particular graphical operation though, plan on doing it from your main thread.

For more information about Cocoa thread safety, see [Thread Safety Summary](../ThreadSafetySummary/ThreadSafetySummary.html#//apple_ref/doc/uid/10000057i-CH12-SW1). For more information about drawing in Cocoa, see *[Cocoa Drawing Guide](../../CocoaDrawingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40003290)*.

### Be Aware of Thread Behaviors at Quit Time
A process runs until all non-detached threads have exited. By default, only the application’s main thread is created as non-detached, but you can create other threads that way as well. When the user quits an application, it is usually considered appropriate behavior to terminate all detached threads immediately, because the work done by detached threads is considered optional. If your application is using background threads to save data to disk or do other critical work, however, you may want to create those threads as non-detached to prevent the loss of data when the application exits.  

Creating threads as non-detached (also known as joinable) requires extra work on your part. Because most high-level thread technologies do not create joinable threads by default, you may have to use the POSIX API to create your thread. In addition, you must add code to your application’s main thread to join with the non-detached threads when they do finally exit. For information on creating joinable threads, see [Setting the Detached State of a Thread](../CreatingThreads/CreatingThreads.html#//apple_ref/doc/uid/10000057i-CH15-SW3). 

If you are writing a Cocoa application, you can also use the `[applicationShouldTerminate:](https://developer.apple.com/documentation/appkit/nsapplicationdelegate/1428642-applicationshouldterminate)` delegate method to delay the termination of the application until a later time or cancel it altogether. When delaying termination, your application would need to wait until any critical threads have finished their tasks and then invoke the `[replyToApplicationShouldTerminate:](https://developer.apple.com/documentation/appkit/nsapplication/1428594-reply)` method. For more information on these methods, see *[NSApplication Class Reference](https://developer.apple.com/documentation/appkit/nsapplication)*. 

### Handle Exceptions
Exception handling mechanisms rely on the current call stack to perform any necessary clean up when an exception is thrown. Because each thread has its own call stack, each thread is therefore responsible for catching its own exceptions. Failing to catch an exception in a secondary thread is the same as failing to catch an exception in your main thread: the owning process is terminated. You cannot throw an uncaught exception to a different thread for processing.

If you need to notify another thread (such as the main thread) of an exceptional situation in the current thread, you should catch the exception and simply send a message to the other thread indicating what happened. Depending on your model and what you are trying to do, the thread that caught the exception can then continue processing (if that is possible), wait for instructions, or simply exit. 

> **Note:** In Cocoa, an `NSException` object is a self-contained object that can be passed from thread to thread once it has been caught. 

In some cases, an exception handler may be created for you automatically. For example, the `@synchronized` directive in Objective-C contains an implicit exception handler.

### Terminate Your Threads Cleanly
The best way for a thread to exit is naturally, by letting it reach the end of its main entry point routine. Although there are functions to terminate threads immediately, those functions should be used only as a last resort. Terminating a thread before it has reached its natural end point prevents the thread from cleaning up after itself. If the thread has allocated memory, opened a file, or acquired other types of resources, your code may be unable to reclaim those resources, resulting in memory leaks or other potential problems. 

For more information on the proper way to exit a thread, see [Terminating a Thread](../CreatingThreads/CreatingThreads.html#//apple_ref/doc/uid/10000057i-CH15-SW10). 

### Thread Safety in Libraries
Although an application developer has control over whether an application executes with multiple threads, library developers do not. When developing libraries, you must assume that the calling application is multithreaded or could switch to being multithreaded at any time. As a result, you should always use locks for critical sections of code.

For library developers, it is unwise to create locks only when an application becomes multithreaded. If you need to lock your code at some point, create the lock object early in the use of your library, preferably in some sort of explicit call to initialize the library. Although you could also use a static library initialization function to create such locks, try to do so only when there is no other way. Execution of an initialization function adds to the time required to load your library and could adversely affect performance.

> **Note:** Always remember to balance calls to lock and unlock a mutex lock within your library. You should also remember to lock library data structures rather than rely on the calling code to provide a thread-safe environment.

If you are developing a Cocoa library, you can register as an observer for the `[NSWillBecomeMultiThreadedNotification](https://developer.apple.com/documentation/foundation/nsnotification/name/1417567-nswillbecomemultithreaded)` if you want to be notified when the application becomes multithreaded. You should not rely on receiving this notification, though, as it might be dispatched before your library code is ever called.

        
            [Next](../CreatingThreads/CreatingThreads.html)[Previous](../Introduction/Introduction.html)
        
         Copyright &#x00a9; 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-07-15