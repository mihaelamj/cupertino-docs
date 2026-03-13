---
title: "Designing Secure User Interfaces"
book: "Secure Coding Guide"
chapterId: "TP40002862"
date: "2016-09-13"
description: "Describes techniques to use and factors to consider to make your code more secure from attack."
identifier: "//apple_ref/doc/uid/TP40002415"
source: apple-archive
---

# Designing Secure User Interfaces

> **Secure Coding Guide**
> Last updated: 2016-09-13

[Next](../DesigningSecureHelpers/DesigningSecureHelpers.html)[Previous](AccessControl.html)
        
        
        [](../index.html)
        
        # Designing Secure User Interfaces
The user is often the weak link in the security of a system. Many security breaches have been caused by weak passwords, unencrypted files left on unprotected computers, and successful social engineering attacks. Therefore, it is vitally important that your program’s user interface enhance security by making it easy for the user to make secure choices and avoid costly mistakes.

In a social engineering attack, the user is tricked into either divulging secret information or running malicious code. For example, the Melissa virus and the Love Letter worm each infected thousands of computers when users downloaded and opened files sent in email. 

This chapter discusses how doing things that are contrary to user expectations can cause a security risk, and gives hints for creating a user interface that minimizes the risk from social engineering attacks. Secure human interface design is a complex topic affecting operating systems as well as individual programs. This chapter gives only a few hints and highlights.

For an extensive discussion of this topic, see Cranor and Garfinkel, *Security and Usability: Designing Secure Systems that People Can Use*, O’Reilly, 2005.

## Use Secure Defaults
Most users use an application’s default settings and assume that they are secure. If they have to make specific choices and take multiple actions in order to make a program secure, few will do so. Therefore, the default settings for your program should be as secure as possible.

For example:

If your program launches other programs, it should launch them with the minimum privileges they need to run.

If your program supports optionally connecting by SSL, the checkbox should be checked by default.

If your program displays a user interface that requires the user to decide whether to perform a potentially dangerous action, the default option should be the safe choice. If there is no safe choice, there should be no default.

And so on.

There is a common belief that security and convenience are incompatible. With careful design, this does not have to be so. In fact, it is very important that the user not have to sacrifice convenience for security, because many users will choose convenience in that situation. In many cases, a simpler interface is more secure, because the user is less likely to ignore security features and less likely to make mistakes.

Whenever possible, you should make security decisions for your users: in most cases, you know more about security than they do, and if you can’t evaluate the evidence to determine which choice is most secure, the chances are your users will not be able to do so either.

For a detailed discussion of this issue and a case study, see the article “Firefox and the Worry-Free Web” in Cranor and Garfinkel, *Security and Usability: Designing Secure Systems that People Can Use*.

## Meet Users’ Expectations for Security
If your program handles data that the user expects to be kept secret, make sure that you protect that data at all times. That means not only keeping it in a secure location or encrypting it on the user’s computer, but not handing it off to another program unless you can verify that the other program will protect the data, and not transmitting it over an insecure network. If for some reason you cannot keep the data secure, you should make this situation obvious to users and give them the option of canceling the insecure operation.

> **Important:** The absence of an indication that an operation is secure is not a good way to inform the user that the operation is insecure. A common example of this is any web browser that adds a lock icon (usually small and inconspicuous) on web pages that are protected by SSL/TLS or some similar protocol. The user has to notice that this icon is not present (or that it’s in the wrong place, in the case of a spoofed web page) in order to take action. Instead, the program should prominently display some indication for each web page or operation that is *not* secure.

The user must be made aware of when they are granting authorization to some entity to act on their behalf or to gain access to their files or data. For example, a program might allow users to share files with other users on remote systems in order to allow collaboration. In this case, sharing should be off by default. If the user turns it on, the interface should make clear the extent to which remote users can read from and write to files on the local system. If turning on sharing for one file also lets remote users read any other file in the same folder, for example, the interface must make this clear before sharing is turned on. In addition, as long as sharing is on, there should be some clear indication that it is on, lest users forget that their files are accessible by others.

Authorization should be revocable: if a user grants authorization to someone, the user generally expects to be able to revoke that authorization later. Whenever possible, your program should not only make this possible, it should make it easy to do. If for some reason it will not be possible to revoke the authorization, you should make that clear before granting the authorization. You should also make it clear that revoking authorization cannot reverse damage already done (unless your program provides a restore capability).

Similarly, any other operation that affects security but that cannot be undone should either not be allowed or the user should be made aware of the situation before they act. For example, if all files are backed up in a central database and can’t be deleted by the user, the user should be aware of that fact before they record information that they might want to delete later.

As the user’s agent, you must carefully avoid performing operations that the user does not expect or intend. For example, avoid automatically running code if it performs functions that the user has not explicitly authorized. 

## Secure All Interfaces
Some programs have multiple user interfaces, such as a graphical user interface, a command-line interface, and an interface for remote access. If any of these interfaces require authentication (such as with a password), then all the interfaces should require it. Furthermore, if you require authentication through a command line or remote interface, be sure the authentication mechanism is secure—don’t transmit passwords in cleartext, for example.

