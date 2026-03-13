---
title: "Implementing the Full-Screen Experience"
book: "Mac App Programming Guide"
chapterId: "TP40010543-CH6"
date: "2015-03-09"
description: "Introduces the development process for Mac apps."
identifier: "//apple_ref/doc/uid/TP40010543"
source: apple-archive
---

# Implementing the Full-Screen Experience

> **Mac App Programming Guide**
> Last updated: 2015-03-09

[Next](../CommonAppBehaviors/CommonAppBehaviors.html)[Previous](../CoreAppDesign/CoreAppDesign.html)
        
        
        [](../index.html)
        
        # Implementing the Full-Screen Experience
Enabling a window of your app to assume full-screen mode, taking over the entire screen, provides users with a more immersive, cinematic experience. Full-screen appearance can be striking and can make your app stand out. From a practical standpoint, full-screen mode presents a better view of users’ data, enabling them to concentrate fully on their content without the distractions of other apps or the desktop.

In full-screen mode, by default, the menu bar and Dock are autohidden; that is, they are normally hidden but reappear when the user moves the pointer to the top or bottom of the screen. A full-screen window does not draw its titlebar and may have special handling for its toolbar.

The full-screen experience makes sense for many apps but not for all. For example, the Finder, Address Book, and Calculator would not provide any benefit to users by assuming full-screen mode. The same is probably true for most utility apps. Media-rich apps, on the other hand, can often benefit from full-screen presentation.

Beginning with OS X v10.7, Cocoa includes support for full-screen mode through APIs in `NSApplication`, `NSWindow`, and `NSWindowDelegate` protocol. When the user chooses to enter full-screen mode, Cocoa dynamically creates a space and puts the window into that space. This behavior enables the user to have one window of an app running in full-screen mode in one space, while using other windows of that app, as well as other apps, on the desktop in other spaces. While in full-screen mode, the user can switch between windows in the current app or switch apps.

Apps that have implemented full-screen user interfaces in previous versions of OS X should consider standardizing on the full-screen APIs in OS X v10.7.

## Full-Screen API in NSApplication
Full-screen support in `NSApplication` is provided by the presentation option `NSApplicationPresentationFullScreen`. You can find the current presentation mode via the `NSApplication` method `[currentSystemPresentationOptions](https://developer.apple.com/documentation/appkit/nsapplication/1428717-currentsystempresentationoptions)`, which is also key-value observable. You can set the presentation options using the `NSApplication` method `[setPresentationOptions:](https://developer.apple.com/documentation/appkit/nsapplication/1428664-presentationoptions)`. (Be sure to observe the restrictions on presentation option combinations documented with `NSApplicationPresentationOptions`, and set the presentation options in a try-catch block to ensure that your program does not crash from an invalid combination of options.)

A window delegate may also specify that the window toolbar be removed from the window in full-screen mode and be shown automatically with the menu bar by including `NSApplicationPresentationAutoHideToolbar` in the presentation options returned from the `[window:willUseFullScreenPresentationOptions:](https://developer.apple.com/documentation/appkit/nswindowdelegate/1419144-window)` method of `NSWindowDelegate`.

## Full-Screen API in NSWindow

The app must specify whether a given window can enter full-screen mode. Apps can set collection behavior using the `[setCollectionBehavior:](https://developer.apple.com/documentation/appkit/nswindow/1419471-collectionbehavior)` method by passing in various constants, and the current options may be accessed via the `[collectionBehavior](https://developer.apple.com/documentation/appkit/nswindow/1419471-collectionbehavior)` method. You can choose between two constants to override the window collection behavior, as shown in the following table:

Constant

Behavior

`NSWindowCollectionBehaviorFullScreenPrimary`

The frontmost window with this collection behavior
becomes the full-screen window. A window with this collection behavior has a full-screen button in the upper right of its titlebar.`NSWindowCollectionBehaviorFullScreenAuxiliary`

Windows with this collection behavior can be shown in the same space with the full-screen window.

When a window goes into full-screen mode, the `styleMask` changes to `NSFullScreenWindowMask` to reflect the state of the window.The setting of the `styleMask` goes through the `setStyleMask:` method. As a result, a window can override this method if it has customization to do when entering or exiting full-screen. 

