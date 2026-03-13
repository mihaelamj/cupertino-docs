---
title: "String Resources"
book: "Resource Programming Guide"
chapterId: "10000051i-CH6"
date: "2016-03-21"
description: "Explains how to work with nib and bundle resources in apps."
identifier: "//apple_ref/doc/uid/10000051i"
source: apple-archive
---

# String Resources

> **Resource Programming Guide**
> Last updated: 2016-03-21

[Next](../ImageSoundResources/ImageSoundResources.html)[Previous](../CocoaNibs/CocoaNibs.html)
        
        
        [](../index.html)
        
        # String Resources
An important part of the localization process is to localize all of the text strings displayed by your application. By their nature, strings located in [nib files](../../../../General/Conceptual/DevPedia-CocoaCore/NibFile.html#//apple_ref/doc/uid/TP40008195-CH34) can be readily localized along with the rest of the nib file contents. Strings embedded in your code, however, must be extracted, localized, and then reinserted back into your code. To simplify this process—and to make the maintenance of your code easier—OS X and iOS provide the infrastructure needed to separate strings from your code and place them into resource files where they can be localized easily.

Resource files that contain localizable strings are referred to as *strings* files because of their filename extension, which is `.strings`. You can create strings files manually or programmatically depending on your needs. The standard strings file format consists of one or more key-value pairs along with optional comments. The key and value in a given pair are strings of text enclosed in double quotation marks and separated by an equal sign. (You can also use a property list format for strings files. In such a case, the top-level node is a dictionary and each key-value pair of that dictionary is a string entry.) 

Listing 2-1 shows a simple strings file that contains non-localized entries for the default language. When you need to display a string, you pass the string on the left to one of the available string-loading routines. What you get back is the matching value string containing the text translation that is most appropriate for the current user. For the development language, it is common to use the same string for both the key and value, but doing so is not required.

**Listing 2-1**  A simple strings file

/* Insert Element menu item */
```
"Insert Element" = "Insert Element";
```

```
/* Error string used for unknown error types. */
```

```
"ErrorString_1" = "An unknown error occurred.";
```
A typical application has at least one strings file per localization, that is, one strings file in each of the [bundle’s](../../../../General/Conceptual/DevPedia-CocoaCore/Bundle.html#//apple_ref/doc/uid/TP40008195-CH4) `.lproj` subdirectories. The name of the default strings file is `Localizable.strings` but you can create strings files with any file name you choose. Creating strings files is discussed in more depth in [Creating Strings Resource Files](#//apple_ref/doc/uid/10000051i-CH6-SW5). 

> **Note:** It is recommended that you save strings files using the UTF-8 encoding, which is the default encoding for standard strings files. Xcode automatically transcodes strings files from UTF-8 to UTF-16 when they’re copied into the product. For more information about the standard strings file format, see [Creating Strings Resource Files](#//apple_ref/doc/uid/10000051i-CH6-SW5). For more information about Unicode and its text encodings, go to [http://www.unicode.org/](http://www.unicode.org/) or [http://en.wikipedia.org/wiki/Unicode](http://en.wikipedia.org/wiki/Unicode).

The loading of string resources (both localized and nonlocalized) ultimately relies on the bundle and internationalization support found in both OS X and iOS. For information about bundles, see *[Bundle Programming Guide](../../../../CoreFoundation/Conceptual/CFBundles/Introduction/Introduction.html#//apple_ref/doc/uid/10000123i)*. For more information about internationalization and localization, see *[Internationalization and Localization Guide](../../../../MacOSX/Conceptual/BPInternational/Introduction/Introduction.html#//apple_ref/doc/uid/10000171i)*. 

## Creating Strings Resource Files
Although you can create strings files manually, it is rarely necessary to do so. If you write your code using the appropriate string-loading macros, you can use the `genstrings` command-line tool to extract those strings and create strings files for you. 

The following sections describe the process of how to set up your source files to facilitate the use of the `genstrings` tool. For detailed information about the tool, see `genstrings` man page.

### Choosing Which Strings to Localize
When it comes to localizing your application’s interface, it is not always appropriate to localize every string used by your application. Translation is a costly process, and translating strings that are never seen by the user is a waste of time and money. Strings that are not displayed to the user, such as notification names used internally by your application, do not need to be translated. Consider the following example: 

if (CFStringHasPrefix(value, CFSTR("-")) {    CFArrayAppendValue(myArray, value);};In this example, the string “`-`” is used internally and is never seen by the user; therefore, it does not need to be placed in a strings file. 

The following code shows another example of a string the user would not see. The string `"%d %d %s"` does not need to be localized, since the user never sees it and it has no effect on anything that the user does see. 

matches = sscanf(s, "%d %d %s", &first, &last, &other);Because nib files are localized separately, you do not need to include strings that are already located inside of a nib file. Some of the strings you should localize, however, include the following:

Strings that are programmatically added to a window, panel, view, or control and subsequently displayed to the user. This includes strings you pass into standard routines, such as those that display alert boxes.

Menu item title strings if those strings are added programmatically. For example, if you use custom strings for the Undo menu item, those strings should be in a strings file. 

Error messages that are displayed to the user.

Any boilerplate text that is displayed to the user.

Some strings from your application’s information property list (`Info.plist`) file; see *[Runtime Configuration Guidelines](../../../../MacOSX/Conceptual/BPRuntimeConfig/000-Introduction/introduction.html#//apple_ref/doc/uid/10000170i)*.

New file and document names.

### About the String-Loading Macros
The Foundation and Core Foundation [frameworks](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56) define the following macros to make loading strings from a strings file easier:

Core Foundation macros:

`[CFCopyLocalizedString](https://developer.apple.com/documentation/corefoundation/cfcopylocalizedstring)`

`[CFCopyLocalizedStringFromTable](https://developer.apple.com/documentation/corefoundation/cfcopylocalizedstringfromtable)`

`[CFCopyLocalizedStringFromTableInBundle](https://developer.apple.com/documentation/corefoundation/cfcopylocalizedstringfromtableinbundle)`

`[CFCopyLocalizedStringWithDefaultValue](https://developer.apple.com/documentation/corefoundation/cfcopylocalizedstringwithdefaultvalue)`

Foundation macros:

`[NSLocalizedString](https://developer.apple.com/documentation/foundation/nslocalizedstring)`

`[NSLocalizedStringFromTable](https://developer.apple.com/documentation/foundation/nslocalizedstringfromtable)`

`[NSLocalizedStringFromTableInBundle](https://developer.apple.com/documentation/foundation/nslocalizedstringfromtableinbundle)`

`[NSLocalizedStringWithDefaultValue](https://developer.apple.com/documentation/foundation/nslocalizedstringwithdefaultvalue)`

You use these macros in your source code to load strings from one of your application’s strings files. The macros take the user’s current language preferences into account when retrieving the actual string value. In addition, the `genstrings` tool searches for these macros and uses the information they contain to build the initial set of strings files for your application.

For additional information about how to use these macros, see [Loading String Resources Into Your Code](#//apple_ref/doc/uid/20000005-97055).

### Using the genstrings Tool to Create Strings Files
At some point during your development, you need to create the strings files needed by your code. If you wrote your code using the Core Foundation and Foundation macros, the simplest way to create your strings files is using the `genstrings` command-line tool. You can use this tool to generate a new set of strings files or update a set of existing files based on your source code. 

To use the `genstrings` tool, you typically provide at least two arguments:

A list of source files

An optional output directory

The `genstrings` tool can parse C, [Objective-C](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectiveC.html#//apple_ref/doc/uid/TP40008195-CH43), and Java code files with the `.c`, `.m`, or `.java` filename extensions. Although not strictly required, specifying an output directory is recommended and is where `genstrings` places the resulting strings files. In most cases, you would want to specify the directory containing the project resources for your development language. 

The following example shows a simple command for running the `genstrings` tool. This command causes the tool to parse all Objective-C source files in the current directory and put the resulting strings files in the `en.lproj` subdirectory, which must already exist. 

genstrings -o en.lproj *.mThe first time you run the `genstrings` tool, it creates a set of new strings files for you. Subsequent runs replace the contents of those strings files with the current string entries found in your source code. For subsequent runs, it is a good idea to save a copy of your current strings files before running `genstrings`. You can then diff the new and old versions to determine which strings were added to (or changed in) your project. You can then use this information to update any already localized versions of your strings files, rather than replacing those files and localizing them again.   

Within a single strings file, each key must be unique. Fortunately, the `genstrings` tool is smart enough to coalesce any duplicate entries it finds. When it discovers a key string used more than once in a single strings file, the tool merges the comments from the individual entries into one comment string and generates a warning. (You can suppress the duplicate entries warning with the `-q` option.) If the same key string is assigned to strings in different strings files, no warning is generated. 

For more information about using the `genstrings` tool, see the `genstrings` man page. 

### Creating Strings Files Manually
Although the `genstrings` tool is the most convenient way to create strings files, you can also create them manually. To create a strings file manually, create a new file in TextEdit (or your preferred text-editing application) and save it using the Unicode UTF-8 encoding. (When saving files, TextEdit usually chooses an appropriate encoding by default. To force a specific encoding, you must change the save options in the application preferences.) The contents of this file consists of a set of key-value pairs along with optional comments describing the purpose of each key-value pair. Key and value strings are separated by an equal sign, and the entire entry must be terminated with a semicolon character. By convention, comments are enclosed inside C-style comment delimiters (`/*` and `*/`) and are placed immediately before the entry they describe.  

Listing 2-2 shows the basic format of a strings file. The entries in this example come from the English version of the `Localizable.strings` file from the TextEdit application. The string on the left side of each equal sign represents the key, and the string on the right side represents the value. A common convention when developing applications is to use a key name that equals the value in the language used to develop the application. Therefore, because TextEdit was developed using the English language, the English version of the `Localizable.strings` file has keys and values that match.

**Listing 2-2**  Strings localized for English

/* Menu item to make the current document plain text */
```
"Make Plain Text" = "Make Plain Text";
```

```
/* Menu item to make the current document rich text */
```

```
"Make Rich Text" = "Make Rich Text";
```
Listing 2-3 shows the German translation of the same entries. These entries also live inside a file called `Localizable.strings`, but this version of the file is located in the German language project directory of the TextEdit application. Notice that the keys are still in English, but the values assigned to those keys are in German. This is because the key strings are never seen by end users. They are used by the code to retrieve the corresponding value string, which in this case is in German. 

**Listing 2-3**  Strings localized for German

```
/* Menu item to make the current document plain text */
```

```
"Make Plain Text" = "In reinen Text umwandeln";
```

```
/* Menu item to make the current document rich text */
```

```
"Make Rich Text" = "In formatierten Text umwandeln";
```
### Detecting Non-localizable Strings
AppKit–based applications can take advantage of built-in support to detect strings that do not need to be localized and those that need to be localized but currently are not. To use this built-in support, set user defaults or add launch arguments when running your app. Specify a Boolean value to indicate whether the user default should be enabled or disabled. The available user defaults are as follows:

The `NSShowNonLocalizableStrings` user default identifies strings that are not localizable. The strings are logged to the shell in upper case. This option occasionally generates some false positives but is still useful overall. 

The `NSShowNonLocalizedStrings` user default locates strings that were meant to be localized but could not be found in the application’s existing strings files. You can use this user default to catch problems with out-of-date localizations. 

For example, to use the `NSShowNonLocalizedStrings` user default with the TextEdit application, enter the following in Terminal:

```
/Applications/TextEdit.app/Contents/MacOS/TextEdit -NSShowNonLocalizedStrings YES
```
## Loading String Resources Into Your Code
The Core Foundation and Foundation frameworks provide macros for retrieving both localized and nonlocalized strings stored in strings files. Although the main purpose of these macros is to load strings at runtime, they also serve a secondary purpose by acting as markers that the `genstrings` tool can use to locate your application’s string resources. It is this second purpose that explains why many of the macros let you specify much more information than would normally be required for loading a string. The `genstrings` tool uses the information you provide to create or update your application’s strings files automatically. Table 2-1 lists the types of information you can specify for these routines and describes how that information is used by the `genstrings` tool. 

**Table 2-1**  Common parameters found in string-loading routinesParameter

Description

Key

The string used to look up the corresponding value. This string must not contain any characters from the extended ASCII character set, which includes accented versions of ASCII characters. If you want the initial value string to contain extended ASCII characters, use a routine that lets you specify a default value parameter. (For information about the extended ASCII character set, see the corresponding [Wikipedia entry](http://en.wikipedia.org/wiki/Extended_ASCII).) 

Table name

The name of the strings file in which the specified key is located. The `genstrings` tool interprets this parameter as the name of the strings file in which the string should be placed. If no table name is provided, the string is placed in the default `Localizable.strings` file. (When specifying a value for this parameter, include the filename without the `.strings` extension.)

A `.strings` file whose table name ends with `.nocache`—for example `ErrorNames.nocache.strings`—will not have its contents cached by `[NSBundle](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/cl/NSBundle)`.

Default value

The default value to associate with a given key. If no default value is specified, the `genstrings` tool uses the key string as the initial value. Default value strings may contain extended ASCII characters. 

Comment

Translation comments to include with the string. You can use comments to provide clues to the translation team about how a given string is used. The `genstrings` tool puts these comments in the strings file and encloses them in C-style comment delimiters (`/*` and `*/`) immediately above the associated entry.  

Bundle

An `[NSBundle](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/cl/NSBundle)` object or `[CFBundleRef](https://developer.apple.com/documentation/corefoundation/cfbundle)` type corresponding to the bundle containing the strings file. You can use this to load strings from bundles other than your application’s main bundle. For example, you might use this to load localized strings from a [framework](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56) or plug-in.

When you request a string from a strings file, the string that is returned depends on the available localizations (if any). The Cocoa and Core Foundation macros use the built-in [bundle](../../../../General/Conceptual/DevPedia-CocoaCore/Bundle.html#//apple_ref/doc/uid/TP40008195-CH4) internationalization support to retrieve the string whose localization matches the user’s current language preferences. As long as your localized resource files are placed in the appropriate language-specific project directories, loading a string with these macros should yield the appropriate string automatically. If no appropriate localized string resource is found, the bundle’s loading code automatically chooses the appropriate nonlocalized string instead. 

For information about internationalization in general and how to create language-specific project directories, see *[Internationalization and Localization Guide](../../../../MacOSX/Conceptual/BPInternational/Introduction/Introduction.html#//apple_ref/doc/uid/10000171i)*. For information about the bundle structure and how resource files are chosen from a bundle directory, see *[Bundle Programming Guide](../../../../CoreFoundation/Conceptual/CFBundles/Introduction/Introduction.html#//apple_ref/doc/uid/10000123i)*.

### Using the Core Foundation Framework
The Core Foundation framework defines a single function and several macros for loading localized strings from your application bundle. The `[CFBundleCopyLocalizedString](https://developer.apple.com/documentation/corefoundation/1537103-cfbundlecopylocalizedstring)` function provides the basic implementation for retrieving the strings. However, it is recommended that you use the following macros instead:

`[CFCopyLocalizedString](https://developer.apple.com/documentation/corefoundation/cfcopylocalizedstring)``(key, comment)`

`[CFCopyLocalizedStringFromTable](https://developer.apple.com/documentation/corefoundation/cfcopylocalizedstringfromtable)``(key, tableName, comment)`

`[CFCopyLocalizedStringFromTableInBundle](https://developer.apple.com/documentation/corefoundation/cfcopylocalizedstringfromtableinbundle)``(key, tableName, bundle, comment)`

`[CFCopyLocalizedStringWithDefaultValue](https://developer.apple.com/documentation/corefoundation/cfcopylocalizedstringwithdefaultvalue)``(key, tableName, bundle, value, comment)`

There are several reasons to use the macros instead of the `CFBundleCopyLocalizedString` function. First, the macros are easier to use for certain common cases. Second, the macros let you associate a comment string with the string entry. Third, the macros are recognized by the `genstrings` tool but the `CFBundleCopyLocalizedString` function is not.

For information about the syntax of the preceding macros, see *[CFBundle Reference](https://developer.apple.com/documentation/corefoundation/cfbundle-s0a)*.

### Using the Foundation Framework
The Foundation framework defines a single method and several macros for loading string resources. The `[localizedStringForKey:value:table:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSBundle/Description.html#//apple_ref/occ/instm/NSBundle/localizedStringForKey:value:table:)` method of the `NSBundle` class loads the specified string resource from a strings file residing in the current bundle. Cocoa also defines the following macros for getting localized strings:

`[NSLocalizedString](https://developer.apple.com/documentation/foundation/nslocalizedstring)``(key, comment)`

`[NSLocalizedStringFromTable](https://developer.apple.com/documentation/foundation/nslocalizedstringfromtable)``(key, tableName, comment)`

`[NSLocalizedStringFromTableInBundle](https://developer.apple.com/documentation/foundation/nslocalizedstringfromtableinbundle)``(key, tableName, bundle, comment)`

`[NSLocalizedStringWithDefaultValue](https://developer.apple.com/documentation/foundation/nslocalizedstringwithdefaultvalue)``(key, tableName, bundle, value, comment)`

As with Core Foundation, Apple recommends that you use the Cocoa convenience macros for loading strings. The main advantage to these macros is that they can be parsed by the `genstrings` tool and used to create your application’s strings files. They are also simpler to use and let you associate translation comments with each entry.

For information about the syntax of the preceding macros, see *Foundation Functions Reference*. Additional methods for loading strings are also defined in *[NSBundle Class Reference](https://developer.apple.com/documentation/foundation/nsbundle)*.

### Examples of Getting Strings
The following examples demonstrate the basic techniques for using the Foundation and Core Foundation macros to retrieve strings. Each example assumes that the current bundle contains a strings file with the name `Custom.strings` that has been translated into French. This translated file includes the following strings:

/* A comment */
```
"Yes" = "Oui";
```

```
"The same text in English" = "Le même texte en anglais";
```
Using the Foundation framework, you can get the value of the “`Yes`” string using the `[NSLocalizedStringFromTable](https://developer.apple.com/documentation/foundation/nslocalizedstringfromtable)` macro, as shown in the following example:

NSString* theString;
```
theString = NSLocalizedStringFromTable (@"Yes", @"Custom", @"A comment");
```
Using the Core Foundation framework, you could get the same string using the `[CFCopyLocalizedStringFromTable](https://developer.apple.com/documentation/corefoundation/cfcopylocalizedstringfromtable)` macro, as shown in this example:

CFStringRef theString;
```
theString = CFCopyLocalizedStringFromTable(CFSTR("Yes"), CFSTR("Custom"), "A comment");
```
In both examples, the code specifies the key to retrieve, which is the string “Yes”. They also specify the strings file (or table) in which to look for the key, which in this case is the `Custom.strings` file. During string retrieval, the comment string is ignored. 

## Advanced Strings File Tips
The following sections provide some additional tips for working with strings files and string resources. 

### Searching for Custom Functions With genstrings
The `genstrings` tool searches for the Core Foundation and Foundation string macros by default. It uses the information in these macros to create the string entries in your project’s strings files. You can also direct `genstrings` to look for custom string-loading functions in your code and use those functions in addition to the standard macros. You might use custom functions to wrap the built-in string-loading routines and perform some extra processing or you might replace the default string handling behavior with your own custom model.

If you want to use `genstrings` with your own custom functions, your functions must use the naming and formatting conventions used by the Foundation macros. The parameters for your functions must match the parameters for the corresponding macros exactly. When you invoke genstrings, you specify the `-s` option followed by the name of the function that corresponds to the `[NSLocalizedString](https://developer.apple.com/documentation/foundation/nslocalizedstring)` macro. Your other function names should then build from this base name. For example, if you specified the function name `MyStringFunction`, your other function names should be  `MyStringFunctionFromTable`, `MyStringFunctionFromTableInBundle`, and `MyStringFunctionWithDefaultValue`. The `genstrings` tool looks for these functions and uses them to build the corresponding strings files.

### Formatting String Resources
For some strings, you may not want to (or be able to) encode the entire string in a string resource because portions of the string might change at runtime. For example, if a string contains the name of a user document, you need to be able to insert that document name into the string dynamically. When creating your string resources, you can use any of the formatting characters you would normally use for handling string replacement in the Foundation and Core Foundation frameworks. Listing 2-4 shows several string resources that use basic formatting characters: 

**Listing 2-4**  Strings with formatting characters

"Windows must have at least %d columns and %d rows." =
```
"Les fenêtres doivent être composes au minimum de %d colonnes et %d lignes.";
```

```
"File %@ not found." = "Le fichier %@ n’existe pas.";
```
To replace formatting characters with actual values, you use the `[stringWithFormat:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/clm/NSString/stringWithFormat:)` method of `NSString` or the `[CFStringCreateWithFormat](https://developer.apple.com/documentation/corefoundation/1563243-cfstringcreatewithformat)` function, using the string resource as the format string. Foundation and Core Foundation support most of the standard formatting characters used in `printf` statements. In addition, you can use the `%@` specifier shown in the preceding example to insert the descriptive text associated with arbitrary Objective-C objects. See [Formatting String Objects](../../Strings/Articles/FormatStrings.html#//apple_ref/doc/uid/20000943) in *[String Programming Guide](../../Strings/introStrings.html#//apple_ref/doc/uid/10000035i)* for the complete list of specifiers. 

One problem that often occurs during translation is that the translator may need to reorder parameters inside translated strings to account for differences in the source and target languages. If a string contains multiple arguments, the translator can insert special tags of the form *n*`$` (where *n* specifies the position of the original argument) in between the formatting characters. These tags let the translator reorder the arguments that appear in the original string. The following example shows a string whose two arguments are reversed in the translated string: 

```
/* Message in alert dialog when something fails */
```

```
"%@ Error! %@ failed!" = "%2$@ blah blah, %1$@ blah!";
```
### Using Special Characters in String Resources
Just as in C, some characters must be prefixed with a backslash before you can include them in the string. These characters include double quotation marks, the backslash character itself, and special control characters such as linefeed (`\n`) and carriage returns (`\r`).

"File \"%@\" cannot be opened" = " ... ";
```
"Type \"OK\" when done" = " ... ";
```
You can include arbitrary Unicode characters in a value string by specifying `\U` followed immediately by up to four hexadecimal digits. The four digits denote the entry for the desired Unicode character; for example, the space character is represented by hexadecimal 20 and thus would be `\U0020` when specified as a Unicode character. This option is useful if a string must include Unicode characters that for some reason cannot be typed. If you use this option, you must also pass the `-u` option to `genstrings` in order for the hexadecimal digits to be interpreted correctly in the resulting strings file. The `genstrings` tool assumes your strings are low-ASCII by default and only interprets backslash sequences if the `-u` option is specified. 

> **Note:** If you generate your strings files yourself, such as by using `genstrings`, make sure that these strings files end up in the UTF-8 encoding before adding them to your project. 

### Debugging Strings Files
If you run into problems during testing and find that the functions and macros for retrieving strings are always returning the same key (as opposed to the translated value), run the `/usr/bin/plutil` tool on your strings file. A strings file is essentially a property-list file formatted in a special way. Running `plutil` with the `-lint` option can uncover hidden characters or other errors that are preventing strings from being retrieved correctly.

 
        
            [Next](../ImageSoundResources/ImageSoundResources.html)[Previous](../CocoaNibs/CocoaNibs.html)
        
         Copyright &#x00a9; 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-03-21