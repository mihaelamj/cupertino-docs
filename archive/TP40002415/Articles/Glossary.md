---
title: "Glossary"
book: "Secure Coding Guide"
chapterId: "TP40002494"
date: "2016-09-13"
description: "Describes techniques to use and factors to consider to make your code more secure from attack."
identifier: "//apple_ref/doc/uid/TP40002415"
source: apple-archive
---

# Glossary

> **Secure Coding Guide**
> Last updated: 2016-09-13

[Next](../Index/index_of_book.html)[Previous](../RevisionHistory.html)
        
        
        [](../index.html)
        
        # Glossary

**AES encryption**Abbreviation for Advanced Encryption Standard encryption. A Federal Information Processing Standard (FIPS), described in FIPS publication 197. AES has been adopted by the U.S. government for the protection of sensitive, non-classified information.

**attacker**Someone deliberately trying to make a program or operating system do something that it’s not supposed to do, such as allowing the attacker to execute code or read private data. 

**authentication**The process by which a person or other entity (such as a server) proves that it is who (or what) it says it is. Compare with [authorization](#//apple_ref/doc/uid/TP40002494-SW6).

**authorization**The process by which an entity such as a user or a server gets the right to perform a privileged operation. (Authorization can also refer to the right itself, as in “Bob has the authorization to run that program.”) Authorization usually involves first authenticating the entity and then determining whether it has the appropriate privileges. See also [authentication](#//apple_ref/doc/uid/TP40002494-SW5).

**buffer overflow**The insertion of more data into a memory buffer than was reserved for the buffer, resulting in memory locations outside the buffer being overwritten. See also [heap overflow](#//apple_ref/doc/uid/TP40002494-SW1) and [stack overflow](#//apple_ref/doc/uid/TP40002494-SW2).

**CDSA**Abbreviation for Common Data Security Architecture. An open software standard for a security infrastructure that provides a wide array of security services, including fine-grained access permissions, authentication of users, encryption, and secure data storage. CDSA has a standard application programming interface, called CSSM. 

**CERT Coordination Center**A center of Internet security expertise, located at the Software Engineering Institute, a federally funded research and development center operated by Carnegie Mellon University. CERT is an acronym for Computer Emergency Readiness Team.)

**certificate**See [digital certificate](#//apple_ref/doc/uid/TP40002494-SW7).

**Common Criteria**A standardized process and set of standards that can be used to evaluate the security of software products developed by the governments of the United States, Canada, the United Kingdom, France, Germany, and the Netherlands.

**CSSM**Abbreviation for Common Security Services Manager. A public application programming interface for CDSA. CSSM also defines an interface for plug-ins that implement security services for a particular operating system and hardware environment.

**CVE**Abbreviation for Common Vulnerabilities and Exposures. A dictionary of standard names for security vulnerabilities located at [http://www.cve.mitre.org/](http://www.cve.mitre.org/). You can run an Internet search on the CVE number to read details about the vulnerability.

**digital certificate**A collection of data used to verify the identity of the holder. macOS supports the X.509 standard for digital certificates.

**exploit**A program or sample code that demonstrates how to take advantage of a vulnerability.)

**FileVault**An macOS feature, configured through the Security system preference, that encrypts everything in on the root volume (or everything in the user’s home directory prior to 10.7).

**hacker**An expert programmer—generally one with the skill to create an exploit. Most hackers do not attack other programs, and some publish exploits with the intent of forcing software developers to fix vulnerabilities.

**heap**A region of memory reserved for use by a program during execution. Data can be written to or read from any location on the heap, which grows upward (toward higher memory addresses). Compare with stack.

**heap overflow**A buffer overflow in the heap.

**homographs**Characters that look the same but have different Unicode values, such as the Roman character p and the Russian glyph that is pronounced like “r”.

**integer overflow**A buffer overflow caused by entering a number that is too large for an integer data type.

**Kerberos**An industry-standard protocol created by the Massachusetts Institute of Technology (MIT) to provide authentication over a network.

**keychain**A database used in macOS to store encrypted passwords, private keys, and other secrets. It is also used to store certificates and other non-secret information that is used in cryptography and authentication. 

**Keychain Access utility**An application that can be used to manipulate data in the keychain.

**Keychain Services**A public API that can be used to manipulate data in the keychain.

**level of trust**The confidence a user can have in the validity of a certificate. The level of trust for a certificate is used together with the trust policy to answer the question “Should I trust this certificate for this action?”

**nonrepudiation**A process or technique making it impossible for a user to deny performing an operation (such as using a specific credit card number).

**Open Directory**The directory server provided by macOS for secure storage of passwords and user authentication.

**permissions**See [privileges](#//apple_ref/doc/uid/TP40002494-SW4).

**phishing**A social engineering technique in which an email or web page that spoofs one from a legitimate business is used to trick a user into giving personal data and secrets (such as passwords) to someone who has malicious intent.

**policy database**A database containing the set of rules the Security Server uses to determine authorization.

**privileged operation**An operation that requires special rights or privileges.

**privileges**The type of access to a file or directory (read, write, execute, traverse, and so forth) granted to a user or to a group.

**race condition**The occurrence of two events out of sequence. 

**root kit**Malicious code that, by running in the kernel, can not only take over control of the system but can also cover up all evidence of its own existence.

**root privileges**Having the unrestricted permission to perform any operation on the system.

**signal**A message sent from one process to another in a UNIX-based operating system (such as macOS)

**social engineering**As applied to security, tricking a user into giving up secrets or into giving access to a computer to an attacker.

**smart card**A plastic card similar in size to a credit card that has memory and a microprocessor embedded in it. A smart card can store and process information, including passwords, certificates, and keys.

**stack**A region of memory reserved for use by a specific program and used to control program flow. Data is put on the stack and removed in a last-in–first-out fashion. The stack grows downward (toward lower memory addresses). Compare with heap.

**stack overflow**A buffer overflow on the stack.

**time of check–time of use (TOCTOU)**A race condition in which an attacker creates, writes to, or alters a file between the time when a program checks the status of the file and when the program writes to it. 

**trust policy**A set of rules that specify the appropriate uses for a certificate that has a specific level of trust. For example, the trust policy for a browser might state that if a certificate has expired, the user should be prompted for permission before a secure session is opened with a web server.

**vulnerability**A feature of the way a program was written—either a design flaw or a bug—that makes it possible for a hacker to attack the program. 

        
            [Next](../Index/index_of_book.html)[Previous](../RevisionHistory.html)
        
         Copyright &#x00a9; 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-09-13