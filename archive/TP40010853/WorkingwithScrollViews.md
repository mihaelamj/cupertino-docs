---
title: "Working with Scroll Views"
book: "Auto Layout Guide"
chapterId: "TP40010853-CH24"
date: "2016-03-21"
description: "Describes the constraint-based system for laying out user interface elements."
identifier: "//apple_ref/doc/uid/TP40010853"
platforms: "OS X,iOS,tvOS"
source: apple-archive
---

# Working with Scroll Views

> **Auto Layout Guide**
> Last updated: 2016-03-21

## Working with Scroll Views

  
  	
  		
  When working with scroll views, you need to define both the size and position of the scroll view’s frame within its superview, and the size of the scroll view’s content area. All of these features can be set using Auto Layout.

  To support scroll views, the system interprets constraints differently, depending on where the constraints are located.

  
  Any constraints between the scroll view and objects outside the scroll view attach to the scroll view’s frame, just as with any other view.

  For constraints between the scroll view and its content, the behavior varies depending on the attributes being constrained:

  
  Constraints between the edges or margins of the scroll view and its content attach to the scroll view’s content area.

  Constraints between the height, width, or centers attach to the scroll view’s frame.

  You can also use constraints between the scroll view’s content and objects outside the scroll view to provide a fixed position for the scroll view’s content, making that content appear to float over the scroll view.

  For most common layout tasks, the logic becomes far easier if you use a dummy view or layout group to contain the scroll view’s content. When working in Interface Builder, the general approach is shown below:

  
  Add the scroll view to the scene.

  Draw constraints to define the scroll view’s size and position, as normal.

  Add a view to the scroll view. Set the view’s Xcode specific label to Content View.

  Pin the content view’s top, bottom, leading, and trailing edges to the scroll view’s corresponding edges. The content view now defines the scroll view’s content area.

  
  
> 
    Remember
    
    	The content view does not have a fixed size at this point. It can stretch and grow to fit any views and controls you place inside it.
    	
    
  

  (Optional) To disable horizontal scrolling, set the content view’s width equal to the scroll view’s width. The content view now fills the scroll view horizontally.

  (Optional) To disable vertical scrolling, set the content view’s height equal to the scroll view’s height. The content view now fills the scroll view horizontally.

  Lay out the scroll view’s content inside the content view. Use constraints to position the content inside the content view as normal.

  
  
> 
    Important
    
    	Your layout must fully define the size of the content view (except where defined in steps 5 and 6). To set the height based on the intrinsic size of your content, you must have an unbroken chain of constraints and views stretching from the content view’s top edge to its bottom edge. Similarly, to set the width, you must have an unbroken chain of constraints and views from the content view’s leading edge to its trailing edge.
    	
    
  If your content does not have an intrinsic content size, you must add the appropriate size constraints, either to the content view or to the content.

  When the content view is taller than the scroll view, the scroll view enables vertical scrolling. When the content view is wider than the scroll view, the scroll view enables horizontal scrolling. Otherwise, scrolling is disabled by default.

  

		 

  
  	
 	
    		[Size-Class-Specific Layout](Size-ClassSpecificLayout.html#//apple_ref/doc/uid/TP40010853-CH26-SW1)

  			[Working with Self-Sizing Table View Cells](WorkingwithSelf-SizingTableViewCells.html#//apple_ref/doc/uid/TP40010853-CH25-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2016-03-21](RevisionHistory.html#//apple_ref/doc/uid/TP40010853-CH99-SW1)