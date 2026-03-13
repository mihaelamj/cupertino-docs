---
title: "Types of Security Vulnerabilities"
book: "Secure Coding Guide"
chapterId: "TP40002529"
date: "2016-09-13"
description: "Describes techniques to use and factors to consider to make your code more secure from attack."
identifier: "//apple_ref/doc/uid/TP40002415"
source: apple-archive
---

# Types of Security Vulnerabilities

> **Secure Coding Guide**
> Last updated: 2016-09-13

[Next](BufferOverflows.html)[Previous](../Introduction.html)
        
        
        [](../index.html)
        
        # Types of Security Vulnerabilities
Most software security vulnerabilities fall into one of a small set of categories:

buffer overflows

unvalidated input

race conditions

access-control problems

weaknesses in authentication, authorization, or cryptographic practices

This chapter describes the nature of each type of vulnerability.

## Buffer Overflows
A buffer overflow occurs when an application attempts to write data past the end (or, occasionally, past the beginning) of a buffer.

Buffer overflows can cause applications to crash, can compromise data, and can provide an attack vector for further privilege escalation to compromise the system on which the application is running.

Books on software security invariably mention buffer overflows as a major source of vulnerabilities. Exact numbers are hard to come by, but as an indication, approximately 20% of the published exploits reported by the United States Computer Emergency Readiness Team (US-CERT) for 2004 involved buffer overflows.

Any application or system software that takes input from the user, from a file, or from the network has to store that input, at least temporarily. Except in special cases, most application memory is stored in one of two places:

*stack*—A part of an application’s address space that stores data that is specific to a single call to a particular function, method, block, or other equivalent construct.

*heap*—General purpose storage for an application. Data stored in the heap remains available as long as the application is running (or until the application explicitly tells the operating system that it no longer needs that data).

Class instances, data allocated with `[malloc](../../../../System/Conceptual/ManPages_iPhoneOS/man3/malloc.3.html#//apple_ref/doc/man/3/malloc)`, core foundation objects, and most other application data resides on the heap. (Note, however, that the local variables that actually point to the data are stored in the stack.)

