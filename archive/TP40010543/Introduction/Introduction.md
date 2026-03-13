---
title: "Introduction"
book: "Mac App Programming Guide"
chapterId: "TP40010543-CH1"
date: "2015-03-09"
description: "Introduces the development process for Mac apps."
identifier: "//apple_ref/doc/uid/TP40010543"
platforms: "OS X"
source: apple-archive
---

# Introduction

> **Mac App Programming Guide**
> Last updated: 2015-03-09

[Next](../AppRuntime/AppRuntime.html)
        
        
        [](../index.html)
        
        # About OS X App Design
This document is the starting point for learning how to create Mac apps. It contains fundamental information about the OS X environment and how your apps interact with that environment. It also contains important information about the architecture of Mac apps and tips for designing key parts of your app.

## At a Glance
Cocoa is the application environment that unlocks the full power of OS X. Cocoa provides APIs, libraries, and runtimes that help you create fast, exciting apps that automatically inherit the beautiful look and feel of OS X, as well as standard behaviors users expect.

### Cocoa Helps You Create Great Apps for OS X
You write apps for OS X using Cocoa, which provides a significant amount of infrastructure for your program. Fundamental design patterns are used throughout Cocoa to enable your app to interface seamlessly with subsystem frameworks, and core application objects provide key behaviors to support simplicity and extensibility in app architecture. Key parts of the Cocoa environment are designed particularly to support ease of use, one of the most important aspects of successful Mac apps. Many apps should adopt iCloud to provide a more coherent user experience by eliminating the need to synchronize data explicitly between devices.

> **Relevant Chapters:** [The Mac Application Environment](../AppRuntime/AppRuntime.html#//apple_ref/doc/uid/TP40010543-CH2-SW1), [The Core App Design](../CoreAppDesign/CoreAppDesign.html#//apple_ref/doc/uid/TP40010543-CH3-SW1), and [Integrating iCloud Support Into Your App](../CoreAppDesign/CoreAppDesign.html#//apple_ref/doc/uid/TP40010543-CH3-SW19)

### Common Behaviors Make Apps Complete
During the design phase of creating your app, you need to think about how to implement certain features that users expect in well-formed Mac apps. Integrating these features into your app architecture can have an impact on the user experience: accessibility, preferences, Spotlight, services, resolution independence, fast user switching, and the Dock. Enabling your app to assume full-screen mode, taking over the entire screen, provides users with a more immersive, cinematic experience and enables them to concentrate fully on their content without distractions.

> **Relevant Chapters:** [Supporting Common App Behaviors](../CommonAppBehaviors/CommonAppBehaviors.html#//apple_ref/doc/uid/TP40010543-CH7-SW1) and [Implementing the Full-Screen Experience](../FullScreenApp/FullScreenApp.html#//apple_ref/doc/uid/TP40010543-CH6-SW1)

### Get It Right: Meet System and App Store Requirements
Configuring your app properly is an important part of the development process. Mac apps use a structured directory called a *bundle* to manage their code and resource files. And although most of the files are custom and exist to support your app, some are required by the system or the App Store and must be configured properly. The application bundle also contains the resources you need to provide to internationalize your app to support multiple languages.

> **Relevant Chapter:** [Build-Time Configuration Details](../BuildTimeConfiguration/BuildTimeConfiguration.html#//apple_ref/doc/uid/TP40010543-CH8-SW1)

### Finish Your App with Performance Tuning
As you develop your app and your project code stabilizes, you can begin performance tuning. Of course, you want your app to launch and respond to the user’s commands as quickly as possible. A responsive app fits easily into the user’s workflow and gives an impression of being well crafted. You can improve the performance of your app by speeding up launch time and decreasing your app’s code footprint.

> **Relevant Chapter:** [Tuning for Performance and Responsiveness](../Performance/Performance.html#//apple_ref/doc/uid/TP40010543-CH9-SW1)

## How to Use This Document
This guide introduces you to the most important technologies that go into writing an app. In this guide you will see the whole landscape of what's needed to write one. That is, this guide shows you all the "pieces" you need and how they fit together. There are important aspects of app design that this guide does not cover, such as user interface design. However, this guide includes many links to other documents that provide details about the technologies it introduces, as well as links to tutorials that provide a hands-on approach.

In addition, this guide emphasizes certain technologies introduced in OS X v10.7, which provide essential capabilities that set your app apart from older ones and give it remarkable ease of use, bringing some of the best features from iOS to OS X.

## See Also
The following documents provide additional information about designing Mac apps, as well as more details about topics covered in this document:

To work through a tutorial showing you how to create a Cocoa app, see *[Start Developing Mac Apps Today](../../../../../referencelibrary/GettingStarted/RoadMapOSX/index.html#//apple_ref/doc/uid/TP40012262)*.

For information about user interface design enabling you to create effective apps using OS X, see *OS X Human Interface Guidelines*.

To understand how to create an explicit app ID, create provisioning profiles, and enable the correct entitlements for your application, so you can sell your application through the Mac App Store or use iCloud storage, see *App Distribution Guide*.

For a general survey of OS X technologies, see *[Mac Technology Overview](../../../../MacOSX/Conceptual/OSX_Technology_Overview/About/About.html#//apple_ref/doc/uid/TP40001067)*.

To understand how to implement a document-based app, see *[Document-Based App Programming Guide for Mac](../../../../DataManagement/Conceptual/DocBasedAppProgrammingGuideForOSX/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011179)*.

        
            [Next](../AppRuntime/AppRuntime.html)
        
         Copyright &#x00a9; 2015 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2015-03-09