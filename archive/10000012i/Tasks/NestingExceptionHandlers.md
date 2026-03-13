---
title: "Nesting Exception Handlers"
book: "Exception Programming Topics"
chapterId: "20000060"
date: "2013-08-08"
description: "Explains how to raise and handle exceptions in Cocoa applications."
identifier: "//apple_ref/doc/uid/10000012i"
source: apple-archive
---

# Nesting Exception Handlers

> **Exception Programming Topics**
> Last updated: 2013-08-08

[Next](../Concepts/UncaughtExceptions.html)[Previous](RaisingExceptions.html)
        
        
        [](../index.html)
        
        # Nesting Exception Handlers
Exception handlers can be nested so that an exception raised in an inner domain can be treated by the local exception handler *and* any number of encompassing exception handlers. This design allows an exception to be handled by code that, although it is further from the code actually generating the exception̦, might have more knowledge about the conditions leading to the exception.

## Nested Exception Handlers With Compiler Directives
To understand how nested exception handlers defined with the compiler directives are invoked, consider the code fragment in Listing 1.

**Listing 1**  Throwing and re-throwing an exception

@try {
```
    // ...
```

```
    if (someError) {
```

```
        NSException *theException = [NSException exceptionWithName:MyAppException reason:@"Some error just occurred!" userInfo:nil];
```

```
        @throw theException;
```

```
    }
```

```
}
```

```
@catch (NSException *exception) {
```

```
    if ([[exception name] isEqualToString:MyAppException]) {
```

```
        NSRunAlertPanel(@"Error Panel", @"%@", @"OK", nil, nil,
```

```
                localException);
```

```
    }
```

```
    @throw; // rethrow the exception
```

```
}
```

```
@finally {
```

```
    [self cleanUp];
```

```
}
```
In this code the exception (`exception`) is thrown again at the end of the local handler, allowing an encompassing exception handler to take some additional action. Figure 1 illustrates the flow of program control between nested exception handlers created with the `@catch` directive.

**Figure 1**  Control flow with nested exception handlers—using directivesAn exception raised within Method 3's domain causes execution to jump to its local exception handler. In a typical application, this exception handler queries the exception object to determine the nature of the exception. The local handler may handle exception types that it recognizes and then may rethrow the exception object to pass notification of the exception to the handler nested above it—that is, the handler in Method 2. However, before the next outer exception handler is invoked, the code in the `@finally` block associated with the local exception handler is executed. (This has implications for memory management, as discussed in [Exception Handling and Memory Management ](HandlingExceptions.html#//apple_ref/doc/uid/20000059-SW7).)

An exception that is re-thrown appears to the next higher handler just as if the initial exception had been raised within its own exception handling domain. Method 2's exception handler again may handle the exception and may rethrow the exception to Method 1's exception handler; Method 1’s handler does not receive the re-thrown exception until Method 2’s `@finally` block completes its task. Finally, Method 1’s handler rethrows the exception. Because there is no exception handling domain above Method 1, the exception passes to the uncaught exception handler (see [Uncaught Exceptions](../Concepts/UncaughtExceptions.html#//apple_ref/doc/uid/20000056-BAJDDGGD)).

        
            [Next](../Concepts/UncaughtExceptions.html)[Previous](RaisingExceptions.html)
        
         Copyright &#x00a9; 2002, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-08-08