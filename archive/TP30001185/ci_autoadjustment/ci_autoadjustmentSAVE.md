---
title: "Auto Enhancing Images"
book: "Core Image Programming Guide"
framework: "CoreImage"
chapterId: "TP30001185-CH11"
date: "2016-09-13"
description: "Provides an overview and explains how to use and create image filters and image units."
identifier: "//apple_ref/doc/uid/TP30001185"
source: apple-archive
---

# Auto Enhancing Images

> **Core Image Programming Guide**
> Last updated: 2016-09-13

[Next](../ci_concepts/ci_concepts.html)[Previous](../ci_detect_faces/ci_detect_faces.html)
        
        
        [](../index.html)
        
        # Auto Enhancing Images
The auto enhancement feature of Core Image analyzes an image for its histogram, face region contents, and metadata properties. It then returns an array of `[CIFilter](https://developer.apple.com/documentation/coreimage/cifilter)` objects whose input parameters are already set to values that will improve the analyzed image.

Auto enhancement is available in iOS v5.0 and later and in OS X v10.8 and later.

## Auto Enhancement Filters
Table 3-1 shows the filters Core Image uses for automatically enhancing images. These filters remedy some of the most common issues found in photos.

**Table 3-1**  Filters that Core Image uses to enhance an imageFilter

Purpose

CIRedEyeCorrection 

Repairs red/amber/white eye due to camera flash

CIFaceBalance

Adjusts the color of a face to give pleasing skin tones

CIVibrance

Increases the saturation of an image without distorting the skin tones

CIToneCurve

Adjusts image contrast

CIHighlightShadowAdjust

Adjusts shadow details

## Using Auto Enhancement Filters
The auto enhancement API has only two methods: `[autoAdjustmentFilters](https://developer.apple.com/documentation/coreimage/ciimage/1645889-autoadjustmentfilters)` and `[autoAdjustmentFiltersWithOptions:](https://developer.apple.com/documentation/coreimage/ciimage/1437792-autoadjustmentfilters)`. In most cases, you’ll want to use the method that provides an options dictionary. 

You can set these options:

The image orientation, which is important for the CIRedEyeCorrection and CIFaceBalance filters, so that Core Image can find faces accurately.

Whether to apply only red eye correction. (Set `kCIImageAutoAdjustEnhance` to `false`.)

Whether to apply all filters except red eye correction. (Set `kCIImageAutoAdjustRedEye` to `false`.)

The `[autoAdjustmentFiltersWithOptions:](https://developer.apple.com/documentation/coreimage/ciimage/1437792-autoadjustmentfilters)` method returns an array of options filters that you’ll then want to chain together and apply to the analyzed image, as shown in Listing 3-1. The code first creates an options dictionary. It then gets the orientation of the image and sets that as the value for the key `CIDetectorImageOrientation`.

**Listing 3-1**  Getting auto enhancement filters and applying them to an image

NSDictionary *options = @{ CIDetectorImageOrientation :
```
                 [[image properties] valueForKey:kCGImagePropertyOrientation] };
```

```
NSArray *adjustments = [myImage autoAdjustmentFiltersWithOptions:options];
```

```
for (CIFilter *filter in adjustments) {
```

```
     [filter setValue:myImage forKey:kCIInputImageKey];
```

```
     myImage = filter.outputImage;
```

```
}
```
Recall that the input parameter values are already set by Core Image to produce the best result. 

You don’t have to apply the auto adjustment filters right away. You can save the filter names and parameter values for later. Saving them allows your app to perform the enhancements later without the cost of analyzing the image again.

        
            [Next](../ci_concepts/ci_concepts.html)[Previous](../ci_detect_faces/ci_detect_faces.html)
        
         Copyright &#x00a9; 2004, 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-09-13