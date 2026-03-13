---
title: "Naming Methods"
book: "Coding Guidelines for Cocoa"
chapterId: "20001282"
date: "2013-10-22"
description: "Provides naming guidelines for Cocoa API and design advice to framework developers."
identifier: "//apple_ref/doc/uid/10000146i"
source: apple-archive
---

# Naming Methods

> **Coding Guidelines for Cocoa**
> Last updated: 2013-10-22

[Next](NamingFunctions.html)[Previous](NamingBasics.html)
        
        
        [](../index.html)
        
        # Naming Methods
Methods are perhaps the most common element of your programming interface, so you should take particular care in how you name them. This section discusses the following aspects of method naming: 

## General Rules
Here are a few general guidelines to keep in mind when naming methods:

Start the name with a lowercase letter and capitalize the first letter of embedded words. Don’t use prefixes. See [Typographic Conventions](NamingBasics.html#//apple_ref/doc/uid/20001281-1002931).

There are two specific exceptions to these guidelines. You may begin a method name with a well-known acronym in uppercase (such as TIFF or PDF)), and you may use prefixes to group and identify private methods (see [Private Methods](#//apple_ref/doc/uid/20001282-1003829)).

For methods that represent actions an object takes, start the name with a verb:

- (void)invokeWithTarget:(id)target;
```
- (void)selectTabViewItem:(NSTabViewItem *)tabViewItem
```
Do not use “do” or “does” as part of the name because these auxiliary verbs rarely add meaning. Also, never use adverbs or adjectives before the verb.

If the method returns an attribute of the receiver, name the method after the attribute. The use of “get” is unnecessary, unless one or more values are returned indirectly.

`- (NSSize)cellSize;`

Right.

`- (NSSize)calcCellSize;`

Wrong.

`- (NSSize)getCellSize;`

Wrong.

See also [Accessor Methods](#//apple_ref/doc/uid/20001282-1004202).

Use keywords before all arguments.

`- (void)sendAction:(SEL)aSelector toObject:(id)anObject   forAllCells:(BOOL)flag;`

Right.

`- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;`

Wrong.

Make the word before the argument describe the argument.

`- (id)viewWithTag:(NSInteger)aTag;`

Right.

`- (id)taggedView:(int)aTag;`

Wrong.

Add new keywords to the end of an existing method when you create a method that is more specific than the inherited one.

`- (id)initWithFrame:(CGRect)frameRect;`

`NSView`, `UIView`.

`- (id)initWithFrame:(NSRect)frameRect mode:(int)aMode cellClass:(Class)factoryId numberOfRows:(int)rowsHigh numberOfColumns:(int)colsWide;`

NSMatrix, a subclass of NSView

Don’t use “and” to link keywords that are attributes of the receiver.

`- (int)runModalForDirectory:(NSString *)path file:(NSString *) name types:(NSArray *)fileTypes;`

Right.

`- (int)runModalForDirectory:(NSString *)path andFile:(NSString *)name andTypes:(NSArray *)fileTypes;`

Wrong.

Although “and” may sound good in this example, it causes problems as you create methods with more and more keywords.

If the method describes two separate actions, use “and” to link them.

`- (BOOL)openFile:(NSString *)fullPath withApplication:(NSString *)appName andDeactivate:(BOOL)flag;`

`NSWorkspace`.

## Accessor Methods
[Accessor methods](../../../../General/Conceptual/DevPedia-CocoaCore/AccessorMethod.html#//apple_ref/doc/uid/TP40008195-CH2) are those methods that set and return the value of a property of an object. They have certain recommended forms, depending on how the property is expressed:

If the property is expressed as a noun, the format is:

`- (`*type*`)`*noun*`;`

`- (void)set`*Noun*:`(`*type*`)`*aNoun*;

For example:

- (NSString *)title;
```
- (void)setTitle:(NSString *)aTitle;
```
If the property is expressed as an adjective, the format is:

`- (BOOL)is`*Adjective*`;`

`- (void)set`*Adjective*:`(BOOL)flag`;

For example:

- (BOOL)isEditable;
```
- (void)setEditable:(BOOL)flag;
```
If the property is expressed as a verb, the format is:

`- (BOOL)`*verbObject*`;`

`- (void)set`*VerbObject*:`(BOOL)flag`;

For example:

- (BOOL)showsAlpha;
```
- (void)setShowsAlpha:(BOOL)flag;
```
The verb should be in the simple present tense.

Don’t twist a verb into an adjective by using a participle:

`- (void)setAcceptsGlyphInfo:(BOOL)flag;`

Right.

`- (BOOL)acceptsGlyphInfo;`

Right.

`- (void)setGlyphInfoAccepted:(BOOL)flag;`

Wrong.

`- (BOOL)glyphInfoAccepted;`

Wrong.

You may use modal verbs (verbs preceded by “can”, “should”, “will”, and so on) to clarify meaning, but don’t use “do” or “does”.

`- (void)setCanHide:(BOOL)flag;`

Right.

`- (BOOL)canHide;`

Right.

`- (void)setShouldCloseDocument:(BOOL)flag;`

Right.

`- (BOOL)shouldCloseDocument;`

Right.

`- (void)setDoesAcceptGlyphInfo:(BOOL)flag;`

Wrong.

`- (BOOL)doesAcceptGlyphInfo;`

Wrong.

Use “get” only for methods that return objects and values indirectly. You should use this form for methods only when multiple items need to be returned. 

`- (void)getLineDash:(float *)pattern count:(int *)count phase:(float *)phase;`

`NSBezierPath`.

In methods such as these, the implementation should accept `NULL` for these in–out parameters as an indication that the caller is not interested in one or more of the returned values.

## Delegate Methods
Delegate methods (or delegation methods) are those that an object invokes in its [delegate](../../../../General/Conceptual/DevPedia-CocoaCore/Delegation.html#//apple_ref/doc/uid/TP40008195-CH14) (if the delegate implements them) when certain events occur. They have a distinctive form, which apply equally to methods invoked in an object’s data source:

Start the name by identifying the class of the object that’s sending the [message](../../../../General/Conceptual/DevPedia-CocoaCore/Message.html#//apple_ref/doc/uid/TP40008195-CH59):

- (BOOL)tableView:(NSTableView *)tableView shouldSelectRow:(int)row;
```
- (BOOL)application:(NSApplication *)sender openFile:(NSString *)filename;
```
The class name omits the prefix and the first letter is in lowercase.

A colon is affixed to the class name (the argument is a reference to the delegating object) unless the method has only one argument, the sender.

- (BOOL)applicationOpenUntitledFile:(NSApplication *)sender;An exception to this are methods that invoked as a result of a notification being posted. In this case, the sole argument is the notification object.

- (void)windowDidChangeScreen:(NSNotification *)notification;Use “did” or “will” for methods that are invoked to notify the delegate that something has happened or is about to happen.

- (void)browserDidScroll:(NSBrowser *)sender;
```
- (NSUndoManager *)windowWillReturnUndoManager:(NSWindow *)window;
```
Although you can use “did” or “will” for methods that are invoked to ask the delegate to do something on behalf of another object, “should” is preferred.

```
- (BOOL)windowShouldClose:(id)sender;
```

## Collection Methods
For objects that manage a [collection](../../../../General/Conceptual/DevPedia-CocoaCore/Collection.html#//apple_ref/doc/uid/TP40008195-CH10) of objects (each called an element of that collection), the convention is to have methods of the form:

`- (void)add`*Element*`:(`*elementType*`)`*anObj*`;`

`- (void)remove`*Element*`:(`*elementType*`)`*anObj*`;`

`- (NSArray *)`*elements*`;`

For example:

- (void)addLayoutManager:(NSLayoutManager *)obj;
```
- (void)removeLayoutManager:(NSLayoutManager *)obj;
```

```
- (NSArray *)layoutManagers;
```
The following are some qualifications and refinements to this guideline:

If the collection is truly unordered, return an NSSet object rather than an NSArray object.

If it’s important to insert elements into a specific location in the collection, use methods similar to the following instead of or in addition to the ones above:

- (void)insertLayoutManager:(NSLayoutManager *)obj atIndex:(int)index;
```
- (void)removeLayoutManagerAtIndex:(int)index;
```

There are a couple of implementation details to keep in mind with collection methods:

These methods typically imply ownership of the inserted objects, so the code that adds or inserts them must retain them, and the code that removes them must also release them. 

If the inserted objects need to have a pointer back to the main object, you do this (typically) with a `set...` method that sets the back pointer but does not retain. In the case of the `insertLayoutManager:atIndex:` method, the NSLayoutManager class does this in these methods:

- (void)setTextStorage:(NSTextStorage *)textStorage;
```
- (NSTextStorage *)textStorage;
```
You would normally not call `setTextStorage:` directly, but might want to [override](../../../../General/Conceptual/DevPedia-CocoaCore/MethodOverriding.html#//apple_ref/doc/uid/TP40008195-CH57) it.

Another example of the above conventions for collection methods comes from the NSWindow class:

```
- (void)addChildWindow:(NSWindow *)childWin ordered:(NSWindowOrderingMode)place;
```

```
- (void)removeChildWindow:(NSWindow *)childWin;
```

```
- (NSArray *)childWindows;
```

```
 
```

```
- (NSWindow *)parentWindow;
```

```
- (void)setParentWindow:(NSWindow *)window;
```
## Method Arguments
There are a few general rules concerning the names of method arguments:

As with methods, arguments start with a lowercase letter and the first letter of successive words are capitalized (for example, `removeObject:(id)anObject`).

Don’t use “pointer” or “ptr” in the name. Let the argument’s type rather than its name declare whether it’s a pointer.

Avoid one- and two-letter names for arguments.

Avoid abbreviations that save only a few letters.

Traditionally (in [Cocoa](../../../../General/Conceptual/DevPedia-CocoaCore/Cocoa.html#//apple_ref/doc/uid/TP40008195-CH9)), the following keywords and arguments are used together:

```
...action:(SEL)aSelector
```

```
...alignment:(int)mode
```

```
...atIndex:(int)index
```

```
...content:(NSRect)aRect
```

```
...doubleValue:(double)aDouble
```

```
...floatValue:(float)aFloat
```

```
...font:(NSFont *)fontObj
```

```
...frame:(NSRect)frameRect
```

```
...intValue:(int)anInt
```

```
...keyEquivalent:(NSString *)charCode
```

```
...length:(int)numBytes
```

```
...point:(NSPoint)aPoint
```

```
...stringValue:(NSString *)aString
```

```
...tag:(int)anInt
```

```
...target:(id)anObject
```

```
...title:(NSString *)aString
```
## Private Methods
In most cases, private method names generally follow the same rules as public method names. However, a common convention is to give private methods a prefix so it is easy to distinguish them from public methods. Even with this convention,  the names given to private methods can cause a peculiar type of problem. When you design a subclass of a Cocoa framework class, you cannot know if your private methods unintentionally override private framework methods that are identically named. 

Names of most private methods in the Cocoa frameworks have an underscore prefix (for example, `_fooData` ) to mark them as private. From this fact follow two recommendations. 

Don’t use the underscore character as a prefix for your private methods. Apple reserves this convention.

If you are subclassing a large Cocoa framework class (such as `NSView` or `UIView`) and you want to be absolutely sure that your private methods have names different from those in the superclass, you can add your own prefix to your private methods. The prefix should be as unique as possible, perhaps one based on your company or project and of the form "XX_". So if your project is called Byte Flogger, the prefix might be  `BF_addObject:`

Although the advice to give private method names a prefix might seem to contradict the earlier claim that methods exist in the namespace of their class, the intent here is different: to prevent unintentional overriding of superclass private methods. 

        
            [Next](NamingFunctions.html)[Previous](NamingBasics.html)
        
         Copyright &#x00a9; 2003, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-10-22