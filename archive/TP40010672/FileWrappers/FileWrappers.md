---
title: "Using FileWrappers as File Containers"
book: "File System Programming Guide"
chapterId: "TP40010672-CH13"
date: "2018-04-09"
description: "Explains how to create and manage files and directories. "
identifier: "//apple_ref/doc/uid/TP40010672"
source: apple-archive
---

# Using FileWrappers as File Containers

> **File System Programming Guide**
> Last updated: 2018-04-09

[Next](../PerformanceTips/PerformanceTips.html)[Previous](../TechniquesforReadingandWritingCustomFiles/TechniquesforReadingandWritingCustomFiles.html)
        
        
        [](../index.html)
        
        # Using FileWrappers as File Containers
Documents composed of a complex mix of text, binary blobs, and metadata often use a package format to store their data across multiple files and directories, as described in [Document Packages](../../../../CoreFoundation/Conceptual/CFBundles/DocumentPackages/DocumentPackages.html#//apple_ref/doc/uid/10000123i-CH106). You use instances of the `[NSFileWrapper](https://developer.apple.com/documentation/foundation/filewrapper)` class, or *file wrappers*, to efficiently manage packages in your app.

File wrappers represent the nodes (files, symbolic links, and directories) in your package’s hierarchy, linked together in a structure that mirrors that of the underlying file system objects. Like the nodes themselves, file wrappers come in three varieties:

**Regular file wrappers** provide access to regular file content and attributes. Examples of regular files include images (containing binary that encodes a single image in a particular format), plist files (storing metadata as structured text), and plain text files (containing an arbitrary, contiguous block of text).

**Symbolic link file wrappers** represent symbolic links in the file system. Instead of storing content directly, these provide a reference to regular files, directories, or other symbolic links elsewhere in the document package.
**Directory file wrappers** hold a collection of other file wrappers (of any kind), representing the contents of the corresponding directory.

These file wrappers enable you to represent and manipulate the contents (and attributes) of your document package, with the following benefits:

**Single point of control.** Your app keeps only a reference to a top-level document file wrapper. The underlying hierarchy of directories and files in your document package is encoded into a tree of linked file wrappers that you access, starting from the top-level file wrapper.

**Efficient disk access.** During a (recursive) save operation, the system writes only explicitly modified file wrappers to disk. When you have many distinct pieces of data that do not change concurrently, this capability helps to reduce disk access. This is especially important in the context of iCloud-based documents, to avoid unnecessary network activity.

## Using File Wrappers in Cocoa Documents
Because the Cocoa document architecture automatically provides many capabilities (as described in *[Document-Based App Programming Guide for Mac](../../../../DataManagement/Conceptual/DocBasedAppProgrammingGuideForOSX/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011179)*) that integrate seamlessly with file wrappers, you often use file wrappers in the context of Cocoa documents. In this case, you typically interact with file wrappers at three specific times:

**While reading a document from disk.** When a document is read from disk, your app’s override of the `NSDocument` class’s `readFromFileWrapper:ofType:error:` method receives as a first argument a file wrapper representing the document package. From this top-level starting point, you traverse the hierarchy of file wrappers contained by the document wrapper to extract the document’s contents and populate your data model. You also store a reference to the top-level wrapper for later use.

**While updating a document’s model.** Whenever your data model changes, you remove the affected wrapper or wrappers (but only those that are affected) from the hierarchy using the `[removeFileWrapper:](https://developer.apple.com/documentation/foundation/filewrapper/1417343-removefilewrapper)` method. This indicates that the affected file wrappers need to be recreated with new data on the next save operation.

**While writing a document to disk.** When a document is saved to disk, the read process is reversed. Your app’s override of the `NSDocument` class’s `fileWrapperOfType:error:` method provides the system with the one document file wrapper that is the top-level entity of your document package. First, however, you create any sub-wrappers that are missing. That may be all of them in the case of a new document. For a document previously read from disk, for which you have the wrapper provided during `readFromFileWrapper:ofType:error:`, it is only those that were removed because of model changes.

### Managing a Document Package in Your Document Subclass
As a concrete example of how to use file wrappers to manage a document package with Cocoa documents, consider a simple document composed of a block of text and an image that the user can choose to hide or display. Rather than merging this heterogeneous data into a single file, you define a document package, which in this simple example is a directory containing three regular files:

A plain text file called `Text.txt` holds the text.

An image file called `Image.png` holds the image.
A plist file called `MetaData.plist` stores a Boolean indicating whether the image is hidden or displayed in the UI.

To begin your `NSDocument` subclass implementation, you first define the package file names and metadata dictionary keys:

// File names
```
static NSString* const ImageFileName    = @"Image.png";
```

```
static NSString* const TextFileName     = @"Text.txt";
```

```
static NSString* const MetaDataFileName = @"MetaData.plist";
```

```
 
```

```
// Metadata keys
```

```
static NSString* const MetaDataHiddenKey = @"HiddenKey";
```
Next, create a simple data model as a group of properties on your document, and keep a reference to the top-level document object:

```
@interface MyDocument : NSDocument
```

```
 
```

```
// Model
```

```
@property (copy) NSString* text;
```

```
@property (strong) NSImage* image;
```

```
@property (strong) NSMutableDictionary* metaDataDict;
```

```
 
```

```
// Top-level document wrapper
```

```
@property (strong) NSFileWrapper* docWrapper;
```

```
 
```

```
@end
```
### Traversing the File Wrapper Hierarchy When Opening a Document
When reading a document from disk, the system invokes your document’s `readFromFileWrapper:ofType:error:` method, providing as a first argument the file wrapper representing the document package. Begin this method by asking the top-level file wrapper for its own list of file wrappers:

- (BOOL)readFromFileWrapper:(NSFileWrapper *)fileWrapper
```
                     ofType:(NSString *)typeName
```

```
                      error:(NSError **)outError
```

```
{
```

```
    NSDictionary *fileWrappers = [fileWrapper fileWrappers];
```
The `[fileWrappers](https://developer.apple.com/documentation/foundation/filewrapper/1409437-filewrappers)` property, available only in directory file wrappers, contains a dictionary of other file wrappers, corresponding to the files (and directories) inside the top-level directory of the package.

In this simple example, the package contains exactly three regular files in a flat directory structure. For more complex documents, you might define an arbitrarily deep tree of directories, files, and symbolic links, which you traverse by recursively querying the `[fileWrappers](https://developer.apple.com/documentation/foundation/filewrapper/1409437-filewrappers)` property of the directory file wrappers at each level.

When you define a static package structure, and yours is the only code reading and writing it, you know each file wrapper’s type and treat it accordingly. In some cases, however, you may not know the wrapper types ahead of time. For example, if your document format allows for a dynamic tree of directories and files that your code discovers while reading the package, you can query each encountered file wrapper for its type using the methods `[regularFile](https://developer.apple.com/documentation/foundation/filewrapper/1415680-isregularfile)`, `[directory](https://developer.apple.com/documentation/foundation/nsfilewrapper/1409030-directory)`, and `[symbolicLink](https://developer.apple.com/documentation/foundation/filewrapper/1408125-issymboliclink)`.

From the collection of file wrappers at a given level in the hierarchy, you next look for the expected component file wrappers. Continuing the example, first obtain the image file wrapper by using its name as a dictionary key. If the resulting wrapper is `nil`, the package has no image file (perhaps because the image was omitted when the document was last saved). In this case, the document’s image property remains `nil`, to indicate there is no image. If the wrapper is not `nil`, you read the data from the image file indicated by the file wrapper into the `image` property:

    // Get the image data
```
    NSFileWrapper *imageWrapper = fileWrappers[ImageFileName];
```

```
 
```

```
    if (imageWrapper != nil) {
```

```
        NSData *imageData = [imageWrapper regularFileContents];
```

```
        self.image = [[NSImage alloc] initWithData:imageData];
```

```
    }
```
The image file wrapper, itself a regular file wrapper, provides the `[regularFileContents](https://developer.apple.com/documentation/foundation/nsfilewrapper/1410178-regularfilecontents)` method that returns the file content as an `NSData` object. You know that this data represents an image because you defined the document package that way, so you use the data to initialize an `NSImage` object.

Similarly, read the text file wrapper into its model object. For this element, supply a default, empty string if no wrapper is found:

    NSFileWrapper *textWrapper = fileWrappers[TextFileName];
```
 
```

```
    if (textWrapper == nil) {
```

```
        self.text = @“”;   // Default to empty text
```

```
    } else {
```

```
        NSData *textData = [textWrapper regularFileContents];
```

```
        self.text = [[NSString alloc] initWithData:textData
```

```
                                          encoding:NSUTF8StringEncoding];
```

```
    }
```
Finally, read the metadata. In this case, create an `NSError` instance when no wrapper is found to represent a corrupt document package, and store its address in the read method’s `outError` pointer, if one is provided:

    NSFileWrapper *metaDataWrapper = fileWrappers[MetaDataFileName];
```
 
```

```
    if (metaDataWrapper == nil) {
```

```
        if (outError) {
```

```
            *outError = <# Corrupt Package Error #>;
```

```
        }
```

```
        return NO;      // Read failed
```

```
    } else {
```

```
        NSData *metaData = [metaDataWrapper regularFileContents];
```

```
        self.metaDataDict = [NSPropertyListSerialization
```

```
                              propertyListWithData:metaData
```

```
                                           options:NSPropertyListImmutable
```

```
                                            format:NULL
```

```
                                             error:outError];
```

```
        if (self.metaDataDict == nil) {
```

```
            return NO;  // Read failed
```

```
        }
```

```
    }
```
At the end of the read operation, keep a reference to the top-level file wrapper in your document (for use later when making model changes and saving the document), and report success by returning `YES`:

    self.docWrapper = fileWrapper;
```
    return YES;  // Read succeeded
```

```
 
```

```
} // end of readFromFileWrapper
```

> **Note:** When a package item is missing or unreadable, you can ignore it and do nothing, ignore it and set a default value, or indicate a read error, as demonstrated in the example above. Which you choose depends on your app’s particular requirements.

### Invalidating File Wrappers When the Model Changes
Whenever the user makes a change to the document, one or more file wrappers within the hierarchy may become invalid. Continuing with the example above, because this sample app allows the user to add, replace, or delete an image, its document subclass exposes a method to set the image (possibly to a `nil` value). After updating the model, this method additionally searches for the image’s file wrapper. If the file wrapper exists, the method invalidates it by removing it from the document using the `[removeFileWrapper:](https://developer.apple.com/documentation/foundation/filewrapper/1417343-removefilewrapper)` method of the `NSFileWrapper` class:

- (void)updateImageModel:(NSImage *)image
```
{
```

```
    // Update the model
```

```
    self.image = image;
```

```
 
```

```
    // Invalidate the image file wrapper, if it exists
```

```
    NSFileWrapper *imageWrapper = self.docWrapper.fileWrappers[ImageFileName];
```

```
    if (imageWrapper != nil) {
```

```
        [self.docWrapper removeFileWrapper:imageWrapper];
```

```
    }
```

```
}
```
Because the corresponding file on disk is no longer valid, you discard the file wrapper from the hierarchy. As you’ll see shortly, on the next save, the document creates a new file wrapper (and corresponding file) by using the latest version of the model data to overwrite the old one.

Similarly, provide model update methods for the text and the metadata:

- (void)updateTextModel:(NSString *)text
```
{
```

```
    // Update the model
```

```
    self.text = text;
```

```
 
```

```
    // Invalidate the text file wrapper, if it exists
```

```
    NSFileWrapper *textWrapper = self.docWrapper.fileWrappers[TextFileName];
```

```
    if (textWrapper != nil) {
```

```
       [self.docWrapper removeFileWrapper:textWrapper];
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
- (void)updateHidden:(BOOL)hidden
```

```
{
```

```
    // Update the model
```

```
    [self.metaDataDict setValue:@(hidden) forKey:MetaDataHiddenKey];
```

```
 
```

```
    // Invalidate the metadata file wrapper, if it exists
```

```
    NSFileWrapper *metaWrapper = self.docWrapper.fileWrappers[MetaDataFileName];
```

```
    if (metaWrapper != nil) {
```

```
        [self.docWrapper removeFileWrapper:metaWrapper];
```

```
    }
```

```
}
```
Notice that a given model update affects only the relevant file wrapper, which helps minimize disk access. For example, a typical usage scenario for this document type might be that, after opening the document, the user repeatedly updates the text, but infrequently changes the image. As a result, while the text file wrapper is removed, the image file wrapper (and the underlying file) remains intact and unchanged across numerous save operations, making it unnecessary to repeatedly update that data on disk.

### Populating Missing File Wrappers During a Save
When the user saves a document, or during an autosave operation, the system calls your document’s `fileWrapperOfType:error:` method, looking for the document’s file wrapper to write to disk. Begin by checking to see if the wrapper already exists in your document’s properties. If not, create it:

```
- (NSFileWrapper *)fileWrapperOfType:(NSString *)typeName error:(NSError **)outError
```

```
{
```

```
    NSError* error = nil; // Set later, if appropriate
```

```
 
```

```
    if (self.docWrapper == nil) {
```

```
        self.docWrapper = [[NSFileWrapper alloc]
```

```
                                 initDirectoryWithFileWrappers:@{}];
```

```
    }
```
When you initialize a document wrapper, you specify its type (regular, directory, or symbolic link). In this case, the wrapper represents a document package, which is a directory, and so you use the `[initDirectoryWithFileWrappers:](https://developer.apple.com/documentation/foundation/nsfilewrapper/1415121-initdirectorywithfilewrappers)` initialization method. The collection of file wrappers that this directory contains starts empty, to be filled in shortly.
Next, obtain any file wrappers already contained in the document wrapper by looking at the document wrapper’s `[fileWrappers](https://developer.apple.com/documentation/foundation/filewrapper/1409437-filewrappers)` property:    NSDictionary *fileWrappers = self.docWrapper.fileWrappers;If you just created the document wrapper, it is of course empty, but for an existing document wrapper that you stored at the end of the open operation, it contains any file wrappers not invalidated by model updates since then, or since the last save operation.

For each file wrapper that you expect to find in this document, but that are not present, create them from model data, beginning with the image wrapper. If it does not exist in the collection, but if there is an image stored in the model, create an `NSData` object containing the image content. Then create the file wrapper using the `[initRegularFileWithContents:](https://developer.apple.com/documentation/foundation/nsfilewrapper/1409508-initregularfilewithcontents)` method with that data to create a regular file wrapper. Name the wrapper, and add it to the document wrapper collection.

    if ((fileWrappers[ImageFileName] == nil) && (self.image != nil)) {
```
        // Get the image as data as PNG
```

```
        NSData *imageData = [NSBitmapImageRep
```

```
                  representationOfImageRepsInArray:[self.image representations]
```

```
                                         usingType:NSPNGFileType
```

```
                                        properties:@{}];
```

```
 
```

```
        // Convert to PNG, if necessary
```

```
        if (imageData == nil) {
```

```
            imageData = [self.image TIFFRepresentation];
```

```
            NSBitmapImageRep *imageRep = [[NSBitmapImageRep alloc]
```

```
                                             initWithData:imageData];
```

```
 
```

```
            imageData = [imageRep representationUsingType:NSPNGFileType
```

```
                                               properties:@{}];
```

```
        }
```

```
 
```

```
        // Create, name, and add the file wrapper
```

```
        NSFileWrapper *imageFileWrapper = [[NSFileWrapper alloc]
```

```
                                            initRegularFileWithContents:imageData];
```

```
        [imageFileWrapper setPreferredFilename:ImageFileName];
```

```
        [self.docWrapper addFileWrapper:imageFileWrapper];
```

```
    }
```
Notice that when no image exists in the model, the document does not create an image wrapper, and the file is omitted from the package. As described earlier, the read operation accommodates this condition.

Now add the text file wrapper, if missing. In this case, make use of the convenience method `[addRegularFileWithContents:preferredFilename:](https://developer.apple.com/documentation/foundation/filewrapper/1418374-addregularfile)` to create, name, and add the regular file wrapper in one call:

    if (fileWrappers[TextFileName] == nil) {
```
        NSData *textData = [self.text dataUsingEncoding:NSUTF8StringEncoding];
```

```
        [self.docWrapper addRegularFileWithContents:textData
```

```
                                  preferredFilename:TextFileName];
```

```
    }
```
Then add the metadata wrapper, but again, only if it is missing. This is another regular file wrapper, but in this case it is populated with the serialized property list data:

    if (fileWrappers[MetaDataFileName] == nil) {
```
        NSError *plistError = nil;
```

```
        NSData *propertyListData = [NSPropertyListSerialization
```

```
                                     dataWithPropertyList:self.metaDataDict
```

```
                                                   format:NSPropertyListXMLFormat_v1_0
```

```
                                                  options:0
```

```
                                                    error:&plistError];
```

```
 
```

```
        if (propertyListData == nil || plistError != nil) {
```

```
            error = <# an NSError #>;
```

```
        } else {
```

```
            [self.docWrapper addRegularFileWithContents:propertyListData
```

```
                                      preferredFilename:MetaDataFileName];
```

```
        }
```

```
 
```

```
    }
```
Finally, return the complete document wrapper to be written recursively to disk by the system, unless there was an error. In that case, return `nil`, and set `outError` accordingly:

```
    if (error) {
```

```
        if (outError) {
```

```
            *outError = error;
```

```
        }
```

```
        return nil;
```

```
    } else {
```

```
        return self.docWrapper;
```

```
    }
```

```
 
```

```
} // end of fileWrapperOfType
```
## Using File Wrappers Directly
When opening a package in a document-based app, the system prompts the user for a file system object of the appropriate document type using an Open panel, initializes a top-level file wrapper with it, and delivers it to your app. Similarly, when saving a new package, the system prompts the user for a location in the file system using a Save panel, and writes your file wrapper to disk. You get all of this functionality (plus file coordination, undo support, and other features) for free.

However, in some cases, you don’t need the full Cocoa document architecture. Rather than managing many individual user documents, your app might instead keep a package in its `Application Support` directory, hidden from the user, to hold a single app-wide library that represents a collection of some type. Examples of this kind of app might be a mail reader or a photo organizer. You can still benefit from using file wrappers in a case like this. You just have to do a little more work yourself.

### Reading a File Wrapper
When you open a package directly, you create a wrapper and initialize it using the `[initWithURL:options:error:](https://developer.apple.com/documentation/foundation/filewrapper/1415658-init)` method, which automatically assigns the correct type (regular file, symbolic link, or directory) based upon the object found in the file system. In this case, your app supplies an [NSURL](https://developer.apple.com/documentation/foundation/nsurl) object that represents the file’s location. Use the `NSFileWrapperReadingImmediate` option to ensure that any issues are detected and reported right away. Otherwise, because read operations are done lazily, the error from a read may not appear until writing time.

    NSError *error;
```
    NSURL *fileURL = <# a URL #>;
```

```
    NSFileWrapper *docWrapper = [[NSFileWrapper alloc] initWithURL:fileURL
```

```
                                                           options:NSFileWrapperReadingImmediate
```

```
                                                             error:&error];
```

```
    if (docWrapper == nil) {
```

```
        // Handle the read error
```

```
    } else {
```

```
       // Read package components
```

```
    }
```
If the package is private to your app, you might rely on it having the expected structure, in which case you read its components into your data model as in the document-based example. However, if the package is from an unreliable source, you might want to verify that the top-level file wrapper is in fact a directory, using the `[directory](https://developer.apple.com/documentation/foundation/nsfilewrapper/1409030-directory)` property (or the convenience method `isDirectory`), and that it has the proper structure. The following code shows how to read a package more suspiciously in a case where it should contain exactly one text file:

```
    // Read package components (suspiciously)
```

```
    if (![docWrapper isDirectory]) {
```

```
        // Handle not-a-package error
```

```
 
```

```
    } else {
```

```
        NSDictionary* fileWrappers = [docWrapper fileWrappers];
```

```
 
```

```
        NSFileWrapper* textWrapper = fileWrappers[TextFileName];
```

```
        if (([fileWrappers count] != 1) || (textWrapper == nil) || ![textWrapper isRegularFile])
```

```
        {
```

```
            // Handle corrupt-package error
```

```
 
```

```
        } else {
```

```
            NSData *textData = [textWrapper regularFileContents];
```

```
            self.text = [[NSString alloc] initWithData:textData
```

```
                                              encoding:NSUTF8StringEncoding];
```

```
        }
```

```
    }
```
### Writing a File Wrapper
When you write a file wrapper to disk directly, you use the `[writeToURL:options:originalContentsURL:error:](https://developer.apple.com/documentation/foundation/nsfilewrapper/1415981-writetourl)` method. This method recursively writes the directory wrapper and all its sub-wrappers to the file system at the location specified by the `url` parameter. To create a new copy of a document on disk (for example, to implement a Save As operation), you set the `originalContentsURL` parameter to `nil`. You also test the return value for success, handling any error on failure.

    BOOL success = [self.docWrapper writeToURL:url
```
                                       options:0
```

```
                           originalContentsURL:nil
```

```
                                         error:&error];
```

```
    if (!success) {
```

```
        // Inspect the error
```

```
    }
```
When you overwrite a file package with new content, performing a save-in-place, you optimize the operation by setting the `originalContentsURL` parameter to be the same as the `url`, and including both the `NSFileWrapperWritingAtomic` and `NSFileWrapperWritingWithNameUpdating` options:

    BOOL success = [self.docWrapper writeToURL:url
```
                                       options:NSFileWrapperWritingAtomic |
```

```
                                               NSFileWrapperWritingWithNameUpdating
```

```
                           originalContentsURL:url
```

```
                                         error:&error];
```

```
    if (!success) {
```

```
        // Inspect the error
```

```
    }
```
When you use `NSFileWrapperWritingAtomic`, `NSFileWrapper` ensures that the document is written out entirely or not at all. It does this by creating a new version in a temporary location, and then replacing the original in a single step at the end. When you specify an `originalContentsURL` parameter, while creating the temporary copy, `NSFileWrapper` compares the attributes of each regular file subitem in the document wrapper to the attributes of the items on disk at the location given by `originalContentsURL`. `NSFileWrapper` writes out new content when necessary, but makes a hard link in the file system to the corresponding original content if possible, thus reducing the amount of work required for items that have not changed. Finally, the `NSFileWrapperWritingWithNameUpdating` option ensures that filenames of subitems are kept up to date in the document wrapper, so that subsequent save-in-place operations reliably carry out the same optimization.

After you read or store a file wrapper, use the `[matchesContentsOfURL:](https://developer.apple.com/documentation/foundation/nsfilewrapper/1408360-matchescontentsofurl)` method when you need to determine whether file system representation has changed, based on the file attributes stored the last time the file was read or written. If the file wrapper’s modification time or access permissions are different from those of the file on disk, this method returns NO. You can then use `[readFromURL:options:error:](https://developer.apple.com/documentation/foundation/filewrapper/1411645-read)` to reread the file from disk, or take some other action appropriate for your app. 

### File Coordination
Document-based apps use file coordination automatically, and your app does not need to do anything special to adopt it. If your app is not document-based, reading and writing packages directly, but does so only inside its own `Application Support`, `Cache`, or temporary directories, file coordination is typically unnecessary.

On the other hand, if your app manages file wrappers directly, and does so in a user accessible area of the file system, you use file coordination. This means that:

The object managing your document wrapper adopts the `[NSFilePresenter](https://developer.apple.com/documentation/foundation/nsfilepresenter)` protocol, making it a file presenter. By implementing the protocol, your file presenter indicates the URL of the package that it is interested in managing, and responds appropriately to messages regarding actions taken by other entities on the contents of the package.

> **Note:** You only specify the top-level file wrapper for file coordination. It is not necessary to mange each file or directory individually.

When you initialize your file presenter, you register it with the system, using class methods of `[NSFileCoordinator](https://developer.apple.com/documentation/foundation/nsfilecoordinator)`.

You inform the system of your intention to read, write, or move the document you are managing, and only perform these operations with the help of an `NSFileCoordinator` object that you create.

Just before you deallocate your file presenter, you unregister it, again using class methods of `NSFileCoordinator`.

For more details on file coordination, read [The Role of File Coordinators and Presenters](../FileCoordinators/FileCoordinators.html#//apple_ref/doc/uid/TP40010672-CH11-SW1).

## Accessing File Wrapper Identities
The examples throughout this chapter rely on using the preferred file name as both a key to look up the file wrapper in the `[fileWrappers](https://developer.apple.com/documentation/foundation/filewrapper/1409437-filewrappers)` dictionary of the parent file wrapper, and the name of the file in the file system. In many situations, this is sufficient.

However, strictly speaking, a file wrapper held in a directory file wrapper has three different identifiers:

**Preferred filename.** You supply this identifier when you create the wrapper. It doesn’t uniquely identify the wrapper, but the other identifiers are based on it. It is accessible through the `[preferredFilename](https://developer.apple.com/documentation/foundation/filewrapper/1409368-preferredfilename)` property.

**Dictionary key.** The system uses the preferred filename as the dictionary key when you add a file wrapper to a directory wrapper, and there are no other file wrappers with the same preferred filename in the same directory wrapper. Otherwise the system creates the key by adding a unique prefix to the preferred name. Note that a file wrapper can have a different dictionary key for each directory wrapper that contains it. To obtain the key for a file wrapper, use the `[keyForFileWrapper:](https://developer.apple.com/documentation/foundation/nsfilewrapper/1407541-keyforfilewrapper)` method on the directory wrapper that contains it.

**Filename.** This identifier is based on the preferred filename, but isn’t necessarily the same as it or the dictionary key. Note that the filename may change whenever you save a directory wrapper containing the file wrapper, particularly if the file wrapper has been added to several different directory wrappers. Thus, you should always retrieve the filename from the file wrapper’s `[filename](https://developer.apple.com/documentation/foundation/nsfilewrapper/1416684-filename)` property each time you need it rather than caching it.

## Transmitting File Wrappers
In addition to storing a file wrapper to disk, you can also transmit it to another process using the pasteboard. To do this, you use the `[serializedRepresentation](https://developer.apple.com/documentation/foundation/filewrapper/1412119-serializedrepresentation)` method to get an `NSData` object containing the file wrapper’s contents in the `NSFileContentsPboardType` format:

NSData *serializedWrapper = [fileWrapper serializedRepresentation];The recipient of the representation then reconstitutes the file wrapper using the `[initWithSerializedRepresentation:](https://developer.apple.com/documentation/foundation/filewrapper/1407515-init)` method:

```
NSFileWrapper *fileWrapper = [[NSFileWrapper] alloc]
```

```
                   initWithSerializedRepresentation:serializedWrapper];
```
## Defining Your File Wrapper as a Document Package
By default, the system does not recognize document packages as a single entity, instead treating them as regular directories. To overcome this, you export a properly formatted UTI for your document. This ensures that your file wrapper is treated as a single package. For more information, see *[Bundle Programming Guide](../../../../CoreFoundation/Conceptual/CFBundles/Introduction/Introduction.html#//apple_ref/doc/uid/10000123i)*.

        
            [Next](../PerformanceTips/PerformanceTips.html)[Previous](../TechniquesforReadingandWritingCustomFiles/TechniquesforReadingandWritingCustomFiles.html)
        
         Copyright &#x00a9; 2018 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2018-04-09