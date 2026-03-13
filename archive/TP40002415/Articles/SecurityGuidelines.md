---
title: "Appendix B: Third-Party Software Security Guidelines"
book: "Secure Coding Guide"
chapterId: "TP40009511"
date: "2016-09-13"
description: "Describes techniques to use and factors to consider to make your code more secure from attack."
identifier: "//apple_ref/doc/uid/TP40002415"
source: apple-archive
---

# Appendix B: Third-Party Software Security Guidelines

> **Secure Coding Guide**
> Last updated: 2016-09-13

[Next](../RevisionHistory.html)[Previous](../SecurityDevelopmentChecklists/SecurityDevelopmentChecklists.html)
        
        
        [](../index.html)
        
        # Third-Party Software Security Guidelines
This appendix provides secure coding guidelines for software to be bundled with Apple products.

Insecure software can pose a risk to the overall security of users’ systems. Security issues can lead to negative publicity and end-user support problems for Apple and third parties.

## Respect Users’ Privacy
Your bundled software may use the Internet to communicate with your servers or third party servers. If so, you should provide clear and concise information to the user about what information is sent or retrieved and the reason for sending or receiving it.

Encryption should be used to protect the information while in transit. Servers should be authenticated before transferring information.

## Provide Upgrade Information
Provide information on how to upgrade to the latest version. Consider implementing a “Check for updates…” feature. Customers expect (and should receive) security fixes that affect the software version they are running.

You should have a way to communicate available security fixes to customers.

If possible, you should use the Mac App Store for providing upgrades. The Mac App Store provides a single, standard interface for updating all of a user’s software. The Mac App Store also provides an expedited app review process for handling critical security fixes.

## Store Information in Appropriate Places
Store user-specific information in the home directory, with appropriate file system permissions.

Take special care when dealing with shared data or preferences.

Follow the guidelines about file system permissions set forth in *[File System Programming Guide](../../../../FileManagement/Conceptual/FileSystemProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010672)*.

Take care to avoid race conditions and information disclosure when using temporary files. If possible, use a user-specific temporary file directory.

## Avoid Requiring Elevated Privileges
Do not require or encourage users to be logged in as an admin user to install or use your application. You should regularly test your application as a normal user to make sure that it works as expected.

## Implement Secure Development Practices
Educate your developers on how to write secure code to avoid the most common classes of vulnerabilities:

Buffer overflows

Integer overflows

Race conditions

Format string vulnerabilities

Pay special attention to code that:

deals with potentially untrusted data, such as documents or URLs

communicates over the network

handles passwords or other sensitive information

runs with elevated privileges such as root or in the kernel

Use APIs appropriate for the task:

Use APIs that take security into account in their design.

Avoid low-level C code when possible (e.g. use NSString instead of C-strings).

Use the security features of macOS to protect user data.

## Test for Security
As appropriate for your product, use the following QA techniques to find potential security issues:

Test for invalid and unexpected data in addition to testing what is expected. (Use fuzzing tools, include unit tests that test for failure, and so on.)

Static code analysis

Code reviews and audits

## Helpful Resources
The other chapters in this document describe best practices for writing secure code, including more information on the topics referenced above.

*[Security Overview](../../Security_Overview/Introduction/Introduction.html#//apple_ref/doc/uid/TP30000976)* and *[Cryptographic Services Guide](../../cryptoservices/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011172)* contain detailed information on security functionality in macOS that developers can use.

        
            [Next](../RevisionHistory.html)[Previous](../SecurityDevelopmentChecklists/SecurityDevelopmentChecklists.html)
        
         Copyright &#x00a9; 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-09-13