---
title: "Introduction"
book: "Animation Types and Timing Programming Guide"
framework: "QuartzCore"
chapterId: "TP40006668"
date: "2010-05-18"
description: "Describes the animation and timing classes used by both Core Animation and Cocoa Animation proxies."
identifier: "//apple_ref/doc/uid/TP40006166"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Introduction

> **Animation Types and Timing Programming Guide**
> Last updated: 2010-05-18

[Next](../Articles/AnimationTimingTypesOverview.html)
        
        
        [](../index.html)
        
        # Introduction to Animation Types and Timing Programming Guide

> **Important:** This document is no longer being updated. For the latest information about Apple SDKs, visit the [documentation website](https://developer.apple.com/documentation).

This document describes the fundamental concepts involving the timing and animation classes used with Core Animation. Core Animation is an Objective-C framework that combines a high-performance compositing engine with a simple to use animation programming interface.

> **Note:** Animation is an inherently visual medium. The HTML version of *Animation Types and Timing Programming Guide* contains QuickTime movies (along with static images) that show example animations to help illustrate concepts. The PDF version contains only the static images.

You should read this document to gain an understanding of working with Core Animation in a Cocoa application. The Objective-C 2.0 Programming Language should be considered a prerequisite because Core Animation makes extensive use of Objective-C properties. You should also be familiar with key-value coding as described in Key-Value Coding Programming Guide. Familiarity with the Quartz 2D imaging technologies described in Quartz 2D Programming Guide is also helpful, although not required.

## Organization of This Document
*Animation Types and Timing* consists of the following articles:

[Animation Class Roadmap](../Articles/AnimationTimingTypesOverview.html#//apple_ref/doc/uid/TP40006669-SW1) provides an overview of the animation classes and timing protocol.

[Timing, Timespaces, and CAAnimation](../Articles/Timing.html#//apple_ref/doc/uid/TP40006670-SW1) describes in detail the timing model for Core Animation and the abstract `CAAnimation` class.

[Property-Based Animations](../Articles/PropertyAnimations.html#//apple_ref/doc/uid/TP40006672-SW1) describes the property-based animations: `CABasicAnimation` and `CAKeyframeAnimation`.

[Transition Animation](../Articles/TransitionAnimations.html#//apple_ref/doc/uid/TP40006674-SW1) describes the transition animation class, `CATransition`.

## See Also
These programming guides discuss some of the technologies that are used by Core Animation:

*[Animation Overview](../../../../GraphicsImaging/Conceptual/Animation_Overview/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004952)* describes the animation technologies available on OS X.

*[Core Animation Programming Guide](../../CoreAnimation_guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004514)* contains code fragments that demonstrate common Core Animation tasks.

*[Animation Programming Guide for Cocoa](../../AnimationGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40003592)* describes the animation capabilities available to Cocoa Applications.

        
            [Next](../Articles/AnimationTimingTypesOverview.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-05-18