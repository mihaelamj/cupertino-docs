---
title: "Techniques for Reading and Writing Files Without File Coordinators"
book: "File System Programming Guide"
chapterId: "TP40010672-CH5"
date: "2018-04-09"
description: "Explains how to create and manage files and directories. "
identifier: "//apple_ref/doc/uid/TP40010672"
source: apple-archive
---

# Techniques for Reading and Writing Files Without File Coordinators

> **File System Programming Guide**
> Last updated: 2018-04-09

[Next](../FileWrappers/FileWrappers.html)[Previous](../ManagingFIlesandDirectories/ManagingFIlesandDirectories.html)
        
        
        [](../index.html)
        
        # Techniques for Reading and Writing Files Without File Coordinators
Reading and writing files involves transferring a sequence of bytes between your code and the underlying disk. This is the lowest-level form of file management but is also the foundation for more sophisticated techniques as well. At some point, even the most sophisticated data structures have to be turned into a sequence of bytes before they can be stored on disk. Similarly, that same data must be read from the disk as a sequence of bytes before it can be used to reconstruct the more sophisticated data structures that it represents.

There are several different technologies for reading and writing the contents of files, nearly all of which are supported by both iOS and macOS. All of them do essentially the same thing but in slightly different ways. Some technologies require you to read and write file data sequentially, while others may allow you to jump around and operate on only part of a file. Some technologies provide automatic support for reading and writing asynchronously, while others execute synchronously so that you have more control over their execution. 

Choosing from the available technologies is a matter of deciding how much control you want over the reading and writing process and how much effort you want to spend writing your file management code. Higher-level technologies like Cocoa streams limit your flexibility but provide an easy-to-use interface. Lower-level technologies like POSIX and Grand Central Dispatch (GCD) give you maximum flexibility and power but require you to write a little more code.  

## Reading and Writing Files Asynchronously
Because file operations involve accessing a disk (possibly one on a network server), performing those operations asynchronously is almost always preferred. Technologies such as Cocoa streams and Grand Central Dispatch (GCD) are designed to execute asynchronously at all times, which allows you to focus on reading and writing file data rather than worrying about where your code executes. 

### Processing an Entire File Linearly Using Streams
If you always read or write a file’s contents from start to finish, streams provide a simple interface for doing so asynchronously. Streams are typically used for managing sockets and other types of sources where data may become available over time. However, you can also use streams to read or write an entire file in one or more bursts. There are two types of streams available:

Use an `[NSOutputStream](https://developer.apple.com/documentation/foundation/nsoutputstream)` to write data sequentially to disk. 

Use an `[NSInputStream](https://developer.apple.com/documentation/foundation/inputstream)` object to read data sequentially from disk. 

Stream objects use the run loop of the current thread to schedule read and write operations on the stream. An input stream wakes up the run loop and notifies its delegate when there is data waiting to be read. An output stream wakes up the run loop and notifies its delegate when there is space available for writing data. When operating on files, this behavior usually means that the run loop is woken up several times in quick succession so that your delegate code can read or write the file data. It also means that your delegate code is called repeatedly until you close the stream object, or in the case of input streams until the stream reaches the end of the file.

