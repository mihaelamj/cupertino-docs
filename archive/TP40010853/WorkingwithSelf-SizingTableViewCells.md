---
title: "Working with Self-Sizing Table View Cells"
book: "Auto Layout Guide"
chapterId: "TP40010853-CH25"
date: "2016-03-21"
description: "Describes the constraint-based system for laying out user interface elements."
identifier: "//apple_ref/doc/uid/TP40010853"
platforms: "OS X,iOS,tvOS"
source: apple-archive
---

# Working with Self-Sizing Table View Cells

> **Auto Layout Guide**
> Last updated: 2016-03-21

## Working with Self-Sizing Table View Cells

  
  	
  		
  In iOS, you can use Auto Layout to define the height of a table view cell; however, the feature is not enabled by default.

  Normally, a cell’s height is determined by the table view delegate’s `[tableView:heightForRowAtIndexPath:](https://developer.apple.com/documentation/uikit/uitableviewdelegate/1614998-tableview)` method. To enable self-sizing table view cells, you must set the table view’s `[rowHeight](https://developer.apple.com/documentation/uikit/uitableview/1614852-rowheight)` property to `[UITableViewAutomaticDimension](https://developer.apple.com/documentation/uikit/uitableviewautomaticdimension)`. You must also assign a value to the `[estimatedRowHeight](https://developer.apple.com/documentation/uikit/uitableview/1614925-estimatedrowheight)` property. As soon as both of these properties are set, the system uses Auto Layout to calculate the row’s actual height. 

  
  
      
        

            - `tableView.estimatedRowHeight = 85.0`

            - `tableView.rowHeight = UITableViewAutomaticDimension`

        

      
  

  Next, lay out the table view cell’s content within the cell’s content view. To define the cell’s height, you need an unbroken chain of constraints and views (with defined heights) to fill the area between the content view’s top edge and its bottom edge. If your views have intrinsic content heights, the system uses those values. If not, you must add the appropriate height constraints, either to the views or to the content view itself.

  
  
  

  Additionally, try to make the estimated row height as accurate as possible. The system calculates items such as the scroll bar heights based on these estimates. The more accurate the estimates, the more seamless the user experience becomes.

  
  
> 
    Note
    
    	When working with table view cells, you cannot change the layout of the predefined content (for example, the `[textLabel](https://developer.apple.com/documentation/uikit/uitableviewcell/1623210-textlabel)`, `[detailTextLabel](https://developer.apple.com/documentation/uikit/uitableviewcell/1623273-detailtextlabel)`, and `[imageView](https://developer.apple.com/documentation/uikit/uitableviewcell/1623270-imageview)` properties).
    	
    
  The following constraints are supported:

  
  Constraints that position your subview relative to the cell’s content view.

  Constraints that position your subview relative to the cell’s bounds.

  Constraints that position your subview relative to the predefined content.

  

		 

  
  	
 	
    		[Working with Scroll Views](WorkingwithScrollViews.html#//apple_ref/doc/uid/TP40010853-CH24-SW1)

  			[Changing Constraints](ModifyingConstraints.html#//apple_ref/doc/uid/TP40010853-CH29-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2016-03-21](RevisionHistory.html#//apple_ref/doc/uid/TP40010853-CH99-SW1)