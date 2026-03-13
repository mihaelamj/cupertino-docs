---
title: "Synchronization"
book: "Threading Programming Guide"
chapterId: "10000057i-CH8"
date: "2014-07-15"
description: "Explains how to use threads in Cocoa applications."
identifier: "//apple_ref/doc/uid/10000057i"
source: apple-archive
---

# Synchronization

> **Threading Programming Guide**
> Last updated: 2014-07-15

[Next](../ThreadSafetySummary/ThreadSafetySummary.html)[Previous](../RunLoopManagement/RunLoopManagement.html)
        
        
        [](../index.html)
        
        # Synchronization
The presence of multiple threads in an application opens up potential issues regarding safe access to resources from multiple threads of execution. Two threads modifying the same resource might interfere with each other in unintended ways. For example, one thread might overwrite another’s changes or put the application into an unknown and potentially invalid state. If you are lucky, the corrupted resource might cause obvious performance problems or crashes that are relatively easy to track down and fix. If you are unlucky, however, the corruption may cause subtle errors that do not manifest themselves until much later, or the errors might require a significant overhaul of your underlying coding assumptions.

When it comes to thread safety, a good design is the best protection you have. Avoiding shared resources and minimizing the interactions between your threads makes it less likely for those threads to interfere with each other. A completely interference-free design is not always possible, however. In cases where your threads must interact, you need to use synchronization tools to ensure that when they interact, they do so safely. 

OS X and iOS provide numerous synchronization tools for you to use, ranging from tools that provide mutually exclusive access to those that sequence events correctly in your application. The following sections describe these tools and how you use them in your code to affect safe access to your program’s resources. 

## Synchronization Tools
To prevent different threads from changing data unexpectedly, you can either design your application to not have synchronization issues or you can use synchronization tools. Although avoiding synchronization issues altogether is preferable, it is not always possible. The following sections describe the basic categories of synchronization tools available for you to use. 

### Atomic Operations
Atomic operations are a simple form of synchronization that work on simple data types. The advantage of atomic operations is that they do not block competing threads. For simple operations, such as incrementing a counter variable, this can lead to much better performance than taking a lock. 

OS X and iOS include numerous operations to perform basic mathematical and logical operations on 32-bit and 64-bit values. Among these operations are atomic versions of the compare-and-swap, test-and-set, and test-and-clear operations. For a list of supported atomic operations, see the `/usr/include/libkern/OSAtomic.h` header file or see the `[atomic](../../../../System/Conceptual/ManPages_iPhoneOS/man3/atomic.3.html#//apple_ref/doc/man/3/atomic)` man page. 

### Memory Barriers and Volatile Variables
In order to achieve optimal performance, compilers often reorder assembly-level instructions to keep the instruction pipeline for the processor as full as possible. As part of this optimization, the compiler may reorder instructions that access main memory when it thinks doing so would not generate incorrect data. Unfortunately, it is not always possible for the compiler to detect all memory-dependent operations. If seemingly separate variables actually influence each other, the compiler optimizations could update those variables in the wrong order, generating potentially incorrect results. 

