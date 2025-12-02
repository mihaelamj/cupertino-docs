---
title: "Detecting Faces in an Image"
book: "Core Image Programming Guide"
framework: "CoreImage"
chapterId: "TP30001185-CH8"
date: "2016-09-13"
description: "Provides an overview and explains how to use and create image filters and image units."
identifier: "//apple_ref/doc/uid/TP30001185"
source: apple-archive
---

# Detecting Faces in an Image

> **Core Image Programming Guide**
> Last updated: 2016-09-13

[Next](../ci_autoadjustment/ci_autoadjustmentSAVE.html)[Previous](../ci_tasks/ci_tasks.html)
        
        
        [](../index.html)
        
        # Detecting Faces in an Image
Core Image can analyze and find human faces in an image. It performs face detection, not recognition. Face **detection** is the identification of rectangles that contain human face features, whereas face **recognition** is the identification of specific human faces (John, Mary, and so on). After Core Image detects a face, it can provide information about face features, such as eye and mouth positions. It can also track the position an identified face in a video.

**Figure 2-1**  Core Image identifies face bounds in an imageKnowing where the faces are in an image lets you perform other operations, such as cropping or adjusting the image quality of the face (tone balance, red-eye correction and so on). You can also perform other interesting operations on the faces; for example:

[Anonymous Faces Filter Recipe](../ci_filer_recipes/ci_filter_recipes.html#//apple_ref/doc/uid/TP30001185-CH4-SW22) shows how to apply a pixellate filter only to the faces in an image.

[White Vignette for Faces Filter Recipe](../ci_filer_recipes/ci_filter_recipes.html#//apple_ref/doc/uid/TP30001185-CH4-SW12) shows how to place a vignette around a face.

> **Note:** Face detection is available in iOS v5.0 and later and in OS X v10.7 and later.

## Detecting Faces
Use the `[CIDetector](https://developer.apple.com/documentation/coreimage/cidetector)` class to find faces in an image as shown in Listing 2-1.

**Listing 2-1**  Creating a face detector

CIContext *context = [CIContext context];                    // 1
```
NSDictionary *opts = @{ CIDetectorAccuracy : CIDetectorAccuracyHigh };      // 2
```

```
CIDetector *detector = [CIDetector detectorOfType:CIDetectorTypeFace
```

```
                                          context:context
```

```
                                          options:opts];                    // 3
```

```
 
```

```
opts = @{ CIDetectorImageOrientation :
```

```
          [[myImage properties] valueForKey:kCGImagePropertyOrientation] }; // 4
```

```
NSArray *features = [detector featuresInImage:myImage options:opts];        // 5
```
Here’s what the code does:

Creates a context with default options. You can use any of the context-creation functions described in [Processing Images](../ci_tasks/ci_tasks.html#//apple_ref/doc/uid/TP30001185-CH3-TPXREF101).) You also have the option of supplying `nil` instead of a context when you create the detector.)

Creates an options dictionary to specify accuracy for the detector. You can specify low or high accuracy. Low accuracy (`CIDetectorAccuracyLow`) is fast; high accuracy, shown in this example, is thorough but slower.

Creates a detector for faces. The only type of detector you can create is one for human faces.

Sets up an options dictionary for finding faces. It’s important to let Core Image know the image orientation so the detector knows where it can find upright faces. Most of the time you’ll read the image orientation from the image itself, and then provide that value to the options dictionary.

Uses the detector to find features in an image. The image you provide must be a `CIImage` object. Core Image returns an array of `[CIFeature](https://developer.apple.com/documentation/coreimage/cifeature)` objects, each of which represents a face in the image.

After you get an array of faces, you’ll probably want to find out their characteristics, such as where the eyes and mouth are located. The next sections describes how.

## Getting Face and Face Feature Bounds
Face features include:

left and right eye positions

mouth position

tracking ID and tracking frame count which Core Image uses to follow a face in a video segment (available in iOS v6.0 and later and in OS X v10.8 and later)

After you get an array of face features from a `CIDetector` object, you can loop through the array to examine the bounds of each face and each feature in the faces, as shown in Listing 2-2. 

**Listing 2-2**  Examining face feature bounds

```
for (CIFaceFeature *f in features) {
```

```
    NSLog(@"%@", NSStringFromRect(f.bounds));
```

```
 
```

```
    if (f.hasLeftEyePosition) {
```

```
        NSLog(@"Left eye %g %g", f.leftEyePosition.x, f.leftEyePosition.y);
```

```
    }
```

```
    if (f.hasRightEyePosition) {
```

```
        NSLog(@"Right eye %g %g", f.rightEyePosition.x, f.rightEyePosition.y);
```

```
    }
```

```
    if (f.hasMouthPosition) {
```

```
        NSLog(@"Mouth %g %g", f.mouthPosition.x, f.mouthPosition.y);
```

```
    }
```

```
}
```

        
            [Next](../ci_autoadjustment/ci_autoadjustmentSAVE.html)[Previous](../ci_tasks/ci_tasks.html)
        
         Copyright &#x00a9; 2004, 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-09-13