---
source: https://www.swift.org/blog/swift-3-api-design/
crawled: 2025-11-15T21:31:31Z
---

# Swift 3 API Design Guidelines

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
 

 
 
 
 

 
 # Swift 3 API Design Guidelines

 
 
 
 

 December 3, 2015
 
 
 The design of commonly-used libraries has a large impact on the
overall feel of a programming language. Great libraries feel like an
extension of the language itself, and consistency across libraries
elevates the overall development experience. To aid in the
construction of great Swift libraries, one of the major goals for
Swift 3 is to define a set of API design guidelines
and to apply those design guidelines consistently.

The effort to define the Swift API Design Guidelines involves several
 major pieces that, together, are intended to provide a more cohesive
 feel to Swift development. Those major pieces are:

 
- 
 **Swift API Design Guidelines**: The actual API design guidelines
are under active development. The latest draft of Swift API
Design Guidelines is available.
 

 
- 
 **Swift Standard Library**: The entire Swift standard library is
being reviewed and updated to follow the Swift API design
guidelines. The actual work is being performed on the
[swift-3-api-guidelines branch](https://github.com/apple/swift/tree/swift-3-api-guidelines) of the Swift
repository.
 

 
- 
 **Imported Objective-C APIs**: The translation of Objective-C APIs
into Swift is being updated to make Objective-C APIs better match
the Swift API design guidelines, using a variety of heuristics. The
[Better Translation of Objective-C APIs into Swift](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0005-objective-c-name-translation.md)
proposal describes how this transformation
is done. Because this approach naturally involves a number of
heuristics, we track its effects on the Cocoa and Cocoa Touch
frameworks, as well as Swift code using those frameworks. The Swift
3 API Design Guidelines Review
repository provides a way to see how
this automatic translation affects Swift code that uses Cocoa and
Cocoa Touch. Specific Objective-C APIs that translate poorly into
Swift will then be annotated (for example, with `NS_SWIFT_NAME`) to improve
the resulting Swift code. While this change primarily impacts Apple
platforms (where Swift uses the Objective-C runtime), it also has a
direct impact on the cross-platform Swift core
libraries that provide the same APIs as Objective-C
frameworks.
 

 
- 
 **Swift Guideline Checking**: Existing Swift code has been written
to follow a variety of different coding styles, including the Objective-C
Coding Guidelines for Cocoa. By leveraging
the heuristics used to import Objective-C APIs, the Swift compiler
can (optionally!) check for common API design patterns that don’t
meet the Swift API Design Guidelines and suggest improvements.
 

 
- 
 **Swift 2 to Swift 3 Migrator**: The updates to the Swift standard
library and the imported Objective-C APIs are source-breaking
changes. This effort will involve the creation of a migrator to
update Swift 2 code to use the Swift 3 APIs.
 

All of these major pieces are under active development. If you’re
interested in following along, check out the Swift API design
guidelines, the Swift standard library
changes, the Objective-C API importer
changes proposal and corresponding review
repository, then join the discussion on
the swift-evolution mailing
list.

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### The Swift Linux Port

 December 3, 2015
 
 With the launch of the open source Swift project, we are also releasing
a port..
 
 [ More ](/blog/swift-linux-port/)
 
 

 
 
- 
 
 ### Swift 2.2 Release Process

 January 5, 2016
 
 This post describes the goals, release process, and estimated schedule
for Swi..
 
 [ More ](/blog/swift-2.2-release-process/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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
 
 *
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