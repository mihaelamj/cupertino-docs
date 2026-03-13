---
title: "Introduction"
book: "Secure Coding Guide"
chapterId: "TP40002477"
date: "2016-09-13"
description: "Describes techniques to use and factors to consider to make your code more secure from attack."
identifier: "//apple_ref/doc/uid/TP40002415"
platforms: "OS X"
source: apple-archive
---

# Introduction

> **Secure Coding Guide**
> Last updated: 2016-09-13

[Next](Articles/TypesSecVuln.html)
        
        
        [](index.html)
        
        # Introduction to Secure Coding Guide
Secure coding is the practice of writing software that’s resistant to attack by malicious or mischievous people or programs. An insecure program can provide access for an attacker to take control of a server or a user’s computer, resulting in anything from denial of service to a single user, to the compromise of secrets, loss of service, or damage to the systems of thousands of users. Secure coding helps protect a user’s data from theft or corruption.

Secure coding is important for all software, from small scripts your write for yourself to large scale, commercial apps. If you write software of any kind, familiarize yourself with the information in this document.

## At a Glance
Security is not something that can be added to software as an afterthought. Just as a shed made out of cardboard cannot be made secure by adding a padlock to the door, an insecure tool or application may require extensive redesign to secure it. You must identify the nature of the threats to your software and incorporate secure coding practices throughout the planning and development of your product. This book describes specific types of vulnerabilities and gives guidance on code hardening techniques to fix them.

### Hackers and Attackers
Contrary to the usage often found in the media, within the computer industry the term *hacker* refers to an expert programmer—one who enjoys learning about the intricacies of code or an operating system. In general, hackers are not malicious. When most hackers find security vulnerabilities, they inform the company or organization that’s responsible for the code so that they can fix the problem. Unfortunately, however, a small but important minority of hackers devise *exploits*, code that takes advantage of the vulnerability, and use them to attack, or publish the exploits for others to use.

Attackers may be motivated by a desire to steal money, identities, and other secrets for personal gain; corporate secrets for their employer’s or their own use; or state secrets for use by hostile governments or terrorist organizations. Some break into apps or operating systems just to show that they can do it. Others seek to do real damage. Because attacks can be automated and replicated, any weakness, no matter how slight, poses a real danger.

### No Platform Is Immune
Both macOS and iOS have strong records when it comes to resisting attack. Originally built on open source software such as BSD, and reinforced over the years with technologies like code signing and App Sandbox, macOS offers many layers of defense. iOS provides even greater security by strictly enforcing sandboxing for all apps. Additionally, Apple actively reviews all of its platforms for vulnerabilities, and issues downloadable security updates regularly.

That’s the good news. The bad news is that apps and operating systems are constantly under attack. Every day, attackers look for new vulnerabilities, and for ways to exploit them. Further, a large-scale, widespread attack isn’t needed to cause monetary and other damages; a single compromised app is sufficient if it puts valuable information at risk. Although major attacks of viruses or worms get a lot of attention from the media, the destruction or compromise of data on a single computer is what matters to the average user. So it’s important to take every security risk seriously, and work to correct known problems quickly. 

## How to Use This Document
First, familiarize yourself with the concepts in *[Security Overview](../Security_Overview/Introduction/Introduction.html#//apple_ref/doc/uid/TP30000976)*.

This document then starts with a brief introduction to the nature of each of the types of security vulnerability commonly found in software. If you’re not sure what a race condition is, for example, or why it poses a security risk, the [Types of Security Vulnerabilities](Articles/TypesSecVuln.html#//apple_ref/doc/uid/TP40002529-SW2) chapter is the place to start.

The remaining chapters detail specific types of security vulnerabilities. These chapters can be read in any order, or as suggested by the software development checklist in [Security Development Checklists](SecurityDevelopmentChecklists/SecurityDevelopmentChecklists.html#//apple_ref/doc/uid/TP40002415-CH1-SW1).

[Avoiding Buffer Overflows and Underflows](Articles/BufferOverflows.html#//apple_ref/doc/uid/TP40002577-SW1) describes the various types of buffer overflows and explains how to avoid them. 

[Validating Input and Interprocess Communication](Articles/ValidatingInput.html#//apple_ref/doc/uid/TP40007246-SW3) discusses why and how you must validate every type of input your program receives from untrusted sources. 

[Race Conditions and Secure File Operations](Articles/RaceConditions.html#//apple_ref/doc/uid/TP40002585-SW1) explains how race conditions occur, discusses ways to avoid them, and describes insecure and secure file operations. 

[Elevating Privileges Safely](Articles/AccessControl.html#//apple_ref/doc/uid/TP40002589-SW1) describes how to avoid running code with elevated privileges and what to do if you can’t avoid it entirely. 

[Designing Secure User Interfaces](Articles/AppInterfaces.html#//apple_ref/doc/uid/TP40002862-SW1) discusses how the user interface of a program can enhance or compromise security and gives some guidance on how to write a security-enhancing UI. 

[Designing Secure Helpers and Daemons](DesigningSecureHelpers/DesigningSecureHelpers.html#//apple_ref/doc/uid/TP40002415-CH2-SW1) describes how to design helper applications in ways that are conducive to privilege separation.

The appendix [Security Development Checklists](SecurityDevelopmentChecklists/SecurityDevelopmentChecklists.html#//apple_ref/doc/uid/TP40002415-CH1-SW1) provides a convenient list of tasks that you should perform before shipping an application, while the appendix [Third-Party Software Security Guidelines](Articles/SecurityGuidelines.html#//apple_ref/doc/uid/TP40009511-SW1) provides a list of guidelines for third-party applications bundled with macOS.

## See Also
This document concentrates on security vulnerabilities and programming practices of special interest to developers using macOS or iOS. For more general treatments, see the following books and documents:

See Viega and McGraw, *Building Secure Software*, Addison Wesley, 2002; for a general discussion of secure programming, especially as it relates to C programming and writing scripts.

See Wheeler, *Secure Programming HOWTO*, available at [http://www.dwheeler.com/secure-programs/](http://www.dwheeler.com/secure-programs/); for discussions of several types of security vulnerabilities and programming tips for UNIX-based operating systems, most of which apply to macOS.

See Cranor and Garfinkel, *Security and Usability: Designing Secure Systems that People Can Use*, O’Reilly, 2005; for information on writing user interfaces that enhance security.

For documentation of security-related application programming interfaces (APIs), see the following documents: 

For information on secure networking, see *[Secure Transport Reference](https://developer.apple.com/documentation/security/secure_transport)*.

For information on macOS authorization and authentication APIs, see *[Authorization Services C Reference](https://developer.apple.com/documentation/security/authorization_services)* and *[Security Foundation Framework Reference](https://developer.apple.com/documentation/securityfoundation)*.

If you are using digital certificates for authentication, see *[Certificate, Key, and Trust Services Reference](https://developer.apple.com/documentation/security/certificate_key_and_trust_services)*. 

For secure storage of passwords and other secrets, see *[Keychain Services Reference](https://developer.apple.com/documentation/security/keychain_services)*.

For information about security in web application design, visit [http://www.owasp.org/](http://www.owasp.org/).

        
            [Next](Articles/TypesSecVuln.html)
        
         Copyright &#x00a9; 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-09-13