## Place Files in Secure Locations
Unless you are encrypting all output, the location where you save files has important security implications. For example:

FileVault can secure the root volume (or the user’s home folder prior to OS X 10.7), but not other locations where the user might choose to place files.

Folder permissions can be set in such a way that others can manipulate their contents.

You should restrict the locations where users can save files if they contain information that must be protected. If you allow the user to select the location to save files, you should make the security implications of a particular choice clear; specifically, they must understand that, depending on the location of a file, it might be accessible to other applications or even remote users.

## Make Security Choices Clear
Most programs, upon detecting a problem or discrepancy, display a dialog box informing the user of the problem. Often this approach does not work, however. For one thing, the user might not understand the warning or its implications. For example, if the dialog warns the user that the site to which they are connecting has a certificate whose name does not match the name of the site, the user is unlikely to know what to do with that information, and is likely to ignore it. Furthermore, if the program puts up more than a few dialog boxes, the user is likely to ignore all of them.

To solve this problem, when giving the user a choice that has security implications, make the potential consequences of each choice clear. The user should never be surprised by the results of an action. The choice given to the user should be expressed in terms of consequences and trade-offs, not technical details.

For example, a choice of encryption methods should be based on the level of security (expressed in simple terms, such as the amount of time it might take to break the encryption) versus the time and disk space required to encrypt the data, rather than on the type of algorithm and the length of the key to be used. If there are no practical differences of importance to the user (as when the more secure encryption method is just as efficient as the less-secure method), just use the most secure method and don’t give the user the choice at all.

Be sensitive to the fact that few users are security experts. Give as much information—in clear, nontechnical terms—as necessary for them to make an informed decision. In some cases, it might be best not to give them the option of changing the default behavior.

For example, most users don’t know what a digital certificate is, let alone the implications of accepting a certificate signed by an unknown authority. Therefore, it is probably not a good idea to let the user permanently add an anchor certificate (a certificate that is trusted for signing other certificates) unless you can be confident that the user can evaluate the validity of the certificate. (Further, if the user is a security expert, they’ll know how to add an anchor certificate to the keychain without the help of your application anyway.)

If you are providing security features, you should make their presence clear to the user. For example, if your mail application requires the user to double click a small icon in order to see the certificate used to sign a message, most users will never realize that the feature is available.

In an often-quoted but rarely applied monograph, Jerome Saltzer and Michael Schroeder wrote “It is essential that the human interface be designed for ease of use, so that users routinely and automatically apply the protection mechanisms correctly. Also, to the extent that the user’s mental image of his protection goals matches the mechanisms he must use, mistakes will be minimized. If he must translate his image of his protection needs into a radically different specification language, he will make errors.” (Saltzer and Schroeder, “The Protection of Information in Computer Systems,” *Proceedings of the IEEE* 63:9, 1975.)

For example, you can assume the user understands that the data must be protected from unauthorized access; however, you cannot assume the user has any knowledge of encryption schemes or knows how to evaluate password strength. In this case, your program should present the user with choices like the following:

“Is your computer physically secure, or is it possible that an unauthorized user will have physical access to the computer?”

“Is your computer connected to a network?”

From the user’s answers, you can determine how best to protect the data. Unless you are providing an “expert” mode, do *not* ask the user questions like the following:

“Do you want to encrypt your data, and if so, with which encryption scheme?”

“How long a key should be used?”

“Do you want to permit SSH access to your computer?”

These questions don’t correspond with the user’s view of the problem. Therefore, the user’s answers to such questions are likely to be erroneous. In this regard, it is very important to understand the user’s perspective. Very rarely is an interface that seems simple or intuitive to a programmer actually simple or intuitive to average users.

