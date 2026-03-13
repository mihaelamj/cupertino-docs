---
title: "Accessing Files and Directories"
book: "File System Programming Guide"
chapterId: "TP40010672-CH3"
date: "2018-04-09"
description: "Explains how to create and manage files and directories. "
identifier: "//apple_ref/doc/uid/TP40010672"
source: apple-archive
---

# Accessing Files and Directories

> **File System Programming Guide**
> Last updated: 2018-04-09

[Next](../FileCoordinators/FileCoordinators.html)[Previous](../FileSystemOverview/FileSystemOverview.html)
        
        
        [](../index.html)
        
        # Accessing Files and Directories
Before you can open a file, you first have to locate it in the file system. The system frameworks provide many routines for obtaining references to many well-known directories, such as the `Library` directory and its contents. You can also specify locations manually by building a URL or string-based path from known directory names. 

When you know the location of a file, you can then start planning the best way to access it. Depending on the type of file, you may have several options. For known file types, you typically use built-in system routines to read or write the file contents and give you an object that you can use. For custom file types, you may need to read the raw file data yourself. 

## Choose the Right Way to Access Files
Although you can open any file and read its contents as a stream of bytes, doing so is not always the right choice. macOS and iOS provide built-in support that makes opening many types of standard file formats (such as text files, images, sounds, and property lists) much easier. For these standard file formats, use the higher-level options for reading and writing the file contents. Table 2-1 lists the common file types supported by the system along with information about how you access them. 

**Table 2-1**  File types with specialized routinesFile Type

Examples               

Description

Resource files

Nib files

Image files

Sound files

Strings files

Localized resources

Apps use resource files to store data that is independent of the code that uses it. Resource files are commonly used to store localizable content such as strings and images. The process for reading data from a resource file depends on the resource type. 

