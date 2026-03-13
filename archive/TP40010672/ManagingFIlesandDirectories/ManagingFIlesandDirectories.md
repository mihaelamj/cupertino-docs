---
title: "Managing Files and Directories"
book: "File System Programming Guide"
chapterId: "TP40010672-CH6"
date: "2018-04-09"
description: "Explains how to create and manage files and directories. "
identifier: "//apple_ref/doc/uid/TP40010672"
source: apple-archive
---

# Managing Files and Directories

> **File System Programming Guide**
> Last updated: 2018-04-09

[Next](../TechniquesforReadingandWritingCustomFiles/TechniquesforReadingandWritingCustomFiles.html)[Previous](../UsingtheOpenandSavePanels/UsingtheOpenandSavePanels.html)
        
        
        [](../index.html)
        
        # Managing Files and Directories
Some of the most basic operations involving files and directories are creating them and moving them around the file system. These operations are how your app builds the file system structure it needs to perform its tasks. For most operations, the `[NSFileManager](https://developer.apple.com/documentation/foundation/filemanager)` class should offer the functionality you need to create and manipulate files. In the rare cases where it does not, you need to use BSD-level functions directly. 

## Creating New Files and Directories Programmatically
Creating files and directories is a basic part of file management. Typically, you create custom directories to organize the files created by your code. For example, you might create some custom directories in your app’s `Application Support` directory to store any private data files managed by your app. And there are many ways to create files. 

### Creating Directories
When you want to create a custom directory, you do so using the methods of `[NSFileManager](https://developer.apple.com/documentation/foundation/filemanager)`. A process can create directories anywhere it has permission to do so, which always includes the current home directory and may include other file system locations as well. You specify the directory to create by building a path to it and passing your `[NSURL](https://developer.apple.com/documentation/foundation/nsurl)` or `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` object to one of the following methods: 

`[createDirectoryAtURL:withIntermediateDirectories:attributes:error:](https://developer.apple.com/documentation/foundation/nsfilemanager/1415371-createdirectoryaturl)` (macOS 10.7 and later only)

`[createDirectoryAtPath:withIntermediateDirectories:attributes:error:](https://developer.apple.com/documentation/foundation/filemanager/1407884-createdirectory)`

Listing 6-1 shows how to create a custom directory for app files inside the `~/Library/Application Support` directory. This method creates the directory if it does not exist and returns the path to the directory to the calling code.  Because this method touches the file system every time, you would not want to call this method repeatedly to retrieve the URL. Instead, you might call it once and then cache the returned URL.

**Listing 6-1**  Creating a custom directory for app files

- (NSURL*)applicationDirectory
```
{
```

```
    NSString* bundleID = [[NSBundle mainBundle] bundleIdentifier];
```

```
    NSFileManager*fm = [NSFileManager defaultManager];
```

```
    NSURL*    dirPath = nil;
```

```
 
```

```
    // Find the application support directory in the home directory.
```

```
    NSArray* appSupportDir = [fm URLsForDirectory:NSApplicationSupportDirectory
```

```
                                    inDomains:NSUserDomainMask];
```

```
    if ([appSupportDir count] > 0)
```

```
    {
```

```
        // Append the bundle ID to the URL for the
```

```
        // Application Support directory
```

```
        dirPath = [[appSupportDir objectAtIndex:0] URLByAppendingPathComponent:bundleID];
```

```
 
```

```
        // If the directory does not exist, this method creates it.
```

```
        // This method is only available in macOS 10.7 and iOS 5.0 or later.
```

```
        NSError*    theError = nil;
```

```
        if (![fm createDirectoryAtURL:dirPath withIntermediateDirectories:YES
```

```
                   attributes:nil error:&theError])
```

```
        {
```

```
            // Handle the error.
```

```
 
```

```
            return nil;
```

```
        }
```

```
    }
```

```
 
```

```
    return dirPath;
```

```
}
```
If your code needs to run in OS X 10.6 and earlier, you can replace any calls to the `[createDirectoryAtURL:withIntermediateDirectories:attributes:error:](https://developer.apple.com/documentation/foundation/nsfilemanager/1415371-createdirectoryaturl)` method with a similar call to the `[createDirectoryAtPath:withIntermediateDirectories:attributes:error:](https://developer.apple.com/documentation/foundation/filemanager/1407884-createdirectory)` method. The only change you have to make is to pass a string-based path instead of a URL as the first parameter. However, the `NSURL` class defines a `[path](https://developer.apple.com/documentation/foundation/nsurl/1408809-path)` method that returns a string-based version of its path.  

