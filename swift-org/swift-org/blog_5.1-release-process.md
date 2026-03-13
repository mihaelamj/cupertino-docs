---
source: https://www.swift.org/blog/5.1-release-process/
crawled: 2025-11-15T21:27:23Z
---

# Swift 5.1 Release Process

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
 

 
 
 
 

 
 # Swift 5.1 Release Process

 
 
 
 
 
 
 
 
 *
 
 
 [Ted Kremenek](https://github.com/tkremenek/)
 
 
 
 
 
 
 

 February 18, 2019
 
 
 This post describes the goals, release process, and estimated schedule for **Swift 5.1**.

Motivation and Goals 
  
 

The primary goal of Swift 5.1 is for the language to achieve
[module stability](https://forums.swift.org/t/plan-for-module-stability/14551).

Binary Compatibility 
  
 

On Apple platforms, since the ABI is now stabilized, Swift 5.1 is binary
compatible with Swift 5.0, and is binary compatible with future releases
of Swift.

On non-Apple platforms (such as Linux) the ABI is not yet fully stable
in order to allow for more vetting. Such vetting will be particularly
needed for the new platforms based on Linux.

Source Compatibility 
  
 

As with Swift 5.0, we expect that majority of sources that built with
the Swift 5.0 compiler will compile with the Swift 5.1 compiler.
However, it is possible that a bug fix in Swift 5.1 may cause it to
detect errors in code that were not detected before.

Snapshots of Swift 5.1 
  
 

Downloadable snapshots of the Swift 5.1 release branch will be posted
regularly as part of [continuous integration](https://ci.swift.org) testing.

Once Swift 5.1 is released, the official final builds will also be posted in
addition to the snapshots.

Getting Changes into Swift 5.1 
  
 

The development of Swift 5.0 required an unusual amount of focus and
attention throughout the time it converged because every issue had to be
evaluated for its permanent ABI impact. Consequently, Swift 5.1 has a
significantly shorter development window than previous releases. This
tighter time constraint is needed to ensure delivering a mature and
stable 5.1 release, with stricter cutoff dates for disruptive changes.

The `swift-5.1-branch` contains the changes that will be released in Swift
5.1. The branch will be managed as follows:

 
- 
 The `swift-5.1-branch` has already been initially cut from `master`.

 

 
- 
 Periodically, the `master` development branch will be merged into
`swift-5.1-branch` until the final branch date.
 

 
- 
 **March 18, 2019 (final branching)**: The `swift-5.1-branch` will have
changes merged from `master` one last time. After the final branch date
there will be a “bake” period in which only select, critical fixes will go
into the release (via pull requests).
 

Some notable exceptions to this plan are indicated in the table below.
Each will merge from `master` into `swift-5.1-branch` daily. The final
cutoff date for changes to each exception will extend beyond March 18
and will be announced later.

 
 
 Project
 Cutoff date
 
 
 
 
 [indexstore-db](https://github.com/swiftlang/indexstore-db)
 To be announced
 
 
 [sourcekit-lsp](https://github.com/swiftlang/sourcekit-lsp)
 To be announced
 
 
 [swift-corelibs-foundation](https://github.com/swiftlang/swift-corelibs-foundation)
 To be announced
 
 
 [swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch)
 To be announced
 
 
 [swift-corelibs-xctest](https://github.com/swiftlang/swift-corelibs-xctest)
 To be announced
 
 
 [swift-llbuild](https://github.com/swiftlang/swift-llbuild)
 April 10, 2019
 
 
 [swift-package-manager](https://github.com/swiftlang/swift-package-manager)
 April 10, 2019
 
 
 [swift-stress-tester](https://github.com/swiftlang/swift-stress-tester)
 To be announed
 
 
 [swift](https://github.com/apple/swift)
 March 18, 2019
 
 

Philosophy on Taking Changes into Swift 5.1 
  
 

 
- 
 All language and API changes for Swift 5.1 will go through the Swift
Evolution process. Evolution
proposals should aim to be completed by the branch date in order to
increase their chances of impacting the Swift 5.1 release. Exceptions
will be considered on a case-by-case basis, particularly if they tie
in with the core goal of the release.
 

 
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
  
 

The following repositories will have a `swift-5.1-branch` branch to track
sources as part of Swift 5.1 release:

 
- [indexstore-db](https://github.com/swiftlang/indexstore-db)

 
- [sourcekit-lsp](https://github.com/swiftlang/sourcekit-lsp)

 
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

 
- [swift-stress-tester](https://github.com/swiftlang/swift-stress-tester)

 
- [swift-syntax](https://github.com/swiftlang/swift-syntax)

 
- [swift-xcode-playground-support](https://github.com/apple/swift-xcode-playground-support)

Release Managers 
  
 

The overall management of the release will be overseen by the following
individuals, who will announce when stricter control of change goes into
effect for the Swift 5.1 release as the release converges:

 
- 
 [Ted Kremenek](https://github.com/tkremenek) is the overall release manager for Swift 5.1.

 

 
- 
 [Duncan Exon Smith](https://github.com/dexonsmith) is the release manager for
[swift-llvm](https://github.com/apple/swift-llvm), [swift-clang](https://github.com/apple/swift-clang), [swift-compiler-rt](https://github.com/apple/swift-compiler-rt), [swift-clang-tools-extra](https://github.com/apple/swift-clang-tools-extra), and [swift-libcxx](https://github.com/apple/swift-libcxx).
 

 
- 
 [Fred Riss](https://github.com/orgs/apple/people/fredriss) is the release manager for [swift-lldb](https://github.com/apple/swift-lldb).

 

 
- 
 [Ben Cohen](https://github.com/airspeedswift) is the release manager for the
Swift Standard Library.
 

 
- 
 [Tony Parker](https://github.com/parkera) is the release manager for
[swift-corelibs-foundation](https://github.com/swiftlang/swift-corelibs-foundation).
 

 
- 
 [Pierre Habouzit](https://github.com/MadCoder) is the release manager for
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
 

 
- 
 [Argyrios Kyrtzidis](https://github.com/akyrtzi) is the release manager for [sourcekit-lsp](https://github.com/swiftlang/sourcekit-lsp), [indexstore-db](https://github.com/swiftlang/indexstore-db), and [swift-stress-tester](https://github.com/swiftlang/swift-stress-tester).

 

Please feel free to post on the [development forum](https://forums.swift.org/c/development/compiler)
or contact [Ted Kremenek](https://github.com/tkremenek) directly concerning any questions about the release management
process.

Pull Requests for Release Branch 
  
 

In order for a pull request to be considered for inclusion in the release
branch after the final re-branch from `master` it must include the following
information:

 
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
 

**All change** going into the `swift-5.1-branch` (outside changes being merged
in automatically from `master`) **must go through pull requests** that are
accepted by the corresponding release manager.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Ted Kremenek](https://github.com/tkremenek/)
 
 
 
 
 
 
 Ted Kremenek is a member of the Swift Core Team and manages the Languages and Runtimes group at Apple.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Evolving Swift On Apple Platforms After ABI Stability

 February 11, 2019
 
 With the release of Swift 5.0, Swift is now ABI stable and is delivered as a c..
 
 [ More ](/blog/abi-stability-and-apple/)
 
 

 
 
- 
 
 ### Behind the Proposal — SE-0200 Enhancing String Literals Delimiters to Support Raw Text

 February 20, 2019
 
 The development, refinement, and deployment of SE-0200 Enhancing String Litera..
 
 [ More ](/blog/behind-SE-0200/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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