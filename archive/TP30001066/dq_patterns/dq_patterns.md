---
title: "Patterns"
book: "Quartz 2D Programming Guide"
chapterId: "TP30001066-CH206"
date: "2017-03-21"
description: "Explains how to use Quartz 2D. Includes illustrations and sample code."
identifier: "//apple_ref/doc/uid/TP30001066"
source: apple-archive
---

# Patterns

> **Quartz 2D Programming Guide**
> Last updated: 2017-03-21

[Next](../dq_shadows/dq_shadows.html)[Previous](../dq_affine/dq_affine.html)
        
        
        [](../index.html)
        
        # Patterns
A *pattern* is a sequence of drawing operations that is repeatedly painted to a graphics context. You can use patterns in the same way as you use colors. When you paint using a pattern, Quartz divides the page into a set of pattern cells, with each cell the size of the pattern image, and draws each cell using a callback you provide. Figure 6-1 shows a pattern drawn to a window graphics context.

**Figure 6-1**  A pattern drawn to a window## The Anatomy of a Pattern
The pattern cell is the basic component of a pattern. The pattern cell for the pattern shown in [Figure 6-1](#//apple_ref/doc/uid/TP30001066-CH206-BBCBHAIA) is shown in Figure 6-2. The black rectangle is not part of the pattern; it’s drawn to show where the pattern cell ends. 

**Figure 6-2**  A pattern cellThe size of this particular pattern cell includes the area of the four colored rectangles and space above and to the right of the rectangles, as shown in Figure 6-3. The black rectangle surrounding each pattern cell in the figure is not part of the cell; it’s drawn to indicate the *bounds* of the cell. When you create a pattern cell, you define the bounds of the cell and draw within the bounds.

**Figure 6-3**  Pattern cells with black rectangles drawn to show the bounds of each cellYou can specify how far apart Quartz draws the start of each pattern cell from the next in the horizontal and vertical directions. The pattern cells in Figure 6-3 are drawn so that the start of one pattern cell is exactly a pattern width apart from the next pattern cell, resulting in each pattern cell abutting on the next. The pattern cells in Figure 6-4 have space added in both directions, horizontal and vertical. You can specify different *spacing values* for each direction. If you make the spacing less than the width or height of a pattern cell, the pattern cells overlap.

**Figure 6-4**  Spacing between pattern cellsWhen you draw a pattern cell, Quartz uses *pattern space* as the coordinate system. Pattern space is an abstract space that maps to the default user space by the transformation matrix you specify when you create the pattern—the *pattern matrix*. 

> **Note:** Pattern space is separate from user space. The untransformed pattern space maps to the base (untransformed) user space, regardless of the state of the current transformation matrix. When you apply a transformation to pattern space, Quartz applies the transform only to pattern space.

The default conventions for a pattern’s coordinate systems are those of the underlying graphics context. By default, Quartz uses a coordinate system where a positive x value represents a displacement to the right and a positive y value represents an upward displacement. However, a graphics context created by UIKit uses a different convention, where positive y values indicate a downward displacement. While this convention is normally applied to the graphics context by concatenating a transformation onto the coordinate system, in this case, Quartz *also* modifies the default conventions of pattern space to match.

If you don’t want Quartz to transform the pattern cell, you can specify the identity matrix. However, you can achieve interesting effects by supplying a transformation matrix. Figure 6-5 shows the effect of scaling the pattern cell shown in Figure 6-2. Figure 6-6 demonstrates rotating the pattern cell. Translating the pattern cell is a bit more subtle. Figure 6-7 shows the origin of the pattern, with the pattern cell translated in both directions, horizontal and vertical, so that the pattern no longer abuts the window as it does in [Figure 6-1](#//apple_ref/doc/uid/TP30001066-CH206-BBCBHAIA). 

**Figure 6-5**  A scaled pattern cell**Figure 6-6**  A rotated pattern cell**Figure 6-7**  A translated pattern cell## Colored Patterns and Stencil (Uncolored) Patterns
*Colored patterns* have inherent colors associated with them. Change the coloring used to create the pattern cell, and the pattern loses its meaning. A Scottish tartan (such as the sample one shown in Figure 6-8) is an example of a colored pattern. The color in a colored pattern is specified as part of the pattern cell creation process, not as part of the pattern drawing process.

**Figure 6-8**  A colored pattern has inherent colorOther patterns are defined solely on their shape and, for that reason, can be thought of as *stencil patterns*, uncolored patterns, or even as an image mask. The red and black stars shown in Figure 6-9 are each renditions of the same pattern cell. The cell itself consists of one shape—a filled star. When the pattern cell was defined, no color was associated with it. The color is specified as part of the pattern drawing process, not as part of the pattern cell creation.

**Figure 6-9**  A stencil pattern does not have inherent colorYou can create either kind of pattern—colored or stencil—in Quartz 2D.

## Tiling
*Tiling* is the process of rendering pattern cells to a portion of a page. When Quartz renders a pattern to a device, Quartz may need to adjust the pattern to fit the device space. That is, the pattern cell as defined in user space might not fit perfectly when rendered to the device because of differences between user space units and device pixels.

Quartz has three tiling options it can use to adjust patterns when necessary. Quartz can preserve:

The pattern, at the expense of adjusting the spacing between pattern cells slightly, but by no more than one device pixel. This is referred to as *no distortion*.

Spacing between cells, at the expense of distorting the pattern cell slightly, but by no more than one device pixel. This is referred to as *constant spacing with minimal distortion*.

Spacing between cells (as for the minimal distortion option) at the expense of distorting the pattern cell as much as needed to get fast tiling. This is referred to as *constant spacing*.

## How Patterns Work
Patterns operate similarly to colors, in that you set a fill or stroke pattern and then call a painting function. Quartz uses the pattern you set as the “paint.” For example, if you want to paint a filled rectangle with a solid color, you first call a function, such as `CGContextSetFillColor`, to set the fill color. Then you call the function `CGContextFillRect` to paint the filled rectangle with the color you specify. To paint with a pattern, you first call the function `CGContextSetFillPattern` to set the pattern. Then you call `CGContextFillRect` to actually paint the filled rectangle with the pattern you specify. The difference between painting with colors and with patterns is that you must define the pattern. You supply the pattern and color information to the function `CGContextSetFillPattern`. You’ll see how to create, set, and paint patterns in [Painting Colored Patterns](#//apple_ref/doc/uid/TP30001066-CH206-TPXREF117) and [Painting Stencil Patterns](#//apple_ref/doc/uid/TP30001066-CH206-BBCJAJEC). 

Here’s an example of how Quartz works behind the scenes to paint with a pattern you provide. When you fill or stroke with a pattern, Quartz conceptually performs the following tasks to draw each pattern cell:

Saves the graphics state.

Translates the current transformation matrix to the origin of the pattern cell.

Concatenates the CTM with the pattern matrix.

Clips to the bounding rectangle of the pattern cell.

Calls your drawing callback to draw the pattern cell.

Restores the graphics state.

Quartz takes care of all the tiling for you, repeatedly rendering the pattern cell to the drawing space until the entire space is painted. You can fill or stroke with a pattern. The pattern cell can be of any size you specify. If you want to see the pattern, you should make sure the pattern cell fits in the drawing space. For example, if your pattern cell is 8 units by 10 units, and you use the pattern to stroke a line that has a width of 2 units, the pattern cell will be clipped since it is 10 units wide. In this case, you might not recognize the pattern.

## Painting Colored Patterns
The five steps you need to perform to paint a colored pattern are described in the following sections: 

[Write a Callback Function That Draws a Colored Pattern Cell](#//apple_ref/doc/uid/TP30001066-CH206-BBCEHCFD)

[Set Up the Colored Pattern Color Space](#//apple_ref/doc/uid/TP30001066-CH206-BBCEEJEA)

[Set Up the Anatomy of the Colored Pattern](#//apple_ref/doc/uid/TP30001066-CH206-BBCBFEGD)

[Specify the Colored Pattern as a Fill or Stroke Pattern](#//apple_ref/doc/uid/TP30001066-CH206-BBCDECDJ)

[Draw With the Colored Pattern](#//apple_ref/doc/uid/TP30001066-CH206-BBCCIFID)

These are the same steps you use to paint a stencil pattern. The difference between the two is how you set up color information. You can see how all the steps fit together in [A Complete Colored Pattern Painting Function](#//apple_ref/doc/uid/TP30001066-CH206-BBCGHGAG).

### Write a Callback Function That Draws a Colored Pattern Cell
What a pattern cell looks like is entirely up to you. For this example, the code in [Listing 6-1](#//apple_ref/doc/uid/TP30001066-CH206-BBCEGGHH) draws the pattern cell shown in [Figure 6-2](#//apple_ref/doc/uid/TP30001066-CH206-BBCFEAJJ). Recall that the black line surrounding the pattern cell is not part of the cell; it’s drawn to show that the bounds of the pattern cell are larger than the rectangles painted by the code. You specify the pattern size to Quartz later.

Your pattern cell drawing function is a callback that follows this form:

typedef void (*CGPatternDrawPatternCallback) (
```
                        void *info,
```

```
                        CGContextRef context
```

```
    );
```
You can name your callback whatever you like. The one in Listing 6-1 is named `MyDrawColoredPattern`. The callback takes two parameters:

`info`, a generic pointer to private data associated with the pattern. This parameter is optional; you can pass `NULL`. The data passed to your callback is the same data you supply later, when you create the pattern. 

`context`, the graphics context for drawing the pattern cell.

The pattern cell drawn by the code in Listing 6-1 is arbitrary. Your code draws whatever is appropriate for the pattern you create. These details about the code are important:

The pattern size is declared. You need to keep the pattern size in mind as you write your drawing code. Here, the size is declared as a global. The drawing function doesn’t specifically refer to the size, except in a comment. Later, you specify the pattern size to Quartz 2D. See [Set Up the Anatomy of the Colored Pattern](#//apple_ref/doc/uid/TP30001066-CH206-BBCBFEGD).

The drawing function follows the prototype defined by the `CGPatternDrawPatternCallback` callback type definition.

The drawing performed in the code sets colors, which makes this a colored pattern.

**Listing 6-1**  A drawing callback that draws a colored pattern cell

```
#define H_PATTERN_SIZE 16
```

```
#define V_PATTERN_SIZE 18
```

```
 
```

```
void MyDrawColoredPattern (void *info, CGContextRef myContext)
```

```
{
```

```
    CGFloat subunit = 5; // the pattern cell itself is 16 by 18
```

```
 
```

```
    CGRect  myRect1 = {{0,0}, {subunit, subunit}},
```

```
            myRect2 = {{subunit, subunit}, {subunit, subunit}},
```

```
            myRect3 = {{0,subunit}, {subunit, subunit}},
```

```
            myRect4 = {{subunit,0}, {subunit, subunit}};
```

```
 
```

```
    CGContextSetRGBFillColor (myContext, 0, 0, 1, 0.5);
```

```
    CGContextFillRect (myContext, myRect1);
```

```
    CGContextSetRGBFillColor (myContext, 1, 0, 0, 0.5);
```

```
    CGContextFillRect (myContext, myRect2);
```

```
    CGContextSetRGBFillColor (myContext, 0, 1, 0, 0.5);
```

```
    CGContextFillRect (myContext, myRect3);
```

```
    CGContextSetRGBFillColor (myContext, .5, 0, .5, 0.5);
```

```
    CGContextFillRect (myContext, myRect4);
```

```
}
```
### Set Up the Colored Pattern Color Space
The code in [Listing 6-1](#//apple_ref/doc/uid/TP30001066-CH206-BBCEGGHH) uses colors to draw the pattern cell. You must ensure that Quartz paints with the colors you use in your drawing routine by setting the base pattern color space to `NULL`, as shown in Listing 6-2. A detailed explanation for each numbered line of code follows the listing.

**Listing 6-2**  Creating a base pattern color space

CGColorSpaceRef patternSpace;
```
 
```

```
patternSpace = CGColorSpaceCreatePattern (NULL);// 1
```

```
CGContextSetFillColorSpace (myContext, patternSpace);// 2
```

```
CGColorSpaceRelease (patternSpace);// 3
```
Here’s what the code does:

Creates a pattern color space appropriate for a colored pattern by calling the function `CGColorSpaceCreatePattern`, passing `NULL` as the base color space.

Sets the fill color space to the pattern color space. If you are stroking your pattern, call `CGContextSetStrokeColorSpace`.

Releases the pattern color space.

### Set Up the Anatomy of the Colored Pattern
Information about the anatomy of a pattern is kept in a CGPattern object. You create a CGPattern object by calling the function `CGPatternCreate`, whose prototype is shown in Listing 6-3. 

**Listing 6-3**  The CGPatternCreate function prototype

CGPatternRef CGPatternCreate (  void *info,
```
                                CGRect bounds,
```

```
                                CGAffineTransform matrix,
```

```
                                CGFloat xStep,
```

```
                                CGFloat yStep,
```

```
                                CGPatternTiling tiling,
```

```
                                bool isColored,
```

```
                                const CGPatternCallbacks *callbacks );
```
The `info` parameter is a pointer to data you want to pass to your drawing callback. This is the same pointer discussed in [Write a Callback Function That Draws a Colored Pattern Cell](#//apple_ref/doc/uid/TP30001066-CH206-BBCEHCFD).

You specify the size of the pattern cell in the `bounds` parameter. The `matrix` parameter is where you specify the pattern matrix, which maps the pattern coordinate system to the default coordinate system of the graphics context. Use the identity matrix if you want to draw the pattern using the same coordinate system as the graphics context. The `xStep` and `yStep` parameters specify the horizontal and vertical spacing between cells in the pattern coordinate system. See [The Anatomy of a Pattern](#//apple_ref/doc/uid/TP30001066-CH206-BBCIAJCH) to review information on bounds, pattern matrix, and spacing.

The `tiling` parameter can be one of three values:

`kCGPatternTilingNoDistortion`

`kCGPatternTilingConstantSpacingMinimalDistortion`

`kCGPatternTilingConstantSpacing`

See [Tiling](#//apple_ref/doc/uid/TP30001066-CH206-BBCDHCEH) to review information on tiling.

The `isColored` parameter specifies whether the pattern cell is a colored pattern (`true`) or a stencil pattern (`false`). If you pass `true` here, your drawing pattern callback specifies the pattern color, and you must set the pattern color space to the colored pattern color space (see [Set Up the Colored Pattern Color Space](#//apple_ref/doc/uid/TP30001066-CH206-BBCEEJEA)).

The last parameter you pass to the function `CGPatternCreate` is a pointer to a `CGPatternCallbacks` data structure. This structure has three fields:

struct CGPatternCallbacks
```
{
```

```
    unsigned int version;
```

```
    CGPatternDrawPatternCallback drawPattern;
```

```
    CGPatternReleaseInfoCallback releaseInfo;
```

```
};
```
You set the `version` field to `0`. The `drawPattern` field is a pointer to your drawing callback. The `releaseInfo` field is a pointer to a callback that’s invoked when the CGPattern object is released, to release storage for the `info` parameter you passed to your drawing callback. If you didn’t pass any data in this parameter, you set this field to `NULL`.

### Specify the Colored Pattern as a Fill or Stroke Pattern
You can use your pattern for filling or stroking by calling the appropriate function—`CGContextSetFillPattern` or `CGContextSetStrokePattern`. Quartz uses your pattern for any subsequent filling or stroking.

These functions each take three parameters:

The graphics context

The CGPattern object that you created previously

An array of color components

Although colored patterns supply their own color, you must pass a single alpha value to inform Quartz of the overall opacity of the pattern when it’s drawn. Alpha can vary from 1 (completely opaque) to 0 (completely transparent). These lines of code show an example of how to set opacity for a colored pattern used to fill.

```
CGFloat alpha = 1;
```

```
 
```

```
CGContextSetFillPattern (myContext, myPattern, &alpha);
```
### Draw With the Colored Pattern
After you’ve completed the previous steps, you can call any Quartz 2D function that paints. Your pattern is used as the “paint.” For example, you can call `CGContextStrokePath`, `CGContextFillPath`, `CGContextFillRect`, or any other function that paints.

### A Complete Colored Pattern Painting Function
The code in Listing 6-4 contains a function that paints a colored pattern. The function incorporates all the steps discussed previously. A detailed explanation for each numbered line of code follows the listing.

**Listing 6-4**  A function that paints a colored pattern

void MyColoredPatternPainting (CGContextRef myContext,
```
                 CGRect rect)
```

```
{
```

```
    CGPatternRef    pattern;// 1
```

```
    CGColorSpaceRef patternSpace;// 2
```

```
    CGFloat         alpha = 1,// 3
```

```
                    width, height;// 4
```

```
    static const    CGPatternCallbacks callbacks = {0, // 5
```

```
                                        &MyDrawPattern,
```

```
                                        NULL};
```

```
 
```

```
    CGContextSaveGState (myContext);
```

```
    patternSpace = CGColorSpaceCreatePattern (NULL);// 6
```

```
    CGContextSetFillColorSpace (myContext, patternSpace);// 7
```

```
    CGColorSpaceRelease (patternSpace);// 8
```

```
 
```

```
    pattern = CGPatternCreate (NULL, // 9
```

```
                    CGRectMake (0, 0, H_PSIZE, V_PSIZE),// 10
```

```
                    CGAffineTransformMake (1, 0, 0, 1, 0, 0),// 11
```

```
                    H_PATTERN_SIZE, // 12
```

```
                    V_PATTERN_SIZE, // 13
```

```
                    kCGPatternTilingConstantSpacing,// 14
```

```
                    true, // 15
```

```
                    &callbacks);// 16
```

```
 
```

```
    CGContextSetFillPattern (myContext, pattern, &alpha);// 17
```

```
    CGPatternRelease (pattern);// 18
```

```
    CGContextFillRect (myContext, rect);// 19
```

```
    CGContextRestoreGState (myContext);
```

```
}
```
Here’s what the code does:

Declares storage for a CGPattern object that is created later.

Declares storage for a pattern color space that is created later.

Declares a variable for alpha and sets it to `1`, which specifies the opacity of the pattern as completely opaque.

Declares variable to hold the height and width of the window. In this example, the pattern is painted over the area of a window.

Declares and fills a callbacks structure, passing `0` as the version and a pointer to a drawing callback function. This example does not provide a release info callback, so that field is set to `NULL`.

Creates a pattern color space object, setting the pattern’s base color space to `NULL`. When you paint a colored pattern, the pattern supplies its own color in the drawing callback, which is why you set the color space to `NULL`.

Sets the fill color space to the pattern color space object you just created.

Releases the pattern color space object.

Passes `NULL` because the pattern does not need any additional information passed to the drawing callback. 

Passes a CGRect object that specifies the bounds of the pattern cell.

Passes a CGAffineTransform matrix that specifies how to translate the pattern space to the default user space of the context in which the pattern is used. This example passes the identity matrix.

Passes the horizontal pattern size as the horizontal displacement between the start of each cell. In this example, one cell is painted adjacent to the next.

Passes the vertical pattern size as the vertical displacement between start of each cell. 

Passes the constant `kCGPatternTilingConstantSpacing` to specify how Quartz should render the pattern. For more information, see [Tiling](#//apple_ref/doc/uid/TP30001066-CH206-BBCDHCEH).

Passes `true` for the `isColored` parameter, to specify that the pattern is a colored pattern.

Passes a pointer to the callbacks structure that contains version information, and a pointer to your drawing callback function.

Sets the fill pattern, passing the context, the CGPattern object you just created, and a pointer to the alpha value that specifies an opacity for Quartz to apply to the pattern.

Releases the CGPattern object.

Fills a rectangle that is the size of the window passed to the `MyColoredPatternPainting` routine. Quartz fills the rectangle using the pattern you just set up.

## Painting Stencil Patterns
The five steps you need to perform to paint a stencil pattern are described in the following sections: 

[Write a Callback Function That Draws a Stencil Pattern Cell](#//apple_ref/doc/uid/TP30001066-CH206-BBCBGDFH)

[Set Up the Stencil Pattern Color Space](#//apple_ref/doc/uid/TP30001066-CH206-BBCHCBDI)

[Set Up the Anatomy of the Stencil Pattern](#//apple_ref/doc/uid/TP30001066-CH206-BBCCIGGF)

[Specify the Stencil Pattern as a Fill or Stroke Pattern](#//apple_ref/doc/uid/TP30001066-CH206-BBCDEJFH)

[Drawing with the Stencil Pattern](#//apple_ref/doc/uid/TP30001066-CH206-BBCHDJBE)

These are actually the same steps you use to paint a colored pattern. The difference between the two is how you set up color information. You can see how all the steps fit together in [A Complete Stencil Pattern Painting Function](#//apple_ref/doc/uid/TP30001066-CH206-BBCGDFGH).

### Write a Callback Function That Draws a Stencil Pattern Cell
The callback you write for drawing a stencil pattern follows the same form as that described for a colored pattern cell. See [Write a Callback Function That Draws a Colored Pattern Cell](#//apple_ref/doc/uid/TP30001066-CH206-BBCEHCFD). The only difference is that your drawing callback does not specify any color. The pattern cell shown in Figure 6-10 does not get its color from the drawing callback. The color is set outside the drawing color in the pattern color space.

**Figure 6-10**  A stencil pattern cellTake a look at the code in Listing 6-5, which draws the pattern cell shown in Figure 6-10. Notice that the code simply creates a path and fills the path. The code does not set color. 

**Listing 6-5**  A drawing callback that draws a stencil pattern cell

```
#define PSIZE 16    // size of the pattern cell
```

```
 
```

```
static void MyDrawStencilStar (void *info, CGContextRef myContext)
```

```
{
```

```
    int k;
```

```
    double r, theta;
```

```
 
```

```
    r = 0.8 * PSIZE / 2;
```

```
    theta = 2 * M_PI * (2.0 / 5.0); // 144 degrees
```

```
 
```

```
    CGContextTranslateCTM (myContext, PSIZE/2, PSIZE/2);
```

```
 
```

```
    CGContextMoveToPoint(myContext, 0, r);
```

```
    for (k = 1; k < 5; k++) {
```

```
        CGContextAddLineToPoint (myContext,
```

```
                    r * sin(k * theta),
```

```
                    r * cos(k * theta));
```

```
    }
```

```
    CGContextClosePath(myContext);
```

```
    CGContextFillPath(myContext);
```

```
}
```
### Set Up the Stencil Pattern Color Space
Stencil patterns require that you set up a pattern color space for Quartz to paint with, as shown in Listing 6-6. A detailed explanation for each numbered line of code follows the listing.

**Listing 6-6**  Code that creates a pattern color space for a stencil pattern

CGPatternRef pattern;
```
CGColorSpaceRef baseSpace;
```

```
CGColorSpaceRef patternSpace;
```

```
 
```

```
baseSpace = CGColorSpaceCreateWithName (kCGColorSpaceGenericRGB);// 1
```

```
patternSpace = CGColorSpaceCreatePattern (baseSpace);// 2
```

```
CGContextSetFillColorSpace (myContext, patternSpace);// 3
```

```
CGColorSpaceRelease(patternSpace);// 4
```

```
CGColorSpaceRelease(baseSpace);// 5
```
Here’s what the code does:

This function creates a generic RGB space. Generic color spaces leave color matching to the system. For more information, see [Creating Generic Color Spaces](../dq_color/dq_color.html#//apple_ref/doc/uid/TP30001066-CH205-BCIEDGFI).

Creates a pattern color space. The color space you supply specifies how colors are represented for the pattern. Later, when you set colors for the pattern, you must set them using the pattern color space. For this example, you will need to specify color using RGB values.

Sets the color space to use when filling a pattern. You can set a stroke color space by calling the function `CGContextSetStrokeColorSpace`.

Releases the pattern color space object.

Releases the base color space object.

### Set Up the Anatomy of the Stencil Pattern
You specify information about the anatomy of a pattern the way you would for a colored pattern—by calling the function `CGPatternCreate`. The only difference is that you pass `false` for the `isColored` parameter. See [Set Up the Anatomy of the Colored Pattern](#//apple_ref/doc/uid/TP30001066-CH206-BBCBFEGD) for more information on the parameters you supply to the `CGPatternCreate` function.

### Specify the Stencil Pattern as a Fill or Stroke Pattern
You can use your pattern for filling or stroking by calling the appropriate function, `CGContextSetFillPattern` or `CGContextSetStrokePattern`. Quartz uses your pattern for any subsequent filling or stroking.

These functions each take three parameters:

The graphics context

The CGPattern object that you created previously

An array of color components

A stencil pattern does not supply a color in the drawing callback, so you must pass a color to the fill or stroke functions to inform Quartz what color to use. Listing 6-7 shows an example of how to set color for a stencil pattern. The values in the color array are interpreted by Quartz in the color space you set up earlier. Because this example uses device RGB, the color array contains values for red, green, and blue components. The fourth value specifies the opacity of the color.

**Listing 6-7**  Code that sets opacity for a colored pattern

```
static const CGFloat color[4] = { 0, 1, 1, 0.5 }; //cyan, 50% transparent
```

```
 
```

```
CGContextSetFillPattern (myContext, myPattern, color);
```
### Drawing with the Stencil Pattern
After you’ve completed the previous steps, you can call any Quartz 2D function that paints. Your pattern is used as the “paint.” For example, you can call `CGContextStrokePath`, `CGContextFillPath`, `CGContextFillRect`, or any other function that paints.

### A Complete Stencil Pattern Painting Function
The code in Listing 6-8 contains a function that paints a stencil pattern. The function incorporates all the steps discussed previously. A detailed explanation for each numbered line of code follows the listing.

**Listing 6-8**  A function that paints a stencil pattern

#define PSIZE 16
```
 
```

```
void MyStencilPatternPainting (CGContextRef myContext,
```

```
                                const Rect *windowRect)
```

```
{
```

```
    CGPatternRef pattern;
```

```
    CGColorSpaceRef baseSpace;
```

```
    CGColorSpaceRef patternSpace;
```

```
    static const CGFloat color[4] = { 0, 1, 0, 1 };// 1
```

```
    static const CGPatternCallbacks callbacks = {0, &drawStar, NULL};// 2
```

```
 
```

```
    baseSpace = CGColorSpaceCreateDeviceRGB ();// 3
```

```
    patternSpace = CGColorSpaceCreatePattern (baseSpace);// 4
```

```
    CGContextSetFillColorSpace (myContext, patternSpace);// 5
```

```
    CGColorSpaceRelease (patternSpace);
```

```
    CGColorSpaceRelease (baseSpace);
```

```
    pattern = CGPatternCreate(NULL, CGRectMake(0, 0, PSIZE, PSIZE),// 6
```

```
                  CGAffineTransformIdentity, PSIZE, PSIZE,
```

```
                  kCGPatternTilingConstantSpacing,
```

```
                  false, &callbacks);
```

```
    CGContextSetFillPattern (myContext, pattern, color);// 7
```

```
    CGPatternRelease (pattern);// 8
```

```
    CGContextFillRect (myContext,CGRectMake (0,0,PSIZE*20,PSIZE*20));// 9
```

```
}
```
Here’s what the code does:

Declares an array to hold a color value and sets the value (which will be in RGB color space) to opaque green.

Declares and fills a callbacks structure, passing `0` as the version and a pointer to a drawing callback function. This example does not provide a release info callback, so that field is set to `NULL`.

Creates an RGB device color space. If the pattern is drawn to a display, you need to supply this type of color space.

Creates a pattern color space object from the RGB device color space. 

Sets the fill color space to the pattern color space object you just created.

Creates a pattern object. Note that the second to last parameter—the `isColored` parameter—is `false`. Stencil patterns do not supply color, so you must pass `false` for this parameter. All other parameters are similar to those passed for the colored pattern example. See [A Complete Colored Pattern Painting Function](#//apple_ref/doc/uid/TP30001066-CH206-BBCGHGAG).

Sets the fill pattern, passing the color array declared previously.

Releases the CGPattern object.

Fills a rectangle. Quartz fills the rectangle using the pattern you just set up.

        
            [Next](../dq_shadows/dq_shadows.html)[Previous](../dq_affine/dq_affine.html)
        
         Copyright &#x00a9; 2001, 2017 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2017-03-21