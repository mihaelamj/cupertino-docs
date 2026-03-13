---
title: "Integrating Core Data and Storyboards"
book: "Core Data Programming Guide"
chapterId: "TP40001075-CH10"
date: "2017-03-27"
description: "Explains how to manage objects using the Core Data framework."
identifier: "//apple_ref/doc/uid/TP40001075"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Integrating Core Data and Storyboards

> **Core Data Programming Guide**
> Last updated: 2017-03-27

## Integrating Core Data and Storyboards

  
  	
  		
  Core Data integrates well with the Xcode Storyboards feature, which you use to build your UI. This integration allows you to take advantage of the dependency injection pattern. Dependency injection is an inversion of control; it allows the caller of a framework to control the flow by passing references into the called object. Dependency injection is one of the preferred patterns to be used with Cocoa development and specifically with iOS Cocoa development.

		 

  
  
  ### Integrating Core Data with a Storyboard Segue

  
  A complex integration point for Core Data and storyboards is the transition between a table view displaying numerous data objects and a child view controller that presents the details of one of those items. Without using a storyboard, you accomplish this by overriding the `[UITableViewDelegate](https://developer.apple.com/documentation/uikit/uitableviewdelegate)` method `[tableView:didSelectRowAtIndexPath:](https://developer.apple.com/documentation/uikit/uitableviewdelegate/1614877-tableview)`. However, with a storyboard, this method should not be used, and the transition should be handled within the `[prepareForSegue:sender:](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621490-prepareforsegue)` method.

  To demonstrate how you can integrate Core Data with a storyboard segue, consider the following example, which shows a primary view controller that is a table view with a list of employees. When one of those employees in that list is selected, you will want to present a detail view of that employee. It is assumed that the view controller within the segue has a property to receive the selected `[NSManagedObject](https://developer.apple.com/documentation/coredata/nsmanagedobject)`.

  
  
      
          Objective-C

        

            @interface DetailViewController : UIViewController

             

            @property (weak) AAAEmployeeMO *employee;

             

            @end

        

      
      
          Swift

        

            - `class DetailViewController: UIViewController {`

            - `    `

            - `    weak var employee: EmployeeMO?`

            - `    `

            - `}`

        

      
  

  
  
> 
    Note
    
    	Whenever you are passing `[NSManagedObject](https://developer.apple.com/documentation/coredata/nsmanagedobject)` references in your application, it is beneficial to declare them as `weak` references. This helps to protect your view controller in the event of the `NSManagedObject` being deleted and leaving a dangling reference to a non-existent object. When the property is declared as `weak` it is automatically set to `nil` when the object is deleted.
    	
    
  

  Next you implement the `[prepareForSegue:sender:](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621490-prepareforsegue)` method in the primary view controller to pass the appropriate `NSManagedObject` instance during the segue:

  
  
      
          Objective-C

        

            #define CellDetailIdentifier @"CellDetailIdentifier"

             

            - (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender

            {

                id destination = [segue destinationViewController];

                if ([[segue identifier] isEqualToString:CellDetailIdentifier]) {

                    NSIndexPath *indexPath = [[self tableView] indexPathForSelectedRow];

                    id selectedObject = [[self fetchedResultsController] objectAtIndexPath:indexPath];

                    [destination setEmployee:selectedObject];

                    return;

                }

            }

        

      
      
          Swift

        

            - `let CellDetailIdentifier = "CellDetailIdentifier"`

            - ` `

            - `override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {`

            - `    switch segue.identifier! {`

            - `    case CellDetailIdentifier:`

            - `        let destination = segue.destinationViewController as! DetailViewController`

            - `        let indexPath = tableView.indexPathForSelectedRow!`

            - `        let selectedObject = fetchedResultsController.objectAtIndexPath(indexPath) as! EmployeeMO`

            - `        destination.employee = selectedObject`

            - `    default:`

            - `        print("Unknown segue: \(segue.identifier)")`

            - `    }`

            - `}`

        

      
  

  After the segue identifier (which is required to be unique per segue in a storyboard) is retrieved from the segue, you can safely assume what type of class the destination view controller is and pass a reference to the selected Employee instance to the destination view controller. The destination view controller will then have the reference available to it during the loading portion of its life cycle and display the data associated with it. This is dependency injection in that the parent view controller is controlling the flow of the application by deciding which Employee instance to hand off to the destination view controller.

  

  	
 	
    		[Integrating Core Data at iOS Startup](IntegratingCoreData.html#//apple_ref/doc/uid/TP40001075-CH9-SW1)

  			[Using Core Data with Cocoa Bindings](CocoaBindings.html#//apple_ref/doc/uid/TP40001075-CH12-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2017-03-27](RevisionHistory.html#//apple_ref/doc/uid/TP40003204-SW1)