---
title: "Basic Performance Tips"
book: "Performance Overview"
chapterId: "TP40001410-CH204"
date: "2013-10-22"
description: "Introduces the factors that determine performance."
identifier: "//apple_ref/doc/uid/TP40001410"
source: apple-archive
---

# Basic Performance Tips

> **Performance Overview**
> Last updated: 2013-10-22

[Next](../PerformanceTools/PerformanceTools.html)[Previous](../DevelopingForPerf/DevelopingForPerf.html)
        
        
        [](../index.html)
        
        # Basic Performance Tips
This chapter offers practical advice for how to tune your programs. It offers suggestions of areas you should monitor with the performance tools and also provides a list of practical tips for improving performance. 

## Common Areas to Monitor
Many performance problems can be traced to specific parts of your program. As you design and implement your code, you should monitor those areas to make sure they meet the performance targets you set. 

### Code for Your Program’s Key Tasks
As you design your program, consider the tasks or workflows that users will encounter the most. During your implementation phase, be sure to monitor the code for those tasks and make sure their performance does not drop below acceptable levels. If it does, you should take immediate actions to correct the problems.

The key tasks performed by a program varies from program to program. For example, a word processor might need to be fast during text input and display, while a file utility program would need to be fast at scanning files and directories on a hard disk. It is up to you to decide which tasks your users are most likely to perform. 

