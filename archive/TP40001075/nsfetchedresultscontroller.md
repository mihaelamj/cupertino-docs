---
title: "Part II: Integrating Core Data with iOS"
book: "Core Data Programming Guide"
chapterId: "TP40001075-CH8"
date: "2017-03-27"
description: "Explains how to manage objects using the Core Data framework."
identifier: "//apple_ref/doc/uid/TP40001075"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Part II: Integrating Core Data with iOS

> **Core Data Programming Guide**
> Last updated: 2017-03-27

## Connecting the Model to Views

  
  	
  		
  In macOS, Core Data is designed to work with the user interface through Cocoa bindings. However, Cocoa bindings are not a part of the user interface in iOS. In iOS, you use `[NSFetchedResultsController](https://developer.apple.com/documentation/coredata/nsfetchedresultscontroller)` to connect the model (Core Data) to the views (storyboards).

		 

  
  
  ### Creating a Fetched Results Controller

  
  When you use Core Data with a UITableView-based layout, the `NSFetchedResultsController` for your data is typically initialized by the `[UITableViewController](https://developer.apple.com/documentation/uikit/uitableviewcontroller)` instance that will utilize that data. This initialization can take place in the `[viewDidLoad](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621495-viewdidload)` or `[viewWillAppear:](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621510-viewwillappear)` methods, or at another logical point in the life cycle of the view controller.

  This example shows the initialization of the `NSFetchedResultsController`.

  
  
      
          Objective-C

        

            @property (nonatomic, strong) NSFetchedResultsController *fetchedResultsController;

             

            - (void)initializeFetchedResultsController

            {

                NSFetchRequest *request = [NSFetchRequest fetchRequestWithEntityName:@"Person"];

             

                NSSortDescriptor *lastNameSort = [NSSortDescriptor sortDescriptorWithKey:@"lastName" ascending:YES];

             

                [request setSortDescriptors:@[lastNameSort]];

             

                NSManagedObjectContext *moc = …; //Retrieve the main queue NSManagedObjectContext

             

                [self setFetchedResultsController:[[NSFetchedResultsController alloc] initWithFetchRequest:request managedObjectContext:moc sectionNameKeyPath:nil cacheName:nil]];

                [[self fetchedResultsController] setDelegate:self];

             

                NSError *error = nil;

                if (![[self fetchedResultsController] performFetch:&error]) {

                    NSLog(@"Failed to initialize FetchedResultsController: %@\n%@", [error localizedDescription], [error userInfo]);

                    abort();

                }

            }

        

      
      
          Swift

        

            - `var fetchedResultsController: NSFetchedResultsController!`

            - ` `

            - `func initializeFetchedResultsController() {`

            - `    let request = NSFetchRequest(entityName: "Person")`

            - `    let departmentSort = NSSortDescriptor(key: "department.name", ascending: true)`

            - `    let lastNameSort = NSSortDescriptor(key: "lastName", ascending: true)`

            - `    request.sortDescriptors = [departmentSort, lastNameSort]`

            - `    `

            - `    let moc = dataController.managedObjectContext`

            - `    fetchedResultsController = NSFetchedResultsController(fetchRequest: request, managedObjectContext: moc, sectionNameKeyPath: nil, cacheName: nil)`

            - `    fetchedResultsController.delegate = self`

            - `    `

            - `    do {`

            - `        try fetchedResultsController.performFetch()`

            - `    } catch {`

            - `        fatalError("Failed to initialize FetchedResultsController: \(error)")`

            - `    }`

            - `}`

        

      
  

  In the `initializeFetchedResultsController` method shown above that will live within the controlling `UITableViewController` instance, you first construct a fetch request (`NSFetchRequest`), which is at the heart of the `NSFetchedResultsController`. Note that the fetch request contains a sort descriptor (`[NSSortDescriptor](https://developer.apple.com/documentation/foundation/nssortdescriptor)`). `NSFetchedResultsController` requires at least one sort descriptor to control the order of the data that is being presented.

  After the fetch request is initialized, you can initialize the `NSFetchedResultsController` instance. The fetched results controller  requires you to pass it an `NSFetchRequest` instance and a reference to the managed object context (`[NSManagedObjectContext](https://developer.apple.com/documentation/coredata/nsmanagedobjectcontext)`) that the fetch is to be run against. The `[sectionNameKeyPath](https://developer.apple.com/documentation/coredata/nsfetchedresultscontroller/1622285-sectionnamekeypath)` and the `[cacheName](https://developer.apple.com/documentation/coredata/nsfetchedresultscontroller/1622280-cachename)` properties are both optional.

  After the fetched results controller is initialized, you assign it a delegate. The delegate notifies the table view controller when any changes have occurred to the underlying data structure. Typically, the table view controller is also the delegate to the fetched results controller so that it can receive callbacks whenever there are changes to the underlying data.

  Next, you start the `NSFetchedResultsController` by a call to `[performFetch:](https://developer.apple.com/documentation/coredata/nsfetchedresultscontroller/1622305-performfetch)`. This call retrieves the initial data to be displayed and causes the `NSFetchedResultsController` instance to start monitoring the managed object context for changes.

  

  
  ### Integrating the Fetched Results Controller with the Table View Data Source

  
  After you integrate the initialized fetched results controller and have data ready to be displayed in the table view, you integrate the fetched results controller with the table view data source (`[UITableViewDataSource](https://developer.apple.com/documentation/uikit/uitableviewdatasource)`).

  
  
      
          Objective-C

        

            #pragma mark - UITableViewDataSource

             

            - (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath

            {

                id cell = [tableView dequeueReusableCellWithIdentifier:CellReuseIdentifier];

             

                NSManagedObject *object = [self.fetchedResultsController objectAtIndexPath:indexPath];

                // Configure the cell from the object

                return cell;

            }

             

            - (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView

            {

                return [[[self fetchedResultsController] sections] count];

            }

             

            - (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section

            {

                id< NSFetchedResultsSectionInfo> sectionInfo = [[self fetchedResultsController] sections][section];

                return [sectionInfo numberOfObjects];

            }

        

      
      
          Swift

        

            - `override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {`

            - `    guard let cell = tableView.dequeueReusableCell(withIdentifier: "cellIdentifier", for: indexPath) else {`

            - `        fatalError("Wrong cell type dequeued")`

            - `    }`

            - `    // Set up the cell`

            - `    guard let object = self.fetchedResultsController?.object(at: indexPath) else {`

            - `        fatalError("Attempt to configure cell without a managed object")`

            - `    }`

            - `    //Populate the cell from the object`

            - `    return cell`

            - `}`

            - ` `

            - `override func numberOfSectionsInTableView(tableView: UITableView) -> Int {`

            - `    return fetchedResultsController.sections!.count`

            - `}`

            - ` `

            - `override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {`

            - `    guard let sections = fetchedResultsController.sections else {`

            - `        fatalError("No sections in fetchedResultsController")`

            - `    }`

            - `    let sectionInfo = sections[section]`

            - `    return sectionInfo.numberOfObjects`

            - `}`

        

      
  

  As shown in each `UITableViewDataSource` method above, the integration with the fetched results controller is reduced to a single method call that is specifically designed to integrate with the table view data source.

  

  
  ### Communicating Data Changes to the Table View

  
  In addition to making it significantly easier to integrate Core Data with the table view data source, `NSFetchedResultsController` handles the communication with the `UITableViewController` instance when data changes. To enable this, implement the `[NSFetchedResultsControllerDelegate](https://developer.apple.com/documentation/coredata/nsfetchedresultscontrollerdelegate)` protocol:

  
  
      
          Objective-C

        

            #pragma mark - NSFetchedResultsControllerDelegate

            - (void)controllerWillChangeContent:(NSFetchedResultsController *)controller

            {

                [[self tableView] beginUpdates];

            }

            - (void)controller:(NSFetchedResultsController *)controller didChangeSection:(id <NSFetchedResultsSectionInfo>)sectionInfo atIndex:(NSUInteger)sectionIndex forChangeType:(NSFetchedResultsChangeType)type

            {

                switch(type) {

                    case NSFetchedResultsChangeInsert:

                        [[self tableView] insertSections:[NSIndexSet indexSetWithIndex:sectionIndex] withRowAnimation:UITableViewRowAnimationFade];

                        break;

                    case NSFetchedResultsChangeDelete:

                        [[self tableView] deleteSections:[NSIndexSet indexSetWithIndex:sectionIndex] withRowAnimation:UITableViewRowAnimationFade];

                        break;

                    case NSFetchedResultsChangeMove:

                    case NSFetchedResultsChangeUpdate:

                        break;

                }

            }

            - (void)controller:(NSFetchedResultsController *)controller didChangeObject:(id)anObject atIndexPath:(NSIndexPath *)indexPath forChangeType:(NSFetchedResultsChangeType)type newIndexPath:(NSIndexPath *)newIndexPath

            {

                switch(type) {

                    case NSFetchedResultsChangeInsert:

                        [[self tableView] insertRowsAtIndexPaths:@[newIndexPath] withRowAnimation:UITableViewRowAnimationFade];

                        break;

                    case NSFetchedResultsChangeDelete:

                        [[self tableView] deleteRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationFade];

                        break;

                    case NSFetchedResultsChangeUpdate:

                        [self.tableView reloadRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationFade];

                        break;

                    case NSFetchedResultsChangeMove:

                        [[self tableView] deleteRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationFade];

                        [[self tableView] insertRowsAtIndexPaths:@[newIndexPath] withRowAnimation:UITableViewRowAnimationFade];

                        break;

                }

            }

            - (void)controllerDidChangeContent:(NSFetchedResultsController *)controller

            {

                [[self tableView] endUpdates];

            }

        

      
      
          Swift

        

            - `func controllerWillChangeContent(_ controller: NSFetchedResultsController<NSFetchRequestResult>) {`

            - `    tableView.beginUpdates()`

            - `}`

            - ` `

            - `func controller(_ controller: NSFetchedResultsController<NSFetchRequestResult>, didChange sectionInfo: NSFetchedResultsSectionInfo, atSectionIndex sectionIndex: Int, for type: NSFetchedResultsChangeType) {`

            - `    switch type {`

            - `    case .insert:`

            - `        tableView.insertSections(IndexSet(integer: sectionIndex), with: .fade)`

            - `    case .delete:`

            - `        tableView.deleteSections(IndexSet(integer: sectionIndex), with: .fade)`

            - `    case .move:`

            - `        break`

            - `    case .update:`

            - `        break`

            - `    }`

            - `}`

            - ` `

            - `func controller(_ controller: NSFetchedResultsController<NSFetchRequestResult>, didChange anObject: Any, at indexPath: IndexPath?, for type: NSFetchedResultsChangeType, newIndexPath: IndexPath?) {`

            - `    switch type {`

            - `    case .insert:`

            - `        tableView.insertRows(at: [newIndexPath!], with: .fade)`

            - `    case .delete:`

            - `        tableView.deleteRows(at: [indexPath!], with: .fade)`

            - `    case .update:`

            - `        tableView.reloadRows(at: [indexPath!], with: .fade)`

            - `    case .move:`

            - `        tableView.moveRow(at: indexPath!, to: newIndexPath!)`

            - `    }`

            - `}`

            - ` `

            - `func controllerDidChangeContent(_ controller: NSFetchedResultsController<NSFetchRequestResult>) {`

            - `    tableView.endUpdates()`

            - `}`

        

      
  

  Implementing the four protocol methods shown above provides automatic updates to the associated `UITableView` whenever the underlying data changes.

  

  
  ### Adding Sections

  
  So far you have been working with a table view that has only one section, which represents all of the data that needs to be displayed in the table view. If you are working with a large number of Employee objects, it can be advantageous to divide the table view into multiple sections. Grouping the employees by department makes the list of employees more manageable. Without Core Data, a table view with multiple sections would involve an array of arrays, or perhaps an even more complicated data structure. With Core Data, you make a simple change to the construction of the fetched results controller.

  
  
      
          Objective-C

        

            - (void)initializeFetchedResultsController

            {

                NSFetchRequest *request = [NSFetchRequest fetchRequestWithEntityName:@"Person"];

                NSSortDescriptor *departmentSort = [NSSortDescriptor sortDescriptorWithKey:@"department.name" ascending:YES];

                NSSortDescriptor *lastNameSort = [NSSortDescriptor sortDescriptorWithKey:@"lastName" ascending:YES];

                [request setSortDescriptors:@[departmentSort, lastNameSort]];

                NSManagedObjectContext *moc = [[self dataController] managedObjectContext];

                [self setFetchedResultsController:[[NSFetchedResultsController alloc] initWithFetchRequest:request managedObjectContext:moc sectionNameKeyPath:@"department.name" cacheName:nil]];

                [[self fetchedResultsController] setDelegate:self];

            }

        

      
      
          Swift

        

            - `func initializeFetchedResultsController() {`

            - `    let request = NSFetchRequest(entityName: "Person")`

            - `    let departmentSort = NSSortDescriptor(key: "department.name", ascending: true)`

            - `    let lastNameSort = NSSortDescriptor(key: "lastName", ascending: true)`

            - `    request.sortDescriptors = [departmentSort, lastNameSort]`

            - `    let moc = dataController.managedObjectContext`

            - `    fetchedResultsController = NSFetchedResultsController(fetchRequest: request, managedObjectContext: moc, sectionNameKeyPath: "department.name", cacheName: nil)`

            - `    fetchedResultsController.delegate = self`

            - `    do {`

            - `        try fetchedResultsController.performFetch()`

            - `    } catch {`

            - `        fatalError("Failed to initialize FetchedResultsController: \(error)")`

            - `    }`

            - `}`

        

      
  

  In this example you add one more `NSSortDescriptor` instance to the `NSFetchRequest` instance. You set the same key from that new sort descriptor as the `sectionNameKeyPath` on the initialization of the `NSFetchedResultsController`. The fetched results controller uses this initial sort controller to break apart the data into multiple sections and therefore requires that the keys match.

  This change causes the fetched results controller to break the returning Person instances into multiple sections based on the name of the department that each Person instance is associated with. The only conditions of using this feature are:

  
  The `sectionNameKeyPath` property must also be an `NSSortDescriptor` instance.

  The `NSSortDescriptor` must be the first descriptor in the array passed to the fetch request.

  

  
  ### Adding Caching for Performance

  
  In many situations, a table view represents a relatively static type of data. A fetch request is defined at the creation of the table view controller, and it never changes throughout the life of the application. In those situations it can be advantageous to add a cache to the `NSFetchedResultsController` instance so that when the application is launched again and the data has not changed, the table view initializes instantaneously. A cache is especially useful for displaying unusually large data sets.

  
  
      
          Objective-C

        

            [self setFetchedResultsController:[[NSFetchedResultsController alloc] initWithFetchRequest:request managedObjectContext:moc sectionNameKeyPath:@"department.name" cacheName:@"rootCache"]];

        

      
      
          Swift

        

            - `fetchedResultsController = NSFetchedResultsController(fetchRequest: request, managedObjectContext: moc, sectionNameKeyPath: "department.name", cacheName: "rootCache")`

        

      
  

  As shown above, the `cacheName` property is set when the `NSFetchedResultsController` instance is initialized, and the fetched results controller automatically gains a cache. Subsequent loading of the data will be nearly instantaneous. 

  
  
> 
    Note
    
    	If a situation occurs where the fetch request associated with a fetched results controller needs to change, then it is vital that the cache be invalidated prior to changing the fetched results controller. You invalidate the cache by calling `[deleteCacheWithName:](https://developer.apple.com/documentation/coredata/nsfetchedresultscontroller/1622283-deletecachewithname)`, which is a class level method on `NSFetchedResultsController`.
    	
    
  

  

  	
 	
    		[Creating and Modifying Custom Managed Objects](LifeofaManagedObject.html#//apple_ref/doc/uid/TP40001075-CH16-SW1)

  			[Integrating Core Data at iOS Startup](IntegratingCoreData.html#//apple_ref/doc/uid/TP40001075-CH9-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2017-03-27](RevisionHistory.html#//apple_ref/doc/uid/TP40003204-SW1)