Buffer overflow attacks generally occur by compromising either the stack, the heap, or both. For more information, read [Avoiding Buffer Overflows and Underflows](BufferOverflows.html#//apple_ref/doc/uid/TP40002577-SW1)

## Unvalidated Input
As a general rule, you should check all input received by your program to make sure that the data is reasonable.

For example, a graphics file can reasonably contain an image that is 200 by 300 pixels, but cannot reasonably contain an image that is 200 by -1 pixels. Nothing prevents a file from claiming to contain such an image, however. A naive program attempting to read such a file would attempt to allocate a buffer of an incorrect size, leading to the potential for a heap overflow attack or other problem. For this reason, you must check your input data carefully. This process is commonly known as input validation or sanity checking.

Any input received by your program from an untrusted source is a potential target for attack. (In this context, an ordinary user is an untrusted source.) Examples of input from an untrusted source include (but are not restricted to):

text input fields

commands passed through a URL used to launch the program

audio, video, or graphics files provided by users or other processes and read by the program

command line input

any data read from an untrusted server over a network

any untrusted data read from a trusted server over a network (user-submitted HTML or photos, for example)

Hackers look at every source of input to the program and attempt to pass in malformed data of every type they can imagine. If the program crashes or otherwise misbehaves, the hacker then tries to find a way to exploit the problem. Unvalidated-input exploits have been used to take control of operating systems, steal data, corrupt users’ disks, and more. One such exploit was even used to “jail break” iPhones.

[Validating Input and Interprocess Communication](ValidatingInput.html#//apple_ref/doc/uid/TP40007246-SW3) describes common types of input-validation vulnerabilities and what to do about them.

## Race Conditions
A *race condition* exists when changes to the order of two or more events can cause a change in behavior. If the correct order of execution is required for the proper functioning of the program, this is a bug. If an attacker can take advantage of the situation to insert malicious code, change a filename, or otherwise interfere with the normal operation of the program, the race condition is a security vulnerability. Attackers can sometimes take advantage of small time gaps in the processing of code to interfere with the sequence of operations, which they then exploit.

For more information about race conditions and how to prevent them, read [Race Conditions and Secure File Operations](RaceConditions.html#//apple_ref/doc/uid/TP40002585-SW1).

## Interprocess Communication
Separate processes—either within a single program or in two different programs—sometimes have to share information. Common methods include using shared memory or using some messaging protocol, such as Sockets, provided by the operating system. These messaging protocols used for interprocess communication are often vulnerable to attack; thus, when writing an application, you must always assume that the process at the other end of your communication channel could be hostile.

For more information on how to perform secure interprocess communication, read [Validating Input and Interprocess Communication](ValidatingInput.html#//apple_ref/doc/uid/TP40007246-SW3).

## Insecure File Operations
In addition to time-of-check–time-of-use problems, many other file operations are insecure. Programmers often make assumptions about the ownership, location, or attributes of a file that might not be true. For example, you might assume that you can always write to a file created by your program. However, if an attacker can change the permissions or flags on that file after you create it, and if you fail to check the result code after a write operation, you will not detect the fact that the file has been tampered with. 

Examples of insecure file operations include: 

writing to or reading from a file in a location writable by another user 

failing to make the right checks for file type, device ID, links, and other settings before using a file

failing to check the result code after a file operation

assuming that if a file has a local pathname, it has to be a local file

These and other insecure file operations are discussed in more detail in [Securing File Operations](RaceConditions.html#//apple_ref/doc/uid/TP40002585-SW9). 

## Access Control Problems
*Access control* is the process of controlling who is allowed to do what. This ranges from controlling physical access to a computer—keeping your servers in a locked room, for example—to specifying who has access to a resource (a file, for example) and what they are allowed to do with that resource (such as read only). Some access control mechanisms are enforced by the operating system, some by the individual application or server, some by a service (such as a networking protocol) in use. Many security vulnerabilities are created by the improper use of access controls, or by the failure to use them at all.

Much of the discussion of security vulnerabilities in the software security literature is in terms of *privileges*, and many exploits involve an attacker somehow gaining more privileges than they should have. Privileges, also called *permissions*, are access rights granted by the operating system, controlling who is allowed to read and write files, directories, and attributes of files and directories (such as the permissions for a file), who can execute a program, and who can perform other restricted operations such as accessing hardware devices and making changes to the network configuration. File permissions and access control in macOS are discussed in *[File System Programming Guide](../../../../FileManagement/Conceptual/FileSystemProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010672)*.

Of particular interest to attackers is the gaining of *root privileges*, which refers to having the unrestricted permission to perform any operation on the system. An application running with root privileges can access everything and change anything. Many security vulnerabilities involve programming errors that allow an attacker to obtain root privileges. Some such exploits involve taking advantage of buffer overflows or race conditions, which in some special circumstances allow an attacker to escalate their privileges. Others involve having access to system files that should be restricted or finding a weakness in a program—such as an application installer—that is already running with root privileges. For this reason, it’s important to always run programs with as few privileges as possible. Similarly, when it is necessary to run a program with elevated privileges, you should do so for as short a time as possible.

Much access control is enforced by applications, which can require a user to *authenticate* before granting *authorization* to perform an operation. Authentication can involve requesting a user name and password, the use of a smart card, a biometric scan, or some other method. If an application calls the macOS Authorization Services application interface to authenticate a user, it can automatically take advantage of whichever authentication method is available on the user’s system. Writing your own authentication code is a less secure alternative, as it might afford an attacker the opportunity to take advantage of bugs in your code to bypass your authentication mechanism, or it might offer a less secure authentication method than the standard one used on the system. Authorization and authentication are described further in *[Security Overview](../../Security_Overview/Introduction/Introduction.html#//apple_ref/doc/uid/TP30000976)*.

*Digital certificates* are commonly used—especially over the Internet and with email—to authenticate users and servers, to encrypt communications, and to digitally sign data to ensure that it has not been corrupted and was truly created by the entity that the user believes to have created it. Incorrect use of digital certificates can lead to security vulnerabilities. For example, a server administration program shipped with a standard self-signed certificate, with the intention that the system administrator would replace it with a unique certificate. However, many system administrators failed to take this step, with the result that an attacker could decrypt communication with the server. [CVE-2004-0927]

It’s worth noting that nearly all access controls can be overcome by an attacker who has physical access to a machine and plenty of time. For example, no matter what you set a file’s permissions to, the operating system cannot prevent someone from bypassing the operating system and reading the data directly off the disk. Only restricting access to the machine itself and the use of robust encryption techniques can protect data from being read or corrupted under all circumstances. 

The use of access controls in your program is discussed in more detail in [Elevating Privileges Safely](AccessControl.html#//apple_ref/doc/uid/TP40002589-SW1).

## Secure Storage and Encryption
Encryption can be used to protect a user’s secrets from others, either during data transmission or when the data is stored. (The problem of how to protect a vendor’s data from being copied or used without permission is not addressed here.) macOS provides a variety of encryption-based security options, such as 

FileVault

the ability to create encrypted disk images

keychain

certificate-based digital signatures

encryption of email

SSL/TLS secure network communication

Kerberos authentication

The list of security options in iOS includes

passcode to prevent unauthorized use of the device

data encryption

the ability to add a digital signature to a block of data 

keychain

SSL/TLS secure network communication

Each service has appropriate uses, and each has limitations. For example, FileVault, which encrypts the contents of a user’s root volume (in macOS 10.7 and later) or home directory (in earlier versions), is a very important security feature for shared computers or computers to which attackers might gain physical access, such as laptops. However, it is not very helpful for computers that are physically secure but that might be attacked over the network while in use, because in that case the home directory is in an unencrypted state and the threat is from insecure networks or shared files. Also, FileVault is only as secure as the password chosen by the user—if the user selects an easily guessed password, or writes it down in an easily found location, the encryption is useless.

It is a serious mistake to try to create your own encryption method or to implement a published encryption algorithm yourself unless you are already an expert in the field. It is extremely difficult to write secure, robust encryption code that generates unbreakable ciphertext, and it is almost always a security vulnerability to try. For macOS, if you need cryptographic services beyond those provided by the macOS user interface and high-level programming interfaces, you can use the open-source CSSM Cryptographic Services Manager. See the documentation provided with the Open Source security code, which you can download at [http://developer.apple.com/darwin/projects/security/](http://developer.apple.com/darwin/projects/security/). For iOS, the development APIs should provide all the services you need.

For more information about macOS and iOS security features, read *[Authentication, Authorization, and Permissions Guide](../../AuthenticationAndAuthorizationGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011200)*. 

## Social Engineering
Often the weakest link in the chain of security features protecting a user’s data and software is the user. As developers eliminate buffer overflows, race conditions, and other security vulnerabilities, attackers increasingly concentrate on tricking users into executing malicious code or handing over passwords, credit-card numbers, and other private information. Misleading a user into giving up secrets or into giving access to a computer to an attacker is known as *social engineering*. Millions of users suffer losses each year from such attacks.

Software developers can counter this in two ways: by educating their users, and through clear and well-designed user interfaces that give users the information they need to make informed decisions.

For more advice on how to design a user interface that enhances security, see [Designing Secure User Interfaces](AppInterfaces.html#//apple_ref/doc/uid/TP40002862-SW1).

        
            [Next](BufferOverflows.html)[Previous](../Introduction.html)
        
         Copyright &#x00a9; 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-09-13