For information and examples about how to set up and use stream objects to read and write data, see *[Stream Programming Guide](../../../../Cocoa/Conceptual/Streams/Streams.html#//apple_ref/doc/uid/10000188i)*.

### Processing a File Using GCD
Grand Central Dispatch provides several different ways to read or write the content of files asynchronously:

Create a dispatch I/O channel and use it to read or write data.

Use the `[dispatch_read](https://developer.apple.com/documentation/dispatch/1388933-dispatch_read)` or `[dispatch_write](https://developer.apple.com/documentation/dispatch/1388969-dispatch_write)` convenience functions to perform a single asynchronous operation.

Create a dispatch source to schedule the execution of a custom event handler [block](../../../../General/Conceptual/DevPedia-CocoaCore/Block.html#//apple_ref/doc/uid/TP40008195-CH3), in which you use standard POSIX calls to read or write data from the file. 

Dispatch I/O channels are the preferred way to read and write files because they give you direct control over when file operations occur but still allow you to process the data asynchronously on a dispatch queue. A dispatch I/O channel is an `[dispatch_io_t](https://developer.apple.com/documentation/dispatch/dispatch_io_t)` structure that identifies the file whose contents you want to read or write. Channels can be configured for stream-based access or random access of files. A stream-based channel forces you to read or write file data sequentially, whereas a random-access channel lets you read or write at any offset from the beginning of the file.

If you do not want the trouble of creating and managing a dispatch I/O channel, you can use the `[dispatch_read](https://developer.apple.com/documentation/dispatch/1388933-dispatch_read)` or `[dispatch_write](https://developer.apple.com/documentation/dispatch/1388969-dispatch_write)` functions to perform a single read or write operation on a file descriptor. These methods are convenient for situations where you do not want or need the overhead of creating and managing a dispatch I/O channel. However, you should use them only when performing a single read or write operation on a file. If you need to perform multiple operations on the same file, creating a dispatch I/O channel is much more efficient.  

Dispatch sources allow you to process files in a way that is similar to Cocoa stream objects. Like stream objects, they are used more often with sockets or data sources that send and receive data sporadically but they can still be used with files. A dispatch source schedules its associated event handler block whenever there is data waiting to be read or space available for writing. For files, this usually results in the block being scheduled repeatedly and in quick succession until you explicitly cancel the dispatch source or it reaches the end of the file it is reading.

For more information about creating and using dispatch sources, see *[Concurrency Programming Guide](../../../../General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)*. For information about the `dispatch_read` or `dispatch_write` functions, or any other GCD functions, see *Grand Central Dispatch (GCD) Reference*. 

#### Creating and Using a Dispatch I/O Channel
To create a dispatch I/O channel, you must provide either a file descriptor or the name of the file you want to open. If you already have an open file descriptor, passing it to the channel changes the ownership of that file descriptor from your code to the channel. A dispatch I/O channel takes control of its file descriptor so that it can reconfigure that file descriptor as needed. For example, the channel usually reconfigures the file descriptor with the `O_NONBLOCK` flag so that subsequent read and write operations do not block the current thread. Creating a channel using a file path causes the channel to create the necessary file descriptor and take control of it.

Listing 7-1 shows a simple example of how to open a dispatch I/O channel using an `NSURL` object. In this case, the channel is configured for random read-access and assigned to a custom [property](../../../../General/Conceptual/DevPedia-CocoaCore/DeclaredProperty.html#//apple_ref/doc/uid/TP40008195-CH13) of the current class. The queue and block act to clean up the channel in the event that an error occurs during creation or at the end of the channel’s lifecycle. If an error occurs during creation, you can use the error code to determine what happened. An error code of 0 indicates that the channel relinquished control of its file descriptor normally, usually as a result of calling the `[dispatch_io_close](https://developer.apple.com/documentation/dispatch/1388953-dispatch_io_close)` function, and that you can now dispose of the channel safely.

**Listing 7-1**  Creating a dispatch I/O channel

-(void)openChannelWithURL:(NSURL*)anURL {
```
   NSString* filePath = [anURL path];
```

```
   self.channel = dispatch_io_create_with_path(DISPATCH_IO_RANDOM,
```

```
                      [filePath UTF8String],   // Convert to C-string
```

```
                      O_RDONLY,                // Open for reading
```

```
                      0,                       // No extra flags
```

```
                      dispatch_get_main_queue(),
```

```
                      ^(int error){
```

```
                         // Cleanup code for normal channel operation.
```

```
                         // Assumes that dispatch_io_close was called elsewhere.
```

```
                         if (error == 0) {
```

```
                            dispatch_release(self.channel);
```

```
                            self.channel = NULL;
```

```
                         }
```

```
                      });
```

```
}
```
After creating a dispatch channel, you can store a reference to the resulting `[dispatch_io_t](https://developer.apple.com/documentation/dispatch/dispatch_io_t)` structure and use it to initiate read or write calls at your convenience. If you created a channel that supports random access, you can start reading or writing at any location. If you create a stream-based channel, any offset value you specify as a starting point is ignored and data is read or written at the current location. For example, to read the second 1024 bytes from a channel that supports random access, your read call might look similar to the following:

dispatch_io_read(self.channel,
```
                 1024,                        // 1024 bytes into the file
```

```
                 1024,                        // Read the next 1024 bytes
```

```
                 dispatch_get_main_queue(),   // Process the bytes on the main thread
```

```
                 ^(bool done, dispatch_data_t data, int error){
```

```
                     if (error == 0) {
```

```
                        // Process the bytes.
```

```
                     }
```

```
                 });
```
A write operation requires you to specify the bytes you want written to the file, the location at which to begin writing (for random access channels), and a handler block with which to receive progress reports. You initiate write operations using the `[dispatch_io_write](https://developer.apple.com/documentation/dispatch/dispatchio/1388932-write)` function, which is described in *Grand Central Dispatch (GCD) Reference*.

#### Manipulating Dispatch Data for an I/O Channel
All channel-based operations use `[dispatch_data_t](https://developer.apple.com/documentation/dispatch/dispatch_data_t)` structures to manipulate the data read or written using a channel. A `dispatch_data_t` structure is an opaque type that manages one or more contiguous memory buffers. The use of an opaque type allows GCD to use discontiguous buffers internally while still presenting the data to your app as if it were more or less contiguous. The actual implementation details of how dispatch data structures work is not important, but understanding how to create them or get data out of them is. 

To write data to a dispatch I/O channel, your code must provide a `dispatch_data_t` structure with the bytes to write. You do this using the `[dispatch_data_create](https://developer.apple.com/documentation/dispatch/1452970-dispatch_data_create)` function, which takes a pointer to a buffer and the size of the buffer and returns a `dispatch_data_t` structure that encapsulates the data from that buffer. How the data object encapsulates the buffer depends on the destructor you provide when calling the `dispatch_data_create` function. If you use the default destructor, the data object makes a copy of the buffer and takes care of releasing that buffer at the appropriate time. However, if you do not want the data object to copy the buffer you provide, you must provide a custom destructor [block](../../../../General/Conceptual/DevPedia-CocoaCore/Block.html#//apple_ref/doc/uid/TP40008195-CH3) to handle any needed cleanup when the data object itself is released. 

> **Note:** If you have multiple data buffers that you want to write to a file as a single contiguous block of data, you can create a single dispatch data object that represents all of those buffers. Using the `[dispatch_data_create_concat](https://developer.apple.com/documentation/dispatch/1453003-dispatch_data_create_concat)` function, you can append additional data buffers to a `dispatch_data_t` structure. The buffers themselves can all be independent and in different parts of memory but the dispatch data object collects them and represents them as a single entity. (You can even use the `[dispatch_data_create_map](https://developer.apple.com/documentation/dispatch/1452937-dispatch_data_create_map)` function to generate a contiguous version of your buffers.) Especially for disk-based operations, concatenating multiple buffers lets you write large amounts of data to a file using one call to the `[dispatch_io_write](https://developer.apple.com/documentation/dispatch/dispatchio/1388932-write)` function, which is much more efficient than calling `dispatch_io_write` separately for each independent buffer.

To extract bytes from a dispatch data object, you use the `[dispatch_data_apply](https://developer.apple.com/documentation/dispatch/1452852-dispatch_data_apply)` function. Because dispatch data objects are opaque, you use this function to iterate over the buffers in the object and process them using a block that you provide. For a dispatch data object with a single contiguous buffer, your block is called once. For a data object with multiple buffers, your block is called as many times as there are buffers. Each time your block is called, it is passed a data buffer and some information about that buffer.

Listing 7-2 shows an example that opens a channel and reads a UTF8 formatted text file, creating `NSString` objects for the contents of the file. This particular example reads 1024 bytes at a time, which is an arbitrary amount and may not yield the best performance. However, it does demonstrate the basic premise of how to use the `[dispatch_io_read](https://developer.apple.com/documentation/dispatch/dispatchio/1388941-read)` function in combination with the `dispatch_data_apply` function to read the bytes and then convert them into a form that your app might want. In this case, the block that processes the bytes uses the dispatch data object’s buffer to initialize a new string object. It then hands the string off to the custom `addString:toFile:` method, which in this case would store it for later use.

**Listing 7-2**  Reading the bytes from a text file using a dispatch I/O channel

- (void)readContentsOfFile:(NSURL*)anURL {
```
   // Open the channel for reading.
```

```
   NSString*   filePath = [anURL path];
```

```
   self.channel = dispatch_io_create_with_path(DISPATCH_IO_RANDOM,
```

```
                       [filePath UTF8String],   // Convert to C-string
```

```
                       O_RDONLY,                // Open for reading
```

```
                       0,                       // No extra flags
```

```
                       dispatch_get_main_queue(),
```

```
                       ^(int error){
```

```
                          // Cleanup code
```

```
                          if (error == 0) {
```

```
                             dispatch_release(self.channel);
```

```
                             self.channel = nil;
```

```
                          }
```

```
                      });
```

```
 
```

```
   // If the file channel could not be created, just abort.
```

```
   if (!self.channel)
```

```
      return;
```

```
 
```

```
    // Get the file size.
```

```
    NSNumber* theSize;
```

```
    NSInteger fileSize = 0;
```

```
    if ([anURL getResourceValue:&theSize forKey:NSURLFileSizeKey error:nil])
```

```
        fileSize = [theSize integerValue];
```

```
 
```

```
   // Break the file into 1024 size strings.
```

```
   size_t chunkSize = 1024;
```

```
   off_t  currentOffset = 0;
```

```
 
```

```
   for (currentOffset = 0; currentOffset < fileSize; currentOffset += chunkSize) {
```

```
      dispatch_io_read(self.channel, currentOffset, chunkSize, dispatch_get_main_queue(),
```

```
                     ^(bool done, dispatch_data_t data, int error){
```

```
                        if (error)
```

```
                           return;
```

```
 
```

```
                        // Build strings from the data.
```

```
                        dispatch_data_apply(data, (dispatch_data_applier_t)^(dispatch_data_t region,
```

```
                                               size_t offset, const void *buffer, size_t size){
```

```
                           NSAutoreleasePool* pool = [[NSAutoreleasePool alloc] init];
```

```
                           NSString* aString = [[[NSString alloc] initWithBytes:buffer
```

```
                                         length:size encoding:NSUTF8StringEncoding] autorelease];
```

```
 
```

```
                           [self addString:aString toFile:anURL];  // Custom method.
```

```
                           [pool release];
```

```
                           return true;  // Keep processing if there is more data.
```

```
                        });
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
For more information about the functions you use to manipulate dispatch data objects, see *Grand Central Dispatch (GCD) Reference*. 

## Reading and Writing Files Synchronously
The file-related interfaces that operate on data synchronously give you the flexibility to set your code’s execution context yourself. Just because they execute synchronously does not mean that they are less efficient than their asynchronous counterparts. On the contrary, it just means that the interface itself does not provide the asynchronous execution context automatically. If you want to read and write data asynchronously using these technologies, and get all the same benefits, you must provide the asynchronous execution context yourself. Of course, the best way to do that is to execute your code using GCD dispatch queues or operation objects.

### Building Your Content in Memory and Writing It to Disk All at Once
The simplest way to manage the reading and writing of file data is all at once. This works well for custom document types where you expect the size of the document’s on-disk representation to remain reasonably small. You would not want to do this for multimedia files or for files whose size can grow to many megabytes in size.

For custom document types that use a binary or private file format, you can use the `[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)` or `[NSMutableData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSMutableData)` class to transfer your custom data to and from disk. You can create new data objects in many different ways. For example, you can use a keyed archiver object to convert a graph of objects into a linear stream of bytes enclosed in a data object. If you have a binary file format that is very structured, you can append bytes to an `NSMutableData` object and build your data object piece by piece. When you are ready to write the data object to disk, use the `[writeToURL:atomically:](https://developer.apple.com/documentation/foundation/nsdata/1415134-writetourl)` or `[writeToURL:options:error:](https://developer.apple.com/documentation/foundation/nsdata/1410595-writetourl)` method. These methods allow you to create the corresponding on-disk file in one step.

> **Note:** In iOS, one of the options you can pass to the `[writeToURL:options:error:](https://developer.apple.com/documentation/foundation/nsdata/1410595-writetourl)` method allows you to specify whether you want the contents of the file encrypted. If you specify one of these options, the contents are encrypted immediately to ensure the security of the file. 

To read data back from disk, use the `[initWithContentsOfURL:options:error:](https://developer.apple.com/documentation/foundation/nsdata/1407864-initwithcontentsofurl)` method to obtain a data object based on the contents of your file. You can use this data object to reverse the process you used when creating it. Thus, if you used a keyed archiver to create the data object, you can use a keyed unarchiver to re-create your object graph. If you wrote the data out piece by piece, you can parse the byte stream in the data object and use it to reconstruct your document’s data structures. 

Apps that use the `[NSDocument](https://developer.apple.com/documentation/appkit/nsdocument)` infrastructure typically interact with the file system indirectly using `NSData` objects. When the user saves a document, the infrastructure prompts the corresponding `NSDocument` object for a data object to write to disk. Similarly, when the user opens an existing document, it creates the document object and passes it a data object with which to initialize itself. 

For more information about the `NSData` and `NSMutableData` classes, see *[Foundation Framework Reference](https://developer.apple.com/documentation/foundation)*. For more information about using the document infrastructure in a macOS app, see *[Mac App Programming Guide](../../../../General/Conceptual/MOSXAppProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010543)*. 

### Reading and Writing Files Using NSFileHandle
The use of the `[NSFileHandle](https://developer.apple.com/documentation/foundation/filehandle)` class closely parallels the process for reading and writing files at the POSIX level. The basic process is that you open the file, issue read or write calls, and close the file when you are done. In the case of `NSFileHandle`, you open the file automatically when you create an instance of the class. The file handle object acts as a wrapper for the file, managing the underlying file descriptor for you. Depending on whether you requested read access, write access, or both, you then call the methods of `NSFileHandle` to read or write actual bytes. When you are done, you release your file handle object to close the file. 

Listing 7-3 shows a very simple method that reads the entire contents of a file using a file handle object. The `[fileHandleForReadingFromURL:error:](https://developer.apple.com/documentation/foundation/nsfilehandle/1408422-filehandleforreadingfromurl)` method creates the file handle object as an autoreleased object, which causes it to be released automatically at some point after this method returns. 

**Listing 7-3**  Reading the contents of a file using `NSFileHandle`

- (NSData*)readDataFromFileAtURL:(NSURL*)anURL {
```
    NSFileHandle* aHandle = [NSFileHandle fileHandleForReadingFromURL:anURL error:nil];
```

```
    NSData* fileContents = nil;
```

```
 
```

```
    if (aHandle)
```

```
        fileContents = [aHandle readDataToEndOfFile];
```

```
 
```

```
    return fileContents;
```

```
}
```
For more information about the methods of the `NSFileHandle` class, see *[NSFileHandle Class Reference](https://developer.apple.com/documentation/foundation/filehandle)*. 

### Managing Disk Reads and Writes at the POSIX Level
If you prefer to use C-based functions for your file management code, the POSIX layer offers standard functions for working with files. At the POSIX level, you identify a file using a file descriptor, which is an integer value that identifies an open file uniquely within your app. You pass this file descriptor to any subsequent functions that require it. The following list contains the main POSIX functions you use to manipulate files:

Use the `open` function to obtain a file descriptor for your file.

Use the `pread`, `read`, or `readv` function to read data from an open file descriptor. For information about these functions, see `[pread](../../../../System/Conceptual/ManPages_iPhoneOS/man2/pread.2.html#//apple_ref/doc/man/2/pread)`. 

Use the `pwrite`, `write`, or `writev` function to write data to an open file descriptor. For information about these functions, see `[pwrite](../../../../System/Conceptual/ManPages_iPhoneOS/man2/pwrite.2.html#//apple_ref/doc/man/2/pwrite)`.

Use the `[lseek](../../../../System/Conceptual/ManPages_iPhoneOS/man2/lseek.2.html#//apple_ref/doc/man/2/lseek)` function to reposition the current file pointer and change the location at which you read or write data. 

Use the `[pclose](../../../../System/Conceptual/ManPages_iPhoneOS/man3/pclose.3.html#//apple_ref/doc/man/3/pclose)` function to close the file descriptor when you are done with it.

> **Important:** On some file systems, there is no guarantee that a successful write call results in the actual writing of bytes to the file system. For some network file systems, written data might be sent to the server at some point after your write call. To verify that the data actually made it to the file, use the `[fsync](../../../../System/Conceptual/ManPages_iPhoneOS/man2/fsync.2.html#//apple_ref/doc/man/2/fsync)` function to force the data to the server or close the file and make sure it closed successfully—that is, the `close` function did not return `-1`.  

Listing 7-4 shows a simple function that uses POSIX calls to read the first 1024 bytes of a file and return them in an `NSData` object. If the file has fewer than 1024 bytes, the method reads as many bytes as possible and truncates the data object to the actual number of bytes. 

**Listing 7-4**  Reading the contents of a file using POSIX functions

- (NSData*)readDataFromFileAtURL:(NSURL*)anURL {
```
    NSString* filePath = [anURL path];
```

```
    fd = open([filePath UTF8String], O_RDONLY);
```

```
    if (fd == -1)
```

```
        return nil;
```

```
 
```

```
    NSMutableData* theData = [[[NSMutableData alloc] initWithLength:1024] autorelease];
```

```
    if (theData) {
```

```
        void* buffer = [theData mutableBytes];
```

```
        NSUInteger bufferSize = [theData length];
```

```
 
```

```
        NSUInteger actualBytes = read(fd, buffer, bufferSize);
```

```
        if (actualBytes < 1024)
```

```
            [theData setLength:actualBytes];
```

```
    }
```

```
 
```

```
    close(fd);
```

```
    return theData;
```

```
}
```
Because there is a limit to the number of open file descriptors an app may have at any given time, you should always close file descriptors as soon as you are done using them. File descriptors are used not only for open files but for communications channels such as sockets and pipes. And your code is not the only entity creating file descriptors for your app. Every time you load a resource file or use a framework that communicates over the network, the system creates a file descriptor on behalf of your code. If your code opens large numbers of sockets or files and never closes them, system frameworks may not be able to create file descriptors at critical times.

## Getting and Setting File Metadata Information
Files contain a lot of useful information but so does the file system. For each file and directory, the file system stores meta information about things like the item’s size, creation date, owner, permissions, whether a file is locked, or whether a file’s extension is hidden. There are several ways to get and set this information but the most prominent ways are:

Get and set most content metadata information (including Apple-specific attributes) using the `[NSURL](https://developer.apple.com/documentation/foundation/nsurl)` class.

Get and set basic file-related information using the `[NSFileManager](https://developer.apple.com/documentation/foundation/filemanager)` class.

The `NSURL` class offers a wide range of file-related information, including information that is standard for the file system (such as file size, type, owner, and permissions) but also a lot of Apple-specific information (such as the assigned label, localized name, the icon associated with the file, whether the file is a package, and so on). In addition, some methods that take URLs as arguments allow you to cache attributes while you are performing other operations on the file or directory. Especially when accessing large numbers of files, this type of caching behavior can improve performance by minimizing the number of disk-related operations. Regardless of whether attributes are cached, you retrieve them from the NSURL object using its `[getResourceValue:forKey:error:](https://developer.apple.com/documentation/foundation/nsurl/1408874-getresourcevalue)` method and set new values for some attributes using the `[setResourceValue:forKey:error:](https://developer.apple.com/documentation/foundation/nsurl/1413819-setresourcevalue)` or `[setResourceValues:error:](https://developer.apple.com/documentation/foundation/nsurl/1408208-setresourcevalues)` method.

Even if you are not using the `NSURL` class, you can still get and set some file-related information using the `[attributesOfItemAtPath:error:](https://developer.apple.com/documentation/foundation/nsfilemanager/1410452-attributesofitematpath)` and `[setAttributes:ofItemAtPath:error:](https://developer.apple.com/documentation/foundation/nsfilemanager/1413667-setattributes)` methods of the `NSFileManager` class. These methods let you retrieve information about file system items like their type, size, and the level of access currently supported. You can also use the `NSFileManager` class to retrieve more general information about the file system itself, such as its size, the amount of free space, the number of nodes in the file system, and so on. Note that you are only able to get a subset of resources for iCloud files.

For more information about the `NSURL` and `NSFileManager` classes, and the attributes you can obtain for files and directories, see *[NSURL Class Reference](https://developer.apple.com/documentation/foundation/nsurl)* and *[NSFileManager Class Reference](https://developer.apple.com/documentation/foundation/filemanager)*. 

        
            [Next](../FileWrappers/FileWrappers.html)[Previous](../ManagingFIlesandDirectories/ManagingFIlesandDirectories.html)
        
         Copyright &#x00a9; 2018 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2018-04-09