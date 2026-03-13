---
source: https://www.swift.org/blog/4.2-release-process/
crawled: 2025-11-15T21:28:38Z
---

# Swift 4.2 Release Process

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
 

 
 
 
 

 
 # Swift 4.2 Release Process

 
 
 
 
 
 
 
 
 *
 
 
 [Ted Kremenek](https://github.com/tkremenek/)
 
 
 
 
 
 
 

 February 28, 2018
 
 
 This post describes the goals, release process, and estimated schedule for
**Swift 4.2**.

Motivation and Goals 
  
 

Swift 4.2 is meant to be a waypoint towards achieving ABI stability in Swift
5.

Swift 4.2 will include numerous under-the-hood ABI changes as part of the
effort to [stabilize the Swift ABI](/abi-stability/). It is
valuable to incrementally roll out ABI changes — many of which are performance
related — to provide ample time for user feedback in assessing these changes
before they are locked into the final ABI.

Swift 4.2 will also include numerous bug fixes, as well as have a goal of some
focused improvements on compile-time performance.

Binary Compatibility 
  
 

Swift 4.2 is not binary compatible with previous Swift releases.

Source Compatibility 
  
 

As with Swift 4.1, the vast majority of sources that built with the Swift 4.0
compiler (including those using the Swift 3 compatibility mode) should compile
with the Swift 4.2 compiler.

There will be some exceptional cases where this cannot be an absolute
guarantee. This includes fixes to incorrect behavior in the compiler or
corner cases with the uses of generics now addressed by the introduction of
long-anticipated generics features. The expectation, however, is that most
projects will continue to build with no source changes.

Snapshots of Swift 4.2 
  
 

Downloadable snapshots of the Swift 4.2 release branch will be posted
regularly as part of [continuous integration](https://ci.swift.org) testing.

Once Swift 4.2 is released, the official final builds will also be posted in
addition to the snapshots.

Getting Changes into Swift 4.2 
  
 

The `swift-4.2-branch` contains the changes that will be released in Swift
4.2. The branch will be managed as follows:

 
- **Imminently**: The `swift-4.2-branch` will be initially cut from `master`.

 
- Approximately every two weeks, `master` will be merged into
`swift-4.2-branch` until the final branch date.

 
- **April 20, 2018 (final branching)**: The `swift-4.2-branch` will have
changes merged from `master` one last time. After the final branch date
there will be a “bake” period in which only select, critical fixes will go
into the release (via pull requests).

Four notable exceptions to this plan are [swift-package-manager](https://github.com/swiftlang/swift-package-manager), [swift-llbuild](https://github.com/swiftlang/swift-llbuild),
[swift-corelibs-foundation](https://github.com/swiftlang/swift-corelibs-foundation), and [swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch) which
will merge from `master` into `swift-4.2-branch` daily and whose final cutoff
date for changes will extend beyond April 20 and will be announced later.

 
 
 Project
 Cutoff date
 
 
 
 
 [swift](https://github.com/apple/swift)
 April 20, 2018
 
 
 [swift-package-manager](https://github.com/swiftlang/swift-package-manager)
 June 28, 2018
 
 
 [swift-llbuild](https://github.com/swiftlang/swift-llbuild)
 July 05, 2018
 
 

Philosophy on Taking Changes into Swift 4.2 
  
 

 
- 
 All language and API changes for Swift 4.2 will go through the Swift
Evolution process, with criteria
for what changes are in scope for the release documented there.
 

 
- 
 Other changes (e.g., bug fixes, diagnostic improvements, SourceKit interface
improvements) will be accepted based on their risk and impact.
 

 
- 
 Low-risk test tweaks will also be accepted late into the release branch if
it aids in the qualification of the release.
 

 
- 
 As the release converges, the criteria for accepted changes will become
increasingly restrictive.
 

Impacted Repositories 
  
 

The following repositories will have a `swift-4.2-branch` branch to track
sources as part of Swift 4.2 release:

 
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

Release Managers 
  
 

The overall management of the release will be overseen by the following
individuals, who will announce when stricter control of change goes into
effect for the Swift 4 release as the release converges:

 
- 
 [Ted Kremenek](https://github.com/tkremenek) is the overall release manager for Swift 4.2.

 

 
- 
 [Duncan Exon Smith](https://github.com/dexonsmith) is the release manager for
[swift-llvm](https://github.com/apple/swift-llvm), [swift-clang](https://github.com/apple/swift-clang), and [swift-compiler-rt](https://github.com/apple/swift-compiler-rt).
 

 
- 
 [Ben Cohen](https://github.com/airspeedswift) is the release manager for the
Swift Standard Library.
 

 
- 
 [Tony Parker](https://github.com/parkera) is the release manager for
[swift-corelibs-foundation](https://github.com/swiftlang/swift-corelibs-foundation).
 

 
- 
 [Daniel Steffen](https://github.com/das) is the release manager for
[swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch).
 

 
- 
 [Brian Croom](https://github.com/briancroom) is the release manager for
[swift-corelibs-xctest](https://github.com/swiftlang/swift-corelibs-xctest).
 

 
- 
 [Rick Ballard](https://github.com/rballard) is the release manager for
[swift-package-manager](https://github.com/swiftlang/swift-package-manager).
 

 
- 
 [Daniel Dunbar](https://github.com/ddunbar) is the release manager for
[swift-llbuild](https://github.com/swiftlang/swift-llbuild).
 

Please feel free to post on the [development forum](https://forums.swift.org/c/development/compiler)
or contact [Ted Kremenek](https://github.com/tkremenek) directly concerning any questions about the release management
process.

Pull Requests for Release Branch 
  
 

In order for a pull request to be considered for inclusion in the release
branch it must include the following information:

 
- 
 **Explanation**: A description of the issue being fixed or enhancement being
made. This can be brief, but it should be clear.
 

 
- 
 **Scope**: An assessment of the impact/importance of the change. For
example, is the change a source-breaking language change, etc.
 

 
- 
 **SR Issue**: The SR if the change fixes/implements an issue/enhancement on
[bugs.swift.org](https://bugs.swift.org).
 

 
- 
 **Risk**: What is the (specific) risk to the release for taking this change?

 

 
- 
 **Testing**: What specific testing has been done or needs to be done to
further validate any impact of this change?
 

 
- 
 **Reviewer**: One or more [code owners](/community/#code-owners)
for the impacted components should review the change. Technical review can
be delegated by a code owner or otherwise requested as deemed appropriate or
useful.
 

**All change** going into the `swift-4.2-branch` (outside changes being merged
in automatically from `master`) **must go through pull requests** that are
accepted by the corresponding release manager.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Ted Kremenek](https://github.com/tkremenek/)
 
 
 
 
 
 
 Ted Kremenek is a member of the Swift Core Team and manages the Languages and Runtimes group at Apple.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Code Size Optimization Mode in Swift 4.1

 February 8, 2018
 
 In Swift 4.1 the compiler now supports a new optimization mode which enables d..
 
 [ More ](/blog/osize/)
 
 

 
 
- 
 
 ### Swift 4.1 Released!

 March 29, 2018
 
 Swift 4.1 is now officially released! It contains updates to the core languag..
 
 [ More ](/blog/swift-4.1-released/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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