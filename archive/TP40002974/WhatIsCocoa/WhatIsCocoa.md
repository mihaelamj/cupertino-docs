---
title: "What Is Cocoa?"
book: "Cocoa Fundamentals Guide"
framework: "Cocoa"
chapterId: "TP40002974-CH3"
date: "2013-09-18"
description: "Introduces the basic concepts, terminology, architectures, and design patterns of the Cocoa and Cocoa Touch frameworks and development environment."
identifier: "//apple_ref/doc/uid/TP40002974"
source: apple-archive
---

# What Is Cocoa?

> **Cocoa Fundamentals Guide**
> Last updated: 2013-09-18

[Next](../CocoaObjects/CocoaObjects.html)[Previous](../Introduction/Introduction.html)
        
        
        [](../index.html)
        
# Retired Document
**Important:**
This document may not represent best practices for current development. Links to downloads and other resources may no longer be valid.
        # What Is Cocoa?
Cocoa is an application environment for both the OS X operating system and iOS, the operating system used on Multi-Touch devices such as iPhone, iPad, and iPod touch. It consists of a suite of object-oriented software libraries, a runtime system, and an integrated development environment. 

This chapter expands on this definition, describing the purpose, capabilities, and components of Cocoa on both platforms. Reading this functional description of Cocoa is an essential first step for a developer trying to understand Cocoa.

## The Cocoa Environment
Cocoa is a set of object-oriented frameworks that provides a runtime environment for applications running in OS X and iOS. Cocoa is the preeminent application environment for OS X and the *only* application environment for iOS. (Carbon is an alternative environment in OS X, but it is a compatibility framework with procedural programmatic interfaces intended to support existing OS X code bases.) Most of the applications you see in OS X and iOS, including Mail and Safari, are Cocoa applications. An integrated development environment called Xcode supports application development for both platforms. The combination of this development environment and Cocoa makes it easy to create a well-factored, full-featured application. 

### Introducing Cocoa
As with all application environments, Cocoa presents two faces; it has a runtime aspect and a development aspect. In its runtime aspect, Cocoa applications present the user interface and are tightly integrated with the other visible components of the operating system; in OS X, these include the Finder, the Dock, and other applications from all environments. 

But it is the development aspect that is the more interesting one to programmers. Cocoa is an integrated suite of object-oriented software components—classes—that enables you to rapidly create robust, full-featured OS X and iOS applications. These classes are reusable and adaptable software building blocks; you can use them as-is or extend them for your specific requirements. Cocoa classes exist for just about every conceivable development necessity, from user-interface objects to data formatting. Where a development need hasn’t been anticipated, you can easily create a subclass of an existing class that answers that need. 

