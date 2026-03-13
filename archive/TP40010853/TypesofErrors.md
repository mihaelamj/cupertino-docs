---
title: "Part III: Debugging Auto Layout"
book: "Auto Layout Guide"
chapterId: "TP40010853-CH17"
date: "2016-03-21"
description: "Describes the constraint-based system for laying out user interface elements."
identifier: "//apple_ref/doc/uid/TP40010853"
platforms: "OS X,iOS,tvOS"
source: apple-archive
---

# Part III: Debugging Auto Layout

> **Auto Layout Guide**
> Last updated: 2016-03-21

## Types of Errors

  
  	
  		
  Errors in Auto Layout can be divided into three main categories:

  
  **Unsatisfiable Layouts**. Your layout has no valid solution. For more information, see [Unsatisfiable Layouts](ConflictingLayouts.html#//apple_ref/doc/uid/TP40010853-CH19-SW1).

  **Ambiguous Layouts**. Your layout has two or more possible solutions. For more information, see [Ambiguous Layouts](AmbiguousLayouts.html#//apple_ref/doc/uid/TP40010853-CH18-SW1).

  **Logical Errors**. There is a bug in your layout logic. For more information, see [Logical Errors](LogicalErrors.html#//apple_ref/doc/uid/TP40010853-CH20-SW1).

  Most of the time, the real problem is just determining what went wrong. You added the constraints you thought you needed, but when you ran the app, things did not turn out as you had hoped.

  Usually, as soon as you understand the problem, the solution is obvious. Remove conflicting constraints, add missing constraints, and adjust tied priorities so that there is a clear winner. Of course, getting to the point where you can easily understand the problem may take some trial and error. Like any skill, it gets easier with practice.

  Sometimes, however, things get more complicated. That’s where the [Debugging Tricks and Tips](DebuggingTricksandTips.html#//apple_ref/doc/uid/TP40010853-CH21-SW1) chapter comes in.

		 

  
  	
 	
    		[Views with Intrinsic Content Size](ViewswithIntrinsicContentSize.html#//apple_ref/doc/uid/TP40010853-CH13-SW1)

  			[Unsatisfiable Layouts](ConflictingLayouts.html#//apple_ref/doc/uid/TP40010853-CH19-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2016-03-21](RevisionHistory.html#//apple_ref/doc/uid/TP40010853-CH99-SW1)