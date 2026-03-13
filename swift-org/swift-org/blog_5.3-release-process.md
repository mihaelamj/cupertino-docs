---
source: https://www.swift.org/blog/5.3-release-process/
crawled: 2025-11-15T21:25:56Z
---

# Swift 5.3 Release Process

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
 

 
 
 
 

 
 # Swift 5.3 Release Process

 
 
 
 
 
 
 
 
 *
 
 
 [Nicole Jacque](https://github.com/najacque/)
 
 
 
 
 
 
 

 March 25, 2020
 
 
 This post describes the goals, release process, and estimated schedule for **Swift 5.3**.

Motivation and Goals 
  
 

Swift 5.3 is a release meant to include significant quality and performance enhancements. In addition, this release will expand the number of platforms where Swift is available and supported, notably adding support for Windows and additional Linux distributions.

Snapshots of Swift 5.3 
  
 

Downloadable snapshots of the Swift 5.3 release branch will be posted
regularly as part of [continuous integration](https://ci.swift.org) testing. As support is available, snapshot downloads will be added for newly supported platforms.

Once Swift 5.3 is released, the official final builds will also be posted in addition to the snapshots.

Getting Changes into Swift 5.3 
  
 

On **April 20, 2020** the `release/5.3` branch will be cut in the swift repository and most related project repositories. Please note the new branch naming scheme. This will contain the changes that will be released in Swift 5.3. After the branch is cut, changes can be landed on the branch via pull request if they meet the criteria for the release.

A few projects will cut their Swift 5.3 branches on different dates:

 
 
 Project
 Branch date
 
 
 
 
 [indexstore-db](https://github.com/swiftlang/indexstore-db)
 March 27, 2020
 
 
 [swift-llbuild](https://github.com/swiftlang/swift-llbuild)
 March 27, 2020
 
 
 [swift-package-manager](https://github.com/swiftlang/swift-package-manager)
 March 27, 2020
 
 
 [sourcekit-lsp](https://github.com/swiftlang/sourcekit-lsp)
 March 27, 2020
 
 
 [swift-tools-support-core](https://github.com/swiftlang/swift-tools-support-core)
 March 27, 2020
 
 

The same policies will apply to these projects: once the branch is cut, changes can be landed on the branch via pull request if they meet the criteria for the release.

Philosophy on Taking Changes into Swift 5.3 
  
 

 
- 
 All language and API changes for Swift 5.3 will go through the Swift
Evolution process. Evolution
proposals should aim to be completed by the branch date in order to
increase their chances of impacting the Swift 5.3 release. Exceptions
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
  
 

The following repositories will have a `release/5.3` branch to track
sources as part of Swift 5.3 release:

 
- [indexstore-db](https://github.com/swiftlang/indexstore-db)

 
- [sourcekit-lsp](https://github.com/swiftlang/sourcekit-lsp)

 
- [swift](https://github.com/apple/swift)

 
- [swift-cmark](https://github.com/swiftlang/swift-cmark)

 
- [swift-corelibs-foundation](https://github.com/swiftlang/swift-corelibs-foundation)

 
- [swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch)

 
- [swift-corelibs-xctest](https://github.com/swiftlang/swift-corelibs-xctest)

 
- [swift-integration-tests](https://github.com/swiftlang/swift-integration-tests)

 
- [swift-llbuild](https://github.com/swiftlang/swift-llbuild)

 
- [swift-package-manager](https://github.com/swiftlang/swift-package-manager)

 
- [swift-stress-tester](https://github.com/swiftlang/swift-stress-tester)

 
- [swift-syntax](https://github.com/swiftlang/swift-syntax)

 
- [swift-tools-support-core](https://github.com/swiftlang/swift-tools-support-core)

 
- [swift-xcode-playground-support](https://github.com/apple/swift-xcode-playground-support)

The [llvm-project](https://github.com/swiftlang/llvm-project) will have a corresponding `swift/release/5.3` branch.

Release Managers 
  
 

The overall management of the release will be overseen by the following
individuals, who will announce when stricter control of change goes into
effect for the Swift 5.3 release as the release converges.

For the Swift 5.3 release, we are adding release managers for each of our supported platforms. They will oversee platform specific issues as well as qualification of that platform for the release.

 
- 
 [Ted Kremenek](https://github.com/tkremenek) is the overall release manager for Swift 5.3.

 

 
- 
 [Doug Gregor](https://github.com/DougGregor) is the release manager for the Swift Compiler

 

 
- 
 [Duncan Exon Smith](https://github.com/dexonsmith) is the release manager for
[llvm-project](https://github.com/swiftlang/llvm-project).
 

 
- 
 [Fred Riss](https://github.com/fredriss) is the release manager for LLDB in [llvm-project](https://github.com/swiftlang/llvm-project).

 

 
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
[swift-llbuild](https://github.com/swiftlang/swift-llbuild) and [swift-tools-support-core](https://github.com/swiftlang/swift-tools-support-core).
 

 
- 
 [Argyrios Kyrtzidis](https://github.com/akyrtzi) is the release manager for [sourcekit-lsp](https://github.com/swiftlang/sourcekit-lsp), [indexstore-db](https://github.com/swiftlang/indexstore-db), [swift-syntax](https://github.com/swiftlang/swift-syntax), and [swift-stress-tester](https://github.com/swiftlang/swift-stress-tester).

 

Platform Release Managers 
  
 

 
- 
 [Nicole Jacque](https://github.com/najacque) is the release manager for the Darwin platform.

 

 
- 
 [Tom Doron](https://github.com/tomerd) is the release manager for Linux platforms.

 

 
- 
 [Saleem Abdulrasool](https://github.com/compnerd) is the release manager for the Windows platform.

 

Please feel free to post on the [development forum](https://forums.swift.org/c/development/compiler)
or contact [Ted Kremenek](https://github.com/tkremenek) directly concerning any questions about the release management
process.

Pull Requests for Release Branch 
  
 

In order for a pull request to be considered for inclusion in the release
branch (`release/5.3`) after it has been cut, it must include the following
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
 

**All changes** going on the `release/5.3` branch **must go through pull requests** that are
accepted by the corresponding release manager.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Nicole Jacque](https://github.com/najacque/)
 
 
 
 
 
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Swift 5.2 Released!

 March 24, 2020
 
 Swift 5.2 is now officially released! 🎉

 
 [ More ](/blog/swift-5.2-released/)
 
 

 
 
- 
 
 ### Additional Linux Distributions

 May 5, 2020
 
 It is my pleasure to announce a new set of Linux distributions officially supp..
 
 [ More ](/blog/additional-linux-distros/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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