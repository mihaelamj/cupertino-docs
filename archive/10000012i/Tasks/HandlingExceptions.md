---
title: "Handling Exceptions"
book: "Exception Programming Topics"
chapterId: "20000059"
date: "2013-08-08"
description: "Explains how to raise and handle exceptions in Cocoa applications."
identifier: "//apple_ref/doc/uid/10000012i"
source: apple-archive
---

# Handling Exceptions

> **Exception Programming Topics**
> Last updated: 2013-08-08

[Next](RaisingExceptions.html)[Previous](../Articles/ExceptionsAndCocoaFrameworks.html)
        
        
        [](../index.html)
        
        # Handling Exceptions
The exception handling mechanisms available to Objective-C programs are effective ways of dealing with exceptional conditions. They decouple the detection and handling of these conditions and automate the propagation of the exception from the point of detection to the point of handling. As a result, your code can be much cleaner, easier to write correctly, and easier to maintain.

## Handling Exceptions Using Compiler Directives
Compiler support for exceptions is based on four compiler directives:

`@try` —Defines a block of code that is an exception handling domain: code that can potentially throw an exception. 

`@catch()` —Defines a block containing code for handling the exception thrown in the `@try` block. The parameter of `@catch` is the exception object thrown locally; this is usually an `NSException` object, but can be other types of objects, such as `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` objects.

`@finally` — Defines a block of related code that is subsequently executed whether an exception is thrown or not.