For information about how to read the contents of resource files, see *[Resource Programming Guide](../../../../Cocoa/Conceptual/LoadingResources/Introduction/Introduction.html#//apple_ref/doc/uid/10000051i)*. 

Text files

Plain text file 

UTF-8 formatted file

UTF-16 formatted file

A text file is an unstructured sequence of ASCII or Unicode characters. You typically load the contents of a text file into an `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` object but may also read and write a text file as a raw stream of characters.

For information about using the `NSString` class to load text from a file, see *[String Programming Guide](../../../../Cocoa/Conceptual/Strings/introStrings.html#//apple_ref/doc/uid/10000035i)*. 

Structured data files

XML file

Property list file

Preference file

A structured data file usually consists of string-based data arranged using a set of special characters. 

For information about parsing XML, see *[Event-Driven XML Programming Guide](../../../../Cocoa/Conceptual/XMLParsing/XMLParsing.html#//apple_ref/doc/uid/10000186i)*. 

Archive files

Binary files created using a keyed archiver object

An archive is a file format used to store a persistent version of your app’s runtime objects. An archiver object encodes the state of the objects into a stream of bytes that can be written to disk all at once. An unarchiver reverses the process, using the stream of bytes to re-create the objects and restore them to their previous state. 

Archives are often a convenient alternative to implementing custom binary file formats for your documents or other data files.  

For information on how to create and read archive files, see *[Archives and Serializations Programming Guide](../../../../Cocoa/Conceptual/Archiving/Archiving.html#//apple_ref/doc/uid/10000047i)*. 

File packages

Custom document formats

A file package is a directory that contains any number of custom data files but which is presented to the user as if it were a single file. Apps can use file packages to implement complex file formats that contain multiple distinct files, or a mixture of different types of files. For example, you might use a file package if your file format includes both a binary data file and one or more image, video, or audio files. You access the contents of a file package using `[NSFileWrapper](https://developer.apple.com/documentation/foundation/filewrapper)` objects, as described in [Using FileWrappers as File Containers](../FileWrappers/FileWrappers.html#//apple_ref/doc/uid/TP40010672-CH13-SW2).  

Bundles

Apps

Plug-ins

Frameworks

Bundles provide a structured environment for storing code and the resources used by that code. Most of the time, you do not work with the bundle itself but with its contents. However, you can locate bundles and obtain information about them as needed. 

For information about bundles and how you access them, see *[Bundle Programming Guide](../../../../CoreFoundation/Conceptual/CFBundles/Introduction/Introduction.html#//apple_ref/doc/uid/10000123i)*

Code files

Binary code resources

Dynamic shared libraries

Apps that work with plug-ins and shared libraries need to be able to load the associated code for that item to take advantage of its functionality.

For information about how to load code resources into memory, see *[Code Loading Programming Topics](../../../../Cocoa/Conceptual/LoadingCode/LoadingCode.html#//apple_ref/doc/uid/10000052i)*. 

In situations where the standard file formats are insufficient, you can always create your own custom file formats. When reading and writing the content of custom files, you read and write data as a stream of bytes and apply those bytes to your app’s file-related data structures. You have complete control over how you read and write the bytes and how you manage your file-related data structures.  For more information about the techniques for reading and writing files that use custom file formats, see [Techniques for Reading and Writing Files Without File Coordinators](../TechniquesforReadingandWritingCustomFiles/TechniquesforReadingandWritingCustomFiles.html#//apple_ref/doc/uid/TP40010672-CH5-SW1).

### Specifying the Path to a File or Directory
The preferred way to specify the location of a file or directory is to use the `[NSURL](https://developer.apple.com/documentation/foundation/nsurl)` class. Although the `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` class has many methods related to path creation, URLs offer a more robust way to locate files and directories. For apps that also work with network resources, URLs also mean that you can use one type of object to manage items located on a local file system or on a network server.

> **Note:** In addition to `NSURL`, you can also use the `[CFURLRef](https://developer.apple.com/documentation/corefoundation/cfurl)` opaque type to manipulate paths as URLs. The `NSURL` class is toll-free bridged with the `CFURLRef` type, which means you can use them interchangeably in your code. For more information about how to create and manipulate URLs using Core Foundation, see *[CFURL Reference](https://developer.apple.com/documentation/corefoundation/cfurl-rd7)*. 

For most URLs, you build the URL by concatenating directory and file names together using the appropriate `NSURL` methods until you have the path to the item. A URL built in that way is referred to as a *path-based URL* because it stores the names needed to traverse the directory hierarchy to locate the item. (You also build string-based paths by concatenating directory and file-names together, with the results stored in a slightly different format than that used by the `NSURL` class.) In addition to path-based URLs, you can also create a *file reference URL*, which identifies the location of the file or directory using a unique ID. 

All of the following entries are valid references to a file called `MyFile.txt` in a user’s `Documents` directory:

**Path-based URL:** `file://localhost/Users/steve/Documents/MyFile.txt` 

**File reference URL:** `file:///.file/id=6571367.2773272/`

**String-based path:** `/Users/steve/Documents/MyFile.txt`

You create URL objects using the `NSURL` methods and convert them to file reference URLs only when needed. Path-based URLs are easier to manipulate, easier to debug, and are generally preferred by classes such as `[NSFileManager](https://developer.apple.com/documentation/foundation/filemanager)`. An advantage of file reference URLs is that they are less fragile than path-based URLs while your app is running. If the user moves a file in the Finder, any path-based URLs that refer to the file immediately become invalid and must be updated to the new path. However, as long as the file moved to another location on the same disk, its unique ID does not change and any file reference URLs remain valid. 

> **Important:** Although they are safe to use while your app is running, file reference URLs are not safe to store and reuse between launches of your app because a file’s ID may change if the system is rebooted. If you want to store the location of a file persistently between launches of your app, create a bookmark as described in Locating Files Using Bookmarks.  

Of course, there are still times when you might need to use strings to refer to a file. Fortunately, the `NSURL` class provides methods to convert path-based URLs to and from `NSString` objects. You might use a string-based path when presenting that path to the user or when calling a system routine that accepts strings instead of URLs. The conversion between `NSURL` objects and `NSString` objects is done using the NSURL class’s method `[absoluteString](https://developer.apple.com/documentation/foundation/nsurl/1409868-absolutestring)`.

Because `NSURL` and `NSString` describe only the location of a file or directory, you can create them before the actual file or directory exists. Neither class attempts to validate the actual existence of the file or directory you specify. In fact, you must create the path to a nonexistent file or directory before you can create it on disk. 

If you have an `NSURL` object that refers to an actual file or directory on disk, and you want to get the user visible name of the volume on which it resides, you use the method `[getResourceValue:forKey:error:](https://developer.apple.com/documentation/foundation/nsurl/1408874-getresourcevalue)` in a two step process:

Use the `[NSURLVolumeURLKey](https://developer.apple.com/documentation/foundation/nsurlvolumeurlkey)` constant to obtain the volume URL from the resource URL.

Use the `[NSURLLocalizedNameKey](https://developer.apple.com/documentation/foundation/urlresourcekey/1412706-localizednamekey)` constant to obtain the user visible volume name from the volume URL.

Use the `NSURLLocalizedNameKey` constant rather than the `[NSURLNameKey](https://developer.apple.com/documentation/foundation/nsurlnamekey)` constant. Using `NSURLNameKey` returns the name of the volume in the file system, which may not be the same as the user visible name. Listing 2-1 demonstrates this process for the current user’s home directory.

**Listing 2-1**  Obtaining the user visible volume name for a resource URL

NSURL *url = [NSURL fileURLWithPath:NSHomeDirectory()];
```
NSURL *volumeURL = nil;
```

```
NSString *volumeName = nil;
```

```
if ([url getResourceValue:&volumeURL forKey:NSURLVolumeURLKey error:nil]) {
```

```
    if ([volumeURL getResourceValue:&volumeName forKey:NSURLLocalizedNameKey error:nil]) {
```

```
        NSLog(@"The volume name is: %@", volumeName);
```

```
    }
```

```
}
```
For more information about the methods you use to create and manipulate URLs and strings, see *[NSURL Class Reference](https://developer.apple.com/documentation/foundation/nsurl)* and *[NSString Class Reference](https://developer.apple.com/documentation/foundation/nsstring)*.

## Locating Items in the File System
Before you can access a file or directory, you need to know its location. There are several ways to locate files and directories:

Find the file yourself.

Ask the user to specify a location.

Locating files in the standard system directories, in both sandboxed and non-sandboxed apps.

Using bookmarks.

The file systems of iOS and macOS impose specific guidelines on where you should place files, so most of the items your app creates or uses should be stored in a well-known place. Both systems provide interfaces for locating items in such well-known places, and your app can use these interfaces to locate items and build paths to specific files. An app should prompt the user to specify the location of a file or directory only in a limited number of situations that are described in [Using the Open and Save Panels](../UsingtheOpenandSavePanels/UsingtheOpenandSavePanels.html#//apple_ref/doc/uid/TP40010672-CH4-SW1). 

### Asking the User to Locate an Item
In macOS, user interactions with the file system should always be through the standard Open and Save panels. Because these panels involve interrupting the user, use them only in a limited number of situations:

To open user documents

To ask the user where to save a new document

To associate user files (or directories of files) with an open window 

Never use the Open and Save panels to access any files that your app created and uses internally. Support files, caches, and app-generated data files should be placed in one of the standard directories dedicated to app-specific files.

For information on how to present and customize the Open and Save panels, see [Using the Open and Save Panels](../UsingtheOpenandSavePanels/UsingtheOpenandSavePanels.html#//apple_ref/doc/uid/TP40010672-CH4-SW1).  

### Locating Items in Your App Bundle
Apps that need to locate resource files inside their bundle directory (or inside another known bundle) must do so using an `[NSBundle](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/cl/NSBundle)` object. Bundles eliminate the need for your app to remember the location of individual files by organizing those files in a specific way. The methods of the `NSBundle` class understand that organization and use it to locate your app’s resources on demand. The advantage of this technique is that you can generally rearrange the contents of your bundle without rewriting the code you use to access it. Bundles also take advantage of the current user’s language settings to locate an appropriately localized version of a resource file.

The following code shows how to retrieve a URL object for an image named `MyImage.png` that is located in the app’s main bundle. This code determines only the location of the file; it does not open the file. You would pass the returned URL to a method of the `[NSImage](https://developer.apple.com/documentation/appkit/nsimage)` class to load the image from disk so that you could use it.  

NSURL* url = [[NSBundle mainBundle] URLForResource:@"MyImage" withExtension:@"png"];For more information about bundles, including how to locate items in a bundle, see *[Bundle Programming Guide](../../../../CoreFoundation/Conceptual/CFBundles/Introduction/Introduction.html#//apple_ref/doc/uid/10000123i)*. For specific information about loading and using resources in your app, see *[Resource Programming Guide](../../../../Cocoa/Conceptual/LoadingResources/Introduction/Introduction.html#//apple_ref/doc/uid/10000051i)*. 

### Locating Items in the Standard Directories
When you need to locate a file in one of the standard directories, use the system frameworks to locate the directory first and then use the resulting URL to build a path to the file. The Foundation framework includes several options for locating the standard system directories. By using these methods, the paths will be correct whether your app is sandboxed or not: 

The `[URLsForDirectory:inDomains:](https://developer.apple.com/documentation/foundation/nsfilemanager/1407726-urlsfordirectory)` method of the `NSFileManager` class returns a directory’s location packaged in an `NSURL` object. The directory to search for is an `[NSSearchPathDirectory](https://developer.apple.com/documentation/foundation/nssearchpathdirectory)` constant. These constants provide URLs for the user’s home directory, as well as most of the standard directories.

The `[NSSearchPathForDirectoriesInDomains](https://developer.apple.com/documentation/foundation/1414224-nssearchpathfordirectoriesindoma)` function behaves like the `URLsForDirectory:inDomains:` method but returns the directory’s location as a string-based path. Use the `[URLsForDirectory:inDomains:](https://developer.apple.com/documentation/foundation/nsfilemanager/1407726-urlsfordirectory)` method instead.

The `[NSHomeDirectory](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSHomeDirectory)` function returns the path to either the user’s or app’s home directory. (Which home directory is returned depends on the platform and whether the app is in a sandbox.) When an app is sandboxed the home directory points to the app’s sandbox, otherwise it points to the User’s home directory on the file system. If constructing a file to a subdirectory of a user’s home directory, consider using the `[URLsForDirectory:inDomains:](https://developer.apple.com/documentation/foundation/nsfilemanager/1407726-urlsfordirectory)` method instead.

You can use the URL or path-based string you receive from the preceding routines to build new objects with the locations of the files you want. Both the `[NSURL](https://developer.apple.com/documentation/foundation/nsurl)` and `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` classes provide path-related methods for adding and removing path components and making changes to the path in general. Listing 2-2 shows an example that searches for the standard `Application Support` directory and creates a new URL for a directory containing the app’s data files.

**Listing 2-2**  Creating a URL for an item in the app support directory

```
- (NSURL*)applicationDataDirectory {
```

```
    NSFileManager* sharedFM = [NSFileManager defaultManager];
```

```
    NSArray* possibleURLs = [sharedFM URLsForDirectory:NSApplicationSupportDirectory
```

```
                                 inDomains:NSUserDomainMask];
```

```
    NSURL* appSupportDir = nil;
```

```
    NSURL* appDirectory = nil;
```

```
 
```

```
    if ([possibleURLs count] >= 1) {
```

```
        // Use the first directory (if multiple are returned)
```

```
        appSupportDir = [possibleURLs objectAtIndex:0];
```

```
    }
```

```
 
```

```
    // If a valid app support directory exists, add the
```

```
    // app's bundle ID to it to specify the final directory.
```

```
    if (appSupportDir) {
```

```
        NSString* appBundleID = [[NSBundle mainBundle] bundleIdentifier];
```

```
        appDirectory = [appSupportDir URLByAppendingPathComponent:appBundleID];
```

```
    }
```

```
 
```

```
    return appDirectory;
```

```
}
```
### Locating Files Using Bookmarks
If you want to save the location of a file persistently, use the bookmark capabilities of `[NSURL](https://developer.apple.com/documentation/foundation/nsurl)`. A *bookmark* is an opaque data structure, enclosed in an `NSData` object, that describes the location of a file. Whereas path- and file reference URLs are potentially fragile between launches of your app, a bookmark can usually be used to re-create a URL to a file even in cases where the file was moved or renamed.

To create a bookmark for an existing URL, use the `[bookmarkDataWithOptions:includingResourceValuesForKeys:relativeToURL:error:](https://developer.apple.com/documentation/foundation/nsurl/1417795-bookmarkdatawithoptions)` method of `NSURL`. Specifying the `[NSURLBookmarkCreationSuitableForBookmarkFile](https://developer.apple.com/documentation/foundation/nsurlbookmarkcreationoptions/nsurlbookmarkcreationsuitableforbookmarkfile)` option creates an `NSData` object suitable for saving to disk. Listing 2-3 shows a simple example implementation that uses this method to create a bookmark data object.

**Listing 2-3**  Converting a URL to a persistent form

- (NSData*)bookmarkForURL:(NSURL*)url {
```
    NSError* theError = nil;
```

```
    NSData* bookmark = [url bookmarkDataWithOptions:NSURLBookmarkCreationSuitableForBookmarkFile
```

```
                                            includingResourceValuesForKeys:nil
```

```
                                            relativeToURL:nil
```

```
                                            error:&theError];
```

```
    if (theError || (bookmark == nil)) {
```

```
        // Handle any errors.
```

```
        return nil;
```

```
    }
```

```
    return bookmark;
```

```
}
```
If you write the persistent bookmark data to disk using the `[writeBookmarkData:toURL:options:error:](https://developer.apple.com/documentation/foundation/nsurl/1408532-writebookmarkdata)` method of `NSURL`, what the system creates on disk is an alias file. Aliases are similar to symbolic links but are implemented differently. Normally, users create aliases from the Finder when they want to create links to files elsewhere on the system.

To transform a bookmark data object back into a URL, use the `[URLByResolvingBookmarkData:options:relativeToURL:bookmarkDataIsStale:error:](https://developer.apple.com/documentation/foundation/nsurl/1572035-urlbyresolvingbookmarkdata)` method of `NSURL`. Listing 2-4 shows the process for converting a bookmark back into a URL. 

**Listing 2-4**  Returning a persistent bookmark to its URL form

- (NSURL*)urlForBookmark:(NSData*)bookmark {
```
    BOOL bookmarkIsStale = NO;
```

```
    NSError* theError = nil;
```

```
    NSURL* bookmarkURL = [NSURL URLByResolvingBookmarkData:bookmark
```

```
                                            options:NSURLBookmarkResolutionWithoutUI
```

```
                                            relativeToURL:nil
```

```
                                            bookmarkDataIsStale:&bookmarkIsStale
```

```
                                            error:&theError];
```

```
 
```

```
    if (bookmarkIsStale || (theError != nil)) {
```

```
        // Handle any errors
```

```
        return nil;
```

```
    }
```

```
    return bookmarkURL;
```

```
}
```
The Core Foundation framework provides a set of C-based functions that parallel the bookmark interface provided by `NSURL`. For more information about using those functions, see *[CFURL Reference](https://developer.apple.com/documentation/corefoundation/cfurl-rd7)*. 

## Enumerating the Contents of a Directory
To discover the contents of a given directory, you [enumerate](../../../../General/Conceptual/DevPedia-CocoaCore/Enumeration.html#//apple_ref/doc/uid/TP40008195-CH17) that directory. Cocoa supports enumerating a directory one file at a time or all at once. Regardless of which option you choose, enumerate directories sparingly. Each enumeration can touch a large number of files and subdirectories. This quickly becomes expensive.

### Enumerating a Directory One File at a Time
Enumerating a directory one file at a time is recommended when you want to search for a specific file and stop enumerating when you find it. File-by-file [enumeration](../../../../General/Conceptual/DevPedia-CocoaCore/Enumeration.html#//apple_ref/doc/uid/TP40008195-CH17) uses the `[NSDirectoryEnumerator](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDirectoryEnumerator/Description.html#//apple_ref/occ/cl/NSDirectoryEnumerator)` class, which defines the methods for retrieving items. Because `NSDirectoryEnumerator` itself is an abstract class, however, you do not create instances of it directly. Instead, you use either the `[enumeratorAtURL:includingPropertiesForKeys:options:errorHandler:](https://developer.apple.com/documentation/foundation/nsfilemanager/1409571-enumeratoraturl)` or the `[enumeratorAtPath:](https://developer.apple.com/documentation/foundation/filemanager/1408726-enumerator)` method of `[NSFileManager](https://developer.apple.com/documentation/foundation/filemanager)` object to obtain a concrete instance of the class for use in your enumeration.  

Enumerator objects return the paths of all files and directories contained within the enumerated directory. Because enumerations are recursive and cross device boundaries, the number of files and directories returned may be more than what is present in the starting directory. You can skip the contents of any directory you are not interested in by calling the enumerator object’s `[skipDescendents](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDirectoryEnumerator/Description.html#//apple_ref/occ/instm/NSDirectoryEnumerator/skipDescendents)` method. Enumerator objects do not resolve symbolic links or attempt to traverse symbolic links that point to directories. 

Listing 2-5 shows how to use the  `enumeratorAtURL:includingPropertiesForKeys:options:errorHandler:` method to list all the user-visible subdirectories of a given directory, noting whether they are directories or file packages. The `keys` array tells the enumerator object to prefetch and cache information for each item. Prefetching this information improves efficiency by touching the disk only once. The options argument specifies that the enumeration should not list the contents of file packages and hidden files. The error handler is a [block object](../../../../General/Conceptual/DevPedia-CocoaCore/Block.html#//apple_ref/doc/uid/TP40008195-CH3) that returns a Boolean value. If the block returns `YES`, the enumeration continues after the error; if it returns `NO`, the enumeration stops.

**Listing 2-5**  Enumerating the contents of a directory

NSURL *directoryURL = <#An NSURL object that contains a reference to a directory#>;
```
 
```

```
NSArray *keys = [NSArray arrayWithObjects:
```

```
    NSURLIsDirectoryKey, NSURLIsPackageKey, NSURLLocalizedNameKey, nil];
```

```
 
```

```
NSDirectoryEnumerator *enumerator = [[NSFileManager defaultManager]
```

```
                                     enumeratorAtURL:directoryURL
```

```
                                     includingPropertiesForKeys:keys
```

```
                                     options:(NSDirectoryEnumerationSkipsPackageDescendants |
```

```
                                              NSDirectoryEnumerationSkipsHiddenFiles)
```

```
                                     errorHandler:^(NSURL *url, NSError *error) {
```

```
                                         // Handle the error.
```

```
                                         // Return YES if the enumeration should continue after the error.
```

```
                                         return <#YES or NO#>;
```

```
                                     }];
```

```
 
```

```
for (NSURL *url in enumerator) {
```

```
 
```

```
    // Error checking is omitted for clarity.
```

```
 
```

```
    NSNumber *isDirectory = nil;
```

```
    [url getResourceValue:&isDirectory forKey:NSURLIsDirectoryKey error:NULL];
```

```
 
```

```
    if ([isDirectory boolValue]) {
```

```
 
```

```
        NSString *localizedName = nil;
```

```
        [url getResourceValue:&localizedName forKey:NSURLLocalizedNameKey error:NULL];
```

```
 
```

```
        NSNumber *isPackage = nil;
```

```
        [url getResourceValue:&isPackage forKey:NSURLIsPackageKey error:NULL];
```

```
 
```

```
        if ([isPackage boolValue]) {
```

```
            NSLog(@"Package at %@", localizedName);
```

```
        }
```

```
        else {
```

```
            NSLog(@"Directory at %@", localizedName);
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
You can use other methods declared by `NSDirectoryEnumerator` to determine attributes of files during the enumeration—both of the parent directory and the current file or directory—and to control recursion into subdirectories. The code in Listing 2-6 enumerates the contents of a directory and lists files that have been modified within the last 24 hours; if, however, it comes across RTFD file packages, it skips recursion into them.

**Listing 2-6**  Looking for files that have been modified recently

```
NSString *directoryPath = <#Get a path to a directory#>;
```

```
NSDirectoryEnumerator *directoryEnumerator = [[NSFileManager defaultManager] enumeratorAtPath:directoryPath];
```

```
 
```

```
NSDate *yesterday = [NSDate dateWithTimeIntervalSinceNow:(-60*60*24)];
```

```
 
```

```
for (NSString *path in directoryEnumerator) {
```

```
 
```

```
    if ([[path pathExtension] isEqualToString:@"rtfd"]) {
```

```
        // Don't enumerate this directory.
```

```
        [directoryEnumerator skipDescendents];
```

```
    }
```

```
    else {
```

```
 
```

```
        NSDictionary *attributes = [directoryEnumerator fileAttributes];
```

```
        NSDate *lastModificationDate = [attributes objectForKey:NSFileModificationDate];
```

```
 
```

```
        if ([yesterday earlierDate:lastModificationDate] == yesterday) {
```

```
            NSLog(@"%@ was modified within the last 24 hours", path);
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
### Getting the Contents of a Directory in a Single Batch Operation
If you know that you want to look at every item in a directory, you can retrieve a snapshot of the items and iterate over them at your convenience. Retrieving the contents of a directory in a batch operation is not the most efficient way to enumerate a directory, because the file manager must walk the entire directory contents every time. However, if you plan to look at all the items anyway, it is a much simpler way to retrieve the items. 

There are two options for doing a batch enumeration of a directory using `[NSFileManager](https://developer.apple.com/documentation/foundation/filemanager)`: 

To perform a shallow enumeration, use the `[contentsOfDirectoryAtURL:includingPropertiesForKeys:options:error:](https://developer.apple.com/documentation/foundation/nsfilemanager/1413768-contentsofdirectoryaturl)` or `[contentsOfDirectoryAtPath:error:](https://developer.apple.com/documentation/foundation/filemanager/1414584-contentsofdirectory)` method. 

To perform a deep enumeration and return only subdirectories, use the `[subpathsOfDirectoryAtPath:error:](https://developer.apple.com/documentation/foundation/nsfilemanager/1417353-subpathsofdirectoryatpath)` method. 

Listing 2-7 shows an example that uses the `[contentsOfDirectoryAtURL:includingPropertiesForKeys:options:error:](https://developer.apple.com/documentation/foundation/nsfilemanager/1413768-contentsofdirectoryaturl)` method to enumerate the contents of a directory. One of the benefits of using URLs is that you can also efficiently retrieve additional information about each item. This example retrieves the localized name, creation date, and type information for each item in the directory and stores that information in the corresponding `[NSURL](https://developer.apple.com/documentation/foundation/nsurl)` object. When the method returns, you can proceed to iterate over the items in the `array` variable and do what you need with them.

**Listing 2-7**  Retrieving the list of items in a directory all at once

```
NSURL *url = <#A URL for a directory#>;
```

```
NSError *error = nil;
```

```
NSArray *properties = [NSArray arrayWithObjects: NSURLLocalizedNameKey,
```

```
                          NSURLCreationDateKey, NSURLLocalizedTypeDescriptionKey, nil];
```

```
 
```

```
NSArray *array = [[NSFileManager defaultManager]
```

```
                   contentsOfDirectoryAtURL:url
```

```
                   includingPropertiesForKeys:properties
```

```
                   options:(NSDirectoryEnumerationSkipsHiddenFiles)
```

```
                   error:&error];
```

```
if (array == nil) {
```

```
    // Handle the error
```

```
}
```
## Determining Where to Store Your App-Specific Files
The `Library` directory is the designated repository for files your app creates and manages on behalf of the user. Consider that these directories may be in different locations if your app is sandboxed. As a result, always use the `NSFileManager` method `[URLsForDirectory:inDomains:](https://developer.apple.com/documentation/foundation/nsfilemanager/1407726-urlsfordirectory)` with the `[NSUserDomainMask](https://developer.apple.com/documentation/foundation/nssearchpathdomainmask/nsuserdomainmask)` as the domain to locate the specific directory that you should use to store this data.

Use the `Application Support` directory constant  `[NSApplicationSupportDirectory](https://developer.apple.com/documentation/foundation/filemanager/searchpathdirectory/applicationsupportdirectory)`, appending your *<bundle_ID>* for:

Resource and data files that your app creates and manages for the user. You might use this directory to store app state information, computed or downloaded data, or even user created data that you manage on behalf of the user.

Autosave files. 

Use the `Caches` directory constant `[NSCachesDirectory](https://developer.apple.com/documentation/foundation/nssearchpathdirectory/nscachesdirectory)`, appending your *<bundle_ID>* directory for cached data files or any files that your app can re-create easily.

Read and write preferences using the `[NSUserDefaults](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSUserDefaults/Description.html#//apple_ref/occ/cl/NSUserDefaults)` class. This class automatically writes preferences to the appropriate location.

Use the `NSFileManager` method `[URLsForDirectory:inDomains:](https://developer.apple.com/documentation/foundation/nsfilemanager/1407726-urlsfordirectory)` to get the directory for storing temporary files. Temporary files are files you intend to use immediately for some ongoing operation but then plan to discard later. Delete temporary files as soon as you are done with them.

> **Important:** If you do not delete temporary files after three days, the system may delete them for you whether you are using them or not.

> **Note:** Apps rarely share the files stored in the `Application Support`, `Caches` or `Temp` directories. Therefore, you don’t typically need to use a file coordinator (`NSFileCoordinator`) when reading or writing to these locations. For more information about file coordination, see [The Role of File Coordinators and Presenters](../FileCoordinators/FileCoordinators.html#//apple_ref/doc/uid/TP40010672-CH11-SW1).

## Tips for Accessing Files and Directories
Because the file system is a resource shared by all processes, including system processes, use it carefully. Even on systems with solid-state disk drives, file operations tend to be a little slower because of the latency involved in retrieving data from the disk. And when you do access files, it is important that you do so in a way that is secure and does not interfere with other processes.

### Perform File Operations Lazily
Operations involving the file system should be performed only when they are absolutely necessary. Accessing the file system usually takes a lot of time relative to other computer-wide tasks. So make sure you really need to access the disk before you do. Specifically: 

**Write data to disk only when you have something valuable to save.** The definition of what is valuable is different for each app but should generally involve information that the user provides explicitly. For example, if your app creates some default data structures at launch time, do not save those structures to disk unless the user changes them.  

**Read data from disk only when you need it.** In other words, load data that you need for your user interface now but do not load an entire file just to get one small piece of data from that file. For custom file formats, use file mapping or read only the few chunks of a file that you need to present your user interface. Read any remaining chunks as needed in response to user interactions with the data. For structured data formats, use Core Data to manage and optimize the reading of data for you.

### Use Secure Coding Practices
There are several principles you can follow to help ensure that you do not have file-based security vulnerabilities in your program:

Check the result codes for any functions or methods you call. Result codes are there to let you know that there is a problem, so do pay attention to them. For example, if you try to delete a directory that you think is empty and get an error code, you might find that the directory is not empty after all. 

When working in a directory to which your process does not have exclusive access, you must check to make sure a file does not exist before you create it. You must also verify that the file you intend to read from or write to is the same file you created.

Whenever possible use routines that operate on file descriptors rather than pathnames. In this way you can be sure you’re always dealing with the same file.

Intentionally create files as a separate step from opening them so that you can verify that you are opening a file you created rather than one that already existed

Know whether the function you are calling follows symbolic links. For example, the `lstat` function gives you the status of a file regardless of whether it’s a normal file or a symbolic link, but the `stat` function follows symbolic links and, if the specified file was a symbolic link, returns the status of the linked-to file. Therefore, if you use the `stat` function, you might be accessing a different file than you expected.

Before you read a file, make sure that file has the owner and permissions you expect. Be prepared to fail gracefully (rather than hanging) if it does not. 

Set your process’ file code creation mask (umask) to restrict access to files created by your process. The umask is a bitmask that alters the default permissions of a new file. You do not reset the umask for your app, your process inherits the umask from its parent process. For more information about setting the umask, see the `umask(2)` man page.

For additional tips and coding practices, see [Race Conditions and Secure File Operations](../../../../Security/Conceptual/SecureCodingGuide/Articles/RaceConditions.html#//apple_ref/doc/uid/TP40002585) in *[Secure Coding Guide](../../../../Security/Conceptual/SecureCodingGuide/Introduction.html#//apple_ref/doc/uid/TP40002415)*. 

### Assume Case Sensitivity for Paths
When searching or comparing filenames, always assume that the underlying file system is case sensitive. macOS supports many file systems that use case to differentiate between files. Even on file systems (such as APFS and HFS+) that support case insensitivity, there are still times when case may be used to compare filenames. For example, the `[NSBundle](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/cl/NSBundle)` class and CFBundle APIs consider case when searching [bundle](../../../../General/Conceptual/DevPedia-CocoaCore/Bundle.html#//apple_ref/doc/uid/TP40008195-CH4) directories for named resources. 

### Include Filename Extensions for New Files
All files should have a filename extension that reflects the type of content contained in the file. Filename extensions help the system determine how to open files and also make it easier to exchange files with other computers or other platforms. For example, network transfer programs often use filename extensions to determine the best way to to transfer files between computers.

### Use Display Names When Presenting Items
Whenever you need to present the name of a file or directory in your user interface, always use the item’s display name. Using the display name ensures that what your app presents matches what the Finder and other apps are presenting. For example, if your app shows the path to a file, using the display name ensures that the path is localized in accordance with the current user’s language settings.

For more information about display names, see [Files and Directories Can Have Alternate Names](../FileSystemOverview/FileSystemOverview.html#//apple_ref/doc/uid/TP40010672-CH2-SW10). 

### Accessing Files Safely from Background Threads
In general, the objects and functions used to access and manipulate files can be used from any thread of your app. However, as with all threaded programming, make sure you manipulate files safely from your background threads:

Avoid using the same file descriptor from multiple threads. Doing so could cause each thread to disrupt the state of the other.

Always use file coordinators to access files. File coordinators provide notifications when other threads in your program (or other processes) access the same file. You can use these notifications to clean up your local data structures or perform other relevant tasks.

Create a unique instance of `[NSFileManager](https://developer.apple.com/documentation/foundation/filemanager)` for each thread when copying, moving, linking, or deleting files. Although most file manager operations are thread-safe, operations involving a delegate require a unique instance of `NSFileManager`, especially if you are performing those operations on background threads. 

        
            [Next](../FileCoordinators/FileCoordinators.html)[Previous](../FileSystemOverview/FileSystemOverview.html)
        
         Copyright &#x00a9; 2018 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2018-04-09