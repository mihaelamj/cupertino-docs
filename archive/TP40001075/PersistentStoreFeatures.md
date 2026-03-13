---
title: "Persistent Store Types and Behaviors"
book: "Core Data Programming Guide"
chapterId: "TP40001075-CH23"
date: "2017-03-27"
description: "Explains how to manage objects using the Core Data framework."
identifier: "//apple_ref/doc/uid/TP40001075"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Persistent Store Types and Behaviors

> **Core Data Programming Guide**
> Last updated: 2017-03-27

## Persistent Store Types and Behaviors

  
  	
  		
  Core Data provides an in-memory persistent store and three disk-based persistent stores, as described in Table 16-1. The binary store (`[NSBinaryStoreType](https://developer.apple.com/documentation/coredata/nsbinarystoretype)`) is an atomic store, as is the XML store (`[NSXMLStoreType](https://developer.apple.com/documentation/coredata/nsxmlstoretype)`). You can also create custom store types, atomic and incremental. See *[Atomic Store Programming Topics](../AtomicStore_Concepts/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004521)* and *[Incremental Store Programming Guide](../../../DataManagement/Conceptual/IncrementalStorePG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010706)*.

  
  
> 
    Note
    
    	The XML store is not available in iOS.
    	
    
  

  
  
    **Table 16-1**Built-in persistent store types
    
        
            
  Store type

            
  Speed

            
  Object graph in memory

            
  Other factors

        
    
    
        
            
  XML (atomic)

            
  Slow

            
  Whole

            
  Externally parsable

        
        
            
  Binary (atomic)

            
  Fast

            
  Whole

            
  N/A

        
        
            
  SQLite

            
  Fast

            
  Partial

            
  N/A

        
        
            
  In-memory

            
  Fast

            
  Whole

            
  No on-disk storage required

        
    
  

  
  
