---
title: "Overview of the Table View API"
book: "Table View Programming Guide for iOS"
chapterId: "TP40007451-CH4"
date: "2013-09-18"
description: "Describes how to create and manage table views for applications running iOS."
identifier: "//apple_ref/doc/uid/TP40007451"
source: apple-archive
---

# Overview of the Table View API

> **Table View Programming Guide for iOS**
> Last updated: 2013-09-18

[Next](../TableViewAndDataModel/TableViewAndDataModel.html)[Previous](../TableViewStyles/TableViewCharacteristics.html)
        
        
        [](../index.html)
        
        # Overview of the Table View API
The table view programming interface includes several UIKit classes, two formal [protocols](../../../../General/Conceptual/DevPedia-CocoaCore/Protocol.html#//apple_ref/doc/uid/TP40008195-CH45), and a [category](../../../../General/Conceptual/DevPedia-CocoaCore/Category.html#//apple_ref/doc/uid/TP40008195-CH5) added to a Foundation framework class. 

## Table View
A table view itself is an instance of the `[UITableView](https://developer.apple.com/documentation/uikit/uitableview)` class. You use its methods to configure the appearance of the table view—for example, specifying the default height of rows or providing a subview used as the header for the table. Other methods give you access to the currently selected row as well as specific rows or cells. You can call other methods of `[UITableView](https://developer.apple.com/documentation/uikit/uitableview)` to manage selections, scroll the table view, and insert or delete rows and sections. 

`UITableView` inherits from the `[UIScrollView](https://developer.apple.com/documentation/uikit/uiscrollview)` class, which defines scrolling behavior for views with content larger than the size of the window. `UITableView` redefines the scrolling behavior to allow vertical scrolling only.

## Table View Controller
The `[UITableViewController](https://developer.apple.com/documentation/uikit/uitableviewcontroller)` class manages a table view and adds support for many standard table-related behaviors such as selection management, row editing, table configuration, and others. This additional support is there to minimize the amount of code you have to write to create and initialize your table-based interface. You don’t use this class directly—instead you subclass `UITableViewController` to add custom behaviors. 

## Data Source and Delegate
A `[UITableView](https://developer.apple.com/documentation/uikit/uitableview)` object must have a [delegate and a data source](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14). Following the [Model-View-Controller](../../../../General/Conceptual/DevPedia-CocoaCore/MVC.html#//apple_ref/doc/uid/TP40008195-CH32) design pattern, the data source mediates between the app’s data model (that is, its [model objects](../../../../General/Conceptual/DevPedia-CocoaCore/ModelObject.html#//apple_ref/doc/uid/TP40008195-CH31)) and the table view. The delegate, on the other hand, manages the appearance and behavior of the table view. The data source and the delegate are often (but not necessarily) the same object, and that object is usually a custom subclass of `[UITableViewController](https://developer.apple.com/documentation/uikit/uitableviewcontroller)`. (See [Navigating a Data Hierarchy with Table Views](../TableViewAndDataModel/TableViewAndDataModel.html#//apple_ref/doc/uid/TP40007451-CH5-SW4) for further information.)

The data source adopts the `[UITableViewDataSource](https://developer.apple.com/documentation/uikit/uitableviewdatasource)` protocol. `[UITableViewDataSource](https://developer.apple.com/documentation/uikit/uitableviewdatasource)` has two required methods. The `[tableView:numberOfRowsInSection:](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614931-tableview)` method tells the table view how many rows to display in each section, and the `[tableView:cellForRowAtIndexPath:](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614861-tableview)` method provides the cell to display for each row in the table. Optional methods allow the data source to configure multiple sections, provide headers and/or footers, and support adding, removing, and reordering rows in the table.

The delegate adopts the `[UITableViewDelegate](https://developer.apple.com/documentation/uikit/uitableviewdelegate)` protocol. This protocol has no required methods. It declares methods that allow the delegate to modify visible aspects of the table view, manage selections, support an accessory view, and support editing of individual rows in a table.

An app can make use of the convenience class `[UILocalizedIndexedCollation](https://developer.apple.com/documentation/uikit/uilocalizedindexedcollation)` to help the data source organize the data for indexed lists and display the proper section when users tap an item in the index. The `UILocalizedIndexedCollation` class also localizes section titles.

## Extension to the NSIndexPath Class
Many table view methods use index paths as parameters or return values. An index path identifies a path to a specific node in a tree of nested arrays, and in the Foundation framework it is represented by an `[NSIndexPath](https://developer.apple.com/documentation/foundation/nsindexpath)` object. UIKit declares a [category](../../../../General/Conceptual/DevPedia-CocoaCore/Category.html#//apple_ref/doc/uid/TP40008195-CH5) on `NSIndexPath` with methods that return key paths, locate rows in sections, and construct `[NSIndexPath](https://developer.apple.com/documentation/foundation/nsindexpath)` objects from row and section indexes. For more information, see *NSIndexPath UIKit Additions*. 

## Table View Cells
As noted in [Data Source and Delegate](#//apple_ref/doc/uid/TP40007451-CH4-SW1), the data source must return a cell object for each visible row that a table view displays. These cell objects must inherit from the `[UITableViewCell](https://developer.apple.com/documentation/uikit/uitableviewcell)` class. This class includes methods for managing cell selection and editing, managing accessory views, and configuring the cell. You can [instantiate](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39) cells directly in the standard styles defined by the `[UITableViewCell](https://developer.apple.com/documentation/uikit/uitableviewcell)` class and give these cells content consisting of one or two strings of text and, in some styles, both image and text. Instead of using a cell in a standard style, you can put your own custom subviews in the content view of an “off-the-shelf” cell object. You may also subclass `UITableViewCell` to customize the appearance and behavior of table view cells. These approaches are all discussed in [A Closer Look at Table View Cells](../TableViewCells/TableViewCells.html#//apple_ref/doc/uid/TP40007451-CH7-SW1). 

        
            [Next](../TableViewAndDataModel/TableViewAndDataModel.html)[Previous](../TableViewStyles/TableViewCharacteristics.html)
        
         Copyright &#x00a9; 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-09-18