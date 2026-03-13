---
title: "Bundle Structures"
book: "Bundle Programming Guide"
chapterId: "10000123i-CH101"
date: "2017-03-27"
description: "Explains how to use bundle objects to organize resources."
identifier: "//apple_ref/doc/uid/10000123i"
source: apple-archive
---

# Bundle Structures

> **Bundle Programming Guide**
> Last updated: 2017-03-27

[Next](../AccessingaBundlesContents/AccessingaBundlesContents.html)[Previous](../AboutBundles/AboutBundles.html)
        
        
        [](../index.html)
        
        # Bundle Structures
Bundle structures can vary depending on the type of the bundle and the target platform. The following sections describe the bundle structures used most commonly in both macOS and iOS.

> **Note:** Although bundles are one way of packaging executable code, they are not the only way that is supported. UNIX shell scripts and command-line tools do not use the bundle structure, neither do static and dynamic shared libraries. 

## Application Bundles
Application bundles are one of the most common types of bundle created by developers. The application bundle stores everything that the application requires for successful operation. Although the specific structure of an application bundle depends on the platform for which you are developing, the way you use the bundle is the same on both platforms. This chapter describes the structure of application bundles in both iOS and macOS. 

### What Files Go Into an Application Bundle?
Table 2-1 summarizes the types of files you are likely to find inside an application bundle. The exact location of these files varies from platform to platform and some resources may not be supported at all. For examples and more detailed information, see the platform-specific bundle sections in this chapter.

**Table 2-1**  Types of files in an application bundleFile

Description

`Info.plist` file

