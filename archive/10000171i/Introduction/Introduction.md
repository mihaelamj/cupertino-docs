---
title: "Introduction"
book: "Internationalization and Localization Guide"
chapterId: "10000171i-CH1"
date: "2015-09-16"
description: "Explains how to create a user interface and write code that can be localized into multiple languages."
identifier: "//apple_ref/doc/uid/10000171i"
platforms: "OS X,Xcode Developer Tools,iOS"
source: apple-archive
---

# Introduction

> **Internationalization and Localization Guide**
> Last updated: 2015-09-16

[Next](../SpecifyingPreferences/SpecifyingPreferences.html)
        
        
        [](../index.html)
        
        
    # About Internationalization and Localization

    
        
> **Note:** This document was previously titled *Internationalization Programming Topics*.

            Localization is the process of translating your app into multiple languages. But before you can localize your app, you internationalize it. Internationalization is the process of making your app able to adapt to different languages, regions, and cultures. Because a single language can be used in multiple parts of the world, your app should adapt to the regional and cultural conventions of where a person resides. An internationalized app appears as if it is a native app in all the languages and regions it supports.

The App Store is available in over 150 different countries, and internationalizing your app is the first step to reach this global market. In App Store Connect, you specify whether your app is available in all territories or specific territories. Then you customize your app for each target market that you want to support. Users in other countries want to use your app in a language they understand and see dates, times, and numbers in familiar, regional formats.

        
        ## At a Glance
Xcode supports incremental localization of your project. First you internationalize your user interface and code during development. Then you test your app using pseudolocalizations and different region settings. When you are ready to localize your app, you export the localizable text using standard file formats and submit them to a localization team for translation into multiple languages. While you are waiting for these translations, you can continue developing your app and perform additional localization steps yourself—perhaps add language-specific audio and image files to your project. Then import the localizations into your project and thoroughly test your app in each supported language and region. During the next iteration of your app, you only translate changes and add additional languages.

### Learn About Language and Region Settings
Start by familiarizing yourself with the language and region settings available to the user.

> **Related Chapter:** [Reviewing Language and Region Settings](../SpecifyingPreferences/SpecifyingPreferences.html#//apple_ref/doc/uid/10000171i-CH12-SW1)

### Internationalize Your App
Prepare your app for localization by separating language and locale differences from the rest of your user interface and code.

Use base internationalization to separate user-facing text from your `.storyboard` and `.xib` files.

In Interface Builder, use Auto Layout so views adjust to the localized text.

Separate other user-facing text from your code.

Use standard APIs to handle the complexity of different writing systems and locale formats.

Adhere to the user’s settings when formatting, sorting, searching, and parsing data.

If the app supports right-to-left languages, mirror the user interface and change the text direction as needed.

> **Related Chapter:** [Internationalizing the User Interface](../InternationalizingYourUserInterface/InternationalizingYourUserInterface.html#//apple_ref/doc/uid/10000171i-CH3-SW3), [Internationalizing Your Code](../InternationalizingYourCode/InternationalizingYourCode.html#//apple_ref/doc/uid/10000171i-CH4-SW1), [Formatting Data Using the Locale Settings](../InternationalizingLocaleData/InternationalizingLocaleData.html#//apple_ref/doc/uid/10000171i-CH13-SW1), [Supporting Right-to-Left Languages](../SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17-SW1)

### Localize Your App
Export and import the localizations using standard file formats.

Lock views in the user interface.

Export the localizations.

Submit the exported files to translators.

Import the localization files and confirm the changes.

Perform additional localization steps yourself.

Xcode doesn’t translate text for you. For links to third-party localization vendors, see [Build Apps for the World](https://developer.apple.com/internationalization/).

> **Related Chapter:** [Localizing Your App](../LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW1)

### Test Your App
Test your internationalized app, using a variety of techniques, during development and after localization.

Before you localize your app:

In Interface Builder, preview the user interface using pseudolanguages.

Run the app using different pseudolanguages.

After you import localizations:

In Interface Builder, preview the localizations.

Run the app with options that detect non-localized text.

Run the app using all supported languages and regions.

Ask native-language speakers to test the app. 

> **Related Chapter:** [Internationalizing the User Interface](../InternationalizingYourUserInterface/InternationalizingYourUserInterface.html#//apple_ref/doc/uid/10000171i-CH3-SW3), [Testing Your Internationalized App](../TestingYourInternationalApp/TestingYourInternationalApp.html#//apple_ref/doc/uid/10000171i-CH7-SW1)

        
        ## See Also
The following documents provide more information about related topics:

*[Xcode Overview](../../../../ToolsLanguages/Conceptual/Xcode_Overview/index.html#//apple_ref/doc/uid/TP40010215)* describes Xcode features and contains links to other Xcode books.

*[Data Formatting Guide](../../../../Cocoa/Conceptual/DataFormatting/DataFormatting.html#//apple_ref/doc/uid/10000029i)* describes how to present and interpret calendrical and numerical data according to the user’s region settings.

*[Date and Time Programming Guide](../../../../Cocoa/Conceptual/DatesAndTimes/DatesAndTimes.html#//apple_ref/doc/uid/10000039i)* describes how to manage dates and times according to different calendars and time zones in use around the world.

*[Internationalization and Localization for OS X](../../../../../samplecode/Mountains/Introduction/Intro.html#//apple_ref/doc/uid/DTS40007727)* provides code samples that illustrate internationalization and localization techniques and APIs.

Before you submit your localized app to the App Store or Mac App Store, add territories and localize your metadata using App Store Connect:

[View and edit app information](https://help.apple.com/itunes-connect/developer/#/dev97865727c)

[Localize App Store information](https://help.apple.com/itunes-connect/developer/#/deve6f78a8e2)

        
    

        
            [Next](../SpecifyingPreferences/SpecifyingPreferences.html)
        
         Copyright &#x00a9; 2015 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2015-09-16