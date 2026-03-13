---
title: "Quick Start for Property Lists"
book: "Property List Programming Guide"
chapterId: "10000048i-CH4"
date: "2010-03-24"
description: "Explains how to use structured, textual representations of data in Cocoa."
identifier: "//apple_ref/doc/uid/10000048i"
source: apple-archive
---

# Quick Start for Property Lists

> **Property List Programming Guide**
> Last updated: 2010-03-24

[Next](../AboutPropertyLists/AboutPropertyLists.html)[Previous](../Introduction/Introduction.html)
        
        
        [](../index.html)
        
        # Quick Start for Property Lists
This mini-tutorial gives you a quick, practical introduction to property lists. You start by specifying a short property list in XML. Then you design an application that, when it launches, reads and converts the elements of the XML property list into their object equivalents and stores these objects in instance variables. The application displays these object values in the user interface and allows you to change them. When you quit the application, it writes out the modified property list as XML. When you relaunch the application, the new values are displayed.

## Create the XML Property List
In Xcode, create a simple Cocoa application project—call it PropertyListExample. Then select the Resources folder of the project and choose New File from the File menu. In the “Other” template category, select the Property List template and click Next. Name the file “Data.plist”.

Double-click the `Data.plist` file in Xcode (you’ll find it in the Resources folder). Xcode displays an empty property list in a special editor. Edit the property list so that it looks like the following example:

You can also edit the property list in a text editor such as TextEdit or BBEdit. When you’re finished, it should look like the following XML code.

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Name</key>
    <string>John Doe</string>
    <key>Phones</key>
    <array>
        <string>408-974-0000</string>
        <string>503-333-5555</string>
    </array>
</dict>
</plist>
```
Because the property-list file is in the Resources folder, it will be written to the application’s main [bundle](../../../../General/Conceptual/DevPedia-CocoaCore/Bundle.html#//apple_ref/doc/uid/TP40008195-CH4) when you build the project. 

## Define Storage for the Property-List Objects
In this step, you’ll add a [coordinating controller](../../../../General/Conceptual/DevPedia-CocoaCore/ControllerObject.html#//apple_ref/doc/uid/TP40008195-CH11) class to the project and [declare properties](../../../../General/Conceptual/DevPedia-CocoaCore/DeclaredProperty.html#//apple_ref/doc/uid/TP40008195-CH13) to hold the property-list objects defined in `Data.plist`. (Note the distinction here between declared *property* and *property*-list object.)

In Xcode, select the Classes folder and choose New File from the File menu. Select the Objective-C Class template and name the files “Controller.h” and “Controller.m”. Make the following declarations in `Controller.h`.

#import <Cocoa/Cocoa.h>
```
 
```

```
@interface Controller : NSObject {
```

```
    NSString *personName;
```

```
    NSMutableArray *phoneNumbers;
```

```
}
```

```
 
```

```
@property (copy, nonatomic) NSString *personName;
```

```
@property (retain, nonatomic) NSMutableArray *phoneNumbers;
```

```
 
```

```
@end
```
In `Controller.m`, have the compiler synthesize accessor methods for these properties:

```
@implementation Controller
```

```
 
```

```
@synthesize personName;
```

```
@synthesize phoneNumbers;
```

```
 
