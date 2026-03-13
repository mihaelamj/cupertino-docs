---
title: "About Bundles"
book: "Bundle Programming Guide"
chapterId: "10000123i-CH100"
date: "2017-03-27"
description: "Explains how to use bundle objects to organize resources."
identifier: "//apple_ref/doc/uid/10000123i"
source: apple-archive
---

# About Bundles

> **Bundle Programming Guide**
> Last updated: 2017-03-27

[Next](../BundleTypes/BundleTypes.html)[Previous](../Introduction/Introduction.html)
        
        
        [](../index.html)
        
        # About Bundles
Bundles are a convenient way to deliver software in macOS and iOS. Bundles provide a simplified interface for end users and at the same time provide support for development. This chapter provides an introduction to bundles and discusses the role they play in macOS and iOS.

## Bundles and Packages
Although bundles and packages are sometimes referred to interchangeably, they actually represent very distinct concepts:

A *package* is any directory that the Finder presents to the user as if it were a single file.

A *bundle* is a directory with a standardized hierarchical structure that holds executable code and the resources used by that code.

Packages provide one of the fundamental abstractions that makes macOS easy to use. If you look at an application or plug-in on your computer, what you are actually looking at is a directory. Inside the package directory are the code and resource files needed to make the application or plug-in run. When you interact with the package directory, however, the Finder treats it like a single file. This behavior prevents casual users from making changes that might adversely affect the contents of the package. For example, it prevents users from rearranging or deleting resources or code modules that might prevent an application from running correctly.

> **Note:** Even though packages are treated as opaque files by default, it is still possible for users to view and modify their contents. On the contextual menu for package directories is a Show Package Contents command. Selecting this command displays a new Finder window set to the top level of the package directory. The user can use this window to navigate the package's directory structure and make changes as if it were a regular directory hierarchy.  

Whereas packages are there to improve the user experience, bundles are geared more toward helping developers package their code and to helping the operating system access that code. Bundles define the basic structure for organizing the code and resources associated with your software. The presence of this structure also helps facilitate important features such as localization. The exact structure of a bundle depends on whether you are creating an application, [framework](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56), or plug-in. It also depends on other factors such as the target platform and the type of plug-in.

The reason bundles and packages are sometimes considered to be interchangeable is that many types of bundles are also packages. For example, applications and loadable bundles are packages because they are usually treated as opaque directories by the system. However, not all bundles are packages and vice versa.

### How the System Identifies Bundles and Packages
The Finder considers a directory to be a package if any of the following conditions are true:

The directory has a known filename extension: `.app`, `.bundle`, `.framework`, `.plugin`, `.kext`, and so on. 

