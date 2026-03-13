---
title: "Supporting Common App Behaviors"
book: "Mac App Programming Guide"
chapterId: "TP40010543-CH7"
date: "2015-03-09"
description: "Introduces the development process for Mac apps."
identifier: "//apple_ref/doc/uid/TP40010543"
source: apple-archive
---

# Supporting Common App Behaviors

> **Mac App Programming Guide**
> Last updated: 2015-03-09

[Next](../BuildTimeConfiguration/BuildTimeConfiguration.html)[Previous](../FullScreenApp/FullScreenApp.html)
        
        
        [](../index.html)
        
        # Supporting Common App Behaviors
During the design phase of creating your app, you need to think about how to implement certain features that users expect in well-formed Mac apps. Integrating these features into your app architecture can have an impact on the data model or may require cooperation between significantly different portions in the app.

## You Can Prevent the Automatic Relaunch of Your App
By default, as part of the Resume feature of OS X v10.7, apps that were open at logout are relaunched by the system when the user logs in again. You can prevent the automatic relaunching of your app at login by sending it a `disableRelaunchOnLogin` message. This `NSApplication` method increments a counter that controls the app being relaunched; if the counter is 0 at the time the user logs out, then the app is relaunched when the user logs back in. The counter is initially zero, providing the default relaunch behavior.

Your can reinstate automatic relaunching of your app by sending it an `enableRelaunchOnLogin` message. This message decrements the relaunch counter, so an equal number of `disableRelaunchOnLogin` and `enableRelaunchOnLogin` messages enables relaunching. Both methods are thread safe.

If your app should not be relaunched because it launches via some other mechanism, such as the `launchd` system process, then the recommended practice is to send the app a `disableRelaunchOnLogin` message once, and never pair it with an `enableRelaunchOnLogin` message.

If your app should not be relaunched because it triggers a restart (for example, if it is an installer), then the recommended practice is to send it a `disableRelaunchOnLogin` message immediately before you attempt to trigger a restart and send it an `enableRelaunchOnLogin` message immediately after. This procedure handles the case where the user cancels restarting; if the user later restarts for another reason, then your app should be relaunched.

## Making Your App Accessible Enables Many Users
Millions of people have a disability or special need. These include visual and hearing impairments, physical disabilities, and cognitive and learning challenges. Access to computers is vitally important for this population, because computers can provide a level of independence that is difficult to attain any other way. As populations around the world age, an increasing number of people will experience age-related disabilities, such as vision or hearing loss. Current and future generations of the elderly will expect to be able to continue using their computers and accessing their data, regardless of the state of their vision and hearing. Apps that support customizable text displays, access by a screen reader, or the replacement of visual cues by audible ones can serve this population well.

OS X is designed to accommodate assistive technologies and has many built-in features to help people with disabilities. Users access most of this functionality through the Universal Access pane of System Preferences. Some of these built-in technologies take advantage of the same accessibility architecture that allows external assistive technologies to access your app. Designing your app with accessibility in mind not only allows you to reach a larger group of users, it results in a better experience for all your users.

As a first step in designing your app, be sure to read *OS X Human Interface Guidelines*. That book provides detailed specifications and best practices for designing and implementing an intuitive, consistent, and aesthetically pleasing user interface that delivers the superlative experience Macintosh users have come to expect. During the design process, you also should be aware of the accessibility perspective on many basic design considerations. Consider the following basic accessibility-related design principles:

**Support full keyboard navigation.** For many users, a mouse is difficult, if not impossible, to use. Consequently, a user should be able to perform all your app’s functions using the keyboard alone.

