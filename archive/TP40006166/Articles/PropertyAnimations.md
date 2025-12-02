---
title: "Property-Based Animations"
book: "Animation Types and Timing Programming Guide"
framework: "QuartzCore"
chapterId: "TP40006672"
date: "2010-05-18"
description: "Describes the animation and timing classes used by both Core Animation and Cocoa Animation proxies."
identifier: "//apple_ref/doc/uid/TP40006166"
source: apple-archive
---

# Property-Based Animations

> **Animation Types and Timing Programming Guide**
> Last updated: 2010-05-18

[Next](TransitionAnimations.html)[Previous](Timing.html)
        
        
        [](../index.html)
        
        # Property-Based Animations
Property-based animations are animations that interpolate values of a single attribute of a layer, for example, the position, background color, bounds, etc.

This chapter discusses how Core Animation abstracts property animation and the classes it provides to support basic and multiple keyframe animation of layer properties.

## Property-Based Abstraction
The `CAPropertyAnimation` class is the abstract subclass of `CAAnimation` that provides the base functionality for animating a specific property of a layer. Property-based animations are supported for all value types that can be mathematically interpolated, including:

integers and doubles

`CGRect`, `CGPoint`, `CGSize`, and `CGAffineTransform` structures

`CATransform3D` data structures

`CGColor` and `CGImage` references

As with all animations, a property animation must be associated with a layer. The property that is to be animated is specified using a key-value coding key path relative to the layer. For example, to animate the x component of the `position` property of “layerA” you’d create an animation using the key path “position.x” and add that animation to “layerA”.

You will never need to instantiate an instance of `CAPropertyAnimation` directly. Instead you would create an instance of one of its subclasses, `CABasicAnimation` or `CAKeyframeAnimation`. Likewise, you would never subclass `CAPropertyAnimation`, instead you would subclass `CABasicAnimation` or `CAKeyframeAnimation` to add functionality or store additional properties.

## Basic Animations
The `CABasicAnimation` class provides basic, single-keyframe animation capabilities for a layer property. You create an instance of  `CABasicAnimation` using the inherited `[animationWithKeyPath:](https://developer.apple.com/documentation/quartzcore/capropertyanimation/1412534-init)` method, specifying the key path of the layer property to be animated. Animatable Properties in *[Core Animation Programming Guide](../../CoreAnimation_guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004514)* summarize the animatable properties for `CALayer` and its filter properties.

Figure 1 shows a 3-second animation of a layer’s position property from (74.0,74.0) to a final position of (566.0,406.0). The parent layer is assumed to have a bounds of (0.0,0.0,640.0,480.0). 

**Figure 1**  3 second basic animation of a layer’s position property### Configuring a Basic Animation’s Interpolation Values
The `[fromValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue)`, `[byValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412445-byvalue)` and `[toValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue)` properties of the `CABasicAnimation` class define the values being interpolated between. All are optional, and no more than two should be non-`nil`. The object type that the property is set to should match the type of the property being animated.

The interpolation values are used as follows:

Both `[fromValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue)` and `[toValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue)` are non-`nil`. Interpolates between `[fromValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue)` and `[toValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue)`.

`[fromValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue)` and `[byValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412445-byvalue)` are non-`nil`. Interpolates between `[fromValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue)` and (`[fromValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue)` + `[byValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412445-byvalue)`).

`[byValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412445-byvalue)` and `[toValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue)` are non-`nil`. Interpolates between (`[toValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue)` - `[byValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412445-byvalue)`) and `[toValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue)`.

`[fromValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue)` is non-`nil`. Interpolates between `[fromValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412519-fromvalue)` and the current presentation value of the property.

`[toValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue)` is non-`nil`. Interpolates between the current value of `keyPath` in the target layer’s presentation layer and `[toValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412523-tovalue)`.

`[byValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412445-byvalue)` is non-`nil`. Interpolates between the current value of `keyPath` in the target layer’s presentation layer and that value plus `[byValue](https://developer.apple.com/documentation/quartzcore/cabasicanimation/1412445-byvalue)`.

All properties are `nil`. Interpolates between the previous value of `keyPath` in the target layer’s presentation layer and the current value of  `keyPath` in the target layer’s presentation layer.

> **Note:** On OS X the values passed to these properties are `NSPoint` structures. However, on iOS the types are `CGPoints`. Aside from that small difference, their use is identical.

### An Example Basic Animation
Listing 1 shows a code fragment that implements an explicit animation equivalent to the animation in Figure 1. 

**Listing 1**  CABasicAnimation code fragment

CABasicAnimation *theAnimation;
```
 
```

```
// create the animation object, specifying the position property as the key path
```

```
// the key path is relative to the target animation object (in this case a CALayer)
```

```
theAnimation=[CABasicAnimation animationWithKeyPath:@"position"];
```

```
 
```

```
// set the fromValue and toValue to the appropriate points
```

```
theAnimation.fromValue=[NSValue valueWithPoint:NSMakePoint(74.0,74.0)];
```

