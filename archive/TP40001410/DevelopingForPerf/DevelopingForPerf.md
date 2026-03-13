---
title: "Developing for Performance"
book: "Performance Overview"
chapterId: "TP40001410-CH203"
date: "2013-10-22"
description: "Introduces the factors that determine performance."
identifier: "//apple_ref/doc/uid/TP40001410"
source: apple-archive
---

# Developing for Performance

> **Performance Overview**
> Last updated: 2013-10-22

[Next](../BasicTips/BasicTips.html)[Previous](../Introduction/Introduction.html)
        
        
        [](../index.html)
        
        # Developing for Performance
Performance is an aspect of software design that is often overlooked until it becomes a serious problem. If you wait until the end of your development cycle to do performance tuning, it may be too late to achieve any significant improvements. Performance is something to include early in the design phase and continue improving all throughout the development cycle.

Of course, in order to design for performance, it helps to understand what performance is. The sections in this chapter provide background information about the factors that influence performance, how those factors manifest themselves in OS X and iOS, and how you can approach the monitoring of those factors. 

## What Is Performance?
The term “performance” may mean different things to different people. So before embarking on a quest to improve the performance of your application, now is a good time to consider what this term means. 

Many people equate performance with speed. Indeed, if a program performs a complex operation in one second, you might think the program has good performance. Taken by itself, though, speed can be a misleading measurement. In complex software systems, the speed of an operation is not a fixed value. If you perform the same operation several times under different conditions, the time it takes to complete that operation could vary widely. That is because the program is only one of many processes sharing resources on the local system, and the use (or abuse) of those resources affects all other processes.

The following sections explain performance in terms of two different concepts: efficient resource usage and perceived performance. Both of these concepts have an important impact on how you design and implement your application, and understanding how to use both can lead to better overall performance. 

### The Efficient Use of Resources
A computer shares a limited number of resources among all the running processes. At the lowest level, these resources break down to the following categories:

CPU time

Memory space

Mass storage space

All of your data resides either in memory or on some sort of mass storage device and must be operated on by the CPU. An efficient application uses all of these resources carefully. The following sections provide more detail about each type of resource and its effects on your programs. 

#### CPU Time
CPU time is doled out by the system so you must make the best possible use of what time you have. Because both OS X and iOS implement symmetric multiprocessing, each thread on the system is assigned a slice of time (maximum of 10 milliseconds) in which to run. At the end of that time (or before in many cases) the system takes back control of the CPU and gives it to a different thread. 

On a typical system with many active threads, if every thread used its full allotment of time, performance would be terrible. This leads to one of the most important goals for writing an application:

*Goal:**If your program has nothing to do, it should not consume CPU time*. 