**Don’t override built-in keyboard shortcuts.** This applies both to the keyboard shortcuts OS X reserves (listed in Keyboard Shortcuts) and to the accessibility-related keyboard shortcuts (listed in [Accessibility Keyboard Shortcuts](../../../../Accessibility/Conceptual/AccessibilityMacOSX/OSXAXKeyboardShortcuts.html#//apple_ref/doc/uid/TP40001078-CH253)). As a general rule, you should never override reserved keyboard shortcuts. In particular, you should take care not to override accessibility-related keyboard shortcuts or your app will not be accessible to users who enable full keyboard access.

A corollary to this principle is to avoid creating too many new keyboard shortcuts that are specific to your app. Users should not have to memorize a new set of keyboard commands for each app they use.

**Provide alternatives for drag-and-drop operations.** If your app relies on drag-and-drop operations in its workflow, you should provide alternate ways to accomplish the same tasks. This may not be easy; in fact, it may require the design of an alternate interface for apps that are heavily dependent on drag and drop.

**Make sure there’s always a way out of your app’s workflow.** This is important for all users, of course, but it’s essential for users of assistive technologies. A user relying on an assistive app to use your app may have a somewhat narrower view of your app’s user interface. For this reason, it’s especially important to make canceling operations and retracing steps easy.

In addition to the basic design principles, you should consider the requirements of specific disabilities and resulting design solutions and adaptations you can implement.  The main theme of these suggestions is to provide as many alternate modes of content display as possible, so that users can find the way that suits their needs best. Consider the following categories of disabilities:

**Visual Disabilities.** Visual disabilities include blindness, color blindness, and low vision. Make your app accessible to assistive apps, such as screen readers. Ensure that color is not the only source of come particular information in your user interface. Provide an audio option for all visual cues and feedback, as well as succinct descriptions of images and animated content.

**Hearing Disabilities.** When you design the audio output of your app, remember that some users may not be able to hear your app’s sound effects well or at all. And, of course, there are situations in which any user could not use audio output effectively, such as in a library. Providing redundant audio and visual output can aid comprehension for users with other types of disabilities as well, such as cognitive and learning disabilities.

**Motor and Cognitive Disabilities.** People with motor disabilities may need to use alternatives to the standard mouse and keyboard input devices. Other users may have difficulty with the fine motor control required to double-click a mouse or to press key combinations on the keyboard. Users with cognitive or learning disabilities may need extra time to complete tasks or respond to alerts.

OS X provides support for many types of disabilities at the system level through solutions offered in the Universal Access system preferences, illustrated in Figure 4-1. In addition, most standard Cocoa objects implement accessibility through the `NSAccessibility` protocol, providing reasonable default behavior in most cases, Cocoa apps built with standard objects are automatically accessible. In general, you need to explicitly implement the `NSAccessibility` protocol methods only if you subclass one of them, adding new behavior.

**Figure 4-1**  Universal Access system preference dialogThe `NSAccessibility` informal protocol defines methods that Cocoa classes must implement to make themselves available to an external assistive app. An assistive app interacts with your app to allow persons with disabilities to use your app. For example, a person with a visual impairment could use an app to convert menu items and button labels into speech and then perform actions by verbal command.

An accessible object is described by a set of attributes that define characteristics such as the object type, its value, its size and position on the screen, and its place in the accessibility hierarchy. For some objects, the set of attributes can include parameterized attributes. Parameterized attributes behave similar to a function by allowing you to pass a parameter when requesting an attribute value.

For more information, see *[Accessibility Programming Guide for OS X](../../../../Accessibility/Conceptual/AccessibilityMacOSX/index.html#//apple_ref/doc/uid/TP40001078)*.

## Provide User Preferences for Customization
Preferences are settings that enable users to customize the appearance or behavior of your software. The OS X user defaults system lets you access and manage user preferences. You can use the defaults system to provide reasonable initial values for app settings, as well as save and retrieve the user's own preference selections across sessions. The Cocoa `NSUserDefaults` class provides programmatic access to the user defaults system.

The `NSUserDefaults` class provides convenience methods for accessing common types such as floats, doubles, integers, Booleans, and URLs. A default object must be a property list, that is, an instance of (or for collections a combination of instances of): `NSData`, `NSString`, `NSNumber`, `NSDate`, `NSArray`, or `NSDictionary`. If you want to store any other type of object, you should typically archive it to create an instance of `NSData`.

The user defaults system groups defaults into domains. Two of the domains are persistently saved in the user defaults database: the app domain stores app-specific defaults, and the global domain stores defaults applicable to all apps. In addition, the user defaults system provides three volatile domains whose values last only while the user defaults object exists: the argument domain for defaults set from command-line arguments, the languages domain containing defaults for a locale if one is specified, and the registration domain for “factory defaults” registered by the app.

Your app uses the `NSUserDefaults` class to register default preferences, and typically it also provides a user interface (a preferences panel) that enables the user to change those preferences. Preferences are stored in the `~/Library/Preferences/` folder in a standard XML-format property list file as a set of key-value pairs. The app-specific preferences property list is identified by the bundle identifier of the app.

For more details, see *[Preferences and Settings Programming Guide](../../../../Cocoa/Conceptual/UserDefaults/Introduction/Introduction.html#//apple_ref/doc/uid/10000059i)*.

## Integrate Your App With Spotlight Search
Spotlight is a fast desktop search technology that allows users to organize and search for files based on metadata. Metadata is data about a file, rather than the actual content stored in the file. Metadata can include familiar information such as an asset’s author and modification date but it can also be keywords or other information that is custom to a particular asset. For example, an image file might have metadata describing the image’s dimensions and color model.

Developers of apps that save documents to disk should consider providing Spotlight support by implementing a metadata importer. A Spotlight metadata importer is a small plug-in bundle that you create to extract information from files created by your app. Spotlight importers are used by the Spotlight server to gather information about new and existing files, as illustrated in Figure 4-2.

**Figure 4-2**  Spotlight extracting metadataApple provides importers for many standard file types that the system uses, including RTF, JPEG, Mail, PDF and MP3. However, if you define a custom document format, you must create a metadata importer for your own content. Xcode provides a template for writing Spotlight importer plug-ins. For information about writing a Spotlight importer, see *[Spotlight Importer Programming Guide](../../../../Carbon/Conceptual/MDImporters/MDImporters.html#//apple_ref/doc/uid/TP40001267)*.

In addition, you may wish to provide metadata search capability in your app. Cocoa provides the `NSMetadataQuery` class which enables you to construct queries using a subset of the `NSPredicate` classes and execute the queries asynchronously. For information about providing Spotlight search capability in your app, see *[File Metadata Search Programming Guide](../../../../Carbon/Conceptual/SpotlightQuery/Concepts/Introduction.html#//apple_ref/doc/uid/TP40001841)*.

For more information about Spotlight, see *[Spotlight Overview](../../../../Carbon/Conceptual/MetadataIntro/MetadataIntro.html#//apple_ref/doc/uid/TP40001268)*.

## Use Services to Increase Your App’s Usefulness
Services allow a user to access the functionality of one app from within another app. An app that provides a service advertises the operations it can perform on a particular type of data—for example, encryption of text, optical character recognition of a bitmapped image, or generating text such as a message of the day. When the user is manipulating that particular type of data in some app, the user can choose the appropriate item in the Services menu to operate on the current data selection (or merely insert new data into the document).

See *[Services Implementation Guide](../../../../Cocoa/Conceptual/SysServices/introduction.html#//apple_ref/doc/uid/10000101i)* for more information.

## Optimize for High Resolution
High-resolution displays provide a rich visual experience, allowing users to see sharper text and more details in photos than on standard-resolution displays. The high-resolution model for OS X enables your app to draw into an abstract coordinate space called **user space**, without regard for the characteristics of the final drawing destination—printer, display screen, bitmap, or PDF—and without regard to the resolution of the display.

OS X provides much support for high-resolution automatically. For example, standard AppKit views and controls automatically render correctly at any resolution, vector-based content automatically uses additional pixels to render sharper lines and shapes, Cocoa text displays sharper in high-resolution, and AppKit automatically loads high-resolution variants of your images.

You must do the following things to optimize your app for high-resolution:

Provide properly-named high-resolution versions of your bitmapped images.

Use high-resolution-savvy image-loading methods.

Use the most recent APIs that support high resolution.

These techniques are described in the following sections.

### Think About Points, Not Pixels
OS X refers to screen size in points, not pixels. A point is one unit in user space, prior to any transformations on the space. Because, on a high-resolution display, there are four onscreen pixels for each point, points can be expressed as floating-point values. Values that are integers in standard resolution, such as mouse coordinates, are floating-point values on a high-resolution display, allowing for greater precision for such things as graphics alignment.

Your app draws to a view using points in user space. The window server composites drawing operations to an offscreen buffer called the **backing store**. When it comes time to display the contents of the backing store onscreen, the window server scales the content appropriately, mapping points to onscreen pixels. The result is that if you draw the same content on two similar devices, and only one of them has a high-resolution screen, the content appears to be about the same size on both devices, as shown in Figure 4-3.

**Figure 4-3**  Content appears the same size at standard resolution and high resolution In some situations you may need to know how points are mapped to pixels, in which case you can obtain the **backing scale factor** which is always either 1.0 or 2.0. The backing scale factor is a property of a layer, view, or window, and it depends on the resolution of the underlying display device.

### Provide High-Resolution Versions of Graphics
OS X automatically magnifies standard-resolution bitmapped images so they appear to the user in the correct size, but they appear fuzzy. To avoid this problem, you must provide high-resolution versions of your graphics, along with the standard-resolution versions in the app bundle. In addition to any images your app displays, you must do this for your app’s icons and any custom controls, cursors, and other artwork.

High-resolution graphics must be scaled with twice as many pixels in each dimension to display the same size in user space. For example, if you supply a standard-resolution image sized at 50x50 pixels, the high-resolution version must be sized at 100x100 pixels.

For AppKit to recognize and load high-resolution versions of your graphics at appropriate times, you must adopt the naming convention of appending `@2x` to the image name. For example, a standard-resolution image named `circle.png` would have a high-resolution counterpart named `circle@2x.png`. Ideally, you can package both image versions into a single TIFF file. This is most easily done by setting the Xcode option Combine High Resolution Artwork to `Yes` in Target Build Settings under Deployment.

You should create a set of icons for your app consisting of standard- and high-resolution versions of each icon size—16x16, 32x32, 128x128, 256x256, 512x512 appending `@2x` to the icon image name which, by convention, specifies the icon size in user space points. For example, an icon named `icon_16x16.png` would have a high-resolution counterpart named `icon_16x16@2x.png`, the `icon_32x32.png` size would have a version named `icon_32x32@2x.png`, and so on. 

After you’ve created your set of app icons, place them in folder a named `icon.iconset`. Then you can use the `iconutil` command-line tool to convert your `.iconset` folder into a single, deployment-ready, high-resolution `.icns` file.

### Use High-Resolution-Savvy Image-Loading Methods
If you follow the `@2x` naming convention, there are two methods available that load standard- and high-resolution versions of an image into an `NSImage` object, whether or not you provide a multirepresentation image file:

The `NSImage` method `[imageNamed:](https://developer.apple.com/documentation/appkit/nsimage/1520015-imagenamed)` finds resources located in the main application bundle, but not in frameworks or plug-ins.

The `NSBundle` method `[imageForResource:](https://developer.apple.com/documentation/foundation/nsbundle/1519901-imageforresource)` looks for resources outside as well as inside the main bundle. Framework authors should use this method.

To create an `NSImage` object from a `CGImageRef` data type, use the `[initWithCGImage:size:](https://developer.apple.com/documentation/appkit/nsimage/1519939-init)` method, specifying the image size in points, not pixels. For custom cursors, you can pass a multirepresentation TIFF to the `NSCursor` class method `[initWithImage:hotSpot:](https://developer.apple.com/documentation/appkit/nscursor/1524612-init)`.

### Use APIs That Support High Resolution
Cocoa apps must replace deprecated APIs with their newer counterparts. Apps that use older technologies need to replace those technologies with newer ones. `NSImage`, `NSView`, `NSWindow`, and `NSScreen` classes have methods that support high resolution, including methods for converting geometry, detecting scaling, and aligning pixels. These APIs and deprecated technologies that you must avoid are described in *[High Resolution Guidelines for OS X](../../../../GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html#//apple_ref/doc/uid/TP40012302)*.

You should also consider whether your app requires further adjustments for special scenarios, such as using pixel-based technologies (OpenGL, Quartz bitmaps) or custom Core Animation layers. These advanced optimization techniques are described in *[High Resolution Guidelines for OS X](../../../../GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html#//apple_ref/doc/uid/TP40012302)*, which also provides much more detailed information about high resolution.

## Prepare for Fast User Switching
Fast user switching lets users share a single machine without having to log out every time they want to access their user account. Users share physical access to the machine, including the same keyboard, mouse, and monitor. However, instead of logging out, a new user simply logs in and switches out the previous user.

Processes in a switched-out login session continue running as before. They can continue processing data, communicating with the system, and drawing to the screen buffer as before. However, because they are switched out, they do not receive input from the keyboard and mouse. Similarly, if they were to check, the monitor would appear to be in sleep mode. As a result, it may benefit some apps to adjust their behavior while in a switched-out state to avoid wasting system resources.

While fast user switching is a convenient feature for users, it does provide several challenges for app developers. Apps that rely on exclusive access to certain resources may need to modify their behavior to live in a fast user switching environment. For example, an app that stores temporary data in `/tmp` may run into problems when a second instance running under a different user tries to modify the same files in that directory.

To support fast user switching, there are certain guidelines you should follow in developing your apps, most of which describe safe ways to identify and share system resources. A summary of these guidelines is as follows:

Incorporate session ID information into the name of any entity that appears in a global namespace, including file names, shared memory regions, semaphores, and named pipes.

Tagging such app-specific resources with a session ID is necessary to distinguish them from similar resources created by apps in a different session. The Security layer of the system assigns a unique ID to each login session. Incorporating this ID into cache file or temporary directory names can prevent namespace collisions when creating these files. See [Supporting Fast User Switching](../../../../MacOSX/Conceptual/BPMultipleUsers/Concepts/FastUserSwitching.html#//apple_ref/doc/uid/20002209) for information on how to get the session ID.

Don't assume you have exclusive access to any resource, especially TCP/IP ports and hardware devices.

Don't assume there is only one instance of a per-user service running.

Use file-level or range-level locks when accessing files.

Accept and handle user switch notifications. See [User Switch Notifications](../../../../MacOSX/Conceptual/BPMultipleUsers/Concepts/UserSwitchNotifications.html#//apple_ref/doc/uid/20002210) for more information.

For more information on user switching, see *[Multiple User Environment Programming Topics](../../../../MacOSX/Conceptual/BPMultipleUsers/BPMultipleUsers.html#//apple_ref/doc/uid/10000180i)*.

## Take Advantage of the Dock
The Dock is a desktop app designed to reduce desktop clutter, provide users with feedback about an app, and allow users to switch easily between tasks. You can customize your app Dock tile by modifying the Dock icon and adding items to the menu displayed for your app, and you can customize the Dock icon of a minimized window.

An app’s Dock icon is, by default, the app’s icon. While your app is running, you can modify or replace the default icon with another image that indicates the current state of your app. For example, the icon for Mail changes when messages are waiting to be read. A badge—the red circle and number in the figure—is overlaid onto Mail’s app icon to indicate the number of unread messages. The number changes each time Mail retrieves more messages.

When the user holds the Control key down and clicks on a Dock tile, a menu appears. If your app does nothing to customize the menu, the Dock tile’s menu contains a list of the app’s open documents (if any), followed by the standard menu items Keep in Dock, Open at Login, Show in Finder, Hide, and Quit. You can add other menu items to the Dock menu, either statically by providing a custom menu nib file, or dynamically by overriding the application delegate’s `[applicationDockMenu:](https://developer.apple.com/documentation/appkit/nsapplicationdelegate/1428564-applicationdockmenu)` method.

You can also customize a dock tile when your app is not currently running by creating a Dock tile plug-in that can update the Dock tile icon and menu. For example, you may want to update the badge text to indicate that new content will be available the next time the app is launched, and you may want to customize the app’s Dock menu to deal with the new content.

For information explaining how to customize your app’s Dock icon and menu, see *Dock Tile Programming Guide*.

        
            [Next](../BuildTimeConfiguration/BuildTimeConfiguration.html)[Previous](../FullScreenApp/FullScreenApp.html)
        
         Copyright &#x00a9; 2015 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2015-03-09