A window can be taken into or out of full-screen mode using the `[toggleFullScreen:](https://developer.apple.com/documentation/appkit/nswindow/1419527-togglefullscreen)` method. If an app supports full-screen mode, it should add a menu item to the View menu with `toggleFullScreen:` as the action, and `nil` as the target.

## Full-Screen API in NSWindowDelegate Protocol
The following notifications are sent before and after the window enters and exits full-screen mode:

NSWindowWillEnterFullScreenNotification
```
NSWindowDidEnterFullScreenNotification
```

```
NSWindowWillExitFullScreenNotification
```

```
NSWindowDidExitFullScreenNotification
```
The window delegate has the following corresponding window delegate notification methods:

`[windowWillEnterFullScreen:](https://developer.apple.com/documentation/appkit/nswindowdelegate/1419563-windowwillenterfullscreen)`

`[windowDidEnterFullScreen:](https://developer.apple.com/documentation/appkit/nswindowdelegate/1419116-windowdidenterfullscreen)`

`[windowWillExitFullScreen:](https://developer.apple.com/documentation/appkit/nswindowdelegate/1419332-windowwillexitfullscreen)`

`[windowDidExitFullScreen:](https://developer.apple.com/documentation/appkit/nswindowdelegate/1419146-windowdidexitfullscreen)`

The `NSWindowDelegate` protocol methods supporting full-screen mode are listed in Table 3-1.

**Table 3-1**  Window delegate methods supporting full-screen modeMethod

Description

`[window:willUseFullScreenContentSize:](https://developer.apple.com/documentation/appkit/nswindowdelegate/1419282-window)`

Invoked to allow the delegate to modify the full-screen content size.

`[window:willUseFullScreenPresentationOptions:](https://developer.apple.com/documentation/appkit/nswindowdelegate/1419144-window)`

Returns the presentation options the window will use when transitioning to full-screen mode.

`[customWindowsToEnterFullScreenForWindow:](https://developer.apple.com/documentation/appkit/nswindowdelegate/1419521-customwindowstoenterfullscreen)`

Invoked when the window is about to enter full-screen mode. The window delegate can implement this method to customize the animation by returning a custom window or array of windows containing layers or other effects.

`[window:startCustomAnimationToEnterFullScreenWithDuration:](https://developer.apple.com/documentation/appkit/nswindowdelegate/1419406-window)`

The system has started its animation into full-screen mode, including transitioning into a new space. You can implement this method to perform custom animation with the given duration to be in sync with the system animation.

`[windowDidFailToEnterFullScreen:](https://developer.apple.com/documentation/appkit/nswindowdelegate/1419591-windowdidfailtoenterfullscreen)`

Invoked if the window failed to enter full-screen mode.

`[customWindowsToExitFullScreenForWindow:](https://developer.apple.com/documentation/appkit/nswindowdelegate/1419070-customwindowstoexitfullscreenfor)`

Invoked when the window is about to exit full-screen mode. The window delegate can implement this method to customize the animation when the window is about to exit full-screen by returning a custom window or array of windows containing layers or other effects.

`[window:startCustomAnimationToExitFullScreenWithDuration:](https://developer.apple.com/documentation/appkit/nswindowdelegate/1419705-window)`

The system has started its animation out of full-screen, including transitioning back to the desktop space. You can implement this method to perform custom animation with the given duration to be in sync with the system animation.

`[windowDidFailToExitFullScreen:](https://developer.apple.com/documentation/appkit/nswindowdelegate/1419573-windowdidfailtoexitfullscreen)`

Invoked if the window failed to exit full-screen mode.

For more information about full-screen mode, see *[NSWindowDelegate Protocol Reference](https://developer.apple.com/documentation/appkit/nswindowdelegate)* and the *OS X Human Interface Guidelines*.

        
            [Next](../CommonAppBehaviors/CommonAppBehaviors.html)[Previous](../CoreAppDesign/CoreAppDesign.html)
        
         Copyright &#x00a9; 2015 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2015-03-09