Cocoa has one of the most distinguished pedigrees of any object-oriented development environment. From its introduction as NeXTSTEP in 1989 to the present day, it has been continually refined and tested (see [A Bit of History](#//apple_ref/doc/uid/TP40002974-CH3-SW12)). Its elegant and powerful design is ideally suited for the rapid development of software of all kinds, not only applications but command-line tools, plug-ins, and various types of bundles. Cocoa gives your application much of its behavior and appearance “for free,” freeing up more of your time to work on those features that are distinctive. (For details on what Cocoa offers, see [Features of a Cocoa Application](#//apple_ref/doc/uid/TP40002974-CH3-SW6).)

> **iOS Note:** Cocoa for iOS supports only application development and not development of any other kind of executable.

You can use several programming languages when developing Cocoa software, but the essential, required language is Objective-C. Objective-C is a superset of ANSI C that has been extended with certain syntactical and semantic features (derived from Smalltalk) to support object-oriented programming. The few added conventions are easy to learn and use. Because Objective-C rests on a foundation of ANSI C, you can freely intermix straight C code with Objective-C code. Moreover, your code can call functions defined in non-Cocoa programmatic interfaces, such as the BSD library interfaces in `/usr/include`. You can even mix C++ code with your Cocoa code and link the compiled code into the same executable. 

> **OS X Note:** In OS X, you can also program in Cocoa using scripting bridges such as PyObjC (the Python–Objective-C bridge) and RubyCocoa (the Ruby–Cocoa bridge). Both bridged languages let you write Cocoa applications in the respective scripting languages, Python and Ruby. Both of these are interpreted, interactive, and object-oriented programming languages that make it possible for Python or Ruby objects to send messages to Objective-C objects as if they were Python or Ruby objects, and also for Objective-C objects to send messages to Python or Ruby objects. For more information, see *[Ruby and Python Programming Topics for Mac](../../RubyPythonCocoa/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004936)*.

The most important Cocoa class libraries come packaged in two core frameworks for each platform: Foundation and AppKit for OS X, and Foundation and UIKit for iOS. As with all frameworks, these contain not only a dynamically sharable library (or sometimes several versions of libraries required for backward compatibility), but header files, API documentation, and related resources. The pairing of Foundation with AppKit or UIKit reflects the division of the Cocoa programmatic interfaces into those classes that are not related to a graphical user interface and those that are. For each platform, its two core frameworks are essential to any Cocoa project whose end product is an application. Both platforms additionally support the  Core Data framework, which is as important and useful as the core frameworks. 

OS X also ships with several other frameworks that publish Cocoa programmatic interfaces, such as the WebKit and Address Book frameworks; more Cocoa frameworks will be added to the operating system over time. See [The Cocoa Frameworks](#//apple_ref/doc/uid/TP40002974-CH3-SW10) for further information.

### How Cocoa Fits into OS X
Architecturally, OS X is a series of software layers going from the foundation of Darwin to the various application frameworks and the user experience they support. The intervening layers represent the system software largely (but not entirely) contained in the two major umbrella frameworks, Core Services and Application Services. A component at one layer generally has dependencies on the layer beneath it. Figure 1-1 situates Cocoa in this architectural setting. 

**Figure 1-1**  Cocoa in the architecture of OS X For example, the system component that is largely responsible for rendering the Aqua user interface, Quartz (implemented in the Core Graphics framework), is part of the Application Services layer. And at the base of the architectural stack is Darwin; everything in OS X, including Cocoa, ultimately depends on Darwin to function.

In OS X, Cocoa has two *core* Objective-C frameworks that are essential to application development for OS X: 

**AppKit.** AppKit, one of the application frameworks, provides the objects an application displays in its user interface and defines the structure for application behavior, including event handling and drawing. For a description of AppKit, see [AppKit (OS X)](#//apple_ref/doc/uid/TP40002974-CH3-SW25). 

**Foundation.** This framework, in the Core Services layer, defines the basic behavior of objects, establishes mechanisms for their management, and provides objects for primitive data types, collections, and operating-system services. Foundation is essentially an object-oriented version of the Core Foundation framework; see [Foundation](#//apple_ref/doc/uid/TP40002974-CH3-SW20) for a discussion of the Foundation framework.

AppKit has close, direct dependences on Foundation, which functionally is in the Core Services layer. If you look closer, at individual, or groups, of Cocoa classes and at particular frameworks, you begin to see where Cocoa either has specific dependencies on other parts of OS X or where it exposes underlying technology with its interfaces. Some major underlying frameworks on which Cocoa depends or which it exposes through its classes and methods are Core Foundation, Carbon Core, Core Graphics (Quartz), and Launch Services:

*Core Foundation*. Many classes of the Foundation framework are based on equivalent Core Foundation opaque types. This close relationship is what makes “toll-free bridging”—cast-conversion between compatible Core Foundation and Foundation types—possible. Some of the implementation of Core Foundation, in turn, is based on the BSD part of the Darwin layer. 

*Carbon Core*. AppKit and Foundation tap into the Carbon Core framework for some of the system services it provides. For example, Carbon Core has the File Manager, which Cocoa uses for conversions between various file-system representations.

*Core Graphics*. The Cocoa drawing and imaging classes are (quite naturally) closely based on the Core Graphics framework, which implements Quartz and the window server.

*Launch Services*. The `NSWorkspace` class exposes the underlying capabilities of Launch Services. Cocoa also uses the application-registration feature of Launch Services to get the icons associated with applications and documents.

> **Note:** The intent of this architectural overview is not to itemize every relationship that Cocoa has to other parts of OS X. Instead, it surveys the more interesting ones in order to give you a general idea of the architectural context of the framework.

Apple has carefully designed Cocoa so that some of its programmatic interfaces give access to the capabilities of underlying technologies that applications typically need. But if you require some capability that is not exposed through the programmatic interfaces of Cocoa, or if you need some finer control of what happens in your application, you may be able to use an underlying framework directly. (A prime example is Core Graphics; by calling its functions or those of OpenGL, your code can draw more complex and nuanced images than is possible with the Cocoa drawing methods.) Fortunately, using these lower-level frameworks is not a problem because the programmatic interfaces of most dependent frameworks are written in standard ANSI C, of which Objective-C language is a superset.

> **Further Reading:** *[Mac Technology Overview](../../../../MacOSX/Conceptual/OSX_Technology_Overview/About/About.html#//apple_ref/doc/uid/TP40001067)* gives an overview of the frameworks, services, technologies, and other components of OS X. *OS X Human Interface Guidelines* specifies how the Aqua human interface should appear and behave.

### How Cocoa Fits into iOS
The application-framework layer of iOS is called Cocoa Touch. Although the iOS infrastructure on which Cocoa Touch depends is similar to that for Cocoa in OS X, there are some significant differences. Compare Figure 1-2, which depicts the architectural setting of iOS, to the diagram in [Figure 1-1](#//apple_ref/doc/uid/TP40002974-CH3-SW5). The iOS diagram also shows the software supporting its platform as a series of layers going from a Core OS foundation to a set of application frameworks, the most critical (for applications) being the UIKit framework. As in the OS X diagram, the iOS diagram has middle layers consisting of core-services frameworks and graphics and media frameworks and libraries. Here also, a component at one layer often has dependencies on the layer beneath it. 

**Figure 1-2**  Cocoa in the architecture of iOSGenerally, the system libraries and frameworks of iOS that ultimately support UIKit are a subset of the libraries and frameworks in OS X. For example, there is no Carbon application environment in iOS, there is no command-line access (the BSD environment in Darwin), there are no printing frameworks and services, and QuickTime is absent from the platform. However, because of the nature of the devices supported by iOS, there are some frameworks, both public and private, that are specific to iOS. 

The following summarizes some of the frameworks found at each layer of the iOS stack, starting from the foundation layer.

**Core OS.** This level contains the kernel, the file system, networking infrastructure, security, power management, and a number of device drivers. It also has the libSystem library, which supports the POSIX/BSD 4.4/C99 API specifications and includes system-level APIs for many services.

**Core Services.** The frameworks in this layer provide core services, such as string manipulation, collection management, networking, URL utilities, contact management, and preferences. They also provide services based on hardware features of a device, such as the GPS, compass, accelerometer, and gyroscope. Examples of frameworks in this layer are Core Location, Core Motion, and System Configuration. 

This layer includes both Foundation and Core Foundation, frameworks that provide abstractions for common data types such as strings and collections. The Core Frameworks layer also contains Core Data, a framework for object graph management and object persistence.

**Media.** The frameworks and services in this layer depend on the Core Services layer and provide graphical and multimedia services to the Cocoa Touch layer. They include Core Graphics, Core Text, OpenGL ES, Core Animation, AVFoundation, Core Audio, and video playback.

**Cocoa Touch.** The frameworks in this layer directly support applications based in iOS. They include frameworks such as Game Kit, Map Kit, and iAd.

The Cocoa Touch layer and the Core Services layer each has an Objective-C framework that is especially important for developing applications for iOS. These are the *core* Cocoa frameworks in iOS:

**UIKit.** This framework provides the objects an application displays in its user interface and defines the structure for application behavior, including event handling and drawing. For a description of UIKit, see [UIKit (iOS)](#//apple_ref/doc/uid/TP40002974-CH3-SW22). 

**Foundation.** This framework defines the basic behavior of objects, establishes mechanisms for their management, and provides objects for primitive data types, collections, and operating-system services. Foundation is essentially an object-oriented version of the Core Foundation framework; see [Foundation](#//apple_ref/doc/uid/TP40002974-CH3-SW20) for a discussion of the Foundation framework.

> **Notes:** This document uses “Cocoa” generically when referring to things that are common between the platforms. When it is necessary to say something specific about Cocoa on a given platform, it uses a phrase such as “Cocoa in OS X.” 

As with Cocoa in OS X, the programmatic interfaces of Cocoa in iOS give your applications access to the capabilities of underlying technologies. Usually there is a Foundation or UIKit method or function that can tap into a lower-level framework to do what you want. But, as with Cocoa in OS X, if you require some capability that is not exposed through a Cocoa API, or if you need some finer control of what happens in your application, you may choose to use an underlying framework directly. For example, UIKit uses the WebKit to draw text and publishes some methods for drawing text; however, you may decide to use Core Text to draw text because that gives you the control you need for text layout and font management. Again, using these lower-level frameworks is not a problem because the programmatic interfaces of most dependent frameworks are written in standard ANSI C, of which the Objective-C language is a superset.

> **Further Reading:** To learn more about the frameworks, services, and other aspects of the iOS platform, see *iOS Technology Overview*.

## Features of a Cocoa Application
In OS X it is possible to create a Cocoa application without adding a single line of code. You make a new Cocoa application project using Xcode and then build the project. That’s it. Of course, this application won’t do much, or at least much that’s interesting. But this extremely simple application still launches when double-clicked, displays its icon in the Dock, displays its menus and window (titled “Window”), hides itself on command, behaves nicely with other running applications, and quits on command. You can move, resize, minimize, and close the window. You can even print the emptiness contained by the window. 

You can do the same with an iOS application. Create a project in Xcode using one of the project templates, immediately build it, and run it in the iOS Simulator. The application quits when you click the Home button (or press it on a device). To launch the application, click its icon in Simulator. It may even have additional behaviors; for example, with an application made from the Utility Application template, the initial view "flips” to a second view when you click or tap the information (“i”) icon.

Imagine what you could do with a little code. 

> **iOS Note:** The features and behavior of an application running in iOS are considerably different from a Mac app, largely because it runs in a more constrained environment. For discussions of application capabilities and constraints in iOS, see *[App Programming Guide for iOS](../../../../iPhone/Conceptual/iPhoneOSProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007072)*. 

In terms of programming effort, Cocoa gives you, the developer, much that is free and much that is low-cost. Of course, to become a productive Cocoa developer means becoming familiar with possibly new concepts, design patterns, programming interfaces, and development tools, and this effort is not negligible. But familiarity yields greater productivity. Programming becomes largely an exercise in assembling the programmatic components that Cocoa provides along with the custom objects and code that define your program’s particular logic, then fitting everything together.

What follows is a short list of how Cocoa adds value to an application with only a little (and sometimes no) effort on your part:

*Basic application framework*—Cocoa provides the infrastructure for event-driven behavior and for management of applications, windows, and (in the case of OS X) workspaces. In most cases, you won’t have to handle events directly or send any drawing commands to a rendering library.

*User-interface objects*—Cocoa offers a rich collection of ready-made objects for your application’s user interface. Most of these objects are available in the library of Interface Builder, a development application for creating user interfaces; you simply drag an object from the library onto the surface of your interface, configure its attributes, and connect it to other objects. (And, of course, you can always instantiate, configure, and connect these objects programmatically.) 

Here is a sampling of Cocoa user-interface objects:

Windows

Text fields

Image views

Date pickers

Sheets and dialogs

Segmented controls

Table views

Progress indicators

Buttons

Sliders

Radio buttons (OS X)

Color wells (OS X)

Drawers (OS X)

Page controls (iOS)

Navigation bars (iOS)

Switch controls (iOS)

Cocoa in OS X also features technologies that support user interfaces, including those that promote accessibility, perform validation, and facilitate the connections between objects in the user interface and custom objects. 

*Drawing and imaging*—Cocoa enables efficient drawing of custom views with a framework for locking graphical focus and marking views (or portions of views) as “dirty.” Cocoa includes programmatic tools for drawing Bezier paths, performing affine transforms, compositing images, generating PDF content, and (in OS X) creating various representations of images. 

*System interaction*—In OS X, Cocoa gives your application ways to interact with (and use the services of) the file system, the workspace, and other applications. In iOS, Cocoa lets you pass URLs to applications to have them handle the referenced resource (for example, email or websites); it also provides support for managing user interactions with files in the local system and for scheduling local notifications.

*Performance*—To enhance the performance of your application, Cocoa provides programmatic support for concurrency, multithreading, lazy loading of resources, memory management, and run-loop manipulation. 

*Internationalization*—Cocoa provides a rich architecture for internationalizing applications, making it possible for you to support localized resources such as text, images, and even user interfaces. The Cocoa approach is based on users’ lists of preferred languages and puts localized resources in bundles of the application. Based on the settings it finds, Cocoa automatically selects the localized resource that best matches the user’s preferences. It also provides tools and programmatic interfaces for generating and accessing localized strings. Moreover, text manipulation in Cocoa is based on Unicode by default, and is thus an asset for internationalization.

*Text*—In OS X, Cocoa provides a sophisticated text system that allows you to do things with text ranging from the simple (for example, displaying a text view with editable text) to the more complex, such as controlling kerning and ligatures, spell checking, regular expressions, and embedding images in text. Although Cocoa in iOS has no native text system (it uses WebKit for string drawing) and its text capabilities are more limited, it still includes support for spellchecking, regular expressions, and interacting with the text input system.

*Preferences*—The user defaults system is based on a systemwide database in which you can store global and application-specific preferences. The procedure for specifying application preferences is different for OS X and iOS.

*Networking*—Cocoa also offers programmatic interfaces for communicating with servers using standard Internet protocols, communicating via sockets, and taking advantage of Bonjour, which lets your application publish and discover services on an IP network.

In OS X, Cocoa includes a distributed objects architecture that allows one Cocoa process to communicate with another process on the same computer or on a different one. In iOS, Cocoa supports the capability for servers to push notifications to devices for applications registered to received such notifications.

*Printing*—Cocoa on both platforms supports printing. Their printing architecture lets you print images, documents, and other application content along a range of control and sophistication. At the simplest level, you can print the contents of any view or print an image or PDF document with just a little code. At a more complicated level, you can define the content and format of printed content, control how a print job is performed, and do pagination. In OS X, you can add an accessory view to the Print dialog. 

*Undo management*—You can register user actions that occur with an undo manager, and it will take care of undoing them (and redoing them) when users choose the appropriate menu items. The manager maintains undo and redo operations on separate stacks. 

*Multimedia*—Both platforms programmatically support video and audio. In OS X, Cocoa offers support for QuickTime video.

*Data exchange*—Cocoa simplifies the exchange of data within an application and between applications using the copy-paste model. In OS X, Cocoa also supports drag-and-drop models and the sharing of application capabilities through the Services menu. 

Cocoa in OS X has a couple of other features:

*Document-based applications*—Cocoa specifies an architecture for applications composed of a potentially unlimited number of documents, with each contained in its own window (a word processor, for example). Indeed, if you choose the “Document-based application” project type in Xcode, many of the components of this sort of application are created for you.

*Scripting*— Through application scriptability information and a suite of supporting Cocoa classes, you can make your application scriptable; that is, it can respond to commands emitted by AppleScript scripts. Applications can also execute scripts or use individual Apple events to send commands to, and receive data from, other applications. As a result, every scriptable application can supply services to both users and other applications.

## The Development Environment
You develop Cocoa software primarily by using the two developer applications, Xcode and Interface Builder. It is possible to develop Cocoa applications without using these applications at all. For example, you could write code using a text editor such as Emacs, build the application from the command line using makefiles, and debug the application from the command line using the `gdb` debugger. But why would you want to give yourself so much grief?

> **Note:** "Xcode” is sometimes used to refer to the complete suite of development tools and frameworks, and other times specifically to the  application that allows you to manage projects, edit source code, and build executable code. 

The origins of Xcode and Interface Builder coincide with the origins of Cocoa itself, and consequently there is a high degree of compatibility between tools and frameworks. Together, Xcode and Interface Builder make it extraordinarily easy to design, manage, build, and debug Cocoa software projects. 

When you install the development tools and documentation, you may select the installation location. Traditionally that location has been `/Developer`, but it can be anywhere in the file system you wish. To designate this installation location, the documentation uses `<Xcode>`. Thus, the development applications are installed in `<Xcode>/Applications`. 

### Platform SDKs
Beginning with Xcode 3.1 and the introduction of iOS, when you create a software project you must choose a platform SDK. The SDK enables you to build an executable that is targeted for a particular release of OS X or iOS. 

The platform SDK contains everything that is required for developing software for a given platform and operating-system release. An OS X SDK consists of frameworks, libraries, header files, and system tools. The SDK for iOS has the same components, but includes a platform-specific compiler and other tools. There is also a separate SDK for iOS Simulator (see [The iOS Simulator Application](#//apple_ref/doc/uid/TP40002974-CH3-SW23)). All SDKs include build settings and project templates appropriate to their platform.

> **Further reading:** For more on platform SDKs, see *[SDK Compatibility Guide](../../../../DeveloperTools/Conceptual/cross_development/Introduction/Introduction.html#//apple_ref/doc/uid/10000163i)*.

### Overview of Development Workflows
Application development differs for OS X and iOS, not only in the tools used but in the development workflow. 

In OS X, the typical development workflow is the following: 

In Xcode, [create a project](../../../../../recipes/XcodeRecipes/Creating_a_Project/CreateProject.html#//apple_ref/doc/uid/TP40009043-CH10) using a template from the OS X SDK.

Write code and, using Interface Builder, construct your application’s user interface.

[Define the targets and executable environment](../../../../../recipes/XcodeRecipes/Editing_Build_Settings/BuildSettings.html#//apple_ref/doc/uid/TP40009043-CH2) for your project.

Test and debug the application using the Xcode debugging facilities.

As part of debugging, you can check the system logs in the Console window.

Measure application performance using one or more of the available performance tools.

For iOS development, the workflow when developing an application is a bit more complex. Before you can develop for iOS, you must register as a developer for the platform. Thereafter, building an application that’s ready to deploy requires that you go through the following steps:

Configure the remote device. 

This configuration results in the required tools, frameworks, and other components being installed on the device.

In Xcode, create a project using a template from the iOS SDK.

Write code, and construct your application’s user interface.

Define the targets and executable environment for the project.

Build the application (locally).

Test and debug the application, either in iOS Simulator or remotely in the device. (If remotely, your debug executable is downloaded to the device.)

As you debug, you can check the system logs for the device in the Console window.

Measure application performance using one or more of the available performance tools.

> **Further reading:** For more on the development workflow in iOS, see *Tools Workflow Guide for iOS*.

### Xcode
Xcode is the engine that powers Apple’s integrated development environment (IDE) for OS X and iOS. It is also an application that takes care of most project details from inception to deployment. It allows you to:

Create and manage projects, including specifying platforms, target requirements, dependencies, and build configurations.

Write source code in editors with features such as syntax coloring and automatic indenting.

Navigate and search through the components of a project, including header files and documentation.

Build the project.

Debug the project locally, in iOS Simulator, or remotely, in a graphical source-level debugger.

Xcode builds projects from source code written in C, C++, Objective-C, and Objective-C++. It generates executables of all types supported in OS X, including command-line tools, frameworks, plug-ins, kernel extensions, bundles, and applications. (For iOS, only application executables are possible.) Xcode permits almost unlimited customization of build and debugging tools, executable packaging (including information property lists and localized bundles), build processes (including copy-file, script-file, and other build phases), and the user interface (including detached and multi-view code editors). Xcode also supports several source-code management systems—namely CVS, Subversion, and Perforce—allowing you to add files to a repository, commit changes, get updated versions, and compare versions.

Figure 1-3 shows an example of a project in Xcode.

**Figure 1-3**  The TextEdit example project in XcodeXcode is especially suited for Cocoa development. When you create a project, Xcode sets up your initial development environment using project templates corresponding to Cocoa project types: application, document-based application, Core Data application, tool, bundle, framework, and others. For compiling Cocoa software, Xcode gives you several options:    

GCC—The GNU C compiler (`gcc`).

LLVM-GCC—A configuration where GCC is used as the front end for the LLVM (Low Level Virtual Machine) compiler. LLVM offers fast optimization times and high-quality code generation.

This option is available only for projects built for OS X v10.6 and later.

Clang—A front end specifically designed for the LLVM compiler. Clang offers fast compile times and excellent diagnostics. 

This option is available only for projects built for OS X v10.6 and later.

For details about these compiler options, see *[Xcode Build System Guide](../../../../DeveloperTools/Conceptual/XcodeBuildSystem/000-Introduction/Introduction.html#//apple_ref/doc/uid/TP40006904)*.

For debugging software, Xcode offers the GNU source-level debugger (`gdb`) and the Clang Static Analyzer. The Clang Static Analyzer consists of a framework for source-code analysis and a standalone tool that finds bugs in C and Objective-C programs. See [http://clang-analyzer.llvm.org/](http://clang-analyzer.llvm.org/) for more information.

Xcode is well integrated with the other major development application, Interface Builder. See Interface Builder for details.

> **Further Reading:** *[A Tour of Xcode](../../../../DeveloperTools/Conceptual/A_Tour_of_Xcode/000-Introduction/qt_intro.html#//apple_ref/doc/uid/TP30000890)* gives an overview of Xcode and provides links to additional development-tools documentation.

### Interface Builder
The second major development application for Cocoa projects is Interface Builder. As its name suggests, Interface Builder is a graphical tool for creating user interfaces. Interface Builder has been around almost since the inception of Cocoa as NeXTSTEP. Not surprisingly, its integration with Cocoa is airtight.

Interface Builder is centered around four main design elements:

*Nib files*. A nib file is a file wrapper (an opaque directory) that contains the objects appearing on a user interface in an archived form. Essentially, the archive is an object graph that contains information about each object, including its size and location, about the connections between objects, and about proxy references for custom classes. When you create and save a user interface in Interface Builder, all information necessary to re-create the interface is stored in the nib file. 

Nib files offer a way to easily localize user interfaces. Interface Builder stores a nib file in a localized directory inside a Cocoa project; when that project is built, the nib file is copied to a corresponding localized directory in the created bundle. 

Interface Builder presents the contents of a nib file in a nib document window (also called a *nib file window*). The nib document window gives you access to the important objects in a nib file, especially top-level objects such as windows, menus, and controller objects that have no parent in their object graph. (Controller objects mediate between user-interface objects and the model objects that represent application data; they also provide overall management for an application.)

Figure 1-4 shows a nib file opened in Interface Builder and displayed in a nib document window, along with supporting windows.

**Figure 1-4**  The TextEdit Document Properties window in Interface Builder*Object library*. The Library window of Interface Builder  contains objects that you can place on a user interface. They range from typical UI objects—for example, windows, controls, menus, text views, and outline views—to controller objects, custom view objects, and framework-specific objects, such as the Image Kit browser view. The Library groups the objects by categories and lets you browse through them and search for specific objects. When an object is dragged from the Library to an interface, Interface Builder instantiates a default instance of that object. You can resize, configure, and connect the object to other objects using the inspector. 

*Inspector*. Interface Builder has the inspector, a window for configuring the objects of a user interface. The inspector has a number of selectable panes for setting the initial runtime configuration of objects (although size and some attributes can also be set by direct manipulation). The inspector in Figure 1-4 shows the primary attributes for a text field; note that different collapsible sections of the pane reveal attributes at various levels of the inheritance hierarchy (text field, control, and view). In addition to primary attributes and size, the inspector features panes for animation effects, event handlers, and target-action connections between objects; for nib files in OS X projects, there are additional panes for AppleScript and bindings.

**Connections panel**. The connections panel is a context-sensitive display that shows the current outlet and action connections for a selected object and lets you manage those connections. To get the connections panel to appear, Control-click the target object. Figure 1-5 shows what the connections panel looks like.

**Figure 1-5**  The Interface Builder connections panel
Interface Builder uses blue lines that briefly appear to show the compliance of each positioned object, when moved or resized, to the Aqua human interface guidelines. This compliance includes recommended size, alignment, and position relative to other objects on the user interface and to the boundaries of the window.

Interface Builder is tightly integrated with Xcode. It “knows” about the outlets, actions, and bindable properties of your custom classes. When you add, remove, or modify any of these things, Interface Builder detects those changes and updates its presentation of them.

> **Further Reading:** For further information on Interface Builder, see *[Interface Builder User Guide](../../../../DeveloperTools/Conceptual/IB_UserGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40005344)*. Also refer to [Communicating with Objects](../CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW15) for overviews of outlets, the target-action mechanism, and the Cocoa bindings technology.

### The iOS Simulator Application
For iOS projects, you can select iOS Simulator as the platform SDK for the project. When you build and run the project, Xcode runs Simulator, which presents your application as it would appear on the device (iPhone or iPad) and allows you to manipulate parts of the user interface. You can use Simulator to help you debug the application prior to loading it onto the device.

You should always perform the final phase of debugging on the device. Simulator does not perfectly simulate the device. For example, you must use the mouse pointer instead of finger touches, and so manipulations of the interface requiring multiple fingers are not possible. In addition, Simulator does not use versions of the OpenGL framework that are specific to iOS, and it uses the OS X versions of the Foundation, Core Foundation, and CFNetwork frameworks, as well as the OS X version of libSystem.

More importantly, you should not assume that the performance of your application on Simulator is the same as it would be on the device. Simulator is essentially running your iOS application as a "guest” Mac app. As such, it has a 4 GB memory partition and swap space available to it as it runs on a processor that is more powerful than the one on the device.

> **Further reading:** For more on iOS Simulator, see *Tools Workflow Guide for iOS*. 

### Performance Applications and Tools
Although Xcode and Interface Builder are the major tools you use to develop Cocoa applications, there are dozens of other tools at your disposal. Many of these tools are performance applications.

#### Instruments
Instruments is an application introduced in Xcode 3.0 that lets you run multiple performance-testing tools simultaneously and view the results in a timeline-based graphical presentation.  It can show you CPU usage, disk reads and writes, memory statistics, thread activity, garbage collection, network statistics, directory and file usage, and other measurements—individually or in different combinations—in the form of graphs tied to time. This simultaneous presentation of instrumentation data helps you to discover the relationships between what is being measured.  It also displays the specific data behind the graphs. 

> **Further Reading:** See the *[Instruments User Guide](../../../../DeveloperTools/Conceptual/InstrumentsUserGuide/index.html#//apple_ref/doc/uid/TP40004652)* for complete information about the Instruments application.

**Figure 1-6**  The Instruments application#### Shark
Shark is a performance-analysis application that creates a time-based profile of your program’s execution; over a given period it traces function calls and graphs memory allocations. You can use Shark to track information for a single program or for the entire system, which in OS X includes kernel components such as drivers and kernel extensions. Shark also monitors file-system calls, traces system calls and memory allocations, performs static analyses of your code, and gathers information about cache misses, page faults, and other system metrics. Shark supports the analysis of code written in C, Objective-C, C++, and other languages.

#### Other Performance Applications (OS X)
Many applications are used in measuring and analyzing aspects of an OS X program’s performance. These applications are located in `<Xcode>/Applications/Performance Tools`. 

**BigTop** graphs performance trends over time, providing a real-time display of memory usage, page faults, CPU usage, and other data.

**Spin Control** automatically samples unresponsive applications. You leave Spin Control running in the background while you launch and test your applications. If applications become unresponsive to the point where the spinning cursor appears, Spin Control automatically samples your application to gather information about what your application was doing during that time.

*MallocDebug* shows all currently allocated blocks of memory in your program, organized by the call stack at the time of allocation. At a glance you can see how much allocated memory your application consumes, where that memory was allocated from, and which functions allocated large amounts of memory. MallocDebug can also find allocated memory that is not referenced elsewhere in the program, thus helping you find leaks and track down exactly where the memory was allocated.

*QuartzDebug* is a tool to help you debug how your application displays itself. It is especially useful for applications that do significant amounts of drawing and imaging. QuartzDebug has several debugging options, including the following: 

Auto-flush drawing, which flushes the contents of graphics contexts after each drawing operation

A mode that paints regions of the screen in yellow just before they’re updated

An option that takes a static snapshot of the systemwide window list, showing the owner of each window and how much memory each window consumes

For performance analysis, you can also use command-line tools such as:

`top`, which shows a periodically sampled set of statistics on currently running processes

`gprof`, which produces an execution profile of a program

`fs_usage`, which displays file-system access statistics

Many other command-line tools for performance analysis and other development tasks are available. Some are located in `/usr/bin` and `/usr/sbin`, and some Apple-developed command-line tools are installed in `<Xcode>/Tools`. For many of these tools you can consult their manual page for usage information. (To do this, either choose Help > Open man page in Xcode or type `man` followed by the name of the tool in a Terminal shell.)

> **Further Reading:** For more on the performance tools and applications you can use in Cocoa application development, as well as information on concepts, techniques, guidelines, and strategy related to performance, see *[Performance Overview](../../../../Performance/Conceptual/PerformanceOverview/Introduction/Introduction.html#//apple_ref/doc/uid/TP40001410)*. *[Cocoa Performance Guidelines](../../CocoaPerformance/CocoaPerformance.html#//apple_ref/doc/uid/TP40001448)* covers the performance guidelines for Cocoa.

## The Cocoa Frameworks
What makes a program a Cocoa program? It’s not really the language, because you can use a variety of languages in Cocoa development. It’s not the development tools, because you could create a Cocoa application from the command line (although that would be a complex, time-consuming task). No, what all Cocoa programs have in common—what makes them distinctive—is that they are composed of objects that inherit ultimately from the root class, `NSObject`, and that are ultimately based upon the Objective-C runtime. This statement is also true of all Cocoa frameworks.

> **Note:** The statement about the root class needs to be qualified a bit. First, the Foundation framework supplies another root class, `NSProxy`; however, `NSProxy` is rarely used in Cocoa programming. Second, you could create your own root class, but this would be a lot of work (entailing the writing of code that interacts with the Objective-C runtime) and probably not worth your time.

On any system there are many Cocoa frameworks, and Apple and third-party vendors are releasing more frameworks all the time. Despite this abundance of Cocoa frameworks, two of them stand out on each platform as core frameworks: 

In OS X: Foundation and AppKit

In iOS: Foundation and UIKit

The Foundation, AppKit, and UIKit frameworks are essential to Cocoa application development, and all other frameworks are secondary and elective. You cannot develop a Cocoa application for OS X unless you link against (and use the classes of) the AppKit, and you cannot develop a Cocoa application for iOS unless you link against (and use the classes of) UIKit. Moreover, you cannot develop Cocoa software of any kind unless you link against and use the classes of the Foundation framework. (Linking against the right frameworks in OS X happens automatically when you link against the Cocoa umbrella framework.) Classes, functions, data types, and constants in Foundation and the AppKit have a prefix of “NS”; classes, functions, data types, and constants in UIKit have a prefix of “UI”.

> **Note:** In OS X version 10.5 the Cocoa frameworks were ported to support 64-bit addressing. iOS also supports 64-bit addressing. As part of this effort, various general changes have been made to the Cocoa API, most significantly the introduction of the `NSInteger` and `NSUInteger` types (replacing `int` and `unsigned int` where appropriate) and the `CGFloat` type (replacing most instances of `float`). Most Cocoa applications have no immediate need to make the transition to 64-bit, but for those that do, porting tools and guidelines are available. *[64-Bit Transition Guide for Cocoa](../../Cocoa64BitGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004247)* discusses these matters in detail.

The Cocoa frameworks handle many low-level tasks for you. For example, classes that store and manipulate integer and floating-point values automatically handle the endianness of those values for you.

The following sections survey the features and classes of the three core Cocoa frameworks and briefly describe some of the secondary frameworks. Because each of these core frameworks has dozens of classes, the descriptions of the core frameworks categorize classes by their function. Although these categorizations have a strong logical basis, one can plausibly group classes in other ways. 

### Foundation
The Foundation framework defines a base layer of classes that can be used for any type of Cocoa program. The criterion separating the classes in Foundation from those in the AppKit is the user interface. If an object doesn’t either appear in a user interface or isn’t *exclusively* used to support a user interface, then its class belongs in Foundation. You can create Cocoa programs that use Foundation and no other framework; examples of these are command-line tools and Internet servers. 

The Foundation framework was designed with certain goals in mind:

Define basic object behavior and introduce consistent conventions for such things as memory management, object mutability, and notifications.

Support internationalization and localization with (among other things) bundle technology and Unicode strings.

Support object persistence.

Support object distribution.

Provide some measure of operating-system independence to support portability.

Provide object wrappers or equivalents for programmatic primitives, such as numeric values, strings, and collections. It also provides utility classes for accessing underlying system entities and services, such as ports, threads, and file systems.

Cocoa applications, which by definition link either against the AppKit framework or UIKit framework, invariably must link against the Foundation framework as well. The class hierarchies share the same root class, `NSObject`, and many if not most of the AppKit and UIKit methods and functions have Foundation objects as parameters or return values. Some Foundation classes may seem designed for applications—`NSUndoManager` and `NSUserDefaults`, to name two—but they are included in Foundation because there can be uses for them that do not involve a user interface.

#### Foundation Paradigms and Policies
Foundation introduces several paradigms and policies to Cocoa programming to ensure consistent behavior and expectations among the objects of a program in certain situations.: 

*Object retention and object disposal*. The Objective-C runtime and Foundation give Cocoa programs two ways to ensure that objects persist when they’re needed and are freed when they are no longer needed. Garbage collection, which was introduced in Objective-C 2.0, automatically tracks and disposes of objects that your program no longer needs, thus freeing up memory. Foundation also still offers the traditional approach of memory management. It institutes a policy of object ownership that specifies that objects are responsible for releasing other objects that they have created, copied, or explicitly retained. `NSObject` (class and protocol) defines methods for retaining and releasing objects. Autorelease pools (defined in the `NSAutoreleasePool` class) implement a delayed-release mechanism and enable Cocoa programs to have a consistent convention for returning objects for which the caller is not responsible. For more about garbage collection and explicit memory management, see [Object Retention and Disposal](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW39). 

> **iOS Note:** Garbage collection is not available to applications running in iOS. 

*Mutable class variants*. Many value and container classes in Foundation have a mutable variant of an immutable class, with the mutable class always being a subclass of the immutable one. If you need to dynamically change the encapsulated value or membership of such an object, you create an instance of the mutable class. Because it inherits from the immutable class, you can pass the mutable instance in methods that take the immutable type. For more on object mutability, see [Object Mutability](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW33).

*Class clusters*. A class cluster is an abstract class and a set of private concrete subclasses for which the abstract class acts as an umbrella interface. Depending on the context (particularly the method you use to create an object), an instance of the appropriate optimized class is returned to you. `NSString` and `NSMutableString`, for example, act as brokers for instances of various private subclasses optimized for different kinds of storage needs. Over the years the set of concrete classes has changed several times without breaking applications. For more on class clusters, see [Class Clusters](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW34).

*Notifications*. Notification is a major design pattern in Cocoa. It is based on a broadcast mechanism that allows objects (called *observers*) to be kept informed of what another object is doing or is encountering in the way of user or system events. The object originating the notification can be unaware of the existence or identity of the observers of the notification. There are several types of notifications: synchronous, asynchronous, and distributed. The Foundation notification mechanism is implemented by the `NSNotification`, `NSNotificationCenter`, `NSNotificationQueue`, and `NSDistributedNotificationCenter` classes. For more on notifications, see [Notifications](../CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW7).

#### Foundation Classes
The Foundation class hierarchy is rooted in the `NSObject` class, which (along with the `NSObject` and `NSCopying` protocols) define basic object attributes and behavior. For further information on `NSObject` and basic object behavior, see [The Root Class](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW35).

The remainder of the Foundation framework consists of several related groups of classes as well as a few individual classes. There are classes representing basic data types such as strings and byte arrays, collection classes for storing other objects, classes representing system information such as dates, and classes representing system entities such as ports, threads, and processes. The class hierarchy charts in Figure 1-7 (for printing purposes, in three parts) depict the logical groups these classes form as well as their inheritance relationships. Classes in blue-shaded areas are present in both the OS X and iOS versions of Foundation; classes in gray-shaded areas are present only in the OS X version. 

**Figure 1-7**  The Foundation class hierarchyThese diagrams logically group the classes of the Foundation framework in categories (with other associations pointed out). Of particular importance are the classes whose instances are value objects and collections. 

Value Objects Value objects encapsulate values of various primitive types, including strings, numbers (integers and floating-point values), dates, and even structures and pointers. They mediate access to these values and manipulate them in suitable ways. When you compare two value objects of the same class type, it is their encapsulated values that are compared, not their pointer values. Value objects are frequently the attributes of other objects, including custom objects.

Of course, you may choose to use scalars and other primitive values directly in your program—after all, Objective-C is a superset of ANSI C—and in many cases using scalars is a reasonable thing to do. But in other situations, wrapping these values in objects is either advantageous or required. For example, because value objects are objects, you can, at runtime, find out their class type, determine the messages that can be sent to them, and perform other tasks that can be done with objects. The elements of collection objects such as arrays and dictionaries must be objects. And many if not most methods of the Cocoa frameworks that take and return values such as strings, numbers, and dates require that these values be encapsulated in objects. (With string values, in particular, you should always use `NSString` objects and not C-strings.)

Some classes for value objects have mutable and immutable variants—for example, there are the `NSMutableData` and `NSData` classes. The values encapsulated by immutable objects cannot be modified after they are created, whereas the values encapsulated by mutable objects can be modified. ([Object Mutability](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW33) discusses mutable and immutable objects in detail.)

The following are descriptions of what the more important value objects do:

Instances of the `NSValue` class encapsulate a single ANSI C or Objective-C data item—for example, scalar types such as floating-point values as well as pointers and structures  

The `NSNumber` class (a subclass of `NSValue`) instantiates objects that contain numeric values such as integers, floats, and doubles.

Instances of the `NSData` class provides object-oriented storage for streams of bytes (for example, image data). The class has methods for writing data objects to the file system and reading them back.

The `NSDate` class, along with the supporting `NSTimeZone`, `NSCalendar`, `NSDateComponents`, and `NSLocale` classes, provide objects that represent times, dates, calendar, and locales. They offer methods for calculating date and time differences, for displaying dates and times in many formats, and for adjusting times and dates based on location in the world.

Objects of the `NSString` class (commonly referred to as *strings*) are a type of value object that provides object-oriented storage for a sequence of Unicode characters. Methods of `NSString` can convert between representations of character strings, such as between UTF-8 and a null-terminated array of bytes in a particular encoding. `NSString` also offers methods for searching, combining, and comparing strings and for manipulating file-system paths. Similar to `NSData`, `NSString` includes methods for writing strings to the file system and reading them back.

The `NSString` class also has a few associated classes. You can use an instance of the `NSScanner` utility class to parse numbers and words from an `NSString` object. `NSCharacterSet` represents a set of characters that are used by various `NSString` and `NSScanner` methods. Attributed strings, which are instances of the `NSAttributedString` class, manage ranges of characters that have attributes such as font and kerning associated with them.

Formatter objects—that is, objects derived from `NSFormatter` and its descendent classes—are not themselves value objects, but they perform an important function related to value objects. They convert value objects such as `NSDate` and `NSNumber` instances to and from specific string representations, which are typically presented in the user interface.

Collections Collections are objects that store other objects in a particular ordering scheme for later retrieval. Foundation defines three major collection classes that are common to both iOS and OS X: `NSArray`, `NSDictionary`, and `NSSet`. As with many of the value classes, these collection classes have immutable and mutable variants. For example, once you create an `NSArray` object that holds a certain number of elements, you cannot add new elements or remove existing ones; for that purpose you need the `NSMutableArray` class. (To learn about mutable and immutable objects, see [Object Mutability](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW33).)

Objects of the major collection classes have some common behavioral characteristics and requirements. The items they contain must be objects, but the objects may be of any type. Collection objects, as with value objects, are essential components of property lists and, like all objects, can be archived and distributed. Moreover, collection objects automatically retain—that is, keep a strong reference to—any object they contain. If you remove an object from a mutable collection, it is released, which results in the object being freed if no other object claims it.

> **Note:** "Retain,” “release,” and related terms refer to memory-management of objects in Cocoa. To learn about this important topic, see [Object Retention and Disposal](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW39).

The collection classes provide methods to access specific objects they contain. In addition, there are special enumerator objects (instances of `NSEnumerator`) and language-level support to iterate through collections and access each element in sequence.

The major collection classes are differentiated by the ordering schemes they use:

Arrays (`NSArray`) are ordered collections that use zero-based indexing for accessing the elements of the collection.

Dictionaries (`NSDictionary`) are collections managing pairs of keys and values; the key is an object that identifies the value, which is also an object. Because of this key-value scheme, the elements in a dictionary are unordered. Within a dictionary, the keys must be unique. Although they are typically string objects, keys can be any object that can be copied.

Sets (`NSSet`) are similar to arrays, but they provide unordered storage of their elements instead of ordered storage. In other words, the order of elements in a set is not important. The items in an `NSSet` object must be distinct from each other; however, an instance of the `NSCountedSet` class (a subclass of `NSMutableSet`) may include the same object more than once.  

In OS X, the Foundation framework includes several additional collection classes. `NSMapTable` is a mutable dictionary-like collection class; however, unlike `NSDictionary`, it can hold pointers as well as objects and it maintains weak references to its contained objects rather than strong references. `NSPointerArray` is an array that can hold pointers and `NULL` values and can maintain either strong or weak references to them. The `NSHashTable` class is modeled after `NSSet` but it can store pointers to functions and it provides different options, in particular to support weak relationships in a garbage-collected environment.  

For more on collection classes and objects, see *[Collections Programming Topics](../../Collections/Collections.html#//apple_ref/doc/uid/10000034i)*.

Other Categories of Foundation ClassesThe remaining classes of the Foundation framework fall into various categories, as indicated by the diagram in [Figure 1-7](#//apple_ref/doc/uid/TP40002974-CH3-SW15). The major categories of classes, shown in Figure 1-7, are described here:

*Operating-system services*. Many Foundation classes facilitate access of various lower-level services of the operating system and, at the same time, insulate you from operating-system idiosyncrasies. For example, `NSProcessInfo` lets you query the environment in which an application runs and `NSHost` yields the names and addresses of host systems on a network. You can use an `NSTimer` object to send a message to another object at specific intervals, and `NSRunLoop` lets you manage the input sources of an application or other type of program. `NSUserDefaults` provides a programmatic interface to a system database of global (per-host) and per-user default values (preferences). 

*File system and URL*. `NSFileManager` provides a consistent interface for file operations such as creating, renaming, deleting, and moving files. `NSFileHandle` permits file operations at a lower level (for example, seeking within a file). `NSBundle` finds resources stored in bundles and can dynamically load some of them (for example, nib files and code). You use `NSURL` and related `NSURL...` classes to represent, access, and manage URL sources of data.

*Concurrency*. `NSThread` lets you create multithreaded programs, and various lock classes offer mechanisms for controlling access to process resources by competing threads. You can use `NSOperation` and `NSOperationQueue` to perform multiple operations (concurrent or nonconcurrent) in priority and dependence order. With `NSTask`, your program can fork off a child process to perform work and monitor its progress. 

*Interprocess communication*. Most of the classes in this category represent various kinds of system ports, sockets, and name servers and are useful in implementing low-level IPC. `NSPipe` represents a BSD pipe, a unidirectional communications channel between processes. 

> **iOS Note:** The name server classes are not in the iOS version of Foundation.

*Networking*. The `NSNetService` and `NSNetServiceBrowser` classes support the zero-configuration networking architecture called *Bonjour*. Bonjour is a powerful system for publishing and browsing for services on an IP network.

*Notifications*. See the summary of the notification classes in [Foundation Paradigms and Policies](#//apple_ref/doc/uid/TP40002974-CH3-SW7). 

*Archiving and serialization*. The classes in this category make object distribution and persistence possible. `NSCoder` and its subclasses, along with the `NSCoding` protocol, represent the data an object contains in an architecture-independent way by allowing class information to be stored along with the data. `NSKeyedArchiver` and `NSKeyedUnarchiver` offer methods for encoding objects and scalar values and decoding them in a way that is not dependent on the ordering of encoding messages. 

*Objective-C language services*. `NSException` and `NSAssertionHandler` provide an object-oriented way of making assertions and handling exceptions in code. An `NSInvocation` object is a static representation of an Objective-C message that your program can store and later use to invoke a message in another object; it is used by the undo manager (`NSUndoManager`) and by the distributed objects system. An `NSMethodSignature` object records the type information of a method and is used in message forwarding. `NSClassDescription` is an abstract class for defining and querying the relationships and properties of a class. 

**XML processing**. Foundation on both platforms has the `NSXMLParser` class, which is an object-oriented implementation of a streaming parser that enables you to process XML data in an event-driven way. 

Foundation in OS X includes the NSXML classes (so called because the class names begin with “NSXML”). Objects of these classes represent an XML document as a hierarchical tree structure. This approach lets you to query this structure and manipulate its nodes. The NSXML classes support several XML-related technologies and standards, such as XQuery, XPath, XInclude, XSLT, DTD, and XHTML.

*Predicates and expressions*. The predicate classes—`NSPredicate`, `NSCompoundPredicate`, and `NSComparisonPredicate`—encapsulate the logical conditions to constrain a fetch or filter object. `NSExpression` objects represent expressions in a predicate.

The Foundation framework for iOS has a subset of the classes for OS X. The following categories of classes are present only in the OS X version of Foundation:

*Spotlight queries*. The `NSMetadataItem`, `NSMetadataQuery` and related query classes encapsulate file-system metadata and make it possible to query that metadata.

*Scripting*. The classes in this category help to make your program responsive to AppleScript scripts and Apple event commands. 

*Distributed objects*. You use the distributed object classes for communication between processes on the same computer or on different computers on a network. Two of these classes, `NSDistantObject` and `NSProtocolChecker`, have a root class (`NSProxy`) different from the root class of the rest of Cocoa.

### AppKit (OS X)
AppKit is a framework containing all the objects you need to implement your graphical, event-driven user interface in OS X: windows, dialogs, buttons, menus, scrollers, text fields—the list goes on. AppKit handles all the details for you as it efficiently draws on the screen, communicates with hardware devices and screen buffers, clears areas of the screen before drawing, and clips views. The number of classes in AppKit may seem daunting at first. However, most AppKit classes are support classes that you use indirectly. You also have the choice at which level you use AppKit:

Use Interface Builder to create connections from user-interface objects to your application’s controller objects, which manage the user interface and coordinate the flow of data between the user interface and internal data structures. For this, you might use off-the-shelf controller objects (for Cocoa bindings) or you may need to  implement one or more custom controller classes—particularly the action and delegate methods of those classes. For example, you would need to implement a method that is invoked when the user chooses a menu item (unless it has a default implementation that is acceptable).

Control the user interface programmatically, which requires more familiarity with AppKit classes and protocols. For example, allowing the user to drag an icon from one window to another requires some programming and familiarity with the `NSDragging...` protocols.

Implement your own objects by subclassing `NSView` or other classes. When subclassing `NSView`, you write your own drawing methods using graphics functions. Subclassing requires a deeper understanding of how AppKit works.

#### Overview of the AppKit Framework
The AppKit framework consists of more than 125 classes and protocols. All classes ultimately inherit from the Foundation framework’s `NSObject` class. The diagrams in Figure 1-8 show the inheritance relationships of the AppKit classes. 

**Figure 1-8**  AppKit class hierarchy—Objective-CAs you can see, the hierarchy tree of AppKit is broad but fairly shallow; the classes deepest in the hierarchy are a mere five superclasses away from the root class and most classes are much closer than that. Some of the major branches in this hierarchy tree are particularly interesting. 

At the root of the largest branch in AppKit is the `NSResponder` class. This class defines the responder chain, an ordered list of objects that respond to user events. When the user clicks the mouse button or presses a key, an event is generated and passed up the responder chain in search of an object that can respond to it. Any object that handles events must inherit from the `NSResponder` class. The core AppKit classes—`NSApplication`, `NSWindow`, and `NSView`—inherit from `NSResponder`. 

The second largest branch of classes in AppKit descend from `NSCell`. The noteworthy thing about this group of classes is that they roughly mirror the classes that inherit from `NSControl`, which inherits from `NSView`. For its user-interface objects that respond to user actions, AppKit uses an architecture that divides the labor between control objects and cell objects. The `NSControl` and `NSCell` classes, and their subclasses, define a common set of user-interface objects such as buttons, sliders, and browsers that the user can manipulate graphically to control some aspect of your application. Most control objects are associated with one or more cell objects that implement the details of drawing and handling events. For example, a button comprises both an `NSButton` object and an `NSButtonCell` object. 

Controls and cells implement a mechanism that is based on an important design pattern of the AppKit: the target-action mechanism. A cell can hold information that identifies the message that should be sent to a particular object when the user clicks (or otherwise acts upon) the cell. When a user manipulates a control (by, for example, clicking it), the control extracts the required information from its cell and sends an action message to the target object. Target-action allows you to give meaning to a user action by specifying what the target object and invoked method should be. You typically use Interface Builder to set these targets and actions by Control-dragging from the control object to your application or other object. You can also set targets and actions programmatically. 

Another important design pattern–based mechanism of AppKit (and also UIKit) is delegation. Many objects in a user interface, such as text fields and table views, define a delegate. A delegate is an object that acts on behalf of, or in coordination with, the delegating object. It is thus able to impart application-specific logic to the operation of the user interface. For more on delegation, target–action, and other paradigms and mechanisms of AppKit, see [Communicating with Objects](../CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW15). For a discussion of the design patterns on which these paradigms and mechanisms are based, see [Cocoa Design Patterns](../CocoaDesignPatterns/CocoaDesignPatterns.html#//apple_ref/doc/uid/TP40002974-CH6-SW6).

One of the general features of OS X v10.5  and later system versions is resolution independence: The resolution of the screen is decoupled from the drawing done by code. The system automatically scales content for rendering on the screen. The AppKit classes support resolution independence in their user-interface objects. However, for your own applications to take advantage of resolution independence, you might have to supply images at a higher resolution or make minor adjustments in your drawing code that take the current scaling factor into account. 

The following sections briefly describe some of the capabilities and architectural aspects of the AppKit framework and its classes and protocols. It groups classes according to the class hierarchy diagrams shown in [Figure 1-8](#//apple_ref/doc/uid/TP40002974-CH3-SW1).

#### General User-Interface Classes
For the overall functioning of a user interface, the AppKit provides the following classes:

*The global application object*. Every application uses a singleton instance of `NSApplication` to control the main event loop, keep track of the application’s windows and menus, distribute events to the appropriate objects (that is, itself or one of its windows), set up top-level autorelease pools, and receive notification of application-level events. An `NSApplication` object has a delegate (an object that you assign) that is notified when the application starts or terminates, is hidden or activated, should open a file selected by the user, and so forth. By setting the `NSApplication` object’s delegate and implementing the delegate methods, you customize the behavior of your application without having to subclass `NSApplication`.

*Windows and views*. The window and view classes, `NSWindow` and `NSView`, also inherit from `NSResponder`, and so are designed to respond to user actions. An `NSApplication` object maintains a list of `NSWindow` objects—one for each window belonging to the application—and each `NSWindow` object maintains a hierarchy of `NSView` objects. The view hierarchy is used for drawing and handling events within a window. An `NSWindow` object handles window-level events, distributes other events to its views, and provides a drawing area for its views. An `NSWindow` object also has a delegate allowing you to customize its behavior. 

Beginning with OS X v10.5, the window and view classes of the AppKit support enhanced animation features. 

`NSView` is the superclass for all objects displayed in a window. All subclasses implement a drawing method using graphics functions; `drawRect:` is the primary method you override when creating a new `NSView`.

*Controller classes for Cocoa bindings*. The abstract `NSController` class and its concrete subclasses `NSObjectController`, `NSArrayController`, `NSDictionaryController`, and `NSTreeController` are part of the implementation of Cocoa bindings. This technology automatically synchronizes the application data stored in objects and the presentation of that data in a user interface. See [The Model-View-Controller Design Pattern](../CocoaDesignPatterns/CocoaDesignPatterns.html#//apple_ref/doc/uid/TP40002974-CH6-SW1) for a description of these types of controller objects.

*Panels (dialogs)*. The `NSPanel` class is a subclass of `NSWindow` that you use to display transient, global, or pressing information. For example, you would use an instance of `NSPanel`, rather than an instance of `NSWindow`, to display error messages or to query the user for a response to remarkable or unusual circumstances. The AppKit implements some common dialogs for you such as the Save, Open, and Print dialogs, used to save, open, and print documents. Using these dialogs gives the user a consistent look and feel across applications for common operations.

*Menus and cursors*. The `NSMenu`, `NSMenuItem`, and `NSCursor` classes define the look and behavior of the menus and cursors that your application displays to the user.

*Grouping and scrolling views*. The `NSBox`, `NSScrollView`, and `NSSplitView` classes provide graphic “accessories” to other view objects or collections of views in windows. With the `NSBox` class, you can group elements in windows and draw a border around the entire group. The `NSSplitView` class lets you append views vertically or horizontally, apportioning to each view some amount of a common territory; a sliding control bar lets the user redistribute the territory among views. The `NSScrollView` class and its helper class, `NSClipView`, provide a scrolling mechanism as well as the graphic objects that let the user initiate and control a scroll. The `NSRulerView` class allows you to add a ruler and markers to a scroll view.

*Table views and outline views*. The `NSTableView` class displays data in rows and columns. `NSTableView` is ideal for, but not limited to, displaying database records, where rows correspond to each record and columns contain record attributes. The user can edit individual cells and rearrange the columns. You control the behavior and content of an `NSTableView` object by setting its delegate and data source objects. Outline views (instances of `NSOutlineView`, a subclass of `NSTableView`) offer another approach to displaying tabular data. With the `NSBrowser` class you can create an object with which users can display and navigate hierarchical data.

#### Text and Fonts
The Cocoa text system is based on the Core Text framework, which was introduced in OS X v10.5. The Core Text framework provides a modern, low-level, high-performance technology for laying out text. If you use the Cocoa text system, you should rarely have reason to use Core Text directly.

The `NSTextField` class implements a simple editable text-input field, and the `NSTextView` class provides more comprehensive editing features for larger text bodies. 

`NSTextView`, a subclass of the abstract `NSText` class, defines the interface to the extended text system. `NSTextView` supports rich text, attachments (graphics, file, and other), input management and key binding, and marked text attributes. `NSTextView` works with the Fonts window and Font menu, rulers and paragraph styles, the Services facility, and the pasteboard (Clipboard). `NSTextView` also allows customizing through delegation and notifications—you rarely need to subclass `NSTextView`. You rarely create instances of `NSTextView` programmatically either, because objects in the Interface Builder library, such as `NSTextField`, `NSForm`, and `NSScrollView`, already contain `NSTextView` objects. 

It is also possible to do more powerful and more creative text manipulation (such as displaying text in a circle) using `NSTextStorage`, `NSLayoutManager`, `NSTextContainer`, and related classes. The Cocoa text system also supports lists, tables, and noncontiguous selections.

The `NSFont` and `NSFontManager` classes encapsulate and manage font families, sizes, and variations. The `NSFont` class defines a single object for each distinct font; for efficiency, these objects, which can represent a lot of data, are shared by all the objects in your application. The `NSFontPanel` class defines the Fonts window that’s presented to the user.

#### Graphics and Colors
The classes `NSImage` and `NSImageRep` encapsulate graphics data, allowing you to easily and efficiently access images stored in files on the disk and displayed on the screen. `NSImageRep` subclasses each know how to draw an image from a particular kind of source data. The `NSImage` class provides multiple representations of the same image, and it also provides behaviors such as caching. The imaging and drawing capabilities of Cocoa are integrated with the Core Image framework.

Color is supported by the classes `NSColor`, `NSColorSpace`, `NSColorPanel`, `NSColorList`, `NSColorPicker`, and `NSColorWell`. `NSColor` and `NSColorSpace` support a rich set of color formats and representations, including custom ones. The other classes are mostly interface classes: They define and present panels and views that allow the user to select and apply colors. For example, the user can drag colors from the Colors window to any color well. 

The `NSGraphicsContext`, `NSBezierPath`, and `NSAffineTransform` classes help you with vector drawing and support graphical transformations such as scaling, rotation, and translation.

#### Printing and Faxing
The `NSPrinter`, `NSPrintPanel`, `NSPageLayout`, and `NSPrintInfo` classes work together to provide the means for printing and faxing the information that your application displays in its windows and views. You can also create a PDF representation of an `NSView` object. 

#### Document and File-System Support
You can use the `NSFileWrapper` class to create objects that correspond to files or directories on disk. `NSFileWrapper` holds the contents of the file in memory so that it can be displayed, changed, or transmitted to another application. It also provides an icon for dragging the file or representing it as an attachment. You can use the `NSFileManager` class in the Foundation framework to access and enumerate file and directory contents. The `NSOpenPanel` and `NSSavePanel` classes also provide a convenient and familiar user interface to the file system.

The `NSDocumentController`, `NSDocument`, and `NSWindowController` classes define an architecture for creating document-based applications. (The `NSWindowController` class is shown in the User Interface group of classes in the class hierarchy charts.) Such applications can generate identical window containers that hold uniquely composed sets of data that can be stored in files. They have built-in or easily acquired capabilities for saving, opening, reverting, closing, and managing these documents. 

#### Internationalization and Character Input Support
If an application is to be used in more than one part of the world, its resources may need to be customized, or localized, for language, country, or cultural region. For example, an application may need to have separate Japanese, English, French, and German versions of character strings, icons, nib files, or context help. Resource files specific to a particular language are grouped together in a subdirectory of the bundle directory (the directories with the `.lproj` extension). Usually you set up localization resource files using Interface Builder. See *[Internationalization and Localization Guide](../../../../MacOSX/Conceptual/BPInternational/Introduction/Introduction.html#//apple_ref/doc/uid/10000171i)* for more information on the Cocoa internationalization facilities.

The `NSInputServer` and `NSInputManager` classes, along with the `NSTextInput` protocol, give your application access to the text input management system. This system interprets keystrokes generated by various international keyboards and delivers the appropriate text characters or Control-key events to text view objects. (Typically the text classes deal with these classes and you won’t have to.)

#### Operating-System Services
The AppKit provides operating-system support for your application through classes that implement the following features:

*Sharing data with other applications*. The `NSPasteboard` class defines the pasteboard, a repository for data that’s copied from your application, making this data available to any application that cares to use it. `NSPasteboard` implements the familiar cut/copy-and-paste operation. 

*Dragging*. With very little programming on your part, custom view objects can be dragged and dropped anywhere. Objects become part of this dragging mechanism by conforming to `NSDragging...` protocols; draggable objects conform to the `NSDraggingSource` protocol, and destination objects (receivers of a drop) conform to the `NSDraggingDestination` protocol. The AppKit hides all the details of tracking the cursor and displaying the dragged image.

*Spell Checking*. The `NSSpellServer` class lets you define a spell-checking service and provide it as a service to other applications. To connect your application to a spell-checking service, you use the `NSSpellChecker` class. The `NSIgnoreMisspelledWords` and `NSChangeSpelling` protocols support the spell-checking mechanism.

#### Interface Builder Support
The abstract `NSNibConnector` class and its two concrete subclasses, `NSNibControlConnector` and `NSNibOutletConnector`, represent connections in Interface Builder. `NSNibControlConnector` manages an action connection in Interface Builder and `NSNibOutletConnector` manages an outlet connection.

### UIKit (iOS)
The UIKit framework in iOS is the sister framework of the AppKit framework in OS X. Its purpose is essentially the same: to provide all the classes that an application needs to construct and manage its user interface. However, there are significant differences in how the frameworks realize this purpose.

One of the greatest differences is that, in iOS, the objects that appear in the user interface of a Cocoa application look and behave in a way that is different from the way their counterparts in a Cocoa application running in OS X look and behave. Some common examples are text views, table views, and buttons.  In addition, the event-handling and drawing models for Cocoa applications on the two platforms are significantly different. The following sections explain the reasons for these and other differences.

You can add UIKit objects to your application’s user interface in three ways: 

Use the Interface Builder development application to drag windows, views, and other objects from an object library. 

Create, position, and configure framework objects programmatically.  

Implement custom user-interface objects by subclassing `UIView` or classes that inherit from `UIView`.

#### Overview of UIKit Classes
As with AppKit, the classes of the UIKit framework ultimately inherit from `[NSObject](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/cl/NSObject)`. Figure 1-9 presents the classes of the UIKit framework in their inheritance relationship. 

**Figure 1-9**  UIKit class hierarchyAs with AppKit, a base responder class is at the root of the largest branch of UIKit classes. `UIResponder` also defines the interface and default behavior (if any) for event-handling methods and for the responder chain, which is a chain of objects that are potential event handlers. When the user scrolls a table view with his or her finger or types characters in a virtual keyboard, UIKit generates an event and that event is passed up the responder chain until an object handles it. The corresponding core objects—application (`UIApplication`), window (`UIWindow`), and view (`UIView`)—all directly or indirectly inherit from `UIResponder`.

Unlike the AppKit, UIKit does not make use of cells. Controls in UIKit—that is, all objects that inherit from `UIControl`—do not require cells to carry out their primary role: sending action messages to a target object. Yet the way UIKit implements the target-action mechanism is different from the way it is implemented in AppKit. The `UIControl` class defines a set of event types for controls; if, for example, you want a button (`UIButton`) to send an action message to a target object, you call `UIControl` methods to associate the action and target with one or more of the control event types. When one of those events happens, the control sends the action message. 

The UIKit framework makes considerable use of delegation, another design pattern of AppKit. Yet the UIKit implementation of delegation is different. Instead of using informal protocols, UIKit uses formal protocols with possibly some protocol methods marked optional. 

> **Note:** For a complete description of the target-action mechanism in UIKit and the AppKit, see [The Target-Action Mechanism](../CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW14). To learn more about delegation and protocols, both formal and informal, see [Delegates and Data Sources](../CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW18) and [Protocols](../CocoaObjects/CocoaObjects.html#//apple_ref/doc/uid/TP40002974-CH4-SW41). 

#### Application Coordination
Each application running in iOS is managed by a singleton application object, and this object has a job that is almost identical to that for the global `NSApplication` object. A `UIApplication` object controls the main event loop, keeps track of the application’s windows and views, and dispatches incoming events to the appropriate responder objects. 

The `UIApplication` object also receives notification of system-level and application-level events. Many of these it passes to its delegate, allowing it to inject application-specific behavior when the application launches and terminates, to respond to low-memory warnings and changes in time, and to handle other tasks.  

#### Differences in Event and Drawing Models
In OS X, the mouse and keyboard generate most user events. AppKit uses `NSEvent` objects to encapsulate these events. In iOS, however, a user’s finger movements on the screen are what originate events. UIKit also has a class, `UIEvent`, to represent these events. But finger touches are different in nature from mouse clicks; two or more touches occurring over a period could form a discrete event—a pinch gesture, for example. Thus a `UIEvent` object contains one or more objects representing finger touches (`UITouch`). The model for distributing and dispatching events to objects that can handle them on the two platforms is almost identical. However, to handle an event, an object must take into consideration the sequence of touches specific to that event.

UIKit also has gesture recognizers, objects that automate the detection of touches as gestures. A gesture recognizer analyzes a series of touches and, when it recognizes its gesture, sends an action message to a target. UIKit provides ready-made gesture recognizers for gestures such as tap, swipe, rotate, and pan. You can also create custom gesture recognizers that detect application-specific gestures.

The drawing models are similar for AppKit and UIKit. Animation is integrated into the UIKit drawing implementation. The programmatic support that UIKit directly provides for drawing is a bit more limited compared to AppKit. The framework offers classes and functions for Bezier paths, PDF generation, and simple line and rectangle drawing (through functions declared in `UIGraphics.h`). You use `UIColor` objects to set colors in the current graphics context and `UIImage` objects to represent and encapsulate images. For drawing of greater sophistication, an application must use the Core Graphics or OpenGL ES framework.

#### General User-Interface Classes
Objects on an iOS user interface are visibly different than objects on an OS X user interface. Because of the nature of the device—specifically the smaller screen size and the use of fingers instead of the mouse and keyboard for input—user-interface objects in iOS must typically be larger (to be an adequate target for touches) while at the same time make as efficient use of screen real estate as possible. These objects are sometimes based on visual and tactile analogs of an entirely different sort. As an example, consider the date picker object, which is instantiated from a class that both the UIKit and AppKit frameworks define. In OS X, the date picker looks like this:

This style of date picker has a two tiny areas for incrementing date components, and thus is suited to manipulation by a mouse pointer. Contrast this with the date picker seen in iOS applications:

This style of date picker is more suited for finger touches as an input source; users can swipe a month, day, or year column to spin it to a new value.

As with AppKit classes, many of UIKit classes fall into functional groups:

**Controls**. The subclasses of `UIControl` instantiate objects that let users communicate their intent to an application. In addition to the standard button object (`UIButton`) and slider object (`UISlider`), there is a control that simulates off/on switches (`UISwitch`), a spinning-wheel control for selecting from multidimensional sets of values (`UIPickerView`), a control for paging through documents (`UIPageControl`), and other controls.

**Modal views**. The two classes inheriting from `UIModalView` are for displaying messages to users either in the form of  “sheets” attached to specific views or windows (`UIActionSheet`) or as unattached alert dialogs (`UIAlertView`). On iPad, an application can uses popover views (`UIPopoverController`) instead of action sheets.

**Scroll views**. The `UIScrollView` class enables instances of its subclasses to respond to touches for scrolling within large views. As users scroll, the scroll view displays transient indicators of position within the document. The subclasses of `UIScrollView` implement table views, text views, and web views.

**Toolbars, navigation bars, split views, and view controllers**. The `UIViewController` class is a base class for managing a view. A view controller provides methods for creating and observing views, overlaying views, handling view rotation, and responding to low-memory warnings. UIKit includes concrete subclasses of `UIViewController` for managing toolbars, navigation bars, and image pickers. 

Applications use both toolbars and navigation bars to manage behavior related to the “main” view on the screen; typically, toolbars are placed beneath the main view and navigation bars above it. You use toolbar (`UIToolbar`) objects to switch between modes or views of an application; you can also use them to display a set of functions that perform some action related to the current main view. You use navigation bars (`UINavigationBar`) to manage sequences of windows or views in an application and, in effect, to “drill down” a hierarchy of objects defined by the application; the Mail application, for example, uses a navigation bar to navigate from accounts to mailbox folders and from there to individual email messages. On iPad, applications can use a `UISplitViewController` object to present a master-detail interface. 

#### Text 
Users can enter text in an iOS application either through a text view (`UITextView`) or a text field (`UITextField`). These classes adopt the `UITextInputTraits` protocol to specify the appearance and behavior of the virtual keyboard that is presented when users touch the text-entry object; any subclasses that enable entry of text should also conform to this protocol. Applications can draw text in views using `UIStringDrawing` methods, a category on the `NSString` class. And with the `UIFont` class you can specify the font characteristics of text in all objects that display text, including table cells, navigation bars, and labels.

Applications that do their own text layout and font management can adopt the `UITextInput` protocol and use related classes and protocols to communicate with the text input system of iOS.

### Comparing AppKit and UIKit Classes
AppKit and UIKit are Cocoa application frameworks that are designed for different platforms, one for OS X and the other for iOS. Because of this affinity, it is not surprising that many of the classes in each framework have similar names; in most cases, the prefix (“NS” versus “UI”) is the only name difference. These similarly named classes fulfill mostly similar roles, but there are differences. These differences can be a matter of scope, of inheritance, or of design. Generally, UIKit classes have fewer methods than their AppKit counterparts.

Table 1-1 describes the differences between the major classes in each framework.

**Table 1-1**  Major classes of the AppKit and UIKit frameworksClasses

Comparison

`NSApplication`

`UIApplication`

The classes are strikingly similar in their primary roles. They provide a singleton object that sets up the application’s display environment and event loop, distributes events, and notifies a delegate when application-specific events occur (such as launch and termination). However, the `NSApplication` class performs functions (for example, managing application suspension, reactivation, and hiding) that are not available to an iOS application.

`NSResponder`

`UIResponder`

These classes also have nearly identical roles. They are abstract classes that define an interface for responding to events and managing the responder chain. The main difference is that the `NSResponder` event-handling methods are defined for the mouse and keyboard, whereas the `UIResponder` methods are defined for the Multi-Touch event model.

`NSWindow`

`UIWindow`

The `UIWindow` class occupies a place in the class hierarchy different from the place `NSWindow` occupies in AppKit; it is a subclass of `UIView`, whereas the AppKit class inherits directly from `NSResponder`. `UIWindow` has a much more restricted role in an application than does `NSWindow`. It also provides an area for displaying views, dispatches events to those views, and converts between window and view coordinates.

`NSView`

`UIView`

These classes are very similar in purpose and in their basic sets of methods. They allow you to move and resize views, manage the view hierarchy, draw view content, and convert view coordinates. The design of `UIView`, however, makes view objects inherently capable of animation. 

`NSControl`

`UIControl`

Both classes define a mechanism for objects such as buttons and sliders so that, when manipulated, the control object sends an action message to a target object. The classes implement the target-action mechanism in different ways, largely because of the difference between event models. See [The Target-Action Mechanism](../CommunicatingWithObjects/CommunicateWithObjects.html#//apple_ref/doc/uid/TP40002974-CH7-SW14) for information.

`NSViewController`

`UIViewController`

The role of both of these classes is, as their names suggest, to manage views. How they accomplish this task is different. The management provided by an `NSViewController` object is dependent on bindings, which is a technology supported only in OS X. `UIViewController` objects are used in the iOS application model for modal and navigation user interfaces (for example, the views controlled by navigation bars).

`NSTableView`

`UITableView`

`NSTableView` inherits from `NSControl`, but `UITableView` does not inherit from `UIControl`. More importantly, `NSTableView` objects support multiple columns of data; `UITableView` objects display only a single column of data at a time, and thus function more as lists than presentations of tabular data.  

Among the minor classes you can find some differences too. For example, UIKit has the `UITextField` and `UILabel` classes, the former for editable text fields and the latter for noneditable text fields used as labels; with the `NSTextField` class you can create both kinds of text fields simply by setting text-field attributes. Similarly, the `NSProgressIndicator` class can create objects in styles that correspond to instances of the `UIProgressIndicator` and `UIProgressBar` classes.

### Core Data
Core Data is a Cocoa framework that provides an infrastructure for managing object graphs, including support for persistent storage in a variety of file formats. Object-graph management includes features such as undo and redo, validation, and ensuring the integrity of object relationships. Object persistence means that Core Data saves model objects to a persistent store and fetches them when required. The persistent store of a Core Data application—that is, the ultimate form in which object data is archived—can range from XML files to SQL databases. Core Data is ideally suited for applications that act as front ends for relational databases, but any Cocoa application can take advantage of its capabilities.

The central concept of Core Data is the managed object. A managed object is simply a model object that is managed by Core Data, but it must be an instance of the `[NSManagedObject](https://developer.apple.com/documentation/coredata/nsmanagedobject)` class or a subclass of that class. You describe the managed objects of your Core Data application using a schema called a managed object model.  (The Xcode application includes a data modeling tool to assist you in creating these schemas.) A managed object model contains descriptions of an application’s managed objects (also referred to as *entities*). Each description specifies the attributes of an entity, its relationships with other entities, and metadata such as the names of the entity and the representing class. 

In a running Core Data application, an object known as a *managed object context* is responsible for a graph of managed objects. All managed objects in the graph must be registered with a managed object context. The context allows an application to add objects to the graph and remove them from it. It also tracks changes made to those objects, and thus can provide undo and redo support. When you’re ready to save changes made to managed objects, the managed object context ensures that those objects are in a valid state. When a Core Data application wants to retrieve data from its external data store, it sends a fetch request—an object that specifies a set of criteria—to a managed object context. The managed object context returns the objects from the store that match the request after automatically registering them.

A managed object context also functions as a gateway to an underlying collection of Core Data objects called the *persistence stack*. The persistence stack mediates between the objects in your application and external data stores.  The stack consists of two different types of objects, persistent stores and persistent store coordinators. Persistent stores are at the bottom of the stack. They map between data in an external store—for example, an XML file—and corresponding objects in a managed object context. They don't interact directly with managed object contexts, however. Above a persistence store in the stack is a persistent store coordinator, which presents a facade to one or more managed object contexts so that multiple persistence stores below it appear as a single aggregate store. Figure 1-10 shows the relationships between objects in the Core Data architecture. 

**Figure 1-10**  Examples of managed object contexts and the persistence stackCore Data includes the `[NSPersistentDocument](https://developer.apple.com/documentation/appkit/nspersistentdocument)` class, a subclass of `NSDocument` that helps to integrate Core Data and the document architecture. A persistent-document object creates its own persistence stack and managed object context, mapping the document to an external data store. An `NSPersistentDocument` object provides default implementations of the `NSDocument` methods for reading and writing document data.

### Other Frameworks with a Cocoa API
As part of a standard installation, Apple includes, in addition to the core frameworks for both platforms, several frameworks that vend Cocoa programmatic interfaces. You can use these secondary frameworks to give your application capabilities that are desirable, if not essential. Some notable secondary frameworks include: 

Sync Services—(OS X only) Using Sync Services you can sync existing contacts, calendars and bookmarks schemas as well as your own application data. You can also extend existing schemas. See *[Sync Services Programming Guide](../../SyncServices/SyncServices.html#//apple_ref/doc/uid/TP40001178)* for more information.  

Address Book—This framework implements a centralized database for contact and other personal information. Applications that use the Address Book framework can share this contact information with other applications, including Apple’s Mail and iChat. See *[Address Book Programming Guide for Mac](../../../../UserExperience/Conceptual/AddressBook/AddressBook.html#//apple_ref/doc/uid/10000117i)* for more information. 

> **iOS Note:** There are different versions of this framework in OS X and iOS. In addition, the Address Book framework in iOS has only ANSI C (procedural) programmatic interfaces.

Preference Panes—(OS X only) With this framework you can create plug-ins that your application dynamically loads to obtain a user interface for recording user preferences, either for the application itself or systemwide. See *[Preference Pane Programming Guide](../../../../UserExperience/Conceptual/PreferencePanes/PreferencePanes.html#//apple_ref/doc/uid/10000110i)* for more information.

Screen Saver—(OS X only) The Screen Saver framework helps you create Screen Effects modules, which can be loaded and run via System Preferences. See *[Screen Saver Framework Reference](https://developer.apple.com/documentation/screensaver)* for more information. 

WebKit—(not public in iOS) The WebKit framework provides a set of core classes to display web content in windows, and by default, implements features such as following links clicked by the user. See *[WebKit Objective-C Programming Guide](../../DisplayWebContent/DisplayWebContent.html#//apple_ref/doc/uid/10000164i)* for more information.

iAd—(iOS only) This framework allows your application to earn revenue by displaying advertisements to the user. 

Map Kit—(iOS only) This framework lets an application embed maps into its own windows and views. It also supports map annotation, overlays, and reverse-geocoding lookups.

Event Kit—(iOS only) With the Event Kit framework, an application can access event information from a user’s Calendar database and enable users to create and edit events for their calendars. The framework also supports efficient fetching of event records, notifications of event changes, and automatic syncing with appropriate calendar databases. 

Core Motion—(iOS only) The Core Motion processes low-level data received from a device’s accelerometer and gyroscope (if available) and presents that to applications for handling.

Core Location—(iOS only) Core Location lets an application determine the current location or heading associated with a device. With it an application can also define geographic regions and monitor when the user crosses the boundaries of those regions.

Media Player—(iOS only)  This framework enables an application to play movies, music, audio podcasts, and audio book files. It also gives your application access to the iPod library.

## A Bit of History
Many years ago Cocoa was known as NeXTSTEP. NeXT Computer developed and released version 1.0 of NeXTSTEP in September of 1989, and versions 2.0 and 3.0 followed not far behind (in 1990 and 1992, respectively). In this early phase, NeXTSTEP was more than an application environment; the term referred to the entire operating system, including the windowing and imaging system (which was based on Display PostScript), the Mach kernel, device drivers, and so on. 

Back then, there was no Foundation framework. Indeed, there were no frameworks; instead, the software libraries (dynamically shared) were known as *kits*, the most prominent of them being Application Kit. Much of the role that Foundation now occupies was taken by an assortment of functions, structures, constants, and other types. Application Kit itself had a much smaller set of classes than it does today. Figure 1-11 shows a class hierarchy chart of NeXTSTEP 0.9 (1988).

**Figure 1-11**  Application Kit class hierarchy in 1988In addition to Application Kit, the early NeXTSTEP included the Sound Kit and the Music Kit, libraries containing a rich set of Objective-C classes that provided high-level access to the Display Postscript layer for audio and music synthesis. 

In early 1993, NeXTSTEP 3.1 was ported to (and shipped on) Intel, Sparc, and Hewlett-Packard computers. NeXTSTEP 3.3 also marked a major new direction, for it included a preliminary version of Foundation. Around this time (1993), the OpenStep initiative also took form. OpenStep was a collaboration between Sun and NeXT to port the higher levels of NeXTSTEP (particularly Application Kit and Display PostScript) to Solaris. The “Open” in the name referred to the open API specification that the companies would publish jointly. The official OpenStep API, published in September of 1994, was the first to split the API between Foundation and Application Kit and the first to use the “NS” prefix. Eventually, Application Kit became known as, simply, AppKit.

By June 1996, NeXT had ported and shipped versions of OpenStep 4.0 that could run Intel, Sparc, and Hewlett-Packard computers as well as an OpenStep runtime that could run on Windows systems. Sun also finished their port of OpenStep to Solaris and shipped it as part of their Network Object Computing Environment. OpenStep, however, never became a significant part of Sun’s overall strategy.

When Apple acquired NeXT Software (as it was then called) in 1997, OpenStep became the Yellow Box and was included with OS X Server (also known as Rhapsody) and Windows. Then, with the evolution of the OS X strategy, it was finally renamed to “Cocoa.”

        
            [Next](../CocoaObjects/CocoaObjects.html)[Previous](../Introduction/Introduction.html)
        
         Copyright &#x00a9; 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-09-18