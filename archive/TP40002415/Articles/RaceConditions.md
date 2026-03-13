---
title: "Race Conditions and Secure File Operations"
book: "Secure Coding Guide"
chapterId: "TP40002585"
date: "2016-09-13"
description: "Describes techniques to use and factors to consider to make your code more secure from attack."
identifier: "//apple_ref/doc/uid/TP40002415"
source: apple-archive
---

# Race Conditions and Secure File Operations

> **Secure Coding Guide**
> Last updated: 2016-09-13

[Next](AccessControl.html)[Previous](ValidatingInput.html)
        
        
        [](../index.html)
        
        # Race Conditions and Secure File Operations
When working with shared data, whether in the form of files, databases, network connections, shared memory, or other forms of interprocess communication, there are a number of easily made mistakes that can compromise security. This chapter describes many such pitfalls and how to avoid them.

## Avoiding Race Conditions
A *race condition* exists when changes to the order of two or more events can cause a change in behavior. If the correct order of execution is required for the proper functioning of the program, this is a bug. If an attacker can take advantage of the situation to insert malicious code, change a filename, or otherwise interfere with the normal operation of the program, the race condition is a security vulnerability. Attackers can sometimes take advantage of small time gaps in the processing of code to interfere with the sequence of operations, which they then exploit.

macOS, like all modern operating systems, is a multitasking OS; that is, it allows multiple processes to run or appear to run simultaneously by rapidly switching among them on each processor. The advantages to the user are many and mostly obvious; the disadvantage, however, is that there is no guarantee that two consecutive operations in a given process are performed without any other process performing operations between them. In fact, when two processes are using the same resource (such as the same file), there is no guarantee that they will access that resource in any particular order unless both processes explicitly take steps to ensure it.

For example, if you open a file and then read from it, even though your application did nothing else between these two operations, some other process might alter the file after the file was opened and before it was read. If two different processes (in the same or different applications) were writing to the same file, there would be no way to know which one would write first and which would overwrite the data written by the other. Such situations cause security vulnerabilities.

There are two basic types of race condition that can be exploited: time of check–time of use (TOCTOU), and signal handling.

### Time of Check Versus Time of Use
It is fairly common for an application to need to check some condition before undertaking an action. For example, it might check to see if a file exists before writing to it, or whether the user has access rights to read a file before opening it for reading. Because there is a time gap between the check and the use (even though it might be a fraction of a second), an attacker can sometimes use that gap to mount an attack. Thus, this is referred to as a time-of-check–time-of-use problem.

	**Temporary Files**A classic example is the case where an application writes temporary files to publicly accessible directories. You can set the file permissions of the temporary file to prevent another user from altering the file. However, if the file already exists before you write to it, you could be overwriting data needed by another program, or you could be using a file prepared by an attacker, in which case it might be a hard link or symbolic link, redirecting your output to a file needed by the system or to a file controlled by the attacker.

To prevent this, programs often check to make sure a temporary file with a specific name does not already exist in the target directory. If such a file exists, the application deletes it or chooses a new name for the temporary file to avoid conflict. If the file does not exist, the application opens the file for writing, because the system routine that opens a file for writing automatically creates a new file if none exists.

An attacker, by continuously running a program that creates a new temporary file with the appropriate name, can (with a little persistence and some luck) create the file in the gap between when the application checked to make sure the temporary file didn’t exist and when it opens it for writing. The application then opens the attacker’s file and writes to it (remember, the system routine opens an existing file if there is one, and creates a new file only if there is no existing file).

The attacker’s file might have different access permissions than the application’s temporary file, so the attacker can then read the contents. Alternatively, the attacker might have the file already open. The attacker could replace the file with a hard link or symbolic link to some other file (either one owned by the attacker or an existing system file). For example, the attacker could replace the file with a symbolic link to the system password file, so that after the attack, the system passwords have been corrupted to the point that no one, including the system administrator, can log in.

For a real-world example, in a vulnerability in a directory server, a server script wrote private and public keys into temporary files, then read those keys and put them into a database. Because the temporary files were in a publicly writable directory, an attacker could have created a race condition by substituting the attacker’s own files (or hard links or symbolic links to the attacker’s files) before the keys were reread, thus causing the script to insert the attacker’s private and public keys instead. After that, anything encrypted or authenticated using those keys would be under the attacker’s control. Alternatively, the attacker could have read the private keys, which can be used to decrypt encrypted data. [CVE-2005-2519]

Similarly, if an application temporarily relaxes permissions on files or folders in order to perform some operation, an attacker might be able to create a race condition by carefully timing his or her attack to occur in the narrow window in which those permissions are relaxed.

