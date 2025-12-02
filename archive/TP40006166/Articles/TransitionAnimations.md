---
title: "Transition Animation"
book: "Animation Types and Timing Programming Guide"
framework: "QuartzCore"
chapterId: "TP40006674"
date: "2010-05-18"
description: "Describes the animation and timing classes used by both Core Animation and Cocoa Animation proxies."
identifier: "//apple_ref/doc/uid/TP40006166"
source: apple-archive
---

# Transition Animation

> **Animation Types and Timing Programming Guide**
> Last updated: 2010-05-18

[Next](../RevisionHistory.html)[Previous](PropertyAnimations.html)
        
        
        [](../index.html)
        
        # Transition Animation
Transition animation is used when it is impossible to mathematically interpolate the effect of changing the value of a layer property, or the state of a layer in the layer tree.

This chapter discusses the transition animation functionality provided by Core Animation.

## Creating a Transition Animation
The `CATransition` class provides transition functionality to Core Animation. It is a direct subclass of `CAAnimation` as it affects an entire layer, rather than a specific property of a layer.

A new instance of `CATransition` is created using the inherited class method `animation`. This will create a transition animation with the default values shown in Table 1:

**Table 1**  Default `CATransition` property valuesTransition Property

Value

`type`

Uses a fade transition. The value is `[kCATransitionFade](https://developer.apple.com/documentation/quartzcore/kcatransitionfade)`.

`subType`

Not applicable.

`duration`

Uses the duration of the current transaction or 0.25 seconds if the duration has not been set for a transaction. The value is 0.0

`timingFunction`

Uses linear pacing. The value is `nil`.

`startProgress`

0.0

`endProgress`

1.0

## Configuring a Transition
Once created, you can configure the transition animation using one of the predefined transition types or, on OS X, create a custom transition using a Core Image filter.

The predefined transitions are used by setting the `type` property to one of the constants in Table 2.

**Table 2**  Common transition typesTransition Type

Description

`[kCATransitionFade](https://developer.apple.com/documentation/quartzcore/kcatransitionfade)`

The layer fades as it becomes visible or hidden.

`[kCATransitionMoveIn](https://developer.apple.com/documentation/quartzcore/catransitiontype/1412487-movein)`

The layer slides into place over any existing content.

`[kCATransitionPush](https://developer.apple.com/documentation/quartzcore/catransitiontype/1412528-push)`

The layer pushes any existing content as it slides into place

`[kCATransitionReveal](https://developer.apple.com/documentation/quartzcore/catransitiontype/1412489-reveal)`

The layer is gradually revealed in the direction specified by the transition subtype.

With the exception of `kCATransitionFade`, the predefined transition types also allow you to specify a direction for the transition by setting the `subType` property to one of the constants in Table 3.

**Table 3**  Common transition subtypesTransition Subtype Constant

Description

`[kCATransitionFromRight](https://developer.apple.com/documentation/quartzcore/kcatransitionfromright)`

The transition begins at the right side of the layer.

`[kCATransitionFromLeft](https://developer.apple.com/documentation/quartzcore/catransitionsubtype/1412459-fromleft)`

The transition begins at the left side of the layer.

`[kCATransitionFromTop](https://developer.apple.com/documentation/quartzcore/kcatransitionfromtop)`

The transition begins at the top of the layer.

`[kCATransitionFromBottom](https://developer.apple.com/documentation/quartzcore/kcatransitionfrombottom)`

The transition begins at the bottom of the layer.

The `startProgress` property allows you to change the start point of the transition by setting a value that represents a fraction of the entire animation. For example, to start a transition half way through its progress the `startProgress` value would be set to 0.5. Similarly, you can specify the `endProgress` value for the transition. The `endProgress` is the fraction of the entire transition that the transition should stop at. The default values are 0.0 and 1.0, respectively.

If the predefined transitions don’t provide the desired visual effect and your application is targeted to OS X rather than iOS, you can specify a custom Core Image filter object that is used to display the transition. A custom  filter must support both the `kCIInputImageKey` and the `kCIInputTargetImageKey` input keys, and the `kCIOutputImageKey` output key. The filter may optionally support the `kCIInputExtentKey` input key, which is set to a rectangle describing the region in which the transition should run.

        
            [Next](../RevisionHistory.html)[Previous](PropertyAnimations.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-05-18