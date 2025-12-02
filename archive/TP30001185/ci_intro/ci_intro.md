---
title: "Introduction"
book: "Core Image Programming Guide"
chapterId: "TP30001185-CH1"
date: "2016-09-13"
description: "Provides an overview and explains how to use and create image filters and image units."
identifier: "//apple_ref/doc/uid/TP30001185"
platforms: "OS X,iOS,tvOS"
source: apple-archive
---

# Introduction

> **Core Image Programming Guide**
> Last updated: 2016-09-13

[Next](../ci_tasks/ci_tasks.html)
        
        
        [](../index.html)
        
        # About Core Image
Core Image is an image processing and analysis technology designed to provide near real-time processing for still and video images. It operates on image data types from the Core Graphics, Core Video, and Image I/O frameworks, using either a GPU or CPU rendering path. Core Image hides the details of low-level graphics processing by providing an easy-to-use application programming interface (API). You don’t need to know the details of OpenGL, OpenGL ES, or Metal to leverage the power of the GPU, nor do you need to know anything about Grand Central Dispatch (GCD) to get the benefit of multicore processing. Core Image handles the details for you.

**Figure I-1**  Core Image in relation to the operating system## At a Glance
The Core Image framework provides:

Access to built-in image processing filters

Feature detection capability

Support for automatic image enhancement

The ability to chain multiple filters together to create custom effects

Support for creating custom filters that run on a GPU

Feedback-based image processing capabilities

On macOS, Core Image also provides a means for packaging custom filters for use by other apps.

### Core Image is Efficient and Easy to Use for Processing and Analyzing Images
Core Image provides hundreds of built-in filters. You set up filters by supplying key-value pairs for a filter’s input parameters. The output of one filter can be the input of another, making it possible to chain numerous filters together to create amazing effects. If you create a compound effect that you want to use again, you can subclass CIFilter to capture the effect “recipe.”

There are more than a dozen categories of filters. Some are designed to achieve artistic results, such as the stylize and halftone filter categories. Others are optimal for fixing image problems, such as color adjustment and sharpen filters.

Core Image can analyze the quality of an image and provide a set of filters with optimal settings for adjusting such things as hue, contrast, and tone color, and for correcting for flash artifacts such as red eye. It does all this with one method call on your part.

Core Image can detect human face features in still images and track them over time in video images. Knowing where faces are can help you determine where to place a vignette or apply other special filters.

> **Relevant chapters:** [Processing Images](../ci_tasks/ci_tasks.html#//apple_ref/doc/uid/TP30001185-CH3-TPXREF101), [Detecting Faces in an Image](../ci_detect_faces/ci_detect_faces.html#//apple_ref/doc/uid/TP30001185-CH8-SW1), [Auto Enhancing Images](../ci_autoadjustment/ci_autoadjustmentSAVE.html#//apple_ref/doc/uid/TP30001185-CH11-SW1), [Subclassing CIFilter: Recipes for Custom Effects](../ci_filer_recipes/ci_filter_recipes.html#//apple_ref/doc/uid/TP30001185-CH4-SW1)

### Query Core Image to Get a List of Filters and Their Attributes
Core Image has “built-in” reference documentation for its filters. You can query the system to find out which filters are available. Then, for each filter, you can retrieve a dictionary that contains its attributes, such as its input parameters, defaults parameter values, minimum and maximum values, display name, and more.

> **Relevant chapter:**  [Querying the System for Filters](../ci_concepts/ci_concepts.html#//apple_ref/doc/uid/TP30001185-CH2-TPXREF101)

### Core Image Can Achieve Real-Time Video Performance
If your app needs to process video in real-time, there are several things you can do to optimize performance.

> **Relevant chapter:** [Getting the Best Performance](../ci_performance/ci_performance.html#//apple_ref/doc/uid/TP30001185-CH10-SW1)

### Use an Image Accumulator to Support Feedback-Based Processing
The `CIImageAccumulator` class is designed for efficient feedback-based image processing, which you might find useful if your app needs to image dynamical systems. 

> **Relevant chapter:**  [Using Feedback to Process Images](../ci_feedback_based/ci_feedback_based.html#//apple_ref/doc/uid/TP30001185-CH5-SW5)

### Create and Distribute Custom Kernels and Filters
If none of the built-in filters suits your needs, even when chained together, consider creating a custom filter. You’ll need to understand kernels—programs that operate at the pixel level—because they are at the heart of every filter. 

In macOS, you can package one or more custom filter as an image unit so that other apps can load and use them.

> **Relevant chapters:** [What You Need to Know Before Writing a Custom Filter](../ci_advanced_concepts/ci.advanced_concepts.html#//apple_ref/doc/uid/TP30001185-CH9-SW1), [Creating Custom Filters](../ci_custom_filters/ci_custom_filters.html#//apple_ref/doc/uid/TP30001185-CH6-TPXREF101), [Packaging and Loading Image Units](../ci_image_units/ci_image_units.html#//apple_ref/doc/uid/TP30001185-CH7-SW12)

## See Also
Other important documentation for Core Image includes:

*[Core Image Reference Collection](https://developer.apple.com/documentation/coreimage)* provides a detailed description of the classes available in the Core Image framework.

*[Core Image Filter Reference](../../../Reference/CoreImageFilterReference/index.html#//apple_ref/doc/uid/TP40004346)* describes the built-in image processing filters that Apple provides, and shows how images appear before and after processing with a filter.

*[Core Image Kernel Language Reference](../../../Reference/CIKernelLangRef/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004397)* describes the language for creating kernel routines for custom filters.

        
            [Next](../ci_tasks/ci_tasks.html)
        
         Copyright &#x00a9; 2004, 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-09-13