The `NSFileManager` methods are the preferred way to create new directories because of their simplicity. However, you can also use the `[mkdir](../../../../System/Conceptual/ManPages_iPhoneOS/man2/mkdir.2.html#//apple_ref/doc/man/2/mkdir)` function to create directories yourself. If you do so, you are responsible for creating intermediate directories and handling any errors that occur. 

### Creating New Files
There are two parts to creating a file: creating a record for the file in the file system and filling the file with content. All of the high-level interfaces for creating files perform both tasks at the same time, usually filling the file with the contents of an `[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)` or `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` object and then closing the file. You can also use lower-level functions to create an empty file and obtain a file descriptor that you can then use to fill the file with data. Some of the routines you can use to create files are:

`[createFileAtPath:contents:attributes:](https://developer.apple.com/documentation/foundation/filemanager/1410695-createfile)` (`NSFileManager`)

`[writeToURL:atomically:](https://developer.apple.com/documentation/foundation/nsdata/1415134-writetourl)` (`NSData`)

`[writeToURL:atomically:](https://developer.apple.com/documentation/foundation/nsstring/1497299-write)` (`NSString`)

`[writeToURL:atomically:encoding:error:](https://developer.apple.com/documentation/foundation/nsstring/1417341-write)` (`NSString`)

`writeToURL:atomically:` (Various collection classes define this method for writing out property lists.)

`open` function with the `O_CREAT` option creates a new empty file

> **Note:** Any files you create inherit the permissions associated with the current user and process. 

