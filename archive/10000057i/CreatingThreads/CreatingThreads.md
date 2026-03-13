---
title: "Thread Management"
book: "Threading Programming Guide"
chapterId: "10000057i-CH15"
date: "2014-07-15"
description: "Explains how to use threads in Cocoa applications."
identifier: "//apple_ref/doc/uid/10000057i"
source: apple-archive
---

# Thread Management

> **Threading Programming Guide**
> Last updated: 2014-07-15

[Next](../RunLoopManagement/RunLoopManagement.html)[Previous](../AboutThreads/AboutThreads.html)
        
        
        [](../index.html)
        
        # Thread Management
Each process (application) in OS X or iOS is made up of one or more threads, each of which represents a single path of execution through the application's code. Every application starts with a single thread, which runs the application's `main` function. Applications can spawn additional threads, each of which executes the code of a specific function.

When an application spawns a new thread, that thread becomes an independent entity inside of the application's process space. Each thread has its own execution stack and is scheduled for runtime separately by the kernel. A thread can communicate with other threads and other processes, perform I/O operations, and do anything else you might need it to do. Because they are inside the same process space, however, all threads in a single application share the same virtual memory space and have the same access rights as the process itself. 

This chapter provides an overview of the thread technologies available in OS X and iOS along with examples of how to use those technologies in your applications. 

> **Note:** For a historical look at the threading architecture of Mac OS, and for additional background information on threads, see Technical Note TN2028, “Threading Architectures”. 

## Thread Costs
Threading has a real cost to your program (and the system) in terms of memory use and performance. Each thread requires the allocation of memory in both the kernel memory space and your program’s memory space. The core structures needed to manage your thread and coordinate its scheduling are stored in the kernel using wired memory. Your thread’s stack space and per-thread data is stored in your program’s memory space. Most of these structures are created and initialized when you first create the thread—a process that can be relatively expensive because of the required interactions with the kernel.  

Table 2-1 quantifies the approximate costs associated with creating a new user-level thread in your application. Some of these costs are configurable, such as the amount of stack space allocated for secondary threads. The time cost for creating a thread is a rough approximation and should be used only for relative comparisons with each other. Thread creation times can vary greatly depending on processor load, the speed of the computer, and the amount of available system and program memory.

**Table 2-1**  Thread creation costsItem

Approximate cost

Notes

Kernel data structures

Approximately 1 KB

This memory is used to store the thread data structures and attributes, much of which is allocated as wired memory and therefore cannot be paged to disk.

Stack space

512 KB (secondary threads)

8 MB (OS X main thread)

1 MB (iOS main thread)

The minimum allowed stack size for secondary threads is 16 KB and the stack size must be a multiple of 4 KB. The space for this memory is set aside in your process space at thread creation time, but the actual pages associated with that memory are not created until they are needed. 

Creation time

Approximately 90 microseconds

This value reflects the time between the initial call to create the thread and the time at which the thread’s entry point routine began executing. The figures were determined by analyzing the mean and median values generated during thread creation on an Intel-based iMac with a 2 GHz Core Duo processor and 1 GB of RAM running OS X v10.5.

> **Note:** Because of their underlying kernel support, operation objects can often create threads more quickly. Rather than creating threads from scratch every time, they use pools of threads already residing in the kernel to save on allocation time. For more information about using operation objects, see *[Concurrency Programming Guide](../../../../General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)*.  

Another cost to consider when writing threaded code is the production costs. Designing a threaded application can sometimes require fundamental changes to the way you organize your application’s data structures. Making those changes might be necessary to avoid the use of synchronization, which can itself impose a tremendous performance penalty on poorly designed applications. Designing those data structures, and debugging problems in threaded code, can increase the time it takes to develop a threaded application. Avoiding those costs can create bigger problems at runtime, however, if your threads spend too much time waiting on locks or doing nothing.

