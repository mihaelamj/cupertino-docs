---
title: "Data Resource Files"
book: "Resource Programming Guide"
chapterId: "10000051i-CH8"
date: "2016-03-21"
description: "Explains how to work with nib and bundle resources in apps."
identifier: "//apple_ref/doc/uid/10000051i"
source: apple-archive
---

# Data Resource Files

> **Resource Programming Guide**
> Last updated: 2016-03-21

[Next](../RevisionHistory.html)[Previous](../ImageSoundResources/ImageSoundResources.html)
        
        
        [](../index.html)
        
        # Data Resource Files
Separating your application’s data from its code can make it easier to modify your application later. If you store the configuration data for your application in resource files, you can change that configuration without having to recompile your application. Data resource files can be used to store any type of information. The following sections highlight some of the data resource types supported by iOS and OS X.  

## Property List Files
Property list files are a way to store custom configuration data outside of your application code. OS X and iOS use property lists extensively to implement features such as user preferences and information property list files for bundles. You can similarly use property lists to store private (or public) configuration data for your applications. 

A property-list file is essentially a set of structured data values. You can create and edit property lists either programmatically or using the Property List Editor application (located in `/Developer/Applications/Utilities`). The structure of custom property-list files is completely up to you. You can use property lists to store string, number, Boolean, date, and raw data values. By default, a property list stores data in a single dictionary structure, but you can assign additional dictionaries and arrays as values to create a more hierarchical data set. 

For information about using property lists, see *[Property List Programming Guide](../../PropertyLists/Introduction/Introduction.html#//apple_ref/doc/uid/10000048i)* and *[Property List Programming Topics for Core Foundation](../../../../CoreFoundation/Conceptual/CFPropertyLists/CFPropertyLists.html#//apple_ref/doc/uid/10000130i)*.  

## OS X Data Resource Files
Table 4-1 lists some additional resource file types that are supported in Mac apps. 

**Table 4-1**  Other resource typesResource Type

Description

AppleScript files

In OS X, AppleScript terminology and suite files contain information about the scriptability of an application. These files can use the file extensions `.sdef`, `.scriptSuite`, or `.scriptTerminology`. Because the actual AppleScript commands used to script an application are visible in user scripts and the Script Editor application, these resources need to be localized. For information on supporting AppleScript, see *[AppleScript Overview](../../../../AppleScript/Conceptual/AppleScriptX/AppleScriptX.html#//apple_ref/doc/uid/10000156i)*. 

Help files

In OS X, help content typically consists of a set of HTML files created using a standard text-editing program and registered with the Help Viewer application. (For information on how to register with Help Viewer, see *[Apple Help Programming Guide](../../../../Carbon/Conceptual/ProvidingUserAssitAppleHelp/user_help_intro/user_assistance_intro.html#//apple_ref/doc/uid/TP30000903)*.) It is also possible to embed PDF files, RTF files, HTML files or other custom documents in your bundle and open them using an external application, such as Preview or Safari. For information on how to open files, see *[Launch Services Programming Guide](../../../../Carbon/Conceptual/LaunchServicesConcepts/LSCIntro/LSCIntro.html#//apple_ref/doc/uid/TP30000999)*.

        
            [Next](../RevisionHistory.html)[Previous](../ImageSoundResources/ImageSoundResources.html)
        
         Copyright &#x00a9; 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-03-21