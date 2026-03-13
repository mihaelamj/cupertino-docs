---
title: "Tuning for Performance and Responsiveness"
book: "Mac App Programming Guide"
chapterId: "TP40010543-CH9"
date: "2015-03-09"
description: "Introduces the development process for Mac apps."
identifier: "//apple_ref/doc/uid/TP40010543"
source: apple-archive
---

# Tuning for Performance and Responsiveness

> **Mac App Programming Guide**
> Last updated: 2015-03-09

[Next](../RevisionHistory.html)[Previous](../BuildTimeConfiguration/BuildTimeConfiguration.html)
        
        
        [](../index.html)
        
        # Tuning for Performance and Responsiveness
As you develop your app and your project code stabilizes, you can begin performance tuning. Of course, you want your app to launch and respond to the user’s commands as quickly as possible. A responsive app fits easily into the user’s workflow and feels well crafted.

## Speed Up Your App’s Launch Time
You can improve your app’s performance at launch time by minimizing or deferring work until after the launch sequence has completed. The launch of an app provides users with the first impression of your app, and it’s something they see on a regular basis.

Your overriding goal during launch should be to display the app’s menu bar and main window and then start responding to user commands as quickly as possible. Making your app responsive to commands quickly provides a better experience for the user. The following sections provide some general tips on how to make your app launch faster.

### Delay Initialization Code
Many apps spend a lot of time initializing code that isn’t used until much later. Delaying the initialization of subsystems that are not immediately needed can speed up your launch time considerably. Remember that the goal is to display your app interface quickly, so try to initialize only the subsystems related to that goal initially. 

Once you have posted your interface, your app can continue to initialize additional subsystems as needed. However, remember that just because your app is able to process commands does not mean you need all of that code right away. The preferred way of initializing subsystems is on an as-needed basis. Wait until the user executes a command that requires a particular subsystem and then initialize it. That way, if the user never executes the command, you will not have wasted any time running the code to prepare for it. 

