---
source: https://www.swift.org/blog/swift-4.1-release-process/
crawled: 2025-11-15T21:29:01Z
---

# Swift 4.1 Release Process

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
 

 
 
 
 

 
 # Swift 4.1 Release Process

 
 
 
 
 
 
 
 
 *
 
 
 [Ted Kremenek](https://github.com/tkremenek/)
 
 
 
 
 
 
 

 October 17, 2017
 
 
 This post describes the goals, release process, and estimated schedule for Swift 4.1.

Swift 4.1 is a source compatible update to Swift 4.0. It will contain a few additive enhancements to the core language as well as improvements to the Swift Package Manager, Swift on Linux, and general quality improvements to the compiler and Standard Library.

Swift 4.1 is not binary compatible with 4.0. It contains a variety of under-the-hood changes that are part of the effort to [stabilize the Swift ABI](/abi-stability/) in Swift 5.

Swift 4.1 is intended to be released in the first half of 2018.

Source Compatibility 
  
 

The vast majority of sources that built with the Swift 4.0 compiler (including those using the Swift 3 compatibility mode) should compile with the Swift 4.1 compiler. There will be some exceptional cases where this cannot be an absolute guarantee. This includes fixes to incorrect behavior in the compiler or corner cases with the uses of generics now addressed by the introduction of long-anticipated generics features. The expectation, however, is that most projects will continue to build with no source changes.

Snapshots of Swift 4.1 
  
 

Downloadable snapshots of the Swift 4.1 release branch will be posted regularly as part of [continuous integration](https://ci.swift.org) testing.

Once Swift 4.1 is released the official final builds will also be posted in addition to the snapshots.

Getting Changes into Swift 4.1 
  
 

The `swift-4.1-branch` contains the changes that will be released in Swift 4.1. The branch will be managed as follows:

 
- **October 18, 2017 (initial branching)**: The `swift-4.1-branch` will be initially cut from `master`.

 
- Approximately every two weeks, `master` will be merged into `swift-4.1-branch` until the final branch date.

 
- **December 4, 2017 (final branching)**: The `swift-4.1-branch` will have changes merged from `master` one last time. After the final branch date there will be a “bake” period in which only select, critical fixes will go into the release (via pull requests).

Four notable exceptions to this plan are [swift-package-manager](https://github.com/swiftlang/swift-package-manager), [swift-llbuild](https://github.com/swiftlang/swift-llbuild), [swift-corelibs-foundation](https://github.com/swiftlang/swift-corelibs-foundation), and [swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch) which will merge from `master` into `swift-4.1-branch` daily and whose final cutoff date for changes will extend beyond December 4 and will be announced later.

Philosophy on Taking Changes into Swift 4.1 
  
 

 
- 
 All language and API changes for Swift 4.1 will go through the [Swift Evolution](https://github.com/swiftlang/swift-evolution) process, with criteria for what changes are in scope for the release documented there.

 

 
- 
 Other changes (e.g., bug fixes, diagnostic improvements, SourceKit interface improvements) will be accepted based on their risk and impact.

 

 
- 
 Low-risk test tweaks will also be accepted late into the release branch if it aids in the qualification of the release.

 

 
- 
 As the release converges, the criteria for accepted changes will become increasingly restrictive.

 

Impacted Repositories 
  
 

The following repositories will have a `swift-4.1-branch` branch to track sources as part of Swift 4.1 release:

 
- [swift](https://github.com/apple/swift)

 
- [swift-clang](https://github.com/apple/swift-clang)

 
- [swift-cmark](https://github.com/swiftlang/swift-cmark)

 
- [swift-compiler-rt](https://github.com/apple/swift-compiler-rt)

 
- [swift-corelibs-foundation](https://github.com/swiftlang/swift-corelibs-foundation)

 
- [swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch)

 
- [swift-corelibs-xctest](https://github.com/swiftlang/swift-corelibs-xctest)

 
- [swift-integration-tests](https://github.com/swiftlang/swift-integration-tests)

 
- [swift-llbuild](https://github.com/swiftlang/swift-llbuild)

 
- [swift-lldb](https://github.com/apple/swift-lldb)

 
- [swift-llvm](https://github.com/apple/swift-llvm)

 
- [swift-package-manager](https://github.com/swiftlang/swift-package-manager)

 
- [swift-xcode-playground-support](https://github.com/apple/swift-xcode-playground-support)

The [swift-llvm](https://github.com/apple/swift-llvm), [swift-clang](https://github.com/apple/swift-clang), [swift-compiler-rt](https://github.com/apple/swift-compiler-rt), and [swift-lldb](https://github.com/apple/swift-lldb) repositories have already branched `swift-4.1-branch` from `master` and will not rebranch again.

Release Managers 
  
 

The overall management of the release will be overseen by the following individuals, who will announce when stricter control of change goes into effect for the Swift 4 release as the release converges:

 
- 
 [Ted Kremenek](https://github.com/tkremenek) is the overall release manager for Swift 4.1.

 

 
- 
 [Frédéric Riss](https://github.com/fredriss)
is the release manager for [swift-llvm](https://github.com/apple/swift-llvm), [swift-clang](https://github.com/apple/swift-clang), and [swift-compiler-rt](https://github.com/apple/swift-compiler-rt).
 

 
- 
 [Ben Cohen](https://github.com/airspeedswift) is the release manager for the Swift Standard Library.

 

 
- 
 [Tony Parker](https://github.com/parkera) is the release
manager for [swift-corelibs-foundation](https://github.com/swiftlang/swift-corelibs-foundation).
 

 
- 
 [Daniel Steffen](https://github.com/das) is the release
manager for [swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch).
 

 
- 
 [Brian Croom](https://github.com/briancroom) is the
release manager for [swift-corelibs-xctest](https://github.com/swiftlang/swift-corelibs-xctest).
 

 
- 
 [Rick Ballard](https://github.com/rballard) is the release
manager for [swift-package-manager](https://github.com/swiftlang/swift-package-manager).
 

 
- 
 [Daniel Dunbar](https://github.com/ddunbar) is the release
manager for [swift-llbuild](https://github.com/swiftlang/swift-llbuild).
 

Please feel free to email [swift-dev](https://lists.swift.org/pipermail/swift-dev/) or [Ted Kremenek](https://github.com/tkremenek) directly concerning any
questions about the release management process.

 Note: Swift mailing lists have been shut down, archived, and replaced with
[Swift Forums](https://forums.swift.org). See
[the announcement here](/blog/forums/).

Pull Requests for Release Branch 
  
 

In order for a pull request to be considered for inclusion in the release branch it must include the following information:

 
- 
 **Explanation**: A description of the issue being fixed or
enhancement being made. This can be brief, but it should be
clear.
 

 
- 
 **Scope**: An assessment of the impact/importance of the change.
For example, is the change a source-breaking language change, etc.
 

 
- 
 **SR Issue**: The SR if the change fixes/implements an
issue/enhancement on [bugs.swift.org](https://bugs.swift.org).
 

 
- 
 **Risk**: What is the (specific) risk to the release for taking this
change?
 

 
- 
 **Testing**: What specific testing has been done or needs to be done
to further validate any impact of this change?
 

 
- 
 **Reviewer**: One or more [code owners](/community/#code-owners) for the impacted components should review the change. Technical review can be delegated by a code owner or otherwise requested as deemed appropriate or
useful.
 

**All change** going into the `swift-4.1-branch` (outside changes being merged in automatically from `master`) **must go through pull requests** that are accepted by the corresponding release manager.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Ted Kremenek](https://github.com/tkremenek/)
 
 
 
 
 
 
 Ted Kremenek is a member of the Swift Core Team and manages the Languages and Runtimes group at Apple.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Xcode 9.1 Improves Display of Fatal Errors

 October 5, 2017
 
 Swift has language constructs that allow you to specify your program’s expecta..
 
 [ More ](/blog/xcode-9.1-improves-display-of-fatal-errors/)
 
 

 
 
- 
 
 ### Conditional Conformance in the Standard Library

 January 8, 2018
 
 The Swift 4.1 compiler brings the next phase of improvements from the
roadmap ..
 
 [ More ](/blog/conditional-conformance/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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