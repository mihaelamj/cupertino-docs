---
title: "Revision History"
book: "Core Image Programming Guide"
chapterId: "TP30001185-CH99"
date: "2016-09-13"
description: "Provides an overview and explains how to use and create image filters and image units."
identifier: "//apple_ref/doc/uid/TP30001185"
source: apple-archive
---

# Revision History

> **Core Image Programming Guide**
> Last updated: 2016-09-13

[Previous](../ci_image_units/ci_image_units.html)
        
        
        [](../index.html)
        
        # Document Revision History
This table describes the changes to *Core Image Programming Guide*.

**Date****Notes**2016-09-13Updated for iOS 10.0, tvOS 10.0, and OS X v10.12.

 Rewrote the [Processing Images](../ci_tasks/ci_tasks.html#//apple_ref/doc/uid/TP30001185-CH3-TPXREF101) chapter to reflect modern Core Image best practices.

2016-03-21Corrected typos and references to features that were formerly for OS X only and are now available in iOS.

2015-10-21Corrected errors in example code listings.

2015-09-16Fixed errors in code examples.

2014-11-18Fixed errors in code examples.

2013-10-22Corrected several minor errors.

 Updated code examples to use number, array, and dictionary literals.

 Updated code examples to use standard key constants where available (for example, `[kCIInputImageKey](https://developer.apple.com/documentation/coreimage/kciinputimagekey)` instead of `@"inputImage"`).

2013-01-28Corrected code listing in "The color cube in code".

 Corrected code line `c[3] = rgb[3] * alpha` in [Listing 5-3](../ci_filer_recipes/ci_filter_recipes.html#//apple_ref/doc/uid/TP30001185-CH4-SW8) to read simply `c[3] = alpha`.

2012-09-19Updated for iOS 6.0 and OS X v10.7.

 Updated the introduction chapter.

 Added chapters on face detection and auto enhancement filters.

 Added information on performance best practices.

 Added recipes for subclassing CIFilter to get custom effects.

 Moved information on using the `CIImageAccumulator` class to its own chapter and revised the content to bring it up to date.

 Added clarification on naming input parameters for custom filters.

 Added information on thread safety.

 Fixed broken link to external article.

2011-10-12Included for Core Image on iOS 5.

2008-06-09Added details on coordinate spaces.

 Added information to [Building a Dictionary of Filters](../ci_concepts/ci_concepts.html#//apple_ref/doc/uid/TP30001185-CH2-SW1).

2007-10-31Updated for OS X v10.5.

 Added a note to [Building a Dictionary of Filters](../ci_concepts/ci_concepts.html#//apple_ref/doc/uid/TP30001185-CH2-SW1).

 Add information about CIFilter Image Kit Additions.

 Added information about support for RAW images.

 Updated links to references and added links in several places to *[Image Unit Tutorial](../../ImageUnitTutorial/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004531)*.

 Revised [Executable and Nonexecutable Filters](../ci_advanced_concepts/ci.advanced_concepts.html#//apple_ref/doc/uid/TP30001185-CH9-SW13).

 Revised [Write an Output Image Method](../ci_custom_filters/ci_custom_filters.html#//apple_ref/doc/uid/TP30001185-CH6-BCIEDBFC).

2007-05-29Added link to Cocoa memory management.

 Removed section on memory management. Instead, see *[Advanced Memory Management Programming Guide](../../../../Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html#//apple_ref/doc/uid/10000011i)*.

 Added a note to [Building a Dictionary of Filters](../ci_concepts/ci_concepts.html#//apple_ref/doc/uid/TP30001185-CH2-SW1).

 Fixed a typographical error.

2007-01-08Fixed minor technical and typographical errors.

2006-09-05Fixed minor technical problem.

 Corrected the angular values for colors in [Creating a CIFilter Object and Setting Values](../ci_tasks/ci_tasks.html#//apple_ref/doc/uid/TP30001185-CH3-SW3).

2006-06-28Reorganized content and added task information.

 Removed the appendix “Core Image Filters” and created a new document named *[Core Image Filter Reference](../../../Reference/CoreImageFilterReference/index.html#//apple_ref/doc/uid/TP40004346)*.

 Removed the appendix “Core Image Kernel Language” and created a new document named *[Core Image Kernel Language Reference](../../../Reference/CIKernelLangRef/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004397)*.

 Added [Kernel Routine Examples](../ci_custom_filters/ci_custom_filters.html#//apple_ref/doc/uid/TP30001185-CH6-SW5) to [Creating Custom Filters](../ci_custom_filters/ci_custom_filters.html#//apple_ref/doc/uid/TP30001185-CH6-TPXREF101) and changed some of the short variable names to long ones in the code listings. Added information to [Computing a Hole Distortion](../ci_custom_filters/ci_custom_filters.html#//apple_ref/doc/uid/TP30001185-CH6-SW4) to clarify the purpose of the example.

 Moved information about packaging filters as image units into its own chapter. Added additional information about the files needed in the project and where to install the image unit. See [Before You Get Started](../ci_image_units/ci_image_units.html#//apple_ref/doc/uid/TP30001185-CH7-SW14), [Build and Test the Image Unit](../ci_image_units/ci_image_units.html#//apple_ref/doc/uid/TP30001185-CH7-SW13), and [See Also](../ci_image_units/ci_image_units.html#//apple_ref/doc/uid/TP30001185-CH7-SW15).

 Updated the book introduction and some of the chapter introductions to reflect the chapter and appendix changes.

 Revised [Creating Custom Filters](../ci_custom_filters/ci_custom_filters.html#//apple_ref/doc/uid/TP30001185-CH6-TPXREF101). In particular, see [Write a Custom Attributes Method](../ci_custom_filters/ci_custom_filters.html#//apple_ref/doc/uid/TP30001185-CH6-SW1) and [Register the Filter](../ci_custom_filters/ci_custom_filters.html#//apple_ref/doc/uid/TP30001185-CH6-BCIBGCGJ).

 Added additional information on how to create nonexecutable filters. See [Writing Nonexecutable Filters](../ci_custom_filters/ci_custom_filters.html#//apple_ref/doc/uid/TP30001185-CH6-CJBFHGGC).

 Revised information on creating a CIContext object from an OpenGL graphics context.

 Fixed formatting and, in online versions of this document, provided hyperlinks to the image creation functions in “Methods used to create a CIImage object from existing image sources.”

 Added hyperlinks to most symbols and to sample code available in the ADC Reference Library.

 Numerous small formatting and grammatical changes throughout.

2005-12-06Made minor corrections to a few filter parameters. Added information on the CIFilterBrowser widget.

2005-11-09Fixed several typographical errors and a broken hyperlink.

2005-08-11Updated a figure in the PDF version of this document.

2005-07-07Corrected typographical errors.

2005-04-29Updated for public release of OS X v10.4. First public version.

 Changed the title from *Image Processing With Core Image* to make it more consistent with the titles of similar documentation.

 Completely revised [Querying the System for Filters](../ci_concepts/ci_concepts.html#//apple_ref/doc/uid/TP30001185-CH2-TPXREF101) to provide more in-depth information about how Core Image works. 

 Split the chapter titled Core Image Tasks into two chapters: [Processing Images](../ci_tasks/ci_tasks.html#//apple_ref/doc/uid/TP30001185-CH3-TPXREF101) and [Creating Custom Filters](../ci_custom_filters/ci_custom_filters.html#//apple_ref/doc/uid/TP30001185-CH6-TPXREF101). Completely updated the content in each to reflect additions to the API and to provide more in-depth information. 

 Added Using Transition Effects.

 Added [Using Feedback to Process Images](../ci_feedback_based/ci_feedback_based.html#//apple_ref/doc/uid/TP30001185-CH5-SW5).

 Added Applying a Filter to Video.

 Added [Expressing Image Processing Operations in Core Image](../ci_custom_filters/ci_custom_filters.html#//apple_ref/doc/uid/TP30001185-CH6-BCIHEJJI).

 Added [Use Quartz Composer to Test the Kernel Routine](../ci_custom_filters/ci_custom_filters.html#//apple_ref/doc/uid/TP30001185-CH6-BCICGCJF).

 Provided more information on the region of interest and ROI functions. See [The Region of Interest](../ci_advanced_concepts/ci.advanced_concepts.html#//apple_ref/doc/uid/TP30001185-CH9-SW12) and [Supplying an ROI Function](../ci_custom_filters/ci_custom_filters.html#//apple_ref/doc/uid/TP30001185-CH6-CJBECAEC).

 Provided more information on executable and nonexecutable filters. See [Executable and Nonexecutable Filters](../ci_advanced_concepts/ci.advanced_concepts.html#//apple_ref/doc/uid/TP30001185-CH9-SW13) and [Writing Nonexecutable Filters](../ci_custom_filters/ci_custom_filters.html#//apple_ref/doc/uid/TP30001185-CH6-CJBFHGGC).

 Updated the appendix “Core Image Filters to include recently-added built-in Core Image filters. Also replaced many of the figures to provide a better idea of the result produced by a filter.

 Updated the appendix “Core Image Kernel Language” to reflect changes in the kernel language. Added explanations for the kernel routine examples.

2004-06-29New seed draft that describes an image processing technology, built into OS X v10.4, that provides access to built-in image filters for both video and still images and support for custom filters and real-time processing.

        
            [Previous](../ci_image_units/ci_image_units.html)
        
         Copyright &#x00a9; 2004, 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-09-13