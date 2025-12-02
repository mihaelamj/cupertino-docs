---
title: "Timing, Timespaces, and CAAnimation"
book: "Animation Types and Timing Programming Guide"
framework: "QuartzCore"
chapterId: "TP40006670"
date: "2010-05-18"
description: "Describes the animation and timing classes used by both Core Animation and Cocoa Animation proxies."
identifier: "//apple_ref/doc/uid/TP40006166"
source: apple-archive
---

# Timing, Timespaces, and CAAnimation

> **Animation Types and Timing Programming Guide**
> Last updated: 2010-05-18

[Next](PropertyAnimations.html)[Previous](AnimationTimingTypesOverview.html)
        
        
        [](../index.html)
        
        # Timing, Timespaces, and CAAnimation
When broken down to its simplest definition an animation is simply the varying of a value over a time. Core Animation provides base timing functionality for both animations and layers, providing a powerful timeline capability.

This chapter discusses the timing protocol and the basic animation support common to all animation subclasses.

## Media Timing Protocol
The Core Animation timing model is described by the `CAMediaTiming` protocol and adopted by the `CAAnimation` class and its subclasses. The timing model specifies the time offset, duration, speed, and repeating behavior of an animation. 

The `CAMediaTiming` protocol is also adopted by the `CALayer` class, allowing a layer to define a timespace relative to its superlayer; similar manner to describing a relative coordinate space. This concept of a layer-tree timespace provides a scalable timeline that starts at the root layer, through its descendants. Since an animation must be associated with a layer to be displayed, the animation's timing is scaled to the timespace defined by the layer. 

The `speed` property of an animation or layer specifies this scaling factor. For example, a 10 second animation that is attached to a layer with a timespace that has a speed value of 2 will take 5 seconds to display (twice the speed). If a sublayer of that layer also defines a speed factor of 2, then its animations will display in 1/4 the time (the speed of the superlayer * the speed of the sublayer).

Similarly, a layer's timespace can also affect the playback of dynamic layer media such as Quartz Composer compositions. Doubling the speed of a `QCCompositionLayer` causes the composition to play twice as fast, as well as doubling the speed of any animations attached to that layer. Again, this effect is hierarchical, so any sublayers of the `QCCompositionLayers` will also display their content using the increased speed.

The `duration` property of the `CAMediaTiming` protocol is used by animations to define how long, in seconds, a single iteration of an animation will take to display. The default duration for subclasses of `CAAnimation` is 0 seconds, which indicates that the animation should use the duration specified by the transaction in which it is run, or .25 seconds if no transaction duration is specified. 

The timing protocol provides the means of starting an animation a certain number of seconds into its duration using two properties: `beginTime` and `timeOffset`. The `beginTime` specifies the number of seconds into the duration the animation should start and is scaled to the timespace of the animation's layer. The `timeOffset` specifies an additional offset, but is stated in the local active time. Both values are combined to determine the final starting offset.

### Repeating Animations
The repetition behavior of an animation is also determined by the `CAMediaTiming` protocol by the `repeatCount` and `repeatDuration` properties. The `repeatCount` specifies the number of times the animation should repeat and can be a fractional number. Setting the `repeatCount` to a value of 2.5 for a 10 second animation would cause the animation to run for a total of 25 seconds, ending half way through its third iteration. Setting the repeatCount to `1e100f` will cause the animation to repeat until it is removed from the layer.

The `repeatDuration` is similar to the `repeatCount`, although it is specified in seconds rather than iterations. The `repeatDuration` can also be a fractional value.

The `autoreverses` property of an animation determines whether the animation plays backwards after it finishes playing forwards; assuming that multiple repetitions are specified.

### Fill Mode
The `fillMode` property of the timing protocol defines how an animation will be displayed outside of its active duration. The animation can be frozen at its starting position, at its ending position, both, or removed entirely from display. The default behavior is to remove the animation from display when it has completed.
## Animation Pacing
The pacing of an animation determines how the interpolated values are distributed over the duration of the animation. Using the appropriate pacing for a particular visual effect can greatly enhance its affect on the user.

The pacing of an animation is represented by a timing function that is expressed as a cubic Bezier curve. This function maps the duration of a single cycle of the animation (normalized to the range [0.0,1.0]) to the output time (also normalized to that range).

The timingFunction property of the `CAAnimation` class specifies an instance of the `CAMediaTimingFunction`, the class responsible for encapsulating the timing functionality.

`CAMediaTimingFunction` provides two options for specifying the mapping function: constants for the common pacing curves, and methods for creating custom functions by specifying two control points. 

The predefined timing functions are returned by specifying one of the following constants to the `CAMediaTimingFunction` class method `functionWithName:`:

`kCAMediaTimingFunctionLinear` specifies linear pacing. A linear pacing causes an animation to occur evenly over its duration.

`kCAMediaTimingFunctionEaseIn` specifies ease-in pacing. Ease-in pacing causes the animation to begin slowly, and then speed up as it progresses.

`kCAMediaTimingFunctionEaseOut` specifies ease-out pacing. An ease-out pacing causes the animation to begin quickly, and then slow as it completes.

`kCAMediaTimingFunctionEaseInEaseOut` specifies ease-in ease-out pacing. An ease-in ease-out animation begins slowly, accelerates through the middle of its duration, and then slows again before completing.

Figure 1 shows the predefined timing functions as their cubic Bezier timing curves.

**Figure 1**  Cubic Bezier curve representations of the predefined timing functionsCustom timing functions are created using the `functionWithControlPoints::::` class method or the `initWithControlPoints::::` instance method. The end points of the Bezier curve are automatically set to (0.0,0.0) and (1.0,1.0). and the creation methods expect the `c1x`, `c1y`, `c2x`, and `c2y` as the parameters. The resulting control points defining the bezier curve are: `[(0.0,0.0), (c1x,c1y), (c2x,c2y), (1.0,1.0)]`.

Listing 1 shows an example code fragment that creates a custom timing function using the points `[(0.0,0.0), (0.25,0.1), (0.25,0.1), (1.0,1.0)]`.

**Listing 1**  Custom CAMediaTimingFunction code fragment

CAMediaTimingFunction *customTimingFunction;
```
customTimingFunction=[CAMediaTimingFunction functionWithControlPoints:0.25f :0.1f :0.25f :1.0f];
```

> **Note:** Keyframe animation requires a more nuanced pacing and timing model than can be provided by a single instance of `CAMediaTimingFunction`. See [Keyframe Timing and Pacing Extensions](PropertyAnimations.html#//apple_ref/doc/uid/TP40006672-SW7) for more information.

## Animation Delegates
The `CAAnimation` class provides the means to notify a delegate object when an animation starts and stops.

If an animation has a delegate specified it will receive `animationDidStart:` message, passing the animation instance that began. When an animation stops the delegate receives an `animationDidStop:finished:` message, passing the animation instance that stopped and a Boolean indicating whether the animation completed its duration successfully or was stopped manually.

> **Important:** The `CAAnimation` delegate object is retained by the receiver. This is a rare exception to the memory management rules described in *[Advanced Memory Management Programming Guide](../../MemoryMgmt/Articles/MemoryMgmt.html#//apple_ref/doc/uid/10000011i)*.

        
            [Next](PropertyAnimations.html)[Previous](AnimationTimingTypesOverview.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-05-18