> 
    Important
    
    Although Core Data supports SQLite as a store type, the store format—like those of the other native Core Data stores—is private. You cannot create a SQLite database using the native SQLite API and use it directly with Core Data, nor should you manipulate an existing Core Data SQLite store using native SQLite API. If you have an existing SQLite database, you need to import it into a Core Data store.
    
    
  

  Given the abstraction that Core Data offers, there is typically no need to use the same store throughout the development process. It is common, for example, to use the XML store early in a project life cycle, because it is fairly human-readable and you can inspect a file to determine whether or not it contains the data you expect. In a deployed application that uses a large data set, you typically use an SQLite store because it offers high performance and does not require that the entire object graph reside in memory. You might use the binary store if you want store writes to be atomic.

		 

  
  
  ### Limitations of Persistent Store Security

  
  Core Data makes no guarantees regarding the security of persistent stores from untrusted sources (as opposed to stores generated internally) and cannot detect whether files have been maliciously modified. The SQLite store offers slightly better security than the XML and binary stores, but it should not be considered inherently secure. Note also that it is possible for data archived in the metadata to be tampered with independently of the store data. To ensure data security, use a technology such as an encrypted disk image.

  

  
  ### Fetch Predicates and Sort Descriptors

  
  Fetching differs somewhat according to the type of store. In the XML, binary, and in-memory stores, evaluation of the predicate and sort descriptors is performed in Objective-C with access to all Cocoa functionality, including the comparison methods on `[NSString](../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)`. 

  The SQLite store, on the other hand, compiles the predicate and sort descriptors to SQL and evaluates the result in the database itself. This is done primarily for performance, but it means that evaluation happens in a non-Cocoa environment, and so sort descriptors (or predicates) that rely on Cocoa cannot work. The supported sort selectors for SQLite are `[compare:](https://developer.apple.com/documentation/healthkit/hkquantity/1615160-compare)` and `[caseInsensitiveCompare:](../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/caseInsensitiveCompare:)`, `[localizedCompare:](https://developer.apple.com/documentation/foundation/nsstring/1416999-localizedcompare)`, `[localizedCaseInsensitiveCompare:](https://developer.apple.com/documentation/foundation/nsstring/1417333-localizedcaseinsensitivecompare)`, and `[localizedStandardCompare:](https://developer.apple.com/documentation/foundation/nsstring/1409742-localizedstandardcompare)`. The latter is Finder-like sorting and what most people should use most of the time. In addition, you cannot sort on transient properties using the SQLite store.

  There are additional constraints on the predicates you can use with the SQLite store:

  
  You cannot necessarily translate arbitrary SQL queries into predicates.

  You can have only one to-many element in a key path in a predicate.

  For example, no `toOne.toMany.toMany` or `toMany.toOne.toMany` type constructions (they evaluate to sets of sets) are allowed. As a consequence, in any predicate sent to the SQL store, there may be only one operator (and one instance of that operator) from `ALL`, `ANY`, and `IN`.

  CoreData supports a `noindex:` that can be used to drop indices in queries passed to SQLite. This is done primarily for performance reasons: SQLite uses a limited number of indices per query, and noindex: allows the user to preferentially specify which indexes should not be used. See `[NSPredicate](https://developer.apple.com/documentation/foundation/nspredicate)` documentation regarding function expressions.

  

  
  ### SQLite-Supported File Systems

  
  The SQLite store supports reading data from a file that resides on any type of file system. However, the SQLite store does not generally support writing directly to file systems that do not implement byte-range locking. For DOS file systems and for some NFS file system implementations that do not support byte-range locking correctly, SQLite uses `<dbfile>.lock` locking, and for SMB file systems it uses flock-style (file level) locking.

  In summary, byte-range locking file systems have the best concurrent read/write support; these include HFS+, AFP, and NFS. File systems with simple file locking are also supported but do not allow for as much concurrent read/write access by multiple processes. Simple file locking systems include SMB and DOS. The SQLite store does *not* support writing to WebDAV file systems.

  

  
  ### SQLite File Size and Record Deletion

  
  Simply deleting a record from an SQLite store does not necessarily result in a reduction in the size of the file. If enough items are removed to free up a page in the database file, SQLite’s automatic database vacuuming reduces the size of the file as it rearranges the data to remove that page. Similarly, the file size is reduced if you remove an item that itself occupies multiple pages (such as a thumbnail image).

  An SQLite file is organized as a collection of pages. The data within those pages is managed through B-trees, not as simple fixed-length records. This format is more efficient for searching and for overall storage, because it allows SQLite to optimize how it stores both data and indexes in a single file. This format is also the foundation of SQLite’s data integrity (transaction and journaling) mechanism. However, the cost of this design is that some delete operations may leave holes in the file and impact read and write performance. If you delete some data and add other data, the holes left by the deleted data may be filled by the added data, or the file may be vacuumed to compact its data, whichever SQLite considers most appropriate based on the operations you’re performing.

  

  
  ### Configuring Save Behavior for an SQLite Store

  
  When Core Data saves an SQLite store, SQLite updates just part of the store file. Loss of that partial update would be catastrophic, so ensure that the file is written correctly before your application continues. Unfortunately, doing partial file updates means that in some situations saving even a small set of changes to an SQLite store can take considerably longer than saving to, say, an XML store. For example, where saving to an XML file might take less than a hundredth of a second, saving to a SQLite store may take almost half a second. This data loss risk is not an issue for XML or binary stores. Because writes to these stores are typically atomic, it is less likely that data loss involves corruption of the file, and the old file is not deleted until the new one has been successfully written.

  
  