(Required) The *information property list* file is a [structured file](../../../../General/Conceptual/DevPedia-CocoaCore/PropertyList.html#//apple_ref/doc/uid/TP40008195-CH44) that contains configuration information for the application. The system relies on the presence of this file to identify relevant information about your application and any related files. 

Executable

(Required) Every application must have an executable file. This file contains the application’s main entry point and any code that was statically linked to the application target.  

Resource files

Resources are data files that live outside your application’s executable file. Resources typically consist of things like images, icons, sounds, [nib files](../../../../General/Conceptual/DevPedia-CocoaCore/NibFile.html#//apple_ref/doc/uid/TP40008195-CH34), strings files, configuration files, and data files (among others). Most resource files can be localized for a particular language or region or shared by all localizations. 

The placement of resource files in the bundle directory structure depends on whether you are developing an iOS or Mac app. 

Other support files

Mac apps can embed additional high-level resources such as private [frameworks](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56), plug-ins, document templates, and other custom data resources that are integral to the application. Although you can include custom data resources in your iOS application bundles, you cannot include custom frameworks or plug-ins. 

Although most of the resources in an application bundle are optional, this may not always be the case. For example, iOS applications typically require additional image resources for the application’s icon and default screen. And although not explicitly required, most Mac apps include a custom icon instead of the default one provided by the system. 

### Anatomy of an iOS Application Bundle
The project templates provided by Xcode do most of the work necessary for setting up the bundle for your iPhone or iPad application. However, understanding the bundle structure can help you decide where you should place your own custom files. The bundle structure of iOS applications is geared more toward the needs of a mobile device. It uses a relatively flat structure with few extraneous directories in an effort to save disk space and simplify access to the files.

#### The iOS Application Bundle Structure
A typical iOS application bundle contains the application executable and any resources used by the application (for instance, the application icon, other images, and localized content) in the top-level bundle directory. Listing 2-1 shows the structure of a simple iPhone application called `MyApp`. The only files that are required to be in subdirectories are those that need to be localized; however, you could create additional subdirectories in your own applications to organize resources and other relevant files.  

**Listing 2-1**  Bundle structure of an iOS application

MyApp.app
```
   MyApp
```

```
   MyAppIcon.png
```

```
   MySearchIcon.png
```

```
   Info.plist
```

```
   Default.png
```

```
   MainWindow.nib
```

```
   Settings.bundle
```

```
   MySettingsIcon.png
```

```
   iTunesArtwork
```

```
   en.lproj
```

```
      MyImage.png
```

```
   fr.lproj
```

```
      MyImage.png
```
Table 2-2 describes the contents of the application shown in [Listing 2-1](#//apple_ref/doc/uid/10000123i-CH101-SW12). Although the application itself is for demonstration purposes only, many of the files it contains represent specific files that iOS looks for when scanning an application bundle. Your own bundles would include some or all of these files depending on the features you support. 

**Table 2-2**  Contents of a typical iOS application bundleFile

Description

`MyApp`

(Required) The executable file containing your application’s code. The name of this file is the same as your application name minus the `.app` extension.

Application icons (`MyAppIcon.png`, `MySearchIcon.png`, and `MySettingsIcon.png`)

(Required/Recommended) Application icons are used at specific times to represent the application. For example, different sizes of the application icon are displayed in the Home screen, in search results, and in the Settings application. Not all of the icons are required but most are recommended. For information about application icons, see [Application Icon and Launch Images](#//apple_ref/doc/uid/10000123i-CH101-SW16). 

`Info.plist`

(Required) This file contains configuration information for the application, such as its bundle ID, version number, and display name. See [The Information Property List File](#//apple_ref/doc/uid/10000123i-CH101-SW5) for further information.

Launch images (`Default.png`)

(Recommended) One or more images that show the initial interface of your application in a specific orientation. The system uses one of the provided launch images as a temporary background until your application loads its window and user interface. If your application does not provide any launch images, a black background is displayed while the application launches. For information about application icons, see [Application Icon and Launch Images](#//apple_ref/doc/uid/10000123i-CH101-SW16).

`MainWindow.nib`

(Recommended) The application’s main [nib file](../../../../General/Conceptual/DevPedia-CocoaCore/NibFile.html#//apple_ref/doc/uid/TP40008195-CH34) contains the default interface objects to load at application launch time. Typically, this nib file contains the application’s main window object and an instance of the application [delegate object](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14). Other interface objects are then either loaded from additional nib files or created programmatically by the application. (The name of the main nib file can be changed by assigning a different value to the `NSMainNibFile` key in the `Info.plist` file. See [The Information Property List File](#//apple_ref/doc/uid/10000123i-CH101-SW5) for further information.) 

`Settings.bundle`

The Settings bundle is a special type of plug-in that contains any application-specific preferences that you want to add to the Settings application. This bundle contains [property lists](../../../../General/Conceptual/DevPedia-CocoaCore/PropertyList.html#//apple_ref/doc/uid/TP40008195-CH44) and other resource files to configure and display your preferences. 

Custom resource files

Non-localized resources are placed at the top level directory and localized resources are placed in language-specific subdirectories of the application bundle. Resources consist of [nib files](../../../../General/Conceptual/DevPedia-CocoaCore/NibFile.html#//apple_ref/doc/uid/TP40008195-CH34), images, sound files, configuration files, strings files, and any other custom data files you need for your application. For more information about resources, see [Resources in an iOS Application](#//apple_ref/doc/uid/10000123i-CH101-SW8).

> **Note:** An iOS app bundle cannot include a custom folder named “Resources.”

An iOS application should be [internationalized](../../../../General/Conceptual/DevPedia-CocoaCore/Internationalization.html#//apple_ref/doc/uid/TP40008195-CH23) and have a *language*`.lproj` folder for each language it supports. In addition to providing localized versions of your application’s custom resources, you can also localize your launch images by placing files with the same name in your language-specific project directories. Even if you provide localized versions, however, you should always include a default version of these files at the top-level of your application bundle. The default version is used in situations where a specific localization is not available. For more information about localized resources, see [Localized Resources in Bundles](#//apple_ref/doc/uid/20001119-105003). 

#### The Information Property List File
Every iOS application must have an information property list (`Info.plist`) file containing the application’s configuration information. When you create a new iOS application project, Xcode creates this file automatically and sets the value of some of the key properties for you. Table 2-3 lists some additional keys that you should set explicitly. (Xcode obscures actual key names by default, so the string displayed by Xcode is also listed in parenthesis where one is used. You can see the real key names for all keys by Control-clicking the Information Property List key in the editor and choosing Show Raw Keys/Values from the contextual menu that appears.) 

**Table 2-3**  Required keys for the `Info.plist` fileKey

Value

`CFBundleDisplayName` (Bundle display name)

The bundle display name is the name displayed underneath the application icon. This value should be localized for all supported languages.

`CFBundleIdentifier` (Bundle identifier)

The bundle identifier string identifies your application to the system. This string must be a uniform type identifier (UTI) that contains only alphanumeric (`A`-`Z`,`a`-`z`,`0`-`9`), hyphen (`-`), and period (`.`) characters. The string should also be in reverse-DNS format. For example, if your company’s domain is `Ajax.com` and you create an application named Hello, you could assign the string `com.Ajax.Hello` as your application’s bundle identifier.  

The bundle identifier is used in validating the application signature. 

`CFBundleVersion` (Bundle version)

The bundle version string specifies the build version number of the bundle. This value is a monotonically increased string, comprised of one or more period-separated integers. This value cannot be localized. 

`CFBundleIconFiles`

An array of strings containing the filenames of the images used for the application’s assorted icons. Although technically not required, it is strongly encouraged that you use it. 

This key is supported in iOS 3.2 and later.

`LSRequiresIPhoneOS` (Application requires iOS environment)

A Boolean value that indicates whether the bundle can run on iOS only. Xcode adds this key automatically and sets its value to true. You should not change the value of this key.

`UIRequiredDeviceCapabilities`

A key that tells iTunes and the App Store know which device-related features an application requires in order to run. iTunes and the mobile App Store use this list to prevent customers from installing applications on a device that does not support the listed capabilities.

The value of this key is either an [array or a dictionary](../../../../General/Conceptual/DevPedia-CocoaCore/Collection.html#//apple_ref/doc/uid/TP40008195-CH10). If you use an array, the presence of a given key indicates the corresponding feature is required. If you use a dictionary, you must specify a Boolean value for each key indicating whether the feature is required. In both cases, not including a key indicates that the feature is not required.  

For a list of keys to include in the dictionary, see *[Information Property List Key Reference](../../../../General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009247)*. This key is supported in iOS 3.0 and later.

In addition to the keys in the preceding table, Table 2-4 lists some keys that are commonly used by iOS applications. Although these keys are not required, most provide a way to adjust the configuration of your application at launch time. Providing these keys can help ensure that your application is presented appropriately by the system.  

**Table 2-4**  Keys commonly included in the `Info.plist` filePara

Para

`NSMainNibFile` (Main nib file base name)

A string that identifies the name of the application’s main [nib file](../../../../General/Conceptual/DevPedia-CocoaCore/NibFile.html#//apple_ref/doc/uid/TP40008195-CH34). If you want to use a nib file other than the default one created for your project, associate the name of that nib file with this key. The name of the nib file should not include the `.nib` filename extension.

`UIStatusBarStyle`

A string that identifies the style of the status bar as the application launches. This value is based on the `[UIStatusBarStyle](https://developer.apple.com/documentation/uikit/uistatusbarstyle)` constants declared in `UIApplication.h` header file. The default style is `[UIStatusBarStyleDefault](https://developer.apple.com/documentation/uikit/uistatusbarstyle/uistatusbarstyledefault)`. The application can change this initial status-bar style when it finishes launching.

If you do not specify this key, iOS displays the default status bar. 

`UIStatusBarHidden`

A Boolean value that determines whether the status bar is initially hidden when the application launches. Set it to true to hide the status bar. The default value is `false`.

`UIInterfaceOrientation`

A string that identifies the initial orientation of the application’s user interface. This value is based on the `[UIInterfaceOrientation](https://developer.apple.com/documentation/uikit/uiinterfaceorientation)` constants declared in the `UIApplication.h` header file. The default style is `[UIInterfaceOrientationPortrait](https://developer.apple.com/documentation/uikit/uiinterfaceorientation/portrait)`.

`UIPrerenderedIcon`

A Boolean value that indicates whether the application icon already includes gloss and bevel effects. The default value is `false`. Set it to `true` if you do not want the system to add these effects to your artwork.

`UIRequiresPersistentWiFi`

A Boolean value that notifies the system that the application uses the Wi-Fi network for communication. Applications that use Wi-Fi for any period of time must set this key to `true`; otherwise, after 30 minutes, the device shuts down Wi-Fi connections to save power. Setting this flag also lets the system know that it should display the network selection dialog when Wi-Fi is available but not currently being used. The default value is `false`. 

Even if the value of this property is `true`, this key has no effect when the device is idle (that is, screen-locked). During that time, the application is considered inactive and, although it may function on some levels, it has no Wi-Fi connection.

`UILaunchImageFile`

A String containing the base filename used by the application’s launch images. If you do not specify this key, the base name is assumed to be the string `Default`. 

#### Application Icon and Launch Images
Application icons and launch images are standard graphics that must be present in every application. Every application must specify an icon to be displayed on the device’s Home screen and in the App Store. And an application may specify several different icons for use in different situations. For example, applications can provide a small version of the application icon to use when displaying search results. Launch images provide visual feedback to the user that your application launched.

The image files used to represent icons and launch images must all reside in the root level of your bundle. How you identify these images to the system can vary, but the recommended way to specify your application icons is to use the `CFBundleIconFiles` key. For detailed information about how to specify the icons and launch images in your application, see the discussion of these items in [Advanced App Tricks](../../../../iPhone/Conceptual/iPhoneOSProgrammingGuide/PerformanceTips/PerformanceTips.html#//apple_ref/doc/uid/TP40007072-CH7) in *[App Programming Guide for iOS](../../../../iPhone/Conceptual/iPhoneOSProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007072)*. 

> **Note:** In addition to the icons and launch image at the top level of your bundle, you can also include localized versions of launch images in your application’s [language-specific project subdirectories](../../../../General/Conceptual/DevPedia-CocoaCore/Internationalization.html#//apple_ref/doc/uid/TP40008195-CH23). For more information about including localized resources in your application, see [Localized Resources in Bundles](#//apple_ref/doc/uid/20001119-105003). 

#### Resources in an iOS Application
In an iOS application, nonlocalized resources are located at the top-level of the bundle directory, along with the application’s executable file and the `Info.plist` file. Most iOS applications have at least a few files at this level, including the application’s icon, launch image, and one or more [nib files](../../../../General/Conceptual/DevPedia-CocoaCore/NibFile.html#//apple_ref/doc/uid/TP40008195-CH34). Although you should place most nonlocalized resources in this top-level directory, you can also create subdirectories to organize your resource files. Localized resources must be placed in one or more language-specific subdirectories, which are discussed in more detail in [Localized Resources in Bundles](#//apple_ref/doc/uid/20001119-105003). 

Listing 2-2 shows a fictional application that includes both localized and nonlocalized resources. The nonlocalized resources include `Hand.png`, `MainWindow.nib`, `MyAppViewController.nib`, and the contents of the `WaterSounds` directory. The localized resources include everything in the `en.lproj` and `jp.lproj` directories. 

**Listing 2-2**  An iOS application with localized and nonlocalized resources

MyApp.app/
```
   Info.plist
```

```
   MyApp
```

```
   Default.png
```

```
   Icon.png
```

```
   Hand.png
```

```
   MainWindow.nib
```

```
   MyAppViewController.nib
```

```
   WaterSounds/
```

```
      Water1.aiff
```

```
      Water2.aiff
```

```
   en.lproj/
```

```
      CustomView.nib
```

```
      bird.png
```

```
      Bye.txt
```

```
      Localizable.strings
```

```
   jp.lproj/
```

```
      CustomView.nib
```

```
      bird.png
```

```
      Bye.txt
```

```
      Localizable.strings
```
For information about finding resource files in your application bundle, see [Accessing a Bundle's Contents](../AccessingaBundlesContents/AccessingaBundlesContents.html#//apple_ref/doc/uid/10000123i-CH104-SW1). For information about how to load resource files and use them in your program, see *[Resource Programming Guide](../../../../Cocoa/Conceptual/LoadingResources/Introduction/Introduction.html#//apple_ref/doc/uid/10000051i)*. 

### Anatomy of a macOS Application Bundle
The project templates provided by Xcode do most of the work necessary for setting up your Mac app bundle during development. However, understanding the bundle structure can help you decide where you should place your own custom files. macOS bundles use a highly organized structure to make it easier for the bundle-loading code to find resources and other important files in the bundle. The hierarchical nature also helps the system distinguish code bundles such as applications from the directory packages used by other applications to implement document types.

#### The Structure of a macOS Application Bundle
The basic structure of a Mac app bundle is very simple. At the top-level of the bundle is a directory named `Contents`. This directory contains everything, including the resources, executable code, private frameworks, private plug-ins, and support files needed by the application. While the `Contents` directory might seem superfluous, it identifies the bundle as a modern-style bundle and separates it from document and legacy bundle types found in earlier versions of Mac OS. 

Listing 2-3 shows the high-level structure of a typical application bundle, including the immediate files and directories you are most likely to find inside the `Contents` directory. This structure represents the core of every Mac app. 

**Listing 2-3**  The basic structure of a Mac app

MyApp.app/
```
   Contents/
```

```
      Info.plist
```

```
      MacOS/
```

```
      Resources/
```
Table 2-5 lists some of the directories that you might find inside the `Contents` directory, along with the purpose of each one. This list is not exhaustive but merely represents the directories in common usage.

**Table 2-5**  Subdirectories of the `Contents` directoryDirectory

Description

`MacOS`

(Required) Contains the application’s standalone executable code. Typically, this directory contains only one binary file with your application’s main entry point and statically linked code. However, you may put other standalone executables (such as command-line tools) in this directory as well.

`Resources`

Contains all of the application’s resource files. This contents of this directory are further organized to distinguish between localized and nonlocalized resources. For more information about the structure of this directory, see [The Resources Directory](#//apple_ref/doc/uid/20001119-110730) 

`Frameworks`

Contains any private shared libraries and [frameworks](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56) used by the executable. The frameworks in this directory are revision-locked to the application and cannot be superseded by any other, even newer, versions that may be available to the operating system. In other words, the frameworks included in this directory take precedence over any other similarly named frameworks found in other parts of the operating system. 

For information on how to add private frameworks to your application bundle, see *[Framework Programming Guide](../../../../MacOSX/Conceptual/BPFrameworks/Frameworks.html#//apple_ref/doc/uid/10000183i)*.  

`PlugIns`

Contains loadable bundles that extend the basic features of your application. You use this directory to include code modules that must be loaded into your application’s process space in order to be used. You would not use this directory to store standalone executables. 

`SharedSupport`

Contains additional non-critical resources that do not impact the ability of the application to run. You might use this directory to include things like document templates, clip art, and tutorials that your application expects to be present but that do not affect the ability of your application to run. 

Application bundles have evolved significantly over the years but the overall goal has been the same. The bundle organization makes it easier for the application to find its resources while making it harder for users to interfere with those resources. Because the Finder treats most bundles as opaque entities, it is difficult for casual users to move or delete the resources an application might need. 

#### The Information Property List File
For the Finder to recognize an application bundle as such, you need to include an information property list (`Info.plist`) file. This file contains XML [property-list](../../../../General/Conceptual/DevPedia-CocoaCore/PropertyList.html#//apple_ref/doc/uid/TP40008195-CH44) data that identifies the configuration of your bundle. For a minimal bundle, this file would contain very little information, most likely just the name and identifier of the bundle. For more complex bundles, the `Info.plist` file includes much more information. 

> **Important:** Bundle resources are located using a case-sensitive search. Therefore, the name of your information property list file must start with a capital “I”.

Table 2-6 lists the keys that you should always include in your `Info.plist` file. Xcode provides all of these keys automatically when you create a new project. (Xcode obscures actual key names by default, so the string displayed by Xcode is also listed in parenthesis. You can see the real key names for all keys by Control-clicking the Information Property List key in the editor and choosing Show Raw Keys/Values from the contextual menu that appears.)  

**Table 2-6**  Expected keys in the `Info.plist` fileKey

Description

`CFBundleName` (Bundle name)

The short name for the bundle. The value for this key is usually the name of your application. Xcode sets the value of this key by default when you create a new project. 

`CFBundleDisplayName` (Bundle display name)

The localized version of your application name. You typically include a localized value for this key in an `InfoPlist.strings` files in each of your language-specific resource directories. 

`CFBundleIdentifier` (Bundle identifier)

The string that identifies your application to the system. This string must be a uniform type identifier (UTI) that contains only alphanumeric (`A`-`Z`,`a`-`z`,`0`-`9`), hyphen (`-`), and period (`.`) characters. The string should also be in reverse-DNS format. For example, if your company’s domain is `Ajax.com` and you create an application named Hello, you could assign the string `com.Ajax.Hello` as your application’s bundle identifier.  

The bundle identifier is used in validating the application signature. 

`CFBundleVersion` (Bundle version)

The string that specifies the build version number of the bundle. This value is a monotonically increased string, comprised of one or more period-separated integers. This value can correspond to either released or unreleased versions of the application. This value cannot be localized. 

`CFBundlePackageType` (Bundle OS Type code)

The type of bundle this is. For applications, the value of this key is always the four-character string `APPL`.

`CFBundleSignature` (Bundle creator OS Type code)

The creator code for the bundle. This is a four-character string that is specific to the bundle. For example, the signature for the TextEdit application is `ttxt`. 

`CFBundleExecutable` (Executable file)

The name of the main executable file. This is the code that is executed when the user launches your application. Xcode typically sets the value of this key automatically at build time. 

Table 2-7 lists the keys that you should also consider including in your `Info.plist` file. 

**Table 2-7**  Recommended keys for the `Info.plist` fileKey

Description

`CFBundleDocumentTypes` (Document types)

The document types supported by the application. This type consists of an array of dictionaries, each of which provides information about a specific document type.

`CFBundleShortVersionString` (Bundle versions string, short)

The release version of the application. The value of this key is a string comprised of three period-separated integers. 

`LSMinimumSystemVersion` (Minimum system version)

The minimum version of macOS required for this application to run. The value for this key is a string of the form *n*.*n*.*n* where each *n* is a number representing either the major or minor version number of macOS that is required. For example, the value 10.1.5 would represent macOS v10.1.5. 

`NSHumanReadableCopyright` (Copyright (human-readable))

The copyright notice for the application. This is a human readable string and can be localized by including the key in an `InfoPlist.strings` file in your language-specific project directories.

`NSMainNibFile` (Main nib file base name)

The [nib file](../../../../General/Conceptual/DevPedia-CocoaCore/NibFile.html#//apple_ref/doc/uid/TP40008195-CH34) to load when the application is launched (without the `.nib` filename extension). The main nib file is an Interface Builder archive containing the objects (main window, application delegate, and so on) needed at launch time.

`NSPrincipalClass` (Principal class)

The entry point for [dynamically loaded](../../../../General/Conceptual/DevPedia-CocoaCore/DynamicBinding.html#//apple_ref/doc/uid/TP40008195-CH15) Objective-C code. For an application bundle, this is almost always the `[NSApplication](https://developer.apple.com/documentation/appkit/nsapplication)` class or a custom subclass.

The exact information you put into your `Info.plist` file is dependent on your bundle’s needs and can be localized as necessary. For more information on this file, see *[Runtime Configuration Guidelines](../../../../MacOSX/Conceptual/BPRuntimeConfig/000-Introduction/introduction.html#//apple_ref/doc/uid/10000170i)*. 

#### The Resources Directory
The `Resources` directory is where you put all of your images, sounds, [nib files](../../../../General/Conceptual/DevPedia-CocoaCore/NibFile.html#//apple_ref/doc/uid/TP40008195-CH34), string resources, icon files, data files, and configuration files among others. The contents of this directory are further subdivided into areas where you can store localized and nonlocalized resource files. Non-localized resources reside at the top level of the `Resources` directory itself or in a custom subdirectory that you define. [Localized resources](../../../../General/Conceptual/DevPedia-CocoaCore/Internationalization.html#//apple_ref/doc/uid/TP40008195-CH23) reside in separate subdirectories called language-specific project directories, which are named to coincide with the specific localization.

The best way to see how the `Resources` directory is organized is to look at an example.  Listing 2-4 shows a fictional application that includes both localized and nonlocalized resources. The nonlocalized resources include `Hand.tiff`, `MyApp.icns` and the contents of the `WaterSounds` directory. The localized resources include everything in the `en.lproj` and `jp.lproj` directories or their subdirectories. 

**Listing 2-4**  A Mac app with localized and nonlocalized resources

MyApp.app/
```
   Contents/
```

```
      Info.plist
```

```
      MacOS/
```

```
         MyApp
```

```
      Resources/
```

```
         Hand.tiff
```

```
         MyApp.icns
```

```
         WaterSounds/
```

```
            Water1.aiff
```

```
            Water2.aiff
```

```
         en.lproj/
```

```
            MyApp.nib
```

```
            bird.tiff
```

```
            Bye.txt
```

```
            InfoPlist.strings
```

```
            Localizable.strings
```

```
            CitySounds/
```

```
               city1.aiff
```

```
               city2.aiff
```

```
         jp.lproj/
```

```
            MyApp.nib
```

```
            bird.tiff
```

```
            Bye.txt
```

```
            InfoPlist.strings
```

```
            Localizable.strings
```

```
            CitySounds/
```

```
               city1.aiff
```

```
               city2.aiff
```
Each of your language-specific project directories should contain a copy of the same set of resource files, and the name for any single resource file must be the same across all localizations. In other words, only the content for a given file should change from one localization to another. When you request a resource file in your code, you specify only the name of the file you want. The bundle-loading code uses the current language preferences of the user to decide which directories to search for the file you requested. 

For information about finding resource files in your application bundle, see [Accessing a Bundle's Contents](../AccessingaBundlesContents/AccessingaBundlesContents.html#//apple_ref/doc/uid/10000123i-CH104-SW1). For information about how to load resource files and use them in your program, see *[Resource Programming Guide](../../../../Cocoa/Conceptual/LoadingResources/Introduction/Introduction.html#//apple_ref/doc/uid/10000051i)*. 

The Application Icon FileOne special resource that belongs in your top-level `Resources` directory is your application icon file. By convention, this file takes the name of the bundle and an extension of `.icns`; the image format can be any supported type, but if no extension is specified, the system assumes `.icns`.

Localizing the Information Property ListBecause some of the keys in an application’s `Info.plist` file contain user-visible strings, macOS provides a mechanism for specifying localized versions of those strings. Inside each language-specific project directory, you can include an `InfoPlist.strings` file that specifies the appropriate localizations. This file is a strings file (not a property list) whose entries consist of the `Info.plist` key you want to localize and the appropriate translation. For example, in the TextEdit application, the German localization of this file contains the following strings:

```
CFBundleDisplayName = "TextEdit";
```

```
NSHumanReadableCopyright = "Copyright © 1995-2009 Apple Inc.\nAlle Rechte vorbehalten.";
```
### Creating an Application Bundle
The simplest way to create an application bundle is using Xcode. All new application projects include an appropriately configured application *target*, which defines the rules needed to build an application bundle, including which source files to compile, which resource files to copy to the bundle, and so on. New projects also include a preconfigured `Info.plist` file and typically several other files to help you get started quickly. You can add any custom files as needed using the project window and configure those files using the Info or Inspector windows. For example, you might use the Info window to specify custom locations for resource files inside your bundle. 

For information on how to configure targets in Xcode, see *[Xcode Build System Guide](../../../../DeveloperTools/Conceptual/XcodeBuildSystem/000-Introduction/Introduction.html#//apple_ref/doc/uid/TP40006904)*.

## Framework Bundles
A [framework](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56) is a hierarchical directory that encapsulates a dynamic shared library and the resource files needed to support that library. Frameworks provide some advantages over the typical dynamic shared library in that they provide a single location for all of the framework’s related resources. For example, most frameworks include the header files that define the symbols exported by the framework. Grouping these files with the shared library and its resources makes it easier to install and uninstall the framework and to locate the framework’s resources.

Just like a dynamic shared library, frameworks provide a way to factor out commonly used code into a central location that can be shared by multiple applications. Only one copy of a framework’s code and resources reside in-memory at any given time, regardless of how many processes are using those resources. Applications that link against the framework then share the memory containing the framework. This behavior reduces the memory footprint of the system and helps improve performance.

> **Note:** Only the code and read-only resources of a framework are shared. If a framework defines writable variables, each application gets its own copy of those variables to prevent it from affecting other applications.  

Although you can create frameworks of your own, most developers’ experience with frameworks comes from including them in their projects. Frameworks are how macOS delivers many key features to your application. The publicly available frameworks provided by macOS are located in the `/System/Library/Frameworks` directory. In iOS, the public frameworks are located in the `System/Library/Frameworks` directory of the appropriate iOS SDK directory. For information about adding frameworks to your Xcode projects, see *[Xcode Build System Guide](../../../../DeveloperTools/Conceptual/XcodeBuildSystem/000-Introduction/Introduction.html#//apple_ref/doc/uid/TP40006904)*. 

> **Note:** The creation of custom frameworks is not supported in iOS.

For more detailed information about frameworks and framework bundles, see *[Framework Programming Guide](../../../../MacOSX/Conceptual/BPFrameworks/Frameworks.html#//apple_ref/doc/uid/10000183i)*.

### Anatomy of a Framework Bundle
The structure of framework bundles differs from that used by applications and plug-ins. The structure for frameworks is based on a bundle format that predates macOS and supports the inclusion of multiple versions of the framework’s code and resources in the framework bundle. This type of bundle is known as a *versioned bundle*. Supporting multiple versions of a framework allows older applications to continue running even as the framework shared library continues to evolve. The bundle’s `Versions` subdirectory contains the individual framework revisions while symbolic links at the top of the bundle directory point to the latest revision.

The system identifies a framework bundle by the `.framework` extension on its directory name. The system also uses the `Info.plist` file inside the framework’s `Resources` directory to gather information about the configuration of the framework. Listing 2-5 shows the basic structure of a framework bundle. The arrows (`->`) in the listing indicate symbolic links to specific files and subdirectories. These symbolic links provide convenient access to the latest version of the framework. 

**Listing 2-5**  A simple framework bundle

MyFramework.framework/
```
   MyFramework  -> Versions/Current/MyFramework
```

```
   Resources    -> Versions/Current/Resources
```

```
   Versions/
```

```
      A/
```

```
         MyFramework
```

```
         Headers/
```

```
            MyHeader.h
```

```
         Resources/
```

```
            English.lproj/
```

```
               InfoPlist.strings
```

```
            Info.plist
```

```
      Current  -> A
```
Frameworks are not required to include a `Headers` directory but doing so allows you to include the header files that define the framework’s exported symbols. Frameworks can store other resource files in both standard and custom directories. 

### Creating a Framework Bundle
If you are developing software for macOS, you can create your own custom frameworks and use them privately or make them available for other applications to use. You can create a new framework using a separate Xcode project or by adding a framework target to an existing project. 

For information about how to create a framework, see *[Framework Programming Guide](../../../../MacOSX/Conceptual/BPFrameworks/Frameworks.html#//apple_ref/doc/uid/10000183i)*. 

## Loadable Bundles
Plug-ins and other types of loadable bundles provide a way for you to extend the behavior of an application dynamically. A loadable bundle consists of executable code and any resources needed to support that code stored in a bundle directory. You can use loadable bundles to load code lazily into your application or to allow other developers to extend the basic behavior of your application.

> **Note:** The creation and use of loadable bundles is not supported in iOS.

### Anatomy of a Loadable Bundle
Loadable bundles are based on the same structure as application bundles. At the top-level of the bundle is a single `Contents` directory. Inside this directory are several subdirectories for storing executable code and resources. The `Contents` directory also contains the bundle’s `Info.plist` file with information about the bundle’s configuration. 

Unlike the executable of an application, loadable bundles generally do not have a `main` function as their main entry point. Instead, the application that loads the bundle is responsible for defining the expected entry point. For example, a bundle could be expected to define a function with a specific name or it could be expected to include information in its `Info.plist` file identifying a specific function or class to use. This choice is left to the application developer who defines the format of the loadable bundle.

Listing 2-6 shows the layout of a loadable bundle. The top-level directory of a loadable bundle can have any extension, but common extensions include `.bundle` and `.plugin`. macOS always treats bundles with those extensions as packages, hiding their contents by default. 

**Listing 2-6**  A simple loadable bundle

MyLoadableBundle.bundle
```
   Contents/
```

```
      Info.plist
```

```
      MacOS/
```

```
         MyLoadableBundle
```

```
      Resources/
```

```
         Lizard.jpg
```

```
         MyLoadableBundle.icns
```

```
         en.lproj/
```

```
            MyLoadableBundle.nib
```

```
            InfoPlist.strings
```

```
         jp.lproj/
```

```
            MyLoadableBundle.nib
```

```
            InfoPlist.strings
```
In addition to the `MacOS` and `Resources` directories, loadable bundles may contain additional directories such as `Frameworks`, `PlugIns`, `SharedFrameworks`, and `SharedSupport`—all the features supported by full-fledged application packages. 

The basic structure of a loadable bundle is the same regardless of which language that bundle uses in its implementation. For more information about the structure of loadable bundles, see *[Code Loading Programming Topics](../../../../Cocoa/Conceptual/LoadingCode/LoadingCode.html#//apple_ref/doc/uid/10000052i)*.  

### Creating a Loadable Bundle
If you are developing software for macOS, you can create your own custom loadable bundles and incorporate them into your applications. If other applications export a plug-in API, you can also develop bundles targeted at those APIs. Xcode includes template projects for implementing bundles using either C or Objective-C, depending on the intended target application. 

For more information about how to design loadable bundles using Objective-C, see *[Code Loading Programming Topics](../../../../Cocoa/Conceptual/LoadingCode/LoadingCode.html#//apple_ref/doc/uid/10000052i)*. For information about how to design loadable bundles using the C language, see *[Plug-in Programming Topics](../../CFPlugIns/CFPlugIns.html#//apple_ref/doc/uid/10000128i)*. 

## Localized Resources in Bundles
Within the `Resources` directory of a macOS bundle (or the top-level directory of an iOS application bundle), you can create one or more [language-specific project subdirectories](../../../../General/Conceptual/DevPedia-CocoaCore/Internationalization.html#//apple_ref/doc/uid/TP40008195-CH23) to store language- and region-specific resources. The name of each directory is based on the language and region of the desired localization followed by the `.lproj` extension. To specify the language and region, you use the following format: 

*language*`_`*region*`.lproj`

The *language* portion of the directory name is a two-letter code that conforms to the ISO 639 conventions. The *region* portion is also a two-letter code but it conforms to the ISO 3166 conventions for designating specific regions. Although the region portion of the directory name is entirely optional, it can be a useful way to tune your localizations for specific parts of the world. For example, you could use a single `en.lproj` directory to support all English speaking nations. However, providing separate localizations for Great Britain (`en_GB.lproj`), Australia (`en_AU.lproj`), and the United States (`en_US.lproj`) lets you tailor your content for each of those countries.  

> **Note:** For backwards compatibility, the `[NSBundle](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/cl/NSBundle)` class and `[CFBundleRef](https://developer.apple.com/documentation/corefoundation/cfbundle)` functions also support human-readable directory names for several common languages, including `English.lproj`, `German.lproj`, `Japanese.lproj`, and others. Although the human-readable names are supported, the ISO names are preferred.

If most of your resource files are the same for all regions of a given language, you can combine a language-only resource directory with one or more region-specific directories. Providing both types of directories alleviates the need to duplicate every resource file for each region you support. Instead, you can customize only the subset of files that are needed for a particular region. When looking for resources in your bundle, the bundle-loading code looks first in any region-specific directories, followed by the language-specific directory. And if neither localized directory contains the resource, the bundle-loading code looks for an appropriate nonlocalized resource. 

> **Important:** Do not store code in your `lproj` folders, because the system does not load or execute code stored in `lproj` folders. To learn more about where code and other types of data should be stored in your app bundle, see *[macOS Code Signing In Depth](../../../../../technotes/tn2206/_index.html#//apple_ref/doc/uid/DTS40007919)*.

Listing 2-7 shows the potential structure of a Mac app that contains both language- and region-specific resource files. (In an iOS application, the contents of the `Resources` directory would be at the top-level of the bundle directory.) Notice that the region-specific directories contain only a subset of the files in the `en.lproj` directory. If a region-specific version of a resource is not found, the bundle looks in the language-specific directory (in this case `en.lproj`) for the resource. The language-specific directory should always contain a complete copy of any language-specific resource files. 

**Listing 2-7**  A bundle with localized resources

Resources/
```
   MyApp.icns
```

```
   en_GB.lproj/
```

```
      MyApp.nib
```

```
      bird.tiff
```

```
      Localizable.strings
```

```
   en_US.lproj/
```

```
      MyApp.nib
```

```
      Localizable.strings
```

```
   en.lproj/
```

```
      MyApp.nib
```

```
      bird.tiff
```

```
      Bye.txt
```

```
      house.jpg
```

```
      InfoPlist.strings
```

```
      Localizable.strings
```

```
      CitySounds/
```

```
         city1.aiff
```

```
         city2.aiff
```
For more information on language codes and the process for localizing resources, see *[Internationalization and Localization Guide](../../../../MacOSX/Conceptual/BPInternational/Introduction/Introduction.html#//apple_ref/doc/uid/10000171i)*.

        
            [Next](../AccessingaBundlesContents/AccessingaBundlesContents.html)[Previous](../AboutBundles/AboutBundles.html)
        
         Copyright &#x00a9; 2017 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2017-03-27