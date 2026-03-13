---
title: "Appendix A: Thread Safety Summary"
book: "Threading Programming Guide"
chapterId: "10000057i-CH12"
date: "2014-07-15"
description: "Explains how to use threads in Cocoa applications."
identifier: "//apple_ref/doc/uid/10000057i"
source: apple-archive
---

# Appendix A: Thread Safety Summary

> **Threading Programming Guide**
> Last updated: 2014-07-15

[Next](../Glossary/Glossary.html)[Previous](../ThreadSafety/ThreadSafety.html)
        
        
        [](../index.html)
        
        # Thread Safety Summary
This appendix describes the high-level thread safety of some key frameworks in OS X and iOS. The information in this appendix is subject to change. 

## Cocoa
Guidelines for using Cocoa from multiple threads include the following:

Immutable objects are generally thread-safe. Once you create them, you can safely pass these objects to and from threads. On the other hand, mutable objects are generally not thread-safe. To use mutable objects in a threaded application, the application must synchronize appropriately. For more information, see [Mutable Versus Immutable](#//apple_ref/doc/uid/20000736-126010). 

Many objects deemed “thread-unsafe” are only unsafe to use from multiple threads. Many of these objects can be used from any thread as long as it is only one thread at a time. Objects that are specifically restricted to the main thread of an application are called out as such.

The main thread of the application is responsible for handling events. Although the Application Kit continues to work if other threads are involved in the event path, operations can occur out of sequence.

If you want to use a thread to draw to a view, bracket all drawing code between the `[lockFocusIfCanDraw](https://developer.apple.com/documentation/appkit/nsview/1483285-lockfocusifcandraw)` and `[unlockFocus](https://developer.apple.com/documentation/appkit/nsview/1483711-unlockfocus)` methods of `[NSView](https://developer.apple.com/documentation/appkit/nsview)`.

To use POSIX threads with Cocoa, you must first put Cocoa into multithreaded mode. For more information, see [Using POSIX Threads in a Cocoa Application](../CreatingThreads/CreatingThreads.html#//apple_ref/doc/uid/20000738-125024). 

### Foundation Framework Thread Safety
There is a misconception that the Foundation framework is thread-safe and the Application Kit framework is not. Unfortunately, this is a gross generalization and somewhat misleading. Each framework has areas that are thread-safe and areas that are not thread-safe. The following sections describe the general thread safety of the Foundation framework. 

#### Thread-Safe Classes and Functions
The following classes and functions are generally considered to be thread-safe. You can use the same instance from multiple threads without first acquiring a lock. 

`[NSArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSArray)`

`[NSAssertionHandler](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAssertionHandler/Description.html#//apple_ref/occ/cl/NSAssertionHandler)`

`[NSAttributedString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAttributedStrngClstr/Description.html#//apple_ref/occ/cl/NSAttributedString)`

`[NSBundle](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/cl/NSBundle)`

`[NSCalendar](https://developer.apple.com/documentation/foundation/nscalendar)`

`[NSCalendarDate](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSCalendarDate/Description.html#//apple_ref/occ/cl/NSCalendarDate)`

`[NSCharacterSet](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSCharacterSetClstr/Description.html#//apple_ref/occ/cl/NSCharacterSet)`

`[NSConditionLock](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSConditionLock/Description.html#//apple_ref/occ/cl/NSConditionLock)`

`[NSConnection](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSConnection/Description.html#//apple_ref/occ/cl/NSConnection)`

`[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)`

`[NSDate](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateClassCluster/Description.html#//apple_ref/occ/cl/NSDate)`

`[NSDateFormatter](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateFormatter/Description.html#//apple_ref/occ/cl/NSDateFormatter)`

`NSDecimal` functions

`[NSDecimalNumber](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDecimalNumber/Description.html#//apple_ref/occ/cl/NSDecimalNumber)`

`[NSDecimalNumberHandler](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDecimalNumberHandler/Description.html#//apple_ref/occ/cl/NSDecimalNumberHandler)`

`[NSDeserializer](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDeserializer/Description.html#//apple_ref/occ/cl/NSDeserializer)`

`[NSDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSDictionary)`

`[NSDistantObject](https://developer.apple.com/documentation/foundation/nsdistantobject)`

`[NSDistributedLock](https://developer.apple.com/documentation/foundation/nsdistributedlock)`

`[NSDistributedNotificationCenter](https://developer.apple.com/documentation/foundation/nsdistributednotificationcenter)`

`[NSException](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/cl/NSException)`

`[NSFileManager](https://developer.apple.com/documentation/foundation/filemanager)`

`[NSFormatter](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSFormatter/Description.html#//apple_ref/occ/cl/NSFormatter)`

`[NSHost](https://developer.apple.com/documentation/foundation/host)`

`[NSJSONSerialization](https://developer.apple.com/documentation/foundation/nsjsonserialization)`

`[NSLock](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSLock/Description.html#//apple_ref/occ/cl/NSLock)`

`[NSLog](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSLog)`/`[NSLogv](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSLogv)`

`[NSMethodSignature](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSMethodSignature/Description.html#//apple_ref/occ/cl/NSMethodSignature)`

`[NSNotification](https://developer.apple.com/documentation/foundation/nsnotification)`

`[NSNotificationCenter](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNotificationCenter/Description.html#//apple_ref/occ/cl/NSNotificationCenter)`

`[NSNumber](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumber/Description.html#//apple_ref/occ/cl/NSNumber)`

`[NSNumberFormatter](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumberFormatter/Description.html#//apple_ref/occ/cl/NSNumberFormatter)`

`[NSObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/cl/NSObject)`

`[NSOrderedSet](https://developer.apple.com/documentation/foundation/nsorderedset)`

`[NSPortCoder](https://developer.apple.com/documentation/foundation/nsportcoder)`

`[NSPortMessage](https://developer.apple.com/documentation/foundation/nsportmessage)`

`[NSPortNameServer](https://developer.apple.com/documentation/foundation/nsportnameserver)`

`[NSProgress](https://developer.apple.com/documentation/foundation/nsprogress)`

`[NSProtocolChecker](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSProtocolChecker/Description.html#//apple_ref/occ/cl/NSProtocolChecker)`

`[NSProxy](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSProxy/Description.html#//apple_ref/occ/cl/NSProxy)`

`[NSRecursiveLock](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRecursiveLock/Description.html#//apple_ref/occ/cl/NSRecursiveLock)`

`[NSSet](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSSetClassCluster/Description.html#//apple_ref/occ/cl/NSSet)`

`[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)`

`[NSThread](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/cl/NSThread)`

`[NSTimer](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTimer/Description.html#//apple_ref/occ/cl/NSTimer)`

`[NSTimeZone](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTimeZoneClassCluster/Description.html#//apple_ref/occ/cl/NSTimeZone)`

`[NSUserDefaults](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSUserDefaults/Description.html#//apple_ref/occ/cl/NSUserDefaults)`

`[NSValue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSValue/Description.html#//apple_ref/occ/cl/NSValue)`

`[NSXMLParser](https://developer.apple.com/documentation/foundation/xmlparser)`

Object allocation and retain count functions

Zone and memory functions

#### Thread-Unsafe Classes
The following classes and functions are generally not thread-safe. In most cases, you can use these classes from any thread as long as you use them from only one thread at a time. Check the class documentation for additional details.

`[NSArchiver](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArchiver/Description.html#//apple_ref/occ/cl/NSArchiver)`

`[NSAutoreleasePool](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAutoreleasePool/Description.html#//apple_ref/occ/cl/NSAutoreleasePool)`

`[NSCoder](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSCoder/Description.html#//apple_ref/occ/cl/NSCoder)`

`[NSCountedSet](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSCountedSet/Description.html#//apple_ref/occ/cl/NSCountedSet)`

`[NSEnumerator](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSEnumerator/Description.html#//apple_ref/occ/cl/NSEnumerator)`

`[NSFileHandle](https://developer.apple.com/documentation/foundation/filehandle)`

`NSHashTable` functions

`[NSInvocation](https://developer.apple.com/documentation/foundation/nsinvocation)`

`NSMapTable` functions

`[NSMutableArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSMutableArray)`

`[NSMutableAttributedString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAttributedStrngClstr/Description.html#//apple_ref/occ/cl/NSMutableAttributedString)`

`[NSMutableCharacterSet](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSCharacterSetClstr/Description.html#//apple_ref/occ/cl/NSMutableCharacterSet)`

`[NSMutableData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSMutableData)`

`[NSMutableDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSMutableDictionary)`

`[NSMutableOrderedSet](https://developer.apple.com/documentation/foundation/nsmutableorderedset)`

`[NSMutableSet](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSSetClassCluster/Description.html#//apple_ref/occ/cl/NSMutableSet)`

`[NSMutableString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSMutableString)`

`[NSNotificationQueue](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNotificationQueue/Description.html#//apple_ref/occ/cl/NSNotificationQueue)`

`[NSPipe](https://developer.apple.com/documentation/foundation/nspipe)`

`[NSPort](https://developer.apple.com/documentation/foundation/nsport)`

`[NSProcessInfo](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSProcessInfo/Description.html#//apple_ref/occ/cl/NSProcessInfo)`

`[NSRunLoop](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRunLoop/Description.html#//apple_ref/occ/cl/NSRunLoop)`

`[NSScanner](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSScannerClassCluster/Description.html#//apple_ref/occ/cl/NSScanner)`

`[NSSerializer](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSSerializer/Description.html#//apple_ref/occ/cl/NSSerializer)`

`[NSTask](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTask/Description.html#//apple_ref/occ/cl/NSTask)`

`[NSUnarchiver](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSUnarchiver/Description.html#//apple_ref/occ/cl/NSUnarchiver)`

`[NSUndoManager](https://developer.apple.com/documentation/foundation/undomanager)`

User name and home directory functions

Note that although `NSArchiver`, `NSCoder`, and `NSEnumerator` objects are themselves thread-safe, they are listed here because it is not safe to change the data objects wrapped by them while they are in use. For example, in the case of an archiver, it is not safe to change the object graph being archived. For an enumerator, it is not safe for any thread to change the enumerated collection. 

#### Main Thread Only Classes
The following class must be used only from the main thread of an application.

`[NSAppleScript](https://developer.apple.com/documentation/foundation/nsapplescript)`

#### Mutable Versus Immutable
Immutable objects are generally thread-safe; once you create them, you can safely pass these objects to and from threads. Of course, when using immutable objects, you still need to remember to use reference counts correctly. If you inappropriately release an object you did not retain, you could cause an exception later. 

Mutable objects are generally not thread-safe. To use mutable objects in a threaded application, the application must synchronize access to them using locks. (For more information, see [Atomic Operations](../ThreadSafety/ThreadSafety.html#//apple_ref/doc/uid/10000057i-CH8-SW2)). In general, the collection classes (for example, `[NSMutableArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSMutableArray)`, `[NSMutableDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSMutableDictionary)`) are not thread-safe when mutations are concerned. That is, if one or more threads are changing the same array, problems can occur. You must lock around spots where reads and writes occur to assure thread safety.

Even if a method claims to return an immutable object, you should never simply assume the returned object is immutable. Depending on the method implementation, the returned object might be mutable or immutable. For example, a method with the return type of `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` might actually return an `[NSMutableString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSMutableString)` due to its implementation. If you want to guarantee that the object you have is immutable, you should make an immutable copy. 

#### Reentrancy
Reentrancy is only possible where operations “call out” to other operations in the same object or on different objects. Retaining and releasing objects is one such “call out” that is sometimes overlooked.

The following table lists the portions of the Foundation framework that are explicitly reentrant. All other classes may or may not be reentrant, or they may be made reentrant in the future. A complete analysis for reentrancy has never been done and this list may not be exhaustive.

Distributed Objects

`[NSConditionLock](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSConditionLock/Description.html#//apple_ref/occ/cl/NSConditionLock)`

`[NSDistributedLock](https://developer.apple.com/documentation/foundation/nsdistributedlock)`

`[NSLock](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSLock/Description.html#//apple_ref/occ/cl/NSLock)`

`[NSLog](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSLog)`/`[NSLogv](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSLogv)`

`[NSNotificationCenter](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNotificationCenter/Description.html#//apple_ref/occ/cl/NSNotificationCenter)`

`[NSRecursiveLock](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRecursiveLock/Description.html#//apple_ref/occ/cl/NSRecursiveLock)`

`[NSRunLoop](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRunLoop/Description.html#//apple_ref/occ/cl/NSRunLoop)`

`[NSUserDefaults](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSUserDefaults/Description.html#//apple_ref/occ/cl/NSUserDefaults)`

#### Class Initialization
The Objective-C runtime system sends an `initialize` message to every class object before the class receives any other messages. This gives the class a chance to set up its runtime environment before it’s used. In a multithreaded application, the runtime guarantees that only one thread—the thread that happens to send the first message to the class—executes the `initialize` method. If a second thread tries to send messages to the class while the first thread is still in the `initialize` method, the second thread blocks until the `initialize` method finishes executing. Meanwhile, the first thread can continue to call other methods on the class. The `initialize` method should not rely on a second thread calling methods of the class; if it does, the two threads become deadlocked.

Due to a bug in OS X version 10.1.x and earlier, a thread could send messages to a class before another thread finished executing that class’s `initialize` method. The thread could then access values that have not been fully initialized, perhaps crashing the application. If you encounter this problem, you need to either introduce locks to prevent access to the values until after they are initialized or force the class to initialize itself before becoming multithreaded.

#### Autorelease Pools
Each thread maintains its own stack of `[NSAutoreleasePool](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAutoreleasePool/Description.html#//apple_ref/occ/cl/NSAutoreleasePool)` objects. Cocoa expects there to be an autorelease pool always available on the current thread’s stack. If a pool is not available, objects do not get released and you leak memory. An `NSAutoreleasePool` object is automatically created and destroyed in the main thread of applications based on the Application Kit, but secondary threads (and Foundation-only applications) must create their own before using Cocoa. If your thread is long-lived and potentially generates a lot of autoreleased objects, you should periodically destroy and create autorelease pools (like the Application Kit does on the main thread); otherwise, autoreleased objects accumulate and your memory footprint grows. If your detached thread does not use Cocoa, you do not need to create an autorelease pool.

#### Run Loops
Every thread has one and only one run loop. Each run loop, and hence each thread, however, has its own set of input modes that determine which input sources are listened to when the run loop is run. The input modes defined in one run loop do not affect the input modes defined in another run loop, even though they may have the same name.

The run loop for the main thread is automatically run if your application is based on the Application Kit, but secondary threads (and Foundation-only applications) must run the run loop themselves. If a detached thread does not enter the run loop, the thread exits as soon as the detached method finishes executing.

Despite some outward appearances, the `[NSRunLoop](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRunLoop/Description.html#//apple_ref/occ/cl/NSRunLoop)` class is not thread safe. You should call the instance methods of this class only from the thread that owns it. 

### Application Kit Framework Thread Safety
The following sections describe the general thread safety of the Application Kit framework.

#### Thread-Unsafe Classes
The following classes and functions are generally not thread-safe. In most cases, you can use these classes from any thread as long as you use them from only one thread at a time. Check the class documentation for additional details.

`[NSGraphicsContext](https://developer.apple.com/documentation/appkit/nsgraphicscontext)`. For more information, see [NSGraphicsContext Restrictions](#//apple_ref/doc/uid/10000057i-CH12-126712).

`[NSImage](https://developer.apple.com/documentation/appkit/nsimage)`. For more information, see [NSImage Restrictions](#//apple_ref/doc/uid/10000057i-CH12-126728).

`[NSResponder](https://developer.apple.com/documentation/appkit/nsresponder)`

`[NSWindow](https://developer.apple.com/documentation/appkit/nswindow)` and all of its descendants. For more information, see [Window Restrictions](#//apple_ref/doc/uid/10000057i-CH12-123364).

#### Main Thread Only Classes
The following classes must be used only from the main thread of an application.

`[NSCell](https://developer.apple.com/documentation/appkit/nscell)` and all of its descendants

`[NSView](https://developer.apple.com/documentation/appkit/nsview)` and all of its descendants. For more information, see [NSView Restrictions](#//apple_ref/doc/uid/10000057i-CH12-123427).

#### Window Restrictions
You can create a window on a secondary thread. The Application Kit ensures that the data structures associated with a window are deallocated on the main thread to avoid race conditions. There is some possibility that window objects may leak in an application that deals with a lot of windows concurrently. 

You can create a modal window on a secondary thread. The Application Kit blocks the calling secondary thread while the main thread runs the modal loop.

#### Event Handling Restrictions
The main thread of the application is responsible for handling events. The main thread is the one blocked in the `run` method of `[NSApplication](https://developer.apple.com/documentation/appkit/nsapplication)`, usually invoked in an application’s `main` function. While the Application Kit continues to work if other threads are involved in the event path, operations can occur out of sequence. For example, if two different threads are responding to key events, the keys could be received out of order. By letting the main thread process events, you achieve a more consistent user experience. Once received, events can be dispatched to secondary threads for further processing if desired.

You can call the `[postEvent:atStart:](https://developer.apple.com/documentation/appkit/nsapplication/1428710-postevent)` method of `NSApplication` from a secondary thread to post an event to the main thread’s event queue. Order is not guaranteed with respect to user input events, however. The main thread of the application is still responsible for handling events in the event queue.

#### Drawing Restrictions
The Application Kit is generally thread-safe when drawing with its graphics functions and classes, including the `[NSBezierPath](https://developer.apple.com/documentation/appkit/nsbezierpath)` and `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` classes. Details for using particular classes are described in the following sections. Additional information about drawing and threads is available in *[Cocoa Drawing Guide](../../CocoaDrawingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40003290)*. 

NSView RestrictionsThe `[NSView](https://developer.apple.com/documentation/appkit/nsview)` class is generally not thread-safe. You should create, destroy, resize, move, and perform other operations on `NSView` objects only from the main thread of an application. Drawing from secondary threads is thread-safe as long as you bracket drawing calls with calls to `[lockFocusIfCanDraw](https://developer.apple.com/documentation/appkit/nsview/1483285-lockfocusifcandraw)` and `[unlockFocus](https://developer.apple.com/documentation/appkit/nsview/1483711-unlockfocus)`. 

If a secondary thread of an application wants to cause portions of the view to be redrawn on the main thread, it must not do so using methods like `display`, `[setNeedsDisplay:](https://developer.apple.com/documentation/appkit/nsview/1483360-needsdisplay)`, `[setNeedsDisplayInRect:](https://developer.apple.com/documentation/appkit/nsview/1483475-setneedsdisplayinrect)`, or `[setViewsNeedDisplay:](https://developer.apple.com/documentation/appkit/nswindow/1419609-viewsneeddisplay)`. Instead, it should send a message to the main thread or call those methods using the `[performSelectorOnMainThread:withObject:waitUntilDone:](https://developer.apple.com/documentation/objectivec/nsobject/1414900-performselector)` method instead. 

The view system’s graphics states (gstates) are per-thread. Using graphics states used to be a way to achieve better drawing performance over a single-threaded application but that is no longer true. Incorrect use of graphics states can actually lead to drawing code that is less efficient than drawing in the main thread.

NSGraphicsContext RestrictionsThe `[NSGraphicsContext](https://developer.apple.com/documentation/appkit/nsgraphicscontext)` class represents the drawing context provided by the underlying graphics system. Each `NSGraphicsContext` instance holds its own independent graphics state: coordinate system, clipping, current font, and so on. An instance of the class is automatically created on the main thread for each `[NSWindow](https://developer.apple.com/documentation/appkit/nswindow)` instance. If you do any drawing from a secondary thread, a new instance of `NSGraphicsContext` is created specifically for that thread.

If you do any drawing from a secondary thread, you must flush your drawing calls manually. Cocoa does not automatically update views with content drawn from secondary threads, so you need to call the `[flushGraphics](https://developer.apple.com/documentation/appkit/nsgraphicscontext/1527919-flushgraphics)` method of `NSGraphicsContext` when you finish your drawing. If your application draws content from the main thread only, you do not need to flush your drawing calls.

NSImage RestrictionsOne thread can create an `[NSImage](https://developer.apple.com/documentation/appkit/nsimage)` object, draw to the image buffer, and pass it off to the main thread for drawing. The underlying image cache is shared among all threads. For more information about images and how caching works, see *[Cocoa Drawing Guide](../../CocoaDrawingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40003290)*. 

### Core Data Framework
The Core Data framework generally supports threading, although there are some usage caveats that apply. For information on these caveats, see Concurrency with Core Data in *[Core Data Programming Guide](../../CoreData/index.html#//apple_ref/doc/uid/TP40001075)*.

## Core Foundation
Core Foundation is sufficiently thread-safe that, if you program with care, you should not run into any problems related to competing threads. It is thread-safe in the common cases, such as when you query, retain, release, and pass around immutable objects. Even central shared objects that might be queried from more than one thread are reliably thread-safe.

Like Cocoa, Core Foundation is not thread-safe when it comes to mutations to objects or their contents. For example, modifying a mutable data or mutable array object is not thread-safe, as you might expect, but neither is modifying an object inside of an immutable array. One reason for this is performance, which is critical in these situations. Moreover, it is usually not possible to achieve absolute thread safety at this level. You cannot rule out, for example, indeterminate behavior resulting from retaining an object obtained from a collection. The collection itself might be freed before the call to retain the contained object is made.

In those cases where Core Foundation objects are to be accessed from multiple threads and mutated, your code should protect against simultaneous access by using locks at the access points. For instance, the code that enumerates the objects of a Core Foundation array should use the appropriate locking calls around the enumerating block to protect against someone else mutating the array. 

        
            [Next](../Glossary/Glossary.html)[Previous](../ThreadSafety/ThreadSafety.html)
        
         Copyright &#x00a9; 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-07-15