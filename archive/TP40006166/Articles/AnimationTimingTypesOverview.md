---
title: "Animation Class Roadmap"
book: "Animation Types and Timing Programming Guide"
chapterId: "TP40006669"
date: "2010-05-18"
description: "Describes the animation and timing classes used by both Core Animation and Cocoa Animation proxies."
identifier: "//apple_ref/doc/uid/TP40006166"
source: apple-archive
---

# Animation Class Roadmap

> **Animation Types and Timing Programming Guide**
> Last updated: 2010-05-18

[Next](Timing.html)[Previous](../Introduction/Introduction.html)
        
        
        [](../index.html)
        
        # Animation Class Roadmap
Core Animation provides an expressive set of animation classes you can use in your application:

`CAAnimation` is the abstract class that all animations subclass. `CAAnimation` adopts the `CAMediaTiming` protocol which provides the simple duration, speed, and repeat count for an animation. `CAAnimation` also adopts the `CAAction` protocol. This protocol provides a standardized means for starting an animation in response to an action triggered by a layer.

The `CAAnimation` class also defines an animation’s timing as an instance of `CAMediaTimingFunction`. The timing function describes the pacing of the animation as a simple Bezier curve. A linear timing function specifies that the animation's pace is even across its duration, while an ease-in timing function causes an animation to speed up as it nears its duration.

`CAPropertyAnimation` is an abstract subclass of `CAAnimation` that provides support for animating a layer property specified by a key path.

`CABasicAnimation` is a subclass of `CAPropertyAnimation` that provides simple interpolation for a layer property.

`CAKeyframeAnimation` (a subclass of `CAPropertyAnimation`) provides support for key frame animation. You specify the key path of the layer property to be animated, an array of values that represent the value at each stage of the animation, as well as arrays of key frame times and timing functions. As the animation runs, each value is set in turn using the specified interpolation.

`CATransition` provides a transition effect that affects the entire layer's content. It fades, pushes, or reveals layer content when animating. On OS X, the stock transition effects can be extended by providing your own custom Core Image filters.

`CAAnimationGroup` allows an array of animation objects to be grouped together and run concurrently.

 Figure 1 shows the animation class hierarchy, and also summarizes the properties available through inheritance.

**Figure 1**  Core Animation classes and protocol
        
            [Next](Timing.html)[Previous](../Introduction/Introduction.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-05-18