The best way to accomplish this goal is to use an event-based model. Using modern event-handling systems, such as the ones found in [Cocoa and iOS](../../../../General/Conceptual/DevPedia-CocoaCore/Cocoa.html#//apple_ref/doc/uid/TP40008195-CH9), means your program’s threads are run only when there is work to be done.

When your application does have work to do, it should use CPU time as effectively as it can. This means choosing algorithms that are appropriate for the amount of data you expect to handle. It also means using other system resources, such as an available vector unit (AltiVec or SSE in OS X) or a graphics processor, to perform specialized operations, which leads to the following goal:

*Goal:**Move work out of the CPU whenever you can*.

For basic information about how to use CPU time effectively, see [Fundamental Optimization Tips](../BasicTips/BasicTips.html#//apple_ref/doc/uid/TP40001410-CH204-BBCIFICC). For tips specifically related to improving the speed of drawing operations, see [Drawing Code](../BasicTips/BasicTips.html#//apple_ref/doc/uid/TP40001410-CH204-BBCGCFAB). 

#### Memory  Space
Memory on modern computing hardware is typically composed of progressively slower (but larger) types of memory. The fastest memory available to the CPU is the CPU’s own registers. The next fastest is the L1 cache, followed by the L2 and L3 caches when they are available. The next fastest memory is the main memory. The slowest memory of all consists of virtual memory pages in OS X that reside on disk and must be paged in before they can be used.

In an ideal world, every application would be small enough to fit into the system’s fastest cache memory. Unfortunately, most of an application’s code and data resides either in main memory or on the disk. Therefore, it is important that the application’s code and data is organized in a way that minimizes the time spent in these slower media, which leads to the following goal:

*Goal:**Reduce the memory footprint of your program*.

Reducing the memory footprint of your program can significantly improve its performance. A small memory footprint usually has two advantages. First, the smaller your program, the fewer memory pages it occupies. Fewer memory pages, typically means less paging. Second, code is usually smaller as a result of being more heavily optimized and better organized. Thus, fewer instructions are needed to perform a given task and all of the code for that task is gathered on the same set of memory pages.

In addition to reducing your application’s memory footprint, you should also try to reduce the footprint of writable memory pages in your application. Writable memory pages store global or allocated data for your application. In OS X, such pages can be written to disk if needed, but doing so is slow. In iOS, the contents of these pages must be purged manually by the application itself, which might later require the application to recreate the data that was on those pages. In both cases, the system’s efforts to free up memory take time—time that could be better spent executing your application code.

For basic information about how to reduce the footprint of your program, see [Application Footprint](../BasicTips/BasicTips.html#//apple_ref/doc/uid/TP40001410-CH204-BBCJHDJH). For tips specifically related to using memory more efficiently, see [Memory Allocation Code](../BasicTips/BasicTips.html#//apple_ref/doc/uid/TP40001410-CH204-BBCFHEGA). 

#### Mass Storage Space
File-system performance on any computer is important because nearly everything resides in a file somewhere. Your applications, data, and even the operating system itself all reside in files that must be loaded into memory from a device that is incredibly slow compared to other parts of the system. File systems, whether they are local or network-based, are one of the biggest bottlenecks to performance. This leads to yet another goal:

*Goal:**Eliminate unnecessary file operations and delay others until the information is actually needed*. 

Removing this bottleneck, by eliminating or delaying your file operations, is important to improving the overall performance of your application. Tens of millions of CPU cycles can pass between the time you request data from a file and the time your program actually sees that data. If your program accesses a large number of files, it may wait many seconds before it receives all of the requested data.

Another important thing to remember is that your application and any files it creates may be on the network instead of on a local hard disk. OS X, in particular, makes the network as invisible as possible, so you should never make assumptions about the locality of files. 

For basic information about how to improve the file-based performance of your program, see [File Access Code](../BasicTips/BasicTips.html#//apple_ref/doc/uid/TP40001410-CH204-BBCEGGIH). 

### The Perception of Speed
Even if you tune your application code for optimal performance, it’s entirely possible for your application to appear slow to the user. The problem is unavoidable: if you have a lot of work to do, you need the CPU time and resources to do that work. There are things that you can do though to give your application the appearance of speed, which leads to the following goal:

*Goal:**Make your program responsive to the user*. 

Responsiveness is usually a more important factor to users than raw speed. As long as a program responds to commands in a timely manner, the user is often willing to accept the fact that some tasks take longer to perform. The perception of speed is achieved by letting the user continue to work while your program processes data in the background. Improving the number of concurrent tasks performed by your application is a good way to make it responsive to the user. Concurrency is typically implemented using Grand Central Dispatch or threads. While your application’s main thread responds to the user, dispatch queues or background threads perform calculations or handle other time-consuming tasks.

Another common way to make your application appear fast is to improve its launch time. An application that takes more than a second or two to launch is probably doing too much. Not only is it unresponsive to the user during that time but it may also be loading resources that are not needed right away or might not be used at all, which is wasteful.

For information about how to improve launch times, see [Launch Time Initialization Code](../BasicTips/BasicTips.html#//apple_ref/doc/uid/TP40001410-CH204-BBCCHDAF). For information about improving the perceived performance of your program, see [Take Advantage of Perceived Performance](../BasicTips/BasicTips.html#//apple_ref/doc/uid/TP40001410-CH204-BCIFDFAA). 

## Tracking Performance
The only way to ensure high performance is to include performance goals in your product design and measure your product against those goals throughout the development process. High performance is not a feature that you can graft onto your code at the end of the development cycle; it is intimately tied to that cycle. As code is written, it is important to know the impact it has on your program’s overall performance. If you detect performance problems early, you have a good chance to fix them before it is too late. 

The way to determine if you are meeting or exceeding a specific goal is to gather metrics. Apple provides several tools for monitoring and analyzing the performance of a program. You can also build measurement tools directly into your code to help automate the process of gathering data. Whichever approach you choose, you need to exercise those tools regularly and analyze the results. 

### Establish Your Baseline Metrics 
The first thing you need to do is decide on a set of baseline metrics you want to measure. Choose the tasks you think are most important to your users and identify a set of constraints for performing those tasks. For example, you might want your application to load and display it’s initial window in less than 1 second, or you might want to keep your total memory usage within a given target range.

The tasks you choose to measure should reflect the needs of your users. Your marketing department should be able to help you choose a set of tasks that users find relevant. If you have an established product, talk to you users and find out what features they consider slow and consider improving the performance of those features as part of your planned update. 

Once you have a list of tasks you want to track, you need to determine the performance targets for each task. For existing products, you might simply be trying to improve on the performance of the previous version. You might also try to measure the performance of competing products and set goals that meet or exceed their performance. If you have a new product, you might have to experiment with numbers to find reasonable values. Alternatively, you might want to establish aggressive baseline values and try to come as close to them as possible. 

As with any performance measurement, consistency is important. Your process for establishing baseline metrics should include information about the system on which you are gathering those metrics. Record the hardware and software configuration of your system in some detail and always run your tests against the same configuration. Try to use the slowest possible hardware configuration for establishing your baselines. Measurements on a fast machine might lead you to believe that your software performs well, but many users will be running computers with slower processors and less memory. 

### Measure Early, Measure Often
Performance data is not something you can gather once and hope to find all of the performance bottlenecks in your program. It’s easier to find problems if you maintain a history of your program’s performance. Maintaining a history makes it easy to see whether your application’s performance is improving or declining. If it’s declining, you can take action to correct the problem before your product ships. 

Another reason for measuring performance regularly is that you can correlate those results with code checkins. If performance at a particular milestone declines, you can review the code checked in during that period and try to find out why. Similarly, if performance improves, you can use the recent code checkins as a model for good programming practices and encourage your team to use similar techniques. 

You should start making performance measurements as soon as you have a partially functional program. As new features are added, you can add measurements for those features. Incorporating a set of automated diagnostic routines directly into your program makes it easier for the members of your team to see the results immediately. Having this information readily available makes it easier for them to fix performance problems before checking in their code. 

### Analyze Your Results
Gathering data is the most important step in identifying performance bottlenecks. But once you have the data, it’s also important that you use it to find problems. Analyzing performance data is not as simple as looking at the output and seeing the problem right away. You might get lucky and see the problem quickly, but some problems are subtler and require more careful analysis. 

One way to help analyze results is to plot them graphically. Visualizing performance data can help you see trends much more quickly than if they are in a spreadsheet or other text-based medium. For example, you could plot the time to complete a single operation against a particular build to determine if performance is improving or declining from build to build. Plotting multiple data sets against the same set of milestones might also reveal trends and provide insight as to why performance is increasing or decreasing.  

#### Analyze Higher-Level Algorithms
As you analyze performance data, keep an open mind towards the abstraction level at which the problem resides. Suppose the data you have indicates that a lot of time is spent inside a particular function. It may be that the code in the function itself can be optimized so that it performs faster, but is that the real cause of the problem? Run your program again but this time check the number of times that function is called. If the function is called one million times, the problem might be in the higher-level algorithm that is calling it in the first place. If the function is called once, the body of the function is likely the problem. 

> **Note:** Instruments is a powerful tool for analyzing your program’s runtime behavior. Instruments lets you record multiple metrics and display them side by side, making it much easier to spot trends. Instrument’s data mining capabilities are another excellent way to identify problems in higher-level algorithms quickly. For more information about Instruments and other Apple-provided tools, see [Performance Tools](../PerformanceTools/PerformanceTools.html#//apple_ref/doc/uid/TP40001410-CH205-BCIIHAAJ). 

The performance tools themselves have limitations that you need to understand and take into account when analyzing data. For example, sampling programs may point out places where your application is spending a lot of time, but you should understand how those tools gather their data before drawing too many conclusions. Sampling tools do not track every function call. Instead, they offer a statistical analysis of your program based on samples taken at fixed intervals. Use the output from these tools as a guide, but be sure to correlate it with other data you record.

#### Other Analysis Techniques
If you are ever in doubt as to the true cause of a performance problem, avoid making assumptions about the cause of the problem. Instead, refine your analysis by focusing your data gathering efforts on the relevant code. Try using different tools to gather new types of information. A different tool might provide a unique perspective that reveals more about the actual problem. 

Some additional ways you can analyze your program include the following:

Watch the code in the debugger. Walking through code in the debugger might reveal logic errors that are slowing the code down. 

Add checkpoints to the code to log information about when that code was executed. For an example of using checkpoints to track initialization code, see the *[Launch Time Performance Guidelines](../../LaunchTime/LaunchTime.html#//apple_ref/doc/uid/10000148i)*.

Try coding alternate solutions to the problem and see if they run into similar problems. 

        
            [Next](../BasicTips/BasicTips.html)[Previous](../Introduction/Introduction.html)
        
         Copyright &#x00a9; 2004, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-10-22