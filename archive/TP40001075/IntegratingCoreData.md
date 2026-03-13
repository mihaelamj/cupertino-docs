---
title: "Integrating Core Data at iOS Startup"
book: "Core Data Programming Guide"
chapterId: "TP40001075-CH9"
date: "2017-03-27"
description: "Explains how to manage objects using the Core Data framework."
identifier: "//apple_ref/doc/uid/TP40001075"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Integrating Core Data at iOS Startup

> **Core Data Programming Guide**
> Last updated: 2017-03-27

## Integrating Core Data at iOS Startup

  
  	
  		
  The beginning of an application life cycle is subtly different in iOS and macOS.

  When an macOS application takes an unusually long time to launch and becomes unresponsive, the operating system changes the cursor to indicate this state. The user can then decide to wait for the application to finish launching or quit the application.

  In iOS, there is no such concept. If an app does not launch within a finite amount of time, the operating system terminates the application. Therefore, it is vital that an application complete the startup procedure as quickly as possible.

  On the other hand, you want your application to be able to access data inside of Core Data as quickly as possible, which usually means initializing Core Data as one of the first steps in the application’s life cycle. Although atypical, Core Data occasionally takes longer than usual to complete its initialization.

  Therefore, it is recommended that the startup sequence of an iOS app be broken into two phases, to avoid termination:

  
  A minimal startup that indicates to the user that the application is launching

  Complete loading of the application UI after Core Data has completed its initialization

		 

  
  
  ### Initializing Core Data in iOS

  
  The first step is to change how the `[application:didFinishLaunchingWithOptions:](https://developer.apple.com/documentation/uikit/uiapplicationdelegate/1622921-application)` method is implemented. In the `application:didFinishLaunchingWithOptions:` method, consider initializing Core Data and doing very little else. If you are using a storyboard, you can continue to display the launch image during this method.

  As part of the initialization of Core Data, assign the adding of the persistent store (`[NSPersistentStore](https://developer.apple.com/documentation/coredata/nspersistentstore)`) to the persistent store coordinator (`[NSPersistentStoreCoordinator](https://developer.apple.com/documentation/coredata/nspersistentstorecoordinator)`) to a background queue. That action can take an unknown amount of time, and performing it on the main queue can block the user interface, possibly causing the application to terminate.

  Once the persistent store has been added to the persistent store coordinator, you can then call back onto the main queue and request that the user interface be completed and displayed to the user.

  

  
  ### Separating Core Data from the Application Delegate

  
  Previously in iOS, the Core Data stack was typically initialized within the application delegate. However, doing so causes a significant amount of code to get muddled in with application life cycle events.

  Create the Core Data stack within its own top-level controller object, and configure the application to initialize that controller object and hold a reference to it. This action promotes the consolidation of the Core Data code within its own controller and keeps the application delegate relatively clean. This isolated controller design is shown in detail in [Initializing the Core Data Stack](InitializingtheCoreDataStack.html#//apple_ref/doc/uid/TP40001075-CH4-SW1).

  To integrate the code from the [Initializing the Core Data Stack](InitializingtheCoreDataStack.html#//apple_ref/doc/uid/TP40001075-CH4-SW1) section into an iOS app, add a property to the application delegate and initialize the controller in the `applicationDidFinishLaunching` life cycle method:

  
  
      
          Objective-C

        

            @interface AppDelegate : UIResponder <UIApplicationDelegate>

             

            @property (strong, nonatomic) UIWindow *window;

            @property (strong, nonatomic) DataController *dataController;

             

            @end

             

            @implementation AppDelegate

             

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions

            {

                [self setDataController:[[DataController alloc] initWithCompletionBlock:^{

                    //Complete user interface initialization

                }]];

                return YES;

            }

             

            @end

        

      
      
          Swift

        

            - `class AppDelegate: UIResponder, UIApplicationDelegate {`

            - `    `

            - `    var window: UIWindow?`

            - `    var dataController: DataController!`

            - `    `

            - `    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey : Any]? = nil) -> Bool {`

            - `        dataController = DataController() {`

            - `            //Complete user interface initialization`

            - `        }`

            - `        return true`

            - `}`

        

      
  

  By initializing a separate controller object, you have moved the Core Data stack out of the application delegate, but you still allow access to Core Data throughout the application.

  

  	
 	
    		[Connecting the Model to Views](nsfetchedresultscontroller.html#//apple_ref/doc/uid/TP40001075-CH8-SW1)

  			[Integrating Core Data and Storyboards](CoreDataandStoryboards.html#//apple_ref/doc/uid/TP40001075-CH10-SW1)

    Copyright &#x00a9; 2018 Apple Inc. All rights reserved. 
  [Terms of Use](http://www.apple.com/legal/terms/site.html) | 
  [Privacy Policy](http://www.apple.com/privacy/) | 
  [Updated: 2017-03-27](RevisionHistory.html#//apple_ref/doc/uid/TP40003204-SW1)