```

```
@end
```
## Create the User Interface
Double-click the project’s [nib file](../../../../General/Conceptual/DevPedia-CocoaCore/NibFile.html#//apple_ref/doc/uid/TP40008195-CH34) to open it in Interface Builder. Create a simple user interface similar to the following:

> **Note:** This mini-tutorial shows user-interface techniques and programming interfaces that are specific to OS X. If you want to use an iOS application for this example, you must use techniques and API that are appropriate to that platform. The code specific to property lists is applicable to both platforms.

The table view should have a single column that is editable. 

For reasons of simplicity and efficiency, you’ll next bind the text field to the `personName` [property](../../../../General/Conceptual/DevPedia-CocoaCore/DeclaredProperty.html#//apple_ref/doc/uid/TP40008195-CH13). But your Controller object will act as a [data source](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14) for the table. Let’s start with the text field.

Drag an generic Object proxy from the Library into the nib document window. Select it and, in the Identify pane of the inspector, type or select “Controller” for its class identity.

Drag an Object Controller object from the Library into the nib document window. Control-drag (or right-click-drag) a line from Object Controller to Controller and, in the connection window that pops up, select “content”.

Select the editable text field and, in the Bindings pane of the inspector, bind the value attribute of the text field according to the following:

Bind to: Object Controller

Controller Key: selection

Model Key Path: personName

Next, Control-drag a line from File’s Owner in the nib document window (File’s Owner in this case represents the global `[NSApplication](https://developer.apple.com/documentation/appkit/nsapplication)` object) to the Controller object and then select `delegate` in the connection window. As you’ll see, the application delegate (Controller) plays a role in saving the property list to its XML representation.

For the table view, Control-drag a line from the table view to the Controller object in the nib document window. Select the `dataSource` outlet in the connection window. Save the nib file. Copy the code in Listing 1-1 to `Controller.m`. 

**Listing 1-1**  Implementation code for table view’s data source

 - (NSInteger)numberOfRowsInTableView:(NSTableView *)tableView {
```
    return self.phoneNumbers.count;
```

```
}
```

```
 
```

```
 - (id)tableView:(NSTableView *)tableView
```

```
         objectValueForTableColumn:(NSTableColumn *)tableColumn
```

```
         row:(NSInteger)row {
```

```
    return [phoneNumbers objectAtIndex:row];
```

```
}
```

```
 
```

```
- (void)tableView:(NSTableView *)tableView setObjectValue:(id)object
```

```
         forTableColumn:(NSTableColumn *)tableColumn row:(NSInteger)row {
```

```
    [phoneNumbers replaceObjectsAtIndexes:[NSIndexSet indexSetWithIndex:row]
```

```
                  withObjects:[NSArray arrayWithObject:object]];
```

```
}
```
Note that the last method synchronizes changes to items in the table view with the `phoneNumbers` mutable array that backs it.

## Read in the Property List
Now that the necessary user-interface tasks are completed, we can focus on code that is specific to property lists. In its `init` method, the Controller object reads in the initial XML property list from the [main bundle](../../../../General/Conceptual/DevPedia-CocoaCore/Bundle.html#//apple_ref/doc/uid/TP40008195-CH4) when the application is first launched; thereafter, it gets the property list from the user’s `Documents` directory. Once it has the property list, it converts its elements into the corresponding property-list objects. Listing 1-2 shows how it does this.

**Listing 1-2**  Reading in and converting the XML property list

- (id) init {
```
 
```

```
    self = [super init];
```

```
    if (self) {
```

```
        NSString *errorDesc = nil;
```

```
        NSPropertyListFormat format;
```

```
        NSString *plistPath;
```

```
        NSString *rootPath = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory,
```

```
           NSUserDomainMask, YES) objectAtIndex:0];
```

```
        plistPath = [rootPath stringByAppendingPathComponent:@"Data.plist"];
```

```
        if (![[NSFileManager defaultManager] fileExistsAtPath:plistPath]) {
```

```
            plistPath = [[NSBundle mainBundle] pathForResource:@"Data" ofType:@"plist"];
```

```
        }
```

```
        NSData *plistXML = [[NSFileManager defaultManager] contentsAtPath:plistPath];
```

```
        NSDictionary *temp = (NSDictionary *)[NSPropertyListSerialization
```

```
            propertyListFromData:plistXML
```

```
            mutabilityOption:NSPropertyListMutableContainersAndLeaves
```

```
            format:&format
```

```
            errorDescription:&errorDesc];
```

```
        if (!temp) {
```

```
            NSLog(@"Error reading plist: %@, format: %d", errorDesc, format);
```

```
        }
```

```
        self.personName = [temp objectForKey:@"Name"];
```

```
        self.phoneNumbers = [NSMutableArray arrayWithArray:[temp objectForKey:@"Phones"]];
```

```
 
```

```
    }
```

```
    return self;
```

```
}
```
This code first gets the file-system path to the file containing the XML property list (`Data.plist`) in the `~/Documents` directory. If there is no file by that name at that location, it obtains the property-list file from the application’s main bundle. Then it uses the `NSFileManager` method `[contentsAtPath:](https://developer.apple.com/documentation/foundation/filemanager/1407347-contents)` to read the property list into memory as an `[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)` object. After that, it calls the `NSPropertyListSerialization` class method `[propertyListFromData:mutabilityOption:format:errorDescription:](https://developer.apple.com/documentation/foundation/propertylistserialization/1411993-propertylistfromdata)` to convert the static property list into the corresponding property-list objects—specifically, a dictionary containing a string and an array of strings. It assigns the string and the array of strings to the appropriate properties of the Controller object.

## Write Out the Property List
When the user quits the application, you want to save the current values of the `personName` and `phoneNumbers` properties in a dictionary object, convert those property-list objects to a static XML representation, and then write that XML data to a file in `~/Documents`. The `[applicationShouldTerminate:](https://developer.apple.com/documentation/appkit/nsapplicationdelegate/1428642-applicationshouldterminate)`  [delegate](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14) method of `NSApplication` is the appropriate place to write the code shown in Listing 1-3.

**Listing 1-3**  Converting and writing the property list to the application bundle 

- (NSApplicationTerminateReply)applicationShouldTerminate:(NSApplication *)sender {
```
    NSString *error;
```

```
    NSString *rootPath = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) objectAtIndex:0];
```

```
    NSString *plistPath = [rootPath stringByAppendingPathComponent:@"Data.plist"];
```

```
    NSDictionary *plistDict = [NSDictionary dictionaryWithObjects:
```

```
            [NSArray arrayWithObjects: personName, phoneNumbers, nil]
```

```
            forKeys:[NSArray arrayWithObjects: @"Name", @"Phones", nil]];
```

```
    NSData *plistData = [NSPropertyListSerialization dataFromPropertyList:plistDict
```

```
                            format:NSPropertyListXMLFormat_v1_0
```

```
                            errorDescription:&error];
```

```
    if(plistData) {
```

```
        [plistData writeToFile:plistPath atomically:YES];
```

```
    }
```

```
    else {
```

```
        NSLog(error);
```

```
        [error release];
```

```
    }
```

```
    return NSTerminateNow;
```

```
}
```
This code creates an `[NSDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSDictionary)` object containing the values of the `personName` and `phoneNumbers` properties and associates these with the keys “Name” and “Phones”. Then, using the `[dataFromPropertyList:format:errorDescription:](https://developer.apple.com/documentation/foundation/nspropertylistserialization/1416061-datafrompropertylist)` [class method](../../../../General/Conceptual/DevPedia-CocoaCore/ClassMethod.html#//apple_ref/doc/uid/TP40008195-CH8) of `[NSPropertyListSerialization](https://developer.apple.com/documentation/foundation/nspropertylistserialization)`, it converts this top-level dictionary and the other property-list objects it contains into XML data. Finally, it writes the XML data to `Data.plist` in the user’s `Documents` directory. 

## Run and Test the Application
Build and run the application. The window appears with the name and phone numbers you specified in the XML property list. Modify the name and a phone number and quit the application. Find the `Data.plist` file in `~/Documents` and open it in a text editor. You’ll see that the changes you made in the user interface are reflected in the XML property list. If you launch the application again, it displays the changed values.

        
            [Next](../AboutPropertyLists/AboutPropertyLists.html)[Previous](../Introduction/Introduction.html)
        
         Copyright &#x00a9; 2010 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2010-03-24