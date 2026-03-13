---
title: "Introduction"
book: "Quartz 2D Programming Guide"
chapterId: "TP40007533"
date: "2017-03-21"
description: "Explains how to use Quartz 2D. Includes illustrations and sample code."
identifier: "//apple_ref/doc/uid/TP30001066"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Introduction

> **Quartz 2D Programming Guide**
> Last updated: 2017-03-21

[Next](../dq_overview/dq_overview.html)
        
        
        [](../index.html)
        
        # Introduction

Core Graphics, also known as Quartz 2D, is an advanced, two-dimensional drawing engine available for iOS, tvOS and macOS application development. Quartz 2D provides low-level, lightweight 2D rendering with unmatched output fidelity regardless of display or printing device. Quartz 2D is resolution- and device-independent.

The Quartz 2D API is easy to use and provides access to powerful features such as transparency layers, path-based drawing, offscreen rendering, advanced color management, anti-aliased rendering, and PDF document creation, display, and parsing. 

## Who Should Read This Document?
This document is intended for developers who need to perform any of the following tasks:

Draw graphics

Provide graphics editing capabilities in an application

Create or display bitmap images

Work with PDF documents

## Organization of This Document
This document is organized into the following chapters:

[Overview of Quartz 2D](../dq_overview/dq_overview.html#//apple_ref/doc/uid/TP30001066-CH202-TPXREF101) describes the page, drawing destinations, Quartz opaque data types, graphics states, coordinates, and memory management, and it takes a look at how Quartz works “under the hood.”

[Graphics Contexts](../dq_context/dq_context.html#//apple_ref/doc/uid/TP30001066-CH203-TPXREF101) describes the kinds of drawing destinations and provides step-by-step instructions for creating all flavors of graphics contexts.

[Paths](../dq_paths/dq_paths.html#//apple_ref/doc/uid/TP30001066-CH211-TPXREF101) discusses the basic elements that make up paths, shows how to create and paint them, shows how to set up a clipping area, and explains how blend modes affect painting.

[Color and Color Spaces](../dq_color/dq_color.html#//apple_ref/doc/uid/TP30001066-CH205-TPXREF101) discusses color values and using alpha values for transparency, and it describes how to create a color space, set colors, create color objects, and set rendering intent.

[Transforms](../dq_affine/dq_affine.html#//apple_ref/doc/uid/TP30001066-CH204-TPXREF101) describes the current transformation matrix and explains how to modify it, shows how to set up affine transforms, shows how to convert between user and device space, and provides background information on the mathematical operations that Quartz performs.

[Patterns](../dq_patterns/dq_patterns.html#//apple_ref/doc/uid/TP30001066-CH206-TPXREF101) defines what a pattern and its parts are, tells how Quartz renders them, and shows how to create colored and stenciled patterns.

[Shadows](../dq_shadows/dq_shadows.html#//apple_ref/doc/uid/TP30001066-CH208-TPXREF101) describes what shadows are, explains how they work, and shows how to paint with them.

[Gradients](../dq_shadings/dq_shadings.html#//apple_ref/doc/uid/TP30001066-CH207-TPXREF101) discusses axial and radial gradients and shows how to create and use CGShading and CGGradient objects.

[Transparency Layers](../dq_trans_layers/dq_trans_layers.html#//apple_ref/doc/uid/TP30001066-CH210-TPXREF101) gives examples of what transparency layers look like, discusses how they work, and provides step-by-step instructions for implementing them.

[Data Management in Quartz 2D](../dq_data_mgr/dq_data_mgr.html#//apple_ref/doc/uid/TP30001066-CH216-TPXREF101) discusses how to move data into and out of Quartz.

[Bitmap Images and Image Masks](../dq_images/dq_images.html#//apple_ref/doc/uid/TP30001066-CH212-TPXREF101) describes what makes up a bitmap image definition and shows how to use a bitmap image as a Quartz drawing primitive. It also describes masking techniques you can use on images and shows the various effects you can achieve by using blend modes when drawing images.

[Core Graphics Layer Drawing](../dq_layers/dq_layers.html#//apple_ref/doc/uid/TP30001066-CH219-TPXREF101) describes how to create and use drawing layers to achieve high-performance patterned drawing or to draw offscreen.

[PDF Document Creation, Viewing, and Transforming](../dq_pdf/dq_pdf.html#//apple_ref/doc/uid/TP30001066-CH214-TPXREF101) shows how to open and view PDF documents, apply transforms to them, create a PDF file, access PDF metadata, add links, and add security features (such as password protection).

[PDF Document Parsing](../dq_pdf_scan/dq_pdf_scan.html#//apple_ref/doc/uid/TP30001066-CH220-TPXREF101) describes how to use CGPDFScanner and CGPDFContentStream objects to parse and inspect PDF documents.

[PostScript Conversion](../dq_ps_convert/dq_ps_convert.html#//apple_ref/doc/uid/TP30001066-CH215-TPXREF101) gives an overview of the functions you can use in Mac OS X to convert a PostScript file to a PDF document. These functions are not available in iOS.

[Text](../dq_text/dq_text.html#//apple_ref/doc/uid/TP30001066-CH213-TPXREF101) describes Quartz 2D low-level support for text and glyphs, and alternatives that provide higher-level and Unicode text support. It also discusses how to copy font variations.

[Glossary](../glossary/glossary.html#//apple_ref/doc/uid/TP30001066-CH221-SW7) defines the terms used in this guide.

## See Also
These items are essential reading for anyone using Quartz 2D:

[Core Graphics Framework Reference](http://developer.apple.com/reference/coregraphics) provides a complete reference for the Quartz 2D application programming interface.

*[Color Management Overview](../../csintro/csintro_intro/csintro_intro.html#//apple_ref/doc/uid/TP30001148)* is a brief introduction to the principles of color perception, color spaces, and color management systems. 

Mailing lists. Join the [quartz-dev](http://lists.apple.com/mailman/listinfo/quartz-dev) mailing list to discuss problems using Quartz 2D.

        
            [Next](../dq_overview/dq_overview.html)
        
         Copyright &#x00a9; 2001, 2017 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2017-03-21