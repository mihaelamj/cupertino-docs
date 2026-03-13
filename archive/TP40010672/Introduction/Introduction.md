---
title: "Introduction"
book: "File System Programming Guide"
chapterId: "TP40010672-CH1"
date: "2018-04-09"
description: "Explains how to create and manage files and directories. "
identifier: "//apple_ref/doc/uid/TP40010672"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Introduction

> **File System Programming Guide**
> Last updated: 2018-04-09

[Next](../FileSystemOverview/FileSystemOverview.html)
        
        
        [](../index.html)
        
        # About Files and Directories
The file system is an important part of any operating system. After all, it’s where users keep their stuff. The organization of the file system plays an important role in helping the user find files. The organization also makes it easier for apps and the system itself to find and access the resources they need to support the user.

This document is intended for developers who are writing software for macOS, iOS, and iCloud. It shows you how to use the system interfaces to access files and directories and how to move files to and from iCloud. This document also provides guidance on how best to work with files, and it shows you where you should be placing any new files you create.

> **Important:** When you adopt App Sandbox in your macOS app, the behavior of many file system features changes. For example, to obtain access to locations outside of your app’s container directory, you must request appropriate entitlements. To obtain persistent access to a file system resource, you must employ the security-scoped bookmark features of the `NSURL` class or the `CFURLRef` opaque type. There are changes to the location of app support files (which are relative to your container rather than to the user’s home folder) and to the behavior of Open and Save dialogs (which are presented by the macOS security technology, Powerbox, not AppKit). For details on all these changes, read *[App Sandbox Design Guide](../../../../Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html#//apple_ref/doc/uid/TP40011183)*.

## At a Glance
To effectively use the file system, you must know what to expect from the file system and which technologies are available for accessing it.

### The File System Imposes a Specific Organization
The file systems in iOS and macOS are structured to help keep files organized, both for the user and for apps. From the perspective of your code, a well-organized file system makes it easier to locate files your app needs. Of course, you also need to know where you should put any files you create.  

> **Relevant chapters and appendixes:** [File System Basics](../FileSystemOverview/FileSystemOverview.html#//apple_ref/doc/uid/TP40010672-CH2-SW2), [macOS Library Directory Details](../MacOSXDirectories/MacOSXDirectories.html#//apple_ref/doc/uid/TP40010672-CH10-SW1), [File System Details](../FileSystemDetails/FileSystemDetails.html#//apple_ref/doc/uid/TP40010672-CH8-SW1)

### Access Files Safely
On a multiuser system like macOS, it is possible that more than one app may attempt to read or write a file at the same time as another app is using it. The `NSFileCoordinator` and `NSFilePresenter` classes allow you to maintain file integrity and ensure that if files are made available to other apps (for example, emailing the current TextEdit document) that the most current version is sent.

> **Relevant Chapter:** [The Role of File Coordinators and Presenters](../FileCoordinators/FileCoordinators.html#//apple_ref/doc/uid/TP40010672-CH11-SW1)

### How You Access a File Depends on the File Type
Different files require different treatments by your code. For file formats defined by your app, you might read the contents as a binary stream of bytes. For more common file formats, though, iOS and macOS provide higher-level support that make reading and writing those files easier. 

> **Relevant section:** [Choose the Right Way to Access Files](../AccessingFilesandDirectories/AccessingFilesandDirectories.html#//apple_ref/doc/uid/TP40010672-CH3-SW5)

### System Interfaces Help You Locate and Manage Your Files
Hard-coded pathnames are fragile and liable to break over time, so the system provides interfaces that search for files in well-known locations. Using these interfaces makes your code more robust and future proof, ensuring that regardless of where files move to, you can still find them. 

> **Relevant chapter:** [Accessing Files and Directories](../AccessingFilesandDirectories/AccessingFilesandDirectories.html#//apple_ref/doc/uid/TP40010672-CH3-SW1)

### Users Interact with Files Using the Standard System Panels
For files that the user creates and manages, your code can use the standard Open and Save panels to ask for the locations of those files. The standard panels present the user with a navigable version of the file system that matches what the Finder presents. You can use these panels without customization, modify the default behavior, or augment them with your own custom content. Even sandboxed apps can use the panels to access files because they work with the underlying security layer to allow exceptions for files outside of your sandbox that the user explicitly chooses.

> **Relevant chapter:** [Using the Open and Save Panels](../UsingtheOpenandSavePanels/UsingtheOpenandSavePanels.html#//apple_ref/doc/uid/TP40010672-CH4-SW1)

### Read and Write Files Asynchronously
Because file operations require accessing a disk or network server, you should always access files from a secondary thread of your app. Some of the technologies for reading and writing files run asynchronously without you having to do anything, while others require you to provide your own execution context. All of the technologies do essentially the same thing but offer different levels of simplicity and flexibility. 

As you read and write files, you should also use file coordinators to ensure that any actions you take on a file do not cause problems for other apps that care about the file.

> **Relevant chapter:** [Techniques for Reading and Writing Files Without File Coordinators](../TechniquesforReadingandWritingCustomFiles/TechniquesforReadingandWritingCustomFiles.html#//apple_ref/doc/uid/TP40010672-CH5-SW1)

### Move, Copy, Delete, and Manage Files Like the Finder 
The system interfaces support all of the same types of behaviors that the Finder supports and many more. You can move, copy, create, and delete files and directories just like the user can. Using the programmatic interfaces, you can also iterate through the contents of directories and work with invisible files much more efficiently. More importantly, most of the work can be done asynchronously to avoid blocking your app’s main thread. 

> **Relevant chapter:** [Managing Files and Directories](../ManagingFIlesandDirectories/ManagingFIlesandDirectories.html#//apple_ref/doc/uid/TP40010672-CH6-SW1)

### Optimize Your File-Related Operations
The file system is one of the slowest parts of a computer so writing efficient code is important. Optimizing your file-system code is about minimizing the work you do and making sure that the operations you do perform are done efficiently. 

> **Relevant chapter:** [Performance Tips](../PerformanceTips/PerformanceTips.html#//apple_ref/doc/uid/TP40010672-CH7-SW1)

## See Also
For information about more advanced file-system tasks that you can perform, see *[File System Advanced Programming Topics](../../FileSystemAdvancedPT/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010765)*.  

        
            [Next](../FileSystemOverview/FileSystemOverview.html)
        
         Copyright &#x00a9; 2018 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2018-04-09