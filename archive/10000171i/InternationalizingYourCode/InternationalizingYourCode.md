---
title: "Internationalizing Your Code"
book: "Internationalization and Localization Guide"
chapterId: "10000171i-CH4"
date: "2015-09-16"
description: "Explains how to create a user interface and write code that can be localized into multiple languages."
identifier: "//apple_ref/doc/uid/10000171i"
source: apple-archive
---

# Internationalizing Your Code

> **Internationalization and Localization Guide**
> Last updated: 2015-09-16

[Next](../InternationalizingLocaleData/InternationalizingLocaleData.html)[Previous](../InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
        
        
        [](../index.html)
        
        # Internationalizing Your Code
In addition to internationalizing your user interface, write code that handles text in multiple languages. First store international text in strings files similar to the strings files used by base internationalization in [Internationalizing the User Interface](../InternationalizingYourUserInterface/InternationalizingYourUserInterface.html#//apple_ref/doc/uid/10000171i-CH3-SW3). Also use language and locale-sensitive APIs for enumerating, searching, and sorting text in your code. Use standard text views for displaying and parsing text input as well. Let these APIs handle the complexity of different writing and input systems for you.

## Separating User-Facing Text from Your Code
All user-facing text supplied by your app programmatically needs to be localized—that is, user-facing text that is not contained in `.storyboard` or `.xib` files, such as error messages, needs to be translated into the current language before it’s presented to the user. iOS and OS X provide a mechanism to retrieve localized text from strings files at runtime. In your code, replace strings containing user-facing text with the return value of an `NSLocalizedString` macro. When you export localizations, Xcode searches your code for the macros and includes the strings files in the exported localization file for translation. When you import localizations, Xcode adds the strings files, used by your code, to your Xcode project. 

For example, instead of using the `@"26.22 miles"` string in your code, use:

NSLocalizedString(@"RunningDistance", @"distance for a marathon")where `@"RunningDistance"` is the key for text that is retrieved from a localized strings file. The `@"distance for a marathon"` parameter is a comment about the key-value pair stored in the strings file as a hint to localizers. If you want different behavior, use one of the other `NSLocalizedString` macros that take more parameters, described in *Foundation Functions Reference*.

> **Tip:** Don’t overload keys or compose phrases from multiple keys. Some languages have gender articles, adjective endings, and completely different word order. Instead, add a separate key-value pair to the strings file for all unique phrases.

For example, replace these key-value pairs:

```
/* Go to next page/chapter */
```

```
"GoToNext" = "Go to next %@";
```

```
"chapter" = "chapter";
```

```
"page" = "page";
```
with separate key-value pairs for each phrase:

```
/* Go to next chapter */
```

```
"GoToNextChapter" = "Go to next chapter";
```

```
 
```

```
/* Go to next page */
```

```
"GoToNextPage" = "Go to next page";
```
Don’t put numbers in localizable strings because different regions can use different numbers.

You don’t need to store all your key-value pairs in the same strings files. You can use other `NSLocalizedString` macros to create separate strings files and optionally, store them in different bundles. For more information on `NSLocalizedString` macros, read [String Resources](../../../../Cocoa/Conceptual/LoadingResources/Strings/Strings.html#//apple_ref/doc/uid/10000051i-CH6) in *[Resource Programming Guide](../../../../Cocoa/Conceptual/LoadingResources/Introduction/Introduction.html#//apple_ref/doc/uid/10000051i)*. 

To retrieve a localized string from a strings file, rather than adding it to a strings file, use the `[localizedStringForKey:value:table:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/instm/NSBundle/localizedStringForKey:value:table:)` method in the `[NSBundle](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/cl/NSBundle)` class. When the strings file corresponding to the specified table is not in your project, the `NSLocalizedString` macros and the `[localizedStringForKey:value:table:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/instm/NSBundle/localizedStringForKey:value:table:)` method return the value parameter as the localized string.

Later, when you import localizations, as described in [Importing Localizations](../LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW4), the localized strings files are added to your project. (Alternatively, you can generate the development language strings files from `NSLocalizedString` macros directly, as described in [Creating Strings Files for User-Facing Text in Your Code](../MaintaingYourOwnStringsFiles/MaintaingYourOwnStringsFiles.html#//apple_ref/doc/uid/10000171i-CH19-SW1).)

If your strings contain plurals of nouns or units of measurement, read [Handling Noun Plurals and Units of Measurement](../LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW10) for how to extend this mechanism for languages that have different plural rules.

## Using Unicode Strings
For all user-facing text, use string objects—instances of `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)`, `[NSAttributedString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSAttributedStrngClstr/Description.html#//apple_ref/occ/cl/NSAttributedString)`, and their subclasses—that support Unicode. Unicode is a standard for encoding characters from all the world’s writing systems. String objects encapsulate a Unicode string encoded in UTF-16 format. What the user sees as a character may be represented and encoded as multiple characters in a Unicode string. Therefore, use string methods that manipulate composed character sequences, not individual characters in a string. Use the appropriate string APIs for iteration, searching, and sorting too. Use standard views and controls that display Unicode string objects correctly.

For comprehensive documentation on string objects, read *[String Programming Guide](../../../../Cocoa/Conceptual/Strings/introStrings.html#//apple_ref/doc/uid/10000035i)*.

### Accessing Characters in Strings
The `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` class handles the complexity of character encoding for you by allowing you to access character clusters or ranges. Use the `[rangeOfComposedCharacterSequenceAtIndex:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/rangeOfComposedCharacterSequenceAtIndex:)` and `[rangeOfComposedCharacterSequencesForRange:](https://developer.apple.com/documentation/foundation/nsstring/1410993-rangeofcomposedcharactersequence)` methods to ensure that you don’t split user characters in a string and break the text. These methods return a range within a string that represents the user character.

For example, Table 3-1 shows the numeric representation of user characters in UTF-16 and UTF-32 encoding. Note that the user characters have different lengths no matter what encoding format you use.

**Table 3-1**  Unicode Encoding of User CharactersUser Character

UTF-16

UTF-32

D85E DFFD

27BFD

1100 1161 11A8

01100 01161 011A8

> **Video:** [WWDC 2013 Making Your App World-Ready: International Text > Composed Character Sequences](http://devstreaming.apple.com/videos/wwdc/2013/219xax4xjor8i6b9h77lafay32/219/ref.mov#t=47:30,48:20)

### Enumerating Strings
Enumerate strings by composed character sequence, word, sentence, or paragraph, not individual characters in a string. To enumerate a string by composed character sequence, use the `[enumerateSubstringsInRange:options:usingBlock:](https://developer.apple.com/documentation/foundation/nsstring/1416774-enumeratesubstringsinrange)` method and pass `[NSStringEnumerationByComposedCharacterSequences](https://developer.apple.com/documentation/foundation/nsstring/enumerationoptions/1407290-bycomposedcharactersequences)` as the options parameter. To enumerate a string by word (skipping over punctuation), pass `[NSStringEnumerationByWords](https://developer.apple.com/documentation/foundation/nsstring/enumerationoptions/1407663-bywords)` as the options parameter.

For example, if you pass `[NSStringEnumerationByComposedCharacterSequences](https://developer.apple.com/documentation/foundation/nsstring/enumerationoptions/1407290-bycomposedcharactersequences)` to the `[enumerateSubstringsInRange:options:usingBlock:](https://developer.apple.com/documentation/foundation/nsstring/1416774-enumeratesubstringsinrange)` method, it returns the user characters, as in the composed character sequences:

If the string is

and you pass `[NSStringEnumerationByWords](https://developer.apple.com/documentation/foundation/nsstring/enumerationoptions/1407663-bywords)` as the options parameter, the following words are returned:

Notice that spaces and punctuation are not included in the words.

> **Video:** [WWDC 2013 Making Your App World-Ready: International Text > String APIs: Iteration](http://devstreaming.apple.com/videos/wwdc/2013/219xax4xjor8i6b9h77lafay32/219/ref.mov#t=48:21,49:28)

### Searching Strings
To search the contents of a string or verify the presence of a string within a string using locale-sensitive comparison algorithms, use the `[rangeOfString:options:range:locale:](https://developer.apple.com/documentation/foundation/nsstring/1417348-range)` method, passing the current locale as the locale parameter. The constants you can combine and pass as the options parameter are:

	`[NSCaseInsensitiveSearch](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/econst/NSCaseInsensitiveSearch)`Case-insensitive search. For example, `‘B’` is the same as `‘b’`.

`[NSDiacriticInsensitiveSearch](https://developer.apple.com/documentation/foundation/nsstring/compareoptions/1412313-diacriticinsensitive)`Ignores diacritic marks. For example, `‘ö’` is equal to `‘o’`.

`[NSBackwardsSearch](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/econst/NSBackwardsSearch)`Search backwards. (The default is forwards.)

`[NSAnchoredSearch](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/econst/NSAnchoredSearch)`Search at the starting point.

For example, if you are searching for user text in a string, pass the `[NSCaseInsensitiveSearch](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/TypesAndConstants/FoundationTypesConstants/Description.html#//apple_ref/c/econst/NSCaseInsensitiveSearch)` and `[NSDiacriticInsensitiveSearch](https://developer.apple.com/documentation/foundation/nsstring/compareoptions/1412313-diacriticinsensitive)` constants as the options parameter to the `[rangeOfString:options:range:locale:](https://developer.apple.com/documentation/foundation/nsstring/1417348-range)` method. Typically, searching text is a case and diacritic insensitive operation, but sorting text is case and diacritic sensitive.

### Sorting Strings
For text you display to users, use locale-sensitive APIs for sorting and comparing strings. Different languages and regions have different sort order standards. For example, in French the diacritics are significant, and in English they are not. In some languages multiple letters are combined and affect the sort order. 

To use the locale-sensitive comparison algorithms, use the `[localizedStandardCompare:](https://developer.apple.com/documentation/foundation/nsstring/1409742-localizedstandardcompare)` method which produces the same results as the Finder.

If you don’t want the same results as the Finder, use the `[compare:options:range:locale:](https://developer.apple.com/documentation/foundation/nsstring/1414561-compare)` method, passing the current locale as the locale parameter, or the `[localizedCompare:](https://developer.apple.com/documentation/foundation/nsstring/1416999-localizedcompare)` method. 

Don’t use the `[localizedCaseInsensitiveCompare:](https://developer.apple.com/documentation/foundation/nsstring/1417333-localizedcaseinsensitivecompare)` method for sorting.

> **Video:** [WWDC 2013 Making Your App World-Ready: International Text > String APIs: Sorting](http://devstreaming.apple.com/videos/wwdc/2013/219xax4xjor8i6b9h77lafay32/219/ref.mov#t=51:15,51:50)

### Displaying Text
Use standard views and controls that handle the complexity of Unicode text layout and display for you. Characters in a string do not directly correspond to text rendered on the screen. What appears on the screen is a sequence of glyphs. A glyph is the smallest displayable unit in a font. A glyph may represent one character, more than one character, or part of a character. The mapping of characters to glyphs is not simple—it can be many-to-many. In addition, the order and position of glyphs in a line is complex. The standard views and controls even lay out bidirectional text properly for you—for example, the order of characters in a string containing an English word followed by a Hebrew word is not the same order used to lay out the text in a view, as described in [Handling Bidirectional Text](../SupportingRight-To-LeftLanguages/SupportingRight-To-LeftLanguages.html#//apple_ref/doc/uid/10000171i-CH17-SW4).

If you need to write custom display code, use the appropriate low-level text APIs. To learn about the text classes for iOS, read *[Text Programming Guide for iOS](../../../../StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009542)* and for Mac, read *[Text Layout Programming Guide](../../../../Cocoa/Conceptual/TextLayout/TextLayout.html#//apple_ref/doc/uid/10000158i)*.

### Parsing Text Input
The user might enter text in any language and format. iOS and OS X can recognize the language the user is typing and provide the appropriate keyboard options. If you are parsing text as the user enters it, keep in mind that there’s a many-to-many mapping from keyboard characters to language characters.

#### Parsing Language Characters
For some languages, the user doesn’t enter text one character at a time. That is, keys the user presses on a keyboard don’t necessarily correspond to characters in the language. In French, the user adds an accent at the insertion point by choosing it from a pop-up menu. In Japanese and Chinese languages, the user enters phonetic representations and selects a candidate from the candidate list to confirm the marked text. In both cases, preliminary text is inserted first and then converted to final text when the user confirms it.

> **Video:** [WWDC 2013 Making Your App World-Ready: International Text > Text Input](http://devstreaming.apple.com/videos/wwdc/2013/219xax4xjor8i6b9h77lafay32/219/ref.mov#t=54:33,56:02)

#### Determining When the User Confirms Marked Text (iOS Only)
To determine if the user confirmed marked text, send `[markedTextRange](https://developer.apple.com/documentation/uikit/uitextinput/1614489-markedtextrange)` to a text view. If this method returns an empty string, the user confirmed some entered text.

#### Determining the Typed Language (iOS Only)
To get the language that the user is currently typing, use the `[textInputMode](https://developer.apple.com/documentation/uikit/uiresponder/1621133-textinputmode)` property in the `[UIResponder](https://developer.apple.com/documentation/uikit/uiresponder)` class, as in:

NSString *languageID = [[[UIApplication sharedApplication] textInputMode] primaryLanguage];The returned string is a language ID, as described in [Language and Locale IDs](../LanguageandLocaleIDs/LanguageandLocaleIDs.html#//apple_ref/doc/uid/10000171i-CH15-SW1), that identifies a written language or dialect.

To get the set of languages that the user enables:

NSArray *languages = [[[UIApplication sharedApplication] textInputMode] activeInputModes];where the returned array contains instances of the `[UITextInputMode](https://developer.apple.com/documentation/uikit/uitextinputmode)` class.

## Detecting Personal Names, Mailing Addresses, and Phone Numbers
Worldwide, the format of personal names, mailing addresses, and phone numbers varies considerably. Personal names have many different formats including different ordering of the components. For example, in Asian countries, the family name is followed by the given name with no spaces between. The format of mailing addresses depends on the country. Phone numbers have different amounts of digits and punctuation between them. To handle varying input formats in your text views, use Interface Builder to add data detectors to your text views. A data detector identifies addresses and phone numbers in many different international formats and optionally turns them into links.

To detect this type of data in strings programmatically, read *[NSDataDetector Class Reference](https://developer.apple.com/documentation/foundation/nsdatadetector)*.

## Getting the Current Language
To get the language that the app is using from the main application bundle, use the `[preferredLocalizations](https://developer.apple.com/documentation/foundation/nsbundle/1413220-preferredlocalizations)` method in the `[NSBundle](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/cl/NSBundle)` class:

```
NSString *languageID = [[NSBundle mainBundle] preferredLocalizations].firstObject;
```

        
            [Next](../InternationalizingLocaleData/InternationalizingLocaleData.html)[Previous](../InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
        
         Copyright &#x00a9; 2015 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2015-09-16