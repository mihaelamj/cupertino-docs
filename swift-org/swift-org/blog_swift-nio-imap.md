---
source: https://www.swift.org/blog/swift-nio-imap/
crawled: 2025-11-15T21:22:34Z
---

# Announcing SwiftNIO IMAP

[ ](/)
 
 
 
 
 
 
 
- 
 [Docs](/documentation/)
 

 
 
- 
 [Community](/community/)
 

 
 
- 
 [Packages](/packages/)
 

 
 
- 
 [Blog](/blog/)
 

 
 
- 
 
 

 
- 
 [Install (6.2.1)](/install)
 

 
 
 
 
 
 
 
 
 
 
 
- 
 [Docs](/documentation/)
 

 
 
- 
 [Community](/community/)
 

 
 
- 
 [Packages](/packages/)
 

 
 
- 
 [Blog](/blog/)
 

 
 
- 
 
 

 
- 
 [Install (6.2.1)](/install)
 

 
 
 
 

 
 # Announcing SwiftNIO IMAP

 
 
 
 
 
 
 
 
 *
 
 
 [Daniel Eggert](https://github.com/danieleggert/)
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 [Cory Benfield](https://github.com/Lukasa/)
 
 
 
 
 
 
 

 August 30, 2022
 
 
 As part of expanding the Swift on Server ecosystem, we’re thrilled to announce the release of a new IMAPv4 parser and encoder, SwiftNIO IMAP.

This package implements:

 
- Parsing IMAPv4 wire-format to type-safe Swift data structures

 
- Encoding these Swift data structures to the IMAPv4 wire format

 
- Extensive support for common IMAP extensions

 
- High performance

 
- Integration with SwiftNIO

Motivation 
  
 

Email has been a indispensable part of the internet for over 40 years and is a ubiquitous part of many products and services today.

Internet Message Access Protocol (IMAP) is the most widely used open standard to retrieve email messages. It has been around for multiple decades and has evolved substantially over the years through various RFCs.

Correctly parsing and encoding IMAP is notoriously difficult. SwiftNIO IMAP reduces this difficulty by handling many subtle details of encoding and parsing IMAP, making it easier than ever to write rich and powerful email integrations with Swift on server.

This package is focused on parsing and encoding IMAP while also providing some common convenience methods related to core IMAP types. It does not implement any of busines logic related to IMAP.

A Brief Tour 
  
 

The SwiftNIO IMAP targets RFC 3501, IMAP version 4rev1, and additionally supports extensions from more than 20 RFCs: RFC 2087, 2177, 2221, 2342, 2971, 3348, 3501, 3502, 3516, 3691, 4315, 4467, 4469, 4731, 4959, 5032, 5161, 5182, 5258, 5464, 5819, 6154, 6851, 7162, 7377, 7888, and 8438.

IMAP uses a text-based “human readable” wire format, and SwiftNIO IMAP bridges this to a type-safe world using modern Swift data structures. The protocol is “asymetric”: messages sent from the server follow different patterns that messages sent by the client.

Example Exchange 
  
 

As a quick example, here’s part of the the exchange listed in RFC 3501 section 8, where lines starting with `S:` and `C:` are from the server and client respectively:



```
S:   * OK IMAP4rev1 Service Ready
C:   a001 login mrc secret
S:   a001 OK LOGIN completed
C:   a002 select inbox
S:   * 18 EXISTS
S:   * FLAGS (Answered Flagged Deleted Seen Draft)
S:   * 2 RECENT
S:   * OK [UNSEEN 17] Message 17 is the first unseen message
S:   * OK [UIDVALIDITY 3857529045] UIDs valid
S:   a002 OK [READ-WRITE] SELECT completed

```



The first 3 lines would correspond to the following in SwiftNIO IMAP:



```
Response.untagged(.conditionalState(.ok(ResponseText(text: "IMAP4rev1 Service Ready"))))
CommandStreamPart.tagged(TaggedCommand(tag: "a001", command: .login(username: "mrc", password: "secret")))
Response.tagged(.init(tag: "a001", state: .ok(ResponseText(text: "LOGIN completed"))))

```



Next, up is the `SELECT` command and its responses, which are more interesting:



```
CommandStreamPart.tagged(TaggedCommand(tag: "a002", command: .select(MailboxName("box1"), [])))
Response.untagged(.mailboxData(.exists(18)))
Response.untagged(.mailboxData(.flags([.answered, .flagged, .deleted, .seen, .draft])))
Response.untagged(.mailboxData(.recent(2)))
Response.untagged(.conditionalState(.ok(ResponseText(code: .unseen(17), text: "Message 17 is the first unseen message"))))
Response.untagged(.conditionalState(.ok(ResponseText(code: .uidValidity(3857529045), text: "UIDs valid"))))
Response.tagged(.init(tag: "a002", state: .ok(ResponseText(code: .readWrite, text: "SELECT completed"))))

```



There’s more going on here than this example shows. But this gives a general idea of how things look and feel.

Common Values 
  
 

Some of the very common values used in IMAP are UIDs, message sequence numbers, and mailbox names.

SwiftNIO IMAP has a `UID` and `SequenceNumber` type, and related types such as `UIDRange`, `UIDSet`, `SequenceRange`, and `SequenceSet`. The two set types conform to `SetAlgebra`. And all of these have convenience methods for common operations.

Mailboxes are identified by a “modified UTF-7” encoded string. The `MailboxName` and `MailboxPath` types support decoding and encoding these, while allowing to round-trip wrongly-encoded byte strings sometimes found in the wild.

Transparent Literal Support 
  
 

SwiftNIO IMAP can transparently encode and decode client messages both with the synchronizing and non-synchronizing literal encodings from the base RFC 3501 and RFC 7888 extensions.

These variations are handled transparently - for both client and server:

RFC 3501 “quoted” strings:



```
C: A001 LOGIN "FRED FOOBAR" "fat man"
S: A001 OK LOGIN completed

```



RFC 3501 command continuations:



```
C: A001 LOGIN {11}
S: + Ready for additional command text
C: FRED FOOBAR {7}
S: + Ready for additional command text
C: fat man
S: A001 OK LOGIN completed

```



RFC 7888 non-synchronizing literals:



```
C: A001 LOGIN {11+}
C: FRED FOOBAR {7+}
C: fat man
S: A001 OK LOGIN completed

```



The so-called `LITERAL+` / `LITERAL-` support can be enabled either using a `CAPABILITY` response from the server or alternatively by explicitly setting encoding options.

Integration with SwiftNIO 
  
 

SwiftNIO IMAP provides a pair of `ChannelHandler` objects that can be integrated into a SwiftNIO `ChannelPipeline`. This allows sending IMAP commands using NIO `Channel`s.

The two handlers SwiftNIO IMAP provides are `IMAPClientHandler` and `IMAPServerHandler`. Each of these can be inserted into the `ChannelPipeline`. They can then be used to encode and decode messages. For example:



```
let group = MultiThreadedEventLoopGroup(numberOfThreads: 1)
let channel = try await ClientBootstrap(group).channelInitializer { channel in
    channel.pipeline.addHandler(IMAPClientHandler())
}.connect(host: example.com, port: 143).get()

try await channel.writeAndFlush(CommandStreamPart.tagged(TaggedCommand(tag: "a001", command: .login(username: "mrc", password: "secret"))), promise: nil)

```



The `ChannelHandler`s support transparent literals, IMAP capabilities, and all of the rest of the functionality of SwiftNIO IMAP. They’re powerful building blocks for IMAP applications on both the server and client.

What’s Next 
  
 

The version of SwiftNIO IMAP we’re releasing today is still a prototype. We want to solicit feedback from the community.

We’ve done extensive testing of the project’s code and we believe that it’s close to being “production ready”. But we would love to have design discussions on the Swift forums about what is missing, and which bits and pieces could be improved.

Get Involved 
  
 

Your feedback, experience, and contributions are very welcome!

 
- Get started by trying out [the SwiftNIO IMAP library on GitHub](https://github.com/apple/swift-nio-imap/).

 
- Discuss the library, suggest improvements and get help in [the Swift forums](https://forums.swift.org).

 
- Open issues for problems you find.

 
- Contribute pull requests to fix any issues.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Daniel Eggert](https://github.com/danieleggert/)
 
 
 
 
 
 
 Daniel Eggert is a member of the team at Apple working on Mail for iOS and macOS.
 
 
 
 
 
 
 
 
 
 
 
 
 [Cory Benfield](https://github.com/Lukasa/)
 
 
 
 
 
 
 Cory Benfield is a member of a team developing foundational server-side Swift libraries as part of Apple‘s Cloud Services division, and is a core developer on SwiftNIO.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Server Guides Now on Swift.org

 August 18, 2022
 
 The Swift Server Workgroup has maintained a set of open source guides for Swif..
 
 [ More ](/blog/sswg-server-guides/)
 
 

 
 
- 
 
 ### Swift 5.7 Released!

 September 12, 2022
 
 Swift 5.7 is now officially released! Swift 5.7 includes major additions to th..
 
 [ More ](/blog/swift-5.7-released/)
 
 

 
 

 
 
 
 
 

 
 
 
 
 [ ](/)

 
 
 
- 
 [Docs](/documentation/)
 

 
 
- 
 [Community](/community/)
 

 
 
- 
 [Packages](/packages/)
 

 
 
- 
 [Blog](/blog/)
 

 
 
- 
 [Install](/install/)
 

 
 

 
 ### Tools

 
 
 
- 
 [Xcode](https://developer.apple.com/xcode/)
 

 
 
- 
 [Visual Studio Code](/documentation/articles/getting-started-with-vscode-swift.html)
 

 
 
- 
 [Emacs](/documentation/articles/zero-to-swift-emacs.html)
 

 
 
- 
 [Neovim](/documentation/articles/zero-to-swift-nvim.html)
 

 
 
- 
 [Other Editors](https://github.com/swiftlang/sourcekit-lsp/tree/main/Documentation/Editor%20Integration.md)
 

 
 

 
 ### Community

 
 
 
- 
 [Overview](/community/)
 

 
 
- 
 [Swift Evolution](/swift-evolution/)
 

 
 
- 
 [Diversity](/diversity/)
 

 
 
- 
 [Mentorship](/mentorship/)
 

 
 
- 
 [Contributing](/contributing/)
 

 
 

 
 ### Governance

 
 
 
- 
 [Code of Conduct](/code-of-conduct/)
 

 
 
- 
 [License](/legal/license.html)
 

 
 
- 
 [Security](/support/security.html)
 

 
 

 
 Color scheme preference
 
 
 Light
 
 
 
 Dark
 
 
 
 Auto
 

 

 
 
 
 
 
 Copyright © 2025 Apple Inc. All rights reserved.
 
 
 Swift and the Swift logo are trademarks of Apple Inc.
 
 

 
 
 
 
- 
 [Privacy Policy](//www.apple.com/privacy/privacy-policy/)
 

 
 
- 
 [Cookies](//www.apple.com/legal/privacy/en-ww/cookies/)
 

 
 
- 
 [API](/openapi)
 

 
 

 

 
 
 
 
- 
 [*](https://x.com/swiftlang)
 

 
 
- 
 [**](https://bsky.app/profile/swift.org)
 

 
 
- 
 [**](https://mastodon.social/@swiftlang)
 

 
 
- 
 [**](/atom.xml)