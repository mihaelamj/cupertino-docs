---
title: "Introduction"
book: "Networking Overview"
chapterId: "TP40010220-CH12"
date: "2017-03-27"
description: "Explains basic networking concepts and terminology, and provides an overview of networking APIs."
identifier: "//apple_ref/doc/uid/TP40010220"
platforms: "OS X,iOS,tvOS,watchOS"
source: apple-archive
---

# Introduction

> **Networking Overview**
> Last updated: 2017-03-27

[Next](../WhyNetworkingIsHard/WhyNetworkingIsHard.html)
        
        
        [](../index.html)
        
        # About Networking
The world of networking is complex. Users can connect to the Internet using a wide range of technologies—cable modems, DSL, Wi-Fi, cellular connections, satellite uplinks, Ethernet, and even traditional acoustic modems. Each of these connections has distinct characteristics, including differences in bandwidth, latency, packet loss, and reliability.

To add further complexity, the user’s connection to the Internet does not tell the whole story. On its way from the user to an Internet server, the user’s network data passes through anywhere from one to dozens of physical interconnects, any one of which could be a high-speed OC-768 line (at almost 40 billion bits per second), a meager 300 baud modem (at 300 bits per second), or anything in-between. Worse, at any moment, the speed of the user’s connection to a server could change drastically—someone could turn on a microwave oven that interferes with the user’s Wi-Fi communications, the user could walk or drive out of cellular range, someone on the other side of the world could start downloading a large movie from the server that the user is trying to access, and so on.

As a developer of network-based software, your code must be able to adapt to changing network conditions, including performance, availability, and reliability. This document tells you how.

## At a Glance
Networks are inherently unreliable—cellular networks doubly so. As a result, good networking code tends to be somewhat complex. Among other things, your software should:

**Transfer only as much data as required to accomplish a task.** Minimizing the amount of data sent and received prolongs battery life, and may reduce the cost for users on metered Internet connections that bill by the megabyte.

**Avoid timeouts whenever possible.** You probably don’t want a webpage to stop loading just because the loading process took too long. Instead, provide a way for the user to cancel the operation.

In certain rare situations, data becomes irrelevant if delayed substantially. In these situations, it may make sense to use a protocol that does not retransmit packets. For example, if you are writing a real-time multiplayer game that sends tiny state messages to another device over a local area network (LAN) or Bluetooth, it is often better to miss a message and make assumptions about what is happening on the other device than to allow the operating system to queue those packets and deliver them all at once. For most purposes, however, unless you have to maintain compatibility with existing protocols, you should generally use TCP.

**Design user interfaces that allow the user to easily cancel transactions that are taking too long to complete.** If your app performs downloads of potentially large files, you should also provide a way to pause those downloads and resume them later.

**Handle failures gracefully.** A connection might fail for any number of reasons—the network might be unavailable, a hostname might not resolve successfully, and so on. When failures occur, your program should continue to function to the maximum degree possible in an offline state.

To add further complexity, sometimes a user may have access to resources only while on certain networks. For example, AirPlay can connect to an Apple TV only while on the same network. Corporate network resources can be accessed only while at work or over a virtual private network (VPN). Visual Voicemail may be accessible only over the cellular carrier’s network (depending on the carrier). And so on.

In particular, you should avoid interfaces that require the user to babysit your program when the network is malfunctioning. Don’t display modal dialogs to tell the user that the network is down. Do retry automatically when the network is working again. Don’t alert the user to connection failures that the user did not initiate.

**Degrade gracefully when network performance is slow.** Because the bandwidth between the user's device and his or her ISP is limited, your app can reach other devices on the user’s home network much more quickly than servers on the other side of the world. This difference becomes even greater when someone else on the local network starts using that limited bandwidth for other purposes.

**Choose APIs that are appropriate for the task.** If there is a high-level API that can meet your needs, use it instead of rolling your own implementation using low-level APIs. If there is an API specific to what you are doing (such as a game-centric API), use it.

By using the highest-level API, you are providing the operating system with more information about what you are actually trying to accomplish so that it can more optimally handle your request. These higher-level APIs also solve many of the most complex and difficult networking problems for you—caching, proxies, choosing from among multiple IP addresses for a host, and so on. If you write your own low-level code to perform the same tasks, you have to handle that complexity yourself (and debug and maintain the code in question).