> 
    Important
    
    	In macOS the `fsync` command does not guarantee that bytes are written, so SQLite sends a `F_FULLFSYNC` request to the kernel to ensure that the bytes are actually written through to the drive platter. This request causes the kernel to flush all buffers to the drives and causes the drives to flush their track caches. Without this, there is a significantly large window of time within which data will reside in volatile memory. If system failure occurs, you risk data corruption. 
    	
    
  

  

  
  ### Changing a Store’s Type and Location

  
  You can migrate a store from one type or location to another (for example, for a Save As operation) using the `[NSPersistentStoreCoordinator](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator)` method `[migratePersistentStore:toURL:options:withType:error:](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator/1468927-migratepersistentstore)`. After invocation of this method, the original store is removed from the coordinator; thus the persistent store is no longer a useful reference. The method is illustrated in the following code fragment, which shows how you can migrate a store from one location to another. If the old store type is XML, the example also converts the store to SQLite.

  
  
      
          Objective-C

        

            NSPersistentStoreCoordinator *psc = [[self managedObjectContext] persistentStoreCoordinator];

            NSURL *oldURL = <#URL identifying the location of the current store#>;

            NSURL *newURL = <#URL identifying the location of the new store#>;

            NSError *error = nil;

            NSPersistentStore *xmlStore = [psc persistentStoreForURL:oldURL];

            NSPersistentStore *sqLiteStore = [psc migratePersistentStore:xmlStore

                toURL:newURL

                options:nil

                withType:NSSQLiteStoreType

                error:&error];

        

      
      
          Swift

        

            - `let psc = self.persistentContainer.persistentStoreCoordinator`

            - `let oldURL = URL(fileURLWithPath: "<#oldURL#>")`

            - `let newURL = URL(fileURLWithPath: "<#newURL#>")`

            - `guard let oldStore = psc.persistentStore(for: oldURL) else {`

            - `    fatalError("Failed to reference old store")`

            - `}`

            - `do {`

            - `    try psc.migratePersistentStore(oldStore, to: newURL, options:nil, withType:NSSQLiteStoreType)`

            - `} catch {`

            - `    fatalError("Failed to migrate store: \(error)")`

            - `}`

            - `}`

        

      
  

  To migrate a store, Core Data:

  
  Creates a temporary persistence stack.

  Mounts the old and new stores.

  Loads all objects from the old store.

  Migrates the objects to the new store.

  The objects are given temporary IDs, then assigned to the new store. The new store then saves the newly assigned objects (committing them to the external repository).

  Informs other stacks that the object IDs have changed (from the old to the new stores), which keeps the stack running after a migration.

  Unmounts the old store.

  Returns the new store.

  An error can occur if:

  
  You provide invalid parameters to the method

  Core Data cannot add the new store

  Core Data cannot remove the old store

  In the latter two cases, you get the same errors that you would get if you called `addPersistentStore:` or `removePersistentStore:` directly. If an error occurs when the store is being added or removed, treat this as an exception, because the persistence stack is likely to be in an inconsistent state.

  If a failure occurs during the migration itself, instead of an error you get an exception. In these cases, Core Data unwinds cleanly and there should be no repair work necessary. You can examine the exception description to determine what went wrong—possible errors range widely from "disk is full" and "permissions problems" to "The SQLite store became corrupted" and "Core Data does not support cross store relationships."

  

  
  ### Associating Metadata with a Store

  
  A store’s *metadata* provides additional information about the store that is not directly associated with any of the entities in the store.

  The metadata is represented by a dictionary. Core Data automatically sets key-value pairs to indicate the store type and its UUID. You can create additional custom keys for your application or provide a standard set of keys such as `kMDItemKeywords` to support Spotlight indexing (if you also write a suitable importer).

  Be careful about what information you put into metadata. Spotlight imposes a limit to the size of metadata, and replicating an entire document in metadata is probably not useful. However, if you create a URL to identify a particular object in a store (using `[URIRepresentation](https://developer.apple.com/documentation/coredata/nsmanagedobjectid/1391689-urirepresentation)`), the URL may be useful to include as metadata.

  
  
  ### Getting the Metadata

  
  There are two ways to get the metadata for a store:

  
  Given an instance of a persistent store, get its metadata using the `[NSPersistentStoreCoordinator](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator)` instance method `[metadataForPersistentStore:](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator/1468911-metadata)`. 

  Retrieve metadata from a store without the overhead of creating a persistence stack by using the `[NSPersistentStoreCoordinator](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator)` class method, `[metadataForPersistentStoreOfType:URL:error:](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator/1468804-metadataforpersistentstore)`.

  There is an important difference between these approaches. The instance method, `metadataForPersistentStore:`, returns the metadata as it currently is in your program, including any changes that may have been made since the store was last saved. The class method, `metadataForPersistentStoreOfType:URL:error:`, returns the metadata as it is currently represented in the store itself. If there are pending changes to the store, the returned value may therefore be out of sync.

  

  
  ### Setting the Metadata

  
  There are two ways you can set the metadata for a store:

  
  Given an instance of a persistent store, set its metadata using the `[NSPersistentStoreCoordinator](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator)` instance method, `[setMetadata:forPersistentStore:](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator/1468899-setmetadata)`.

  Set the metadata without the overhead of creating a persistence stack by using the `[NSPersistentStoreCoordinator](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator)` class method, `[setMetadata:forPersistentStoreOfType:URL:error:](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator/1468897-setmetadata)`. 

  There is again an important difference between these approaches. If you use `setMetadata:forPersistentStore:`, you must save the store (through a managed object context) before the new metadata is saved. If you use `setMetadata:forPersistentStoreOfType:URL:error:`, however, the metadata is updated immediately, and the last-modified date of the file is changed. 

  This difference has particular implications if you use `[NSPersistentDocument](https://developer.apple.com/documentation/appkit/nspersistentdocument)` in macOS. If you update the metadata using `setMetadata:forPersistentStoreOfType:URL:error:` while you are actively working on the persistent store (that is, while there are unsaved changes), when you save the document you will see a warning, “This document's file has been changed by another application since you opened or saved it.” To avoid this, instead use `setMetadata:forPersistentStore:`. To find the document’s persistent store, you typically ask the persistent store coordinator for its persistent stores (`[persistentStores](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator/1468790-persistentstores)`) and use the first item in the returned array to set the metadata. When you use `setMetadata:forPersistentStoreOfType:URL:error:`, the action is treated as if the change occurred externally to your Core Data stack.

  Because Core Data manages the values for `[NSStoreTypeKey](https://developer.apple.com/documentation/coredata/nsstoretypekey)` and `[NSStoreUUIDKey](https://developer.apple.com/documentation/coredata/nsstoreuuidkey)` in the same metadata, make a mutable copy of any existing metadata before setting your own keys and values, as illustrated in the following code fragment:

  
  
      
          Objective-C

        

            NSURL *url = [NSURL fileURLWithPath:@"url to store"];

            NSPersistentStore *store = [self.managedObjectContext.persistentStoreCoordinator persistentStoreForURL:url];

            NSMutableDictionary *metadata = [[store metadata] mutableCopy];

            metadata[@"MyKeyWord"] = @"MyStoredValue";

            [store setMetadata:metadata];

        

      
      
          Swift

        

            - `let url = NSURL(fileURLWithPath: "url to store")`

            - `let psc = self.persistentContainer.persistentStoreCoordinator`

            - `guard let store = psc.persistentStore(for: url) else {`

            - `    fatalError("Failed to retrieve store from \(url)")`

            - `}`

            - `var metadata = store.metadata`

            - `metadata["MyKeyWord"] = "MyStoredValue"`

            - `store.metadata = metadata`

        

      
  

  

  	
 	
    		[Change Management](ChangeManagement.html#//apple_ref/doc/uid/TP40001075-CH22-SW1)

  			[Concurrency](Concurrency.html#//apple_ref/doc/uid/TP40001075-CH24-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2017-03-27](RevisionHistory.html#//apple_ref/doc/uid/TP40003204-SW1)