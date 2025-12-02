---
title: "Adding Behavior to a Cocoa Program"
book: "Cocoa Fundamentals Guide"
chapterId: "TP40002974-CH5"
date: "2013-09-18"
description: "Introduces the basic concepts, terminology, architectures, and design patterns of the Cocoa and Cocoa Touch frameworks and development environment."
identifier: "//apple_ref/doc/uid/TP40002974"
source: apple-archive
---

# Adding Behavior to a Cocoa Program

> **Cocoa Fundamentals Guide**
> Last updated: 2013-09-18

[Next](../CocoaDesignPatterns/CocoaDesignPatterns.html)[Previous](../CocoaObjects/CocoaObjects.html)
        
        
        [](../index.html)
        
# Retired Document
**Important:**
This document may not represent best practices for current development. Links to downloads and other resources may no longer be valid.
        # Adding Behavior to a Cocoa Program
When you develop a Cocoa program in Objective-C, you won’t be on your own. You’ll be drawing on the work done by Apple and others, on the classes they’ve developed and packaged in Objective-C frameworks. These frameworks give you a set of interdependent classes that work together to structure a part—often a substantial part—of your program.

This chapter describes what it’s like to write an Objective-C program using a Cocoa framework. It also gives you the basic information you need to know to make a subclass of a framework class. 

## Starting Up
Using a framework of Objective-C classes and their methods differs from using a library of C functions. With a library of C functions, you can pretty much pick and choose which functions to use and when to use them, depending on the program you’re trying to write. A framework, on the other hand, imposes a design on your program, or at least on a certain problem space your program is trying to address. With a procedural program, you call library functions as necessary to get the work of the program done. Using an object-oriented framework is similar in that you must invoke methods of the framework to do much of the work of the program. However, you’ll also need to customize the framework and adapt it to your needs by implementing methods that the framework will invoke at the appropriate time. These methods are hooks that introduce your code into the structure imposed by the framework, augmenting it with the behavior that characterizes your program. In a sense, the usual roles of program and library are reversed. Instead of incorporating library code into your program, you incorporate your program code into the framework.

You can gain some insight on the relationship between custom code and framework by considering what takes place when a Cocoa program begins executing.

### What Happens in the main Function
Objective-C programs begin executing where C programs do, in the `main`  function. In a complex Objective-C program, the job of `main` is fairly simple. It consists of two steps:

Set up a core group of objects.

Turn program control over to those objects.

Objects in the core group might create other objects as the program runs, and those objects might create still other objects. From time to time, the program might also load classes, unarchive instances, connect to remote objects, and find other resources as they’re needed. However, all that’s required at the outset is enough structure—enough of the object network—to handle the program’s initial tasks. The `main` function puts this initial structure in place and gets it ready for the task ahead.

Typically, one of the core objects has the responsibility for overseeing the program or controlling its input. When the core structure is ready, `main` sets this overseer object to work. If the program is a command-line tool or a background server, what this entails might be as simple as passing command-line parameters or opening a remote connection. But for the most common type of Cocoa program, an application, what happens is a bit more involved.

For an application, the core group of objects that `main` sets up must include some objects that draw the user interface. This interface, or at least part of it (such as an application’s menu), must appear on the screen when the user launches the application. Once the initial user interface is on the screen, the application is thereafter driven by external events, the most important of which are those originated by users: clicking a button, choosing a menu item, dragging an icon, typing something in a field, and so on. Each such event is reported to the application along with a good deal of information about the circumstances of the user action—for example, which key was pressed, whether the mouse button was pressed or released, where the cursor was located, and which window was affected.

An application gets an event, looks at it, responds to it—often by drawing a part of the user interface—then waits for the next event. It keeps getting events, one after another, as long as the user or some other source (such as a timer) initiates them. From the time it’s launched to the time it terminates, almost everything the application does is driven by user actions in the form of events.

The mechanism for getting and responding to events is the main event loop (called *main* because an application can set up subordinate events loops for brief periods). An event loop is essentially a run loop with one or more input sources attached to it. One object in the core group is responsible for running the main event loop—getting an event, dispatching the event to the object or objects that can best handle it, and then getting the next event. In a Cocoa application, this coordinating object is the global application object. In OS X, this object is an instance of `[NSApplication](https://developer.apple.com/documentation/appkit/nsapplication)`, and in iOS, it’s an instance of `[UIApplication](https://developer.apple.com/documentation/uikit/uiapplication)`. Figure 3-1 illustrates the main event loop for Cocoa applications in OS X.

**Figure 3-1**  The main event loop in OS XThe `main` function in almost all Cocoa applications is extremely simple. In OS X, it consists of only one function call (see Listing 3-1). The `NSApplicationMain` function creates the application object, sets up an autorelease pool, loads the initial user interface from the main nib file, and runs the application, thereby requesting it to begin handling events received on the main event loop.

**Listing 3-1**  The main function for a Cocoa application in OS X

#import <AppKit/AppKit.h>
```
 
```

```
int main(int argc, const char *argv[]) {
```

```
    return NSApplicationMain(argc, argv);
```

```
}
```
The `main` function for iOS-based applications calls a similar function: `UIApplicationMain`.

## Using a Cocoa Framework
Library functions impose few restrictions on the programs that use them; you can call them whenever you need to. The methods in an object-oriented library or framework, on the other hand, are tied to class definitions and can’t be invoked unless you create or obtain an object that has access to those definitions. Moreover, in most programs the object must be connected to at least one other object so that it can operate in the program network. A class defines a program component; to access its services, you need to craft it into the structure of your application.

That said, some framework classes generate instances that behave much as a set of library functions does. You simply create an instance, initialize it, and either send it a message to accomplish a task or insert it into a waiting slot in your application. For example, you can use the `[NSFileManager](https://developer.apple.com/documentation/foundation/filemanager)` class to perform various file-system operations, such as moving, copying, and deleting files. If you need to display an alert dialog, you would create an instance of the `[NSAlert](https://developer.apple.com/documentation/appkit/nsalert)` class—or, with UIKit, an instance of the `[UIAlertView](https://developer.apple.com/documentation/uikit/uialertview)` class—and send the instance the appropriate message.

In general, however, environments such as Cocoa are more than a grab bag of individual classes that offer their services. They consist of object-oriented frameworks, collections of classes that structure a problem space and present an integrated solution to it. Instead of providing discrete services that you can use as needed (as with function libraries), a framework maps out and implements an entire program structure—or model—that your own code must adapt to. Because this program model is generic, you can specialize it to meet the requirements of your particular program. Rather than design a program that you plug library functions into, you plug your own code into the design provided by the framework.

To use a framework, you must accept the program model it defines and employ and customize as many of its classes as necessary to mold your particular program to that model. The classes are mutually dependent and come as a group, not individually. At first glance, the need to adapt your code to a framework’s program model might seem restrictive. But the reality is quite the opposite. A framework offers you many ways in which you can alter and extend its generic behavior. It simply requires you to accept that all Cocoa programs behave in the same fundamental ways because they are all based on the same program model.

### Kinds of Framework Classes
The classes in a Cocoa framework deliver their services in four ways:

*Off the shelf*. Some classes define off-the-shelf objects, ready to be used. You simply create instances of the class and initialize them as needed. For the AppKit framework, subclasses of `[NSControl](https://developer.apple.com/documentation/appkit/nscontrol)`, such as `[NSTextField](https://developer.apple.com/documentation/appkit/nstextfield)`, `[NSButton](https://developer.apple.com/documentation/appkit/nsbutton)`, and `[NSTableView](https://developer.apple.com/documentation/appkit/nstableview)`, fall into this category. The corresponding UIKit classes are `[UIControl](https://developer.apple.com/documentation/uikit/uicontrol)`, `[UITextField](https://developer.apple.com/documentation/uikit/uitextfield)`, `[UIButton](https://developer.apple.com/documentation/uikit/uibutton)`, and `[UITableView](https://developer.apple.com/documentation/uikit/uitableview)`. You typically create and initialize off-the-shelf objects using Interface Builder, although you can create and initialize them programmatically.

*Behind the scenes*. As a program runs, Cocoa creates some framework objects for it behind the scenes. You don’t need to explicitly allocate and initialize these objects; it’s done for you. Often the classes are private, but they are necessary to implement the desired behavior.

*Generically*. Some framework classes are generic. A framework might provide some concrete subclasses of the generic class that you can use unchanged. Yet you can—and *must* in some circumstances—define your own subclasses and override the implementations of certain methods. `[NSView](https://developer.apple.com/documentation/appkit/nsview)` and `[NSDocument](https://developer.apple.com/documentation/appkit/nsdocument)` are examples of this kind of class in AppKit, and `[UIView](https://developer.apple.com/documentation/uikit/uiview)` and `[UIScrollView](https://developer.apple.com/documentation/uikit/uiscrollview)` are UIKit examples.

*Through delegation and notification*. Many framework objects keep other objects informed of their actions and even delegate certain responsibilities to those other objects. The mechanisms for delivering this information are delegation and notification. A delegating object publishes an interface known as a protocol (see [Protocols](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW41) for details). Client objects must first register as delegates and then implement one or more methods of this interface. A notifying object publishes the list of notifications it broadcasts, and any client is free to observe one or more of them. Many framework classes broadcast notifications. 

Some classes provide more than one of these general kinds of services. For example, you can drag a ready-made `NSWindow` object from the Interface Builder library and use it with only minor initializations. Thus the `NSWindow` class provides off-the-shelf instances. But an `NSWindow` object also sends messages to its delegate and posts a variety of notifications. You can even subclass `NSWindow` if, for example, you want to have round windows.

