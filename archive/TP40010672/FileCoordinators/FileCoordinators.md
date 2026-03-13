---
title: "The Role of File Coordinators and Presenters"
book: "File System Programming Guide"
chapterId: "TP40010672-CH11"
date: "2018-04-09"
description: "Explains how to create and manage files and directories. "
identifier: "//apple_ref/doc/uid/TP40010672"
source: apple-archive
---

# The Role of File Coordinators and Presenters

> **File System Programming Guide**
> Last updated: 2018-04-09

[Next](../iCloud/iCloud.html)[Previous](../AccessingFilesandDirectories/AccessingFilesandDirectories.html)
        
        
        [](../index.html)
        
        # The Role of File Coordinators and Presenters
Because the file system is shared by all running processes, problems can occur when two processes (or two threads in the same process) try to act on the same file at the same time. Two different processes acting on the same file might make changes that the other is not expecting, which could lead to more serious problems like crashes or data corruption. And even within a single program, two threads acting on the file simultaneously can corrupt it and render it unusable. To avoid this type of file contention, macOS 10.7 and later includes support for *file coordinators*, which allow you to coordinate file access safely between different processes or different threads.

The job of a file coordinator is to notify interested parties whenever a file they care about is acted on by another process or thread. The interested parties in this case are the objects of your app. Any object of your app can designate itself as a *file presenter*—that is, an object that works directly with a file and needs to be notified when actions are initiated on that file by other objects. Any object in your app can be a file presenter and a file presenter can monitor individual files or whole directories of files. For example, an app that works with images would designate one or more objects as file presenters for the corresponding image files. If the user deleted an image file from the Finder while the app was running, the file presenter for that image file would be notified and given an opportunity to accommodate the deletion of the file.

## Using NSDocument and UIDocument to Handle File Coordinators and Presenters
The `[UIDocument](https://developer.apple.com/documentation/uikit/uidocument)` class in iOS and the `[NSDocument](https://developer.apple.com/documentation/appkit/nsdocument)` class in macOS encapsulate the data for a user document. These classes automatically support:

The `[NSFileCoordinator](https://developer.apple.com/documentation/foundation/nsfilecoordinator)` implementation

The `[NSFilePresenter](https://developer.apple.com/documentation/foundation/nsfilepresenter)` implementation

The Autosave implementation

Whenever possible explore using these classes to store your user data. Your app need only work directly with file coordinators and file presenters when dealing with files other than the user’s data files.

Apps that use the document classes don’t need to use file coordinators when the app reads and writes private files in the `Application Support`, `Cache`, or temporary directories. These files are considered private.

## Ensuring Safe Read and Write Operations Using File Coordinators
When your app wants to modify a file, it creates a file coordinator object to notify any interested file presenters of its intentions. A file coordinator communicates with all of the relevant file presenters about your app’s intentions toward the file. After the file presenters have all responded, the file coordinator executes a block containing the actual actions you want to perform. All of these takes place asynchronously so that your app threads do not have to wait explicitly. 

The steps for adding support for file coordinators to your code are as follows:

For each object that manages a file or directory, make the corresponding class adopt the `[NSFilePresenter](https://developer.apple.com/documentation/foundation/nsfilepresenter)` protocol. The system uses the methods of this protocol to notify your object when changes occur to the monitored file.

When you initialize your file presenter object at runtime, register it immediately by calling the `[addFilePresenter:](https://developer.apple.com/documentation/foundation/nsfilecoordinator/1417120-addfilepresenter)` class method of `NSFileCoordinator`.

When your file presenter object performs its own actions on a file or directory, create an `NSFileCoordinator` object and call the method that corresponds to the action you plan to take.

Before you release your file presenter object, call the `[removeFilePresenter:](https://developer.apple.com/documentation/foundation/nsfilecoordinator/1414924-removefilepresenter)` class method of `NSFileCoordinator` to unregister it as a file presenter. Because `NSFileCoordinator` retains your file presenter object, the system does not deallocate it until you unregister it.

