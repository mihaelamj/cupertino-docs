---
title: "Controlling a Program’s Response to Exceptions"
book: "Exception Programming Topics"
chapterId: "20000473"
date: "2013-08-08"
description: "Explains how to raise and handle exceptions in Cocoa applications."
identifier: "//apple_ref/doc/uid/10000012i"
source: apple-archive
---

# Controlling a Program’s Response to Exceptions

> **Exception Programming Topics**
> Last updated: 2013-08-08

[Next](../Articles/Exceptions64Bit.html)[Previous](../Concepts/PredefinedExceptions.html)
        
        
        [](../index.html)
        
        # Controlling a Program’s Response to Exceptions
This document describes some user defaults and the API of the Exception Handling framework that you can use to control the behavior of applications in response to certain types of errors.

To use the services of the Exception Handling framework in your Cocoa project (whether application or non-application), add `ExceptionHandling.framework` in `/System/Library/Frameworks` to your Xcode project. Also insert the following import directive in the header or implementation file of the class that uses the framework:

#import <ExceptionHandling/NSExceptionHandler.h>
> **Important:** The Exception Handing framework is not available on iOS.

The services described below are made possible through an uncaught exception handler set by the Exception Handling framework. These services won't be available if a custom uncaught exception handler is set through the `[NSSetUncaughtExceptionHandler](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSSetUncaughtExceptionHandler)` function.

## Application Errors
Certain types of application errors typically cause Cocoa applications to exit abruptly. You can use a user default, `NSExceptionHandlingMask`, to control this behavior (for Application Kit-based applications only) for three of the most common classes of such errors:

uncaught NSExceptions;

system-level exceptions (such as invalid memory accesses)

Objective-C runtime errors (such as messages sent to freed objects).

For these error types you can set `NSExceptionHandlingMask` to do one of the following actions: 

Print a descriptive log and a stack trace to the console when such an error occurs.

Handle the error in such a way as to prevent the resulting abrupt termination,

Do both of the above.

You construct the mask by adding the values corresponding to the types of errors to be logged or handled:

**Table 1**  Exception-handling constants and `defaults` valuesType of Action

Constant

Value for `defaults`

Log uncaught NSExceptions

`NSLogUncaughtExceptionMask`

1

Handle uncaught NSExceptions

`NSHandleUncaughtExceptionMask`

2

Log system-level exceptions

`NSLogUncaughtSystemExceptionMask`

4

Handle system-level exceptions

`NSHandleUncaughtSystemExceptionMask`

8

Log runtime errors

`NSLogUncaughtRuntimeErrorMask`

16

Handle runtime errors

`NSHandleUncaughtRuntimeErrorMask`

32

Thus, if you enter the following on the command line (in the Terminal application):

defaults write NSGlobalDomain NSExceptionHandlingMask 63you cause the logging and handling behavior described above for all uncaught exceptions, system-level exceptions, and runtime errors in all applications.

The word "handle" in the exception-handling constants has a specific meaning depending on the type of exception. The Exception Handling framework handles system-level exceptions and runtime errors  by converting them into `[NSException](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/cl/NSException)` objects. These exception objects contain a stack trace in their `userInfo`dictionary under the key `NSStackTraceKey`. The framework handles uncaught `NSException` objects by terminating the thread in which they occur. Exceptions on the main thread of a Cocoa application are caught by the top-level handlers, which are usually installed by the Application Kit.

Instead of the `NSExceptionHandlingMask` user default, you can use the `setExceptionHandlingMask:` method of the Exception Handling framework to get the same exception-handling behavior. For both application and non-application Cocoa executables, link against the Exception Handling framework and send the following message:

[[NSExceptionHandler defaultExceptionHandler] setExceptionHandlingMask: aMask]The `aMask` parameter is a bit mask composed by bitwise-ORing the constants listed in the table above. See the header files of the Exception Handling framework for more details on the NSExceptionHandler API.

## Debugging Aids
For debugging purposes, it is also possible to use the same mechanisms to report on NSExceptions that would otherwise be caught. You can also use either the `NSExceptionHandlingMask` property of the `defaults` system for this purpose or the `setExceptionHandlingMask:` method of the NSExceptionHandler class. The related constants and values are listed in the following table:

**Table 2**  Debugging constants and `defaults` valuesType of Action

Constant

Value for `defaults`

Log exceptions that would be caught by the top-level exception handlers in NSApplication. See note below.

`NSLogTopLevelExceptionMask`

64

Handle exceptions that would be caught by the top-level exception handlers in NSApplication

`NSHandleTopLevelExceptionMask`

128

Log exceptions that will be caught at lower levels

`NSLogOtherExceptionMask`

256

Handle exceptions that will be caught at lower levels

`NSHandleOtherExceptionMask`

512

> **Note:** When exception-handling domains are nested, log exceptions that make it to the top two levels. On the main thread of a Cocoa application, this means log exceptions caught by `NSApp`.

In these cases, handling an exception means nothing more than adding a stack trace to its `userInfo` dictionary under the key `NSStackTraceKey`. Note that caught exceptions should be logged or handled only for debugging, not under normal circumstances, because doing so may generate large amounts of output, or alter the normal behavior of applications.