It is the Cocoa classes in the last two categories—generic and delegation/notification—that offer the most possibilities for integrating your program-specific code into the structure provided by the frameworks. [Inheriting from a Cocoa Class](#//apple_ref/doc/uid/TP40002974-CH5-SW19) discusses in general terms how you create subclasses of framework classes, particularly generic classes. See [Communicating with Objects](../CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW15) for information on delegation, notification, and other mechanisms for communication between objects in a program network.

### Cocoa API Conventions
When you start using the classes, methods, and other symbols declared by the Cocoa frameworks, you should be aware of a few conventions that are intended to ensure efficiency and consistency in usage.

Methods that return objects typically return `nil` to indicate that there was a failure to create an object or that, for any other reason, no object was returned. They do not directly return a status code.

The convention of returning `nil` is often used to indicate a runtime error or other nonexceptional condition. The Cocoa frameworks deal with errors such as an array index out of bounds or an unrecognized method selector by raising an exception (which is handled by a top-level handler) and, if the method signature so requires, returning `nil`.

Some of these same methods that might return `nil` include a final parameter for returning error information by reference.

This final parameter takes a pointer to an `[NSError](https://developer.apple.com/documentation/foundation/nserror)` object. Upon return from a method call that fails (that is, returns `nil`), you can inspect the returned error object to determine the cause of the error or you can display the error to the user in a dialog.

As an example, here’s a method from the `[NSDocument](https://developer.apple.com/documentation/appkit/nsdocument)` class:

- (id)initWithType:(NSString *)typeName error:(NSError **)outError;In a similar fashion, methods that perform some system operation (such as reading or writing a file) often return a Boolean value to indicate success or failure.

These methods might also include a pointer to an `NSError` object as a final by-reference parameter. For example, there’s this method from the `[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)` class:

- (BOOL)writeToFile:(NSString *)path options:(unsigned)writeOptionsMask error:(NSError **)errorPtr;Empty container objects are used to indicate a default value or no value—`nil` is usually not a valid object parameter. 

Many objects encapsulate values or collections of objects—for example, instances of `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)`, `[NSDate](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDateClassCluster/Description.html#//apple_ref/occ/cl/NSDate)`, `[NSArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSArray)`, and `[NSDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSDictionary)`.  Methods that take these objects as parameters may accept an empty object (for example, `@""`) to indicate that there is no value or to request that the default value be used. For example, the following message sets the represented filename for a window to “no value” by specifying an empty string:

[aWindow setRepresentedFilename:@""];
> **Note:** The Objective-C construct `@“`*characters*`”` creates an `NSString` object containing the literal *characters*. Thus, `@““` would create a string object with no characters—or, an empty string. For more information, see *[String Programming Guide](../../Strings/introStrings.html#//apple_ref/doc/uid/10000035i)*.

The Cocoa frameworks expect that global string constants rather than string literals are used for dictionary keys, notification and exception names, and some method parameters that take strings. 

You should always prefer string constants over string literals when you have a choice. By using string constants, you enlist the help of the compiler to check your spelling and thus avoid runtime errors. 

The Cocoa frameworks use types consistently, giving higher impedance matching across their API sets.

For example, the frameworks use `float` for coordinate values, `CGFloat` for both graphical and coordinate values, `[NSPoint](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/tdef/NSPoint)` (AppKit) or `[CGPoint](https://developer.apple.com/documentation/coregraphics/cgpoint)` (UIKit) for a location in a coordinate system,  `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` objects for string values, `[NSRange](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/tdef/NSRange)` for ranges, and `NSInteger` and `NSUInteger` for, respectively, signed  and unsigned integral values. When you design your own APIs, you should strive for similar type consistency.

Unless stated otherwise in the documentation or header files, sizes that are returned in methods such as `[frame](https://developer.apple.com/documentation/appkit/nsview/1483713-frame)` and `[bounds](https://developer.apple.com/documentation/appkit/nsview/1483817-bounds)` (`NSView`) are in points.

A substantial subset of Cocoa API conventions concerns the naming of classes, methods, functions, constants, and other symbols. You should be aware of these conventions when you begin designing your own programmatic interfaces. Some of the more important of these naming conventions are the following:

Use prefixes for class names and for symbols associated with the class, such as functions and type definitions (`typedef`).

A prefix protects against collisions and helps to differentiate functional areas. The prefix convention is two or three unique uppercase letters, for example, the “AC” in `ACCircle`.

With API names, it’s better to be clear than brief.

For example, it’s easy to understand what `removeObjectAtIndex:` does but `remove:` is ambiguous.

Avoid ambiguous names.

For example, `displayName` is ambiguous because it’s unclear whether it displays the name or returns the display name. 

Use verbs in the names of methods or functions that represent actions.

If a method returns an attribute or computed value, the name of the method is the name of the attribute.

These methods are known as “getter” accessor methods. For example, if the attribute is background color, the getter method should be named `backgroundColor`. Getter methods that return a Boolean value are of a slight variation, using an “is” or “has” prefix—for example, `hasColor`.

If a method sets the value of an attribute—that is, a “setter” accessor method—it begins with “set” followed by the attribute name.

The first letter of the attribute name is in uppercase—for example, `setBackgroundColor:`.

> **Note:** The implementation of setter and getter methods is discussed in detail in [Storing and Accessing Properties](#//apple_ref/doc/uid/TP40002974-CH5-SW5) and in *Model Object Implementation Guide*.

Do not abbreviate parts of API names unless the abbreviation is well known (for example, HTML or TIFF).

For the complete set of naming guidelines for Objective-C programmatic interfaces, see *[Coding Guidelines for Cocoa](../../CodingGuidelines/CodingGuidelines.html#//apple_ref/doc/uid/10000146i)*.

A general, overarching API convention pertains to object ownership with memory-managed applications (versus garbage-collected applications). Briefly stated, the convention is that a client owns an object if it creates the object (by allocation then initialization), copies it, or retains it (by sending it `[retain](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intfm/NSObject/retain)`). An owner of an object is responsible for its disposal by sending `[release](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intfm/NSObject/release)` or `[autorelease](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intfm/NSObject/autorelease)` to the object when it no longer needs it. For more on this subject, see [How Memory Management Works](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW10) and *[Advanced Memory Management Programming Guide](../../MemoryMgmt/Articles/MemoryMgmt.html#//apple_ref/doc/uid/10000011i)*.

## Inheriting from a Cocoa Class
A framework such as AppKit or UIKit defines a program model that, because it is generic, many different types of applications can share. Because the model is generic, it is not surprising that some framework classes are abstract or intentionally incomplete. A class often does much of its work in low-level and common code, but leaves significant portions of the work either undone or completed in a safe but generic “default” fashion. 

An application often needs to create a subclass that fills in these gaps in its superclass, supplying the pieces the framework class is missing. A subclass is the primary way to add application-specific behavior to a framework. An instance of your custom subclass takes its place in the network of objects the framework defines. It inherits the ability to work with other objects from the framework. For example, if you create a subclass of `[NSCell](https://developer.apple.com/documentation/appkit/nscell)` (AppKit), instances of this new class are able to appear in an `[NSMatrix](https://developer.apple.com/documentation/appkit/nsmatrix)` object, just as `[NSButtonCell](https://developer.apple.com/documentation/appkit/nsbuttoncell)`, `[NSTextFieldCell](https://developer.apple.com/documentation/appkit/nstextfieldcell)`, and other framework-defined cell objects can.

When you make a subclass, one of your primary tasks is to implement a specific set of methods declared by a superclass (or in a protocol adopted by a superclass). Reimplementing an inherited method is known as *overriding* that method.

### When to Override a Method
Most methods defined in a framework class are fully implemented; they exist so you can invoke them to obtain the services the class provides. You rarely need to override such methods and shouldn’t attempt to. The framework depends on them doing just what they do—nothing more and nothing less. In other cases, you can override a method, but there’s no real reason to do so. The framework’s version of the method does an adequate job. But just as you might implement your own version of a string-comparison function rather than use `strcmp`, you can choose to override the framework method.

Some framework methods, however, are intended to be overridden; they exist to let you add program-specific behavior to the framework. Often the method, as implemented by the framework, does little or nothing that’s of value to your application, but is invoked in messages initiated by other framework methods. To give content to these kinds of methods, an application must implement its own version.

#### Invoke or Override?
The framework methods you override in a subclass generally won’t be ones that you’ll invoke yourself, at least directly. You simply reimplement the method and leave the rest up to the framework. In fact, the more likely you are to write an application-specific version of a method, the less likely you are to invoke it in your own code. There’s a good reason for this. In a general sense, a framework class declares public methods so that you, the developer, can do one of two things:

Invoke them to avail yourself of the services the class provides

Override them to introduce your own code into the program model defined by the framework

Sometimes a method falls into both these categories; it renders a valuable service upon invocation, and it can be strategically overridden. But generally, if a method is one that you can invoke, it’s fully defined by the framework and doesn’t need to be redefined in your code. If the method is one that you need to reimplement in a subclass, the framework has a particular job for it to do and so will invoke the method itself at the appropriate times. Figure 3-2 illustrates the two general types of framework methods.

**Figure 3-2**  Invoking a framework method that invokes an overridden methodMuch of the work of object-oriented programming with a Cocoa framework is implementing methods that your program uses only indirectly, through messages arranged by the framework.

#### Types of Overridden Methods
You may choose to define several different types of methods in a subclass:

Some framework methods are fully implemented and are meant to be invoked by other framework methods. In other words, even though you may reimplement these methods, you often don’t invoke them elsewhere in your code. They provide some service—data or behavior—required by some other code at some point during program execution. These methods exist in the public interface for just one reason—so that you can override them if you want to. They give you an opportunity either to substitute your own algorithm for the one used by the framework or to modify or extend the framework algorithm.

An example of this type of method is `trackWithEvent:`, defined in the `NSMenuView` class of the AppKit framework. `NSMenuView` implements this method to satisfy the immediate requirement—handling menu tracking and item selection—but you may override it if you want different behavior.

Another type of method is one that makes an object-specific decision, such as whether an attribute is turned on or whether a certain policy is in effect. The framework implements a default version of this method that makes the decision one way, and you must implement your own version if you want a different decision. In most cases, implementation is simply a matter of returning `YES` or `NO`, or of calculating a value other than the default. 

The `[acceptsFirstResponder](https://developer.apple.com/documentation/appkit/nsresponder/1528708-acceptsfirstresponder)` method of `NSResponder` is typical of this sort of method. Views are sent `acceptsFirstResponder` messages asking, among other things, if they respond to keystrokes or mouse clicks. By default, `[NSView](https://developer.apple.com/documentation/appkit/nsview)` objects return `NO` for this method—most views don’t accept typed input. But some do, and they should override `acceptsFirstResponder` to return `YES`. A UIKit example is the `[layerClass](https://developer.apple.com/documentation/uikit/uiview/1622626-layerclass)` class method of `UIView`; if you do not want the default layer class for the view, you can override the method and substitute your own.

Some methods must be overridden, but only to add something, not to replace what the framework implementation of the method does. The subclass version of the method augments the behavior of the superclass version. When your program implements one of these methods, it’s important that it incorporate the very method it overrides. It does this by sending a message to `super` (the superclass), invoking the framework-defined version of the method.

Often this kind of method is one that every class in a chain of inheritance is expected to contribute to. For example, objects that archive themselves must conform to the `NSCoding` protocol and implement the `[initWithCoder:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intfm/NSCoding/initWithCoder:)`and `[encodeWithCoder:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intfm/NSCoding/encodeWithCoder:)` methods. But before a class performs the encoding or decoding that is specific to its instance variables, it must invoke the superclass version of the method.

Sometimes a subclass version of a method wants to reuse the superclass behavior and then add something, however small, to the final result. In the `NSView` method  `[drawRect:](https://developer.apple.com/documentation/appkit/nsview/1483686-draw)`, for example, a subclass of a view class that performs some complicated drawing might want to draw a border around the drawing, so it would invoke `super` first. 

Some framework methods do nothing at all or merely return some trivial default value (such as `self`) to prevent runtime or compile-time errors. These methods are meant to be overridden. The framework cannot define these methods even in rudimentary form because they carry out tasks that are entirely program-specific. There’s no need to incorporate the framework implementation of the method with a message to `super`.

Most methods that a subclass overrides belong in this group. For example, the `NSDocument` methods `[dataOfType:error:](https://developer.apple.com/documentation/appkit/nsdocument/1515205-data)` and `[readFromData:ofType:error:](https://developer.apple.com/documentation/appkit/nsdocument/1515198-readfromdata)` (among others) must be overridden when you create a document-based application.

Overriding a method does not have to be a formidable task. You can often make significant change in superclass behavior by a careful reimplementation of the method that entails no more than one or two lines of code. And you are not entirely on your own when you implement your own version of a method. You can draw on the classes, methods, functions, and types already provided by the Cocoa frameworks. 

### When to Make a Subclass
Just as important as knowing which methods of a class to override—and indeed preceding that decision—is identifying those classes to inherit from. Sometimes these decisions can be obvious, and sometimes they can be far from simple. A few design considerations can guide your choices.

First, know the framework. You should become familiar with the purpose and capabilities of each class in the framework. Maybe there is a class that already does what you want to do. And if you find a class that does *almost* what you want done, you’re in luck. That class is a promising superclass for your custom class. Subclassing is a process of reusing an existing class and specializing it for your needs. Sometimes all a subclass needs to do is override a single inherited method and have the method do something slightly different from the original behavior. Other subclasses might add one or two attributes to their superclass (as instance variables), and then define the methods that access and operate on these attributes, integrating them into the superclass behavior.

There are other considerations that can help you decide where your subclass best fits into the class hierarchy. What is the nature of the application, or of the part of the application you’re trying to craft? Some Cocoa architectures impose their own subclassing requirements. For example, if yours is a multiple-document application in OS X, the document-based architecture defined by AppKit requires you to subclass `[NSDocument](https://developer.apple.com/documentation/appkit/nsdocument)` and perhaps other classes as well. To make your Mac app scriptable (that is, responsive to AppleScript commands), you might have to subclass one of the scripting classes of AppKit, such as `[NSScriptCommand](https://developer.apple.com/documentation/foundation/nsscriptcommand)`. 

Another factor is the role that instances of the subclass will play in the application. The Model-View-Control design pattern, a major one in Cocoa, assigns roles to objects: They’re view objects that appear on the user interface; model objects holding application data (and the algorithms that act on that data); or controller objects, which mediate between view and model objects. (For details, see [The Model-View-Controller Design Pattern](../CocoaDesignPatterns/CocoaDesignPatterns.html#//apple_ref/doc/uid/TP40002974-CH6-SW1).) Knowing what role an object plays can narrow the decision for which superclass to use. If instances of your class are view objects that do their own custom drawing and event-handling, your class should probably inherit from `[NSView](https://developer.apple.com/documentation/appkit/nsview)` (if you are using AppKit) or from `[UIView](https://developer.apple.com/documentation/uikit/uiview)` (if you are using UIKit). If your application needs a controller object, you can either use one of the off-the-shelf controller classes of AppKit (such as `[NSObjectController](https://developer.apple.com/documentation/appkit/nsobjectcontroller)`) or, if you want a different behavior, subclass `[NSController](https://developer.apple.com/documentation/appkit/nscontroller)` or `[NSObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/cl/NSObject)`. If your class is a typical model class—say, one whose objects represent rows of corporate data in a spreadsheet—you probably should subclass `NSObject` or use the Core Data framework. 

> **iOS Note:** The controller classes of the AppKit framework, including `NSController`, have no counterparts in UIKit. There are controller classes in UIKit—`[UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller)`, `[UITableViewController](https://developer.apple.com/documentation/uikit/uitableviewcontroller)`, `[UINavigationController](https://developer.apple.com/documentation/uikit/uinavigationcontroller)`, and `[UITabBarController](https://developer.apple.com/documentation/uikit/uitabbarcontroller)`—but these classes are based on a different design and have a different purpose—application navigation and mode selection (see [View Controllers in UIKit](../CocoaDesignPatterns/CocoaDesignPatterns.html#//apple_ref/doc/uid/TP40002974-CH6-SW88) for a summary).

Subclassing is sometimes not the best way to solve a problem. There may be a better approach you could take. If you just want to add a few convenience methods to a class, you might create a category instead of a subclass. Or you could employ one of the many other design-pattern–based resources of the Cocoa development toolbox, such as delegation, notification, and target-action (described in [Communicating with Objects](../CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW15)). When deciding on a candidate superclass, scan the header file of the class (or scan the reference documentation) to see if there is a delegation method, a notification, or some other mechanism that will enable you to do what you want without subclassing.

In a similar vein, you can also examine the header files or documentation for the framework’s protocols. By adopting a protocol, you might be able to accomplish your goal while avoiding the difficulties of a complex subclass. For example, if you want to manage the enabled states of menu items, you can adopt the `NSMenuValidation` protocol in a custom controller class; you don’t have to subclass `[NSMenuItem](https://developer.apple.com/documentation/appkit/nsmenuitem)` or `[NSMenu](https://developer.apple.com/documentation/appkit/nsmenu)` to get this behavior. 

Just as some framework methods are not intended to be overridden, some framework classes (such as `[NSFileManager](https://developer.apple.com/documentation/foundation/filemanager)` in AppKit and `[UIWebView](https://developer.apple.com/documentation/uikit/uiwebview)` in UIKit) are not intended to be subclassed. If you do attempt such a subclass, you should proceed with caution. The implementations of certain framework classes are complicated and tightly integrated into the implementations of other classes and even different parts of the operating system. Often it is difficult to duplicate correctly what a framework method does or to anticipate interdependencies or effects the method might have. Changes that you make in some method implementations could have far-reaching, unforeseen, and unpleasant consequences. 

In some cases, you can get around these difficulties by using object composition, a general technique of assembling objects in a “host” object, which manages them to get complex and highly customized behavior (see Figure 3-3). Instead of inheriting directly from a complex framework superclass, you might create a custom class that holds an instance of that superclass as an instance variable. The custom class itself could be fairly simple, perhaps inheriting directly from the root class, `[NSObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/cl/NSObject)`; although simple in terms of inheritance, the class manipulates, extends, and augments the embedded instance. To client objects it can appear in some respects to be a subclass of the complex superclass, although it probably won’t share the interface of the superclass. The Foundation framework class `[NSAttributedString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAttributedStrngClstr/Description.html#//apple_ref/occ/cl/NSAttributedString)` gives an example of object composition. `NSAttributedString` holds an `NSString` object as an instance variable, and exposes it through the `[string](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAttributedStrngClstr/Description.html#//apple_ref/occ/instm/NSAttributedString/string)` method. `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` is a class with complex behaviors, including string encoding, string searching, and path manipulation. `NSAttributedString` augments these behaviors with the capability for attaching attributes such as font, color, alignment, and paragraph style to a range of characters. And it does so without subclassing `NSString`. 

**Figure 3-3**  Object composition## Basic Subclass Design
All subclasses, regardless of ancestor or role, share certain characteristics when they are well designed. A poorly designed subclass is susceptible to error, hard to use, difficult to extend, and a drag on performance. A well-designed subclass is quite the opposite. This section offers suggestions for designing subclasses that are efficient, robust, and both usable and reusable.

> **Further Reading:** Although this document describes some of the mechanics of crafting a subclass, it focuses primarily on basic design. It does not describe how you can use the development applications Xcode and Interface Builder to automate part of class definition. To start learning about these applications, read *[A Tour of Xcode](../../../../DeveloperTools/Conceptual/A_Tour_of_Xcode/000-Introduction/qt_intro.html#//apple_ref/doc/uid/TP30000890)*. The design information presented in this section is particularly applicable to model objects. *Model Object Implementation Guide* discusses the proper design and implementation of model objects at length.

### The Form of a Subclass Definition
An Objective-C class consists of an interface and an implementation. By convention, these two parts of a class definition go in separate files. The name of the interface file (as with ANSI C header files) has an extension of `.h`; the name of the implementation file has an extension of `.m`. The name of the file up to the extension is typically the name of the class. Thus for a class named `Controller`, the files would be: 

Controller.h
```
Controller.m
```
The interface file contains a list of property, method, and function declarations that establish the public interface of the class. It also holds declarations of instance variables, constants, string globals, and other data types. The directive `@interface` introduces the essential declarations of the interface, and the `@end` directive terminates the declarations. The `@interface` directive is particularly important because it identifies the name of the class and the class directly inherited from, using the form:

`@interface` *ClassName*` : `*Superclass*

Listing 3-2 gives an example of the interface file for the hypothetical `Controller` class, before any declarations are made.

**Listing 3-2**  The basic structure of an interface file

#import <Foundation/Foundation.h>
```
 
```

```
 
```

```
@interface Controller : NSObject {
```

```
 
```

```
}
```

```
 
```

```
@end
```
To complete the class interface, you must make the necessary declarations in the appropriate places in this interface structure, as shown in Figure 3-4.

**Figure 3-4**  Where to put declarations in the interface fileSee [Instance Variables](#//apple_ref/doc/uid/TP40002974-CH5-SW12) and [Functions, Constants, and Other C Types](#//apple_ref/doc/uid/TP40002974-CH5-SW8) for more information about these types of declarations.

You begin the class interface by importing the appropriate Cocoa frameworks as well as the header files for any types that appear in the interface. The `#import` preprocessor command is similar to `#include` in that it incorporates the specified header file. However, `#import` improves on efficiency by including the file only if it was not previously included, directly or indirectly, for compilation. The angle brackets (`<...>`) following the command identify the framework the header file is from and, after a slash character, the header file itself. Thus the syntax for an `#import` line takes the form:

`#import <`*Framework*`/`*File*`.h>`

The framework must be in one of the standard system locations for frameworks. In the example in [Listing 3-2](#//apple_ref/doc/uid/TP40002974-CH5-SW10), the class interface file imports the file `Foundation.h` from the Foundation framework. As in this case, the Cocoa convention is that the header file having the same name as the framework contains a list of `#import` commands that include all the public interfaces (and other public headers) of the framework. Incidentally, if the class you’re defining is part of an application, all you need to do is import the `Cocoa.h` file of the Cocoa umbrella framework.

If you must import a header file that is part of your project, use quotation marks rather than angle brackets as delimiters; for example:

#import "MyDataTypes.h"The class implementation file has a simpler structure, as shown by Listing 3-3. It is important that the class implementation file begin by importing the class interface file.

**Listing 3-3**  The basic structure of an implementation file

#import "Controller.h"
```
 
```

```
 
```

```
@implementation Controller
```

```
 
```

```
 
```

```
@end
```
You must write all method and property implementations between the  `@implementation` and `@end` directives. The implementations of functions related to the class can go anywhere in the file, although by convention they are also put between the two directives. Declarations of private types (functions, structures, and so on) typically go between the `#import` commands and the `@implementation` directive.

### Overriding Superclass Methods
Custom classes, by definition, modify the behavior of their superclass in some program-specific way. Overriding superclass methods is usually the way to effect this change. When you design a subclass, an essential step is identifying the methods you are going to override and thinking about how you are going to reimplement them.

Although [When to Override a Method](#//apple_ref/doc/uid/TP40002974-CH5-SW13) offers some general guidelines, identifying which superclass methods to override requires you to investigate the class. Peruse the header files and documentation of the class. Also locate the designated initializer of the superclass because, for initialization to succeed, you must invoke the superclass version of this method in your class’s designated initializer. (See [Object Creation](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW17) for details.)

Once you have identified the methods, you can start—from a purely practical standpoint—by copying the declarations of the methods to be overridden into your interface file. Then copy those same declarations into your `.m` file as skeletal method implementations by replacing the terminating semicolon with enclosing braces. 

Remember, as discussed in [When to Override a Method](#//apple_ref/doc/uid/TP40002974-CH5-SW13), a Cocoa framework (and not your own code) usually invokes the framework methods that you override. In some situations you need to let the framework know that it should invoke your overridden version of a method and not the original method. Cocoa gives you various ways to accomplish this. The Interface Builder application, for example, allows you to substitute your class for a (compatible) framework class in its Info (inspector) window. If you create a custom `[NSCell](https://developer.apple.com/documentation/appkit/nscell)` class, you can associate it with a particular control using the  `[setCell:](https://developer.apple.com/documentation/appkit/nscontrol/1428960-cell)` method of the `NSControl` class. 

### Instance Variables
The reason for creating a custom class, aside from modifying the behavior of the superclass, is to add properties  to it. (The word *properties* here is intended in a general sense, indicating both the attributes of an instance of the subclass and the relationships that instance has to other objects.) Given a hypothetical `Circle` class, if you are going to make a subclass that adds color to the shape, the subclass must somehow carry the attribute of color. To accomplish this, you might add a `color` instance variable (most likely typed as an `[NSColor](https://developer.apple.com/documentation/appkit/nscolor)` object for a Mac app, or a `[UIColor](https://developer.apple.com/documentation/uikit/uicolor)` object for an iOS application) to the class interface. Instances of your custom class encapsulate the new attribute, holding it as a persistent, characterizing piece of data. 

Instance variables in Objective-C are declarations of objects, structures, and other data types that are part of the class definition. If they are objects, the declaration can either dynamically type (using `id`) or statically type the object. The following example shows both styles:

id delegate;
```
NSColor *color;
```
Generally, you dynamically type an object instance variable when the class membership of the object is indeterminate or unimportant. Objects held as instance variables should be created, copied, or explicitly retained—unless they are already retained by a parent object. If an instance is unarchived, it should retain its object instance variables as it decodes and assigns them in `[initWithCoder:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intfm/NSCoding/initWithCoder:)`. (For more on object archiving and unarchiving, see [Entry and Exit Points](#//apple_ref/doc/uid/TP40002974-CH5-SW6).)

The convention for naming instance variables is to make them a lowercase string containing no punctuation marks or special characters. If the name contains multiple words, run them together, but make the first letter of the second and each succeeding word uppercase. For example: 

NSString *title;
```
UIColor *backgroundColor;
```

```
NSRange currentSelectedRange;
```
When the tag `IBOutlet` precedes the declarations of instance variables, it identifies an outlet with (presumably) a connection archived in a nib file. The tag also enables Interface Builder and Xcode to synchronize their activities. When outlets are unarchived from an Interface Builder nib file, the connections are automatically reestablished. ([Communicating with Objects](../CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW15) discusses outlets in more detail.)

Instance variables can hold more than an object’s attributes, vendable to clients of the object. Sometimes instance variables can hold private data that is used as the basis for some task performed by the object; a backing store or a cache are examples. (If data is not per-instance, but is to be shared among instances of a class, use global variables instead of instance variables.)

When you add instance variables to a subclass, follow these guidelines:

Add only instance variables that are absolutely necessary. The more instance variables you add, the more the instance size swells. And the more instances of your class that are created, the more of a concern this becomes. If it’s possible, compute a critical value from existing instance variables rather than add another instance variable to hold that value.

Again, for economy represent the instance data of the class efficiently. For example, if you want to specify a number of flags as instance variables, use a bit field instead of a series of Boolean declarations. (Be aware, however, that there are archiving complications with bit fields). You might use an `[NSDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSDictionary)` object to consolidate a number of related attributes as key-value pairs; if you do this, make sure that the keys are adequately documented. 

Give the proper scope to your instance variables. Never scope a variable as `@public` as this violates the principle of encapsulation. Use `@protected` for instance variables if you know which are the likely subclasses of your class (such as the classes of your application) and you require efficient access of data. Otherwise, `@private` is a reasonable choice for the high degree of implementation hiding it provides. This implementation hiding is especially important for classes exposed by a framework and used by applications or other frameworks; it permits the modification of the class implementation without the need to recompile all clients.

Make sure there are declared  properties or accessor methods for those instance variables that are essential attributes or relationships of the class. Accessor methods and declared properties preserve encapsulation by enabling the values of instance variables to be set and retrieved. See [Storing and Accessing Properties](#//apple_ref/doc/uid/TP40002974-CH5-SW5) for more information on this subject.

If you intend your subclass to be public—that is, you anticipate that others might subclass it—pad the end of the instance variable list with a reserved field, usually typed as `id`. This reserved field helps to ensure binary compatibility if, at any time in the future, you need to add another instance variable to the class. See [When the Class Is Public (OS X)](#//apple_ref/doc/uid/TP40002974-CH5-SW17) for more information on preparing public classes.

### Entry and Exit Points
The Cocoa frameworks send messages to an object at various points in the object’s life. Almost all objects (including classes, which are really objects themselves) receive certain messages at the beginning of their runtime life and just before their destruction. The methods invoked by these messages (if implemented) are hooks that allow the object to perform tasks relevant to the moment. These methods are (in the order of invocation):

`[initialize](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/clm/NSObject/initialize)`—This class method enables a class to initialize itself before it or its instances receive any other messages. Superclasses receive this message before subclasses. Classes that use the old archiving mechanism can implement `initialize` to set their class version. Other possible uses of `initialize` are registering a class for some service and initializing some global state that is used by all instances. However, sometimes it’s better to do these kinds of tasks lazily (that is, the first time they’re needed) in an instance method rather than in `initialize`.

`[init](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/instm/NSObject/init)` (or other initializer)—You must implement `init` or some other primary initializer to initialize the state of the instance unless the work done by the superclass’s designated initializer is sufficient for your class. See [Object Creation](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW17) for information on initializers, including designated initializers.

`[initWithCoder:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intfm/NSCoding/initWithCoder:)`—If you expect objects of your class to be archived—for example, yours is a model class— you should adopt (if necessary) the `NSCoding` protocol and implement its two methods, `initWithCoder:` and `[encodeWithCoder:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intfm/NSCoding/encodeWithCoder:)`. In these methods you must decode and encode, respectively, the instance variables of the object in a form suitable for an archive. You can use the decoding and encoding methods of `[NSCoder](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSCoder/Description.html#//apple_ref/occ/cl/NSCoder)` for this purpose or the key-archiving facilities of `[NSKeyedArchiver](https://developer.apple.com/documentation/foundation/nskeyedarchiver)` and `[NSKeyedUnarchiver](https://developer.apple.com/documentation/foundation/nskeyedunarchiver)`. When the object is being unarchived instead of being explicitly created, the `initWithCoder:` method is invoked instead of the initializer. In this method, after decoding the instance-variable values, you assign them to the appropriate variables, retaining or copying them if necessary. For more on this subject, see *[Archives and Serializations Programming Guide](../../Archiving/Archiving.html#//apple_ref/doc/uid/10000047i)*.

`[awakeFromNib](https://developer.apple.com/documentation/objectivec/nsobject/1402907-awakefromnib)`—When an application loads a nib file, an `awakeFromNib` message is sent to each object loaded from the archive, but only if it can respond to the message, and only after all the objects in the archive have been loaded and initialized. When an object receives an `awakeFromNib` message, it’s guaranteed to have all its outlet instance variables set. Typically, the object that owns the nib file (File’s Owner) implements `awakeFromNib` to perform programmatic initializations that require outlet and target-action connections to be set.

`[encodeWithCoder:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intfm/NSCoding/encodeWithCoder:)`—Implement this method if instances of your class should be archived. This method is invoked just prior to the destruction of the object. See the description of `initWithCoder:` above.

`[dealloc](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/instm/NSObject/dealloc)` or `[finalize](https://developer.apple.com/documentation/objectivec/nsobject/1418513-finalize)`—In memory-managed programs, implement the `dealloc` method to release instance variables and deallocate any other memory claimed by an instance of your class. The instance is destroyed soon after this method returns. In garbage-collected code, you may need to implement the `finalize` method instead. See [The dealloc and finalize Methods](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW40) for further information.

An application’s singleton application object (stored by AppKit in the global variable `NSApp`) also sends messages to an object—if it’s the application object’s  delegate and it implements the appropriate methods—at the beginning and end of the application’s life. Just after the application launches, the application object sends its delegate the following messages, depending on the framework and platform:  

In AppKit, the application object sends `[applicationWillFinishLaunching:](https://developer.apple.com/documentation/appkit/nsapplicationdelegate/1428623-applicationwillfinishlaunching)` and `[applicationDidFinishLaunching:](https://developer.apple.com/documentation/appkit/nsapplicationdelegate/1428385-applicationdidfinishlaunching)` to the delegate. The former is sent just before any double-clicked document is opened, the latter just afterward.

In UIKit, the application object sends `[application:didFinishLaunchingWithOptions:](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622921-application)` to the delegate.

In these methods the delegate can restore application state and specify global application logic that needs to happen just once early in the application’s runtime existence. Just before it terminates, the application object for both frameworks sends `[applicationWillTerminate:](https://developer.apple.com/documentation/appkit/nsapplicationdelegate/1428522-applicationwillterminate)` to its delegate, which can implement the method to gracefully handle program termination by saving documents and (particularly in iOS) application state. 

The Cocoa frameworks give you numerous other hooks for events ranging from view loading to application activation and deactivation. Sometimes these are implemented as delegation messages and require your object to be the delegate of the framework object and to implement the required method. Sometimes the hooks are notifications. In iOS, they can be methods inherited from the `[UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller)` that controllers override. (See [Communicating with Objects](../CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW15) for more on delegation and notification.) 

### Initialize or Decode?
If you want objects of your class to be archived and unarchived, the class must conform to the `[NSCoding](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intf/NSCoding)` protocol; it must implement the methods that encode its objects (`[encodeWithCoder:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intfm/NSCoding/encodeWithCoder:)`) and decode them (`[initWithCoder:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intfm/NSCoding/initWithCoder:)`). Instead of an initializer method or methods, the `initWithCoder:` method is invoked to initialize an object being created from an archive. 

Because the class's initializer method and `initWithCoder:` might be doing much of the same work, it makes sense to centralize that common work in a helper method called by both the initializer and `initWithCoder:`. For example, if as part of its set-up routine, an object specifies drag types and dragging source, it might implement something such as shown in Listing 3-4. 

**Listing 3-4**  Initialization helper method

 (id)initWithFrame:(NSRect)frame {
```
    self = [super initWithFrame:frame];
```

```
    if (self) {
```

```
        [self setTitleColor:[NSColor lightGrayColor]];
```

```
        [self registerForDragging];
```

```
    }
```

```
    return self;
```

```
}
```

```
- (id)initWithCoder:(NSCoder *)aCoder {
```

```
    self = [super initWithCoder:aCoder];
```

```
    titleColor = [[aCoder decodeObject] copy];
```

```
    [self registerForDragging];
```

```
    return self;
```

```
}
```

```
 
```

```
- (void)registerForDragging {
```

```
    [theView registerForDraggedTypes:
```

```
        [NSArray arrayWithObjects:DragDropSimplePboardType, NSStringPboardType,
```

```
        NSFilenamesPboardType, nil]];
```

```
    [theView setDraggingSourceOperationMask:NSDragOperationEvery forLocal:YES];
```

```
}
```
An exception to the parallel roles of a class initializer and `initWithCoder:` is when you create a custom subclass of a framework class whose instances appear on an Interface Builder palette. The general procedure is to define the class in your project, drag the object from the Interface Builder palette into your interface, and then associate that object with your custom subclass using the Custom Class pane of the Interface Builder Info window. However, in this case the `initWithCoder:` method for the subclass is not invoked during unarchiving; an `[init](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/instm/NSObject/init)` message is sent instead. You should perform any special set-up tasks for your custom object in `[awakeFromNib](https://developer.apple.com/documentation/objectivec/nsobject/1402907-awakefromnib)`, which is invoked after all objects in a nib file have been unarchived.

### Storing and Accessing Properties
The word *property* has both a general and a language-specific meaning in Cocoa. The general notion comes from the design pattern of object modeling. The language-specific meaning relates to the language feature of declared properties. 

As described in [Object Modeling](../CocoaDesignPatterns/CocoaDesignPatterns.html#//apple_ref/doc/uid/TP40002974-CH6-SW2), a property is a defining characteristic of an object (called in the object modeling pattern an *entity*) and is part of its encapsulated data. A property can be of two sorts: an attribute, such as title and location, or a relationship. Relationships can be to-one or to-many, and can take many forms, such as an outlet, a delegate, or a collection of other objects. Properties are usually, but not always, stored as instance variables.

Part of the overall strategy for a well-designed class is to enforce encapsulation. Client objects should not be able to directly access properties where they are stored, but instead access them through the interface of the class. The methods that grant access to the properties of an object are called *accessor methods*. Accessor methods (or, simply, accessors) get and set the values of an object’s properties. They function as the gates to those properties, regulating access to them and enforcing the encapsulation of the object’s instance data. The accessor method that gets a value is, by convention, called a *getter* method and the method that sets a property’s value is called a *setter* method.

The declared properties feature (introduced in Objective-C 2.0) has brought changes, some subtle, to how you control access to properties. A declared property is a syntactical shorthand for declaring the getter and setter methods of a class’s property. In the implementation block you can then request the compiler to synthesize those methods. As summarized in [Declared Properties](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW38), you declare a property, using the `@property` directive, in the `@interface` block of a class definition. The declaration specifies the name and type of the property and may include qualifiers that tell the compiler *how* to implement the accessor methods. 

When you design a class, several factors affect how you might store and access the properties of the class. You may choose to use the declared properties feature and have the compiler synthesize accessor methods for you. Or you may elect to implement the class’s accessor methods yourself; this is an option even if you do use declared properties. Another important factor is whether your code operates in a garbage-collected environment. If the garbage collector is enabled, the implementation of accessor methods is much simpler than it is in memory-managed code. The following sections describe the various alternatives for storing and accessing properties.

#### Taking Advantage of Declared Properties
Unless you have a compelling reason to do otherwise, it is recommended that you use the declared properties feature of Objective-C 2.0 in new projects rather than manually writing all of your own accessor methods. First, declared properties relieve you of the tedious burden of accessor-method implementation. Second, by having the compiler synthesize accessor methods for you, you reduce the possibility of programming errors and ensure more consistent behavior in your class implementation. And finally, the declarative style of declared properties makes your intent clear to other developers.

You might declare some properties with `@property` but still need to implement your own accessor methods for them. The typical reason for doing this is wanting the method to do something more than simply get or set the value of the property. For example, the resource for an attribute—say, a large image file—has to be loaded from the file system, and for performance reasons you want your getter method to load the resource lazily (that is, the first time it is requested). If you have to implement an accessor method for such a reason, see [Implementing Accessor Methods](#//apple_ref/doc/uid/TP40002974-CH5-SW23) for tips and guidelines. You tell the compiler not to synthesize accessor methods for a property either by designating that property with a `@dynamic` directive in the class’s `@implementation` block or by simply not specifying a `@synthesize` directive for the property (`@dynamic` is the default); the compiler refrains from synthesizing an accessor method if it sees that such a method already exists in the implementation and there is no `@synthesize` directive for it.

If garbage collection is disabled for your program, the attribute you use to qualify your property declarations is important. By default (the `assign` attribute), a synthesized setter method performs simple assignment,  which is appropriate for garbage-collected code. But in memory-managed code, simple assignment is not appropriate for properties with object values. You must either retain or copy the assigned object in your setter method; you tell the compiler to do this when you qualify a property declaration with a `retain` or `copy` attribute. Note that you should *always* specify the `copy` attribute for an object property whose class conforms to the `[NSCopying](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCopying/Description.html#//apple_ref/occ/intf/NSCopying)` protocol, even if garbage collection is enabled.

The following examples illustrate a variety of behaviors you can achieve with property declarations (with different sets of qualifying attributes) in the `@interface` block and `@synthesize` and `@dynamic` directives in the `@implementation` block. The following code instructs the compiler to synthesize accessor methods for the declared `name` and `accountID` properties; because the `accountID` property is read-only, the compiler synthesizes only a getter method. In this case, garbage collection is assumed to be enabled. 

@interface MyClass : NSObject {
```
    NSString *name;
```

```
    NSNumber *accountID;
```

```
}
```

```
@property (copy) NSString *name;
```

```
@property (readonly) NSNumber *accountID;
```

```
// ...
```

```
@end
```

```
@implementation MyClass
```

```
@synthesize name, accountID;
```

```
// ...
```

```
@end
```

> **Note:** As shown in this and subsequent code examples, a property in a 32-bit process must be backed by an instance variable. In a program with a 64-bit address space, properties do not have to be backed by instance variables.

The following code illustrates another aspect of declared properties. The assumption is that garbage collection is not enabled, so the declaration for the `currentHost` property has a `retain` attribute, which instructs the compiler to retain the new value in its synthesized getter method. In addition, the declaration for the `hidden` property includes a qualifying attribute that instructs the compiler to synthesize a getter method named `isHidden`. (Because the value of this property is a non-object value, simple assignment is sufficient in the setter method.)

@interface MyClass : NSObject {
```
    NSHost *currentHost;
```

```
    Boolean *hidden;
```

```
}
```

```
@property (retain, nonatomic) NSHost *currentHost;
```

```
@property (getter=isHidden, nonatomic) Boolean *hidden;
```

```
// ...
```

```
@end
```

```
@implementation MyClass
```

```
@synthesize currentHost, hidden;
```

```
// ...
```

```
@end
```

> **Note:** This example specifies the `nonatomic` attribute for the properties, a change from the default `atomic`. When a property is `atomic`, its synthesized getter method is guaranteed to return a valid value, even when other threads are executing simultaneously. See the discussion of properties in *[The Objective-C Programming Language](../../ObjectiveC/Introduction/introObjectiveC.html#//apple_ref/doc/uid/TP30001163)* for a discussion of atomic versus nonatomic properties, especially with regard to performance. For more on thread-safety issues, see *[Threading Programming Guide](../../Multithreading/Introduction/Introduction.html#//apple_ref/doc/uid/10000057i)*.

The property declaration in the following example tells the compiler that the `previewImage` property is read-only so that it won’t expect to find a setter method. By using the `@dynamic` directive in the `@implementation` block, it also instructs the compiler not to synthesize the getter method, and then provides an implementation of that method.

@interface MyClass : NSObject {
```
    NSImage *previewImage;
```

```
}
```

```
@property (readonly) NSImage *previewImage;
```

```
// ...
```

```
@end
```

```
@implementation MyClass
```

```
@dynamic previewImage;
```

```
// ...
```

```
- (NSImage *)previewImage {
```

```
    if (previewImage == nil) {
```

```
        // lazily load image and assign to ivar
```

```
    }
```

```
   return previewImage;
```

```
}
```

```
// ...
```

```
@end
```

> **Further Reading:** Read [Declared Properties](../../ObjectiveC/Chapters/ocProperties.html#//apple_ref/doc/uid/TP30001163-CH17) in *[The Objective-C Programming Language](../../ObjectiveC/Introduction/introObjectiveC.html#//apple_ref/doc/uid/TP30001163)* for the definitive description of declared properties and for guidelines on using them in your code.

#### Implementing Accessor Methods
For reasons of convention—and because it enables classes to be compliant with key-value coding (see [Key-Value Mechanisms](#//apple_ref/doc/uid/TP40002974-CH5-SW4))—the names of accessor methods must have a specific form. For a method that returns the value of an instance variable (sometimes called the *getter*), the name of the method is simply the name of the instance variable. For a method that sets the value of an instance variable (the *setter*), the name begins with “set” and is immediately followed by the name of the instance variable (with the first letter uppercase). For example, if you had an instance variable named “color,” the declarations of the getter and setter accessors would be:

- (NSColor *)color;
```
- (void)setColor:(NSColor *)aColor;
```
With Boolean attributes a variation of the form for getter methods is acceptable. The syntax in this case would take the form `is`*Attribute*. For example, a Boolean instance variable named `hidden` would have a `isHidden` getter method and a `setHidden:` setter method.

If an instance variable is a scalar C type, such as `int` or `float`, the implementations of accessor methods tend to be very simple. Assuming an instance variable named `currentRate`, of type `float`, Listing 3-5 shows how to implement the accessor methods.

**Listing 3-5**  Implementing accessors for a scalar instance variable

- (float)currentRate {
```
    return currentRate;
```

```
}
```

```
 
```

```
- (void)setCurrentRate:(float)newRate {
```

```
    currentRate = newRate;
```

```
}
```
If garbage collection is enabled, implementation of the setter accessor method for properties with object values is also a matter of simple assignment. Listing 3-6 shows how you would implement such methods.

**Listing 3-6**  Implementing accessors for an object instance variable (garbage collection enabled)

- (NSString *)jobTitle {
```
    return jobTitle;
```

```
}
```

```
 
```

```
- (void)setJobTitle:(NSString *)newTitle {
```

```
    jobTitle = newTitle;
```

```
}
```
When instance variables hold objects and garbage collection is not enabled, the situation becomes more nuanced. Because they are instance variables, these objects must be persistent and so must be created, copied, or retained when they are assigned. When a setter accessor changes the value of an instance variable, it is responsible not only for ensuring persistence but for properly disposing of the old value. The getter accessor vends the value of the instance variable to the object requesting it. The operations of both types of accessors have implications for memory management given two assumptions from the Cocoa object-ownership policy:

Objects returned from methods (such as from getter accessors) are valid in the scope of the invoking object. In other words, it is guaranteed that the object will not be released or change value within that scope (unless documented otherwise).

When an invoking object receives an object from a method such as an accessor, it should not release the object unless it has explicitly retained (or copied) it first.

With these two assumptions in mind, let’s look at two possible implementations of a getter and setter accessor for an `NSString` instance variable named `title`. Listing 3-7 shows the first.

**Listing 3-7**  Implementing accessors for an object instance variable—good technique

- (NSString *)title {
```
    return title;
```

```
 }
```

```
 - (void)setTitle:(NSString *)newTitle {
```

```
    if (title != newTitle) {
```

```
        [title autorelease];
```

```
        title = [newTitle copy];
```

```
    }
```

```
 }
```
Note that the getter accessor simply returns a reference to the instance variable. The setter accessor, on the other hand, is busier. After verifying that the passed-in value for the instance variable isn’t the same as the current value, it autoreleases the current value before copying the new value to the instance variable. (Sending `[autorelease](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intfm/NSObject/autorelease)` to the object is “more thread-safe” than sending it `[release](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intfm/NSObject/release)`.) However, there is still a potential danger with this approach. What if a client is using the object returned by the getter accessor and meanwhile the setter accessor autoreleases the old `NSString` object and then, soon after, that object is released and destroyed? The client object’s reference to the instance variable would no longer be valid. 

Listing 3-8 shows another implementation of the accessor methods that works around this problem by retaining and then autoreleasing the instance variable value in the getter method.

**Listing 3-8**  Implementing accessors for an object instance variable—better technique

 - (NSString *)title {
```
    return [[title retain] autorelease];
```

```
 }
```

```
 - (void)setTitle:(NSString *)newTitle {
```

```
    if (title != newTitle) {
```

```
        [title release];
```

```
        title = [newTitle copy];
```

```
    }
```

```
}
```
In both examples of the setter method above (Listing 3-5 and Listing 3-7), the new `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` instance variable is copied rather than retained. Why isn’t it retained? The general rule is this: When the object assigned to an instance variable is a value object—that is, an object that represents an attribute such as a string, date, number, or corporate record—you should copy it. You are interested in preserving the attribute value, and don't want to risk having it mutate from under you.  In other words, you want your own copy of the object.

However, if the object to be stored and accessed is an entity object, such as an `[NSView](https://developer.apple.com/documentation/appkit/nsview)` or an `[NSWindow](https://developer.apple.com/documentation/appkit/nswindow)` object, you should retain it. Entity objects are more aggregate and relational, and copying them can be expensive. One way to determine whether an object is a value object or entity object is to decide whether you’re interested in the value of the object or in the object itself. If you’re interested in the value, it’s probably a value object and you should copy it (assuming, of course, the object conforms to the `[NSCopying](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCopying/Description.html#//apple_ref/occ/intf/NSCopying)` protocol). 

Another way to decide whether to retain or copy an instance variable in a setter method is to determine whether the instance variable is an attribute or a relationship. This is especially true for model objects, which are objects that represent the data of an application. An attribute is essentially the same thing as a value object: an object that is a defining characteristic of the object that encapsulates it, such as a color (`[NSColor](https://developer.apple.com/documentation/appkit/nscolor)` object) or title (`[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` object). A relationship on the other hand is just that: a relationship with (or a reference to) one or more other objects. Generally, in setter methods you copy the values of attributes and you retain relationships. However, relationships have a cardinality; they can be one-to-one or one-to-many. One-to-many relationships, which are typically represented by collection objects such as `[NSArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSArray)` instances or `[NSSet](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSSetClassCluster/Description.html#//apple_ref/occ/cl/NSSet)` instances, may require setter methods to do something other than simply retain the instance variable. See *Model Object Implementation Guide* for details. For more on object properties as either attributes or relationships, see [Key-Value Mechanisms](#//apple_ref/doc/uid/TP40002974-CH5-SW4).

> **Important:** If you are implementing a getter accessor method to load the value of an instance variable value lazily—that is, when it is first requested—do *not* call the corresponding setter method in your implementation. Instead, set the value of the instance variable directly in the getter method.

If the setter accessors of a class are implemented as shown in either [Listing 3-5](#//apple_ref/doc/uid/TP40002974-CH5-SW2) or [Listing 3-7](#//apple_ref/doc/uid/TP40002974-CH5-SW3), to deallocate an instance variable in the class’s `dealloc` method, all you need to do is invoke the appropriate setter method, passing in `nil`.

### Key-Value Mechanisms
Several mechanisms with “key-value” in their names are fundamental parts of Cocoa: key-value binding, key-value coding, and key-value observing. They are essential ingredients of Cocoa technologies such as bindings, which automatically communicate and synchronize values between objects. They also provide at least part of the infrastructure for making applications scriptable—that is, responsive to AppleScript commands. Key-value coding and key-value observing are especially important considerations in the design of a custom subclass.

> **iOS Note:** The controller classes used in bindings (`[NSController](https://developer.apple.com/documentation/appkit/nscontroller)` and subclasses) and in application scriptability are defined only by AppKit and not by UIKit.

The term *key-value* refers to the technique of using the name of a property as the key to obtain its value. The term is part of the vocabulary of the object modeling pattern, which is briefly discussed in [Storing and Accessing Properties](#//apple_ref/doc/uid/TP40002974-CH5-SW5). Object modeling derives from the entity-relationship modeling used for describing relational databases. In object modeling, objects—particularly the data-bearing model objects of the Model-View-Controller pattern—have properties, which usually (but not always) take the form of instance variables. A property can either be an attribute, such as name or color, or it can be a reference to one or more other objects. These references are known as *relationships*, and relationships can be one-to-one or one-to-many. The network of objects in a program form an object graph through their relationships with each other. In object modeling you can use key paths—strings of dot-separated keys—to traverse the relationships in an object graph and access the properties of objects.

> **Note:** For a full description of object modeling, see [Object Modeling](../CocoaDesignPatterns/CocoaDesignPatterns.html#//apple_ref/doc/uid/TP40002974-CH6-SW2).

Key-value binding, key-value coding, and key-value observing are enabling mechanisms for this traversal.

Key-value binding (KVB) establishes bindings between objects and also removes and advertises those bindings. It makes use of several informal protocols. A binding for a property must specify the object and a key path to the property.

Key-value coding (KVC), through an implementation of the `NSKeyValueCoding` informal protocol, makes it possible to get and set the values of an object’s properties through keys without having to invoke that object’s accessor methods directly. (Cocoa provides a default implementation of the protocol.) A key typically corresponds to the name of an instance variable or accessor method in the object being accessed. 

Key-value observing (KVO), through an implementation of the `NSKeyValueObserving` informal protocol, allows objects to register themselves as observers of other objects. The observed object directly notifies its observers when there are changes to one of its properties. Cocoa implements automatic observer notifications for each property of a KVO-compliant object.

To make each property of a subclass compliant with the requirements of key-value coding, do the following:

For an attribute or a to-one relationship named *key*, implement accessor methods named *key* (getter) and `set`*Key*`:` (setter). For example, if you have a property named `salary`, you would have accessor methods `salary` and `setSalary:`. (If the compiler synthesizes a declared property, it follows this pattern, unless instructed otherwise.)

For a to-many relationship, if the property is based on an instance variable that is a collection (for example, an `[NSArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSArray)` object) or an accessor method that returns a collection, give the getter method the same name as the property (for example, `employees`). If the property is mutable, but the getter method doesn’t return a mutable collection (such as `[NSMutableArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSMutableArray)`), you must implement `insertObject:in`*Key*`AtIndex:` and `removeObjectFrom`*Key*`AtIndex:`. If the instance variable is not a collection and the getter method does not return a collection, you must implement other `NSKeyValueCoding` methods.

To make your object KVO-compliant, simply ensure that your object is KVC-compliant if you are satisfied with automatic observer notification. However, you may choose to implement manual key-value observing, which requires additional work.

> **Further Reading:** Read *[Key-Value Coding Programming Guide](../../KeyValueCoding/index.html#//apple_ref/doc/uid/10000107i)* and *[Key-Value Observing Programming Guide](../../KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i)* to learn more about these technologies. For a summary of the Cocoa bindings technology, which uses KVC, KVO, and KVB, see [Bindings (OS X)](../CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW17) in [Communicating with Objects](../CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW15).

### Object Infrastructure
If a subclass is well-designed, instances of that class will behave in ways expected of Cocoa objects. Code using the object can compare it to other instances of the class, discover its contents (for example, in a debugger), and perform similar basic operations with the object.

Custom subclasses should implement most, if not all, of the following root-class and basic protocol methods:

`[isEqual:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intfm/NSObject/isEqual:)` and `[hash](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intfm/NSObject/hash)`—Implement these `NSObject` methods to bring some object-specific logic to their comparison. For example, if what individuates instances of your class is their serial number, make that the basis for equality. For further information, see [Introspection](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW25).

`[description](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/clm/NSObject/description)`—Implement this `NSObject` method to return a string that concisely describes the properties or contents of the object. This information is returned by the `print object` command in the `gdb` debugger and is used by the `%@` specifier for objects in formatted strings. For example, say you have a hypothetical `Employee` class with attributes for name, date of hire, department, and position ID. The description method for this class might look like the following:

- (NSString *)description {
```
    return [NSString stringWithFormat:@”Employee:Name = %@,
```

```
        Hire Date = %@, Department = %@, Position = %i\n”, [self name],
```

```
        [[self dateOfHire] description], [self department],
```

```
        [self position]];
```

```
}
```
`[copyWithZone:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCopying/Description.html#//apple_ref/occ/intfm/NSCopying/copyWithZone:)`—If you expect clients of your subclass to copy instances of it, you should implement this method of the `[NSCopying](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCopying/Description.html#//apple_ref/occ/intf/NSCopying)` protocol. Value objects, including model objects, are typical candidates for copying; objects such as `[UITableView](https://developer.apple.com/documentation/uikit/uitableview)` and `[NSColorPanel](https://developer.apple.com/documentation/appkit/nscolorpanel)` are not. If instances of your class are mutable, you should instead conform to the `[NSMutableCopying](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSMutableCopying/Description.html#//apple_ref/occ/intf/NSMutableCopying)` protocol.

`[initWithCoder:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intfm/NSCoding/initWithCoder:)` and `[encodeWithCoder:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intfm/NSCoding/encodeWithCoder:)`—If you expect instances of your class to be archived (for example, as with a model class), you should adopt the `[NSCoding](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intf/NSCoding)` protocol (if necessary) and implement these two methods. See [Entry and Exit Points](#//apple_ref/doc/uid/TP40002974-CH5-SW6) for more on the `NSCoding` methods.

If any of the ancestors of your class adopts a formal protocol, you must also ensure that your class properly conforms to the protocol. That is, if the superclass implementation of any of the protocol methods is inadequate for your class, your class should reimplement the methods.

### Error Handling
It is axiomatic for any programming discipline that the programmer should properly handle errors. However, what is proper often varies by programming language, application environment, and other factors. Cocoa has its own set of conventions and prescriptions for handling errors in subclass code.

If the error encountered in a method implementation is a system-level or Objective-C runtime error, create and raise an exception, if necessary, and handle it locally, if possible.

In Cocoa, exceptions are typically reserved for programming or *unexpected* runtime errors such as out-of-bounds collection access, attempts to mutate immutable objects, sending an invalid message, and losing the connection to the window server. You usually take care of these errors with exceptions when an application is being created rather than at runtime. Cocoa predefines several exceptions that you can catch with an exception handler. For information on predefined exceptions and the procedure and API for raising and handling exceptions, see *[Exception Programming Topics](../../Exceptions/Exceptions.html#//apple_ref/doc/uid/10000012i)*.

For other sorts of errors, including *expected* runtime errors, return `nil`, `NO`, `NULL`, or some other type-suitable form of zero to the caller. Examples of these errors include the inability to read or write a file, a failure to initialize an object, the inability to establish a network connection, or a failure to locate an object in a collection. Use an `[NSError](https://developer.apple.com/documentation/foundation/nserror)` object if you feel it necessary to return supplemental information about the error to the sender.

An `NSError` object encapsulates information about an error, including an error code (which can be specific to the Mach, POSIX, or `OSStatus` domains) and a dictionary of program-specific information. The negative value that is directly returned (`nil`, `NO`, and so on) should be the principal indicator of error; if you do communicate more specific error information, return an `NSError` object indirectly in an parameter of the method.

If the error requires a decision or action from the user, display an alert dialog.

For OS X, use an instance of the `[NSAlert](https://developer.apple.com/documentation/appkit/nsalert)` class (and related facilities) for displaying an alert dialog and handling the user’s response. For iOS, use the facilities of the `[UIActionSheet](https://developer.apple.com/documentation/uikit/uiactionsheet)` or `[UIAlertView](https://developer.apple.com/documentation/uikit/uialertview)` classes. See *[Dialogs and Special Panels](../../Dialog/Dialog.html#//apple_ref/doc/uid/10000071i)* for information.

For further information about `NSError` objects, handling errors, and displaying error alerts, see *[Error Handling Programming Guide](../../ErrorHandlingCocoa/ErrorHandling/ErrorHandling.html#//apple_ref/doc/uid/TP40001806)*.

### Resource Management and Other Efficiencies
There are all sorts of things you can do to enhance the performance of your objects and, of course, the application that incorporates and manages those objects. These procedures and disciplines range from multithreading to drawing optimizations and techniques for reducing your code footprint. You can learn more about these things by reading *[Cocoa Performance Guidelines](../../CocoaPerformance/CocoaPerformance.html#//apple_ref/doc/uid/TP40001448)* and other performance documents.

Yet you can, before even adopting a more advanced performance technique, improve the performance of your objects significantly by following three simple, common-sense guidelines:

Don’t load a resource or allocate memory until you really need it.

If you load a resource of the program, such as a nib file or an image, and don’t use it until much later, or don’t use it at all, that is a gross inefficiency. Your program’s memory footprint is bloated for no good reason. You should load resources or allocate memory only when there is an immediate need.

For example, if your application’s preferences window is in a separate nib file, don’t load that file until the user first chooses Preferences from the application menu. The same caution goes for allocating memory for some task; hold off on allocation until the need for that memory arises. This technique of lazy-loading or lazy-allocation is easily implemented. For example, say your application has an image that it loads, upon the first user request, for displaying in the user interface. Listing 3-9 shows one approach: loading the image in the getter accessor for the image.

**Listing 3-9**  Lazy-loading of a resource

- (NSImage *)fooImage {
```
    if (!fooImage) { // fooImage is an instance variable
```

```
        NSString *imagePath = [[NSBundle mainBundle] pathForResource:@"Foo" ofType:@"jpg"];
```

```
        if (!imagePath) return nil;
```

```
        fooImage = [[NSImage alloc] initWithContentsOfFile:imagePath];
```

```
    }
```

```
    return fooImage;
```

```
}
```
Use the Cocoa API; don’t dig down into lower-level programming interfaces.

Much effort has gone into making the implementations of the Cocoa frameworks as robust, secure, and efficient as possible. Moreover, these implementations manage interdependencies that you might not be aware of. If, instead of using the Cocoa API for a certain task, you decide to “roll your own” solution using lower-level interfaces, you’ll probably end up writing more code, which introduces a greater risk of error or inefficiency. Also, by leveraging Cocoa, you are better prepared to take advantage of future enhancements while at the same time you are more insulated from changes in the underlying implementations. So use the Cocoa alternative for a programming task if one exists and its capabilities answer your needs. 

Practice good memory-management techniques.

If you decide not to enable garbage collection (which is disabled by default in iOS), perhaps the most important single thing you can do to improve the efficiency and robustness of your custom objects is to practice good memory-management techniques. Make sure every object allocation, `copy` message, or `retain` message is matched with a corresponding `release` message. Become familiar with the policies and techniques for proper memory management and practice, practice, practice them. See *[Advanced Memory Management Programming Guide](../../MemoryMgmt/Articles/MemoryMgmt.html#//apple_ref/doc/uid/10000011i)* for complete details.

### Functions, Constants, and Other C Types
Because Objective-C is a superset of ANSI C, it permits any C type in your code, including functions, `typedef` structures, `enum` constants, and macros. An important design issue is when and how to use these types in the interface and implementation of custom classes.

The following list offers some guidance on using C types in the definition of a custom class:

Define a function rather than a method for functionality that is often requested but doesn’t need to be overridden by subclasses. The reason for doing this is performance. In these cases, it’s best for the function to be private rather then being part of the class API. You can also implement functions for behavior that is not associated with any class (because it is global) or for operations on simple types (C primitives or structures). However, for global functionality it might be better—for reasons of extensibility—to create a class from which a singleton instance is generated.

Define a structure type rather than a simple class when the following conditions apply:

You don’t expect to augment the list of fields.

All the fields are public (for performance reasons).

None of the fields is dynamic (dynamic fields might require special handling, such as retaining or releasing).

You don’t intend to make use of object-oriented techniques, such as subclassing.

Even when all these conditions apply, the primary justification for structures in Objective-C code is performance. In other words, if there is no compelling performance reason, a simple class is preferable.

Declare `enum` constants rather than `#define` constants. The former are more suitable for typing, and you can discover their values in the debugger.

### When the Class Is Public (OS X)
When you are defining a custom class for your own use, such as a controller class for an application, you have great flexibility because you know the class well and can always redesign it if you have to. However, if your custom class will likely be a superclass for other developers—in other words, if it’s a public class—the expectations others have of your class mean that you have to be more careful of your design. 

Here are few guidelines for developers of public Cocoa classes:

Make sure those instance variables that subclasses need to access are scoped as `@protected`.

Add one or two reserved instance variables to help ensure binary compatibility with future versions of your class (see [Instance Variables](#//apple_ref/doc/uid/TP40002974-CH5-SW12)).

In memory-managed code, implement your accessor methods to properly handle memory management of vended objects (see [Storing and Accessing Properties](#//apple_ref/doc/uid/TP40002974-CH5-SW5)). Clients of your class should get back autoreleased objects from your getter accessors.

Observe the naming guidelines for public Cocoa interfaces as recommended in *[Coding Guidelines for Cocoa](../../CodingGuidelines/CodingGuidelines.html#//apple_ref/doc/uid/10000146i)*.

Document the class, at least with comments in the header file.

## Multithreaded Cocoa Programs
It has long been a standard programming technique to optimize program performance through multithreading. By having data processing and I/O operations run on their own secondary threads and dedicating the main thread to user-interface management, you can enhance the responsiveness of your applications. But in recent years, multithreading has assumed even greater importance with the emergence of  multicore computers and symmetric multiprocessing (SMP). Concurrent processing on such systems involves synchronizing the execution of not only multiple threads on the same processor but multiple threads on multiple processors.

But making your program multithreaded is not without risk or costs. Although threads share the virtual address space of their process, they still add to the process’s memory footprint; each thread requires an additional data structure, associated attributes, and its own stack space. Multithreading also requires greater complexity of program design to synchronize access to shared memory among multiple threads. Such designs make a program harder to maintain and debug. Moreover, multithreading designs that overuse locks can actually degrade performance (relative to a single-threaded program) because of the high contention for shared resources. Designing a program for multithreading often involves tradeoffs between performance and protection and requires careful consideration of your data structures and the intended usage pattern for extra threads. Indeed, in some situations the best approach is to avoid multithreading altogether and keep all program execution on the main thread.

> **Further Reading:** To get a solid understanding of issues and guidelines related to multithreading, read *[Threading Programming Guide](../../Multithreading/Introduction/Introduction.html#//apple_ref/doc/uid/10000057i)*. 

### Multithreading and Multiprocessing Resources
Cocoa provides several classes that are useful for multithreading programs.

#### Threads and Run Loops
In Cocoa  threads are represented by objects of the `[NSThread](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/cl/NSThread)` class. The implementation of the class is based upon POSIX threads. Although the interface of `NSThread` does not provide as many options for thread management as do POSIX threads, it is sufficient for most multithreading needs.

 Run loops are a key component of the architecture for the distribution of events. Run loops have input sources (typically ports or sockets) and timers; they also have input modes for specifying which input sources the run loop listens to. Objects of the `[NSRunLoop](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRunLoop/Description.html#//apple_ref/occ/cl/NSRunLoop)` class represent run loops in Cocoa. Each thread has its own run loop, and the run loop of the main thread is set up and run automatically when a process begins execution. Secondary threads, however, must run their own run loops.

#### Operations and Operation Queues
With the  `[NSOperation](https://developer.apple.com/documentation/foundation/nsoperation)` and `[NSOperationQueue](https://developer.apple.com/documentation/foundation/operationqueue)`  classes, you can manage the execution of one or more encapsulated tasks that may be concurrent or nonconcurrent. (Note that the word *task* here does not necessarily imply an `[NSTask](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTask/Description.html#//apple_ref/occ/cl/NSTask)` object; a task, or operation, for example, typically runs on a secondary thread within the same process.) An `NSOperation` object represents a discrete task that can be executed only once. Generally, you use `NSOperationQueue` objects to schedule operations in a sequence determined by priority and interoperation dependency. An operation object that has dependencies does not execute until all of its dependent operation objects finish executing. An operation object remains in a queue until it is explicitly removed or it finishes executing. 

> **Note:** The `[NSOperation](https://developer.apple.com/documentation/foundation/nsoperation)` and `[NSOperationQueue](https://developer.apple.com/documentation/foundation/operationqueue)` classes were introduced in OS X v10.5 and in the initial public version of the iOS SDK. They are not available in earlier versions of OS X.

#### Locks and Conditions
Locks act as resource sentinels for contending threads; they prevent threads from simultaneously accessing a shared resource. Several classes in Cocoa provide lock objects of different types.

**Table 3-1**  Lock classesClasses

Description

`[NSLock](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSLock/Description.html#//apple_ref/occ/cl/NSLock)`

Implements a mutex lock—a lock that enforces **mutually** **exclusive** access to a shared resource. Only one thread can acquire the lock and use the resource, thereby blocking other threads until it releases the lock.

`[NSRecursiveLock](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRecursiveLock/Description.html#//apple_ref/occ/cl/NSRecursiveLock)`

Implements a recursive lock—a mutex lock that can be acquired multiple times by the thread that currently owns it. The lock remains in place until each recursive acquisition of the lock has been balanced with a call that releases the lock.

`[NSConditionLock](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSConditionLock/Description.html#//apple_ref/occ/cl/NSConditionLock)` 

`[NSCondition](https://developer.apple.com/documentation/foundation/nscondition)`

Both of these classes implement condition locks. This type of lock combines a semaphore with a mutex lock to implement locking behavior that is effective based on a program-defined condition. A thread blocks and waits for another thread to signal the condition, at which point it (or another waiting thread) is unblocked and continues execution. You can signal multiple threads simultaneously, causing them to unblock and continue execution.

`NSConditionLock` and `NSCondition` are similar in purpose and implementation; both are object-oriented implementations of `pthread` condition locks. The implementation of `NSConditionLock` is more thorough but less flexible (although it does offer features such as locking timeouts); it implements both the semaphore signaling *and* the mutex locking for you.  The implementation of `NSCondition`, on the other hand, wraps the `pthread` condition variables and mutex locks and requires you to do your own locking, signaling, and predicate state management. However, it is more flexible. This approach closely follows the implementation pattern for `pthread` conditions. 

#### Interthread Communication
When your program has multiple threads, those threads often need a way to communicate with each other. The condition-lock classes use `pthread`-based condition signaling (semaphores) as a way to communicate between threads. But Cocoa offers several other resources for inter-thread communication.

Objects of the `[NSMachPort](https://developer.apple.com/documentation/foundation/nsmachport)` and `[NSMessagePort](https://developer.apple.com/documentation/foundation/nsmessageport)` classes enable inter-thread communication over Mach ports. Objects of the `[NSSocketPort](https://developer.apple.com/documentation/foundation/nssocketport)` class enable inter-thread *and* interprocess communication over BSD sockets.  In addition, the `NSObject` class provides the `[performSelectorOnMainThread:withObject:waitUntilDone:](https://developer.apple.com/documentation/objectivec/nsobject/1414900-performselector)` method, which allows secondary threads to communicate with and send data to the main thread.

### Multithreading Guidelines for Cocoa Programs
Whether your program uses the Cocoa frameworks or something else, there are some common observations that can be made about the appropriateness and wisdom of having multiple threads in your program. You might start by asking yourself the following questions; if you answer “yes” to any of them, you should consider multithreading:

Is there a CPU- or I/O-intensive task that could block your application’s user interface?

The main thread manages an application’s user interface—the view part of the Model-View-Controller pattern. Thus the application can perform work that involves the application’s data model on one or more secondary threads. In a Cocoa application, because controller objects are directly involved in the management of the user interface, you would run them on the main thread.

Does your program have multiple sources of input or multiple outputs, and is it feasible to handle them simultaneously?

Do you want to spread the work your program performs across multiple CPU cores?

Let’s say one of these situations applies to your application. As noted earlier, multithreading can involve costs and risks as well as benefits, so it’s worthwhile to consider alternatives to multithreading. For example, you might try asynchronous processing. 

Just as there are situations that are appropriate for multithreading, there are situations where it’s not:

The work requires little processing time.

The work requires steps that must be performed serially.

Underlying subsystems are not thread safe.

The last item brings up the important issue of thread safety. There are some guidelines you should observe to ensure—as much as possible—that your multithreaded code is thread safe. Some of these guidelines are general and others are specific to Cocoa programs. The general guidelines are as follows: 

Avoid sharing data structures across threads if possible.

Instead you can give each thread its own copy of an object (or other data) and then synchronize changes using a transaction-based model. Ideally you want to minimize resource contention as much as possible.

Lock data at the right level.

Locks are an essential component of multithreaded code, but they do impose a performance bottleneck and may not work as planned if used at the wrong level.

Catch local exceptions in your thread.

Each thread has its own call stack and is therefore responsible for catching any local exceptions and cleaning things up. The exception cannot be thrown to the main thread or any other thread for handling. Failure to catch and handle an exception may result in the termination of the thread or the owning process.

Avoid `volatile` variables in protected code.

The additional guidelines for Cocoa programs are few:

Deal with  immutable objects in your code as much as possible, especially across interface boundaries.

Objects whose value or content cannot be modified—immutable objects—are usually thread safe. Mutable objects are often not. As a corollary to this guideline, respect the declared mutability status of objects returned from method calls; if you receive an object declared as immutable but it is mutable and you modify it, you can cause behavior that is catastrophic to the program.

The main thread of a Cocoa application is responsible for receiving and dispatching events. Although your application can handle events on a secondary thread instead of the main thread, if it does it must keep all event-handling code on that thread. If you spin off the handling of events to different threads, user events (such as the letters typed on the keyboard) can occur out of sequence.

If a secondary thread is to draw a view in a Cocoa application in OS X, ensure that all drawing code falls between calls to the `NSView` methods `[lockFocusIfCanDraw](https://developer.apple.com/documentation/appkit/nsview/1483285-lockfocusifcandraw)` and `[unlockFocus](https://developer.apple.com/documentation/appkit/nsview/1483711-unlockfocus)`.

> **Note:** You can learn more about these guidelines in *[Threading Programming Guide](../../Multithreading/Introduction/Introduction.html#//apple_ref/doc/uid/10000057i)*.

 

### Are the Cocoa Frameworks Thread Safe?
Before you can make your program thread safe, it’s essential to know the thread safety of the frameworks that your program relies on. The core Cocoa frameworks, Foundation and AppKit, have parts that are thread safe and parts that are not. 

Generally, the classes of Foundation whose objects are immutable collections (`[NSArray](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSArrayClassCluster/Description.html#//apple_ref/occ/cl/NSArray)`, `[NSDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSDictionary)`, and so on) as well as those whose immutable objects encapsulate primitive values or constructs (for example, `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)`, `[NSNumber](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSNumber/Description.html#//apple_ref/occ/cl/NSNumber)`, `[NSException](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/cl/NSException)`) are thread safe. Conversely, objects of the mutable versions of these collection and primitive-value classes are not thread safe. A number of classes whose objects represent system-level entities—`[NSTask](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTask/Description.html#//apple_ref/occ/cl/NSTask)`, `[NSRunLoop](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSRunLoop/Description.html#//apple_ref/occ/cl/NSRunLoop)`, `[NSPipe](https://developer.apple.com/documentation/foundation/nspipe)`, `[NSHost](https://developer.apple.com/documentation/foundation/host)`, and `[NSPort](https://developer.apple.com/documentation/foundation/nsport)`, among others—are not thread safe. (`[NSThread](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSThread/Description.html#//apple_ref/occ/cl/NSThread)` and the lock classes, on the other hand, are thread safe.)

In AppKit, windows (`[NSWindow](https://developer.apple.com/documentation/appkit/nswindow)` objects) are generally thread safe in that you can create and manage them on a secondary thread. Events (`[NSEvent](https://developer.apple.com/documentation/appkit/nsevent)` objects), however, are safely handled only on the same thread, whether that be the main thread or a secondary thread; otherwise you run the risk of having events get out of sequence. Drawing and views (`[NSView](https://developer.apple.com/documentation/appkit/nsview)` objects) are generally thread safe, although operations on views such as creating, resizing, and moving should happen on the main thread.

All UIKit objects should be used on the main thread only.

For more information on the thread safety of the Cocoa frameworks, see “[Thread Safety Summary](../../Multithreading/ThreadSafetySummary/ThreadSafetySummary.html#//apple_ref/doc/uid/10000057i-CH12)“ in *[Threading Programming Guide](../../Multithreading/Introduction/Introduction.html#//apple_ref/doc/uid/10000057i)*.

        
            [Next](../CocoaDesignPatterns/CocoaDesignPatterns.html)[Previous](../CocoaObjects/CocoaObjects.html)
        
         Copyright &#x00a9; 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-09-18