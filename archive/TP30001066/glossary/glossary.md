---
title: "Glossary"
book: "Quartz 2D Programming Guide"
framework: "CoreGraphics"
chapterId: "TP30001066-CH221"
date: "2017-03-21"
description: "Explains how to use Quartz 2D. Includes illustrations and sample code."
identifier: "//apple_ref/doc/uid/TP30001066"
source: apple-archive
---

# Glossary

> **Quartz 2D Programming Guide**
> Last updated: 2017-03-21

[Next](../RevisionHistory.html)[Previous](../dq_text/dq_text.html)
        
        
        [](../index.html)
        
        # Glossary

**alpha value**The graphics state parameter that Quartz uses to determine how to composite newly painted objects to the existing page. At full intensity (alpha = `1.0`), newly painted objects are opaque. At zero intensity, newly painted objects are invisible (alpha = `0.0`).

**axial gradient**A fill that varies along an axis between two defined end points.  All points that lie on a line perpendicular to the axis have the same color value. Also called a *linear gradient*.

**bitmap**A rectangular array (or raster) of pixels, each pixel representing a point in an image. Bitmap images are also called *sampled images*. 

**blend mode**Specifies how Quartz combines the foreground painting with the background painting.

**clipping area**A path used to constrain the drawing of other objects within its bounds.

**color space** A one-, two-, three-, or four-dimensional environment whose components (or channels) represent intensity values. For example, RGB space is a three-dimensional color space whose stimuli are the red, green, and blue intensities that make up a given color; and red, green, and blue are color channels.

**concatenation**An operation that combines two matrices by multiplying them together.

**current graphics state**The parameters values that determine how Quartz renders results as it paints.

**current point**The last location Quartz used when painting a path.

**current transformation matrix**An affine transform that Quartz uses to map points from one coordinate space to another.

**device color space**A color space that is tied to the system of color representation for a particular device. This type of color space is not suitable for interchanges of color data between different devices.

**device-independent color space**A color representation that is portable between devices and that is used for the interchanges of color data from the native color space of one device to the native color space of another device. Colors in a device-independent color space appear the same when displayed on different devices, to the extent that the capabilities of the device allow.

**even-odd rule**A fill rule  that determines when to paint a pixel. The outcome does not depend on the direction that path segments are drawn. Compare with [nonzero winding number rule](#//apple_ref/doc/uid/TP30001066-CH221-SW2).

**fill**An operation that paints the area within a path.

**generic color space**A device-independent color space chosen automatically by Mac OS X to produce the best color for the drawing destination. 

**gradient**A fill that  varies from one color to another. See also [axial gradient](#//apple_ref/doc/uid/TP30001066-CH221-SW4) and [radial gradient](#//apple_ref/doc/uid/TP30001066-CH221-SW5).

**graphics context**An opaque data type (`[CGContextRef](https://developer.apple.com/documentation/coregraphics/cgcontextref)`) that encapsulates the information Quartz uses to draw images to an output device, such as a PDF file, a bitmap, or a window on a display. The information inside a graphics context includes graphics drawing parameters and a device-specific representation of the paint on the page.

**identity transform**An affine transform that, when applied to input coordinates, always returns the input coordinates.

**image mask**A bitmap that specifies an area to paint, but not the color. An image mask acts like a stencil to specify where to place color on the page. 

**inversion**An operation that produces original coordinates from transformed ones.

**layer context**An offscreen drawing destination (`[CGLayerRef](https://developer.apple.com/documentation/coregraphics/cglayerref)`) designed for optimal performance. A a layer context is a much better choice for offscreen drawing than a bitmap graphics context.

**line cap**The style that Quartz uses to draw the endpoint of a line—butt, round, or projecting square.

**line dash pattern**The repeating series of line segments and spaces used to paint a dashed line.

**line join** The style that Quartz uses to draw the junction between connected line segments—miter, round, or bevel.

**line width**The total width of a line, expressed in user space units.

**linear gradient**See [axial gradient](#//apple_ref/doc/uid/TP30001066-CH221-SW4).

**nonzero winding number rule**A fill rule  that determines when to paint a pixel. The outcome depends on the direction that path segments are drawn. Compare with [even-odd rule](#//apple_ref/doc/uid/TP30001066-CH221-SW3).

**page**The virtual canvas that Quartz paints to.

**painter’s model**A drawing model in which each successive drawing operation applies a layer of paint to a [page](#//apple_ref/doc/uid/TP30001066-CH221-SW1).

**path**One or more shapes (known as subpaths) that Quartz paints as a unit. A subpath can consist of straight lines, curves, or both. It can be open or closed.

**pattern**A sequence of drawing operations that Quartz can repeatedly paint to a graphics context. 

**pattern space**An abstract space that maps to the default user space by the transformation matrix (the pattern matrix) you specify when you create the pattern. Pattern space is separate from user space. The untransformed pattern space maps to the base (untransformed) user space, regardless of the state of the current transformation matrix.

**premultiplied alpha**A source color whose components are already multiplied by an alpha value. Premultiplying speeds up the rendering of an image by eliminating an extra multiplication operation per color component. See also [alpha value](#//apple_ref/doc/uid/TP30001066-CH221-SW6).

**radial gradient**A fill that varies radially along an axis between two defined ends, which typically are both circles. Points share the same color value if they lie on the circumference of a circle whose center point falls on the axis. The radius of the circular sections of the gradient are defined by the radii of the end circles; the radius of each intermediate circle varies linearly from one end to the other.

**rendering intent**Specifies how Quartz maps colors from the source color space to those that are within the gamut of the destination color space of a graphics context.

**rotation**An operation that moves the coordinate space the specified angle.

**scaling**An operation that changes the scale of the coordinate space by the specified x and y factors, effectively stretching or shrinking coordinates. The magnitude of the x and y factors governs whether the new coordinates are larger or smaller than the original. A negative factor flips the corresponding axis.

**shadow**An image painted underneath, and offset from, a graphics object such that the shadow mimics the effect of a light source cast on the graphics object.

**stroke**An operation that paints a line that straddles a path.

**tiling**The process of rendering pattern cells to a portion of a page. Quartz has three tiling options—no distortion, constant spacing with minimal distortion, and constant spacing.

**translation**An operation that moves the origin of the coordinate space by the number of units specified for the x and y axes.

**transparency layer**A composite of two or more objects that Quartz treats as a single object when applying effects, such as shadows.

**user space**The device-independent coordinate system that you use when drawing using Quartz 2D.

        
            [Next](../RevisionHistory.html)[Previous](../dq_text/dq_text.html)
        
         Copyright &#x00a9; 2001, 2017 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2017-03-21