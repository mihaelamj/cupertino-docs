---
title: "Appendix B: Animatable Properties"
book: "Core Animation Programming Guide"
framework: "QuartzCore"
chapterId: "TP40004514-CH11"
date: "2015-03-09"
description: "Introduces the main components and services of Core Animation."
identifier: "//apple_ref/doc/uid/TP40004514"
source: apple-archive
---

# Appendix B: Animatable Properties

> **Core Animation Programming Guide**
> Last updated: 2015-03-09

[Next](../Key-ValueCodingExtensions/Key-ValueCodingExtensions.html)[Previous](../LayerStyleProperties/LayerStyleProperties.html)
        
        
        [](../index.html)
        
        # Animatable Properties
Many of the properties in `CALayer` and `CIFilter` can be animated. This appendix lists those properties, along with the animation used by default.

## CALayer Animatable Properties
Table B-1 lists the properties of the `CALayer` class that you might consider animating. For each property, the table also lists the type of default animation object that is created to execute an implicit animation. 

**Table B-1**  Layer properties and their default animationsProperty

Default animation

`[anchorPoint](https://developer.apple.com/documentation/quartzcore/calayer/1410817-anchorpoint)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2).

`[backgroundColor](https://developer.apple.com/documentation/quartzcore/calayer/1410966-backgroundcolor)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[backgroundFilters](https://developer.apple.com/documentation/quartzcore/calayer/1410827-backgroundfilters)`

Uses the default implied `[CATransition](https://developer.apple.com/documentation/quartzcore/catransition)` object, described in [Table B-3](#//apple_ref/doc/uid/TP40004514-CH11-SW3). Sub-properties of the filters are animated using the default implied `CABasicAnimation` object, described in  [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2).

`[borderColor](https://developer.apple.com/documentation/quartzcore/calayer/1410903-bordercolor)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[borderWidth](https://developer.apple.com/documentation/quartzcore/calayer/1410917-borderwidth)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[bounds](https://developer.apple.com/documentation/quartzcore/calayer/1410915-bounds)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[compositingFilter](https://developer.apple.com/documentation/quartzcore/calayer/1410748-compositingfilter)`

Uses the default implied `[CATransition](https://developer.apple.com/documentation/quartzcore/catransition)` object, described in [Table B-3](#//apple_ref/doc/uid/TP40004514-CH11-SW3). Sub-properties of the filters are animated using the default implied `CABasicAnimation` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2).

`[contents](https://developer.apple.com/documentation/quartzcore/calayer/1410773-contents)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[contentsRect](https://developer.apple.com/documentation/quartzcore/calayer/1410866-contentsrect)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[cornerRadius](https://developer.apple.com/documentation/quartzcore/calayer/1410818-cornerradius)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[doubleSided](https://developer.apple.com/documentation/quartzcore/calayer/1410924-isdoublesided)`

There is no default implied animation.

`[filters](https://developer.apple.com/documentation/quartzcore/calayer/1410901-filters)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). Sub-properties of the filters are animated using the default implied `CABasicAnimation` object, described in  [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2).

`[frame](https://developer.apple.com/documentation/quartzcore/calayer/1410779-frame)`

This property is not animatable. You can achieve the same results by animating the `[bounds](https://developer.apple.com/documentation/quartzcore/calayer/1410915-bounds)` and `[position](https://developer.apple.com/documentation/quartzcore/calayer/1410791-position)` properties. 

`[hidden](https://developer.apple.com/documentation/quartzcore/calayer/1410838-ishidden)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[mask](https://developer.apple.com/documentation/quartzcore/calayer/1410861-mask)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[masksToBounds](https://developer.apple.com/documentation/quartzcore/calayer/1410896-maskstobounds)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[opacity](https://developer.apple.com/documentation/quartzcore/calayer/1410933-opacity)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[position](https://developer.apple.com/documentation/quartzcore/calayer/1410791-position)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[shadowColor](https://developer.apple.com/documentation/quartzcore/calayer/1410829-shadowcolor)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[shadowOffset](https://developer.apple.com/documentation/quartzcore/calayer/1410970-shadowoffset)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[shadowOpacity](https://developer.apple.com/documentation/quartzcore/calayer/1410751-shadowopacity)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[shadowPath](https://developer.apple.com/documentation/quartzcore/calayer/1410771-shadowpath)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[shadowRadius](https://developer.apple.com/documentation/quartzcore/calayer/1410819-shadowradius)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[sublayers](https://developer.apple.com/documentation/quartzcore/calayer/1410802-sublayers)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[sublayerTransform](https://developer.apple.com/documentation/quartzcore/calayer/1410888-sublayertransform)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[transform](https://developer.apple.com/documentation/quartzcore/calayer/1410836-transform)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

`[zPosition](https://developer.apple.com/documentation/quartzcore/calayer/1410884-zposition)`

Uses the default implied `[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)` object, described in [Table B-2](#//apple_ref/doc/uid/TP40004514-CH11-SW2). 

Table B-2 lists the animation attributes for the default property-based animations. 

**Table B-2**  Default Implied Basic AnimationDescription

Value

Class

`[CABasicAnimation](https://developer.apple.com/documentation/quartzcore/cabasicanimation)`

Duration

0.25 seconds, or the duration of the current transaction

Key path

Set to the property name of the layer.

Table B-3 lists the animation object configuration for default transition-based animations. 

**Table B-3**  Default Implied TransitionDescription

Value

Class

`[CATransition](https://developer.apple.com/documentation/quartzcore/catransition)`

Duration

0.25 seconds, or the duration of the current transaction

Type

Fade (`kCATransitionFade`)

Start progress

`0.0`

End progress

`1.0`

## CIFilter Animatable Properties
Core Animation adds the following animatable properties to Core Image’s `[CIFilter](https://developer.apple.com/documentation/coreimage/cifilter)` class. These properties are available only on OS X.

`[name](https://developer.apple.com/documentation/coreimage/cifilter/1437997-setname)`

`[enabled](https://developer.apple.com/documentation/coreimage/cifilter/1438276-enabled)`

For more information about these additions, see *CIFilter Core Animation Additions*.  

        
            [Next](../Key-ValueCodingExtensions/Key-ValueCodingExtensions.html)[Previous](../LayerStyleProperties/LayerStyleProperties.html)
        
         Copyright &#x00a9; 2015 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2015-03-09