### Choosing Which Files to Monitor with File Presenters
Not every file touched by your app needs to be monitored. Apps use file presenters when monitoring the following types of items:

User documents

Directories containing media files (music, photos, movies, and so on) that your app manages

Files located on remote servers

You generally do not need to monitor the files in your app bundle or any files that are private to your app and stored in app-specific subdirectories of the `~/Library` directory. If your app runs in a sandbox, you also do not need to monitor any files you create inside your sandbox directory.

### Implementing Your File Presenter Objects
Most of your app’s file presenter objects are going to be either controller objects or data model objects. Controller objects are a natural choice because they generally coordinate the creation and modification of other objects, including data model objects. However, if your app monitors hundreds or thousands of files, you might want to move the support for managing individual files down into your data model objects.

To implement a file presenter object, make the class for that object conform to the `[NSFilePresenter](https://developer.apple.com/documentation/foundation/nsfilepresenter)`[ protocol](../../../../General/Conceptual/DevPedia-CocoaCore/Protocol.html#//apple_ref/doc/uid/TP40008195-CH45). The methods in this protocol indicate the types of operations that can be performed on the file or directory. Your implementation of the various methods is where your object responds and takes any necessary actions. You do not have to implement every method of the protocol but do implement all methods that might affect how your object interacts with the file or directory. You use the properties and methods of the protocol to do the following:

Provide the URL of the file or directory being monitored. (All file presenters do this by implementing the required `[presentedItemURL](https://developer.apple.com/documentation/foundation/nsfilepresenter/1414861-presenteditemurl)` property.) 

Relinquish control of the file or directory temporarily so that another object can write to it or read from it.

Save any unsaved changes before another object tries to read from the file.

Track changes to the contents or attributes of a file or directory. 

Update your data structures when the file or directory is moved or deleted.

Provide a dispatch queue on which to execute your file presenter methods.

If you are implementing a document-based app, you do not need to incorporate file presenter semantics into your `[NSDocument](https://developer.apple.com/documentation/appkit/nsdocument)` subclasses. The `NSDocument` class already conforms to the `[NSFilePresenter](https://developer.apple.com/documentation/foundation/nsfilepresenter)` protocol and implements the appropriate methods. Thus, all of your documents automatically register themselves as presenters of their corresponding file and do things like save changes and track changes to the document.

All of your file presenter methods should be lightweight and execute quickly. In cases where your file presenter is responding to changes (such as in the `[presentedItemDidChange](https://developer.apple.com/documentation/foundation/nsfilepresenter/1416103-presenteditemdidchange)`,  `[presentedItemDidMoveToURL:](https://developer.apple.com/documentation/foundation/nsfilepresenter/1417861-presenteditemdidmove)`, or `[accommodatePresentedItemDeletionWithCompletionHandler:](https://developer.apple.com/documentation/foundation/nsfilepresenter/1414732-accommodatepresenteditemdeletion)` methods), you might want to avoid incorporating changes directly from your file presenter method. Instead, dispatch a block asynchronously to a dispatch queue and process the changes at a later time. This lets you process the changes at your app’s convenience without causing unnecessary delays to the file coordinator that initiated the change. Of course, when saving or relinquishing control of a file (such as in the `[relinquishPresentedItemToReader:](https://developer.apple.com/documentation/foundation/nsfilepresenter/1410743-relinquishpresenteditemtoreader)`, `[relinquishPresentedItemToWriter:](https://developer.apple.com/documentation/foundation/nsfilepresenter/1413688-relinquishpresenteditemtowriter)`, or `[savePresentedItemChangesWithCompletionHandler:](https://developer.apple.com/documentation/foundation/nsfilepresenter/1414407-savepresenteditemchangeswithcomp)` methods) you perform all necessary actions immediately and not defer them.

For details about how to implement the methods of the `NSFilePresenter` protocol, see *[NSFilePresenter Protocol Reference](https://developer.apple.com/documentation/foundation/nsfilepresenter)*. 

### Registering File Presenters with the System
After your app creates a file presenter object, register it with the system using the `[addFilePresenter:](https://developer.apple.com/documentation/foundation/nsfilecoordinator/1417120-addfilepresenter)` class method of `NSFileCoordinator`:

MyFilePresenter *filePresenter = [[MyFilePresenter alloc] init];
```
[NSFileCoordinator addFilePresenter:filePresenter];
```
Registering an object tells the system that it is interested in a particular file and wants to begin receiving notifications about actions on it. Your app may register the file presenter after it is initialized, as shown in the example above, or as part of the file presenter’s initialization method.

Before your app releases a file presenter object, unregister it using the `[removeFilePresenter:](https://developer.apple.com/documentation/foundation/nsfilecoordinator/1414924-removefilepresenter)` method of `NSFileCoordinator`:

[NSFileCoordinator removeFilePresenter:filePresenter];While you may register a file presenter in its initialization method, you do not unregister a file presenter in its deallocation method. Because `NSFileCoordinator` retains registered file presenter objects, the system does not deallocate the file presenter until you unregister it, even when your own code no longer retains it. Therefore, releasing a file presenter object without first unregistering it is a programming error.

### Initiating File Changes with a File Coordinator
Before your file presenter object makes any changes to its monitored file, it must create an `[NSFileCoordinator](https://developer.apple.com/documentation/foundation/nsfilecoordinator)` object and indicate the type of change it is about to make. File coordinators are the mechanism by which file presenters system-wide are notified of changes. This object works with the system to notify other threads and processes and give the file presenters in those threads and processes a chance to respond. After all of the interested file presenters have responded, the file coordinator executes the block of code you provide to perform the actual actions. To use a file coordinator object, do the following:

Create an instance of the `[NSFileCoordinator](https://developer.apple.com/documentation/foundation/nsfilecoordinator)` class.

Call the instance method that corresponds to the action you want to take on the file or directory:

Call the `[coordinateReadingItemAtURL:options:error:byAccessor:](https://developer.apple.com/documentation/foundation/nsfilecoordinator/1407416-coordinate)` method to read the contents of a file or directory.

Call the `[coordinateWritingItemAtURL:options:error:byAccessor:](https://developer.apple.com/documentation/foundation/nsfilecoordinator/1413344-coordinate)` method to write to a file or to change the contents of a directory.

Call the `[coordinateReadingItemAtURL:options:writingItemAtURL:options:error:byAccessor:](https://developer.apple.com/documentation/foundation/nsfilecoordinator/1413385-coordinatereadingitematurl)` method to read and write from a file or directory.

Call the `[itemAtURL:didMoveToURL:](https://developer.apple.com/documentation/foundation/nsfilecoordinator/1410328-item)` method to move a file or directory to a new location.

When you call these methods, you provide a block to do the actual reading or writing of the file or directory. If you need to read or write multiple items in a batch, call the `[prepareForReadingItemsAtURLs:options:writingItemsAtURLs:options:error:byAccessor:](https://developer.apple.com/documentation/foundation/nsfilecoordinator/1412420-prepareforreadingitemsaturls)` method instead and use the block passed to that method to coordinate the reading and writing of individual files. Using that method to process batches of files is much more efficient than trying to read or write files individually. 

Release the file coordinator object as soon as you are done. 

For more information about how to use the methods of `NSFileCoordinator`, see *[NSFileCoordinator Class Reference](https://developer.apple.com/documentation/foundation/nsfilecoordinator)*. 

        
            [Next](../iCloud/iCloud.html)[Previous](../AccessingFilesandDirectories/AccessingFilesandDirectories.html)
        
         Copyright &#x00a9; 2018 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2018-04-09