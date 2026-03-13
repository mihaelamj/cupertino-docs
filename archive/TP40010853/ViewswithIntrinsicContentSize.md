---
title: "Views with Intrinsic Content Size"
book: "Auto Layout Guide"
chapterId: "TP40010853-CH13"
date: "2016-03-21"
description: "Describes the constraint-based system for laying out user interface elements."
identifier: "//apple_ref/doc/uid/TP40010853"
platforms: "OS X,iOS,tvOS"
source: apple-archive
---

# Views with Intrinsic Content Size

> **Auto Layout Guide**
> Last updated: 2016-03-21

## Views with Intrinsic Content Size

  
  	
  		
  The following recipes demonstrate working with views that have an intrinsic content size. In general, the intrinsic content size simplifies the layout, reducing the number of constraints you need. However, using the intrinsic content size often requires setting the view’s content-hugging and compression-resistance (CHCR) priorities, which can add additional complications.

  To view the source code for these recipes, see the [Auto Layout Cookbook](https://developer.apple.com/sample-code/xcode/downloads/Auto-Layout-Cookbook.zip) project.

		 

  
  
  ### Simple Label and Text Field

  
  This recipe demonstrates laying out a simple label and text field pair. In this example, the label’s width is based on the size of its text property, and the text field expands and shrinks to fit the remaining space. 

  
  
  

  Because this recipe uses the view’s intrinsic content size, you need only five constraints to uniquely specify the layout. However, you must make sure you have the correct CHCR priorities to get the correct resizing behavior.

  For more information on intrinsic content sizes and CHCR priorities, see [Intrinsic Content Size](AnatomyofaConstraint.html#//apple_ref/doc/uid/TP40010853-CH9-SW21).

  
  
  ### Views and Constraints

  
  In Interface Builder, drag out a label and a text field. Set the label’s text and the text field’s placeholder, and then set up the constraints as shown.

  
  
  

  
  `Name Label.Leading = Superview.LeadingMargin`

  `Name Text Field.Trailing = Superview.TrailingMargin`

  `Name Text Field.Leading = Name Label.Trailing + Standard`

  `Name Text Field.Top = Top Layout Guide.Bottom + 20.0`

  `Name label.Baseline = Name Text Field.Baseline`

  

  
  ### Attributes

  
  To have the text field stretch to fill the available space, its content hugging must be lower than the label’s. By default, Interface Builder should set the label’s content hugging to 251, and the text field to 250. You can verify this in the Size inspector.

  
  
    
    
        
            
  Name

            
  Horizontal hugging

            
  Vertical hugging

            
  Horizontal resistance

            
  Vertical resistance

        
    
    
        
            
              
  Name Label

            
            
  251

            
  251

            
  750

            
  750

        
        
            
              
  Name Text Field

            
            
  250

            
  250

            
  750

            
  750

        
    
  

  

  
  ### Discussion

  
  Notice that this layout only uses two constraints (4 and 5) to define the vertical layout, and three constraints (1, 2, and 3) to define the horizontal layout. The rule of thumb from [Creating Nonambiguous, Satisfiable Layouts](AnatomyofaConstraint.html#//apple_ref/doc/uid/TP40010853-CH9-SW16) states that we need two horizontal and two vertical constraints per view; however, the label and text field’s intrinsic content size provides their heights and the label’s widths, removing the need for three constraints.

  This layout also makes the simplifying assumption that the text field is always taller than the label text, and it uses the text field’s height to define the distance from the top layout guide. Because both the label and text field are used to display text, the recipe aligns them using the text’s baseline.

  Horizontally, you still need to define which view should expand to fill the available size. You do this by modifying the view’s CHCR priorities. In this example, Interface Builder should already set the name label’s horizontal and vertical hugging priority to 251. Because this is larger than the text field’s default 250, the text field expands to fill any additional space.

  
  
> 
    Note
    
    	If the layout might be displayed in a space that is too small for the controls, you need to modify the compression resistances values as well. The compression resistance defines which of the views should be truncated when there isn’t enough space.
    	
    
  In this example, modifying the compression resistances is left as an exercise for the reader. If the name label’s text or font is large enough; however, there won’t be enough space, producing an ambiguous layout. The system then selects a constraint to break, and either the text field or the label is truncated.

  Ideally, you want to create a layout that is never too big for the available space—using alternative layouts for compact size classes as necessary. However, when designing for views that support multiple languages and dynamic type, it is difficult to predict exactly how big your rows might become. Modifying the compression resistance is a good safety valve, just in case.

  

  

  
  ### Dynamic Height Label and Text Field

  
  The [Simple Label and Text Field](#//apple_ref/doc/uid/TP40010853-CH13-SW8) recipe simplified the layout logic by assuming that the text field would always be taller than the name label. That, however, is not necessarily always true. If you increase the label’s font size enough, it will extend above the text field.

  This recipe dynamically sets the vertical spacing of the controls based on the tallest control at runtime. With the regular system font, this recipe appears to be identical to the [Simple Label and Text Field](#//apple_ref/doc/uid/TP40010853-CH13-SW8) recipe (see the screenshot). However, if you increase the label’s font size to 36.0 points, then the layout’s vertical spacing is calculated from the top of the label instead.

  
  
  

  This is a somewhat contrived example. After all, if you increase the font size of a label, you would typically also increase the font size of the text field. However, given the extra, extra, extra large fonts available through the iPhone’s accessibility settings, this technique can prove useful when mixing dynamic type and fixed-sized controls (like images).

  
  
  ### Views and Constraints

  
  Set up the view hierarchy as you did in [Simple Label and Text Field](#//apple_ref/doc/uid/TP40010853-CH13-SW8), but use a somewhat more complex set of constraints:

  
  
  

  
  `Name Label.Leading = Superview.LeadingMargin`

  `Name Text Field.Trailing = Superview.TrailingMargin`

  `Name Text Field.Leading = Name Label.Trailing + Standard`

  `Name Label.Top >= Top Layout Guide.Bottom + 20.0`

  `Name Label.Top = Top Layout Guide.Bottom + 20.0 (Priority 249)`

  `Name Text Field.Top >= Top Layout Guide.Bottom + 20.0`

  `Name Text Field.Top = Top Layout Guide.Bottom + 20.0 (Priority 249)`

  `Name label.Baseline = Name Text Field.Baseline`

  

  
  ### Attributes

  
  To have the text field stretch to fill the available space, it’s content hugging must be lower than the label’s. By default, Interface Builder should set the label’s content hugging to 251, and the text field to 250. You can verify this in the Size inspector.

  
  
    
    
        
            
  Name

            
  Horizontal hugging

            
  Vertical hugging

            
  Horizontal resistance

            
  Vertical resistance

        
    
    
        
            
              
  Name Label

            
            
  251

            
  251

            
  750

            
  750

        
        
            
              
  Name Text Field

            
            
  250

            
  250

            
  750

            
  750

        
    
  

  

  
  ### Discussion

  
  This recipe uses a pair of constraints for each control. A required, greater-than-or-equal constraint defines the minimum distance between that control and the layout guide, while an optional constraint tries to pull the control to exactly 20.0 points from the layout guide.

  Both constraints are satisfiable for the taller constraint, so the system places it exactly 20.0 points from the layout guide. However, for the shorter control, only the minimum distance is satisfiable. The other constraint is ignored. This lets the Auto Layout system dynamically recalculate the layout as the height of the controls change at runtime.

  
  
> 
    Note
    
    	Be sure to set the priority of the optional constraints to a value that is lower than the default content hugging constraint (250). Otherwise, the system breaks the content hugging constraint and stretches the view instead of repositioning it. 
    	
    
  This can be particularly confusing when working with layouts that use baseline alignments, because the baseline alignments are only valid if the text views are displayed at their intrinsic content height. If the system resizes one of the views, the text may not line up properly, despite having a required baseline constraint.

  

  

  
  ### Fixed Height Columns

  
  This recipe extends the [Simple Label and Text Field](#//apple_ref/doc/uid/TP40010853-CH13-SW8) recipe into columns of labels and text fields. Here, the trailing edge of all the labels are aligned. The text fields leading and trailing edges are aligned, and the horizontal placement is based on the longest label. Like the Simple Label and Text Field recipe, however, this recipe simplifies the layout logic by assuming that the text fields are always taller than the labels.

  
  
  

  
  
  ### Views and Constraints

  
  Lay out the labels and text fields, and then set the constraints as shown.

  
  
  

  
  `First Name Label.Leading = Superview.LeadingMargin`

  `Middle Name Label.Leading = Superview.LeadingMargin`

  `Last Name Label.Leading = Superview.LeadingMargin`

  `First Name Text Field.Leading = First Name Label.Trailing + Standard`

  `Middle Name Text Field.Leading = Middle Name Label.Trailing + Standard`

  `Last Name Text Field.Leading = Last Name Label.Trailing + Standard`

  `First Name Text Field.Trailing = Superview.TrailingMargin`

  `Middle Name Text Field.Trailing = Superview.TrailingMargin`

  `Last Name Text Field.Trailing = Superview.TrailingMargin`

  `First Name Label.Baseline = First Name Text Field.Baseline`

  `Middle Name Label.Baseline = Middle Name Text Field.Baseline`

  `Last Name Label.Baseline = Last Name Text Field.Baseline`

  `First Name Text Field.Width = Middle Name Text Field.Width`

  `First Name Text Field.Width = Last Name Text Field.Width`

  `First Name Text Field.Top = Top Layout Guide.Bottom + 20.0`

  `Middle Name Text Field.Top = First Name Text Field.Bottom + Standard`

  `Last Name Text Field.Top = Middle Name Text Field.Bottom + Standard`

  

  
  ### Attributes

  
  In the Attributes inspector, set the following attributes. In particular, right align the text in all the labels. This lets you use labels that are longer than their text, and still line up the edges beside the text fields.

  
  
    
    
        
            
  View

            
  Attribute

            
  Value

        
    
    
        
            
              
  First Name Label

            
            
  Text

            
  First Name

        
        
            
              
  First Name Label

            
            
  Alignment

            
  Right

        
        
            
              
  First Name Text Field

            
            
  Placeholder

            
  Enter first name

        
        
            
              
  Middle Name Label

            
            
  Text

            
  Middle Name

        
        
            
              
  Middle Name Label

            
            
  Alignment

            
  Right

        
        
            
              
  Middle Name Text Field

            
            
  Placeholder

            
  Enter middle name

        
        
            
              
  Last Name Label

            
            
  Text

            
  Last Name

        
        
            
              
  Last Name Label

            
            
  Alignment

            
  Right

        
        
            
              
  Last Name Text Field

            
            
  Placeholder

            
  Enter last name

        
    
  

  For each pair, the label’s content hugging must be higher than the text fields. Again, Interface Builder should do this automatically; however, you can verify these priorities in the Size inspector.

  
  
    
    
        
            
  Name

            
  Horizontal hugging

            
  Vertical hugging

            
  Horizontal resistance

            
  Vertical resistance

        
    
    
        
            
              
  First Name Label

            
            
  251

            
  251

            
  750

            
  750

        
        
            
              
  First Name Text Field

            
            
  250

            
  250

            
  750

            
  750

        
        
            
              
  Middle Name Label

            
            
  251

            
  251

            
  750

            
  750

        
        
            
              
  Middle Name Text Field

            
            
  250

            
  250

            
  750

            
  750

        
        
            
              
  Last Name Label

            
            
  251

            
  251

            
  750

            
  750

        
        
            
              
  Last Name Text Field

            
            
  250

            
  250

            
  750

            
  750

        
    
  

  

  
  ### Discussion

  
  This recipe basically starts as three copies of the [Simple Label and Text Field](#//apple_ref/doc/uid/TP40010853-CH13-SW8) layout, stacked one on top of the other. However, you need to make a few additions so that the rows line up properly.

  First, simplify the problem by right aligning the text for each of the labels. You can now make all the labels the same width, and regardless of the length of the text, you can easily align their trailing edges. Additionally, because a label’s compression resistance is greater than its content hugging, all the labels prefer to be stretched rather than squeezed. Align the leading and trailing edges, and the labels all naturally stretch to the intrinsic content size of the longest label.

  Therefore, you just need to align the leading and trailing edge of all the labels. You also need to align the leading and trailing edges of all the text fields. Fortunately, the labels’ leading edges are already aligned with the superview’s leading margin. Similarly, the text fields’ trailing edges are all aligned with the superview’s trailing margin. You just need to line up one of the other two edges, and because all the rows are the same width, everything will be aligned. 

  There are a number of ways to do this. For this recipe, give each of the text fields an equal width.

  

  
  ### Dynamic Height Columns 

  
  This recipe combines everything you learned in the [Dynamic Height Label and Text Field](#//apple_ref/doc/uid/TP40010853-CH13-SW16) recipe and the [Fixed Height Columns](#//apple_ref/doc/uid/TP40010853-CH13-SW24) recipe. The goals of this recipe include:

  
  The trailing edge of the labels are aligned, based on the length of the longest label.

  The text fields are the same width, and their leading and trailing edges are aligned.

  The text fields expand to fill all the remaining space in the superview.

  The height between rows are based on the tallest element in the row.

  Everything is dynamic, so if the font size or label text changes, the layout automatically updates.

  
  
  

  
  
  ### Views and Constraints

  
  Layout the labels and text fields as you did in Fixed Height Columns; however, you need a few additional constraints.

  
  
  

  
  `First Name Label.Leading = Superview.LeadingMargin`

  `Middle Name Label.Leading = Superview.LeadingMargin`

  `Last Name Label.Leading = Superview.LeadingMargin`

  `First Name Text Field.Leading = First Name Label.Trailing + Standard`

  `Middle Name Text Field.Leading = Middle Name Label.Trailing + Standard`

  `Last Name Text Field.Leading = Last Name Label.Trailing + Standard`

  `First Name Text Field.Trailing = Superview.TrailingMargin`

  `Middle Name Text Field.Trailing = Superview.TrailingMargin`

  `Last Name Text Field.Trailing = Superview.TrailingMargin`

  `First Name Label.Baseline = First Name Text Field.Baseline`

  `Middle Name Label.Baseline = Middle Name Text Field.Baseline`

  `Last Name Label.Baseline = Last Name Text Field.Baseline`

  `First Name Text Field.Width = Middle Name Text Field.Width`

  `First Name Text Field.Width = Last Name Text Field.Width`

  `First Name Label.Top >= Top Layout Guide.Bottom + 20.0`

  `First Name Label.Top = Top Layout Guide.Bottom + 20.0 (Priority 249)`

  `First Name Text Field.Top >= Top Layout Guide.Bottom + 20.0`

  `First Name Text Field.Top = Top Layout Guide.Bottom + 20.0 (Priority 249)`

  `Middle Name Label.Top >= First Name Label.Bottom + Standard`

  `Middle Name Label.Top = First Name Label.Bottom + Standard (Priority 249)`

  `Middle Name Text Field.Top >= First Name Text Field.Bottom + Standard`

  `Middle Name Text Field.Top = First Name Text Field.Bottom + Standard (Priority 249)`

  `Last Name Label.Top >= Middle Name Label.Bottom + Standard`

  `Last Name Label.Top = Middle Name Label.Bottom + Standard (Priority 249)`

  `Last Name Text Field.Top >= Middle Name Text Field.Bottom + Standard`

  `Last Name Text Field.Top = Middle Name Text Field.Bottom + Standard (Priority 249)`

  

  
  ### Attributes

  
  In the Attributes inspector, set the following attributes. In particular, right align the text in all the labels. Right aligning the labels lets you use labels that are longer than their text, and the edge of the text still lines up beside the text fields.

  
  
    
    
        
            
  View

            
  Attribute

            
  Value

        
    
    
        
            
              
  First Name Label

            
            
  Text

            
  First Name

        
        
            
              
  First Name Label

            
            
  Alignment

            
  Right

        
        
            
              
  First Name Text Field

            
            
  Placeholder

            
  Enter first name

        
        
            
              
  Middle Name Label

            
            
  Text

            
  Middle Name

        
        
            
              
  Middle Name Label

            
            
  Alignment

            
  Right

        
        
            
              
  Middle Name Text Field

            
            
  Placeholder

            
  Enter middle name

        
        
            
              
  Last Name Label

            
            
  Text

            
  Last Name

        
        
            
              
  Last Name Label

            
            
  Alignment

            
  Right

        
        
            
              
  Last Name Text Field

            
            
  Placeholder

            
  Enter last name

        
    
  

  For each pair, the label’s content hugging must be higher than the text fields. Again, Interface Builder should do this automatically; however, you can verify these priorities in the Size inspector.

  
  
    
    
        
            
  Name

            
  Horizontal hugging

            
  Vertical hugging

            
  Horizontal resistance

            
  Vertical resistance

        
    
    
        
            
              
  First Name Label

            
            
  251

            
  251

            
  750

            
  750

        
        
            
              
  First Name Text Field

            
            
  250

            
  250

            
  750

            
  750

        
        
            
              
  Middle Name Label

            
            
  251

            
  251

            
  750

            
  750

        
        
            
              
  Middle Name Text Field

            
            
  250

            
  250

            
  750

            
  750

        
        
            
              
  Last Name Label

            
            
  251

            
  251

            
  750

            
  750

        
        
            
              
  Last Name Text Field

            
            
  250

            
  250

            
  750

            
  750

        
    
  

  

  
  ### Discussion

  
  This recipe simply combines the techniques described in the [Dynamic Height Label and Text Field](#//apple_ref/doc/uid/TP40010853-CH13-SW16) and [Fixed Height Columns](#//apple_ref/doc/uid/TP40010853-CH13-SW24) recipes. Like the Dynamic Height Label and Text Field recipe, this recipe uses pairs of constraints to dynamically set the vertical spacing between the rows. Like the Fixed Height Columns recipe, it uses right-aligned text in the labels, and explicit equal width constraints to line up the columns.

  
  
> 
    Note
    
    	This example uses a 20.0-point space for the distance between views and the top layout guide, and an 8.0-space between sibling views. This has the effect of setting a fixed, 20-point top margin. If you want a margin that adjusts automatically to the presence or absence of bars—you must add additional constraints. The general technique is shown in the [Adaptive Single View](WorkingwithSimpleConstraints.html#//apple_ref/doc/uid/TP40010853-CH12-SW4) recipe. However, the exact implementation is left as a challenge for the reader.
    	
    
  

  As you can see, the layout’s logic is beginning to grow somewhat complex; however, there are a couple of ways you can simplify things. First, as mentioned earlier, you should use stack views wherever possible. Alternatively, you can group controls, and then lay out the groups. This lets you break up a single, complex layout into smaller, more manageable chunks.

  

  
  ### Two Equal-Width Buttons

  
  This recipe demonstrates laying out two equal sized buttons. Vertically, the buttons are aligned with the bottom of the screen. Horizontally, they are stretched so that they fill all the available space.

  
  
  

  
  
  ### Views and Constraints

  
  In Interface Builder, drag two buttons onto the scene. Align them using the guidelines along the bottom of the scene. Don’t worry about making the buttons the same width—just stretch one of them to fill the remaining horizontal space. After they are roughly in place, set the following constraints. Auto Layout will calculate their correct, final position.

  
  
  

  
  `Short Button.Leading = Superview.LeadingMargin`

  `Long Button.Leading = Short Button.Trailing + Standard`

  `Long Button.Trailing = Superview.TrailingMargin`

  `Bottom Layout Guide.Top = Short Button.Bottom + 20.0`

  `Bottom Layout Guide.Top = Long Button.Botton + 20.0`

  `Short Button.Width = Long Button.Width`

  

  
  ### Attributes

  
  Give the buttons a visible background color, to make it easier to see how their frames change as the device rotates. Additionally, use different length titles for the buttons, to show that the button title does not affect the button’s width.

  
  
    
    
        
            
  View

            
  Attribute

            
  Value

        
    
    
        
            
              
  Short Button

            
            
  Background

            
  Light Gray Color

        
        
            
              
  Short Button

            
            
  Title

            
  short

        
        
            
              
  Long Button

            
            
  Background

            
  Light Gray Color

        
        
            
              
  Long Button

            
            
  Title

            
  Much Longer Button Title

        
    
  

  

  
  ### Discussion

  
  This recipe uses the buttons’ intrinsic height, but not their width when calculating the layout. Horizontally, the buttons are explicitly sized so that they have an equal width and fill the available space. To see how the button’s intrinsic height affects the layout, compare this recipe with the [Two Equal-Width Views](WorkingwithSimpleConstraints.html#//apple_ref/doc/uid/TP40010853-CH12-SW17) recipe. In this recipe, there are only two vertical constraints, not four.

  The buttons are also given titles with very different lengths to help illustrate how the button’s text affects (or in this case, does not affect) the layout.

  
  
> 
    Note
    
    	For this recipe, the buttons have been given a light gray background color, letting you see their frames. Typically, buttons and labels have transparent backgrounds, making it difficult (if not impossible) to see any changes to their frame.
    	
    
  

  

  
  ### Three Equal-Width Buttons

  
  This recipe extends the [Two Equal-Width Buttons](#//apple_ref/doc/uid/TP40010853-CH13-SW4) recipe, so that it uses three equal-width buttons.

  
  
  

  
  
  ### Views and Constraints

  
  Lay out the buttons and set the constraints as shown.

  
  
  

  
  `Short Button.Leading = Superview.LeadingMargin`

  `Medium Button.Leading = Short Button.Trailing + Standard`

  `Long Button.Leading = Medium Button.Trailing + Standard`

  `Long Button.Trailing = Superview.TrailingMargin`

  `Bottom Layout Guide.Top = Short Button.Bottom + 20.0`

  `Bottom Layout Guide.Top = Medium Button.Bottom + 20.0`

  `Bottom Layout Guide.Top = Long Button.Bottom + 20.0`

  `Short Button.Width = Medium Button.Width`

  `Short Button.Width = Long Button.Width`

  

  
  ### Attributes

  
  Give the buttons a visible background color, to make it easier to see how their frames change as the device rotates. Additionally, use different length titles for the buttons, to show that the button title does not affect the button’s width.

  
  
    
    
        
            
  Para

            
  View

            
  Attribute

            
  Value

        
    
    
        
            
              
  Short Button

            
            
  Background

            
  Light Gray Color

        
        
            
              
  Short Button

            
            
  Title

            
  Short

        
        
            
              
  Medium Button

            
            
  Background

            
  Light Gray Color

        
        
            
              
  Medium Button

            
            
  Title

            
  Medium

        
        
            
              
  Long Button

            
            
  Background

            
  Light Gray Color

        
        
            
              
  Long Button

            
            
  Title

            
  Long Button Title

        
    
  

  

  
  ### Discussion

  
  Adding an extra button requires adding three extra constraints (two horizontal constraints and one vertical constraints). Remember, you are not using the button’s intrinsic width, so you need at least two horizontal constraints to uniquely specify both its position and its size. However, you are using the button’s intrinsic height, so you only need one additional constraint to specify its vertical position.

  
  
> 
    Note
    
    	To quickly set the equal width constraints, select all three buttons, then use Interface Builder’s Pin tool to create an equal width constraint. Interface Builder automatically creates all both required constraints.
    	
    
  

  

  
  ### Two Buttons with Equal Spacing

  
  Superficially, this recipe resembles the [Two Equal-Width Buttons](#//apple_ref/doc/uid/TP40010853-CH13-SW4) recipe (see Screenshot). However, in this recipe, the buttons’ widths are based on the longest title. If there’s enough space, the buttons are stretched only until they both match the intrinsic content size of the longer button. Any additional space is divided evenly around the buttons.

  
  
  

  On the iPhone, the Two Equal-Width Buttons and Two Buttons with Equal Spacing layouts appear nearly identical in the portrait orientation. The difference becomes obvious only when you rotate the device into a landscape orientation (or use a larger device, like an iPad).

  
  
  ### Views and Constraints

  
  In Interface Builder, drag out and position two buttons and three view objects. Position the buttons between the views, and then set the constraints as shown.

  
  
  

  
  `Leading Dummy View.Leading = Superview.LeadingMargin`

  `Short Button.Leading = Leading Dummy View.Trailing`

  `Center Dummy View.Leading = Short Button.Trailing`

  `Long Button.Leading = Center Dummy View.Trailing`

  `Trailing Dummy View.Leading = Long Button.Trailing`

  `Trailing Dummy View.Trailing = Superview.TrailingMargin`

  `Bottom Layout Guide.Top = Leading Dummy View.Bottom + 20.0`

  `Bottom Layout Guide.Top = Short Button.Bottom + 20.0`

  `Bottom Layout Guide.Top = Center Dummy View.Bottom + 20.0`

  `Bottom Layout Guide.Top = Long Button.Bottom + 20.0`

  `Bottom Layout Guide.Top = Trailing Dummy View.Bottom + 20.0`

  `Short Button.Leading >= Superview.LeadingMargin`

  `Long Button.Leading >= Short Button.Trailing + Standard`

  `Superview.TrailingMargin >= Long Button.Trailing`

  `Leading Dummy View.Width = Center Dummy View.Width`

  `Leading Dummy View.Width = Trailing Dummy View.Width`

  `Short Button.Width = Long Button.Width`

  `Leading Dummy View.Height = 0.0`

  `Center Dummy View.Height = 0.0`

  `Trailing Dummy View.Height = 0.0`

  

  
  ### Attributes

  
  Give the buttons a visible background color, to make it easier to see how their frames change as the device rotates. Additionally, use different length titles for the buttons. The buttons should be sized based on the longest title.

  
  
    
    
        
            
  View

            
  Attribute

            
  Value

        
    
    
        
            
              
  Short Button

            
            
  Background

            
  Light Gray Color

        
        
            
              
  Short Button

            
            
  Title

            
  Short

        
        
            
              
  Long Button

            
            
  Background

            
  Light Gray Color

        
        
            
              
  Long Button

            
            
  Title

            
  Much Longer Button Title

        
    
  

  

  
  ### Discussion

  
  As you can see, the set of constraints has become complex. While this example is designed to demonstrate a specific technique, in a real-world app you should consider using a stack view instead.

  In this example, you want the size of the white space to change as the superview’s frame changes. This means you need a set of equal width constraints to control the white space’s width; however, you cannot create constraints on empty space. There must be an object of some sort whose size you can constrain.

  In this recipe, you use dummy views to represent the empty space. These views are empty instances of the `[UIView](https://developer.apple.com/documentation/uikit/uiview)` class. In this recipe, they are given a 0-point height to minimize their effect on the view hierarchy.

  
  
> 
    Note
    
    	Dummy views can add a significant cost to your layout, so you should use them judiciously. If these views are large, their graphic context can consume a considerable amount of memory, even though they don’t contain any meaningful information.
    	
    
  Additionally, these views participate in the view hierarchy’s responder chain. This means they respond to messages, like hit testing, that are sent along the responder chain. If not handled carefully, these views can intercept and respond to these messages, creating hard-to-find bugs.

  

  Alternatively, you could use instances of the `[UILayoutGuide](https://developer.apple.com/documentation/uikit/uilayoutguide)` class to represent the white space. This lightweight class represents a rectangular frame that can participate in Auto Layout constraints. Layout guides do not have a graphic context, and they are not part of the view hierarchy. This makes layout guides ideal for grouping items or defining white space.

  Unfortunately, you cannot add layout guides to a scene in Interface Builder, and mixing programmatically created objects with a storyboard-based scene can become quite complex. As a general rule of thumb, it’s better to use storyboards and Interface Builder, than to use custom layout guides. 

  This recipe uses greater-than-or-equal constraints to set the minimum spacing around the buttons. Required constraints also guarantee that the buttons are always the same width, and the dummy views are also always the same width (though, they can be a different width than the buttons). The rest of the layout is largely managed by the button’s CHCR priorities. If there isn’t enough space, the dummy views collapse to a 0-point width, and the button’s divide the available space amongst themselves (with the standard spacing between them). As the available space increases, the buttons expand until they reach the larger button’s intrinsic width, then the dummy views begin to expand. The dummy views continue to expand to fill any remaining space. 

  

  
  ### Two Buttons with Size Class-Based Layouts

  
  This recipe uses two different sets of constraints. One is installed for the Any-Any layout. These constraints define a pair of equal-width buttons, identical to the [Two Equal-Width Buttons](#//apple_ref/doc/uid/TP40010853-CH13-SW4) recipe.

  The other set of constraints are installed on the Compact-Regular layout. These constraints define a stacked pair of buttons, as shown below.

  
  
  

  The vertically stacked buttons are used on the iPhone in portrait orientation. The horizontal row of buttons is used everywhere else.

  
  
  ### Constraints

  
  Lay out the buttons exactly as you did for the Two Equal-Width Buttons recipe. In the Any-Any size class, set constraints 1 through 6.

  Next, switch Interface Builder’s size class to the Compact-Regular layout.

  
  
  

  Uninstall constraint 2 and constraint 5, and add constraints 7, 8, and 9 as shown.

  
  
  

  
  `Short Button.Leading = Superview.LeadingMargin`

  `Long Button.Leading = Short Button.Trailing + Standard`

  `Long Button.Trailing = Superview.TrailingMargin`

  `Bottom Layout Guide.Top = Short Button.Bottom + 20.0`

  `Bottom Layout Guide.Top = Long Button.Botton + 20.0`

  `Short Button.Width = Long Button.Width`

  `Long Button.Leading = Superview.LeadingMargin`

  `Short Button.Trailing = Superview.TrailingMargin`

  `Long Button.Top = Short Button.Bottom + Standard`

  

  
  ### Attributes

  
  Give the buttons a visible background color, to make it easier to see how their frames change as the device rotates. Additionally, use different length titles for the buttons, to show that the button title does not affect the button’s width.

  
  
    
    
        
            
  View

            
  Attribute

            
  Value

        
    
    
        
            
              
  Short Button

            
            
  Background

            
  Light Gray Color

        
        
            
              
  Short Button

            
            
  Title

            
  short

        
        
            
              
  Long Button

            
            
  Background

            
  Light Gray Color

        
        
            
              
  Long Button

            
            
  Title

            
  Much Longer Button Title

        
    
  

  

  
  ### Discussion

  
  Interface Builder lets you set size-class specific  views, view attributes, and constraints. It allows you to specify different options for three different size classes (Compact, Any, or Regular) for both the width and the height, giving a total of nine different size classes. Four of them correspond to the Final size classes used on devices (Compact-Compact, Compact-Regular, Regular-Compact, and Regular-Regular). The rest are Base size classes, or abstract representations of two or more size classes (Compact-Any, Regular-Any, Any-Compact, Any-Regular, and Any-Any).

  When loading the layout for a given size class, the system loads the most-specific settings for that size class. This means that the Any-Any size class defines the default values used by all views. Compact-Any settings affect all views with a compact width, and the Compact-Regular settings are only used for views with a compact width and a regular height. When the view’s size class changes—for example, when an iPhone rotates from portrait to landscape, the system automatically swaps layouts and animates the change.

  You can use this feature to create different layouts for the different iPhone orientations. You can also use it to create different iPad and iPhone layouts. The size-class-specific customizations can be as broad or as simple as you want. Of course, the more changes you have, the more complex the storyboard becomes, and the harder it is to design and maintain.

  Remember, you need to make sure you have a valid layout for each possible size class, including all the base size classes. As a general rule, it’s usually easiest to pick one layout to be your default layout. Design that layout in the Any-Any size class. Then modify the Final size classes, as needed. Remember, you can both add and remove items in the more specific size classes.

  For more complex layouts, you may want to draw out the 9 x 9 grid of size classes before you begin. Fill in the four corners with the layouts for those size classes. The grid then lets you see which constraints are shared across multiple size classes, and helps you find the best combination of layouts and size classes. 

  For more information on working with size classes, see [Debugging Auto Layout](TypesofErrors.html#//apple_ref/doc/uid/TP40010853-CH22-SW1). 

  

  	
 	
    		[Simple Constraints](WorkingwithSimpleConstraints.html#//apple_ref/doc/uid/TP40010853-CH12-SW1)

  			[Types of Errors](TypesofErrors.html#//apple_ref/doc/uid/TP40010853-CH17-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2016-03-21](RevisionHistory.html#//apple_ref/doc/uid/TP40010853-CH99-SW1)