```
theAnimation.toValue=[NSValue valueWithPoint:NSMakePoint(566.0,406.0)];
```

```
 
```

```
// set the duration to 3.0 seconds
```

```
theAnimation.duration=3.0;
```

```
 
```

```
// set a custom timing function
```

```
theAnimation.timingFunction=[CAMediaTimingFunction functionWithControlPoints:0.25f :0.1f :0.25f :1.0f];
```

> **Note:** This example is for OS X. When compiling under iOS a small change is required. The `[NSMakePoint](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSMakePoint)` function is not available, instead use the `[CGPointMake](https://developer.apple.com/documentation/coregraphics/1455746-cgpointmake)` function. Aside from that direct substitution the code is identical.

## Keyframe Animations
Keyframe animation, supported in Core Animation by the `CAKeyframeAnimation` class, is similar to basic animation; however it allows you to specify an array of target values. Each of these target values is interpolated, in turn, over the duration of the animation.

Figure 2 shows a 5-second animation of a layer’s position property using a CGPathRef for the keyframe values.

**Figure 2**  5 second keyframe animation of a layer’s position property### Providing Keyframe Values
Keyframe values are specified using one of two properties: a Core Graphics path (the `path` property) or an array of objects (the `values` property).

A Core Graphics path is suitable for animating a layer’s `anchorPoint` or `position` properties, that is, properties that are `CGPoints`. Each point in the path, except for `moveto` points, defines a single keyframe segment for the purpose of timing and interpolation. If the `path` property is specified, the `values` property is ignored.

By default, as a layer is animated along a CGPath it maintains the rotation to which it has been set. Setting the `rotationMode` property to `[kCAAnimationRotateAuto](https://developer.apple.com/documentation/quartzcore/kcaanimationrotateauto)` or `[kCAAnimationRotateAutoReverse](https://developer.apple.com/documentation/quartzcore/caanimationrotationmode/1412471-rotateautoreverse)` causes the layer to rotate to match the path tangent.

Providing an array of objects to the `values` property allows you to animate any type of layer property. For example:

Provide an array of `CGImage` objects and set the animation key path to the `content` property of a layer. This causes the content of the layer to animate through the provided images.

Provide an array of `CGRects` (wrapped as objects) and set the animation key path to the `frame` property of a layer. This causes the frame of the layer to iterate through the provided rectangles.

Provide an array of `CATransform3D` matrices (again, wrapped as objects) and set the `animation` key path to the `transform` property. This causes each transform matrix to be applied to the layer’s `transform` property in turn.

### Keyframe Timing and Pacing Extensions
Keyframe animation requires a more complex timing and pacing model than that of a basic animation.

The inherited `timingFunction` property is ignored. Instead you can pass an optional array of `CAMediaTimingFunction` instances in the `timingFunctions` property. Each timing function describes the pacing of one keyframe to keyframe segment.

While the inherited duration property is valid for `CAKeyframeAnimation`, you can attain more subtle control of timing by using the `keyTimes` property.

The `keyTimes` property specifies an array of `NSNumber` objects that define the duration of each keyframe segment. Each value in the array is a floating point number between 0.0 and 1.0 and corresponds to one element in the `values` array. Each element in the `keyTimes` array defines the duration of the corresponding keyframe value as a fraction of the total duration of the animation. Each element value must be greater than, or equal to, the previous value.

The appropriate values for the `keyTimes` array are dependent on the `calculationMode` property.

If the `calculationMode` is set to `kCAAnimationLinear`, the first value in the array must be 0.0 and the last value must be 1.0. Values are interpolated between the specified keytimes.

If the `calculationMode` is set to `kCAAnimationDiscrete`, the first value in the array must be 0.0.

If the calculationMode is set to kCAAnimationPaced, the keyTimes array is ignored.

### An Example Keyframe Animation
Listing 2 shows a code fragment that implements an explicit animation equivalent to the animation in Figure 2. 

**Listing 2**  CAKeyframeAnimation code fragment

```
// create a CGPath that implements two arcs (a bounce)
```

```
CGMutablePathRef thePath = CGPathCreateMutable();
```

```
CGPathMoveToPoint(thePath,NULL,74.0,74.0);
```

```
CGPathAddCurveToPoint(thePath,NULL,74.0,500.0,
```

```
                                   320.0,500.0,
```

```
                                   320.0,74.0);
```

```
CGPathAddCurveToPoint(thePath,NULL,320.0,500.0,
```

```
                                   566.0,500.0,
```

```
                                   566.0,74.0);
```

```
 
```

```
CAKeyframeAnimation * theAnimation;
```

```
 
```

```
// create the animation object, specifying the position property as the key path
```

```
// the key path is relative to the target animation object (in this case a CALayer)
```

```
theAnimation=[CAKeyframeAnimation animationWithKeyPath:@"position"];
```

```
theAnimation.path=thePath;
```

```
 
```

```
// set the duration to 5.0 seconds
```

```
theAnimation.duration=5.0;
```

```
 
```

```
 
```

```
// release the path
```

```
CFRelease(thePath);
```

        
            [Next](TransitionAnimations.html)[Previous](Timing.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-05-18