## Creating a Thread
Creating low-level threads is relatively simple. In all cases, you must have a function or method to act as your thread’s main entry point and you must use one of the available thread routines to start your thread. The following sections show the basic creation process for the more commonly used thread technologies. Threads created using these techniques inherit a default set of attributes, determined by the technology you use. For information on how to configure your threads, see [Configuring Thread Attributes](#//apple_ref/doc/uid/10000057i-CH15-SW8).  

### Using NSThread
There are two ways to create a thread using the `[NSThread](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/cl/NSThread)` class:

Use the `[detachNewThreadSelector:toTarget:withObject:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/clm/NSThread/detachNewThreadSelector:toTarget:withObject:)` class method to spawn the new thread.

Create a new `NSThread` object and call its `start` method. (Supported only in iOS and OS X v10.5 and later.)

Both techniques create a detached thread in your application. A detached thread means that the thread’s resources are automatically reclaimed by the system when the thread exits. It also means that your code does not have to join explicitly with the thread later. 

Because the `detachNewThreadSelector:toTarget:withObject:` method is supported in all versions of OS X, it is often found in existing Cocoa applications that use threads. To detach a new thread, you simply provide the name of the method (specified as a selector) that you want to use as the thread’s entry point, the object that defines that method, and any data you want to pass to the thread at startup. The following example shows a basic invocation of this method that spawns a thread using a custom method of the current object.  

[NSThread detachNewThreadSelector:@selector(myThreadMainMethod:) toTarget:self withObject:nil];Prior to OS X v10.5, you used the `NSThread` class primarily to spawn threads. Although you could get an `NSThread` object and access some thread attributes, you could only do so from the thread itself after it was running. In OS X v10.5, support was added for creating `NSThread` objects without immediately spawning the corresponding new thread. (This support is also available in iOS.) This support made it possible to get and set various thread attributes prior to starting the thread. It also made it possible to use that thread object to refer to the running thread later.  

The simple way to initialize an `NSThread` object in OS X v10.5 and later is to use the `[initWithTarget:selector:object:](https://developer.apple.com/documentation/foundation/nsthread/1414773-initwithtarget)` method. This method takes the exact same information as the `detachNewThreadSelector:toTarget:withObject:` method and uses it to initialize a new `NSThread` instance. It does not start the thread, however. To start the thread, you call the thread object’s `start` method explicitly, as shown in the following example: 

NSThread* myThread = [[NSThread alloc] initWithTarget:self
```
                                        selector:@selector(myThreadMainMethod:)
```

```
                                        object:nil];
```

```
[myThread start];  // Actually create the thread
```

> **Note:** An alternative to using the `initWithTarget:selector:object:` method is to subclass `NSThread` and override its `main` method. You would use the overridden version of this method to implement your thread’s main entry point. For more information, see the subclassing notes in *[NSThread Class Reference](https://developer.apple.com/documentation/foundation/nsthread)*. 

If you have an `NSThread` object whose thread is currently running, one way you can send messages to that thread is to use the `[performSelector:onThread:withObject:waitUntilDone:](https://developer.apple.com/documentation/objectivec/nsobject/1414476-performselector)` method of almost any object in your application. Support for performing selectors on threads (other than the main thread) was introduced in OS X v10.5 and is a convenient way to communicate between threads. (This support is also available in iOS.) The messages you send using this technique are executed directly by the other thread as part of its normal run-loop processing. (Of course, this does mean that the target thread has to be running in its run loop; see [Run Loops](../RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW1).) You may still need some form of synchronization when you communicate this way, but it is simpler than setting up communications ports between the threads. 

> **Note:** Although good for occasional communication between threads, you should not use the  `performSelector:onThread:withObject:waitUntilDone:` method for time critical or frequent communication between threads. 

For a list of other thread communication options, see [Setting the Detached State of a Thread](#//apple_ref/doc/uid/10000057i-CH15-SW3). 

### Using POSIX Threads
OS X and iOS provide C-based support for creating threads using the POSIX thread API. This technology can actually be used in any type of application (including Cocoa and Cocoa Touch applications) and might be more convenient if you are writing your software for multiple platforms. The POSIX routine you use to create threads is called, appropriately enough, `pthread_create`. 

Listing 2-1 shows two custom functions for creating a thread using POSIX calls. The `LaunchThread` function creates a new thread whose main routine is implemented in the `PosixThreadMainRoutine` function. Because POSIX creates threads as joinable by default, this example changes the thread’s attributes to create a detached thread. Marking the thread as detached gives the system a chance to reclaim the resources for that thread immediately when it exits.  

**Listing 2-1**  Creating a thread in C

#include <assert.h>
```
#include <pthread.h>
```

```
 
```

```
void* PosixThreadMainRoutine(void* data)
```

```
{
```

```
    // Do some work here.
```

```
 
```

```
    return NULL;
```

```
}
```

```
 
```

```
void LaunchThread()
```

```
{
```

```
    // Create the thread using POSIX routines.
```

```
    pthread_attr_t  attr;
```

```
    pthread_t       posixThreadID;
```

```
    int             returnVal;
```

```
 
```

```
    returnVal = pthread_attr_init(&attr);
```

```
    assert(!returnVal);
```

```
    returnVal = pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_DETACHED);
```

```
    assert(!returnVal);
```

```
 
```

```
    int     threadError = pthread_create(&posixThreadID, &attr, &PosixThreadMainRoutine, NULL);
```

```
 
```

```
    returnVal = pthread_attr_destroy(&attr);
```

```
    assert(!returnVal);
```

```
    if (threadError != 0)
```

```
    {
```

```
         // Report an error.
```

```
    }
```

```
}
```
If you add the code from the preceding listing to one of your source files and call the `LaunchThread` function, it would create a new detached thread in your application. Of course, new threads created using this code would not do anything useful. The threads would launch and almost immediately exit. To make things more interesting, you would need to add code to the `PosixThreadMainRoutine` function to do some actual work. To ensure that a thread knows what work to do, you can pass it a pointer to some data at creation time. You pass this pointer as the last parameter of the `pthread_create` function. 

To communicate information from your newly created thread back to your application’s main thread, you need to establish a communications path between the target threads. For C-based applications, there are several ways to communicate between threads, including the use of ports, conditions, or shared memory. For long-lived threads, you should almost always set up some sort of inter-thread communications mechanism to give your application’s main thread a way to check the status of the thread or shut it down cleanly when the application exits. 

For more information about POSIX thread functions, see the `[pthread](../../../../System/Conceptual/ManPages_iPhoneOS/man3/pthread.3.html#//apple_ref/doc/man/3/pthread)` man page. 

### Using NSObject to Spawn a Thread
In iOS and OS X v10.5 and later, all objects have the ability to spawn a new thread and use it to execute one of their methods. The `[performSelectorInBackground:withObject:](https://developer.apple.com/documentation/objectivec/nsobject/1412390-performselector)` method creates a new detached thread and uses the specified method as the entry point for the new thread. For example, if you have some object (represented by the variable `myObj`) and that object has a method called `doSomething` that you want to run in a background thread, you could use the following code to do that: 

[myObj performSelectorInBackground:@selector(doSomething) withObject:nil];The effect of calling this method is the same as if you called the `[detachNewThreadSelector:toTarget:withObject:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/clm/NSThread/detachNewThreadSelector:toTarget:withObject:)` method of `[NSThread](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/cl/NSThread)` with the current object, selector, and parameter object as parameters. The new thread is spawned immediately using the default configuration and begins running. Inside the selector, you must configure the thread just as you would any thread. For example, you would need to set up an autorelease pool (if you were not using garbage collection) and configure the thread’s run loop if you planned to use it. For information on how to configure new threads, see [Configuring Thread Attributes](#//apple_ref/doc/uid/10000057i-CH15-SW8). 

### Using POSIX Threads in a Cocoa Application
Although the `[NSThread](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/cl/NSThread)` class is the main interface for creating threads in Cocoa applications, you are free to use POSIX threads instead if doing so is more convenient for you. For example, you might use POSIX threads if you already have code that uses them and you do not want to rewrite it. If you do plan to use the POSIX threads in a Cocoa application, you should still be aware of the interactions between Cocoa and threads and obey the guidelines in the following sections.  

#### Protecting the Cocoa Frameworks
For multithreaded applications, Cocoa frameworks use locks and other forms of internal synchronization to ensure they behave correctly. To prevent these locks from degrading performance in the single-threaded case, however, Cocoa does not create them until the application spawns its first new thread using the `[NSThread](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/cl/NSThread)` class. If you spawn threads using only POSIX thread routines, Cocoa does not receive the notifications it needs to know that your application is now multithreaded. When that happens, operations involving the Cocoa frameworks may destabilize or crash your application. 

To let Cocoa know that you intend to use multiple threads, all you have to do is spawn a single thread using the `NSThread` class and let that thread immediately exit. Your thread entry point need not do anything. Just the act of spawning a thread using `NSThread` is enough to ensure that the locks needed by the Cocoa frameworks are put in place. 

If you are not sure if Cocoa thinks your application is multithreaded or not, you can use the `[isMultiThreaded](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/clm/NSThread/isMultiThreaded)` method of `NSThread` to check. 

#### Mixing POSIX and Cocoa Locks
It is safe to use a mixture of POSIX and Cocoa locks inside the same application. Cocoa lock and condition objects are essentially just wrappers for POSIX mutexes and conditions. For a given lock, however, you must always use the same interface to create and manipulate that lock. In other words, you cannot use a Cocoa `[NSLock](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSLock/Description.html#//apple_ref/occ/cl/NSLock)` object to manipulate a mutex you created using the `[pthread_mutex_init](../../../../System/Conceptual/ManPages_iPhoneOS/man3/pthread_mutex_init.3.html#//apple_ref/doc/man/3/pthread_mutex_init)` function, and vice versa.

## Configuring Thread Attributes
After you create a thread, and sometimes before, you may want to configure different portions of the thread environment. The following sections describe some of the changes you can make and when you might make them. 

### Configuring the Stack Size of a Thread
For each new thread you create, the system allocates a specific amount of memory in your process space to act as the stack for that thread. The stack manages the stack frames and is also where any local variables for the thread are declared. The amount of memory allocated for threads is listed in [Thread Costs](#//apple_ref/doc/uid/10000057i-CH15-SW7). 

If you want to change the stack size of a given thread, you must do so before you create the thread. All of the threading technologies provide some way of setting the stack size, although setting the stack size using `[NSThread](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/cl/NSThread)` is available only in iOS and OS X v10.5 and later. Table 2-2 lists the different options for each technology. 

**Table 2-2**  Setting the stack size of a threadTechnology

Option

Cocoa

In iOS and OS X v10.5 and later, allocate and initialize an `NSThread` object (do not use the `[detachNewThreadSelector:toTarget:withObject:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/clm/NSThread/detachNewThreadSelector:toTarget:withObject:)` method). Before calling the `start` method of the thread object, use the `[setStackSize:](https://developer.apple.com/documentation/foundation/nsthread/1415190-stacksize)` method to specify the new stack size. 

POSIX

Create a new `pthread_attr_t` structure and use the `pthread_attr_setstacksize` function to change the default stack size. Pass the attributes to the `pthread_create` function when creating your thread. 

Multiprocessing Services

Pass the appropriate stack size value to the `[MPCreateTask](https://developer.apple.com/documentation/coreservices/1585779-mpcreatetask)` function when you create your thread. 

### Configuring Thread-Local Storage
Each thread maintains a dictionary of key-value pairs that can be accessed from anywhere in the thread. You can use this dictionary to store information that you want to persist throughout the execution of your thread. For example, you could use it to store state information that you want to persist through multiple iterations of your thread’s run loop. 

Cocoa and POSIX store the thread dictionary in different ways, so you cannot mix and match calls to the two technologies. As long as you stick with one technology inside your thread code, however, the end results should be similar. In Cocoa, you use the `[threadDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/instm/NSThread/threadDictionary)` method of an `[NSThread](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/cl/NSThread)` object to retrieve an `NSMutableDictionary` object, to which you can add any keys required by your thread. In POSIX, you use the `pthread_setspecific` and `pthread_getspecific` functions to set and get the keys and values of your thread. 

### Setting the Detached State of a Thread
Most high-level thread technologies create detached threads by default. In most cases, detached threads are preferred because they allow the system to free up the thread’s data structures immediately upon completion of the thread. Detached threads also do not require explicit interactions with your program. The means of retrieving results from the thread is left to your discretion. By comparison, the system does not reclaim the resources for joinable threads until another thread explicitly joins with that thread, a process which may block the thread that performs the join. 

You can think of joinable threads as akin to child threads. Although they still run as independent threads, a joinable thread must be joined by another thread before its resources can be reclaimed by the system. Joinable threads also provide an explicit way to pass data from an exiting thread to another thread. Just before it exits, a joinable thread can pass a data pointer or other return value to the `pthread_exit` function. Another thread can then claim this data by calling the `pthread_join` function. 

> **Important:** At application exit time, detached threads can be terminated immediately but joinable threads cannot. Each joinable thread must be joined before the process is allowed to exit. Joinable threads may therefore be preferable in cases where the thread is doing critical work that should not be interrupted, such as saving data to disk. 

If you do want to create joinable threads, the only way to do so is using POSIX threads. POSIX creates threads as joinable by default. To mark a thread as detached or joinable, modify the thread attributes using the `pthread_attr_setdetachstate` function prior to creating the thread. After the thread begins, you can change a joinable thread to a detached thread by calling the `pthread_detach` function. For more information about these POSIX thread functions, see the `[pthread](../../../../System/Conceptual/ManPages_iPhoneOS/man3/pthread.3.html#//apple_ref/doc/man/3/pthread)` man page. For information on how to join with a thread, see the `[pthread_join](../../../../System/Conceptual/ManPages_iPhoneOS/man3/pthread_join.3.html#//apple_ref/doc/man/3/pthread_join)` man page.

### Setting the Thread Priority
Any new thread you create has a default priority associated with it. The kernel’s scheduling algorithm takes thread priorities into account when determining which threads to run, with higher priority threads being more likely to run than threads with lower priorities. Higher priorities do not guarantee a specific amount of execution time for your thread, just that it is more likely to be chosen by the scheduler when compared to lower-priority threads. 

> **Important:** It is generally a good idea to leave the priorities of your threads at their default values. Increasing the priorities of some threads also increases the likelihood of starvation among lower-priority threads. If your application contains high-priority and low-priority threads that must interact with each other, the starvation of lower-priority threads may block other threads and create performance bottlenecks.

If you do want to modify thread priorities, both Cocoa and POSIX provide a way to do so. For Cocoa threads, you can use the `[setThreadPriority:](https://developer.apple.com/documentation/foundation/nsthread/1407523-setthreadpriority)` class method of `[NSThread](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/cl/NSThread)` to set the priority of the currently running thread. For POSIX threads, you use the `pthread_setschedparam` function. For more information, see *[NSThread Class Reference](https://developer.apple.com/documentation/foundation/nsthread)* or `[pthread_setschedparam](../../../../System/Conceptual/ManPages_iPhoneOS/man3/pthread_setschedparam.3.html#//apple_ref/doc/man/3/pthread_setschedparam)` man page.

## Writing Your Thread Entry Routine
For the most part, the structure of your thread’s entry point routines is the same in OS X as it is on other platforms. You initialize your data structures, do some work or optionally set up a run loop, and clean up when your thread’s code is done. Depending on your design, there may be some additional steps you need to take when writing your entry routine. 

### Creating an Autorelease Pool
Applications that link in Objective-C frameworks typically must create at least one autorelease pool in each of their threads. If an application uses the managed model—where the application handles the retaining and releasing of objects—the autorelease pool catches any objects that are autoreleased from that thread. 

If an application uses garbage collection instead of the managed memory model, creation of an autorelease pool is not strictly necessary. The presence of an autorelease pool in a garbage-collected application is not harmful, and for the most part is simply ignored. It is allowed for cases where a code module must support both garbage collection and the managed memory model. In such a case, the autorelease pool must be present to support the managed memory model code and is simply ignored if the application is run with garbage collection enabled.

If your application uses the managed memory model, creating an autorelease pool should be the first thing you do in your thread entry routine. Similarly, destroying this autorelease pool should be the last thing you do in your thread. This pool ensures that autoreleased objects are caught, although it does not release them until the thread itself exits. Listing 2-2 shows the structure of a basic thread entry routine that uses an autorelease pool.

**Listing 2-2**  Defining your thread entry point routine

- (void)myThreadMainRoutine
```
{
```

```
    NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init]; // Top-level pool
```

```
 
```

```
    // Do thread work here.
```

```
 
```

```
    [pool release];  // Release the objects in the pool.
```

```
}
```
Because the top-level autorelease pool does not release its objects until the thread exits, long-lived threads should create additional autorelease pools to free objects more frequently. For example, a thread that uses a run loop might create and release an autorelease pool each time through that run loop. Releasing objects more frequently prevents your application’s memory footprint from growing too large, which can lead to performance problems. As with any performance-related behavior though, you should measure the actual performance of your code and tune your use of autorelease pools appropriately.

For more information on memory management and autorelease pools, see *[Advanced Memory Management Programming Guide](../../MemoryMgmt/Articles/MemoryMgmt.html#//apple_ref/doc/uid/10000011i)*. 

### Setting Up an Exception Handler
If your application catches and handles exceptions, your thread code should be prepared to catch any exceptions that might occur. Although it is best to handle exceptions at the point where they might occur, failure to catch a thrown exception in a thread causes your application to exit. Installing a final try/catch in your thread entry routine allows you to catch any unknown exceptions and provide an appropriate response. 

You can use either the C++ or Objective-C exception handling style when building your project in Xcode. For information about setting how to raise and catch exceptions in Objective-C, see *[Exception Programming Topics](../../Exceptions/Exceptions.html#//apple_ref/doc/uid/10000012i)*.  

### Setting Up a Run Loop
When writing code you want to run on a separate thread, you have two options. The first option is to write the code for a thread as one long task to be performed with little or no interruption, and have the thread exit when it finishes. The second option is put your thread into a loop and have it process requests dynamically as they arrive. The first option requires no special setup for your code; you just start doing the work you want to do. The second option, however, involves setting up your thread’s run loop. 

OS X and iOS provide built-in support for implementing run loops in every thread. The app frameworks start the run loop of your application’s main thread automatically. If you create any secondary threads, you must configure the run loop and start it manually. 

For information on using and configuring run loops, see [Run Loops](../RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW1). 

## Terminating a Thread
The recommended way to exit a thread is to let it exit its entry point routine normally. Although Cocoa, POSIX, and Multiprocessing Services offer routines for killing threads directly, the use of such routines is strongly discouraged. Killing a thread prevents that thread from cleaning up after itself. Memory allocated by the thread could potentially be leaked and any other resources currently in use by the thread might not be cleaned up properly, creating potential problems later. 

If you anticipate the need to terminate a thread in the middle of an operation, you should design your threads from the outset to respond to a cancel or exit message. For long-running operations, this might mean stopping work periodically and checking to see if such a message arrived. If a message does come in asking the thread to exit, the thread would then have the opportunity to perform any needed cleanup and exit gracefully; otherwise, it could simply go back to work and process the next chunk of data.

One way to respond to cancel messages is to use a run loop input source to receive such messages. Listing 2-3 shows the structure of how this code might look in your thread’s main entry routine. (The example shows the main loop portion only and does not include the steps for setting up an autorelease pool or configuring the actual work to do.) The example installs a custom input source on the run loop that presumably can be messaged from another one of your threads; for information on setting up input sources, see [Configuring Run Loop Sources](../RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW7). After performing a portion of the total amount of work, the thread runs the run loop briefly to see if a message arrived on the input source. If not, the run loop exits immediately and the loop continues with the next chunk of work. Because the handler does not have direct access to the `exitNow` local variable, the exit condition is communicated through a key-value pair in the thread dictionary. 

**Listing 2-3**  Checking for an exit condition during a long job

```
- (void)threadMainRoutine
```

```
{
```

```
    BOOL moreWorkToDo = YES;
```

```
    BOOL exitNow = NO;
```

```
    NSRunLoop* runLoop = [NSRunLoop currentRunLoop];
```

```
 
```

```
    // Add the exitNow BOOL to the thread dictionary.
```

```
    NSMutableDictionary* threadDict = [[NSThread currentThread] threadDictionary];
```

```
    [threadDict setValue:[NSNumber numberWithBool:exitNow] forKey:@"ThreadShouldExitNow"];
```

```
 
```

```
    // Install an input source.
```

```
    [self myInstallCustomInputSource];
```

```
 
```

```
    while (moreWorkToDo && !exitNow)
```

```
    {
```

```
        // Do one chunk of a larger body of work here.
```

```
        // Change the value of the moreWorkToDo Boolean when done.
```

```
 
```

```
        // Run the run loop but timeout immediately if the input source isn't waiting to fire.
```

```
        [runLoop runUntilDate:[NSDate date]];
```

```
 
```

```
        // Check to see if an input source handler changed the exitNow value.
```

```
        exitNow = [[threadDict valueForKey:@"ThreadShouldExitNow"] boolValue];
```

```
    }
```

```
}
```

        
            [Next](../RunLoopManagement/RunLoopManagement.html)[Previous](../AboutThreads/AboutThreads.html)
        
         Copyright &#x00a9; 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-07-15