To learn more about creating temporary files securely, read [Create Temporary Files Correctly](#//apple_ref/doc/uid/TP40002585-SW10).

**Interprocess Communication**Time-of-check–time-of-use problems do not have to involve files, of course. They can apply to any data storage or communications mechanism that does not perform operations atomically.

Suppose, for example, that you wrote a program designed to automatically count the number of people entering a sports stadium for a game. Each turnstile talks to a web service running on a server whenever someone walks through. Each web service instance inherently runs as a separate process. Each time a turnstile sends a signal, an instance of the web service starts up, retrieves the gate count from a database, increments it by one, and writes it back to the database. Thus, multiple processes are keeping a single running total.

Now suppose two people enter different gates at exactly the same time. The sequence of events might then be as follows:

Server process A receives a request from gate A.

Server process B receives a request from gate B.

Server process A reads the number `1000` from the database.

Server process B reads the number `1000` from the database.

Server process A increments the gate count by 1 so that `Gate == 1001`.

Server process B increments the gate count by 1 so that `Gate == 1001`.

Server process A writes `1001` as the new gate count.

Server process B writes `1001` as the new gate count.

Because server process B read the gate count before process A had time to increment it and write it back, both processes read the same value. After process A increments the gate count and writes it back, process B overwrites the value of the gate count with the same value written by process A. Because of this race condition, one of the two people entering the stadium was not counted. Since there might be long lines at each turnstile, this condition might occur many times before a big game, and a dishonest ticket clerk who knew about this undercount could pocket some of the receipts with no fear of being caught.

Other race conditions that can be exploited, like the example above, involve the use of shared data or other interprocess communication methods. If an attacker can interfere with important data after it is written and before it is re-read, he or she can disrupt the operation of the program, alter data, or do other mischief. The use of non-thread-safe calls in multithreaded programs can result in data corruption. If an attacker can manipulate the program to cause two such threads to interfere with each other, it may be possible to mount a denial-of-service attack.

In some cases, by using such a race condition to overwrite a buffer in the heap with more data than the buffer can hold, an attacker can cause a buffer overflow. As discussed in [Avoiding Buffer Overflows and Underflows](BufferOverflows.html#//apple_ref/doc/uid/TP40002577-SW1), buffer overflows can be exploited to cause execution of malicious code.

The solution to race conditions involving shared data is to use a locking mechanism to prevent one process from changing a variable until another is finished with it. There are problems and hazards associated with such mechanisms, however, and they must be implemented carefully. And, of course, locking mechanisms only apply to processes that participate in the locking scheme. They cannot prevent an untrusted application from modifying the data maliciously. For a full discussion, see Wheeler, *Secure Programming HOWTO*, at [http://www.dwheeler.com/secure-programs/](http://www.dwheeler.com/secure-programs/).

Time-of-check–time-of-use vulnerabilities can be prevented in different ways, depending largely on the domain of the problem. When working with shared data, you should use locking to protect that data from other instances of your code. When working with data in publicly writable directories, you should also take the precautions described in [Files in Publicly Writable Directories Are Dangerous](#//apple_ref/doc/uid/TP40002585-SW15).

### Signal Handling
Because signal handlers execute code at arbitrary times, they can be used to cause incorrect behavior. In daemons running as root, running the wrong code at the wrong time can even cause privilege escalation. [Securing Signal Handlers](#//apple_ref/doc/uid/TP40002585-SW7) describes this problem in more detail.

## Securing Signal Handlers
Signal handlers are another common source of race conditions. Signals from the operating system to a process or between two processes are used for such purposes as terminating a process or causing it to reinitialize.

If you include signal handlers in your program, they should not make any system calls and should terminate as quickly as possible. Although there are certain system calls that are safe from within signal handlers, writing a safe signal handler that does so is tricky. The best thing to do is to set a flag that your program checks periodically, and do no other work within the signal handler. This is because the signal handler can be interrupted by a new signal before it finishes processing the first signal, leaving the system in an unpredictable state or, worse, providing a vulnerability for an attacker to exploit.

For example, in 1997, a vulnerability was reported in a number of implementations of the FTP protocol in which a user could cause a race condition by closing an FTP connection. Closing the connection resulted in the near-simultaneous transmission of two signals to the FTP server: one to abort the current operation, and one to log out the user. The race condition occurred when the logout signal arrived just before the abort signal.

When a user logged onto an FTP server as an anonymous user, the server would temporarily downgrade its privileges from root to nobody so that the logged-in user had no privileges to write files. When the user logged out, however, the server reassumed root privileges. If the abort signal arrived at just the right time, it would abort the logout procedure after the server had assumed root privileges but before it had logged out the user. The user would then be logged in with root privileges, and could proceed to write files at will. An attacker could exploit this vulnerability with a graphical FTP client simply by repeatedly clicking the “Cancel” button. [CVE-1999-0035] 

For a discourse on how signal handler race conditions can be exploited, see the article by Michal Zalewski at [http://lcamtuf.coredump.cx/signals.txt](http://lcamtuf.coredump.cx/signals.txt).

## Securing File Operations
Insecure file operations are a major source of security vulnerabilities. In some cases, opening or writing to a file in an insecure fashion can give attackers the opportunity to create a race condition (see [Time of Check Versus Time of Use](#//apple_ref/doc/uid/TP40002585-SW2)). Often, however, insecure file operations give an attacker the ability to read confidential information, perform a denial of service attack, take control of an application, or even take control of the entire system.

This section discusses what you should do to make your file operations more secure.

### Check Result Codes
Always check the result codes of every routine that you call. Be prepared to handle the situation if the operation fails. Most file-based security vulnerabilities could have been avoided if the developers of the programs had checked result codes.

Some common mistakes are listed below.

	**When writing to files or changing file permissions**A failure when change permissions on a file or to open a file for writing can be caused by many things, including:

Insufficient permissions on the file or enclosing directory.

The immutable flag (set with the `chflags` utility or the `[chflags](../../../../System/Conceptual/ManPages_iPhoneOS/man2/chflags.2.html#//apple_ref/doc/man/2/chflags)` system call).

A network volume becoming unavailable.

An external drive getting unplugged.

A drive failure.

Depending on the nature of your software, any one of these could potentially be exploited if you do not properly check error codes.

See the manual pages for the `chflags`, `chown`, and `chgrp` commands and the `[chflags](../../../../System/Conceptual/ManPages_iPhoneOS/man2/chflags.2.html#//apple_ref/doc/man/2/chflags)` and `[chown](../../../../System/Conceptual/ManPages_iPhoneOS/man2/chown.2.html#//apple_ref/doc/man/2/chown)` functions for more information.

**When removing files**Although the `rm` command can often ignore permissions if you pass the `-f` flag, it can still fail.

For example, you can’t remove a directory that has anything inside it. If a directory is in a location where other users have access to it, any attempt to remove the directory might fail because another process might add new files while you are removing the old ones.

The safest way to fix this problem is to use a private directory that no one else has access to. If that’s not possible, check to make sure the `rm` command succeeded and be prepared to handle failures.

### Watch Out for Hard Links
A hard link is a second name for a file—the file appears to be in two different locations with two different names.

If a file has two (or more) hard links and you check the file to make sure that the ownership, permissions, and so forth are all correct, but fail to check the number of links to the file, an attacker can write to or read from the file through their own link in their own directory. Therefore, among other checks before you use a file, you should check the number of links.

Do not, however, simply fail if there’s a second link to a file, because there are some circumstances where a link is okay or even expected. For example, every directory is linked into at least two places in the hierarchy—the directory name itself and the special `.` record from the directory that links back to itself. Also, if that directory contains other directories, each of those subdirectories contains a `..` record that points to the outer directory.

You need to anticipate such conditions and allow for them. Even if the link is unexpected, you need to handle the situation gracefully. Otherwise, an attacker can cause denial of service just by creating a link to the file. Instead, you should notify the user of the situation, giving them as much information as possible so they can try to track down the source of the problem.

### Watch Out for Symbolic Links
A symbolic link is a special type of file that contains a path name. Symbolic links are more common than hard links.

Functions that follow symbolic links automatically open, read, or write to the file whose path name is in the symbolic link file rather than the symbolic link file itself. Your application receives no notification that a symbolic link was followed; to your application, it appears as if the file addressed is the one that was used.

An attacker can use a symbolic link, for example, to cause your application to write the contents intended for a temporary file to a critical system file instead, thus corrupting the system. Alternatively, the attacker can capture data you are writing or can substitute the attacker’s data for your own when you read the temporary file.

In general, you should avoid functions, such as `[chown](../../../../System/Conceptual/ManPages_iPhoneOS/man2/chown.2.html#//apple_ref/doc/man/2/chown)` and `[stat](../../../../System/Conceptual/ManPages_iPhoneOS/man2/stat.2.html#//apple_ref/doc/man/2/stat)`, that follow symbolic links (see [Table 4-1](#//apple_ref/doc/uid/TP40002585-SW3) for alternatives). As with hard links, your program should evaluate whether a symbolic link is acceptable, and if not, should handle the situation gracefully.

### Case-Insensitive File Systems Can Thwart Your Security Model
In macOS, any partition (including the boot volume) can be either case-sensitive, case-insensitive but case-preserving, or, for non-boot volumes, case-insensitive. For example, HFS+ can be either case-sensitive or case-insensitive but case-preserving. FAT32 is case-insensitive but case-preserving. FAT12, FAT16, and ISO-9660 (without extensions) are case-insensitive.

An application that is unaware of the differences in behavior between these volume formats can cause serious security holes if you are not careful. In particular:

If your program uses its own permission model to provide or deny access (for example, a web server that allows access only to files within a particular directory), you must either enforce this with a `[chroot](../../../../System/Conceptual/ManPages_iPhoneOS/man2/chroot.2.html#//apple_ref/doc/man/2/chroot)` jail or be vigilant about ensuring that you correctly identify paths even in a case-insensitive world.

Among other things, this means that you should ideally deny by default with allowed exceptions, rather than allow by default with denied exceptions. If this is not possible, for correctness, you must compare each individual path part against your denial list using case-sensitive or case-insensitive comparisons, depending on what type of volume the file resides on.

For example, if your program prevents users from uploading or downloading the file `/etc/ssh_host_key`, if your software is installed on a case-insensitive volume, you must also reject someone who makes a request for `/etc/SSH_host_key`, `/ETC/SSH_HOST_KEY`, or even `/ETC/ssh_host_key`.

If your program periodically accesses a file on a case-sensitive volume using the wrong mix of uppercase and lowercase letters, the open call will fail... until someone creates a second file with the name your program is actually asking for.

If someone creates such a file, your application will dutifully load data from the wrong file. If the contents of that file affect your application’s behavior in some important way, this represents a potential attack vector.

This also presents a potential attack vector if that file is an optional part of your application bundle that gets loaded by dyld when your application is launched.

### Create Temporary Files Correctly
The temporary directories in macOS are shared among multiple users. This requires that they be writable by multiple users. Any time you work on files in a location to which others have read/write access, there’s the potential for the file to be compromised or corrupted.

The best way to handle this is to create a safe temporary directory that only you can access, then write the files into that directory.

To do this:

In a cocoa app, call `[NSTemporaryDirectory](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSTemporaryDirectory)`.

At the POSIX layer, call `[confstr](../../../../System/Conceptual/ManPages_iPhoneOS/man3/confstr.3.html#//apple_ref/doc/man/3/confstr)` and pass the constant `_CS_DARWIN_USER_TEMP_DIR` as the `name` parameter.

Next, to maximize your protection against malicious apps running as the same user, use appropriate functions to create folders and files within that directory, as described below.

	**POSIX Layer**Use the `[mkstemp](../../../../System/Conceptual/ManPages_iPhoneOS/man3/mkstemp.3.html#//apple_ref/doc/man/3/mkstemp)` function to create temporary files at the POSIX layer. The `mkstemp` function guarantees a unique filename and returns a file descriptor, thus allowing you skip the step of checking the `[open](../../../../System/Conceptual/ManPages_iPhoneOS/man2/open.2.html#//apple_ref/doc/man/2/open)` function result for an error, which might require you to change the filename and call `open` again.

If you must create a temporary file in a public directory manually, you can use the `open` function with the `O_CREAT` and `O_EXCL` flags set to create the file and obtain a file descriptor. The `O_EXCL` flag causes this function to return an error if the file already exists. Be sure to check for errors before proceeding.

After you’ve opened the file and obtained a file descriptor, you can safely use functions that take file descriptors, such as the standard C functions `[write](../../../../System/Conceptual/ManPages_iPhoneOS/man2/write.2.html#//apple_ref/doc/man/2/write)` and `[read](../../../../System/Conceptual/ManPages_iPhoneOS/man2/read.2.html#//apple_ref/doc/man/2/read)`, for as long as you keep the file open. See the manual pages for `open(2)`, `mkstemp(3)`, `write(2)`, and `read(2)` for more on these functions, and see Wheeler, *Secure Programming HOWTO* for advantages and shortcomings to using these functions. 

**Cocoa**There are no Cocoa methods that create a file and return a file descriptor. However, you can call the standard C `[open](../../../../System/Conceptual/ManPages_iPhoneOS/man2/open.2.html#//apple_ref/doc/man/2/open)` function from an Objective-C program to obtain a file descriptor (see [Working with Publicly Writable Files Using POSIX Calls](#//apple_ref/doc/uid/TP40002585-SW4)). Or you can call the `[mkstemp](../../../../System/Conceptual/ManPages_iPhoneOS/man3/mkstemp.3.html#//apple_ref/doc/man/3/mkstemp)` function to create a temporary file and obtain a file descriptor. Then you can use the `NSFileHandle` method `[initWithFileDescriptor:](https://developer.apple.com/documentation/foundation/filehandle/1409825-init)` to initialize a file handle, and other `NSFileHandle` methods to safely write to or read from the file.  Documentation for the `NSFileHandle` class is in *[Foundation Framework Reference](https://developer.apple.com/documentation/foundation)*.

To obtain the path to the default location to store temporary files (stored in the `$TMPDIR` environment variable), you can use the `[NSTemporaryDirectory](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Functions/FoundationFunctions/Description.html#//apple_ref/c/func/NSTemporaryDirectory)` function. Note that `NSTemporaryDirectory` can return `/tmp` under certain circumstances such as if you link on a pre-OS X 10.3 development target. Therefore, if you’re using `NSTemporaryDirectory`, you either have to be sure that using `/tmp` is suitable for your operation or, if not, you should consider that an error case and create a more secure temporary directory if that happens.

The `[changeFileAttributes:atPath:](https://developer.apple.com/documentation/foundation/nsfilemanager/1557001-changefileattributes)` method in the `NSFileManager` class is similar to `chmod` or `chown`, in that it takes a file path rather than a file descriptor. You shouldn’t use this method if you’re working in a public directory or a user’s home directory. Instead, call the `[fchown](../../../../System/Conceptual/ManPages_iPhoneOS/man2/fchown.2.html#//apple_ref/doc/man/2/fchown)` or `[fchmod](../../../../System/Conceptual/ManPages_iPhoneOS/man2/fchmod.2.html#//apple_ref/doc/man/2/fchmod)` function (see [Table 4-1](#//apple_ref/doc/uid/TP40002585-SW3)). You can call the `NSFileHandle` class’s `fileDescriptor` method to get the file descriptor of a file in use by `NSFileHandle`.

In addition, when working with temporary files, you should avoid the `writeToFile:atomically` methods of `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` and `[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)`. These are designed to minimize the risk of data loss when writing to a file, but do so in a way that is not recommended for use in directories that are writable by others. See [Working with Publicly Writable Files Using Cocoa](#//apple_ref/doc/uid/TP40002585-SW11) for details.

### Files in Publicly Writable Directories Are Dangerous
Files in publicly writable directories must be treated as inherently untrusted. An attacker can delete the file and replace it with another file, replace it with a symbolic link to another file, create the file ahead of time, and so on. There are ways to mitigate each of these attacks to some degree, but the best way to prevent them is to not read or write files in a publicly writable directory in the first pace. If possible, you should create a subdirectory with tightly controlled permissions, then write your files inside that subdirectory.

If you must work in a directory to which your process does not have exclusive access, however, you must check to make sure a file does not exist before you create it. You must also verify that the file you intend to read from or write to is the same file that you created.

To this end, you should always use routines that operate on file descriptors rather than pathnames wherever possible, so that you can be certain you’re always dealing with the same file. To do this, pass the `O_CREAT` and `O_EXCL` flags to the `[open](../../../../System/Conceptual/ManPages_iPhoneOS/man2/open.2.html#//apple_ref/doc/man/2/open)` system call. This creates a file, but fails if the file already exists.

> **Note:** If you cannot use file descriptors directly for some reason, you should explicitly create files as a separate step from opening them. Although this does not prevent someone from swapping in a new file between those operations, at least it narrows the attack window by making it possible to detect if the file already exists.

Before you create the file, however, you should first set your process’s file creation mask (umask). The file creation mask is a bitmask that alters the default permissions of all new files and directories created by your process. This bitmask is typically specified in octal notation, which means that it must begin with a zero (*not* 0x).

For example, if you set the file creation mask to `022`, any new files created by your process will have `rw-r--r--` permissions because the write permission bits are masked out. Similarly, any new directories will have `rw-r-xr-x` permissions.

> **Note:** New files *never* have the execute bit set. Directories, however, do. Therefore, you should generally mask out execute permission when masking out read permission unless you have a specific reason to allow users to traverse a directory without seeing its contents.

To limit access to any new files or directories so that only the user can access them, set the file creation mask to 077.

You can also mask out permissions in such a way that they apply to the user, though this is rare. For example, to create a file that no one can write or execute, and that only the user can read, you could set the file creation mask to 0377. This is not particularly useful, but it is possible.

There are several ways to set the file creation mask:

	**In C code:**In C code, you can set the file creation mask globally using the `[umask](../../../../System/Conceptual/ManPages_iPhoneOS/man2/umask.2.html#//apple_ref/doc/man/2/umask)` system call.

You can also pass the file creation mask to the `[open](../../../../System/Conceptual/ManPages_iPhoneOS/man2/open.2.html#//apple_ref/doc/man/2/open)` or `[mkdir](../../../../System/Conceptual/ManPages_iPhoneOS/man2/mkdir.2.html#//apple_ref/doc/man/2/mkdir)` system call when creating a file or directory.

**Note:** For maximum portability when writing C code, you should always create your masks using the file mode constants defined in `<sys/stat.h>`.

For example:

```
umask(S_IRWXG|S_IRWXO);
```

**In shell scripts:**In shell scripts, you set the file creation mask by using the `umask` shell builtin. This is documented in the manual pages for `sh` or `csh`.

For example:

umask 0077;As an added security bonus, when a process calls another process, the new process inherits the parent process’s file creation mask. Thus, if your process starts another process that creates a file without resetting the file creation mask, that file similarly will not be accessible to other users on the system. This is particularly useful when writing shell scripts.

For more information on the file creation mask, see the manual page for `[umask](../../../../System/Conceptual/ManPages_iPhoneOS/man2/umask.2.html#//apple_ref/doc/man/2/umask)` and Viega and McGraw, *Building Secure Software*, Addison Wesley, 2002. For a particularly lucid explanation of the use of a file creation mask, see [http://web.archive.org/web/20090517063338/http://www.sun.com/bigadmin/content/submitted/umask_permissions.html?](http://web.archive.org/web/20090517063338/http://www.sun.com/bigadmin/content/submitted/umask_permissions.html?).

Before you read a file (but after opening it), make sure it has the owner and permissions you expect (using `[fstat](../../../../System/Conceptual/ManPages_iPhoneOS/man2/fstat.2.html#//apple_ref/doc/man/2/fstat)`). Be prepared to fail gracefully (rather than hanging) if it does not.

Here are some guidelines to help you avoid time-of-check–time-of-use vulnerabilities when working with files in publicly writable directories.  For more detailed discussions, especially for C code, see Viega and McGraw, *Building Secure Software*, Addison Wesley, 2002, and Wheeler, *Secure Programming HOWTO*, available at [http://www.dwheeler.com/secure-programs/](http://www.dwheeler.com/secure-programs/).

If at all possible, avoid creating temporary files in a shared directory, such as `/tmp`, or in directories owned by the user. If anyone else has access to your temporary file, they can modify its content, change its ownership or mode, or replace it with a hard or symbolic link. It’s much safer to either not use a temporary file at all (use some other form of interprocess communication) or keep temporary files in a directory you create and to which only your process (acting as your user) has access.

If your file must be in a shared directory, give it a unique (and randomly generated) filename (you can use the C function `[mkstemp](../../../../System/Conceptual/ManPages_iPhoneOS/man3/mkstemp.3.html#//apple_ref/doc/man/3/mkstemp)` to do this), and never close and reopen the file. If you close such a file, an attacker can potentially find it and replace it before you reopen it.

Here are some public directories that you can use:

`~/Library/Caches/TemporaryItems`

When you use this subdirectory, you are writing to the user’s own home directory, not some other user’s directory or a system directory. If the user’s home directory has the default permissions, it can be written to only by that user and root. Therefore, this directory is not as susceptible to attack from outside, nonprivileged users as some other directories might be.

`/var/run`

This directory is used for process ID (pid) files and other system files needed just once per startup session. This directory is cleared out each time the system starts up.

`/var/db`

This directory is used for databases accessible to system processes.

`/tmp`

This directory is used for general shared temporary storage. It is cleared out each time the system starts up.

`/var/tmp`

This directory is used for general shared temporary storage. Although you should not count on data stored in this directory being permanent, unlike `/tmp`, the `/var/tmp` directory is currently *not* cleared out on reboot.

For maximum security, you should always create temporary subdirectories within these directories, set appropriate permissions on those subdirectories, and then write files into those subdirectories.

The following sections give some additional hints on how to follow these principles when you are using POSIX-layer C code, Carbon, and Cocoa calls.

#### Working with Publicly Writable Files Using POSIX Calls
If you need to open a preexisting file to modify it or read from it, you should check the file’s ownership, type, and permissions, and the number of links to the file before using it.

To safely opening a file for reading, for example, you can use the following procedure:

Call the `open` function and save the file descriptor. Pass the `O_NOFOLLOW` to ensure that it does not follow symbolic links.

Using the file descriptor, call the `[fstat](../../../../System/Conceptual/ManPages_iPhoneOS/man2/fstat.2.html#//apple_ref/doc/man/2/fstat)` function to obtain the `stat` structure for the file you just opened.

Check the user ID (UID) and group ID (GID) of the file to make sure they are correct.

Check the file's mode flags to make sure that it is a normal file, not a FIFO, device file, or other special file. Specifically, if the stat structure is named `st`, then the value of `(st.st_mode & S_IFMT)` should be equal to `S_IFREG`.

Check the read, write, and execute permissions for the file to make sure they are what you expect.

Check that there is only one hard link to the file.

Pass around the open file descriptor for later use rather than passing the path.

Note that you can avoid all the status checking by using a secure directory instead of a public one to hold your program’s files.

Table 4-1 shows some functions to avoid—and the safer equivalent functions to use—in order to avoid race conditions when you are creating files in a public directory.

**Table 4-1**  C file functions to avoid and to useFunctions to avoid

Functions to use instead

`[fopen](../../../../System/Conceptual/ManPages_iPhoneOS/man3/fopen.3.html#//apple_ref/doc/man/3/fopen)` returns a file pointer; automatically creates the file if it does not exist but returns no error if the file does exist

`[open](../../../../System/Conceptual/ManPages_iPhoneOS/man2/open.2.html#//apple_ref/doc/man/2/open)` returns a file descriptor; creates a file and returns an error if the file already exists when the `O_CREAT` and `O_EXCL` options are used

`[chmod](../../../../System/Conceptual/ManPages_iPhoneOS/man2/chmod.2.html#//apple_ref/doc/man/2/chmod)` takes a file path

`[fchmod](../../../../System/Conceptual/ManPages_iPhoneOS/man2/fchmod.2.html#//apple_ref/doc/man/2/fchmod)` takes a file descriptor

`[chown](../../../../System/Conceptual/ManPages_iPhoneOS/man2/chown.2.html#//apple_ref/doc/man/2/chown)` takes a file path and follows symbolic links

`[fchown](../../../../System/Conceptual/ManPages_iPhoneOS/man2/fchown.2.html#//apple_ref/doc/man/2/fchown)` takes a file descriptor and does not follow symbolic links

`[stat](../../../../System/Conceptual/ManPages_iPhoneOS/man2/stat.2.html#//apple_ref/doc/man/2/stat)` takes a file path and follows symbolic links

`[lstat](../../../../System/Conceptual/ManPages_iPhoneOS/man2/lstat.2.html#//apple_ref/doc/man/2/lstat)` takes a file path but does not follow symbolic links; 

`[fstat](../../../../System/Conceptual/ManPages_iPhoneOS/man2/fstat.2.html#//apple_ref/doc/man/2/fstat)` takes a file descriptor and returns information about an open file

`[mktemp](../../../../System/Conceptual/ManPages_iPhoneOS/man3/mktemp.3.html#//apple_ref/doc/man/3/mktemp)` creates a temporary file with a unique name and returns a file path; you need to open the file in another call

`[mkstemp](../../../../System/Conceptual/ManPages_iPhoneOS/man3/mkstemp.3.html#//apple_ref/doc/man/3/mkstemp)` creates a temporary file with a unique name, opens it for reading and writing, and returns a file descriptor

#### Working with Publicly Writable Files Using Carbon
If you are using the Carbon File Manager to create and open files, you should be aware of how the File Manager accesses files. 

The file specifier `FSSpec` structure uses a path to locate files, not a file descriptor. Functions that use an `FSSpec` file specifier are deprecated and should not be used in any case.

The file reference `FSRef` structure uses a path to locate files and should be used only if your files are in a safe directory, not in a publicly accessible directory. These functions include `FSGetCatalogInfo`, `FSSetCatalogInfo`, `FSCreateFork`, and others.

The File Manager creates and opens files in separate operations.  The create operation fails if the file already exists. However, none of the file-creation functions return a file descriptor.

If you’ve obtained the file reference of a directory (from the `FSFindFolder` function, for example), you can use the `FSRefMakePath` function to obtain the directory’s path name. However, be sure to check the function result, because if the `FSFindFolder` function fails, it returns a null string. If you don’t check the function result, you might end up trying to create a temporary file with a pathname formed by appending a filename to a null string.

#### Working with Publicly Writable Files Using Cocoa
The `[NSString](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSStringClassCluster/Description.html#//apple_ref/occ/cl/NSString)` and `[NSData](../../../../LegacyTechnologies/WebObjects/WebObjects_3.5/Reference/Frameworks/ObjC/Foundation/Classes/NSDataClassCluster/Description.html#//apple_ref/occ/cl/NSData)` classes have `writeToFile:atomically:` methods designed to minimize the risk of data loss when writing to a file. These methods write first to a temporary file, and then, when they’re sure the write is successful, they replace the written-to file with the temporary file. This is not always an appropriate thing to do when working in a public directory or a user’s home directory, because there are a number of path-based file operations involved. Instead, initialize an `[NSFileHandle](https://developer.apple.com/documentation/foundation/filehandle)` object with an existing file descriptor and use `NSFileHandle` methods to write to the file, as mentioned above. The following code, for example, uses the `mkstemp` function to create a temporary file and obtain a file descriptor, which it then uses to initialize `NSFileHandle`:

```
fd = mkstemp(tmpfile); // check return for -1, which indicates an error
```

```
NSFileHandle *myhandle = [[NSFileHandle alloc] initWithFileDescriptor:fd];
```
#### Working with Publicly Writable Files in Shell Scripts
Scripts must follow the same general rules as other programs to avoid race conditions. There are a few tips you should know to help make your scripts more secure.

First, when writing a script, set the temporary directory (`$TMPDIR`) environment variable to a safe directory. Even if your script doesn’t directly create any temporary files, one or more of the routines you call might create one, which can be a security vulnerability if it’s created in an insecure directory. See the manual pages for `setenv` and `[setenv](../../../../System/Conceptual/ManPages_iPhoneOS/man3/setenv.3.html#//apple_ref/doc/man/3/setenv)` for information on changing the temporary directory environment variable. For the same reason, set your process’ file code creation mask (umask) to restrict access to any files that might be created by routines run by your script (see [Securing File Operations](#//apple_ref/doc/uid/TP40002585-SW9) for more information on the umask).

It’s also a good idea to use the `dtruss` command on a shell script so you can watch every file access to make sure that no temporary files are created in an insecure location. See the manual pages for `dtrace` and `dtruss` for more information.

Do not redirect output using the operators `>` or `>>` to a publicly writable location. These operators do not check to see whether the file already exists, and they follow symbolic links.

Instead, pass the `-d` flag to the `mktemp` command to create a subdirectory to which only you have access. It’s important to check the result to make sure the command succeeded. if you do all your file operations in this directory, you can be fairly confident that no one with less than root access can interfere with your script. For more information, see the manual page for `mktemp`.

Do not use the `test` command (or its left bracket (`[`) equivalent) to check for the existence of a file or other status information for the file before writing to it. Doing so always results in a race condition; that is, it is possible for an attacker to create, write to, alter, or replace the file before you start writing. See the manual page for `test` for more information.

For a more in-depth look at security issues specific to shell scripts, read [Shell Script Security](../../../../OpenSource/Conceptual/ShellScripting/ShellScriptSecurity/ShellScriptSecurity.html#//apple_ref/doc/uid/TP40004268-CH8) in *[Shell Scripting Primer](../../../../OpenSource/Conceptual/ShellScripting/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004268)*.

### Other Tips
Here are a few additional things to be aware of when working with files:

Before you attempt a file operation, make sure it is safe to perform the operation on that file. For example, before attempting to read a file (but after opening it), you should make sure that it is not a FIFO or a device special file.

Just because you can write to a file, that doesn’t mean you *should* write to it. For example, the fact that a directory exists doesn’t mean you created it, and the fact that you can append to a file doesn’t mean you own the file or no one else can write to it. 

macOS can perform file operations on files in several different file systems. Some operations can be done only on certain systems. For example, certain file systems honor `setuid` files when executed from them and some don’t. Be sure you know what file system you’re working with and what operations can be carried out on that system.

Local pathnames can point to remote files.  For example, the path `/volumes/foo` might actually be someone’s FTP server rather than a locally-mounted volume. Just because you’re accessing something by a pathname, that does not guarantee that it’s local or that it should be accessed.

A user can mount a file system anywhere they have write access and own the directory. In other words, almost anywhere a user can create a directory, they can mount a file system on top of it. Because this can be done remotely, an attacker running as root on a remote system could mount a file system into your home directory. Files in that file system would appear to be files in your home directory owned by root. For example, `/tmp/foo` might be a local directory, or it might be the root mount point of a remotely mounted file system. Similarly, `/tmp/foo/bar` might be a local file, or it might have been created on another machine and be owned by root over there. Therefore, you can’t trust files based only on ownership, and you can’t assume that setting the UID to 0 was done by someone you trust.  To tell whether the file is mounted locally, use the `[fstat](../../../../System/Conceptual/ManPages_iPhoneOS/man2/fstat.2.html#//apple_ref/doc/man/2/fstat)` call to check the device ID. If the device ID is different from that of files you know to be local, then you’ve crossed a device boundary.

Remember that users can read the contents of executable binaries just as easily as the contents of ordinary files. For example, the user can run `strings` to quickly see a list of (ostensibly) human-readable strings in your executable.

When you fork a new process, the child process inherits all the file descriptors from the parent unless you set the close-on-exec flag. If you fork and execute a child process and drop the child process’ privileges so its real and effective IDs are those of some other user (to avoid running that process with elevated privileges), then that user can use a debugger to attach the child process. They can then run arbitrary code from that running process. Because the child process inherited all the file descriptors from the parent, the user now has access to every file opened by the parent process. See [Inheriting File Descriptors](AccessControl.html#//apple_ref/doc/uid/TP40002589-SW10) for more information on this type of vulnerability.

        
            [Next](AccessControl.html)[Previous](ValidatingInput.html)
        
         Copyright &#x00a9; 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-09-13