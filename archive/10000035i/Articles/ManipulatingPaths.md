---
title: "String Representations of File Paths"
book: "String Programming Guide"
chapterId: "20000152"
date: "2014-02-11"
description: "Explains how to create, search, concatenate, and draw strings in Cocoa."
identifier: "//apple_ref/doc/uid/10000035i"
source: apple-archive
---

# String Representations of File Paths

> **String Programming Guide**
> Last updated: 2014-02-11

[Next](DrawingStrings.html)[Previous](Scanners.html)
        
        
        [](../index.html)
        
        # String Representations of File Paths
`[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` provides a rich set of methods for manipulating strings as file-system paths. You can extract a path’s directory, filename, and extension, expand a tilde expression (such as “`~me`”) or create one for the user’s home directory, and clean up paths containing symbolic links, redundant slashes, and references to “.” (current directory) and “..” (parent directory).

> **Note:** Where possible, you should use instances of `[NSURL](https://developer.apple.com/documentation/foundation/nsurl)` to represent paths—the operating system deals with URLs more efficiently than with string representations of paths.

## Representing a Path
`NSString` represents paths generically with ‘/’ as the path separator and ‘.’ as the extension separator. Methods that accept strings as path arguments convert these generic representations to the proper system-specific form as needed. On systems with an implicit root directory, absolute paths begin with a path separator or with a tilde expression (“`~/...`” or “`~user/...`”). Where a device must be specified, you can do that yourself—introducing a system dependency—or allow the string object to add a default device.

You can create a standardized representation of a path using `[stringByStandardizingPath](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/stringByStandardizingPath)`. This performs a number of tasks including:

Expansion of an initial tilde expression;

Reduction of empty components and references to the current directory (“//” and “/./”) to single path separators;

In absolute paths, resolution of references to the parent directory (“..”) to the real parent directory;

for example:

```
NSString *path = @"/usr/bin/./grep";
```

```
NSString *standardizedPath = [path stringByStandardizingPath];
```

```
// standardizedPath: /usr/bin/grep
```

```
 
```

```
path = @"~me";
```

```
standardizedPath = [path stringByStandardizingPath];
```

```
// standardizedPath (assuming conventional naming scheme): /Users/Me
```

```
 
```

```
path = @"/usr/include/objc/..";
```

```
standardizedPath = [path stringByStandardizingPath];
```

```
// standardizedPath: /usr/include
```

```
 
```

```
path = @"/private/usr/include";
```

```
standardizedPath = [path stringByStandardizingPath];
```

```
// standardizedPath: /usr/include
```
## User Directories
The following examples illustrate how you can use `NSString`’s path utilities and other Cocoa functions to get the user directories.

// Assuming that users’ home directories are stored in /Users
```
 
```

```
NSString *meHome = [@"~me" stringByExpandingTildeInPath];
```

```
// meHome = @"/Users/me"
```

```
 
```

```
NSString *mePublic = [@"~me/Public" stringByExpandingTildeInPath];
```

```
// mePublic = @"/Users/me/Public"
```
You can find the home directory for the current user and for a given user with `[NSHomeDirectory](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSHomeDirectory)` and `[NSHomeDirectoryForUser](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSHomeDirectoryForUser)` respectively:

NSString *currentUserHomeDirectory = NSHomeDirectory();
```
NSString *meHomeDirectory = NSHomeDirectoryForUser(@"me");
```
Note that you should typically use the function `[NSSearchPathForDirectoriesInDomains](https://developer.apple.com/documentation/foundation/1414224-nssearchpathfordirectoriesindoma)` to locate standard directories for the current user. For example, instead of:

NSString *documentsDirectory =
```
                [NSHomeDirectory() stringByAppendingPathComponent:@"Documents"];
```
you should use:

```
NSString *documentsDirectory;
```

```
NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
```

```
if ([paths count] > 0) {
```

```
    documentsDirectory = [paths objectAtIndex:0];
```

```
}
```
## Path Components
`NSString` provides a rich set of methods for manipulating strings as file-system paths, for example:

`[pathExtension](https://developer.apple.com/documentation/foundation/nsstring/1407801-pathextension)`

`[stringByDeletingPathExtension](https://developer.apple.com/documentation/foundation/nsstring/1418214-stringbydeletingpathextension)`

`[stringByDeletingLastPathComponent](https://developer.apple.com/documentation/foundation/nsstring/1411141-stringbydeletinglastpathcomponen)`

Using these and related methods described in *[NSString Class Reference](https://developer.apple.com/documentation/foundation/nsstring)*, you can extract a path’s directory, filename, and extension, as illustrated by the following examples.

```
NSString *documentPath = @"~me/Public/Demo/readme.txt";
```

```
 
```

```
NSString *documentDirectory = [documentPath stringByDeletingLastPathComponent];
```

```
// documentDirectory = @"~me/Public/Demo"
```

```
 
```

```
NSString *documentFilename = [documentPath lastPathComponent];
```

```
// documentFilename = @"readme.txt"
```

```
 
```

```
NSString *documentExtension = [documentPath pathExtension];
```

```
// documentExtension = @"txt"
```
## File Name Completion
You can find possible expansions of file names using `[completePathIntoString:caseSensitive:matchesIntoArray:filterTypes:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/completePathIntoString:caseSensitive:matchesIntoArray:filterTypes:)`. For example, given a directory `~/Demo` that contains the following files:

`ReadMe.txt      readme.html     readme.rtf      recondite.txt   test.txt`

you can find all possible completions for the path `~/Demo/r` as follows:

NSString *partialPath = @"~/Demo/r";
```
NSString *longestCompletion;
```

```
NSArray *outputArray;
```

```
 
```

```
unsigned allMatches = [partialPath completePathIntoString:&longestCompletion
```

```
    caseSensitive:NO
```

```
    matchesIntoArray:&outputArray
```

```
    filterTypes:nil];
```

```
 
```

```
// allMatches = 3
```

```
// longestCompletion = @"~/Demo/re"
```

```
// outputArray = (@"~/Demo/readme.html", "~/Demo/readme.rtf", "~/Demo/recondite.txt")
```
You can find possible completions for the path `~/Demo/r` that have an extension “.txt” or “.rtf” as follows:

```
NSArray *filterTypes = @[@"txt", @"rtf"];
```

```
 
```

```
unsigned textMatches = [partialPath completePathIntoString:&outputName
```

```
    caseSensitive:NO
```

```
    matchesIntoArray:&outputArray
```

```
    filterTypes:filterTypes];
```

```
// allMatches = 2
```

```
// longestCompletion = @"~/Demo/re"
```

```
// outputArray = (@"~/Demo/readme.rtf", @"~/Demo/recondite.txt")
```

        
            [Next](DrawingStrings.html)[Previous](Scanners.html)
        
         Copyright &#x00a9; 1997, 2014 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2014-02-11