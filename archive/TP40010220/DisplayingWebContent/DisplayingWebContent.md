---
title: "Displaying Web and Multimedia Content"
book: "Networking Overview"
chapterId: "TP40012545"
date: "2017-03-27"
description: "Explains basic networking concepts and terminology, and provides an overview of networking APIs."
identifier: "//apple_ref/doc/uid/TP40010220"
source: apple-archive
---

# Displaying Web and Multimedia Content

> **Networking Overview**
> Last updated: 2017-03-27

[Next](../WorkingWithHTTPAndHTTPSRequests/WorkingWithHTTPAndHTTPSRequests.html)[Previous](../Discovering,Browsing,AndAdvertisingNetworkServices/Discovering,Browsing,AndAdvertisingNetworkServices.html)
        
        
        [](../index.html)
        
        # Displaying Web and Multimedia Content
OS X and iOS provide an assortment of APIs to allow you to display web content and streaming multimedia content. In general, if these higher-level multimedia- and web-specific APIs meet your needs, you should use them rather than using networking APIs directly. The sections below briefly summarize these APIs.

## Opening Web Content or Streaming Media in the Default Application
To open a webpage or streaming URL in the user’s default browser or media viewer:

In iOS, use the `[openURL:](https://developer.apple.com/documentation/uikit/uiapplication/1622961-openurl)` method of the `[UIApplication](https://developer.apple.com/documentation/uikit/uiapplication)` class.

For a real-world example, see QA1629: *[Launching the App Store from an iOS application](../../../../../qa/qa1629/_index.html#//apple_ref/doc/uid/DTS40008173)*.

In OS X, use the `[LSOpenCFURLRef](https://developer.apple.com/documentation/coreservices/1442850-lsopencfurlref)` or `[LSOpenFromURLSpec](https://developer.apple.com/documentation/coreservices/1441986-lsopenfromurlspec)` functions in the Launch Services API.

For details, see [Launch Services Tasks](../../../../Carbon/Conceptual/LaunchServicesConcepts/LSCTasks/LSCTasks.html#//apple_ref/doc/uid/TP30000999-CH203) in *[Launch Services Programming Guide](../../../../Carbon/Conceptual/LaunchServicesConcepts/LSCIntro/LSCIntro.html#//apple_ref/doc/uid/TP30000999)*.

## Displaying Web Content in Your Application
OS X and iOS provide an easy way to load and display a webpage with the WebKit engine, the same rendering engine used by Safari.

In OS X, you load web content with the `[WebView](https://developer.apple.com/documentation/webkit/webview)` class. You can add a web view by including it in your application’s nib file or by programmatically constructing a `WebView` object and calling the `[initWithFrame:frameName:groupName:](https://developer.apple.com/documentation/webkit/webview/1408359-initwithframe)` method. Load content by calling the `[loadRequest:](https://developer.apple.com/documentation/webkit/webframe/1494250-load)` method on the web view’s main frame (which you can obtain with the `[mainFrame](https://developer.apple.com/documentation/webkit/webview/1408470-mainframe)` method).

In iOS, you load web content with the `[loadRequest:](https://developer.apple.com/documentation/uikit/uiwebview/1617957-loadrequest)` method of the `[UIWebView](https://developer.apple.com/documentation/uikit/uiwebview)` class. You can add a web view by including it in your application’s nib file or by programmatically creating a `UIWebView` object and initializing it with the `[initWithFrame:](https://developer.apple.com/documentation/uikit/uiview/1622488-init)` method.

> **Note:** Web views in iOS don’t provide access to their underlying connection when they load data, which means a connection that can’t be resolved automatically (such as a connection that requires authentication) fails.

For more information, see [Simple Browsing](../../../../Cocoa/Conceptual/DisplayWebContent/Tasks/SimpleBrowsing.html#//apple_ref/doc/uid/20002025) in *[WebKit Objective-C Programming Guide](../../../../Cocoa/Conceptual/DisplayWebContent/DisplayWebContent.html#//apple_ref/doc/uid/10000164i)* (OS X) and *[UIWebView Class Reference](https://developer.apple.com/documentation/uikit/uiwebview)* (iOS).

## Displaying Streaming Multimedia Content in Your Application
There are several frameworks available for displaying streaming multimedia content in OS X and iOS:

In OS X, use the QTKit Framework for basic playback or the AV Foundation framework for more complex functionality.

In iOS, use the Media Player Framework for basic playback or the AV Foundation framework for more complex functionality.

For more information, read *[Getting Started with Audio & Video](../../../../../referencelibrary/GettingStarted/GS_MusicAudio/_index.html#//apple_ref/doc/uid/TP30001095)*, *[Multimedia Programming Guide](../../../../AudioVideo/Conceptual/MultimediaPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009767)* (iOS), *[QTKit Application Programming Guide](../../../../Cocoa/Conceptual/QTKitApplicationProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008156)* (OS X), and *[AVFoundation Programming Guide](../../../../AudioVideo/Conceptual/AVFoundationPG/Articles/00_Introduction.html#//apple_ref/doc/uid/TP40010188)*.

        
            [Next](../WorkingWithHTTPAndHTTPSRequests/WorkingWithHTTPAndHTTPSRequests.html)[Previous](../Discovering,Browsing,AndAdvertisingNetworkServices/Discovering,Browsing,AndAdvertisingNetworkServices.html)
        
         Copyright &#x00a9; 2004, 2017 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2017-03-27