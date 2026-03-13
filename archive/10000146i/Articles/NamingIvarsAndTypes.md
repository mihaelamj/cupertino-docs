---
title: "Naming Properties and Data Types"
book: "Coding Guidelines for Cocoa"
chapterId: "20001284"
date: "2013-10-22"
description: "Provides naming guidelines for Cocoa API and design advice to framework developers."
identifier: "//apple_ref/doc/uid/10000146i"
source: apple-archive
---

# Naming Properties and Data Types

> **Coding Guidelines for Cocoa**
> Last updated: 2013-10-22

[Next](APIAbbreviations.html)[Previous](NamingFunctions.html)
        
        
        [](../index.html)
        
        # Naming Properties and Data Types

This section describes the naming [conventions](../../../../General/Conceptual/DevPedia-CocoaCore/CodingConventions.html#//apple_ref/doc/uid/TP40008195-CH53) for declared properties, instance variables, constants, [notifications](../../../../General/Conceptual/DevPedia-CocoaCore/Notification.html#//apple_ref/doc/uid/TP40008195-CH35), and [exceptions](../../../../General/Conceptual/DevPedia-CocoaCore/ExceptionHandling.html#//apple_ref/doc/uid/TP40008195-CH18).

## Declared Properties and Instance Variables
A [declared property](../../../../General/Conceptual/DevPedia-CocoaCore/DeclaredProperty.html#//apple_ref/doc/uid/TP40008195-CH13) effectively declares accessor methods for a property, and so conventions for naming a declared property are broadly the same as those for naming accessor methods (see [Accessor Methods](NamingMethods.html#//apple_ref/doc/uid/20001282-1004202)). If the property is expressed as a noun or a verb, the format is:

`@property (…) `*type* *nounOrVerb*;

For example:

@property (strong) NSString *title;
```
@property (assign) BOOL showsAlpha;
```
If the name of a declared property is expressed as an adjective, however, the property name omits the “is” prefix but specifies the conventional name for the get accessor, for example:

@property (assign, getter=isEditable) BOOL editable;In many cases, when you use a declared property you also synthesize a corresponding instance variable.

Make sure the name of the instance variable concisely describes the attribute stored. Usually, you should not access instance variables directly; instead you should use [accessor methods](../../../../General/Conceptual/DevPedia-CocoaCore/AccessorMethod.html#//apple_ref/doc/uid/TP40008195-CH2) (you do access instance variables directly in `init` and `dealloc` methods). To help to signal this, prefix instance variable names with an underscore (`_`), for example:

```
@implementation MyClass {
```

```
    BOOL _showsTitle;
```

```
}
```

If you synthesize the instance variable using a declared property, specify the name of the instance variable in the `@synthesize` statement.

@implementation MyClass
```
@synthesize showsTitle=_showsTitle;
```
There are a few considerations to keep in mind when adding instance variables to a class:

Avoid explicitly declaring public instance variables.

Developers should concern themselves with an object’s interface, not with the details of how it stores its data. You can avoid declaring instance variables explicitly by using declared properties and synthesizing the corresponding instance variable.

If you need to declare an instance variable, explicitly declare it with either `@private` or `@protected`.

If you expect that your class will be subclassed, and that these subclasses will require direct access to the data, use the `@protected` directive.

If an instance variable is to be an accessible attribute of instances of the class, make sure you write [accessor methods](../../../../General/Conceptual/DevPedia-CocoaCore/AccessorMethod.html#//apple_ref/doc/uid/TP40008195-CH2) for it (when possible, use declared properties).

## Constants

The rules for constants vary according to how the constant is created.

### Enumerated constants

Use enumerations for groups of related constants that have integer values.

Enumerated constants *and* the typedef under which they are grouped follow the naming conventions for functions (see [Naming Functions](NamingFunctions.html#//apple_ref/doc/uid/20001283-BAJGGCAD)). The following example comes from `NSMatrix.h` :

typedef enum _NSMatrixMode {
```
    NSRadioModeMatrix           = 0,
```

```
    NSHighlightModeMatrix       = 1,
```

```
    NSListModeMatrix            = 2,
```

```
    NSTrackModeMatrix           = 3
```

```
} NSMatrixMode;
```
Note that the `typedef` tag (`_NSMatrixMode` in the above example) is unnecessary.

You can create unnamed enumerations for things like bit masks, for example:

```
enum {
```

```
    NSBorderlessWindowMask      = 0,
```

```
    NSTitledWindowMask          = 1 << 0,
```

```
    NSClosableWindowMask        = 1 << 1,
```

```
    NSMiniaturizableWindowMask  = 1 << 2,
```

```
    NSResizableWindowMask       = 1 << 3
```

```
 
```

```
};
```

### Constants created with const

Use `const` to create constants for floating point values. You can use `const` to create an integer constant if the constant is unrelated to other constants; otherwise, use enumeration.

The format for `const` constants is exemplified by the following declaration:

const float NSLightGray;As with enumerated constants, the naming conventions are the same as for functions (see [Naming Functions](NamingFunctions.html#//apple_ref/doc/uid/20001283-BAJGGCAD)).

### Other types of constants

In general, don’t use the `#define` preprocessor command to create constants. For integer constants, use enumerations, and for floating point constants use the `const` qualifier, as described above.

Use uppercase letters for symbols that the preprocessor evaluates in determining whether a block of code will be processed. For example:

```
#ifdef DEBUG
```

Note that macros defined by the compiler have leading and trailing double underscore characters. For example:

```
__MACH__
```

Define constants for strings used for such purposes as notification names and dictionary keys. By using string constants, you are ensuring that the compiler  verifies the proper value is specified (that is, it performs spell checking). The [Cocoa](../../../../General/Conceptual/DevPedia-CocoaCore/Cocoa.html#//apple_ref/doc/uid/TP40008195-CH9)  [frameworks](../../../../General/Conceptual/DevPedia-CocoaCore/Framework.html#//apple_ref/doc/uid/TP40008195-CH56) provide many examples of string constants, such as:

APPKIT_EXTERN NSString *NSPrintCopies;The actual NSString value is assigned to the constant in an implementation file. (Note that the `APPKIT_EXTERN` macro evaluates to `extern` for [Objective-C](../../../../General/Conceptual/DevPedia-CocoaCore/ObjectiveC.html#//apple_ref/doc/uid/TP40008195-CH43).)

## Notifications and Exceptions

The names for [notifications](../../../../General/Conceptual/DevPedia-CocoaCore/Notification.html#//apple_ref/doc/uid/TP40008195-CH35) and [exceptions](../../../../General/Conceptual/DevPedia-CocoaCore/ExceptionHandling.html#//apple_ref/doc/uid/TP40008195-CH18) follow similar rules. But both have their own recommended usage patterns.

### Notifications

If a class has a [delegate](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14), most of its notifications will probably be received by the delegate through a defined delegate method. The names of these notifications should reflect the corresponding delegate method. For example, a delegate of the global `NSApplication` object is automatically registered to receive an `applicationDidBecomeActive:` message whenever the application posts an `NSApplicationDidBecomeActiveNotification`.

Notifications are identified by global `NSString` objects whose names are composed in this way:

```
[Name of associated class] + [Did | Will] + [UniquePartOfName] + Notification
```

For example:

```
NSApplicationDidBecomeActiveNotification
```

```
NSWindowDidMiniaturizeNotification
```

```
NSTextViewDidChangeSelectionNotification
```

```
NSColorPanelColorDidChangeNotification
```

### Exceptions

Although you are free to use exceptions (that is, the mechanisms offered by the `NSException` class and related functions) for any purpose you choose, Cocoa reserves exceptions for programming errors such an array index being out of bounds. Cocoa does *not* use exceptions to handle regular, expected error conditions. For these cases, use returned values such as `nil`, `NULL`, `NO`, or error codes. For more details, see *[Error Handling Programming Guide](../../ErrorHandlingCocoa/ErrorHandling/ErrorHandling.html#//apple_ref/doc/uid/TP40001806)*.

Exceptions are identified by global `NSString` objects whose names are composed in this way:

```
[Prefix] + [UniquePartOfName] + Exception
```

The unique part of the name should run constituent words together and capitalize the first letter of each word. Here are some examples:

```
NSColorListIOException
```

```
NSColorListNotEditableException
```

```
NSDraggingException
```

```
NSFontUnavailableException
```

```
NSIllegalSelectorException
```

        
            [Next](APIAbbreviations.html)[Previous](NamingFunctions.html)
        
         Copyright &#x00a9; 2003, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-10-22