For further debugging purposes, you can change the handling behavior for any condition handled by NSExceptionHandler so that the application is instead halted so a debugger can be attached. You can control this behavior with summed values for the `NSExceptionHangingMask` user default or with the bit mask passed into the `setExceptionHangingMask:` of the NSExceptionHandler class. The following table lists the valid constants and `defaults` values:

Type of Action

Constant

Value for `defaults`

Hang for uncaught exceptions

`NSHangOnUncaughtExceptionMask`

1

Hang for system-level exceptions

`NSHangOnUncaughtSystemExceptionMask`

2

Hang for runtime errors

`NSHangOnUncaughtRuntimeErrorMask`

4

Hang for top-level caught exceptions

`NSHangOnTopLevelExceptionMask`

8

Hang for other caught exceptions

`NSHangOnOtherExceptionMask`

16

## Printing Symbolic Stack Traces
As a aid to debugging, you can use the `atos` command-line utility to convert numeric stack traces into symbolic form. (See the `atos(1)` man page for details of this command-line utility.)

> **Note:** You must install the Developer Tools package to have the `atos` utility installed. Also, the `NSException` class provides the `[callStackReturnAddresses](https://developer.apple.com/documentation/foundation/nsexception/1412165-callstackreturnaddresses)`, which you can use for debugging in a manner similar to atos.

Instead of switching between Xcode an a Terminal shell, you can add code to your program that uses `atos` to print a symbolic stack trace to the console. Listing 1 shows how you do this. The method `printStackTrace:` extracts the (numeric) stack trace from the passed-in `NSException` object and then constructs an `[NSTask](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSTask/Description.html#//apple_ref/occ/cl/NSTask)` object that represents the `atos` command with the stack trace as a parameter. It launches the subtask and the resulting symbolic backtrace is printed to standard output (which is the run log in Xcode).

**Listing 1**  A method that prints a symbolic back trace of an exception

- (BOOL)exceptionHandler:(NSExceptionHandler *)sender shouldLogException:(NSException *)exception mask:(unsigned int)mask
```
{
```

```
    [self printStackTrace:exception];
```

```
    return YES;
```

```
}
```

```
 
```

```
- (void)printStackTrace:(NSException *)e
```

```
{
```

```
    NSString *stack = [[e userInfo] objectForKey:NSStackTraceKey];
```

```
    if (stack) {
```

```
        NSTask *ls = [[NSTask alloc] init];
```

```
        NSString *pid = [[NSNumber numberWithInt:[[NSProcessInfo processInfo] processIdentifier]] stringValue];
```

```
        NSMutableArray *args = [NSMutableArray arrayWithCapacity:20];
```

```
 
```

```
        [args addObject:@"-p"];
```

```
        [args addObject:pid];
```

```
        [args addObjectsFromArray:[stack componentsSeparatedByString:@"  "]];
```

```
        // Note: function addresses are separated by double spaces, not a single space.
```

```
 
```

```
        [ls setLaunchPath:@"/usr/bin/atos"];
```

```
        [ls setArguments:args];
```

```
        [ls launch];
```

```
        [ls release];
```

```
 
```

```
    } else {
```

```
        NSLog(@"No stack trace available.");
```

```
    }
```

```
}
```
In this example, the delegate invokes the `printStackTrace:` method in its implementation of `[exceptionHandler:shouldLogException:mask:](https://developer.apple.com/documentation/objectivec/nsobject/1489856-exceptionhandler)`; at this point, the exception is being handled, but has not yet caused termination of the debugged executable. The output of the `atos` utility, when combined with the `NSExceptionHandler` log information, looks similar to Listing 2.

**Listing 2**  Content of NSExceptionHandler log plus `atos` output

2006-08-21 12:18:19.727 ExceptionHandleTest[916] NSExceptionHandler has recorded the following exception:
```
NSInvalidArgumentException -- *** -[NSCFString count]: selector not recognized [self = 0x2a00c]
```

```
Stack trace:  0x9275c27b  0x92782fd7  0x9280b0be  0x9272f207  0x90a51ba1  0x0002995f  0x00023f81  0x00001ca6  0x00001bcd  0x00000001
```

```
__NSRaiseError (in Foundation)
```

```
+[NSException raise:format:] (in Foundation)
```

```
-[NSObject doesNotRecognizeSelector:] (in Foundation)
```

```
-[NSObject(NSForwardInvocation) forward::] (in Foundation)
```

```
__objc_msgForward (in libobjc.A.dylib)
```

```
-[ExceptionTest testException] (in ExceptionHandleTest) (ExceptionTest.m:31)
```

```
_main (in ExceptionHandleTest) (ExceptionHandleTest.m:10)
```

```
start (in ExceptionHandleTest)
```

```
start (in ExceptionHandleTest)
```

```
0x00000001 (in ExceptionHandleTest)
```
There are other ways of accomplishing the same result. For example, the method that prints the symbolic stack trace could be on a category added to `NSException` instead of a method of the delegate’s class.

        
            [Next](../Articles/Exceptions64Bit.html)[Previous](../Concepts/PredefinedExceptions.html)
        
         Copyright &#x00a9; 2002, 2013 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2013-08-08