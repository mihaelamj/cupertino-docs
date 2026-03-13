---
source: https://www.swift.org/blog/swift-2.2-released/
crawled: 2025-11-15T21:30:57Z
---

# Swift 2.2 Released!

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
 

 
 
 
 

 
 # Swift 2.2 Released!

 
 
 
 
 
 
 
 
 *
 
 
 [Ted Kremenek](https://github.com/tkremenek/)
 
 
 
 
 
 
 

 March 21, 2016
 
 
 We are very pleased to announce the release of Swift 2.2! This is the first official release of Swift since it was open-sourced on December 3, 2015. Notably, the release includes contributions from 212 non-Apple contributors — changes that span from simple bug fixes to enhancements and alterations to the core language and Swift Standard Library.

Language Changes 
  
 

Swift 2.2 is a minor language release that is mostly source-compatible with Swift 2.1. It contains the following language changes that went through the Swift’s [evolution process](/contributing/#participating-in-the-swift-evolution-process):

 
- [SE-0001: Allow (most) keywords as argument labels](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0001-keywords-as-argument-labels.md)

 
- [SE-0015: Tuple comparison operators](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0015-tuple-comparison-operators.md)

 
- [SE-0014: Constraining `AnySequence.init`](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0014-constrained-AnySequence.md)

 
- [SE-0011: Replace `typealias` keyword with `associatedtype` for associated type declarations](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0011-replace-typealias-associated.md)

 
- [SE-0021: Naming Functions with Argument Labels](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0021-generalized-naming.md)

 
- [SE-0022: Referencing the Objective-C selector of a method](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0022-objc-selectors.md)

 
- [SE-0020: Swift Language Version Build Configuration](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0020-if-swift-version.md)

Beyond these language changes Swift 2.2 also contains numerous bug fixes, enhancements to diagnostics, and produces even faster-running code.

The [Swift Package Manager](/documentation/package-manager/) is still early in development and is not included in this release.

Documentation 
  
 

An updated version of [The Swift Programming Language](/documentation/tspl) for Swift 2.2 is now available on Swift.org. It is also available for free on Apple’s iBooks store.

Platforms 
  
 

Linux (Ubuntu 14.04 and Ubuntu 15.10) 
  
 

Swift 2.2 includes support for Swift on Linux. The Linux port is still relatively new and in this release does not include the [Swift Core Libraries](/documentation/core-libraries/) (which will appear in Swift 3). The port does, however, include LLDB and the REPL.

Official binaries for Ubuntu 14.04 and Ubuntu 15.10 are [available for download](/download/).

Apple (Xcode) 
  
 

For development on Apple’s platforms, Swift 2.2 ships as part of [Xcode 7.3](https://developer.apple.com/xcode/download/).

Sources 
  
 

Development on Swift 2.2 was tracked in the `swift-2.2-branch` on the following repositories on GitHub:

 
- [swift](https://github.com/apple/swift)

 
- [swift-llvm](https://github.com/apple/swift-llvm)

 
- [swift-clang](https://github.com/apple/swift-clang)

 
- [swift-lldb](https://github.com/apple/swift-lldb)

 
- [swift-cmark](https://github.com/swiftlang/swift-cmark)

The tag `swift-2.2-RELEASE` designates the specific revisions in those repositories that make up the final version of Swift 2.2.

The `swift-2.2-branch` will remain open — but under the same [release management process](/blog/swift-2-2-release-process/) — to accumulate changes for a potential future bug-fix “dot” release.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Ted Kremenek](https://github.com/tkremenek/)
 
 
 
 
 
 
 Ted Kremenek is a member of the Swift Core Team and manages the Languages and Runtimes group at Apple.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Expanding Commit Access

 February 29, 2016
 
 Now that the Swift Continuous Integration system is established and proven, we..
 
 [ More ](/blog/swift-commit-access/)
 
 

 
 
- 
 
 ### New Features in Swift 2.2

 March 30, 2016
 
 Swift 2.2 brings new syntax, new features, and some deprecations too. It is a..
 
 [ More ](/blog/swift-2.2-new-features/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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