Avoid putting a lot of extraneous initialization code in your `[awakeFromNib](https://developer.apple.com/documentation/objectivec/nsobject/1402907-awakefromnib)` methods. The system calls the `awakeFromNib` method of your main nib file before your app enters its main event loop. Use that method to initialize the objects in that nib and to prepare your app interface. For all other initialization, use the `[applicationDidFinishLaunching:](https://developer.apple.com/documentation/appkit/nsapplicationdelegate/1428385-applicationdidfinishlaunching)` method of your `NSApplicationDelegate` object instead. For more information on nib files and how they are loaded, see *[Resource Programming Guide](../../../../Cocoa/Conceptual/LoadingResources/Introduction/Introduction.html#//apple_ref/doc/uid/10000051i)*.

### Simplify Your Main Nib File
Loading a nib file is an expensive process that can slow down your app launch time if you are not careful. When a nib file is loaded, all of the objects in that file are instantiated and made ready for use. The more objects you include in your app’s main nib, the more time it takes to load that file and launch your app.

The instantiation process for objects in a nib file requires that any frameworks used by those objects must themselves reside in memory. Thus loading a nib for a Cocoa app would likely require the loading of both the AppKit and Foundation frameworks, if they were not already resident in memory. Similarly, if you declare a custom class in your main nib file and that class relies on other frameworks, the system must load those frameworks as well. 

When designing your app’s main nib file, you should include only those objects needed to display your app’s initial user interface. Usually, this would involve just your app’s menu bar and initial window. For any custom classes you include in the nib, make sure their initialization code is as minimal as possible. Defer any time-consuming operations or memory allocations until after the class is instantiated. 

### Minimize Global Variables
For both apps and frameworks, be careful not to declare global variables that require significant amounts of initialization. The system initializes global variables before calling your app’s `main` routine. If you use a global variable to declare an object, the system must call the constructor or initialization method for that object during launch time. In general, it’s best to avoid declaring objects as global variables altogether when you can use a pointer instead. 

If you are implementing a framework or any type of reusable code module, you should also minimize the number of global variables you declare. Each app that links to a framework acquires a copy of that framework’s global variables. These variables might require several pages of virtual memory, which then increases the memory footprint of the app. An increased memory footprint can lead to paging in the app, which has a tremendous impact on performance. 

One way to minimize the global variables in a framework is to store the variables in a `malloc`-allocated block of memory instead. In this technique, you access the variables through a pointer to the memory, which you store as a global variable. Another advantage of this technique is that it allows you to defer the creation of any global variables until the first time they are actually used. See [Tips for Allocating Memory](../../../../Performance/Conceptual/ManagingMemory/Articles/MemoryAlloc.html#//apple_ref/doc/uid/20001881) in *[Memory Usage Performance Guidelines](../../../../Performance/Conceptual/ManagingMemory/ManagingMemory.html#//apple_ref/doc/uid/10000160i)* for more information.

### Minimize File Access at Launch Time
Accessing a file is one of the slowest operations performed on a computer, so it is important that you do it as little as possible, especially at launch time. There is always some file access that must occur at launch time, such as loading your executable code and reading in your main nib file, but reducing your initial dependence on startup files can provide significant speed improvements. 

If you can delay the reading of a file until after launch time, do so. The following list includes some files whose contents you may not need until after launch:

Frameworks not used directly by your app—Avoid calling code that uses nonessential frameworks until after launch. 

Nib files whose contents are not displayed immediately—Make sure your nib files and `awakeFromNib:` code are not doing too much at launch time. See [Simplify Your Main Nib File](#//apple_ref/doc/uid/20001855-97354) for more information.

User preference files—User preferences may not be local so read them later if you can. 

Font files—Consider delaying font initialization until after the app has launched. 

Network files—Avoid reading files located on the network if at all possible. 

If you must read a file at launch time, do so only once. If you need multiple pieces of data from the same file, such as from a preferences file, consider reading all of the data once rather than accessing the file multiple times.

## Don’t Block the Main Thread
The main thread is where your app handles user events and other input, so you should keep it free as much as possible to be responsive to the user. In particular, never use the main thread to perform long-running or potentially unbounded tasks, such as tasks that require network access. Instead, always move those tasks onto background threads. The preferred way to do so is to use Grand Central Dispatch (GCD) or operation objects to perform tasks asynchronously.

For more information about doing work on background threads, see *[Concurrency Programming Guide](../../ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)*.

## Decrease Your App’s Code Size
In the context of performance, the more memory your app occupies, the more inefficient it is. More memory means more memory allocations, more code, and a greater potential for paging.

Reducing your code footprint is not just a matter of turning on code optimizations in your compiler, although that does help. You can also reduce your code footprint by organizing your code so that only the minimum set of required functions is in memory at any given time. You implement this optimization by profiling your code. 

See “Memory Instruments” in *[Instruments User Guide](../../../../DeveloperTools/Conceptual/InstrumentsUserGuide/index.html#//apple_ref/doc/uid/TP40004652)* for information about profiling your app’s memory allocations.

### Compiler-Level Optimizations
The Xcode compiler supports optimization options that let you choose whether you prefer a smaller binary size, faster code, or faster build times. For new projects, Xcode automatically disables optimizations for the debug build configuration and selects the Fastest, Smallest option for the release build configuration. Code optimizations of any kind result in slower build times because of the extra work involved in the optimization process. If your code is changing, as it does during the development cycle, you do not want optimizations enabled. As you near the end of your development cycle, though, the release build configuration can give you an indication of the size of your finished product, so the Fastest, Smallest option is appropriate.

Table 6-1 lists the optimization levels available in Xcode. When you select one of these options, Xcode passes the appropriate flags to the compiler for the given group or files. These options are available at the target level or as part of a build configuration. See the *[Xcode Build System Guide](../../../../DeveloperTools/Conceptual/XcodeBuildSystem/000-Introduction/Introduction.html#//apple_ref/doc/uid/TP40006904)* for information on working with build settings for your project.

**Table 6-1**  Compiler optimization optionsXcode setting

Description

None

The compiler does not attempt to optimize code. Use this option during development when you are focused on solving logic errors and need a fast compile time. Do not use this option for shipping your executable. 

Fast

The compiler performs simple optimizations to boost code performance while minimizing the impact to compile time. This option also uses more memory during compilation. 

Faster

The compiler performs nearly all supported optimizations that do not require a space-time tradeoff. The compiler does not perform loop unrolling or function inlining with this option. This option increases both compilation time and the performance of generated code. 

Fastest

The compiler performs all optimizations in an attempt to improve the speed of the generated code. This option can increase the size of generated code as the compiler performs aggressive inlining of functions. 

This option is generally not recommended.

Fastest, Smallest

The compiler performs all optimizations that do not typically increase code size. This is the preferred option for shipping code because it gives your executable a smaller memory footprint. 

As with any performance enhancement, do not make assumptions about which option will give you the best results. You should always measure the results of each optimization you try. For example, the Fastest option might generate extremely fast code for a particular module, but it usually does so at the expense of executable size. Any speed advantages you gain from the code generation are easily lost if the code needs to be paged in from disk at runtime. 

### Use Core Data for Large Data Sets
If your app manipulates large amounts of structured data, store it in a Core Data persistent store or in a SQLite database instead of in a flat file. Both Core Data and SQLite provide efficient ways to manage large data sets without requiring the entire set to be in memory all at once. Use SQLite if you deal with low-level data structures, or an existing SQLite database. Core Data provides a high-level abstraction for efficient object-graph management with an Objective-C interface; it is, however, an advanced framework and you shouldn't use it until you have gained adequate experience.

For more information about Core Data, see *[Core Data Programming Guide](../../../../Cocoa/Conceptual/CoreData/index.html#//apple_ref/doc/uid/TP40001075)* and *Optimizing Core Data with Instruments*.

### Eliminate Memory Leaks
Your app should not have any memory leaks. You can use the Instruments app to track down leaks in your code, both in the simulator and on actual devices. See “Memory Instruments” in *[Instruments User Guide](../../../../DeveloperTools/Conceptual/InstrumentsUserGuide/index.html#//apple_ref/doc/uid/TP40004652)* for information about finding memory leaks.

### Dead Strip Your Code
For statically linked executables, dead-code stripping is the process of removing unreferenced code from the executable file. If the code is unreferenced, it must not be used and therefore is not needed in the executable file. Removing dead code reduces the size of your executable and can help reduce paging. 

To enable dead-code stripping in Xcode, in the Linking group of Build Settings, set the Dead Code Stripping option to Yes.

### Strip Symbol Information
Debugging symbols and dynamic-binding information can take up a lot of space and comprise a large percentage of your executable’s size. Before shipping your code, you should strip out all unneeded symbols.

To strip debugging symbols from your executable, change the Xcode compiler code generation Generate Debug Symbols option to No. You can also generate debugging symbols on a target-by-target basis if you prefer. See the Xcode Help for more information on build configurations and target settings.

        
            [Next](../RevisionHistory.html)[Previous](../BuildTimeConfiguration/BuildTimeConfiguration.html)
        
         Copyright &#x00a9; 2015 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2015-03-09