A memory barrier is a type of nonblocking synchronization tool used to ensure that memory operations occur in the correct order. A memory barrier acts like a fence, forcing the processor to complete any load and store operations positioned in front of the barrier before it is allowed to perform load and store operations positioned after the barrier. Memory barriers are typically used to ensure that memory operations by one thread (but visible to another) always occur in an expected order. The lack of a memory barrier in such a situation might allow other threads to see seemingly impossible results. (For an example, see the Wikipedia entry for [memory barriers](http://en.wikipedia.org/wiki/Memory_barrier).) To employ a memory barrier, you simply call the `OSMemoryBarrier` function at the appropriate point in your code.

Volatile variables apply another type of memory constraint to individual variables. The compiler often optimizes code by loading the values for variables into registers. For local variables, this is usually not a problem. If the variable is visible from another thread however, such an optimization might prevent the other thread from noticing any changes to it. Applying the `volatile` keyword to a variable forces the compiler to load that variable from memory each time it is used. You might declare a variable as `volatile` if its value could be changed at any time by an external source that the compiler may not be able to detect.

Because both memory barriers and volatile variables decrease the number of optimizations the compiler can perform, they should be used sparingly and only where needed to ensure correctness. For information about using memory barriers, see the  `[OSMemoryBarrier](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSMemoryBarrier.3.html#//apple_ref/doc/man/3/OSMemoryBarrier)` man page.

### Locks
Locks are one of the most commonly used synchronization tools. You can use locks to protect a *critical section* of your code, which is a segment of code that only one thread at a time is allowed access. For example, a critical section might manipulate a particular data structure or use some resource that supports at most one client at a time. By placing a lock around this section, you exclude other threads from making changes that might affect the correctness of your code. 

Table 4-1 lists some of the locks that are commonly used by programmers. OS X and iOS provide implementations for most of these lock types, but not all of them. For unsupported lock types, the description column explains the reasons why those locks are not implemented directly on the platform.

**Table 4-1**  Lock typesLock

Description

Mutex

A mutually exclusive (or *mutex*) lock acts as a protective barrier around a resource. A mutex is a type of semaphore that grants access to only one thread at a time. If a mutex is in use and another thread tries to acquire it, that thread blocks until the mutex is released by its original holder. If multiple threads compete for the same mutex, only one at a time is allowed access to it.

Recursive lock

A recursive lock is a variant on the mutex lock. A recursive lock allows a single thread to acquire the lock multiple times before releasing it. Other threads remain blocked until the owner of the lock releases the lock the same number of times it acquired it. Recursive locks are used during recursive iterations primarily but may also be used in cases where multiple methods each need to acquire the lock separately.  

Read-write lock

A read-write lock is also referred to as a shared-exclusive lock. This type of lock is typically used in larger-scale operations and can significantly improve performance if the protected data structure is read frequently and modified only occasionally. During normal operation, multiple readers can access the data structure simultaneously. When a thread wants to write to the structure, though, it blocks until all readers release the lock, at which point it acquires the lock and can update the structure. While a writing thread is waiting for the lock, new reader threads block until the writing thread is finished. The system supports read-write locks using POSIX threads only. For more information on how to use these locks, see the `[pthread](../../../../System/Conceptual/ManPages_iPhoneOS/man3/pthread.3.html#//apple_ref/doc/man/3/pthread)` man page. 

Distributed lock

A distributed lock provides mutually exclusive access at the process level. Unlike a true mutex, a distributed lock does not block a process or prevent it from running. It simply reports when the lock is busy and lets the process decide how to proceed. 

Spin lock

A spin lock polls its lock condition repeatedly until that condition becomes true. Spin locks are most often used on multiprocessor systems where the expected wait time for a lock is small. In these situations, it is often more efficient to poll than to block the thread, which involves a context switch and the updating of thread data structures. The system does not provide any implementations of spin locks because of their polling nature, but you can easily implement them in specific situations. For information on implementing spin locks in the kernel, see *[Kernel Programming Guide](../../../../Darwin/Conceptual/KernelProgramming/About/About.html#//apple_ref/doc/uid/TP30000905)*.

Double-checked lock

A double-checked lock is an attempt to reduce the overhead of taking a lock by testing the locking criteria prior to taking the lock. Because double-checked locks are potentially unsafe, the system does not provide explicit support for them and their use is discouraged. 

> **Note:** Most types of locks also incorporate a memory barrier to ensure that any preceding load and store instructions are completed before entering the critical section.

For information on how to use locks, see [Using Locks](#//apple_ref/doc/uid/10000057i-CH8-SW16).  

### Conditions
A condition is another type of semaphore that allows threads to signal each other when a certain condition is true. Conditions are typically used to indicate the availability of a resource or to ensure that tasks are performed in a specific order. When a thread tests a condition, it blocks unless that condition is already true. It remains blocked until some other thread explicitly changes and signals the condition. The difference between a condition and a mutex lock is that multiple threads may be permitted access to the condition at the same time. The condition is more of a gatekeeper that lets different threads through the gate depending on some specified criteria. 

One way you might use a condition is to manage a pool of pending events. The event queue would use a condition variable to signal waiting threads when there were events in the queue. If one event arrives, the queue would signal the condition appropriately. If a thread were already waiting, it would be woken up whereupon it would pull the event from the queue and process it. If two events came in to the queue at roughly the same time, the queue would signal the condition twice to wake up two threads.

The system provides support for conditions in several different technologies. The correct implementation of conditions requires careful coding, however, so you should look at the examples in [Using Conditions](#//apple_ref/doc/uid/10000057i-CH8-SW4) before using them in your own code. 

### Perform Selector Routines
Cocoa applications have a convenient way of delivering messages in a synchronized manner to a single thread. The `[NSObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/cl/NSObject)` class declares methods for performing a selector on one of the application’s active threads. These methods let your threads deliver messages asynchronously with the guarantee that they will be performed synchronously by the target thread. For example, you might use perform selector messages to deliver results from a distributed computation to your application’s main thread or to a designated coordinator thread. Each request to perform a selector is queued on the target thread’s run loop and the requests are then processed sequentially in the order in which they were received.

For a summary of perform selector routines and more information about how to use them, see [Cocoa Perform Selector Sources](../RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW44). 

## Synchronization Costs and Performance
Synchronization helps ensure the correctness of your code, but does so at the expense of performance. The use of synchronization tools introduces delays, even in uncontested cases. Locks and atomic operations generally involve the use of memory barriers and kernel-level synchronization to ensure code is properly protected. And if there is contention for a lock, your threads could block and experience even greater delays. 

Table 4-2 lists some of the approximate costs associated with mutexes and atomic operations in the uncontested case. These measurements represented average times taken over several thousand samples. As with thread creation times though, mutex acquisition times (even in the uncontested case) can vary greatly depending on processor load, the speed of the computer, and the amount of available system and program memory.

**Table 4-2**  Mutex and atomic operation costsItem

Approximate cost

Notes

Mutex acquisition time

Approximately 0.2 microseconds

This is the lock acquisition time in an uncontested case. If the lock is held by another thread, the acquisition time can be much greater. The figures were determined by analyzing the mean and median values generated during mutex acquisition on an Intel-based iMac with a 2 GHz Core Duo processor and 1 GB of RAM running OS X v10.5.

Atomic compare-and-swap

Approximately 0.05 microseconds

This is the compare-and-swap time in an uncontested case. The figures were determined by analyzing the mean and median values for the operation and were generated on an Intel-based iMac with a 2 GHz Core Duo processor and 1 GB of RAM running OS X v10.5.

When designing your concurrent tasks, correctness is always the most important factor, but you should also consider performance factors as well. Code that executes correctly under multiple threads, but slower than the same code running on a single thread, is hardly an improvement. 

If you are retrofitting an existing single-threaded application, you should always take a set of baseline measurements of the performance of key tasks. Upon adding additional threads, you should then take new measurements for those same tasks and compare the performance of the multithreaded case to the single-threaded case. If after tuning your code, threading does not improve performance, you may want to reconsider your specific implementation or the use of threads altogether. 

For information about performance and the tools for gathering metrics, see *[Performance Overview](../../../../Performance/Conceptual/PerformanceOverview/Introduction/Introduction.html#//apple_ref/doc/uid/TP40001410)*. For specific information about the cost of locks and atomic operations, see [Thread Costs](../CreatingThreads/CreatingThreads.html#//apple_ref/doc/uid/10000057i-CH15-SW7).

## Thread Safety and Signals
When it comes to threaded applications, nothing causes more fear or confusion than the issue of handling signals. Signals are a low-level BSD mechanism that can be used to deliver information to a process or manipulate it in some way. Some programs use signals to detect certain events, such as the death of a child process. The system uses signals to terminate runaway processes and communicate other types of information.

The problem with signals is not what they do, but their behavior when your application has multiple threads. In a single-threaded application, all signal handlers run on the main thread. In a multithreaded application, signals that are not tied to a specific hardware error (such as an illegal instruction) are delivered to whichever thread happens to be running at the time. If  multiple threads are running simultaneously, the signal is delivered to whichever one the system happens to pick. In other words, signals can be delivered to any thread of your application. 

The first rule for implementing signal handlers in applications is to avoid assumptions about which thread is handling the signal. If a specific thread wants to handle a given signal, you need to work out some way of notifying that thread when the signal arrives. You cannot just assume that installation of a signal handler from that thread will result in the signal being delivered to the same thread.

For more information about signals and installing signal handlers, see `[signal](../../../../System/Conceptual/ManPages_iPhoneOS/man3/signal.3.html#//apple_ref/doc/man/3/signal)` and `[sigaction](../../../../System/Conceptual/ManPages_iPhoneOS/man2/sigaction.2.html#//apple_ref/doc/man/2/sigaction)` man pages.

## Tips for Thread-Safe Designs
Synchronization tools are a useful way to make your code thread-safe, but they are not a panacea. Used too much, locks and other types of synchronization primitives can actually decrease your application’s threaded performance compared to its non-threaded performance. Finding the right balance between safety and performance is an art that takes experience. The following sections provide tips to help you choose the appropriate level of synchronization for your application.

### Avoid Synchronization Altogether
For any new projects you work on, and even for existing projects, designing your code and data structures to avoid the need for synchronization is the best possible solution. Although locks and other synchronization tools are useful, they do impact the performance of any application. And if the overall design causes high contention among specific resources, your threads could be waiting even longer. 

The best way to implement concurrency is to reduce the interactions and inter-dependencies between your concurrent tasks. If each task operates on its own private data set, it does not need to protect that data using locks. Even in situations where two tasks do share a common data set, you can look at ways of partitioning that set or providing each task with its own copy. Of course, copying data sets has its costs too, so you have to weigh those costs against the costs of synchronization before making your decision.

### Understand the Limits of Synchronization
Synchronization tools are effective only when they are used consistently by all threads in an application. If you create a mutex to restrict access to a specific resource, all of your threads must acquire the same mutex before trying to manipulate the resource. Failure to do so defeats the protection offered by the mutex and is a programmer error. 

### Be Aware of Threats to Code Correctness
When using locks and memory barriers, you should always give careful thought to their placement in your code. Even locks that seem well placed can actually lull you into a false sense of security. The following series of examples attempt to illustrate this problem by pointing out the flaws in seemingly innocuous code. The basic premise is that you have a mutable array containing a set of immutable objects. Suppose you want to invoke a method of the first object in the array. You might do so using the following code:

NSLock* arrayLock = GetArrayLock();
```
NSMutableArray* myArray = GetSharedArray();
```

```
id anObject;
```

```
 
```

```
[arrayLock lock];
```

```
anObject = [myArray objectAtIndex:0];
```

```
[arrayLock unlock];
```

```
 
```

```
[anObject doSomething];
```
Because the array is mutable, the lock around the array prevents other threads from modifying the array until you get the desired object. And because the object you retrieve is itself immutable, a lock is not needed around the call to the `doSomething` method.

There is a problem with the preceding example, though. What happens if you release the lock and another thread comes in and removes all objects from the array before you have a chance to execute the `doSomething` method? In an application without garbage collection, the object your code is holding could be released, leaving `anObject` pointing to an invalid memory address. To fix the problem, you might decide to simply rearrange your existing code and release the lock after your call to `doSomething`, as shown here: 

NSLock* arrayLock = GetArrayLock();
```
NSMutableArray* myArray = GetSharedArray();
```

```
id anObject;
```

```
 
```

```
[arrayLock lock];
```

```
anObject = [myArray objectAtIndex:0];
```

```
[anObject doSomething];
```

```
[arrayLock unlock];
```
By moving the `doSomething` call inside the lock, your code guarantees that the object is still valid when the method is called. Unfortunately, if the `doSomething` method takes a long time to execute, this could cause your code to hold the lock for a long time, which could create a performance bottleneck. 

The problem with the code is not that the critical region was poorly defined, but that the actual problem was not understood. The real problem is a memory management issue that is triggered only by the presence of other threads. Because it can be released by another thread, a better solution would be to retain `anObject` before releasing the lock. This solution addresses the real problem of the object being released and does so without introducing a potential performance penalty. 

NSLock* arrayLock = GetArrayLock();
```
NSMutableArray* myArray = GetSharedArray();
```

```
id anObject;
```

```
 
```

```
[arrayLock lock];
```

```
anObject = [myArray objectAtIndex:0];
```

```
[anObject retain];
```

```
[arrayLock unlock];
```

```
 
```

```
[anObject doSomething];
```

```
[anObject release];
```
Although the preceding examples are very simple in nature, they do illustrate a very important point. When it comes to correctness, you have to think beyond the obvious problems. Memory management and other aspects of your design may also be affected by the presence of multiple threads, so you have to think about those problems up front. In addition, you should always assume that the compiler will do the worst possible thing when it comes to safety. This kind of awareness and vigilance should help you avoid potential problems and ensure that your code behaves correctly. 

For additional examples of how to make your program thread-safe, see [Thread Safety Summary](../ThreadSafetySummary/ThreadSafetySummary.html#//apple_ref/doc/uid/10000057i-CH12-SW1).

### Watch Out for Deadlocks and Livelocks
Any time a thread tries to take more than one lock at the same time, there is a potential for a deadlock to occur. A deadlock occurs when two different threads hold a lock that the other one needs and then try to acquire the lock held by the other thread. The result is that each thread blocks permanently because it can never acquire the other lock. 

A livelock is similar to a deadlock and occurs when two threads compete for the same set of resources. In a livelock situation, a thread gives up its first lock in an attempt to acquire its second lock. Once it acquires the second lock, it goes back and tries to acquire the first lock again. It locks up because it spends all its time releasing one lock and trying to acquire the other lock rather than doing any real work. 

The best way to avoid both deadlock and livelock situations is to take only one lock at a time. If you must acquire more than one lock at a time, you should make sure that other threads do not try to do something similar. 

### Use Volatile Variables Correctly
If you are already using a mutex to protect a section of code, do not automatically assume you need to use the `volatile` keyword to protect important variables inside that section. A mutex includes a memory barrier to ensure the proper ordering of load and store operations. Adding the `volatile` keyword to a variable within a critical section forces the value to be loaded from memory each time it is accessed. The combination of the two synchronization techniques may be necessary in specific cases but also leads to a significant performance penalty. If the mutex alone is enough to protect the variable, omit the `volatile` keyword. 

It is also important that you do not use volatile variables in an attempt to avoid the use of mutexes. In general, mutexes and other synchronization mechanisms are a better way to protect the integrity of your data structures than volatile variables. The `volatile` keyword only ensures that a variable is loaded from memory rather than stored in a register. It does not ensure that the variable is accessed correctly by your code. 

## Using Atomic Operations
Nonblocking synchronization is a way to perform some types of operations and avoid the expense of locks. Although locks are an effective way to synchronize two threads, acquiring a lock is a relatively expensive operation, even in the uncontested case. By contrast, many atomic operations take a fraction of the time to complete and can be just as effective as a lock. 

Atomic operations let you perform simple mathematical and logical operations on 32-bit or 64-bit values. These operations rely on special hardware instructions (and an optional memory barrier) to ensure that the given operation completes before the affected memory is accessed again. In the multithreaded case, you should always use the atomic operations that incorporate a memory barrier to ensure that the memory is synchronized correctly between threads.

Table 4-3 lists the available atomic mathematical and logical operations and the corresponding function names. These functions are all declared in the `/usr/include/libkern/OSAtomic.h` header file, where you can also find the complete syntax. The 64-bit versions of these functions are available only in 64-bit processes. 

**Table 4-3**  Atomic math and logic operationsOperation

Function name

Description

Add

`[OSAtomicAdd32](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicAdd32.3.html#//apple_ref/doc/man/3/OSAtomicAdd32)`

`[OSAtomicAdd32Barrier](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicAdd32Barrier.3.html#//apple_ref/doc/man/3/OSAtomicAdd32Barrier)`

`[OSAtomicAdd64](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicAdd64.3.html#//apple_ref/doc/man/3/OSAtomicAdd64)`

`[OSAtomicAdd64Barrier](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicAdd64Barrier.3.html#//apple_ref/doc/man/3/OSAtomicAdd64Barrier)`

Adds two integer values together and stores the result in one of the specified variables. 

Increment

`[OSAtomicIncrement32](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicIncrement32.3.html#//apple_ref/doc/man/3/OSAtomicIncrement32)`

`[OSAtomicIncrement32Barrier](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicIncrement32Barrier.3.html#//apple_ref/doc/man/3/OSAtomicIncrement32Barrier)`

`[OSAtomicIncrement64](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicIncrement64.3.html#//apple_ref/doc/man/3/OSAtomicIncrement64)`

`[OSAtomicIncrement64Barrier](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicIncrement64Barrier.3.html#//apple_ref/doc/man/3/OSAtomicIncrement64Barrier)`

Increments the specified integer value by 1. 

Decrement

`[OSAtomicDecrement32](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicDecrement32.3.html#//apple_ref/doc/man/3/OSAtomicDecrement32)`

`[OSAtomicDecrement32Barrier](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicDecrement32Barrier.3.html#//apple_ref/doc/man/3/OSAtomicDecrement32Barrier)`

`[OSAtomicDecrement64](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicDecrement64.3.html#//apple_ref/doc/man/3/OSAtomicDecrement64)`

`[OSAtomicDecrement64Barrier](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicDecrement64Barrier.3.html#//apple_ref/doc/man/3/OSAtomicDecrement64Barrier)`

Decrements the specified integer value by 1.

Logical OR

`[OSAtomicOr32](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicOr32.3.html#//apple_ref/doc/man/3/OSAtomicOr32)`

`[OSAtomicOr32Barrier](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicOr32Barrier.3.html#//apple_ref/doc/man/3/OSAtomicOr32Barrier)`

Performs a logical OR between the specified 32-bit value and a 32-bit mask.

Logical AND

`[OSAtomicAnd32](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicAnd32.3.html#//apple_ref/doc/man/3/OSAtomicAnd32)`

`[OSAtomicAnd32Barrier](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicAnd32Barrier.3.html#//apple_ref/doc/man/3/OSAtomicAnd32Barrier)`

Performs a logical AND between the specified 32-bit value and a 32-bit mask.

Logical XOR

`[OSAtomicXor32](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicXor32.3.html#//apple_ref/doc/man/3/OSAtomicXor32)`

`[OSAtomicXor32Barrier](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicXor32Barrier.3.html#//apple_ref/doc/man/3/OSAtomicXor32Barrier)`

Performs a logical XOR between the specified 32-bit value and a 32-bit mask.

Compare and swap

`[OSAtomicCompareAndSwap32](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicCompareAndSwap32.3.html#//apple_ref/doc/man/3/OSAtomicCompareAndSwap32)`

`[OSAtomicCompareAndSwap32Barrier](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicCompareAndSwap32Barrier.3.html#//apple_ref/doc/man/3/OSAtomicCompareAndSwap32Barrier)`

`[OSAtomicCompareAndSwap64](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicCompareAndSwap64.3.html#//apple_ref/doc/man/3/OSAtomicCompareAndSwap64)`

`[OSAtomicCompareAndSwap64Barrier](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicCompareAndSwap64Barrier.3.html#//apple_ref/doc/man/3/OSAtomicCompareAndSwap64Barrier)`

`[OSAtomicCompareAndSwapPtr](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicCompareAndSwapPtr.3.html#//apple_ref/doc/man/3/OSAtomicCompareAndSwapPtr)`

`[OSAtomicCompareAndSwapPtrBarrier](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicCompareAndSwapPtrBarrier.3.html#//apple_ref/doc/man/3/OSAtomicCompareAndSwapPtrBarrier)`

`[OSAtomicCompareAndSwapInt](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicCompareAndSwapInt.3.html#//apple_ref/doc/man/3/OSAtomicCompareAndSwapInt)`

`[OSAtomicCompareAndSwapIntBarrier](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicCompareAndSwapIntBarrier.3.html#//apple_ref/doc/man/3/OSAtomicCompareAndSwapIntBarrier)`

`[OSAtomicCompareAndSwapLong](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicCompareAndSwapLong.3.html#//apple_ref/doc/man/3/OSAtomicCompareAndSwapLong)`

`[OSAtomicCompareAndSwapLongBarrier](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicCompareAndSwapLongBarrier.3.html#//apple_ref/doc/man/3/OSAtomicCompareAndSwapLongBarrier)`

Compares a variable against the specified old value. If the two values are equal, this function assigns the specified new value to the variable; otherwise, it does nothing. The comparison and assignment are done as one atomic operation and the function returns a Boolean value indicating whether the swap actually occurred. 

Test and set

`[OSAtomicTestAndSet](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicTestAndSet.3.html#//apple_ref/doc/man/3/OSAtomicTestAndSet)`

`[OSAtomicTestAndSetBarrier](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicTestAndSetBarrier.3.html#//apple_ref/doc/man/3/OSAtomicTestAndSetBarrier)`

Tests a bit in the specified variable, sets that bit to 1, and returns the value of the old bit as a Boolean value. Bits are tested according to the formula `(0x80 >> (n & 7)) `of byte `((char*)address + (n >> 3))` where `n` is the bit number and `address` is a pointer to the variable. This formula effectively breaks up the variable into 8-bit sized chunks and orders the bits in each chunk in reverse. For example, to test the lowest-order bit (bit 0) of a 32-bit integer, you would actually specify 7 for the bit number; similarly, to test the highest order bit (bit 32), you would specify 24 for the bit number. 

Test and clear

`[OSAtomicTestAndClear](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicTestAndClear.3.html#//apple_ref/doc/man/3/OSAtomicTestAndClear)`

`[OSAtomicTestAndClearBarrier](../../../../System/Conceptual/ManPages_iPhoneOS/man3/OSAtomicTestAndClearBarrier.3.html#//apple_ref/doc/man/3/OSAtomicTestAndClearBarrier)`

Tests a bit in the specified variable, sets that bit to 0, and returns the value of the old bit as a Boolean value. Bits are tested according to the formula `(0x80 >> (n & 7)) `of byte `((char*)address + (n >> 3))` where `n` is the bit number and `address` is a pointer to the variable. This formula effectively breaks up the variable into 8-bit sized chunks and orders the bits in each chunk in reverse. For example, to test the lowest-order bit (bit 0) of a 32-bit integer, you would actually specify 7 for the bit number; similarly, to test the highest order bit (bit 32), you would specify 24 for the bit number. 

The behavior of most atomic functions should be relatively straightforward and what you would expect. Listing 4-1, however, shows the behavior of atomic test-and-set and compare-and-swap operations, which are a little more complex. The first three calls to the `OSAtomicTestAndSet` function demonstrate how the bit manipulation formula being used on an integer value and its results might differ from what you would expect. The last two calls show the behavior of the `OSAtomicCompareAndSwap32` function. In all cases, these functions are being called in the uncontested case when no other threads are manipulating the values. 

**Listing 4-1**  Performing atomic operations

int32_t  theValue = 0;
```
OSAtomicTestAndSet(0, &theValue);
```

```
// theValue is now 128.
```

```
 
```

```
theValue = 0;
```

```
OSAtomicTestAndSet(7, &theValue);
```

```
// theValue is now 1.
```

```
 
```

```
theValue = 0;
```

```
OSAtomicTestAndSet(15, &theValue)
```

```
// theValue is now 256.
```

```
 
```

```
OSAtomicCompareAndSwap32(256, 512, &theValue);
```

```
// theValue is now 512.
```

```
 
```

```
OSAtomicCompareAndSwap32(256, 1024, &theValue);
```

```
// theValue is still 512.
```
For information about atomic operations, see the `[atomic](../../../../System/Conceptual/ManPages_iPhoneOS/man3/atomic.3.html#//apple_ref/doc/man/3/atomic)` man page and the `/usr/include/libkern/OSAtomic.h` header file.

## Using Locks
Locks are a fundamental synchronization tool for threaded programming. Locks enable you to protect large sections of code easily so that you can ensure the correctness of that code. OS X and iOS provide basic mutex locks for all application types and the Foundation framework defines some additional variants of the mutex lock for special situations. The following sections show you how to use several of these lock types. 

### Using a POSIX Mutex Lock
POSIX mutex locks are extremely easy to use from any application. To create the mutex lock, you declare and initialize a `pthread_mutex_t` structure. To lock and unlock the mutex lock, you use the `pthread_mutex_lock` and `pthread_mutex_unlock` functions. Listing 4-2 shows the basic code required to initialize and use a POSIX thread mutex lock. When you are done with the lock, simply call `pthread_mutex_destroy` to free up the lock data structures. 

**Listing 4-2**  Using a mutex lock

pthread_mutex_t mutex;
```
void MyInitFunction()
```

```
{
```

```
    pthread_mutex_init(&mutex, NULL);
```

```
}
```

```
 
```

```
void MyLockingFunction()
```

```
{
```

```
    pthread_mutex_lock(&mutex);
```

```
    // Do work.
```

```
    pthread_mutex_unlock(&mutex);
```

```
}
```

> **Note:** The preceding code is a simplified example intended to show the basic usage of the POSIX thread mutex functions. Your own code should check the error codes returned by these functions and handle them appropriately. 

### Using the NSLock Class
An `[NSLock](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSLock/Description.html#//apple_ref/occ/cl/NSLock)` object implements a basic mutex for Cocoa applications. The interface for all locks (including `NSLock`) is actually defined by the `[NSLocking](https://developer.apple.com/documentation/foundation/nslocking)` protocol, which defines the `lock` and `unlock` methods. You use these methods to acquire and release the lock just as you would any mutex.

In addition to the standard locking behavior, the `NSLock` class adds the `[tryLock](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSLock/Description.html#//apple_ref/occ/instm/NSLock/tryLock)` and `[lockBeforeDate:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSLock/Description.html#//apple_ref/occ/instm/NSLock/lockBeforeDate:)` methods. The `[tryLock](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSLock/Description.html#//apple_ref/occ/instm/NSLock/tryLock)` method attempts to acquire the lock but does not block if the lock is unavailable; instead, the method simply returns `NO`. The `[lockBeforeDate:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSLock/Description.html#//apple_ref/occ/instm/NSLock/lockBeforeDate:)` method attempts to acquire the lock but unblocks the thread (and returns `NO`) if the lock is not acquired within the specified time limit.

The following example shows how you could use an `NSLock` object to coordinate the updating of a visual display, whose data is being calculated by several threads. If the thread cannot acquire the lock immediately, it simply continues its calculations until it can acquire the lock and update the display. 

```
BOOL moreToDo = YES;
```

```
NSLock *theLock = [[NSLock alloc] init];
```

```
...
```

```
while (moreToDo) {
```

```
    /* Do another increment of calculation */
```

```
    /* until there’s no more to do. */
```

```
    if ([theLock tryLock]) {
```

```
        /* Update display used by all threads. */
```

```
        [theLock unlock];
```

```
    }
```

```
}
```
### Using the @synchronized Directive
The `@synchronized` directive is a convenient way to create mutex locks on the fly in Objective-C code. The `@synchronized` directive does what any other mutex lock would do—it prevents different threads from acquiring the same lock at the same time. In this case, however, you do not have to create the mutex or lock object directly. Instead, you simply use any Objective-C object as a lock token, as shown in the following example:

- (void)myMethod:(id)anObj
```
{
```

```
    @synchronized(anObj)
```

```
    {
```

```
        // Everything between the braces is protected by the @synchronized directive.
```

```
    }
```

```
}
```
The object passed to the `@synchronized` directive is a unique identifier used to distinguish the protected block. If you execute the preceding method in two different threads, passing a different object for the `anObj` parameter on each thread, each would take its lock and continue processing without being blocked by the other. If you pass the same object in both cases, however, one of the threads would acquire the lock first and the other would block until the first thread completed the critical section. 

As a precautionary measure, the `@synchronized` block implicitly adds an exception handler to the protected code. This handler automatically releases the mutex in the event that an exception is thrown. This means that in order to use the `@synchronized` directive, you must also enable Objective-C exception handling in your code. If you do not want the additional overhead caused by the implicit exception handler, you should consider using the lock classes.

For more information about the `@synchronized` directive, see *[The Objective-C Programming Language](../../ObjectiveC/Introduction/introObjectiveC.html#//apple_ref/doc/uid/TP30001163)*. 

### Using Other Cocoa Locks
The following sections describe the process for using several other types of Cocoa locks. 

#### Using an NSRecursiveLock Object
The `[NSRecursiveLock](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRecursiveLock/Description.html#//apple_ref/occ/cl/NSRecursiveLock)` class defines a lock that can be acquired multiple times by the same thread without causing the thread to deadlock. A recursive lock keeps track of how many times it was successfully acquired. Each successful acquisition of the lock must be balanced by a corresponding call to unlock the lock. Only when all of the lock and unlock calls are balanced is the lock actually released so that other threads can acquire it. 

As its name implies, this type of lock is commonly used inside a recursive function to prevent the recursion from blocking the thread. You could similarly use it in the non-recursive case to call functions whose semantics demand that they also take the lock. Here’s an example of a simple recursive function that acquires the lock through recursion. If you did not use an `NSRecursiveLock` object for this code, the thread would deadlock when the function was called again. 

NSRecursiveLock *theLock = [[NSRecursiveLock alloc] init];
```
 
```

```
void MyRecursiveFunction(int value)
```

```
{
```

```
    [theLock lock];
```

```
    if (value != 0)
```

```
    {
```

```
        --value;
```

```
        MyRecursiveFunction(value);
```

```
    }
```

```
    [theLock unlock];
```

```
}
```

```
 
```

```
MyRecursiveFunction(5);
```

> **Note:** Because a recursive lock is not released until all lock calls are balanced with unlock calls, you should carefully weigh the decision to use a performance lock against the potential performance implications. Holding any lock for an extended period of time can cause other threads to block until the recursion completes. If you can rewrite your code to eliminate the recursion or eliminate the need to use a recursive lock, you may achieve better performance.

#### Using an NSConditionLock Object
An `[NSConditionLock](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSConditionLock/Description.html#//apple_ref/occ/cl/NSConditionLock)` object defines a mutex lock that can be locked and unlocked with specific values. You should not confuse this type of lock with a condition (see [Conditions](#//apple_ref/doc/uid/10000057i-CH8-126424)). The behavior is somewhat similar to conditions, but is implemented very differently. 

Typically, you use an `NSConditionLock` object when threads need to perform tasks in a specific order, such as when one thread produces data that another consumes. While the producer is executing, the consumer acquires the lock using a condition that is specific to your program. (The condition itself is just an integer value that you define.) When the producer finishes, it unlocks the lock and sets the lock condition to the appropriate integer value to wake the consumer thread, which then proceeds to process the data. 

The locking and unlocking methods that `NSConditionLock` objects respond to can be used in any combination. For example, you can pair a `lock` message with `[unlockWithCondition:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSConditionLock/Description.html#//apple_ref/occ/instm/NSConditionLock/unlockWithCondition:)`, or a `[lockWhenCondition:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSConditionLock/Description.html#//apple_ref/occ/instm/NSConditionLock/lockWhenCondition:)` message with `unlock`. Of course, this latter combination unlocks the lock but might not release any threads waiting on a specific condition value.

The following example shows how the producer-consumer problem might be handled using condition locks. Imagine that an application contains a queue of data. A producer thread adds data to the queue, and consumer threads extract data from the queue. The producer need not wait for a specific condition, but it must wait for the lock to be available so it can safely add data to the queue. 

id condLock = [[NSConditionLock alloc] initWithCondition:NO_DATA];
```
 
```

```
while(true)
```

```
{
```

```
    [condLock lock];
```

```
    /* Add data to the queue. */
```

```
    [condLock unlockWithCondition:HAS_DATA];
```

```
}
```
Because the initial condition of the lock is set to `NO_DATA`, the producer thread should have no trouble acquiring the lock initially. It fills the queue with data and sets the condition to `HAS_DATA`. During subsequent iterations, the producer thread can add new data as it arrives, regardless of whether the queue is empty or still has some data. The only time it blocks is when a consumer thread is extracting data from the queue. 

Because the consumer thread must have data to process, it waits on the queue using a specific condition. When the producer puts data on the queue, the consumer thread wakes up and acquires its lock. It can then extract some data from the queue and update the queue status. The following example shows the basic structure of the consumer thread’s processing loop. 

```
while (true)
```

```
{
```

```
    [condLock lockWhenCondition:HAS_DATA];
```

```
    /* Remove data from the queue. */
```

```
    [condLock unlockWithCondition:(isEmpty ? NO_DATA : HAS_DATA)];
```

```
 
```

```
    // Process the data locally.
```

```
}
```
#### Using an NSDistributedLock Object
The `[NSDistributedLock](https://developer.apple.com/documentation/foundation/nsdistributedlock)` class can be used by multiple applications on multiple hosts to restrict access to some shared resource, such as a file. The lock itself is effectively a mutex lock that is implemented using a file-system item, such as a file or directory. For an `NSDistributedLock` object to be usable, the lock must be writable by all applications that use it. This usually means putting it on a file system that is accessible to all of the computers that are running the application. 

Unlike other types of lock, `NSDistributedLock` does not conform to the `[NSLocking](https://developer.apple.com/documentation/foundation/nslocking)` protocol and thus does not have a `lock` method. A `lock` method would block the execution of the thread and require the system to poll the lock at a predetermined rate. Rather than impose this penalty on your code, `NSDistributedLock` provides a `[tryLock](https://developer.apple.com/documentation/foundation/nsdistributedlock/1412293-trylock)` method and lets you decide whether or not to poll.

Because it is implemented using the file system, an `NSDistributedLock` object is not released unless the owner explicitly releases it. If your application crashes while holding a distributed lock, other clients will be unable to access the protected resource. In this situation, you can use the `[breakLock](https://developer.apple.com/documentation/foundation/nsdistributedlock/1413425-breaklock)` method to break the existing lock so that you can acquire it. Breaking locks should generally be avoided, though, unless you are certain the owning process died and cannot release the lock. 

As with other types of locks, when you are done using an `NSDistributedLock` object, you release it by calling the `unlock` method. 

## Using Conditions
Conditions are a special type of lock that you can use to synchronize the order in which operations must proceed. They differ from mutex locks in a subtle way.  A thread waiting on a condition remains blocked until that condition is signaled explicitly by another thread. 

Due to the subtleties involved in implementing operating systems, condition locks are permitted to return with spurious success even if they were not actually signaled by your code. To avoid problems caused by these spurious signals, you should always use a predicate in conjunction with your condition lock. The predicate is a more concrete way of determining whether it is safe for your thread to proceed. The condition simply keeps your thread asleep until the predicate can be set by the signaling thread. 

The following sections show you how to use conditions in your code.

### Using the NSCondition Class
The `[NSCondition](https://developer.apple.com/documentation/foundation/nscondition)` class provides the same semantics as POSIX conditions, but wraps both the required lock and condition data structures in a single object. The result is an object that you can lock like a mutex and then wait on like a condition.

Listing 4-3 shows a code snippet demonstrating the sequence of events for waiting on an `NSCondition` object. The `cocoaCondition` variable contains an `NSCondition` object and the `timeToDoWork` variable is an integer that is incremented from another thread immediately prior to signaling the condition. 

**Listing 4-3**  Using a Cocoa condition

[cocoaCondition lock];
```
while (timeToDoWork <= 0)
```

```
    [cocoaCondition wait];
```

```
 
```

```
timeToDoWork--;
```

```
 
```

```
// Do real work here.
```

```
 
```

```
[cocoaCondition unlock];
```
Listing 4-4 shows the code used to signal the Cocoa condition and increment the predicate variable. You should always lock the condition before signaling it. 

**Listing 4-4**  Signaling a Cocoa condition

```
[cocoaCondition lock];
```

```
timeToDoWork++;
```

```
[cocoaCondition signal];
```

```
[cocoaCondition unlock];
```
### Using POSIX Conditions
POSIX thread condition locks require the use of both a condition data structure and a mutex. Although the two lock structures are separate, the mutex lock is intimately tied to the condition structure at runtime. Threads waiting on a signal should always use the same mutex lock and condition structures together. Changing the pairing can cause errors. 

Listing 4-5 shows the basic initialization and usage of a condition and predicate. After initializing both the condition and the mutex lock, the waiting thread enters a while loop using the `ready_to_go` variable as its predicate. Only when the predicate is set and the condition subsequently signaled does the waiting thread wake up and start doing its work.  

**Listing 4-5**  Using a POSIX condition

pthread_mutex_t mutex;
```
pthread_cond_t condition;
```

```
Boolean     ready_to_go = true;
```

```
 
```

```
void MyCondInitFunction()
```

```
{
```

```
    pthread_mutex_init(&mutex);
```

```
    pthread_cond_init(&condition, NULL);
```

```
}
```

```
 
```

```
void MyWaitOnConditionFunction()
```

```
{
```

```
    // Lock the mutex.
```

```
    pthread_mutex_lock(&mutex);
```

```
 
```

```
    // If the predicate is already set, then the while loop is bypassed;
```

```
    // otherwise, the thread sleeps until the predicate is set.
```

```
    while(ready_to_go == false)
```

```
    {
```

```
        pthread_cond_wait(&condition, &mutex);
```

```
    }
```

```
 
```

```
    // Do work. (The mutex should stay locked.)
```

```
 
```

```
    // Reset the predicate and release the mutex.
```

```
    ready_to_go = false;
```

```
    pthread_mutex_unlock(&mutex);
```

```
}
```
The signaling thread is responsible both for setting the predicate and for sending the signal to the condition lock.  Listing 4-6 shows the code for implementing this behavior. In this example, the condition is signaled inside of the mutex to prevent race conditions from occurring between the threads waiting on the condition. 

**Listing 4-6**  Signaling a condition lock

void SignalThreadUsingCondition()
```
{
```

```
    // At this point, there should be work for the other thread to do.
```

```
    pthread_mutex_lock(&mutex);
```

```
    ready_to_go = true;
```

```
 
```

```
    // Signal the other thread to begin work.
```

```
    pthread_cond_signal(&condition);
```

```
 
```

```
    pthread_mutex_unlock(&mutex);
```

```
}
```

> **Note:** The preceding code is a simplified example intended to show the basic usage of the POSIX thread condition functions. Your own code should check the error codes returned by these functions and handle them appropriately. 

        
            [Next](../ThreadSafetySummary/ThreadSafetySummary.html)[Previous](../RunLoopManagement/RunLoopManagement.html)
        
         Copyright &#x00a9; 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-07-15