`@throw` — Throws an exception; this directive is almost identical in behavior to the `[raise](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/instm/NSException/raise)` method of `NSException`. You usually throw `NSException` objects, but are not limited to them. For more information about `@throw`, see [Throwing Exceptions](RaisingExceptions.html#//apple_ref/doc/uid/20000058-BBCCFIBF).

> **Important:** Although you can throw and catch objects other than `NSException` objects, the Cocoa frameworks themselves might only catch `NSException` objects for some conditions. So if you throw other types of objects, the Cocoa handlers for that exception might not run, with undefined results. (Conversely, non-`NSException` objects that you throw could be caught by some Cocoa handlers.) For these reasons, it is recommended that you throw `NSException` objects only, while being prepared to catch exception objects of all types.

The `@try`, `@catch`, and `@finally` directives constitute a control structure. The section of code between the braces in `@try` is the exception handling domain; the code in a `@catch` block is a local exception handler; the `@finally` block of code is a common “housekeeping” section. In Figure 1, the normal flow of program execution is marked by the gray arrow; the code within the local exception handler is executed only if an exception is thrown—either by the local exception handling domain or one further down the call sequence. The throwing (or raising) of an exception causes program control to jump to the first executable line of the local exception handler. After the exception is handled, control “falls through” to the `@finally` block; if no exception is thrown, control jumps from the `@try` block to the `@finally` block. 

**Figure 1**  Flow of exception handling using compiler directivesWhere and how an exception is handled depends on the context where the exception was raised (although most exceptions in most programs go uncaught until they reach the top-level handler installed by the shared `[NSApplication](https://developer.apple.com/documentation/appkit/nsapplication)` or `[UIApplication](https://developer.apple.com/documentation/uikit/uiapplication)` object). In general, an exception object is thrown (or raised) within the domain of an exception handler. Although you can throw an exception directly within a local exception handling domain, an exception is more likely thrown (through `@throw` or `[raise](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/instm/NSException/raise)`) indirectly from a method invoked from the domain. No matter how deep in a call sequence the exception is thrown, execution jumps to the local exception handler (assuming there are no intervening exception handlers, as discussed in [Nesting Exception Handlers](NestingExceptionHandlers.html#//apple_ref/doc/uid/20000060-CJBBFDIF)). In this way, exceptions raised at a low level can be caught at a high level. 

Listing 1 illustrates how you might use the `@try`, `@catch`, and `@finally` compiler directives. In this example, the `@catch` block handles any exception thrown lower in the calling sequence as a consequence of the `[setValue:forKeyPath:](https://developer.apple.com/documentation/objectivec/nsobject/1418139-setvalue)` message by setting the affected property to `nil` instead. The message in the `@finally` block is sent whether an exception is thrown or not.

**Listing 1**  Handling an exception using compiler directives

- (void)endSheet:(NSWindow *)sheet
```
{
```

```
    BOOL success = [predicateEditorView commitEditing];
```

```
    if (success == YES) {
```

```
 
```

```
        @try {
```

```
            [treeController setValue:[predicateEditorView predicate] forKeyPath:@"selection.predicate"];
```

```
        }
```

```
 
```

```
        @catch ( NSException *e ) {
```

```
            [treeController setValue:nil forKeyPath:@"selection.predicate"];
```

```
        }
```

```
 
```

```
        @finally {
```

```
            [NSApp endSheet:sheet];
```

```
        }
```

```
    }
```

```
}
```
One way to handle exceptions is to “promote” them to error messages that either inform users or request their intervention.  You can convert an exception into an `[NSError](https://developer.apple.com/documentation/foundation/nserror)` object and then present the information in the error object to the user in an alert panel. In OS X, you could also hand this object over to the Application Kit’s error-handling mechanism for display to users. You can also return them indirectly in methods that include an error parameter. Listing 2 shows an example of the latter in an Automator action’s implementation of `[runWithInput:fromAction:error:](https://developer.apple.com/documentation/automator/amaction/1438363-runwithinput)` (in this case the error parameter is a pointer to an `[NSDictionary](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDictionaryClassClstr/Description.html#//apple_ref/occ/cl/NSDictionary)` object rather than an `NSError` object). 

**Listing 2**  Converting an exception into an error

- (id)runWithInput:(id)input fromAction:(AMAction *)anAction error:(NSDictionary **)errorInfo {
```
 
```

```
    NSMutableArray *output = [NSMutableArray array];
```

```
    NSString *actionMessage = nil;
```

```
    NSArray *recipes = nil;
```

```
    NSArray *summaries = nil;
```

```
 
```

```
    // other code here....
```

```
 
```

```
    @try {
```

```
        if (managedObjectContext == nil) {
```

```
            actionMessage = @"accessing user recipe library";
```

```
            [self initCoreDataStack];
```

```
        }
```

```
        actionMessage = @"finding recipes";
```

```
        recipes = [self recipesMatchingSearchParameters];
```

```
        actionMessage = @"generating recipe summaries";
```

```
        summaries = [self summariesFromRecipes:recipes];
```

```
    }
```

```
    @catch (NSException *exception) {
```

```
        NSMutableDictionary *errorDict = [NSMutableDictionary dictionary];
```

```
        [errorDict setObject:[NSString stringWithFormat:@"Error %@: %@", actionMessage, [exception reason]] forKey:OSAScriptErrorMessage];
```

```
        [errorDict setObject:[NSNumber numberWithInt:errOSAGeneralError] forKey:OSAScriptErrorNumber];
```

```
        *errorInfo = errorDict;
```

```
        return input;
```

```
    }
```

```
 
```

```
    // other code here ....
```

```
}
```

> **Note:** For more on the Application Kit’s error-handling mechanisms, see *[Error Handling Programming Guide](../../ErrorHandlingCocoa/ErrorHandling/ErrorHandling.html#//apple_ref/doc/uid/TP40001806)*. To learn more about Automator actions, see *[Automator Programming Guide](../../../../AppleApplications/Conceptual/AutomatorConcepts/Automator.html#//apple_ref/doc/uid/TP40001450)*. 

You can have a sequence of `@catch` error-handling blocks. Each block handles an exception object of a different type. You should order this sequence of `@catch` blocks from the most-specific to the least-specific type of exception object (the least specific type being `id`), as shown in Listing 3.  This sequencing allows you to tailor the processing of exceptions as groups.

**Listing 3**  Sequence of exception handlers

@try {
```
    // code that throws an exception
```

```
    ...
```

```
}
```

```
@catch (CustomException *ce) { // most specific type
```

```
    // handle exception ce
```

```
    ...
```

```
}
```

```
@catch (NSException *ne) { // less specific type
```

```
    // do whatever recovery is necessary at his level
```

```
    ...
```

```
    // rethrow the exception so it's handled at a higher level
```

```
    @throw;
```

```
}
```

```
@catch (id ue) { // least specific type
```

```
    // code that handles this exception
```

```
    ...
```

```
}
```

```
@finally {
```

```
    // perform tasks necessary whether exception occurred or not
```

```
    ...
```

```
}
```

> **Note:** You cannot use the `setjmp` and `longjmp` functions if the jump entails crossing an `@try` block. Since the code that your program calls may have exception-handling domains within it, avoid using `setjmp` and `longjmp` in your application. However, you may use `goto` or `return` to exit an exception handling domain. 

## Exception Handling and Memory Management 
Using the exception-handling directives of Objective-C can complicate [memory management](../../../../General/Conceptual/DevPedia-CocoaCore/MemoryManagement.html#//apple_ref/doc/uid/TP40008195-CH27), but with a little common sense you can avoid the pitfalls. To see how, let’s begin with the simple case: a method that, for the sake of efficiency, creates an object, uses it, and then releases it explicitly:

- (void)doSomething {
```
    NSMutableArray *anArray = [[NSMutableArray alloc] initWithCapacity:0];
```

```
    [self doSomethingElse:anArray];
```

```
    [anArray release];
```

```
}
```
The problem here is obvious: If the `doSomethingElse:` method throws an exception there is a memory leak. But the solution is equally obvious: Move the `[release](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intfm/NSObject/release)` to a `@finally` block:

- (void)doSomething {
```
    NSMutableArray *anArray = nil;
```

```
    array = [[NSMutableArray alloc] initWithCapacity:0];
```

```
    @try {
```

```
        [self doSomethingElse:anArray];
```

```
    }
```

```
    @finally {
```

```
        [anArray release];
```

```
    }
```

```
}
```
This pattern of using `@try...@finally` to release objects involved in an exception applies to other resources as well. If you have `malloc`’d blocks of memory or open file descriptors, `@finally` is a good place to free those; it’s also the ideal place to unlock any locks you’ve acquired. 

Another, more subtle memory-management problem is over-releasing an exception object when there are internal `[autorelease](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSObject/Description.html#//apple_ref/occ/intfm/NSObject/autorelease)` pools. Almost all `NSException` objects (and other types of exception objects) are created autoreleased, which assigns them to the nearest (in scope) autorelease pool. When that pool is released, the exception is destroyed. A pool can be either released directly or as a result of an autorelease pool further down the stack (and thus further out in scope) being popped (that is, released). Consider this method:  

- (void)doSomething {
```
    NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
```

```
    NSMutableArray *anArray = [[[NSMutableArray alloc] initWithCapacity:0] autorelease];
```

```
    [self doSomethingElse:anArray];
```

```
    [pool release];
```

```
}
```
This code appears to be sound; if the `doSomethingElse:` message results in a thrown exception, the local autorelease pool will be released when a lower (or outer) autorelease pool on the stack is popped. But there is a potential problem. As explained in [Throwing Exceptions](RaisingExceptions.html#//apple_ref/doc/uid/20000058-BBCCFIBF), a re-thrown exception causes its associated `@finally` block to be executed as an early side effect. If an outer autorelease pool is released in a `@finally` block, the local pool could be released *before* the exception is delivered, resulting in a “zombie” exception. 

There are several ways to resolve this problem. The simplest is to refrain from releasing local autorelease pools in `@finally` blocks. Instead let a pop of a deeper pool take care of releasing the pool holding the exception object. However, if no deeper pool is ever popped as the exception propagates up the stack, the pools on the stack will leak memory; all objects in those pools remain unreleased until the thread is destroyed.  

An alternative approach would be to catch any thrown exception, retain it, and rethrow it . Then, in the `@finally` block, release the autorelease pool and autorelease the exception object.  Listing 4 shows how this might look in code. 

**Listing 4**  Releasing an autorelease pool containing an exception object

- (void)doSomething {
```
    id savedException = nil;
```

```
    NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
```

```
    NSMutableArray *anArray = [[[NSMutableArray alloc] initWithCapacity:0] autorelease];
```

```
    @try {
```

```
        [self doSomethingElse:anArray];
```

```
    }
```

```
    @catch (NSException *theException) {
```

```
        savedException = [theException retain];
```

```
        @throw;
```

```
    }
```

```
    @finally {
```

```
        [pool release];
```

```
        [savedException autorelease];
```

```
    }
```

```
}
```
Doing this retains the thrown exception across the release of the interior autorelease pool—the pool the exception was put into on its way out of `doSomethingElse:`—and ensures that it is autoreleased in the next autorelease pool outward to it in scope (or, in another perspective, the autorelease pool below it on the stack). For things to work correctly, the release of the interior autorelease pool must occur before the retained exception object is autoreleased. 

        
            [Next](RaisingExceptions.html)[Previous](../Articles/ExceptionsAndCocoaFrameworks.html)
        
         Copyright &#x00a9; 2002, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-08-08