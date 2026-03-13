---
title: "Managing Selections"
book: "Table View Programming Guide for iOS"
chapterId: "TP40007451-CH9"
date: "2013-09-18"
description: "Describes how to create and manage table views for applications running iOS."
identifier: "//apple_ref/doc/uid/TP40007451"
source: apple-archive
---

# Managing Selections

> **Table View Programming Guide for iOS**
> Last updated: 2013-09-18

[Next](../ManageInsertDeleteRow/ManageInsertDeleteRow.html)[Previous](../TableViewCells/TableViewCells.html)
        
        
        [](../index.html)
        
        # Managing Selections
When users tap a row of a table view, usually something happens as a result. Another table view could slide into place, the row could display a checkmark, or some other action could be performed. The following sections describe how to respond to selections and how to make selections programmatically.

## Selections in Table Views
There are a few human-interface guidelines to keep in mind when dealing with cell selection in table views:

You should never use selection to indicate state.  Instead, use check marks and accessory views for showing state.

When the user selects a cell, you should respond by deselecting the previously selected cell (by calling the `[deselectRowAtIndexPath:animated:](https://developer.apple.com/documentation/uikit/uitableview/1614989-deselectrow)` method) as well as by performing any appropriate action, such as displaying a detail view.

If you respond to the the selection of a cell by pushing a new [view controller](../../../../General/Conceptual/DevPedia-CocoaCore/ControllerObject.html#//apple_ref/doc/uid/TP40008195-CH11) onto the navigation controller’s stack, you should deselect the cell (with animation) when the view controller is popped off the stack. 

You can control whether rows are selectable when the table view is in editing mode by setting the `[allowsSelectionDuringEditing](https://developer.apple.com/documentation/uikit/uitableview/1614889-allowsselectionduringediting)` property of `UITableView`. In addition, beginning with iOS 3.0, you can control whether cells are selectable when editing mode is not in effect by setting the `[allowsSelection](https://developer.apple.com/documentation/uikit/uitableview/1614911-allowsselection)` property.

## Responding to Selections
Users tap a row in a table view either to signal to the application that they want to know more about what that row signifies or to select what the row represents. In response to the user tapping a row, an application could do any of the following:

Show the next level in a data-model hierarchy.

Show a detail view of an item (that is, a leaf node of the data-model hierarchy). 

Show a checkmark in the row to indicate that the represented item is selected.

If the touch occurred in a control embedded in the row, it could respond to the action message sent by the control.

To handle most selections of rows, the table view’s delegate must implement the `[tableView:didSelectRowAtIndexPath:](https://developer.apple.com/documentation/uikit/uitableviewdelegate/1614877-tableview)` method. In sample method implementation shown in Listing 6-1, the [delegate](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14) first deselects the selected row. Then it [allocates and initializes](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectCreation.html#//apple_ref/doc/uid/TP40008195-CH39) an instance of the next table-view controller in the sequence. It sets the data this view controller needs to populate its table view and then pushes this object onto the stack maintained by the application’s `[UINavigationController](https://developer.apple.com/documentation/uikit/uinavigationcontroller)` object.  

**Listing 6-1**  Responding to a row selection

- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
```
 {
```

```
     [tableView deselectRowAtIndexPath:indexPath animated:NO];
```

```
     BATTrailsViewController *trailsController = [[BATTrailsViewController alloc] initWithStyle:UITableViewStylePlain];
```

```
     trailsController.selectedRegion = [regions objectAtIndex:indexPath.row];
```

```
     [[self navigationController] pushViewController:trailsController animated:YES];
```

```
}
```
If a row has a disclosure control—the white chevron over a blue circle—for an accessory view, clicking the control results in the delegate receiving a `[tableView:accessoryButtonTappedForRowWithIndexPath:](https://developer.apple.com/documentation/uikit/uitableviewdelegate/1614996-tableview)` message (instead of `[tableView:didSelectRowAtIndexPath:](https://developer.apple.com/documentation/uikit/uitableviewdelegate/1614877-tableview)`). The delegate responds to this message in the same general way as it does for other kinds of selections.

A row can also have a control object as its accessory view, such as a switch or a slider. This control object functions as it would in any other context: Manipulating the object in the proper way results in an action message being sent to a target object. Listing 6-2 illustrates a [data source](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14) object that adds a `[UISwitch](https://developer.apple.com/documentation/uikit/uiswitch)` object as a cell’s accessory view and then responds to the action messages sent when the switch is “flipped.”  

**Listing 6-2**  Setting a switch object as an accessory view and responding to its action message

- (UITableViewCell *)tableView:(UITableView *)tv cellForRowAtIndexPath:(NSIndexPath *)indexPath {
```
       UITableViewCell *cell = [tv dequeueReusableCellWithIdentifier:@"CellWithSwitch"];
```

```
        if (cell == nil) {
```

```
            cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"CellWithSwitch"];
```

```
            cell.selectionStyle = UITableViewCellSelectionStyleNone;
```

```
            cell.textLabel.font = [UIFont systemFontOfSize:14];
```

```
        }
```

```
        UISwitch *switchObj = [[UISwitch alloc] initWithFrame:CGRectMake(1.0, 1.0, 20.0, 20.0)];
```

```
        switchObj.on = YES;
```

```
        [switchObj addTarget:self action:@selector(toggleSoundEffects:) forControlEvents:(UIControlEventValueChanged | UIControlEventTouchDragInside)];
```

```
        cell.accessoryView = switchObj;
```

```
 
```

```
        cell.textLabel.text = @"Sound Effects";
```

```
        return cell;
```

```
}
```

```
 
```

```
- (void)toggleSoundEffects:(id)sender {
```

```
    [self.soundEffectsOn = [(UISwitch *)sender isOn];
```

```
    [self reset];
```

```
}
```
You may also define controls as accessory views of table-view cells created in Interface Builder. Drag a control object (switch, slider, and so on) into a [nib document window](../../../../General/Conceptual/DevPedia-CocoaCore/NibFile.html#//apple_ref/doc/uid/TP40008195-CH34) containing a table-view cell. Then, using the connection window, make the control the accessory view of the cell. [Loading Table View Cells from a Storyboard](../TableViewCells/TableViewCells.html#//apple_ref/doc/uid/TP40007451-CH7-SW20) describes the procedure for creating and configuring table-view cell objects in nib files.

Selection management is also important with selection lists. There are two kinds of selection lists:

Exclusive lists where only one row is permitted the checkmark

Inclusive lists where more than one row can have a checkmark

Listing 6-3 illustrates one approach to managing an exclusive selection list. It first deselects the currently selected row and returns if the same row is selected; otherwise it sets the checkmark accessory type on the newly selected row and removes the checkmark on the previously selected row

**Listing 6-3**  Managing a selection list—exclusive list

- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
```
 
```

```
    [tableView deselectRowAtIndexPath:indexPath animated:NO];
```

```
    NSInteger catIndex = [taskCategories indexOfObject:self.currentCategory];
```

```
    if (catIndex == indexPath.row) {
```

```
        return;
```

```
    }
```

```
    NSIndexPath *oldIndexPath = [NSIndexPath indexPathForRow:catIndex inSection:0];
```

```
 
```

```
    UITableViewCell *newCell = [tableView cellForRowAtIndexPath:indexPath];
```

```
    if (newCell.accessoryType == UITableViewCellAccessoryNone) {
```

```
        newCell.accessoryType = UITableViewCellAccessoryCheckmark;
```

```
        self.currentCategory = [taskCategories objectAtIndex:indexPath.row];
```

```
    }
```

```
 
```

```
    UITableViewCell *oldCell = [tableView cellForRowAtIndexPath:oldIndexPath];
```

```
    if (oldCell.accessoryType == UITableViewCellAccessoryCheckmark) {
```

```
        oldCell.accessoryType = UITableViewCellAccessoryNone;
```

```
    }
```

```
}
```
Listing 6-4 illustrates how to manage a inclusive selection list. As the comments in this example indicate, when the delegate adds a checkmark to a row or removes one, it typically also sets or unsets any associated model-object attribute. 

**Listing 6-4**  Managing a selection list—inclusive list

- (void)tableView:(UITableView *)theTableView
```
          didSelectRowAtIndexPath:(NSIndexPath *)newIndexPath {
```

```
 
```

```
    [theTableView deselectRowAtIndexPath:[theTableView indexPathForSelectedRow] animated:NO];
```

```
    UITableViewCell *cell = [theTableView cellForRowAtIndexPath:newIndexPath];
```

```
    if (cell.accessoryType == UITableViewCellAccessoryNone) {
```

```
        cell.accessoryType = UITableViewCellAccessoryCheckmark;
```

```
        // Reflect selection in data model
```

```
    } else if (cell.accessoryType == UITableViewCellAccessoryCheckmark) {
```

```
        cell.accessoryType = UITableViewCellAccessoryNone;
```

```
        // Reflect deselection in data model
```

```
    }
```

```
}
```
In `[tableView:didSelectRowAtIndexPath:](https://developer.apple.com/documentation/uikit/uitableviewdelegate/1614877-tableview)` you should always deselect the currently selected row.

## Programmatically Selecting and Scrolling
Occasionally the selection of a row originates within the application itself rather than from a tap in a table view. There could be an externally induced change in the data model. For example, the user adds a new person to an address book and then returns to the list of contacts; the application wants to scroll this list to the recently added person. For situations like these, you can use the `UITableView` methods `[selectRowAtIndexPath:animated:scrollPosition:](https://developer.apple.com/documentation/uikit/uitableview/1614875-selectrowatindexpath)` and (if the row is already selected) `[scrollToNearestSelectedRowAtScrollPosition:animated:](https://developer.apple.com/documentation/uikit/uitableview/1614910-scrolltonearestselectedrow)`. You may also call `[scrollToRowAtIndexPath:atScrollPosition:animated:](https://developer.apple.com/documentation/uikit/uitableview/1614997-scrolltorowatindexpath)` if you want to scroll to a specific row without selecting it.

The code in Listing 6-5 (somewhat whimsically) programmatically selects and scrolls to a row 20 rows away from the just-selected row using the `selectRowAtIndexPath:animated:scrollPosition:` method.

**Listing 6-5**  Programmatically selecting a row

```
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)newIndexPath {
```

```
    NSIndexPath *scrollIndexPath;
```

```
    if (newIndexPath.row + 20 < [timeZoneNames count]) {
```

```
        scrollIndexPath = [NSIndexPath indexPathForRow:newIndexPath.row+20 inSection:newIndexPath.section];
```

```
    } else {
```

```
        scrollIndexPath = [NSIndexPath indexPathForRow:newIndexPath.row-20 inSection:newIndexPath.section];
```

```
    }
```

```
    [theTableView selectRowAtIndexPath:scrollIndexPath animated:YES
```

```
                        scrollPosition:UITableViewScrollPositionMiddle];
```

```
}
```

        
            [Next](../ManageInsertDeleteRow/ManageInsertDeleteRow.html)[Previous](../TableViewCells/TableViewCells.html)
        
         Copyright &#x00a9; 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-09-18