For information on how to identify and fix slow operations in your program, see *[Code Speed Performance Guidelines](../../CodeSpeed/CodeSpeed.html#//apple_ref/doc/uid/10000150i)*.

### Drawing Code
Most programs do some amount of drawing. If your program uses only standard windows and controls, then you probably do not need to worry too much about drawing performance. However, if you do any custom drawing, you need to monitor your drawing code and make sure it is performing at acceptable levels. In particular, if you support any of the following, you should investigate ways to optimize your drawing code. 

Live resizing

Custom view drawing code, especially if portions of the view can be updated without updating the whole view

Textured graphics

Entirely opaque views

For information on how to optimize drawing performance, see *[Drawing Performance Guidelines](../../Drawing/Articles/DrawingPerformance.html#//apple_ref/doc/uid/10000151i)*. 

### Launch Time Initialization Code
Launch time is where you initialize your program’s data structures and prepare your application to receive user input. However, many programs do much more work at launch time than is necessary. In many cases, tasks performed at launch time can be deferred until after the application has posted its user interface and started processing events. This deferral gives the user the perception that your application is fast, which is a good first impression to make.

For applications that need to run in OS X version 10.3.3 and earlier, another way to improve launch times is to prebind your application. Prebinding involves precalculating library address ranges and storing those values in your application binary. This step eliminates the need for the dynamic loader (`dyld`) to calculate those address ranges at launch time. Improvements in `dyld` for OS X version 10.3.4 make prebinding unnecessary in that and later releases. Similarly, prebinding is unnecessary in iOS.

For information on how to improve launch-time performance, see *[Launch Time Performance Guidelines](../../LaunchTime/LaunchTime.html#//apple_ref/doc/uid/10000148i)*. 

### File Access Code
The file system is a bottleneck for getting information into memory and the CPU. In the time it takes to access a file, tens of millions of instructions may be executed. It is therefore imperative that you examine the way your program uses files and to make sure those files are actually needed and used properly. 

Minimizing the number of files you use is one way to improve file-related performance. When you do access files, keep the following in mind:

Understand how the system caches file data and know how to optimize the use of those caches. Avoid caching data yourself unless you plan to refer to it more than once.

Read and write data sequentially whenever possible. Jumping around a file takes extra time to seek to the new location.

Read larger blocks of data from files whenever possible, keeping in mind that reading too much data at once might cause different problems. For example, reading the entire contents of a 32 MB file might trigger paging of those contents before the operation is complete. 

Avoid closing and reopening files unnecessarily. If caching is enabled, doing so may cause the cache to be refreshed even if the data did not change.

For information on how to identify and fix file-related performance problems, see *[File-System Performance Guidelines](../../FileSystem/FileSystem.html#//apple_ref/doc/uid/10000161i)*. 

### Application Footprint
The size of your code can have a tremendous effect on system performance. The more memory pages used by your program, the fewer there are available for the system and other programs. This memory pressure can eventually lead to paging and an overall system slowdown. 

Managing your code footprint is all about organizing your code and data structures. You need to make sure you have the right pieces in memory and that you are not causing any memory pages to be read or written unnecessarily. Some of the problems that cause a large memory footprint are as follows:

Code pages contain unused code. The compiler typically organizes code by compilation module, which is not always the best technique. Organizing your modules based on what code executes together might be a better option. 

Static or constant data is stored on writable pages. During paging, this data is written to disk unnecessarily. Do what you can to move this data to non-writable pages that can be purged quickly. 

A program exports more symbols than are actually needed. Symbols take up space and are only needed by external code modules that must call into your program. Remove any symbols that should not be used externally.

Code is not properly optimized by the compiler and linker. Be sure to try the compiler’s optimization settings and see which one works best for your program. 

Too many [frameworks](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56) are included by the program. Load only the frameworks your program actually uses. 

For information on how to find and fix code footprint problems, see *[Code Size Performance Guidelines](../../CodeFootprint/CodeFootprint.html#//apple_ref/doc/uid/10000149i)*. 

### Memory Allocation Code
Programs allocate memory for storing both permanent and temporary data structures. Each memory allocation has a cost associated with it, both in CPU time and in memory consumption. Understanding when your program allocates memory and how that memory is used can help you reduce both of those costs. 

Understanding your program’s memory usage can help determine ways to reduce that usage. You can use the available performance tools to find out if [autoreleased Objective-C objects](../../../../General/Conceptual/DevPedia-CocoaCore/MemoryManagement.html#//apple_ref/doc/uid/TP40008195-CH27) are being deallocated before they cause too much paging. You can also use these tools to find memory leaks caused by bugs in your code. Watching the number of times you call `malloc` might also point out places where you can reuse existing memory blocks rather than create new ones. 

One important rule to follow when allocating memory is to be lazy. Defer memory allocations until you actually need the memory being used. For some additional ways you can be lazy with memory allocations, see [Be Lazy ](#//apple_ref/doc/uid/TP40001410-CH204-BCIHHDAA). 

For information about optimizing your memory allocation patterns, see *[Memory Usage Performance Guidelines](../../ManagingMemory/ManagingMemory.html#//apple_ref/doc/uid/10000160i)*. 

## Fundamental Optimization Tips
Before you begin implementing a new program, there are several performance enhancements you should consider adding. Although you might not be able to take advantage of all of these enhancements in every case, you should at least consider them during your design phase.

### Be Lazy 
A very simple way to improve performance is to make sure your application does not perform any unnecessary work. Each moment of an application’s time should be spent responding to the user’s current request, not predicting future requests. If you do not need a resource right away, such as a [nib file](../../../../General/Conceptual/DevPedia-CocoaCore/NibFile.html#//apple_ref/doc/uid/TP40008195-CH34) containing a preferences window, don’t load it. Such an action takes time to execute because it accesses the file system, and if the user never opens that preference window, the process of loading its nib file is a waste of time. 

The basic rule to follow is to wait until the user requests something from your application, then use the necessary resources to fulfill the request. You should cache data only in situations where there is a measurable performance benefit. Preloading caches on the assumption that the rest of the application will run faster can actually degrade performance in low-memory situations. In such a situation, your cached data might be paged to disk before it can be used. Thus, any savings you gained by caching the data turns into a loss because that data must now be read from disk twice before it is ever used. If you really want to cache data, do so after the operation has been performed once.  

Some other things to be lazy about include the following:

Defer memory allocations until you actually need the memory.

Don’t zero-initialize blocks of memory. Call the `calloc` function to do it for you lazily.

Give the system the chance to load your code lazily. Profile and organize your code so that the system loads only the code needed for the current operation.

Defer reading the contents of a file until you actually need the information.

### Take Advantage of Perceived Performance
The perception of performance is just as effective as actual performance in many cases. Many program tasks can be performed in the background, using a dispatch queue, or at idle time. Doing this keeps the application’s main thread free to handle user interactions and makes the program interface feel more responsive to the user. As you design your program, think about which tasks can be moved to the background effectively. For example, if your program needs to scan a number of files or perform lengthy calculations, do so using a dispatch queue.

Another way to improve perceived performance is to make sure your application launches quickly. At launch time, defer any tasks that do not contribute to the immediate presentation of your application interface. For example, defer the creation of large data structures until after your application has finished launching and displayed its main window. If you use your main window to display some data that must be calculated or retrieved at launch time, show the window first along with a progress indicator or other status message indicating that the data is being loaded. For applications that use plug-ins, avoid loading plug-ins until their code is actually needed. 

### Use Event-Based Handlers
All modern Mac apps should be using the [Cocoa](../../../../General/Conceptual/DevPedia-CocoaCore/Cocoa.html#//apple_ref/doc/uid/TP40008195-CH9) event system or Carbon Event Manager. (Similarly, iPhone applications must use the touch-based event system provided by the UIKit framework.) Your application should never retrieve events by polling the system. Doing so is highly inefficient. In fact, when there are no events to process, polling code is a 100 percent waste of CPU time. The modern event-based APIs are designed to provide the following benefits: 

They make your program more responsive to the user.

They reduce your application’s CPU usage.

They minimize your application’s working set—the number of code pages loaded in memory at any given time.

They allow the system to manage power aggressively.

Besides user events, you should avoid polling in other situations as well. Threads in OS X and iOS use run loops to provide on-demand responses to timers, network events, and other incoming data. Many frameworks also use an asynchronous programming model for certain tasks, notifying a designated handler function or method when the task completes. In OS X v10.6 and later, dispatch sources also provide a way for you to receive important events asynchronously and execute them on a dispatch queue. 

### Improve the Concurrency of Your Program’s Tasks
On computers with multiple cores, concurrency is another way to improve both the perceived and actual performance of your program. A program that is able to perform multiple tasks at the same time can execute those tasks in parallel on a multicore computer. Even when a computer has only one core, factoring your code into multiple, asynchronous tasks can provide a perceived speed boost when done correctly. Specifically, you should perform custom tasks using a dispatch queue and leave your main thread free to handle user events and update your user interface primarily. 

Before you begin adding support for concurrency, though, be sure to put some thought into how your program might implement the corresponding tasks effectively. Factoring your code into different tasks requires some consideration of your program data structures and code paths. Tasks that share data structures may require the use of a serial dispatch queue to synchronize access to those structures. 

For information on how to implement concurrency in your program, see *[Concurrency Programming Guide](../../../../General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)*.

### Use the Accelerate Framework
If your application performs a lot of mathematical computations on scalar data, you should consider using the Accelerate [framework](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56) (`Accelerate.framework`) to accelerate those calculations. The Accelerate framework takes advantage of any available vector processing units (such as the Intel x86 SSE extensions) to perform multiple calculations in parallel. By coding to the framework, instead of to the vector unit, you can avoid having to create separate code paths for each platform architecture. The Accelerate framework is highly tuned for all of the architectures OS X supports.

Tools such as Instruments can help point out portions of your program that might benefit from using the Accelerate framework. For more information about using these and other tools, see [Performance Tools](../PerformanceTools/PerformanceTools.html#//apple_ref/doc/uid/TP40001410-CH205-BCIIHAAJ).

### Modernize Your Application 
If your program was designed to run on an older version of Mac OS, you should update your code to support the conventions of OS X. In particular, you should avoid using older technologies such as Carbon or legacy technologies such as QuickDraw. Instead, you should build your applications using [Cocoa or Cocoa Touch](../../../../General/Conceptual/DevPedia-CocoaCore/Cocoa.html#//apple_ref/doc/uid/TP40008195-CH9). You should also update your binary format to Mach-O. The Mach-O format is the only format supported on Intel-based Macintosh computers and iOS-based devices. 

For a list of the technologies available for use in Mac apps, see *[Mac Technology Overview](../../../../MacOSX/Conceptual/OSX_Technology_Overview/About/About.html#//apple_ref/doc/uid/TP40001067)*. For a list of technologies available for use in iOS, see *iOS Technology Overview*. 

        
            [Next](../PerformanceTools/PerformanceTools.html)[Previous](../DevelopingForPerf/DevelopingForPerf.html)
        
         Copyright &#x00a9; 2004, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-10-22