When writing the contents of a new file all at once, the system routines typically close the file after writing the contents to disk. If the routine returns a file descriptor, you can use that descriptor to continue reading and writing from the file. For information on how to read and write the contents of a file, see [Techniques for Reading and Writing Files Without File Coordinators](../TechniquesforReadingandWritingCustomFiles/TechniquesforReadingandWritingCustomFiles.html#//apple_ref/doc/uid/TP40010672-CH5-SW1). 

## Copying and Moving Files and Directories
To copy items around the file system, the `[NSFileManager](https://developer.apple.com/documentation/foundation/filemanager)` class provides the `[copyItemAtURL:toURL:error:](https://developer.apple.com/documentation/foundation/filemanager/1412957-copyitem)` and `[copyItemAtPath:toPath:error:](https://developer.apple.com/documentation/foundation/nsfilemanager/1407903-copyitematpath)` methods. To move files use the `[moveItemAtURL:toURL:error:](https://developer.apple.com/documentation/foundation/nsfilemanager/1414750-moveitematurl)` or `[moveItemAtPath:toPath:error:](https://developer.apple.com/documentation/foundation/filemanager/1413529-moveitem)` methods.

The preceding methods  move or copy a single file or directory at a time. When moving or copying a directory, the directory and all of its contents are affected. The semantics for move and copy operations are the same as in the Finder. Move operations on the same volume do not cause a new version of the item to be created. Move operations between volumes behave the same as a copy operation. And when moving or copying items, the current process must have permission to read the items and move or copy them to the new location. 

Move and copy operations can potentially take a long time to complete, and the `NSFileManager` class performs these operations synchronously. Therefore, it is recommended that you execute any such operations on a concurrent dispatch queue and not on your app’s main thread. Listing 6-2 shows an example that does just that by asynchronously creating a backup of a fictional app’s private data. (For the sake of the example, the private data is located in the `~/Library/Application Support/`*bundleID*`/Data` directory, where *bundleID* is the actual bundle identifier of the app.) If the first attempt to copy the directory fails, this method checks to see whether a previous backup exists and removes it if it does. It then proceeds to try again and aborts if it fails a second time. 

**Listing 6-2**  Copying a directory asynchronously

- (void)backupMyApplicationData {
```
   // Get the application's main data directory
```

```
   NSArray* theDirs = [[NSFileManager defaultManager] URLsForDirectory:NSApplicationSupportDirectory
```

```
                                 inDomains:NSUserDomainMask];
```

```
   if ([theDirs count] > 0)
```

```
   {
```

```
      // Build a path to ~/Library/Application Support/<bundle_ID>/Data
```

```
      // where <bundleID> is the actual bundle ID of the application.
```

```
      NSURL* appSupportDir = (NSURL*)[theDirs objectAtIndex:0];
```

```
      NSString* appBundleID = [[NSBundle mainBundle] bundleIdentifier];
```

```
      NSURL* appDataDir = [[appSupportDir URLByAppendingPathComponent:appBundleID]
```

```
                               URLByAppendingPathComponent:@"Data"];
```

```
 
```

```
      // Copy the data to ~/Library/Application Support/<bundle_ID>/Data.backup
```

```
      NSURL* backupDir = [appDataDir URLByAppendingPathExtension:@"backup"];
```

```
 
```

```
      // Perform the copy asynchronously.
```

```
      dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
```

```
         // It's good habit to alloc/init the file manager for move/copy operations,
```

```
         // just in case you decide to add a delegate later.
```

```
         NSFileManager* theFM = [[NSFileManager alloc] init];
```

```
         NSError* anError;
```

```
 
```

```
         // Just try to copy the directory.
```

```
         if (![theFM copyItemAtURL:appDataDir toURL:backupDir error:&anError]) {
```

```
            // If an error occurs, it's probably because a previous backup directory
```

```
            // already exists.  Delete the old directory and try again.
```

```
            if ([theFM removeItemAtURL:backupDir error:&anError]) {
```

```
               // If the operation failed again, abort for real.
```

```
               if (![theFM copyItemAtURL:appDataDir toURL:backupDir error:&anError]) {
```

```
                  // Report the error....
```

```
               }
```

```
            }
```

```
         }
```

```
 
```

```
      });
```

```
   }
```

```
}
```
For details on how to use the `NSFileManager` methods, see *[NSFileManager Class Reference](https://developer.apple.com/documentation/foundation/filemanager)*.

## Deleting Files and Directories
To delete files and directories, use the following methods of the `[NSFileManager](https://developer.apple.com/documentation/foundation/filemanager)` class:

`[removeItemAtURL:error:](https://developer.apple.com/documentation/foundation/nsfilemanager/1413590-removeitematurl)`

`[removeItemAtPath:error:](https://developer.apple.com/documentation/foundation/nsfilemanager/1408573-removeitematpath)`

When using these methods to delete files, realize that you are permanently removing the files from the file system; these methods do not move the file to the Trash where it can be recovered later.

For details on how to use the `NSFileManager` methods, see *[NSFileManager Class Reference](https://developer.apple.com/documentation/foundation/filemanager)*.

## Creating and Handling Invisible Files
In macOS, you make a file invisible to the user by prepending its name with a period character. Adding this character signals to the Finder and other display-related interfaces that they should treat the file as invisible under normal conditions. You should use this capability sparingly, though, and only in very specific cases. For example, the process for localizing a custom directory name involves placing hidden files with the localized names at the top-level of the directory. You should never use this technique to hide user documents or other data your app creates.

Even if you mark a file as invisible, there are still ways for the user to see it. Users can always view the true file system contents using the Terminal app. Unlike the Finder, the command line in the Terminal window displays the actual names of items in the file system and not their display names or localized names. You can also show hidden files in the Open and Save panels (using the `[setShowsHiddenFiles:](https://developer.apple.com/documentation/appkit/nssavepanel/1524285-showshiddenfiles)` method) in cases where you want the user to be able to select them for viewing or editing.

Regardless of whether a file contains a leading period, your app’s code can always see hidden files. Methods that enumerate the contents of directories generally include hidden files in the enumeration. The only exceptions to this rule is the meta directories `.` and `..` that represent the current and parent directories. Therefore, any code that enumerates the contents of a directory should be prepared to handle hidden files and deal with them appropriately, usually by just ignoring them. 

        
            [Next](../TechniquesforReadingandWritingCustomFiles/TechniquesforReadingandWritingCustomFiles.html)[Previous](../UsingtheOpenandSavePanels/UsingtheOpenandSavePanels.html)
        
         Copyright &#x00a9; 2018 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2018-04-09