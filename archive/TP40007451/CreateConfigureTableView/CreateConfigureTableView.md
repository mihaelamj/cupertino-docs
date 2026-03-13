---
title: "Creating and Configuring a Table View"
book: "Table View Programming Guide for iOS"
chapterId: "TP40007451-CH6"
date: "2013-09-18"
description: "Describes how to create and manage table views for applications running iOS."
identifier: "//apple_ref/doc/uid/TP40007451"
source: apple-archive
---

# Creating and Configuring a Table View

> **Table View Programming Guide for iOS**
> Last updated: 2013-09-18

[Next](../TableViewCells/TableViewCells.html)[Previous](../TableViewAndDataModel/TableViewAndDataModel.html)
        
        
        [](../index.html)
        
        # Creating and Configuring a Table View
Your app must present a table view to users before it can manage it in response to taps on rows and other actions. This chapter shows what you must do to create a table view, configure it, and populate it with data. 

Most of the code examples shown in this chapter come from the sample projects *[UITableView Fundamentals for iOS](../../../../../samplecode/TableViewSuite/Introduction/Intro.html#//apple_ref/doc/uid/DTS40007318)* and *[TheElements](../../../../../samplecode/TheElements/Introduction/Intro.html#//apple_ref/doc/uid/DTS40007419)*.

## Basics of Table View Creation
To create a table view, several entities in an app must interact: the view controller, the table view itself, and the table view’s [data source and delegate](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14). The view controller, data source, and delegate are usually the same object. The view controller starts the calling sequence, diagrammed in [Figure 4-1](#//apple_ref/doc/uid/TP40007451-CH6-SW1). 

The view controller creates a `[UITableView](https://developer.apple.com/documentation/uikit/uitableview)` instance in a certain frame and style. It can do this either programmatically or in a storyboard. The frame is usually set to the screen frame, minus the height of the status bar or, in a navigation-based app, to the screen frame minus the heights of the status bar and the navigation bar. The view controller may also set global properties of the table view at this point, such as its autoresizing behavior or a global row height.

To learn how to create table views in a storyboard and programmatically, see [Creating a Table View Using a Storyboard](#//apple_ref/doc/uid/TP40007451-CH6-SW2) and [Creating a Table View Programmatically](#//apple_ref/doc/uid/TP40007451-CH6-SW4). 

The view controller sets the data source and delegate of the table view and sends a `[reloadData](https://developer.apple.com/documentation/uikit/uitableview/1614862-reloaddata)` message to it. The data source must adopt the `[UITableViewDataSource](https://developer.apple.com/documentation/uikit/uitableviewdatasource)` protocol, and the delegate must adopt the `[UITableViewDelegate](https://developer.apple.com/documentation/uikit/uitableviewdelegate)` protocol.

The data source receives a `[numberOfSectionsInTableView:](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614860-numberofsectionsintableview)` message from the `[UITableView](https://developer.apple.com/documentation/uikit/uitableview)` object and returns the number of sections in the table view. Although this is an optional protocol method, the data source must implement it if the table view has more than one section. 

For each section, the data source receives a `[tableView:numberOfRowsInSection:](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614931-tableview)` message and responds by returning the number of rows for the section.

The data source receives a `[tableView:cellForRowAtIndexPath:](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614861-tableview)` message for each visible row in the table view. It responds by configuring and returning a `[UITableViewCell](https://developer.apple.com/documentation/uikit/uitableviewcell)` object for each row. The `[UITableView](https://developer.apple.com/documentation/uikit/uitableview)` object uses this cell to draw the row.

**Figure 4-1**  Calling sequence for creating and configuring a table viewThe diagram in Figure 4-1 shows the required [protocol](../../../../General/Conceptual/DevPedia-CocoaCore/Protocol.html#//apple_ref/doc/uid/TP40008195-CH45) methods as well as the `[numberOfSectionsInTableView:](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614860-numberofsectionsintableview)` method. Populating the table view with data occurs in steps 3 through 5. To learn how to implement the methods in these steps, see [Populating a Dynamic Table View with Data](#//apple_ref/doc/uid/TP40007451-CH6-SW5). 

The data source and the delegate may implement other optional methods of their protocols to further configure the table view. For example, the data source might want to provide titles for each of the sections in the table view by implementing the `[tableView:titleForHeaderInSection:](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614850-tableview)` method. For more on some of these optional table view customizations, see [Optional Table View Configurations](#//apple_ref/doc/uid/TP40007451-CH6-SW3).

You create a table view in either the plain style (`[UITableViewStylePlain](https://developer.apple.com/documentation/uikit/uitableview/style/plain)`) or the grouped style (`[UITableViewStyleGrouped](https://developer.apple.com/documentation/uikit/uitableview/style/grouped)`). (You specify the style in a storyboard.) Although the procedure for creating a table view in either styles is identical, you may want to perform different kinds of configurations. For example, because a grouped table view generally presents item detail, you may also want to add custom accessory views (for example, switches and sliders) or custom content (for example, text fields). For an example, see [A Closer Look at Table View Cells](../TableViewCells/TableViewCells.html#//apple_ref/doc/uid/TP40007451-CH7-SW1). 

## Recommendations for Creating and Configuring Table Views
There are many ways to put together a table view app. For example, you can use an instance of a custom `NSObject` subclass to create, configure, and manage a table view. However, you will find the task much easier if you adopt the classes, techniques, and design patterns that the UIKit framework offers for this purpose. The following approaches are recommended:

Use an instance of a subclass of `[UITableViewController](https://developer.apple.com/documentation/uikit/uitableviewcontroller)` to create and manage a table view.

Most apps use a custom `UITableViewController` object to manage a table view. As described in [Navigating a Data Hierarchy with Table Views](../TableViewAndDataModel/TableViewAndDataModel.html#//apple_ref/doc/uid/TP40007451-CH5-SW4), `UITableViewController` automatically creates a table view, assigns itself as both [delegate and data source](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14) (and adopts the corresponding [protocols](../../../../General/Conceptual/DevPedia-CocoaCore/Protocol.html#//apple_ref/doc/uid/TP40008195-CH45)), and initiates the procedure for populating the table view with data. It also takes care of several other “housekeeping” details of behavior. The behavior of `UITableViewController` (a subclass of `[UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller)`) within the navigation controller architecture is described in [Table View Controllers](../TableViewAndDataModel/TableViewAndDataModel.html#//apple_ref/doc/uid/TP40007451-CH5-SW1).

If your app is largely based on table views, select the Master-Detail Application template provided by Xcode when you create your project.

As described in [Creating a Table View Using a Storyboard](#//apple_ref/doc/uid/TP40007451-CH6-SW2), the template includes stub code and a [storyboard](../../../../General/Conceptual/Devpedia-CocoaApp/Storyboard.html#//apple_ref/doc/uid/TP40009071-CH99) defining an app delegate, the navigation controller, and the master [view controller](../../../../General/Conceptual/DevPedia-CocoaCore/ControllerObject.html#//apple_ref/doc/uid/TP40008195-CH11) (which is an instance of a custom subclass of `UITableViewController`).

For successive table views, you should implement custom `UITableViewController` objects. You can either load them from a storyboard or create the associated table views programmatically. 

Although either option is possible, the storyboard route is generally easier.

If the view to be managed is a composite view in which a table view is one of multiple subviews, you must use a custom subclass of `[UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller)` to manage the table view (and other views). Do not use `UITableViewController`, because this controller class sizes the table view to fill the screen between the navigation bar and the tab bar (if either is present).

## Creating a Table View Using a Storyboard
Create an app with a table view using Xcode. When you create your project, select a template that contains stub code and a [storyboard](../../../../General/Conceptual/Devpedia-CocoaApp/Storyboard.html#//apple_ref/doc/uid/TP40009071-CH99) that, by default, supply the structure for setting up and managing table views. 

To create an app structured around table views
In Xcode, choose File > New > Project.

In the iOS section at the left side of the dialog, select Application.

In the main area of the dialog, select Master-Detail Application and then click Next.

Choose your project options (make sure Use Storyboard is selected), and then click Next.

Choose a save location for your project and then click Create.

Depending on which device family you chose in step 4, the project has one or two storyboards. To display the storyboard canvas, double-click a storyboard file in the project navigator. If the device family is iPhone, for example, your storyboard should contain a table view controller that looks similar to the one in Figure 4-2.

**Figure 4-2**  The master view controller in the Master-Detail Application storyboardTo make sure that the scene on the canvas represents the master view controller class in your code
On the canvas, click the scene’s title bar to select the table view controller. 

Click the Identity button at the top of the utility area to open the Identity inspector.

Verify that the Class field contains the project’s custom subclass of `UITableViewController`.

### Choose the Table View’s Display Style
As described in [Table View Styles](../TableViewStyles/TableViewCharacteristics.html#//apple_ref/doc/uid/TP40007451-CH3-SW4), every table view has a display style: plain or grouped. 

To choose the display style of a table view in a storyboard
Click the center of the scene to select the table view. 

In the utility area, display the Attributes inspector. 

In the Table View section of the Attributes inspector, use the Style pop-up menu to choose Plain or Grouped.

### Choose the Table View’s Content Type
Storyboards introduce two convenient ways to design a table view’s content: 

**Dynamic prototypes**. Design a prototype cell and then use it as the template for other cells in the table. Use a dynamic prototype when multiple cells in a table should use the same layout to display information. Dynamic content is managed by the table view data source (the table view controller) at runtime, with an arbitrary number of cells. Figure 4-3 shows a plain table view with a one prototype cell. 

**Figure 4-3**  A dynamic table view
> **Note:** If a table view in a storyboard is dynamic, the custom subclass of `[UITableViewController](https://developer.apple.com/documentation/uikit/uitableviewcontroller)` that contains the table view needs to implement the data source protocol. For more information, see [Populating a Dynamic Table View with Data](#//apple_ref/doc/uid/TP40007451-CH6-SW5). 

**Static cells**. Use static content to design the overall layout of the table, including the total number of cells. A table view with static content has a fixed set of cells that you can configure at design time. You can also configure other static data elements such as section headers. Use static cells when a table does not change its layout, regardless of the specific information it displays. Figure 4-4 shows a grouped table view with three static cells. 

**Figure 4-4**  A static table view
> **Note:** If a table view in a storyboard is static, the custom subclass of `[UITableViewController](https://developer.apple.com/documentation/uikit/uitableviewcontroller)` that contains the table view should *not* implement the data source protocol. Instead, the table view controller should use  its `[viewDidLoad](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621495-viewdidload)` method to populate the table view’s data. For more information, see [Populating a Static Table View With Data](#//apple_ref/doc/uid/TP40007451-CH6-SW31).

By default, when you add a table view controller to a storyboard, the controller contains a table view that uses prototype-based cells. If you want to use static cells:

Select the table view. 

Display the Attributes inspector. 

In the the Content pop-up menu, choose Static Cells. 

If you’re designing a prototype cell, the table view needs a way to identify the prototype when the data source dequeues reusable cells for the table at runtime. You do this by assigning a reuse identifier to the cell. In the Table View Cell section of the Attributes inspector, enter a string in the Identifier text field, which you will also use when asking for a new cell of that type. To make understanding the code easier, a cell’s reuse identifier should describe what the cell contains. For example, a cell for displaying bird sightings might have an identifier of `@”BirdSightingCell”`. 

### Design the Table View’s Rows
As described in [Standard Styles for Table View Cells](../TableViewStyles/TableViewCharacteristics.html#//apple_ref/doc/uid/TP40007451-CH3-SW14), UIKit defines four styles for the cells that a table view uses to draw its rows. You can use one of the four standard styles, design a custom style, or subclass `[UITableViewCell](https://developer.apple.com/documentation/uikit/uitableviewcell)` to define additional behavior or properties for the cell. This topic is covered in detail in [A Closer Look at Table View Cells](../TableViewCells/TableViewCells.html#//apple_ref/doc/uid/TP40007451-CH7-SW1). 

A table view cell can also have an accessory, as described in [Accessory Views](../TableViewStyles/TableViewCharacteristics.html#//apple_ref/doc/uid/TP40007451-CH3-SW2). An accessory is a standard user interface element that UIKit draws at the right end of a table cell. For example, the disclosure indicator, which looks similar to a right angle bracket (>), tells users that tapping an item reveals related information in a new screen. In the Attributes inspector, use the Accessory pop‐up menu to select a cell’s accessory. 

### Create Additional Table Views
If your app displays and manages more than one table view, add those table views to your storyboard. You add a table view by adding a custom `UITableViewController` object, which contains the table view it manages.

To add custom class files to your project
In Xcode, choose File > New > File.

In the iOS section at the left side of the dialog, select Cocoa Touch.

In the main area of the dialog, select Objective-C class, and then click Next.

Enter a name for your new class, choose subclass of `UITableViewController`, and then click Next.

Choose a save location for your class files, and then click Create.

To add a table view controller to a storyboard
Display the storyboard to which you want to add the table view controller.

Drag a table view controller out of the object library and drop it on the storyboard.

With the new scene still selected on the canvas, click the Identity button in the utility area to open the Identity inspector.

In the Custom Class section, choose the new custom class in the Class pop-up menu.

Set the new table view’s style and cell content (dynamic or static).

Create a segue to the new scene.

The details of step 7 vary depending on the project. To learn more about adding segues, see *[Xcode Overview](../../../../ToolsLanguages/Conceptual/Xcode_Overview/index.html#//apple_ref/doc/uid/TP40010215)*. 

> **Note:** Populating a table view with data and configuring a table view are discussed in [Populating a Dynamic Table View with Data](#//apple_ref/doc/uid/TP40007451-CH6-SW5) and [Optional Table View Configurations](#//apple_ref/doc/uid/TP40007451-CH6-SW3). 

### Learn More by Creating a Sample App
The tutorial *[Your Second iOS App: Storyboards](../../../../iPhone/Conceptual/SecondiOSAppTutorial/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011318)* shows how to create a sample app that is structured around table views. After you complete the steps in this tutorial, you’ll have a working knowledge of how to create both dynamic and static table views using a storyboard. The tutorial creates a basic navigation-based app called BirdWatching that uses table view controllers connected by both push and modal segues. 

## Creating a Table View Programmatically
If you choose not to use `[UITableViewController](https://developer.apple.com/documentation/uikit/uitableviewcontroller)` for table view management, you must replicate what this class gives you “for free.” 

### Adopt the Data Source and Delegate Protocols
The class creating the table view typically makes itself the [data source and delegate](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14) by adopting the `[UITableViewDataSource](https://developer.apple.com/documentation/uikit/uitableviewdatasource)` and `[UITableViewDelegate](https://developer.apple.com/documentation/uikit/uitableviewdelegate)` [protocols](../../../../General/Conceptual/DevPedia-CocoaCore/Protocol.html#//apple_ref/doc/uid/TP40008195-CH45). The adoption syntax appears just after the superclass in the `@interface` directive, as shown in Listing 4-1.

**Listing 4-1**  Adopting the data source and delegate protocols

```
@interface RootViewController : UIViewController <UITableViewDelegate, UITableViewDataSource>
```

```
 
```

```
@property (nonatomic, strong) NSArray *timeZoneNames;
```

```
@end
```
### Create and Configure a Table View
The next step is for the client to [allocate and initialize](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39) an instance of the `[UITableView](https://developer.apple.com/documentation/uikit/uitableview)` class. Listing 4-2 gives an example of a client that creates a `[UITableView](https://developer.apple.com/documentation/uikit/uitableview)` object in the plain style, specifies its autoresizing characteristics, and then sets itself to be both data source and delegate. Again, keep in mind that the `[UITableViewController](https://developer.apple.com/documentation/uikit/uitableviewcontroller)` does all of this for you automatically.

**Listing 4-2**  Creating a table view

- (void)loadView
```
{
```

```
    UITableView *tableView = [[UITableView alloc] initWithFrame:[[UIScreen mainScreen] applicationFrame] style:UITableViewStylePlain];
```

```
    tableView.autoresizingMask = UIViewAutoresizingFlexibleHeight|UIViewAutoresizingFlexibleWidth;
```

```
    tableView.delegate = self;
```

```
    tableView.dataSource = self;
```

```
    [tableView reloadData];
```

```
 
```

```
    self.view = tableView;
```

```
}
```
Because in this example the class creating the table view is a subclass of `UIViewController`, it assigns the created table view to its `view` property, which it inherits from that class. It also sends a `[reloadData](https://developer.apple.com/documentation/uikit/uitableview/1614862-reloaddata)` message to the table view, causing the table view to initiate the procedure for populating its sections and rows with data.

## Populating a Dynamic Table View with Data
Just after a table view object is created, it receives a `[reloadData](https://developer.apple.com/documentation/uikit/uitableview/1614862-reloaddata)` message, which tells it to start querying the [data source and delegate](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14) for the information it needs for the sections and rows it displays. The table view immediately asks the data source for its logical dimensions—that is, the number of sections and the number of rows in each section. It then repeatedly invokes the `[tableView:cellForRowAtIndexPath:](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614861-tableview)` method to get a cell object for each visible row; it uses this `[UITableViewCell](https://developer.apple.com/documentation/uikit/uitableviewcell)` object to draw the content of the row. (Scrolling a table view also causes an invocation of `[tableView:cellForRowAtIndexPath:](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614861-tableview)` for each newly visible row.) 

As noted in [Choose the Table View’s Content Type](#//apple_ref/doc/uid/TP40007451-CH6-SW27),  if the table view is dynamic then you need to implement the required data source methods. Listing 4-3 shows an example of how the data source and the delegate could configure a dynamic table view. 

**Listing 4-3**  Populating a dynamic table view with data

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
```
    return [regions count];
```

```
}
```

```
 
```

```
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
```

```
    // Number of rows is the number of time zones in the region for the specified section.
```

```
    Region *region = [regions objectAtIndex:section];
```

```
    return [region.timeZoneWrappers count];
```

```
}
```

```
 
```

```
 
```

```
- (NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section {
```

```
    // The header for the section is the region name -- get this from the region at the section index.
```

```
    Region *region = [regions objectAtIndex:section];
```

```
    return [region name];
```

```
}
```

```
 
```

```
 
```

```
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
```

```
    static NSString *MyIdentifier = @"MyReuseIdentifier";
```

```
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:MyIdentifier];
```

```
    if (cell == nil) {
```

```
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault  reuseIdentifier:MyIdentifier];
```

```
    }
```

```
    Region *region = [regions objectAtIndex:indexPath.section];
```

```
    TimeZoneWrapper *timeZoneWrapper = [region.timeZoneWrappers objectAtIndex:indexPath.row];
```

```
    cell.textLabel.text = timeZoneWrapper.localeName;
```

```
    return cell;
```

```
}
```
The data source, in its implementation of the `[tableView:cellForRowAtIndexPath:](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614861-tableview)` method, returns a configured cell object that the table view can use to draw a row. For performance reasons, the data source tries to reuse cells as much as possible. It first asks the table view for a specific reusable cell object by sending it a `[dequeueReusableCellWithIdentifier:](https://developer.apple.com/documentation/uikit/uitableview/1614891-dequeuereusablecell)` message. If no such object exists, the data source creates it, assigning it a reuse identifier. The data source sets the cell’s content (in this example, its text) and returns it. [A Closer Look at Table View Cells](../TableViewCells/TableViewCells.html#//apple_ref/doc/uid/TP40007451-CH7-SW1) discusses this data source method and `[UITableViewCell](https://developer.apple.com/documentation/uikit/uitableviewcell)` objects in more detail. 

If the `[dequeueReusableCellWithIdentifier:](https://developer.apple.com/documentation/uikit/uitableview/1614891-dequeuereusablecell)` method asks for a cell that’s defined in a storyboard, the method always returns a valid cell. If there is not a recycled cell waiting to be reused, the method creates a new one using the information in the storyboard itself. This eliminates the need to check the return value for `nil` and create a cell manually. 

The implementation of the `tableView:cellForRowAtIndexPath:` method in Listing 4-3 includes an `[NSIndexPath](https://developer.apple.com/documentation/foundation/nsindexpath)` argument that identifies the table view section and row. UIKit declares a [category](../../../../General/Conceptual/DevPedia-CocoaCore/Category.html#//apple_ref/doc/uid/TP40008195-CH5) of the `NSIndexPath` class, which is defined in the Foundation framework. This category extends the class to enable the identification of table view rows by section and row number. For more information on this category, see *NSIndexPath UIKit Additions*.

## Populating a Static Table View With Data
As noted in [Choose the Table View’s Content Type](#//apple_ref/doc/uid/TP40007451-CH6-SW27), if a table view is static then you should not implement any data source methods. The configuration of the table view is known at compile time, so UIKit can get this information from the storyboard at runtime. However, you still need to populate a static table view with data from your data model. Populating a Static Table View With Data shows an example of how a table view controller could load user data in a static table view. This example is adapted from *[Your Second iOS App: Storyboards](../../../../iPhone/Conceptual/SecondiOSAppTutorial/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011318)*. 

**Listing 4-4**  Populating a static table view with data

- (void)viewDidLoad
```
{
```

```
    [super viewDidLoad];
```

```
 
```

```
    BirdSighting *theSighting = self.sighting;
```

```
    static NSDateFormatter *formatter = nil;
```

```
    if (formatter == nil) {
```

```
        formatter = [[NSDateFormatter alloc] init];
```

```
        [formatter setDateStyle:NSDateFormatterMediumStyle];
```

```
    }
```

```
    if (theSighting) {
```

```
        self.birdNameLabel.text = theSighting.name;
```

```
        self.locationLabel.text = theSighting.location;
```

```
        self.dateLabel.text = [formatter stringFromDate:(NSDate*)theSighting.date];
```

```
    }
```

```
}
```
The table view is populated with data in the `[UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller)` method `[viewDidLoad](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621495-viewdidload)`, which is called after the view is loaded into memory. The data is passed to the table view controller in the `sighting` object, which is set in the previous view controller’s `[prepareForSegue:sender:](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621490-prepareforsegue)` method. The properties `birdNameLabel`, `locationLabel`, and `dateLabel` are outlets connected to labels in the static table view (see [Figure 4-4](#//apple_ref/doc/uid/TP40007451-CH6-SW26)). 

## Populating an Indexed List
An indexed list (see [Figure 1-2](../TableViewStyles/TableViewCharacteristics.html#//apple_ref/doc/uid/TP40007451-CH3-SW6)) is ideally suited for navigating large amounts of data organized by a conventional ordering scheme such as an alphabet. An indexed list is a table view in the plain style that is specially configured through three `UITableViewDataSource` methods: 

`[sectionIndexTitlesForTableView:](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614857-sectionindextitles)` 

Returns an array of the strings to use as the index entries (in order). 

`[tableView:titleForHeaderInSection:](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614850-tableview)`

Maps these index strings to the titles of the table view’s sections (they don’t have to be the same). 

`[tableView:sectionForSectionIndexTitle:atIndex:](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614933-tableview)`

Returns the section index related to the entry the user tapped in the index. 

The data you use to populate an indexed list should be organized to reflect this indexing model. Specifically, you need to build an [array of arrays](../../../../General/Conceptual/DevPedia-CocoaCore/Collection.html#//apple_ref/doc/uid/TP40008195-CH10). Each inner array corresponds to a section in the table. Section arrays are sorted (or collated) within the outer array according to the prevailing ordering scheme, which is often an alphabetical scheme (for example, A through Z). Additionally, the items in each section array are sorted. You can build and sort this array of arrays yourself, but fortunately the `[UILocalizedIndexedCollation](https://developer.apple.com/documentation/uikit/uilocalizedindexedcollation)` class greatly simplifies the tasks of building and sorting these data structures and providing data to the table view. The class also collates items in the arrays according to the current localization.

However you internally manage this array-of-arrays structure is up to you. The objects to be collated should have a [property](../../../../General/Conceptual/DevPedia-CocoaCore/DeclaredProperty.html#//apple_ref/doc/uid/TP40008195-CH13) or method that returns a string value that the `UILocalizedIndexedCollation` class uses in collation; if it is a method, it should have no parameters. You might find it convenient to define a custom [model class](../../../../General/Conceptual/DevPedia-CocoaCore/ModelObject.html#//apple_ref/doc/uid/TP40008195-CH31) whose instances represent the rows in the table view. These model objects not only return a string value but also define a property that holds the index of the section array to which the object is assigned. Listing 4-5 illustrates the definition of a class that declares a `name` property and a `sectionNumber` property. 

**Listing 4-5**  Defining the model-object interface

@interface State : NSObject
```
 
```

```
@property(nonatomic,copy) NSString *name;
```

```
@property(nonatomic,copy) NSString *capitol;
```

```
@property(nonatomic,copy) NSString *population;
```

```
@property NSInteger sectionNumber;
```

```
@end
```
Before your table view [controller](../../../../General/Conceptual/DevPedia-CocoaCore/ControllerObject.html#//apple_ref/doc/uid/TP40008195-CH11) is asked to populate the table view, you load the data to be used (from whatever source) and create instances of your model class from this data. The example in Listing 4-6 loads data defined in a property list and creates the model objects from that. It also obtains the shared instance of `[UILocalizedIndexedCollation](https://developer.apple.com/documentation/uikit/uilocalizedindexedcollation)` and [initializes](../../../../General/Conceptual/DevPedia-CocoaCore/Initialization.html#//apple_ref/doc/uid/TP40008195-CH21) the [mutable](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectMutability.html#//apple_ref/doc/uid/TP40008195-CH42) array (`states`) that will contain the section arrays.

**Listing 4-6**  Loading the table-view data and initializing the model objects

- (void)viewDidLoad {
```
    [super viewDidLoad];
```

```
    UILocalizedIndexedCollation *theCollation = [UILocalizedIndexedCollation currentCollation];
```

```
    self.states = [NSMutableArray arrayWithCapacity:1];
```

```
 
```

```
    NSString *thePath = [[NSBundle mainBundle] pathForResource:@"States" ofType:@"plist"];
```

```
    NSArray *tempArray;
```

```
    NSMutableArray *statesTemp;
```

```
    if (thePath && (tempArray = [NSArray arrayWithContentsOfFile:thePath]) ) {
```

```
        statesTemp = [NSMutableArray arrayWithCapacity:1];
```

```
        for (NSDictionary *stateDict in tempArray) {
```

```
            State *aState = [[State alloc] init];
```

```
            aState.name = [stateDict objectForKey:@"Name"];
```

```
            aState.population = [stateDict objectForKey:@"Population"];
```

```
            aState.capitol = [stateDict objectForKey:@"Capitol"];
```

```
            [statesTemp addObject:aState];
```

```
        }
```

```
    } else  {
```

```
        return;
```

```
    }
```
After the [data source](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14) has this “raw” array of model objects, it can process it with the facilities of the `[UILocalizedIndexedCollation](https://developer.apple.com/documentation/uikit/uilocalizedindexedcollation)` class. In Listing 4-7, the code is annotated with numbers. 

**Listing 4-7**  Preparing the data for the indexed list

    // viewDidLoad continued...
```
    // (1)
```

```
    for (State *theState in statesTemp) {
```

```
        NSInteger sect = [theCollation sectionForObject:theState collationStringSelector:@selector(name)];
```

```
        theState.sectionNumber = sect;
```

```
    }
```

```
    // (2)
```

```
    NSInteger highSection = [[theCollation sectionTitles] count];
```

```
    NSMutableArray *sectionArrays = [NSMutableArray arrayWithCapacity:highSection];
```

```
    for (int i = 0; i < highSection; i++) {
```

```
        NSMutableArray *sectionArray = [NSMutableArray arrayWithCapacity:1];
```

```
        [sectionArrays addObject:sectionArray];
```

```
    }
```

```
    // (3)
```

```
    for (State *theState in statesTemp) {
```

```
        [(NSMutableArray *)[sectionArrays objectAtIndex:theState.sectionNumber] addObject:theState];
```

```
    }
```

```
    // (4)
```

```
    for (NSMutableArray *sectionArray in sectionArrays) {
```

```
        NSArray *sortedSection = [theCollation sortedArrayFromArray:sectionArray
```

```
            collationStringSelector:@selector(name)];
```

```
        [self.states addObject:sortedSection];
```

```
    }
```

```
} // end of viewDidLoad
```
Here's what the code in Listing 4-7 does:

The data source enumerates the array of model objects and sends `[sectionForObject:collationStringSelector:](https://developer.apple.com/documentation/uikit/uilocalizedindexedcollation/1620378-section)` to the collation manager on each iteration. This method takes as arguments a model object and a property or method of the object that it uses in collation. Each call returns the index of the section array to which the model object belongs, and that value is assigned to the `sectionNumber` property.

The data source source then creates a (temporary) outer mutable array and mutable arrays for each section; it adds each created section array to the outer array.

It then enumerates the array of model objects and adds each object to its assigned section array.

The data source enumerates the array of section arrays and calls `[sortedArrayFromArray:collationStringSelector:](https://developer.apple.com/documentation/uikit/uilocalizedindexedcollation/1620382-sortedarray)` on the collation manager to sort the items in each array. It passes in a section array and a property or method that is to be used in sorting the items in the array. Each sorted section array is added to the final outer array.

Now the data source is ready to populate its table view with data. It implements the methods specific to indexed lists as shown in Listing 4-8. In doing this it calls two `[UILocalizedIndexedCollation](https://developer.apple.com/documentation/uikit/uilocalizedindexedcollation)` methods: `[sectionIndexTitles](https://developer.apple.com/documentation/uikit/uilocalizedindexedcollation/1620383-sectionindextitles)` and `[sectionForSectionIndexTitleAtIndex:](https://developer.apple.com/documentation/uikit/uilocalizedindexedcollation/1620380-sectionforsectionindextitleatind)`. Also note that in `[tableView:titleForHeaderInSection:](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614850-tableview)` it suppresses any headers from appearing in the table view when the associated section does not have any items.

**Listing 4-8**  Providing section-index data to the table view

- (NSArray *)sectionIndexTitlesForTableView:(UITableView *)tableView {
```
    return [[UILocalizedIndexedCollation currentCollation] sectionIndexTitles];
```

```
}
```

```
 
```

```
- (NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section {
```

```
    if ([[self.states objectAtIndex:section] count] > 0) {
```

```
        return [[[UILocalizedIndexedCollation currentCollation] sectionTitles] objectAtIndex:section];
```

```
    }
```

```
    return nil;
```

```
}
```

```
 
```

```
- (NSInteger)tableView:(UITableView *)tableView sectionForSectionIndexTitle:(NSString *)title atIndex:(NSInteger)index
```

```
{
```

```
    return [[UILocalizedIndexedCollation currentCollation] sectionForSectionIndexTitleAtIndex:index];
```

```
}
```

> **Accessibility Note:** To change what VoiceOver reads aloud when the indexed list is selected, assign a localized string to the `[accessibilityLabel](https://developer.apple.com/documentation/uikit/uiaccessibilityelement/1619577-accessibilitylabel)` property of each item in the array that `[sectionIndexTitlesForTableView:](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614857-sectionindextitles)` returns.

Finally, the data source should implement the `UITableViewDataSource` methods that are common to all table views. Listing 4-9 gives examples of these implementations, and illustrates how to use the `section` and `row` properties of the table view–specific [category](../../../../General/Conceptual/DevPedia-CocoaCore/Category.html#//apple_ref/doc/uid/TP40008195-CH5) of the `[NSIndexPath](https://developer.apple.com/documentation/foundation/nsindexpath)` class described in *NSIndexPath UIKit Additions*.

**Listing 4-9**  Populating the rows of an indexed list

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
```
    return [self.states count];
```

```
}
```

```
 
```

```
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
```

```
    return [[self.states objectAtIndex:section] count];
```

```
}
```

```
 
```

```
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
```

```
    static NSString *CellIdentifier = @"StateCell";
```

```
    UITableViewCell *cell;
```

```
    cell = [tableView dequeueReusableCellWithIdentifier:CellIdentifier];
```

```
    if (cell == nil) {
```

```
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:CellIdentifier];
```

```
    }
```

```
    State *stateObj = [[self.states objectAtIndex:indexPath.section] objectAtIndex:indexPath.row];
```

```
    cell.textLabel.text = stateObj.name;
```

```
    return cell;
```

```
}
```
For table views that are indexed lists, when the data source assigns cells for rows in `[tableView:cellForRowAtIndexPath:](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614861-tableview)`, it should ensure that the `[accessoryType](https://developer.apple.com/documentation/uikit/uitableviewcell/1623228-accessorytype)` property of the cell is set to `UITableViewCellAccessoryNone`. 

After initially populating the table view following the procedure outlined above, you can reload the contents of the index by calling the `[reloadSectionIndexTitles](https://developer.apple.com/documentation/uikit/uitableview/1614932-reloadsectionindextitles)` method.

## Optional Table View Configurations
The table view API allows you to configure various visual and behavioral aspects of a table view, including specific rows and sections. The following examples serve to give you some idea of the options available to you.

### Add a Custom Title
In the same block of code that creates the table view, you can apply global configurations using certain methods of the `[UITableView](https://developer.apple.com/documentation/uikit/uitableview)` class. The code example in Listing 4-10  adds a custom title for the table view (using a `[UILabel](https://developer.apple.com/documentation/uikit/uilabel)` object).

**Listing 4-10**  Adding a title to the table view

```
- (void)loadView
```

```
{
```

```
    CGRect titleRect = CGRectMake(0, 0, 300, 40);
```

```
    UILabel *tableTitle = [[UILabel alloc] initWithFrame:titleRect];
```

```
    tableTitle.textColor = [UIColor blueColor];
```

```
    tableTitle.backgroundColor = [self.tableView backgroundColor];
```

```
    tableTitle.opaque = YES;
```

```
    tableTitle.font = [UIFont boldSystemFontOfSize:18];
```

```
    tableTitle.text = [curTrail objectForKey:@"Name"];
```

```
    self.tableView.tableHeaderView = tableTitle;
```

```
    [self.tableView reloadData];
```

```
}
```
### Provide a Section Title
The example in Listing 4-11 returns a title string for a section. 

**Listing 4-11**  Returning a title for a section

```
- (NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section {
```

```
    // Returns section title based on physical state: [solid, liquid, gas, artificial]
```

```
    return [[[PeriodicElements sharedPeriodicElements] elementPhysicalStatesArray] objectAtIndex:section];
```

```
}
```
### Indent a Row
The code in Listing 4-12 moves a specific row to the next level of indentation.

**Listing 4-12**  Custom indentation of a row

```
- (NSInteger)tableView:(UITableView *)tableView indentationLevelForRowAtIndexPath:(NSIndexPath *)indexPath {
```

```
    if ( indexPath.section==TRAIL_MAP_SECTION && indexPath.row==0 ) {
```

```
        return 2;
```

```
    }
```

```
    return 1;
```

```
}
```
### Vary a Row’s Height
The example in Listing 4-13 varies the height of a specific row based on its index value. 

**Listing 4-13**  Varying row height

```
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
```

```
{
```

```
    CGFloat result;
```

```
 
```

```
    switch ([indexPath row])
```

```
    {
```

```
        case 0:
```

```
        {
```

```
            result = kUIRowHeight;
```

```
            break;
```

```
        }
```

```
        case 1:
```

```
        {
```

```
            result = kUIRowLabelHeight;
```

```
            break;
```

```
        }
```

```
    }
```

```
    return result;
```

```
}
```
### Customize Cells
You can also affect the appearance of rows by returning custom `[UITableViewCell](https://developer.apple.com/documentation/uikit/uitableviewcell)` objects with specially formatted subviews for content in `[tableView:cellForRowAtIndexPath:](https://developer.apple.com/documentation/uikit/uitableviewdatasource/1614861-tableview)`. Cell customization is discussed in [A Closer Look at Table View Cells](../TableViewCells/TableViewCells.html#//apple_ref/doc/uid/TP40007451-CH7-SW1).

        
            [Next](../TableViewCells/TableViewCells.html)[Previous](../TableViewAndDataModel/TableViewAndDataModel.html)
        
         Copyright &#x00a9; 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-09-18