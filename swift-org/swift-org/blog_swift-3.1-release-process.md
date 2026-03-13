---
source: https://www.swift.org/blog/swift-3.1-release-process/
crawled: 2025-11-15T21:30:05Z
---

# Swift 3.1 Release Process

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
 

 
 
 
 

 
 # Swift 3.1 Release Process

 
 
 
 
 
 
 
 
 *
 
 
 [Ted Kremenek](https://github.com/tkremenek/)
 
 
 
 
 
 
 

 December 9, 2016
 
 
 This post describes the goals, release process, and estimated schedule for Swift 3.1.

Swift 3.1 is intended to be [source compatible](#source-compatibility) with Swift 3.0.
It will contain a few additive enhancements to the core language as well as improvements to the Swift Package Manager, Swift on Linux, and general quality improvements to the compiler and Standard Library.

Swift 3.1 is intended to be released in the spring of 2017.

Source Compatibility 
  
 

It is a strong goal that the vast majority of sources that built with the Swift 3.0 compiler continue to build with the Swift 3.1 compiler. The exception will be bug fixes to the compiler that cause it to reject code that should never have been accepted in the first place. These cases should be relatively rare in practice.

A description of the intent for source compatibility for Swift releases can be found on a [thread](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20161128/029099.html) on the [swift-evolution](https://lists.swift.org/pipermail/swift-evolution/) mailing list.

Please file [bug reports](https://bugs.swift.org) if you encounter cases where the Swift 3.1 compiler unexpectedly rejects code that previously compiled with the Swift 3.0 compiler.

Snapshots of Swift 3.1 
  
 

Previous releases of Swift have had “Developer Previews”, e.g. “Preview 1”, “Preview 2”, etc., that represent stabilized snapshots of a Swift release as it converges. Developer previews have often been irregularly spaced apart, and have sometimes not provided enough granularity for the Swift community to try out new features or verify bug fixes in a release as it converges.

For Swift 3.1 there will instead be daily downloadable snapshots of the release branch. Snapshots will be produced as part of [continuous integration](https://ci.swift.org) testing. The cadence of downloadable snapshots will thus be more frequent and granular. Snapshots will be posted daily, assuming tests are passing.

Once Swift 3.1 is released, official final builds will also be posted in addition to the snapshots.

Getting Changes into Swift 3.1 
  
 

Swift 3.1 is intended to be limited in scope, with the desire to move focus early in 2017 to the development of Swift 4. To meet this goal, Swift 3.1 will include changes in mainline development (i.e. the `master` branch) only until January 16. After that date there will be a “bake” period in which only select, critical fixes will go into the `swift-3.1-branch` and move `master` on to Swift 4 development.

Branches 
  
 

 
- 
 **master**: With the exception of the [swift-llvm](https://github.com/apple/swift-llvm) and [swift-clang](https://github.com/apple/swift-clang) repositories (see [Impacted Repositories](#impacted-repositories)), development of Swift 3.1 happens in `master`. All changes going in `master` will be part of the final Swift 3.1 release until January 16. At that point `master` tracks development for Swift 4.

 

 
- 
 **swift-3.1-branch**: Release management for Swift 3.1 happens on the `swift-3.1-branch`. All Swift 3.1 snapshots are built from this branch, and Swift 3.1 will GM from this branch as well.

 

Operationally, `master` will be regularly merged into `swift-3.1-branch` approximately every two weeks until January 16. The two week window provides a buffer between hot development on `master` and a curated release branch. Changes may be cherry-picked (via pull requests) into `swift-3.1-branch` between merges of `master`.

A notable exception to this plan is the [swift-package-manager](https://github.com/swiftlang/swift-package-manager), which will merge from `master` into the `swift-3.1-branch` daily.

Philosophy on Taking Changes into Swift 3.1 
  
 

 
- 
 Source compatibility with Swift 3.0 is a top priority.

 

 
- 
 As Swift 3.1 converges only changes that align with the core goals of the release will be considered.

 

 
- 
 All language and API changes for Swift 3.1 will go through the [Swift Evolution](https://github.com/swiftlang/swift-evolution) process.

 

 
- 
 Major work for Swift 3.1 should orient around the January 16 date, but changes can still land in 3.1 afterwards per the judgement of the release manager. As the release converges, the criteria for pulling changes into 3.1 will become increasingly restrictive.

 

Impacted Repositories 
  
 

The following repositories will have a `swift-3.1-branch` branch to track sources as part of Swift 3.1 release:

 
- [swift](https://github.com/apple/swift)

 
- [swift-lldb](https://github.com/apple/swift-lldb)

 
- [swift-cmark](https://github.com/swiftlang/swift-cmark)

 
- [swift-llbuild](https://github.com/swiftlang/swift-llbuild)

 
- [swift-package-manager](https://github.com/swiftlang/swift-package-manager)

 
- [swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch)

 
- [swift-corelibs-foundation](https://github.com/swiftlang/swift-corelibs-foundation)

 
- [swift-corelibs-xctest](https://github.com/swiftlang/swift-corelibs-xctest)

 
- [swift-llvm](https://github.com/apple/swift-llvm)

 
- [swift-clang](https://github.com/apple/swift-clang)

Note that the [swift-llvm](https://github.com/apple/swift-llvm) and [swift-clang](https://github.com/apple/swift-clang) repositories have already branched `swift-3.1-branch` from `master` and will not rebranch again.

Release Managers 
  
 

The overall management of the release will be overseen by the following individuals, who will announce when stricter control of change goes into effect for the Swift 3.1 release as the release converges:

 
- 
 [Ted Kremenek](https://github.com/tkremenek) is the overall release manager for Swift 3.1.

 

 
- 
 [Frédéric Riss](https://github.com/fredriss)
is the release manager for [swift-llvm](https://github.com/apple/swift-llvm) and [swift-clang](https://github.com/apple/swift-clang).
 

 
- 
 [Jason Gosnell](https://github.com/gosnellj) is the
release manager for [swift-lldb](https://github.com/apple/swift-lldb).
 

 
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
 

Please feel free to email [swift-dev](https://lists.swift.org/pipermail/swift-dev/) or [Ted Kremenek](https://github.com/tkremenek) directly concerning any
questions about the release management process.

 Note: Swift mailing lists have been shut down, archived, and replaced with
[Swift Forums](https://forums.swift.org). See
[the announcement here](/blog/forums/).

Pull Requests for Release Branch 
  
 

All pull requests nominating changes for inclusion in the release branch
should include the following information:

 
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
 

One or more [code owners](/community/#code-owners) for the impacted
components should review the change. Technical review can be delegated
by a code owner or otherwise requested as deemed appropriate or
useful.

**All change** going into the `swift-3.1-branch` (outside changes being merged in automatically from `master`) **must go through pull requests** that are accepted by the corresponding release manager.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Ted Kremenek](https://github.com/tkremenek/)
 
 
 
 
 
 
 Ted Kremenek is a member of the Swift Core Team and manages the Languages and Runtimes group at Apple.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Server APIs Work Group

 October 25, 2016
 
 Since Swift became available on Linux there has been a huge amount of interest..
 
 [ More ](/blog/server-api-workgroup/)
 
 

 
 
- 
 
 ### Swift Evolution Status Page Now Available

 January 18, 2017
 
 We’re pleased to announce the release of the new Swift Evolution status page a..
 
 [ More ](/blog/swift-evolution-status-page/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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