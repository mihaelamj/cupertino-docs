---
source: https://www.swift.org/blog/5.2-release-process/
crawled: 2025-11-15T21:26:48Z
---

# Swift 5.2 Release Process

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
 

 
 
 
 

 
 # Swift 5.2 Release Process

 
 
 
 
 
 
 
 
 *
 
 
 [Nicole Jacque](https://github.com/najacque/)
 
 
 
 
 
 
 

 September 24, 2019
 
 
 This post describes the goals, release process, and estimated schedule for **Swift 5.2**.

Motivation and Goals 
  
 

Swift 5.2 is a release meant to include significant quality and performance enhancements.

Snapshots of Swift 5.2 
  
 

Downloadable snapshots of the Swift 5.2 release branch will be posted
regularly as part of [continuous integration](https://ci.swift.org) testing.

Once Swift 5.2 is released, the official final builds will also be posted in addition to the snapshots.

Getting Changes into Swift 5.2 
  
 

On **December 9, 2019** the `swift-5.2-branch` branch will be cut, and this will contain the changes that will be released in Swift
5.2. After the branch is cut, changes can be landed on the branch via pull request if the meet the criteria for the release.

Some notable exceptions to this plan are indicated in the table below. Each will merge from `master` into `swift-5.2-branch` daily. The final
cutoff date for changes to each exception will extend beyond November 5 and will be announced later.

 
 
 Project
 Cutoff date
 
 
 
 
 [indexstore-db](https://github.com/swiftlang/indexstore-db)
 January 7, 2020
 
 
 [sourcekit-lsp](https://github.com/swiftlang/sourcekit-lsp)
 January 7, 2020
 
 
 [swift-llbuild](https://github.com/swiftlang/swift-llbuild)
 January 7, 2020
 
 
 [swift-package-manager](https://github.com/swiftlang/swift-package-manager)
 January 7, 2020
 
 

Philosophy on Taking Changes into Swift 5.2 
  
 

 
- 
 All language and API changes for Swift 5.2 will go through the Swift
Evolution process. Evolution
proposals should aim to be completed by the branch date in order to
increase their chances of impacting the Swift 5.2 release. Exceptions
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
  
 

The following repositories will have a `swift-5.2-branch` branch to track
sources as part of Swift 5.2 release:

 
- [indexstore-db](https://github.com/swiftlang/indexstore-db)

 
- [sourcekit-lsp](https://github.com/swiftlang/sourcekit-lsp)

 
- [swift](https://github.com/apple/swift)

 
- [swift-clang](https://github.com/apple/swift-clang)

 
- [swift-clang-tools-extra](https://github.com/apple/swift-clang-tools-extra)

 
- [swift-cmark](https://github.com/swiftlang/swift-cmark)

 
- [swift-compiler-rt](https://github.com/apple/swift-compiler-rt)

 
- [swift-corelibs-foundation](https://github.com/swiftlang/swift-corelibs-foundation)

 
- [swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch)

 
- [swift-corelibs-xctest](https://github.com/swiftlang/swift-corelibs-xctest)

 
- [swift-integration-tests](https://github.com/swiftlang/swift-integration-tests)

 
- [swift-libcxx](https://github.com/apple/swift-libcxx)

 
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
effect for the Swift 5.2 release as the release converges:

 
- 
 [Ted Kremenek](https://github.com/tkremenek) is the overall release manager for Swift 5.2.

 

 
- 
 [Doug Gregor](https://github.com/DougGregor) is the release manager for the Swift Compiler

 

 
- 
 [Duncan Exon Smith](https://github.com/dexonsmith) is the release manager for
[swift-llvm](https://github.com/apple/swift-llvm), [swift-clang](https://github.com/apple/swift-clang), [swift-compiler-rt](https://github.com/apple/swift-compiler-rt), [swift-clang-tools-extra](https://github.com/apple/swift-clang-tools-extra), and [swift-libcxx](https://github.com/apple/swift-libcxx).
 

 
- 
 [Fred Riss](https://github.com/fredriss) is the release manager for [swift-lldb](https://github.com/apple/swift-lldb).

 

 
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
 [Argyrios Kyrtzidis](https://github.com/akyrtzi) is the release manager for [sourcekit-lsp](https://github.com/swiftlang/sourcekit-lsp), [indexstore-db](https://github.com/swiftlang/indexstore-db), [swift-syntax](https://github.com/swiftlang/swift-syntax), and [swift-stress-tester](https://github.com/swiftlang/swift-stress-tester).

 

Please feel free to post on the [development forum](https://forums.swift.org/c/development/compiler)
or contact [Ted Kremenek](https://github.com/tkremenek) directly concerning any questions about the release management
process.

Pull Requests for Release Branch 
  
 

In order for a pull request to be considered for inclusion in the release
branch (`swift-5.2-branch`) after it has been cut, it must include the following
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
 

**All change** going on the `swift-5.2-branch` **must go through pull requests** that are
accepted by the corresponding release manager.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Nicole Jacque](https://github.com/najacque/)
 
 
 
 
 
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Swift 5.1 Released!

 September 20, 2019
 
 Swift 5.1 is now officially released!

 
 [ More ](/blog/swift-5.1-released/)
 
 

 
 
- 
 
 ### New Diagnostic Architecture Overview

 October 17, 2019
 
 Diagnostics play a very important role in a programming language experience. I..
 
 [ More ](/blog/new-diagnostic-arch-overview/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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