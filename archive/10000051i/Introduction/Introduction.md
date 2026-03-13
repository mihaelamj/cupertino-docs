---
title: "Introduction"
book: "Resource Programming Guide"
chapterId: "10000051i-CH1"
date: "2016-03-21"
description: "Explains how to work with nib and bundle resources in apps."
identifier: "//apple_ref/doc/uid/10000051i"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Introduction

> **Resource Programming Guide**
> Last updated: 2016-03-21

[Next](../CocoaNibs/CocoaNibs.html)
        
        
        [](../index.html)
        
        # About Resources
Applied to computer programs, resources are data files that accompany a program’s executable code. Resources simplify the code you have to write by moving the creation of complex sets of data or graphical content outside of your code and into more appropriate tools. For example, rather than creating images pixel by pixel using code, it is much more efficient (and practical) to create them in an image editor. To take advantage of a resource, all your code has to do is load it at runtime and use it.

In addition to simplifying your code, resources are also an intimate part of the internationalization process for all applications. Rather than hard-coding strings and other user-visible content in your application, you can place that content in external resource files. Localizing your application then becomes a simple process of creating new versions of each resource file for each supported language. The [bundle](../../../../General/Conceptual/DevPedia-CocoaCore/Bundle.html#//apple_ref/doc/uid/TP40008195-CH4) mechanism used in both OS X and iOS provides a way to organize localized resources and to facilitate the loading of resource files that match the user’s preferred language. 

This document provides information about the types of resources supported in OS X and iOS and how you use those resources in your code. This document does not focus on the resource-creation process. Most resources are created using either third-party applications or the developer tools provided in the `/Developer/Applications` directory. In addition, although this document refers to the use of resources in applications, the information also applies to other types of bundled executables, including [frameworks](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56) and plug-ins. 

Before reading this document, you should be familiar with the organizational structure imposed by application bundles. Understanding this structure makes it easier to organize and find the resource files your application uses. For information on the structure of bundles, see *[Bundle Programming Guide](../../../../CoreFoundation/Conceptual/CFBundles/Introduction/Introduction.html#//apple_ref/doc/uid/10000123i)*.

## At a Glance
Applications can contain many types of resources but there are several that are supported directly by iOS and OS X. 

### Nib Files Store the Objects of Your Application’s User Interface
Nib files are the quintessential resource type used to create iOS and Mac apps. A *nib file* is a data archive that essentially contains a set of freeze-dried objects that you want to recreate at runtime. Nib files are used most commonly to store preconfigured windows, views, and other visually oriented objects but they can also store nonvisual objects such as controllers. 

You edit nib files in Xcode with Interface Builder, which provides a graphical editor for assembling your objects. When you subsequently load a nib file into your application, the nib-loading code instantiates each object in the file and restores it to the state you specified in Interface Builder. Thus, what you see in Interface Builder is really what you get in your application at runtime. 

> **Relevant Chapters:** [Nib Files](../CocoaNibs/CocoaNibs.html#//apple_ref/doc/uid/10000051i-CH4-SW8)

### String Resources Containing Localizable Text
Text is a prominent part of most user interfaces but also a resource that is most affected by localization changes. Rather than hard-coding text into your code, iOS and OS X support the storage of user-visible text in *strings files*, which are human-readable text files (in the UTF-16 encoding) containing a set of string resources for an application. (The use of the plural “strings” in is deliberate and due to the `.strings` filename extension used by files of that type.) Strings files greatly simplify the internationalization and localization process by allowing you to write your code once and then load the appropriately localized text from resource files that can be changed easily. 

The Core Foundation and Foundation frameworks provide the facilities for loading text from strings files. Applications that use these facilities can also take advantage of tools that come with Xcode to generate and maintain these resource files throughout the development process. 

> **Relevant Chapters:** [String Resources](../Strings/Strings.html#//apple_ref/doc/uid/10000051i-CH6-SW1)

### Images, Sounds, and Movies Represent Pre-rendered Content
Images, sound, and movie resources play an important role in iOS and Mac apps. Images are responsible for creating the unique visual style used by each operating system; they also help simplify your drawing code for complex visual elements. Sound and movie files similarly help enhance the overall user experience of your application while simplifying the code needed to create that experience. Both operating systems provide extensive support for loading and presenting these types of resources in your applications. 

> **Relevant Chapters:** [Image, Sound, and Video Resources](../ImageSoundResources/ImageSoundResources.html#//apple_ref/doc/uid/10000051i-CH7-SW1)

### Property Lists and Data Files Separate Data from Code
A property list file is a structured file used to store string, number, Boolean, date, and raw data values. Data items in the file are organized using array and dictionary structures with most items associated with a unique key. The system uses property lists to store simple data sets. For example, the `Info.plist` file found in nearly every application is an example of a property list file. You can also use property list files for simple data storage needs.

In addition to property lists, OS X supports some specially structured files for specific uses. For example, AppleScript data and user help are stored using specially formatted data files. You can also create custom data files of your own.

> **Relevant Chapters:** [Data Resource Files](../DataResourceFiles/DataResourceFiles.html#//apple_ref/doc/uid/10000051i-CH8-SW2)

### iOS Supports Device-Specific Resources
In iOS 4.0 and later, it is possible to mark individual resource files as usable only on a specific type of device. This capability simplifies the code you have to write for Universal applications. Rather than creating separate code paths to load one version of a resource file for iPhone and a different version of the file for iPad, you can let the bundle-loading routines choose the correct file. All you have to do is name your resource files appropriately.

To associate a resource file with a particular device, you add a custom modifier string to its filename. The inclusion of this modifier string yields filenames with the following format:

*<basename>**<device>*`.`*<filename_extension>*

The *<basename>* string represents the original name of the resource file. It also represents the name you use when accessing the file from your code. Similarly, the *<filename_extension>* string is the standard filename extension used to identify the type of the file. The *<device>* string is a case-sensitive string that can be one of the following values:

`~ipad` - The resource should be loaded on iPad devices only.

`~iphone` - The resource should be loaded on iPhone or iPod touch devices only.

You can apply device modifiers to any type of resource file. For example, suppose you have an image named `MyImage.png`. To specify different versions of the image for iPad and iPhone, you would create resource files with the names `MyImage~ipad.png` and `MyImage~iphone.png` and include them both in your bundle. To load the image, you would continue to refer to the resource as `MyImage.png` in your code and let the system choose the appropriate version, as shown here:

UIImage* anImage = [UIImage imageNamed:@"MyImage.png"];On an iPhone or iPod touch device, the system loads the `MyImage~iphone.png` resource file, while on iPad, it loads the `MyImage~ipad.png` resource file. If a device-specific version of a resource is not found, the system falls back to looking for a resource with the original filename, which in the preceding example would be an image named `MyImage.png`.

## See Also
The following Apple Developer documents are conceptually related to *Resource Programming Guide*:

*[Bundle Programming Guide](../../../../CoreFoundation/Conceptual/CFBundles/Introduction/Introduction.html#//apple_ref/doc/uid/10000123i)* describes the bundle structure used by applications to store executable code and resources. 

*[Internationalization and Localization Guide](../../../../MacOSX/Conceptual/BPInternational/Introduction/Introduction.html#//apple_ref/doc/uid/10000171i)* describes the process of preparing an application (and its resources) for translation into other languages. 

The chapter Build a User Interface in *[Xcode Overview](../../../../ToolsLanguages/Conceptual/Xcode_Overview/index.html#//apple_ref/doc/uid/TP40010215)* describes the tool for editing nib file resources. 

*[Property List Programming Guide](../../PropertyLists/Introduction/Introduction.html#//apple_ref/doc/uid/10000048i)* describes the facilities in place for loading property-list resource files into a Cocoa application. 

*[Property List Programming Topics for Core Foundation](../../../../CoreFoundation/Conceptual/CFPropertyLists/CFPropertyLists.html#//apple_ref/doc/uid/10000130i)*  describes the facilities in place for loading property-list resource files into a C-based application. 

        
            [Next](../CocoaNibs/CocoaNibs.html)
        
         Copyright &#x00a9; 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-03-21