To quote Ka-Ping Yee (*User Interaction Design for Secure Systems*, at [http://www.eecs.berkeley.edu/Pubs/TechRpts/2002/CSD-02-1184.pdf](http://www.eecs.berkeley.edu/Pubs/TechRpts/2002/CSD-02-1184.pdf)):

In order to have a chance of using a system safely in a world of unreliable and sometimes adversarial software, a user needs to have confidence in all of the following statements:

Things don’t become unsafe all by themselves. (Explicit Authorization)

I can know whether things are safe. (Visibility)

I can make things safer. (Revocability)

I don’t choose to make things unsafe. (Path of Least Resistance)

I know what I can do within the system. (Expected Ability)

I can distinguish the things that matter to me. (Appropriate Boundaries)

I can tell the system what I want. (Expressiveness) 

I know what I’m telling the system to do. (Clarity)

The system protects me from being fooled. (Identifiability, Trusted Path)

## Fight Social Engineering Attacks
Social engineering attacks are particularly difficult to fight. In a social engineering attack, the attacker fools the user into executing attack code or giving up private information.

A common form of social engineering attack is referred to as *phishing*.  Phishing refers to the creation of an official-looking email or web page that fools the user into thinking they are dealing with an entity with which they are familiar, such as a bank with which they have an account. Typically, the user receives an email informing them that there is something wrong with their account, and instructing them to click on a link in the email. The link takes them to a web page that spoofs a real one; that is, it includes icons, wording, and graphical elements that echo those the user is used to seeing on a legitimate web page. The user is instructed to enter such information as their social security number and password. Having done so, the user has given up enough information to allow the attacker to access the user’s account.

Fighting phishing and other social engineering attacks is difficult because the computer’s perception of an email or web page is fundamentally different from that of the user. For example, consider an email containing a link to `http://scamsite.example.com/` but in which the link’s text says `Apple Web Store`. From the computer’s perspective, the URL links to a scam site, but from the user’s perspective, it links to Apple’s online store. The user cannot easily tell that the link does not lead to the location they expect until they see the URL in their browser; the computer similarly cannot determine that the link’s text is misleading.

To further complicate matters, even when the user looks at the actual URL, the computer and user may perceive the URL differently. The Unicode character set includes many characters that look similar or identical to common English letters. For example, the Russian glyph that is pronounced like “r” looks exactly like an English “p” in many fonts, though it has a different Unicode value. These characters are referred to as *homographs*. When web browsers began to support internationalized domain names (IDN), some phishers set up websites that looked identical to legitimate ones, using homographs in their web addresses to fool users into thinking the URL was correct.

Some creative techniques have been tried for fighting social engineering attacks, including trying to recognize URLs that are similar to, but not the same as, well-known URLs, using private email channels for communications with customers, using email signing, and allowing users to see messages only if they come from known, trusted sources. All of these techniques have problems, and the sophistication of social engineering attacks is increasing all the time.

For example, to foil the domain name homograph attack, many browsers display internationalized domain names (IDN) in an ASCII format called “Punycode.” For example, an impostor website with the URL `http://www.apple.com/` that uses a Roman script for all the characters except for the letter “a”, for which it uses a Cyrillic character, is displayed as `http://www.xn--pple-43d.com`.

Different browsers use different schemes when deciding which internationalized domain names to show and which ones to translate. For example, Safari uses this form when a URL contains characters in two or more scripts that are not allowed in the same URL, such as Cyrillic characters and traditional ASCII characters. Other browsers consider whether the character set is appropriate for the user’s default language. Still others maintain a list of registries that actively prevent such spoofing and use punycode for domains from all other registries.

For a more in-depth analysis of the problem, more suggested approaches to fighting it, and some case studies, see *Security and Usability: Designing Secure Systems that People Can Use* by Cranor and Garfinkel.

To learn more about social engineering techniques in general, read *The Art of Deception: Controlling the Human Element of Security* by Mitnick, Simon, and Wozniak.

## Use Security APIs When Possible
One way to avoid adding security vulnerabilities to your code is to use the available security APIs whenever possible. The Security Interface Framework API provides a number of user interface views to support commonly performed security tasks.

> **iOS Note:** The Security Interface Framework is not available in iOS. In iOS, applications are restricted in their use of the keychain, and it is not necessary for the user to create a new keychain or change keychain settings.

The Security Interface Framework API provides the following views:

The `[SFAuthorizationView](https://developer.apple.com/documentation/securityinterface/sfauthorizationview)` class implements an authorization view in a window. An authorization view is a lock icon and accompanying text that indicates whether an operation can be performed. When the user clicks a closed lock icon, an authorization dialog displays. Once the user is authorized, the lock icon appears open. When the user clicks the open lock, Authorization Services restricts access again and changes the icon to the closed state.

The `[SFCertificateView](https://developer.apple.com/documentation/securityinterface/sfcertificateview)` and `[SFCertificatePanel](https://developer.apple.com/documentation/securityinterface/sfcertificatepanel)` classes display the contents of a certificate.

The `[SFCertificateTrustPanel](https://developer.apple.com/documentation/securityinterface/sfcertificatetrustpanel)` class displays and optionally lets the user edit the trust settings in a certificate.

The `[SFChooseIdentityPanel](https://developer.apple.com/documentation/securityinterface/sfchooseidentitypanel)` class displays a list of identities in the system and lets the user select one. (In this context, *identity* refers to the combination of a private key and its associated certificate.)

The `[SFKeychainSavePanel](https://developer.apple.com/documentation/securityinterface/sfkeychainsavepanel)` class adds an interface to an application that lets the user save a new keychain. The user interface is nearly identical to that used for saving a file. The difference is that this class returns a keychain in addition to a filename and lets the user specify a password for the keychain.

The `[SFKeychainSettingsPanel](https://developer.apple.com/documentation/securityinterface/sfkeychainsettingspanel)` class displays an interface that lets the user change keychain settings.

Documentation for the Security Interface framework is in *[Security Interface Framework Reference](https://developer.apple.com/documentation/securityinterface)*.

        
            [Next](../DesigningSecureHelpers/DesigningSecureHelpers.html)[Previous](AccessControl.html)
        
         Copyright &#x00a9; 2016 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2016-09-13