**Design your software carefully to minimize security risks.** Take advantage of security technologies such as Secure Sockets Layer (SSL) and Transport Layer Security (TLS) to prevent spoofing and hide sensitive data from prying eyes, and scrutinize untrusted content to prevent buffer and integer overflows.

This document will help you learn these concepts and more.

### Learn Why Networking Is Hard
Although writing networking code can be easy, for all but the most trivial networking needs, writing *good* networking code is not. Depending on your software’s needs, it may need to adapt to changing network performance, dropped network connections, connection failures, and other problems caused by the inherent unreliability of the Internet itself.

> **Relevant Chapter:** [Designing for Real-World Networks](../WhyNetworkingIsHard/WhyNetworkingIsHard.html#//apple_ref/doc/uid/TP40010220-CH13-SW1)

### OS X and iOS Provide APIs at Many Levels
You can accomplish the following networking tasks in both OS X and iOS with identical or nearly identical code:

Performing HTTP/HTTPS requests, such as GET and POST requests

Establishing a connection to a remote host, with or without encryption or authentication

Listening for incoming connections

Sending and receiving data with connectionless protocols

Publishing, browsing, and resolving network services with Bonjour

> **Relevant Chapters:** [Assessing Your Networking Needs](../ChoosingTheRightNetworkingAPI/ChoosingTheRightNetworkingAPI.html#//apple_ref/doc/uid/TP40010220-CH7-SW6)

[Discovering and Advertising Network Services](../Discovering,Browsing,AndAdvertisingNetworkServices/Discovering,Browsing,AndAdvertisingNetworkServices.html#//apple_ref/doc/uid/TP40010220-CH9-SW1)

[Making HTTP and HTTPS Requests](../WorkingWithHTTPAndHTTPSRequests/WorkingWithHTTPAndHTTPSRequests.html#//apple_ref/doc/uid/TP40010220-CH8-SW1)

[Using Sockets and Socket Streams](../SocketsAndStreams/SocketsAndStreams.html#//apple_ref/doc/uid/TP40010220-CH203-CJBEFGHG)

### Secure Communication Is Your Responsibility
Proper networking security is a necessity. You should treat all data sent by your user as confidential and protect it accordingly. In particular, you should encrypt it during transit and protect against sending it to the wrong person or server.

Most OS X and iOS networking APIs provide easy integration with TLS for this purpose. TLS is the successor to the SSL protocol. In addition to encrypting data over the wire, TLS authenticates a server with a certificate to prevent spoofing.

Your server should also take steps to authenticate the client. This authentication could be as simple as a password or as complex as a hardware authentication token, depending on your needs.

Be wary of all incoming data. Any data received from an untrusted source may be a malicious attack. Your app should carefully inspect incoming data and immediately discard anything that looks suspicious.

> **Relevant Chapter:** [Using Networking Securely](../SecureNetworking/SecureNetworking.html#//apple_ref/doc/uid/TP40010220-CH1-SW1)

### iOS and OS X Offer Platform-Specific Features
The networking environment on OS X is highly configurable and extensible. The System Configuration framework provides APIs for determining and setting the current network configuration. Additionally, network kernel extensions enable you to extend the core networking infrastructure of OS X by adding features such as a firewall or VPN.

On iOS, you can use platform-specific networking APIs to handle authentication for captive networks and to designate Voice over Internet Protocol (VoIP) network streams.

> **Relevant Sections:** [iOS Requires You to Handle Backgrounding and Specify Cellular Usage Policies](../Platform-SpecificNetworkingTechnologies/Platform-SpecificNetworkingTechnologies.html#//apple_ref/doc/uid/TP40010220-CH212-SW5)

[OS X Lets You Make Systemwide Changes](../Platform-SpecificNetworkingTechnologies/Platform-SpecificNetworkingTechnologies.html#//apple_ref/doc/uid/TP40010220-CH212-SW2)

### Networking Must Be Dynamic and Asynchronous
A device’s network environment can change at a moment’s notice. There are a number of simple (yet devastating) networking mistakes that can adversely affect your app’s performance and usability, such as executing synchronous networking code on your program’s main thread, failing to handle network changes gracefully, and so on. You can save a lot of time and effort by designing your program to avoid these issues to begin with instead of debugging it later.

> **Relevant Chapter:** [Avoiding Common Networking Mistakes](../CommonPitfalls/CommonPitfalls.html#//apple_ref/doc/uid/TP40010220-CH4-SW1)

## How to Use This Document
This document is intended to be read sequentially.

The first chapter, [Designing for Real-World Networks](../WhyNetworkingIsHard/WhyNetworkingIsHard.html#//apple_ref/doc/uid/TP40010220-CH13-SW1), explains the challenges you will face when writing software that uses networking, why latency matters, and other concepts that you should know before you write the first line of networking code.

The next chapter, [Assessing Your Networking Needs](../ChoosingTheRightNetworkingAPI/ChoosingTheRightNetworkingAPI.html#//apple_ref/doc/uid/TP40010220-CH7-SW6), provides more details about choosing an API family and determining what types of networking tasks your program will perform. This chapter then points you to other chapters ([Discovering and Advertising Network Services](../Discovering,Browsing,AndAdvertisingNetworkServices/Discovering,Browsing,AndAdvertisingNetworkServices.html#//apple_ref/doc/uid/TP40010220-CH9-SW1), [Making HTTP and HTTPS Requests](../WorkingWithHTTPAndHTTPSRequests/WorkingWithHTTPAndHTTPSRequests.html#//apple_ref/doc/uid/TP40010220-CH8-SW1), and [Using Sockets and Socket Streams](../SocketsAndStreams/SocketsAndStreams.html#//apple_ref/doc/uid/TP40010220-CH203-CJBEFGHG)) that describe some common networking tasks that your program might need to perform.

[Using Networking Securely](../SecureNetworking/SecureNetworking.html#//apple_ref/doc/uid/TP40010220-CH1-SW1) and [Avoiding Common Networking Mistakes](../CommonPitfalls/CommonPitfalls.html#//apple_ref/doc/uid/TP40010220-CH4-SW1) provide guidance that can help you avoid common networking mistakes.

Finally, [Supporting IPv6 DNS64/NAT64 Networks](../UnderstandingandPreparingfortheIPv6Transition/UnderstandingandPreparingfortheIPv6Transition.html#//apple_ref/doc/uid/TP40010220-CH213-SW1) explains how to make sure your app is compatible with IPv6-only networks.

## See Also
This document is intended as a high-level overview of networking concerns in OS X and iOS. The documents below provide additional depth and breadth.

### Learn What’s Happening Under the Hood
A basic understanding of the way networks work can help you understand why they behave (or misbehave) as they do. Thus, you should learn at least the basic underlying concepts before you write the first line of code. At minimum, you should be familiar with packets and encapsulation, connection-based versus connectionless protocols, subnets and routing, domain name lookup, bandwidth, and latency. To learn about this subject, read the following document:

*[Networking Concepts](../../../../NetworkingInternet/Conceptual/NetworkingConcepts/Introduction/Introduction.html#//apple_ref/doc/uid/TP40012487)*

### Learn About Specific Technologies
For more in-depth information, consult one of the following guides for the primary documentation on a particular subject:

*URL Loading System Programming Guide*

*[Stream Programming Guide](../../../../Cocoa/Conceptual/Streams/Streams.html#//apple_ref/doc/uid/10000188i)*

*[CFNetwork Programming Guide](../../../../Networking/Conceptual/CFNetwork/Introduction/Introduction.html#//apple_ref/doc/uid/TP30001132)*

*[NSNetServices and CFNetServices Programming Guide](../../../../Networking/Conceptual/NSNetServiceProgGuide/Introduction.html#//apple_ref/doc/uid/TP40002736)*

### Learn How to Share Documents Between OS X and iOS
The following documents describe techniques you can use to share documents between OS X and iOS:

*[iCloud Design Guide](../../../../General/Conceptual/iCloudDesignGuide/Chapters/Introduction.html#//apple_ref/doc/uid/TP40012094)*

*[Document Transfer Strategies](../../../../../technotes/tn2152/_index.html#//apple_ref/doc/uid/DTS40009179)*

        
            [Next](../WhyNetworkingIsHard/WhyNetworkingIsHard.html)
        
         Copyright &#x00a9; 2004, 2017 Apple Inc. All Rights Reserved.  [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html)   |  [Privacy Policy](http://www.apple.com/privacy/)  |  Updated: 2017-03-27