The directory has an extension that some other application claims represents a package type; see [Document Packages](../DocumentPackages/DocumentPackages.html#//apple_ref/doc/uid/10000123i-CH106-SW1). 

The directory has its package bit set. 

The preferred way to specify a package is to give the package directory a known filename extension. For the most part, Xcode takes care of this for you by providing templates that apply the correct extension. All you have to do is create an Xcode project of the appropriate type.  

Most bundles are also packages. For example, applications and plug-ins are typically presented as a single file by the Finder. However, this is not true for all bundle types. In particular, a framework is a type of bundle that is treated as a single unit for the purposes of linking and runtime usage, but framework directories are transparent so that developers can view the header files and other resources they contain. 

### About Bundle Display Names
Display names give the user some control over how bundles and packages appear in the Finder without breaking clients that rely on them. Whereas a user can rename a file freely, renaming an application or framework might cause related code modules that refer to the application or framework by name to break. Therefore, when the user changes the name of a bundle, the change is superficial only. Rather than change the bundle name in the file system, the Finder associates a separate string (known as the *display name*) with the bundle and displays that string instead. 

Display names are for presentation to the user only. You never use display names to open or access directories in your code, but you do use them when displaying the name of the directory to the user. By default, a bundle’s display name is the same as the bundle name itself. However, the system may alter the default display name in the following cases:

If the bundle is an application, the Finder hides the `.app` extension in most cases.

If the bundle supports localized display names (and the user has not explicitly changed the bundle name), the Finder displays the name that matches the user’s current language settings. 

Although the Finder hides the `.app` extension for applications most of the time, it may display it to prevent confusion. For example, if the user changes the name of an application and the new name contains another filename extension, the Finder shows the `.app`. extension to make it clear that the bundle is an application. For example, if you were to add the `.mov` extension to the `Chess` application, the Finder would display `Chess.mov.app` to prevent users from thinking `Chess.mov` is a QuickTime file.

For more information about display names and specifying localized bundle names, see *[File System Overview](../../../../MacOSX/Conceptual/BPFileSystem/BPFileSystem.html#//apple_ref/doc/uid/10000185i)*. 

## The Advantages of Bundles
Bundles provide the following advantages for developers:

Because bundles are directory hierarchies in the file system, a bundle just contains files. Therefore, you can use all of the same file-based interfaces to open your bundle resources as you do to open other types of files.  

The bundle directory structure makes it easy to support multiple localizations. You can easily add new localized resources or remove unwanted ones. 

Bundles can reside on volumes of many different formats, including multiple fork formats like HFS, HFS+, and AFP, and single-fork formats like UFS, SMB, and NFS. 

Users can install, relocate, and remove bundles simply by dragging them around in the Finder. 

Bundles that are also packages, and are therefore treated as opaque files, are less susceptible to accidental user modifications, such as removal, modification, or renaming of critical resources.

A bundle can support multiple chip architectures (PowerPC, Intel) and different address space requirements (32-bit/64-bit). It can also support the inclusion of specialized executables (for example, libraries optimized for a particular set of vector instructions). 

Most (but not all) executable code can be bundled. Applications, frameworks (shared libraries), and plug-ins all support the bundle model. Static libraries, dynamic libraries, shell scripts, and UNIX command line tools do not use the bundle structure. 

A bundled application can run directly from a server. No special shared libraries, extensions, and resources need to be installed on the local system.

## Types of Bundles
Although all bundles support the same basic features, there are variations in the way you define and create bundles that define their intended usage: 

**Application** - An application bundle manages the code and resources associated with a launchable process. The exact structure of this bundle depends on the platform (iOS or macOS) that you are targeting. For information about the structure of application bundles, see [Application Bundles](../BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW13).  

**Frameworks** - A [framework](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56) bundle manages a dynamic shared library and its associated resources, such as header files. An application can link against one or more frameworks to take advantage of the code they contain. For information about the structure of framework bundles, see [Anatomy of a Framework Bundle](../BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW28).  

**Plug-Ins** - macOS supports plug-ins for many system features. Plug-ins are a way for an application to load custom code modules dynamically. The following list identifies some of the key types of plug-ins you might want to develop:

**Custom plug-ins** are plug-ins you define for your own purposes; see [Anatomy of a Loadable Bundle](../BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW32). 

**Image Unit plug-ins** add custom image-processing behaviors to the Core Image technology; see *[Image Unit Tutorial](../../../../GraphicsImaging/Conceptual/ImageUnitTutorial/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004531)*.

**Interface Builder plug-ins** contain custom objects that you want to integrate into Interface Builder’s library window.

**Preference Pane plug-ins** define custom preferences that you want to integrate into the System Preferences application; see *[Preference Pane Programming Guide](../../../../UserExperience/Conceptual/PreferencePanes/PreferencePanes.html#//apple_ref/doc/uid/10000110i)*.

**Quartz Composer plug-ins** define custom patches for the Quartz Composer application; see *[Quartz Composer Custom Patch Programming Guide](../../../../GraphicsImaging/Conceptual/QuartzComposer_Patch_PlugIn_ProgGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004787)*.

**Quick Look plug-ins** support the display of custom document types using Quick Look; see *[Quick Look Programming Guide](../../../../UserExperience/Conceptual/Quicklook_Programming_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40005020)*. 

**Spotlight plug-ins** support the indexing of custom document types so that those documents can be searched by the user; see *[Spotlight Importer Programming Guide](../../../../Carbon/Conceptual/MDImporters/MDImporters.html#//apple_ref/doc/uid/TP40001267)*. 

**WebKit plug-ins** extend the content types supported by common web browsers.

**Widgets** add new HTML-based applications to Dashboard. 

Although document formats can leverage the bundle structure to organize their contents, documents are generally not considered bundles in the purest sense. A document that is implemented as a directory and treated as an opaque type is considered to be a document package, regardless of its internal format. For more information about document packages, see [Document Packages](../DocumentPackages/DocumentPackages.html#//apple_ref/doc/uid/10000123i-CH106-SW1). 

## Creating a Bundle
For the most part, you do not create bundles or packages manually. When you create a new Xcode project (or add a target to an existing project), Xcode automatically creates the required bundle structure when needed. For example, the application, framework, and loadable bundle targets all have associated bundle structures. When you build any of these targets, Xcode automatically creates the corresponding bundle for you.

> **Note:** Some Xcode targets (such as shell tools and static libraries) do not result in the creation of a bundle or package. This is normal and there is no need to create bundles specifically for these target types. The resulting binaries generated for those targets are intended to be used as is.

If you use make files (instead of Xcode) to build your projects, there is no magic to creating a bundle. A bundle is just a directory in the file system with a well-defined structure and a specific filename extension added to the end of the bundle directory name. As long as you create the top-level bundle directory and structure the contents of your bundle appropriately, you can access those contents using the programmatic support for accessing bundles. For more information on how to structure your bundle directory, see [Bundle Structures](../BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW1).

## Programmatic Support for Accessing Bundles
Programs that refer to bundles, or are themselves bundled, can take advantage of interfaces in Cocoa and Core Foundation to access the contents of a bundle. Using these interfaces you can find bundle resources, get information about the bundle’s configuration, and load executable code. In Objective-C applications, you use the `[NSBundle](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/cl/NSBundle)` class to get and manage bundle information. For C-based applications, you can use the functions associated with the `[CFBundleRef](https://developer.apple.com/documentation/corefoundation/cfbundle)` opaque type to manage a bundle.

> **Note:** Unlike many other Core Foundation and Cocoa types, `NSBundle` and `CFBundleRef` are not toll-free bridged data types and cannot be used interchangeably. However, you can extract the bundle path information from either object and use it to create the other.

For information about how to use the programmatic support in Cocoa and Core Foundation to access bundles, see [Accessing a Bundle's Contents](../AccessingaBundlesContents/AccessingaBundlesContents.html#//apple_ref/doc/uid/10000123i-CH104-SW1).  

## Guidelines for Using Bundles
Bundles are the preferred organization mechanism for software in macOS and iOS. The bundle structure lets you group executable code and the resources to support that code in one place and in an organized way. The following guidelines offer some additional advice on how to use bundles:

Always include an information-property list (`Info.plist`) file in your bundle. Make sure you include the keys recommended for your bundle type. For a list of all keys you can include in this file, see *[Runtime Configuration Guidelines](../../../../MacOSX/Conceptual/BPRuntimeConfig/000-Introduction/introduction.html#//apple_ref/doc/uid/10000170i)*. 

If an application cannot run without a specific resource file, include that file inside the application bundle. Applications should always include all of the images, strings files, localizable resources, and plug-ins that they need to operate. Noncritical resources should similarly be stored inside the application bundle whenever possible but may be placed outside the bundle if needed. For more information about the bundle structure of applications, see [Application Bundles](../BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW13).

If you plan to load C++ code from a bundle, you might want to mark the symbols you plan to load as `extern “C”`. Neither `NSBundle` nor the Core Foundation `CFBundleRef` functions know about C++ name mangling conventions, so marking your symbols this way can make it much easier to identify them later.

You cannot use the `NSBundle` class to load Code Fragment Manager (CFM) code. If you need to load CFM-based code, you must use the functions for the `[CFBundleRef](https://developer.apple.com/documentation/corefoundation/cfbundle)` or `[CFPlugInRef](https://developer.apple.com/documentation/corefoundation/cfplugin)` opaque types. You may load CFM-based plugins from a Mach-O executable using this technique.

You should always use the `[NSBundle](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/cl/NSBundle)` class (as opposed to the functions associated with the `CFBundleRef` opaque type) to load any bundle containing Java code. 

When loading bundles containing [Objective-C](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectiveC.html#//apple_ref/doc/uid/TP40008195-CH43) code, you may use either the `NSBundle` class or the functions associated with the `CFBundleRef` opaque type in macOS v10.5 and later, but there are differences in behavior for each. If you use the Core Foundation functions to load a plug-in or other loadable bundle (as opposed to a framework or dynamic shared library), the functions load the bundle privately and bind its symbols immediately; if you use `NSBundle`, the bundle is loaded globally and its symbols are [bound](../../../../General/Conceptual/DevPedia-CocoaCore/DynamicBinding.html#//apple_ref/doc/uid/TP40008195-CH15) lazily. In addition, bundles loaded using the `NSBundle` class cause the generation of `[NSBundleDidLoadNotification](https://developer.apple.com/documentation/foundation/bundle/1416137-didloadnotification)` [notifications](../../../../General/Conceptual/DevPedia-CocoaCore/Notification.html#//apple_ref/doc/uid/TP40008195-CH35), whereas those loaded using the Core Foundation functions do not.  

        
            [Next](../BundleTypes/BundleTypes.html)[Previous](../Introduction/Introduction.html)
        
         Copyright &#x00a9; 2017 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2017-03-27