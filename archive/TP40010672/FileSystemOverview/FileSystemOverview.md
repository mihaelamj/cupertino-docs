---
title: "File System Basics"
book: "File System Programming Guide"
chapterId: "TP40010672-CH2"
date: "2018-04-09"
description: "Explains how to create and manage files and directories. "
identifier: "//apple_ref/doc/uid/TP40010672"
source: apple-archive
---

# File System Basics

> **File System Programming Guide**
> Last updated: 2018-04-09

[Next](../AccessingFilesandDirectories/AccessingFilesandDirectories.html)[Previous](../Introduction/Introduction.html)
        
        
        [](../index.html)
        
        # File System Basics
A *file system* handles the persistent storage of data files, apps, and the files associated with the operating system itself. Therefore, the file system is one of the fundamental resources used by all processes.

APFS is the default file system in macOS, iOS, watchOS, and tvOS. APFS replaces HFS+ as the default file system for iOS 10.3 and later, and macOS High Sierra and later. macOS additionally supports a variety of other formats, as described in [Supported File Systems](../FileSystemDetails/FileSystemDetails.html#//apple_ref/doc/uid/TP40010672-CH8-97329).

Regardless of the underlying format, all of the disks attached to the device—whether they are physically plugged in or are connected indirectly through the network—contribute space to create a single collection of files. Because the number of files can easily be many millions, the file system uses directories to create a hierarchical organization. Although the basic directory structures are similar for iOS and macOS, there are differences in the way each system organizes apps and user data.

Before you begin writing code that interacts with the file system, you should first understand a little about the organization of file system and the rules that apply to your code. Aside from the basic tenet that you cannot write files to directories for which you do not have appropriate security privileges, apps are also expected to be good citizens and put files in appropriate places. Precisely where you put files depends on the platform, but the overarching goal is to make sure that the user’s files remain easily discoverable and that the files your code uses internally are kept out of the user’s way. 

## About the iOS File System
The iOS file system is geared toward apps running on their own. To keep the system simple, users of iOS devices do not have direct access to the file system and apps are expected to follow this convention. 

### iOS Standard Directories: Where Files Reside
For security purposes, an iOS app’s interactions with the file system are limited to the directories inside the app’s sandbox directory. During installation of a new app, the installer creates a number of container directories for the app inside the sandbox directory. Each container directory has a specific role. The bundle container directory holds the app’s bundle, whereas the data container directory holds data for both the app and the user. The data container directory is further divided into a number of subdirectories that the app can use to sort and organize its data. The app may also request access to additional container directories—for example, the iCloud container—at runtime. 

These container directories constitute the app’s primary view of the file system. Figure 1-1 shows a representation of the sandbox directory for an app.

**Figure 1-1**  An iOS app operating within its own sandbox directoryAn app is generally prohibited from accessing or creating files outside its container directories. One exception to this rule is when an app uses public system interfaces to access things such as the user’s contacts or music. In those cases, the system frameworks use helper apps to handle any file-related operations needed to read from or modify the appropriate data stores.

Table 1-1 lists some of the more important subdirectories inside the sandbox directory and describes their intended usage. This table also describes any additional access restrictions for each subdirectory and points out whether the directory’s contents are backed up by iTunes and iCloud. 

**Table 1-1**  Commonly used directories of an iOS app Directory

Description

*AppName*`.app`

This is the app’s [bundle](../../../../General/Conceptual/DevPedia-CocoaCore/Bundle.html#//apple_ref/doc/uid/TP40008195-CH4). This directory contains the app and all of its resources. 

You cannot write to this directory. To prevent tampering, the bundle directory is signed at installation time. Writing to this directory changes the signature and prevents your app from launching. You can, however, gain read-only access to any resources stored in the apps bundle. For more information, see the *[Resource Programming Guide](../../../../Cocoa/Conceptual/LoadingResources/Introduction/Introduction.html#//apple_ref/doc/uid/10000051i)*

The contents of this directory are not backed up by iTunes or iCloud. However, iTunes does perform an initial sync of any apps purchased from the App Store.

`Documents/`

Use this directory to store user-generated content. The contents of this directory can be made available to the user through file sharing; therefore, his directory should only contain files that you may wish to expose to the user. 

The contents of this directory are backed up by iTunes and iCloud.

`Documents/Inbox`

Use this directory to access files that your app was asked to open by outside entities. Specifically, the Mail program places email attachments associated with your app in this directory. Document interaction controllers may also place files in it. 

Your app can read and delete files in this directory but cannot create new files or write to existing files. If the user tries to edit a file in this directory, your app must silently move it out of the directory before making any changes.

The contents of this directory are backed up by iTunes and iCloud.

`Library/`

This is the top-level directory for any files that are not user data files. You typically put files in one of several standard subdirectories. iOS apps commonly use the `Application Support` and `Caches` subdirectories; however, you can create custom subdirectories.

 Use the `Library` subdirectories for any files you don’t want exposed to the user. Your app should not use these directories for user data files. 

The contents of the `Library` directory (with the exception of the `Caches` subdirectory) are backed up by iTunes and iCloud.

For additional information about the Library directory and its commonly used subdirectories, see [The Library Directory Stores App-Specific Files](#//apple_ref/doc/uid/TP40010672-CH2-SW1).  

`tmp/`

Use this directory to write temporary files that do not need to persist between launches of your app. Your app should remove files from this directory when they are no longer needed; however, the system may  purge this directory when your app is not running. 

The contents of this directory are not backed up by iTunes or iCloud.

An iOS app may create additional directories in the `Documents`, `Library`, and `tmp` directories. You might do this to better organize the files in those locations.

For information about how to get references to the preceding directories from your iOS app, see [Locating Items in the Standard Directories](../AccessingFilesandDirectories/AccessingFilesandDirectories.html#//apple_ref/doc/uid/TP40010672-CH3-SW3). For tips on where to put files, see [Where You Should Put Your App’s Files](#//apple_ref/doc/uid/TP40010672-CH2-SW28). 

### Where You Should Put Your App’s Files
To prevent the syncing and backup processes on iOS devices from taking a long time, be selective about where you place files. Apps that store large files can slow down the process of backing up to iTunes or iCloud. These apps can also consume a large amount of a user's available storage, which may encourage the user to delete the app or disable backup of that app's data to iCloud. With this in mind, you should store app data according to the following guidelines:

Put user data in `Documents/`. User data generally includes any files you might want to expose to the user—anything you might want the user to create, import, delete or edit. For a drawing app, user data includes any graphic files the user might create. For a text editor, it includes the text files. Video and audio apps may even include files that the user has downloaded to watch or listen to later.

Put app-created support files in the `Library/Application support/` directory. In general, this directory includes files that the app uses to run but that should remain hidden from the user. This directory can also include data files, configuration files, templates and modified versions of resources loaded from the app bundle.

Remember that files in `Documents/` and `Application Support/` are backed up by default. You can exclude files from the backup by calling` -[NSURL setResourceValue:forKey:error:]` using the `NSURLIsExcludedFromBackupKey` key. Any file that can be re-created or downloaded must be excluded from the backup. This is particularly important for large media files. If your application downloads video or audio files, make sure they are not included in the backup.

Put temporary data in the `tmp/` directory. Temporary data comprises any data that you do not need to persist for an extended period of time. Remember to delete those files when you are done with them so that they do not continue to consume space on the user’s device. The system will periodically purge these files when your app is not running; therefore, you cannot rely on these files persisting after your app terminates.

Put data cache files in the `Library/Caches/` directory. Cache data can be used for any data that needs to persist longer than temporary data, but not as long as a support file. Generally speaking, the application does not require cache data to operate properly, but it can use cache data to improve performance. Examples of cache data include (but are not limited to) database cache files and transient, downloadable content. Note that the system may delete the `Caches/` directory to free up disk space, so your app must be able to re-create or download these files as needed.

## About the macOS File System
The macOS file system is designed for Mac computers, where both users and software have access to the file system. Users access the file system directly through the Finder, which presents a user-oriented view of the file system by hiding or renaming some files and directories. Apps access the file system using the system interfaces, which show the complete file system precisely as it appears on disk.

### Domains Determine the Placement of Files
In macOS, the file system is divided into multiple domains, which separate files and resources based on their intended usage. This separation provides simplicity for the user, who only needs to worry about a specific subset of files. Arranging files by domain also lets the system apply blanket access privileges to files in that domain, preventing unauthorized users from changing files intentionally or inadvertently.

The *user domain* contains resources specific to the users who log in to the system. Although it technically encompasses all users, this domain reflects only the home directory of the current user at runtime. User home directories can reside on the computer’s boot volume (in the `/Users` directory) or on a network volume. Each user (regardless of privileges) has access to and control over the files in his or her own home directory. 

The *local domain* contains resources such as apps that are local to the current computer and shared among all users of that computer. The local domain does not correspond to a single physical directory, but instead consists of several directories on the local boot (and root) volume. This domain is typically managed by the system, but users with administrative privileges may add, remove, or modify items in this domain.

The *network domain* contains resources such as apps and documents that are shared among all users of a local area network. Items in this domain are typically located on network file servers and are under the control of a network administrator. 

The *system domain* contains the system software installed by Apple. The resources in the system domain are required by the system to run. Users cannot add, remove, or alter items in this domain.

Figure 1-2 shows how the local, system, and user domains map to the local file system of a macOS installation. (The network domain is not shown but is similar in many ways to the local domain.) This figure shows the visible directories that the user might see. Depending on the user’s system, other directories may be visible or some of the ones shown here may be hidden. 

**Figure 1-2**  The local macOS file systemFor information about the contents of the directories in macOS, see [macOS Standard Directories: Where Files Reside](#//apple_ref/doc/uid/TP40010672-CH2-SW6). For information about the directories that macOS normally hides from the user (and why), see [Hidden Files and Directories: Simplifying the User Experience](#//apple_ref/doc/uid/TP40010672-CH2-SW7). 

### macOS Standard Directories: Where Files Reside
Whether provided by the system or created by your app, every file has its place in macOS. Table 1-2 lists some of the top-level directories in a macOS installation and the types of content that each one contains.

**Table 1-2**  Commonly used directories in macOSDirectory

Usage

`/Applications`

This directory is where you install apps intended for use by all users of a computer.  The App Store installs apps purchased by the user in this directory automatically.

The `Utilities` subdirectory contains a subset of apps that are intended for use in managing the local system.  

This directory is part of the local domain.

`Library`

There are multiple `Library` directories on the system, each one associated with a different domain or specific user. Apps should use the `Library` directory to store app-specific (or system-specific) resources. 

For detailed information about the contents of this directory and how you use it to support your apps, see [The Library Directory Stores App-Specific Files](#//apple_ref/doc/uid/TP40010672-CH2-SW1).

`/Network`

This directory contains the list of computers in the local area network. 

There is no guarantee that files located on network file servers will have the `/Network` directory at the beginning of their path. Path names vary depending on several factors, including how the network volume was mounted. For example, if the user uses the Connect to Server command to mount a volume, paths begin with the `/Volumes` directory. When writing code, assume that files on any volume other than the boot volume could be located on a network-based server.

`/System`

This directory contains the system resources required by macOS to run. These resources are provided by Apple and must not be modified.  

This directory comprises the contents of the system domain.

`/Users`

This directory contains one or more user home directories. The user home directory is where user-related files are stored. A typical user’s home directory includes the following subdirectories:

`Applications`—Contains user-specific apps.

`Desktop`—Contains the items on the user’s desktop.

`Documents`—Contains user documents and files. 

`Downloads`—Contains files downloaded from the Internet.

`Library`—Contains user-specific app files (hidden in macOS 10.7 and later).

`Movies`—Contains the user’s video files.

`Music`—Contains the user’s music files.

`Pictures`—Contains the user’s photos.

`Public`—Contains content the user wants to share.

`Sites`—Contains web pages used by the user’s personal site. (Web Sharing must be enabled to display these pages.)

The preceding directories are for storing user documents and media only. Apps must not write files to the preceding directories unless explicitly directed to do so by the user. The sole exception to this rule is the `Library` directory, which apps may use to store data files needed to support the current user. 

Of the subdirectories, only the `Public` directory is accessible by other users on the system. Access to the other directories is restricted by default.

> **Important:** The files in the user’s `Documents` and `Desktop` directories should reflect only the documents that the user created and works with directly. Similarly, the media directories should contain only the user’s media files. Those directories must never be used to store data files that your app creates and manages automatically. If you need a place to store automatically generated files, use the `Library` directory, which is designated specifically for that purpose. For information on where to put files in the Library directory, see [The Library Directory Stores App-Specific Files](#//apple_ref/doc/uid/TP40010672-CH2-SW1). 

Although the directories in [Table 1-2](#//apple_ref/doc/uid/TP40010672-CH2-SW9) are the ones seen by macOS users, they are not the only directories present in the file system. macOS hides many directories to prevent users from accessing files that they don’t need to.

### Sandboxed macOS App File Containers
macOS apps that are sandboxed have all their `Application Support`, `Cache`, temporary directories and other related documents stored within a directory located at a system-defined path that you can obtain by calling the `[NSHomeDirectory](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSHomeDirectory)` function.

For more information, see *[App Sandbox Design Guide](../../../../Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html#//apple_ref/doc/uid/TP40011183)*.

### Hidden Files and Directories: Simplifying the User Experience
To simplify the experience for users, the Finder, and some specific user-facing interfaces (such as the Open and Save panels), hide many files and directories that the user should never have to use. Many of the hidden items are system- or app-specific resources that users cannot (or should not) access directly. Among the files and directories that are hidden are the following:

**Dot directories and files.** Any file or directory whose name starts with a period (`.`) character is hidden automatically. This convention is taken from UNIX, which used it to hide system scripts and other special types of files and directories. Two special directories in this category are the `.` and `..` directories, which are references to the current and parent directories respectively.

**UNIX-specific directories.** The directories in this category are inherited from traditional UNIX installations. They are an important part of the system’s BSD layer but are more useful to software developers than end users. Some of the more important directories that are hidden include:

`/bin`—Contains essential command-line binaries. Typically, you execute these binaries from command-line scripts. 

`/dev`—Contains essential device files, such as mount points for attached hardware.

`/etc`—Contains host-specific configuration files.

`/sbin`—Contains essential system binaries.

`/tmp`—Contains temporary files created by apps and the system.

`/usr`—Contains non-essential command-line binaries, libraries, header files, and other data. 

`/var`—Contains log files and other files whose content is variable. (Log files are typically viewed using the Console app.)

**Explicitly hidden files and directories.** The Finder may hide specific files or directories that should not be accessed directly by the user. The most notable example of this is the `/Volumes` directory, which contains a subdirectory for each mounted disk in the local  file system from the command line. (The Finder provides a different user interface for accessing local disks.) In macOS 10.7 and later, the Finder also hides the `~/Library` directory—that is, the `Library` directory located in the user’s home directory.

**Packages and bundles.** Packages and bundles are directories that the Finder presents to the user as if they were files. Bundles hide the internal workings of executables such as apps and just present a single entity that can be moved around the file system easily. Similarly, packages allow apps to implement complex document formats consisting of multiple individual files while still presenting what appears to be a single document to the user.

Although the Finder and other system interfaces hide files and directories from the user, Cocoa interfaces such as `[NSFileManager](https://developer.apple.com/documentation/foundation/filemanager)` do not filter out files or directories that are normally invisible to users. Thus, code that uses these interfaces theoretically has a complete view of the file system and its contents. (Of course, a process really has access to only those files and directories for which it has appropriate permissions.) 

### Files and Directories Can Have Alternate Names
In some situations, the Finder presents users with file or directory names that do not match the actual names as they appear in the file system. These names are known as *display names* and are used only by the Finder and specific system components (such as the Open and Save panels) when presenting file and directory information to the user. Display names improve the user experience by presenting the user with content in a more friendly way. For example, macOS uses display names in the following situations:

**Localized names.** The system provides localized names for many system directories, such as `Applications`, `Library`, `Music`, `Movies`. An app may similarly provide localized names for itself and for any directories it creates. 

**Filename extension hiding.** The system hides filename extensions for all files by default. The user may change option, but when filename extension hiding is in effect, the characters after the last period in a filename (and the period itself) are not displayed.

Display names do not affect the actual name of the file in the file system. Code that accesses a file or directory programmatically must specify the item’s actual name when opening or manipulating the item using the file system interfaces. The only time your app should ever use display names is when displaying the name of a file or directory to the user. You can get the display name for any file or directory using the `[displayNameAtPath:](https://developer.apple.com/documentation/foundation/nsfilemanager/1409751-displaynameatpath)` method of `NSFileManager`.

> **Important:** Your code should not allow users to modify display names directly. When you want the user to specify the name of a file, use a Save panel. 

For information on how to localize the directories your app creates, see *[File System Advanced Programming Topics](../../FileSystemAdvancedPT/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010765)*. For more information about localizing app content, see *[Internationalization and Localization Guide](../../../../MacOSX/Conceptual/BPInternational/Introduction/Introduction.html#//apple_ref/doc/uid/10000171i)*. 

### The Library Directory Stores App-Specific Files
The `Library` directory is where apps and other code modules store their custom data files. Regardless of whether you are writing code for iOS or macOS, understanding the structure of the `Library` directory is important. You use this directory to store data files, caches, resources, preferences, and even user data in some specific situations.

There are several `Library` directories throughout the system but only a few that your code should ever need to access:

`Library` in the current home directory—This is the version of the directory you use the most because it is the one that contains all user-specific files. In iOS, `Library` is placed inside the apps data bundle. In macOS, it is the app’s sandbox directory or the current user’s home directory (if the app is not in a sandbox).

`/Library` (macOS only)—Apps that share resources between users store those resources in this version of the `Library` directory. Sandboxed apps are not permitted to use this directory. 

`/System/Library` (macOS only)—This directory is reserved for use by Apple.

After selecting which version of the Library directory to use, you still need to know where to store your files. The Library directory itself contains several subdirectories that subdivide app-specific content into a few well-known categories. Table 1-3 lists the most common subdirectories that you might use. Although Library directories in macOS contain many more subdirectories than the ones listed, most are used only by the system. If you want a more complete list of subdirectories, though, see [macOS Library Directory Details](../MacOSXDirectories/MacOSXDirectories.html#//apple_ref/doc/uid/TP40010672-CH10-SW1). 

**Table 1-3**  Key subdirectories of the `Library` directoryDirectory

Usage

`Application Support`

Use this directory to store all app data files except those associated with the user’s documents. For example, you might use this directory to store app-created data files, configuration files, templates, or other fixed or modifiable resources that are managed by the app. An app might use this directory to store a modifiable copy of resources contained initially in the app’s bundle. A game might use this directory to store new levels purchased by the user and downloaded from a server.

All content in this directory should be placed in a custom subdirectory whose name is that of your app’s bundle identifier or your company.

In iOS, the contents of this directory are backed up by iTunes and iCloud.

`Caches`

Use this directory to write any app-specific support files that your app can re-create easily. Your app is generally responsible for managing the contents of this directory and for adding and deleting files as needed. 

In iOS 2.2 and later, the contents of this directory are not backed up by iTunes or iCloud. In addition, the system removes files in this directory during a full restoration of the device. 

In iOS 5.0 and later, the system may delete the `Caches` directory on rare occasions when the system is very low on disk space. This will never occur while an app is running. However, be aware that restoring from backup is not necessarily the only condition under which the Caches directory can be erased.

`Frameworks`

In macOS, frameworks that must be shared by multiple apps can be installed in either the local or user domain. The Frameworks directory in the system domain stores the frameworks you use to create your macOS apps. 

In iOS, apps cannot install custom frameworks.

`Preferences`

This directory contains app-specific preference files. You should not create files in this directory yourself. Instead, use the `[NSUserDefaults](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSUserDefaults/Description.html#//apple_ref/occ/cl/NSUserDefaults)` class or CFPreferences API to get and set preference values for your app. 

In iOS, the contents of this directory are backed up by iTunes and iCloud.

## The iCloud File Storage Container
iCloud provides a structured system for storing files for apps that make use of iCloud:

Apps have a primary iCloud container directory for storing their native files.  They can also access secondary iCloud container directories listed in their app entitlements.

Inside each container directory, files are segregated into "documents" and data.  Every file or file package located in the `Documents` subdirectory (or one of its subdirectories) is presented to the user (via the iCloud UI in macOS and iOS) as a separate document that can be deleted individually.   Anything not in `Documents` or one of its subdirectories is treated as data and shown as a single entry in the iCloud UI.

Documents that the user creates and sees in an app's user interface—for example the document browsers in Pages, Numbers, and Keynote should be stored in the `Documents` directory. Another example of files that might go in the `Documents` directory are saved games, again because they are something that an app could potentially provide some sort of method for selecting.

Anything that the app does not want the user to see or modify directly should be placed outside of the `Documents` directory.  Apps can create any subdirectories inside the container directory, so they can arrange private files as desired.

Apps create files and directories in iCloud container directories in exactly the same way as they create local files and directories.  And all the file’s attributes are saved, if they add extended attributes to a file, those attributes are copied to iCloud and to the user's other devices too. 

iCloud containers also allow the storage of key-value pairs that can be easily accessed without having to create a document format. 

## How the System Identifies the Type of Content in a File
There are two primary techniques for identifying the type of content in a file:

Uniform Type Identifiers (UTIs)

Filename extensions

A *uniform type identifier* is a string that uniquely identifies a class of entities considered to have a “type.” UTIs provide consistent identifiers for data that all apps and services can recognize and rely upon. They are also more flexible than most other techniques because you can use them to represent any type of data, not just files and directories. Examples of UTIs include:

`public.text`—A public type that identifies text data.

`public.jpeg`—A public type that identifies JPEG image data.

`com.apple.bundle`—An Apple type that identifies a bundle directory.

`com.apple.application-bundle`—An Apple type that identifies a bundled app.

Whenever a UTI-based interface is available for specifying file types, you should prefer that interface over any others. Many macOS interfaces allow you to specify UTIs corresponding to the files or directories you want to work with. For example, in the Open panel, you can use UTIs as file filters and limit the types of files the user selects to ones your app can handle. Several AppKit classes, including `NSDocument`, `NSPasteboard`, and `NSImage`, support UTIs. In iOS, UTIs are used to specify pasteboard types only.

One way the system determines the UTI for a given file is by looking at its filename extension. A *filename extension* is a string of characters appended to the end of a file and separated from the main filename with a period. Each unique string of characters identifies a file of a specific type. For example, the `.strings` extension identifies a resource file with localizable string data while the `.png` extension identifies a file with image data in the portable network graphics format. 

> **Note:** Because period characters are valid characters in macOS and iOS filenames, only the characters after the last period in a filename are considered part of the filename extension. Everything to the left of the last period is considered part of the filename itself. 

If your app defines custom file formats, you should register those formats and any associated filename extensions in your app’s `Info.plist` file. The `CFBundleDocumentTypes` key specifies the file formats that your app recognizes and is able to open. Entries for any custom file formats should include both a filename extension and UTI corresponding to the file contents. The system uses that information to direct files with the appropriate type to your app.

For more information about UTIs and how you use them, see *[Uniform Type Identifiers Overview](../../understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319)*. For more information about the  `CFBundleDocumentTypes` key, see *[Information Property List Key Reference](../../../../General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009247)*.  

## Security: Protect the Files You Create
Because all user data and system code are stored on disk somewhere, protecting the integrity of files and the file system is an important job. For that reason, there are several ways to secure content and prevent it from being stolen or damaged by other processes.  

For general information about secure coding practices when working with files, see *[Secure Coding Guide](../../../../Security/Conceptual/SecureCodingGuide/Introduction.html#//apple_ref/doc/uid/TP40002415)*. 

### Sandboxes Limit the Spread of Damage
In iOS and in macOS 10.7 and later, sandboxes prevent apps from writing to parts of the file system that they should not write to. Each sandboxed app receives one or more containers that it can write into. An app cannot write to other apps’ containers or to most directories outside of the sandbox. These restrictions limit the potential damage that can be done in the event that an app’s security is breached.

Developers writing apps for macOS 10.7 and later are encouraged to put their apps in sandboxes to enhance security. Developers of iOS apps do not have to explicitly put their app in a sandbox because the system does it for them automatically at install time.

For more information about sandboxes and the types of restrictions they impose on file system access, see *[Mac App Programming Guide](../../../../General/Conceptual/MOSXAppProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010543)* and *[App Sandbox Design Guide](../../../../Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html#//apple_ref/doc/uid/TP40011183)*.

### Permissions and Access Control Lists Govern All Access to Files
Access to files and directories is governed by a mixture of access control lists (ACLs) and BSD permissions. Access control lists are a set of fine-grained controls that define exactly what can and cannot be done to a file or directory and by whom. With access control lists, you can grant individual users different levels of access to a given file or directory. By contrast, BSD permissions only allow you to give access to three classes of users: the file’s owner, a single group of users that you specify, and all users. See *[Security Overview](../../../../Security/Conceptual/Security_Overview/Introduction/Introduction.html#//apple_ref/doc/uid/TP30000976)* for more information.

> **Note:** For a file on a network server, do not make any assumptions about the ACLs and BSD permissions associated with the file. Some network file systems provide only a summarized version of this information.

Because iOS apps always run in a sandbox, the system assigns specific ACLs and permissions to files created by each app. However, macOS apps can use Identity Services to manage access control lists for files to which they have access. For information about how to use Identity Services (and the Collaboration framework), see *[Identity Services Programming Guide](../../../../Networking/Conceptual/IdentityServices_ProgGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004490)*.  

### Files Can Be Encrypted On Disk
Both macOS and iOS provide support for encrypting files on disk:

**iOS.** An iOS app can designate files that it wants to be encrypted on disk. When the user unlocks a device containing encrypted files, the system creates a decryption key that allows the app to access its encrypted files. When the user locks the device, though, the decryption key is destroyed to prevent unauthorized access to the files. 

**macOS.** Users can encrypt the contents of a volume using the Disk Utility app. (They can also encrypt just the boot volume from the Security & Privacy system preference.) The contents of an encrypted disk are available to apps only while the computer is running. When the user puts the computer to sleep or shuts it down, the decryption keys are destroyed to prevent unauthorized access to the disk’s contents.  

In iOS, apps that take advantage of disk-based encryption need to be discontinue the use of encrypted files when the user locks the device. Because locking the device destroys the decryption keys, access to encrypted files is limited to when the device is unlocked. If your iOS app can run in the background while the device is locked, it must do so without access to any of its encrypted files. Because encrypted disks in macOS are always accessible while the computer is running, macOS apps do not need to do anything special to handle disk-level encryption.

For more information about working with encrypted files in iOS, see *[App Programming Guide for iOS](../../../../iPhone/Conceptual/iPhoneOSProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007072)*. 

## Synchronization Ensures Robustness in Your File-Related Code
The file system is a resource shared by third-party apps and system apps. Because multiple apps are able to access files and directories at the same time, the potential arises for one app to make changes that render a second app’s view of the file system obsolete. If the second app is not prepared to handle such changes, it could enter an unknown state or even crash. In cases where your app relies on the presence of specific files, you can use synchronization interfaces to be notified of changes to those files. 

File system synchronization is primarily an issue in macOS, where the user can manipulate files directly with the Finder or with any number of other apps at the same time. Fortunately, macOS provides the following interfaces to help with synchronization issues:

**File coordinators.** In macOS 10.7 and later, file coordinators are a way to incorporate fine-grained synchronization support directly into the objects of your app; see [The Role of File Coordinators and Presenters](../FileCoordinators/FileCoordinators.html#//apple_ref/doc/uid/TP40010672-CH11-SW1). 

**FSEvents.** In macOS 10.5 and later, file system events allow you to monitor changes to a directory or its contents; see *[File System Events Programming Guide](../../../../Darwin/Conceptual/FSEvents_ProgGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40005289)*. 

## Files, Concurrency, and Thread Safety
Because file-related operations involve interacting with the hard disk and are therefore slow compared to most other operations, most of the file-related interfaces in iOS and macOS are designed with concurrency in mind. Several technologies incorporate asynchronous operation into their design and most others can execute safely from a dispatch queue or secondary thread. Table 1-4 lists some of the key technologies discussed in this document and whether they are safe to use from specific threads or any thread. For specific information about the capabilities of any interface, see the reference documentation for that interface.

**Table 1-4**  Thread safety of key classes and technologiesClass/Technology

Notes

`[NSFileManager](https://developer.apple.com/documentation/foundation/filemanager)`

For most tasks, it is safe to use the default `NSFileManager` object simultaneously from multiple background threads. The only exception to this rule is tasks that interact with the file manager’s delegate. When using a file manager object with a delegate, it is recommended that you create a unique instance of the `NSFileManager` class and use your delegate with that instance. You should then use your unique instance from one thread at a time. 

Grand Central Dispatch

GCD itself is safe to use from any thread. However, you are still responsible for writing your blocks in a way that is thread safe.

`[NSFileHandle](https://developer.apple.com/documentation/foundation/filehandle)`, `[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)`, Cocoa streams

Most of the Foundation objects you use to read and write file data can be used from any single thread but should not be used from multiple threads simultaneously. 

Open and Save panels

Because they are part of your user interface, you should always present and manipulate the Open and Save panels from your app’s main thread. 

POSIX routines

The POSIX routines for manipulating files are generally designed to operate safely from any thread. For details, see the corresponding man pages. 

`[NSURL](https://developer.apple.com/documentation/foundation/nsurl)` and `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)`

The immutable objects you use to specify paths are safe to use from any thread. Because they are immutable, you can also refer to them from multiple threads simultaneously. Of course, the mutable versions of these objects should be used from only one thread at a time. 

`[NSEnumerator](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSEnumerator/Description.html#//apple_ref/occ/cl/NSEnumerator)` and its subclasses

Enumerator objects are safe to use from any single thread but should not be used from multiple threads simultaneously.

Even if you use an thread-safe interface for manipulating a file, problems can still arise when multiple threads or multiple processes attempt to act on the same file. Although there are safeguards to prevent multiple clients from modifying a file at the same time, those safeguards do not always guarantee exclusive access to the file at all times. (Nor should you attempt to prevent other processes from accessing shared files.) To make sure your code knows about changes made to shared files, use file coordinators to manage access to those files. For more information about file coordinators, see [The Role of File Coordinators and Presenters](../FileCoordinators/FileCoordinators.html#//apple_ref/doc/uid/TP40010672-CH11-SW1) 

        
            [Next](../AccessingFilesandDirectories/AccessingFilesandDirectories.html)[Previous](../Introduction/Introduction.html)
        
         Copyright &#x00a9; 2018 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2018-04-09