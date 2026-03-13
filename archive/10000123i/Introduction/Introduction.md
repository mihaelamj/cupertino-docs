---
title: "Introduction"
book: "Bundle Programming Guide"
chapterId: "10000123i-CH1"
date: "2017-03-27"
description: "Explains how to use bundle objects to organize resources."
identifier: "//apple_ref/doc/uid/10000123i"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Introduction

> **Bundle Programming Guide**
> Last updated: 2017-03-27

[Next](../AboutBundles/AboutBundles.html)
        
        
        [](../index.html)
        
        # Introduction
Bundles are a fundamental technology in macOS and iOS that are used to encapsulate code and resources. Bundles simplify the developer experience by providing known locations for needed resources while alleviating the need to create compound binary files. Instead, bundles use directories and files to provide a more natural type of organization—one that can also be modified easily both during development and after deployment.

To support bundles, both Cocoa and Core Foundation provide programming interfaces for accessing the contents of bundles. Because bundles use an organized structure, it is important that all developers understand the fundamental organizing principles of bundles. This document provides you with the foundation for understanding how bundles work and for how you use them during development to access your resource files. 

## Organization of This Document
This document contains the following chapters:

[About Bundles](../AboutBundles/AboutBundles.html#//apple_ref/doc/uid/10000123i-CH100-SW1) introduces the concept of bundles and packages and how they are used by the system.

[Bundle Structures](../BundleTypes/BundleTypes.html#//apple_ref/doc/uid/10000123i-CH101-SW1) describes the structure and contents of the standard bundle types. 

[Accessing a Bundle's Contents](../AccessingaBundlesContents/AccessingaBundlesContents.html#//apple_ref/doc/uid/10000123i-CH104-SW1) shows you how to use the Cocoa and Core Foundation interfaces to get information about a bundle and its contents.

[Document Packages](../DocumentPackages/DocumentPackages.html#//apple_ref/doc/uid/10000123i-CH106-SW1) describes the notion of document packages (which are loosely related to bundles) and how you use them.

## See Also
Although the information in this document applies to all types of bundles, if you are working with more specialized types of bundles (such as [frameworks](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56) and plug-ins), you should also consult the following documents:

*[Framework Programming Guide](../../../../MacOSX/Conceptual/BPFrameworks/Frameworks.html#//apple_ref/doc/uid/10000183i)* provides detailed information about creating and using custom frameworks.

*[Code Loading Programming Topics](../../../../Cocoa/Conceptual/LoadingCode/LoadingCode.html#//apple_ref/doc/uid/10000052i)* provides information about writing plug-ins using the Objective-C language.

*[Plug-in Programming Topics](../../CFPlugIns/CFPlugIns.html#//apple_ref/doc/uid/10000128i)* provides information about writing plug-ins using the C language.

        
            [Next](../AboutBundles/AboutBundles.html)
        
         Copyright &#x00a9; 2017 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2017-03-27