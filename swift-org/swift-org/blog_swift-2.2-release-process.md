---
source: https://www.swift.org/blog/swift-2.2-release-process/
crawled: 2025-11-15T21:31:25Z
---

# Swift 2.2 Release Process

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
 

 
 
 
 

 
 # Swift 2.2 Release Process

 
 
 
 
 
 
 
 
 *
 
 
 [Ted Kremenek](https://github.com/tkremenek/)
 
 
 
 
 
 
 

 January 5, 2016
 
 
 This post describes the goals, release process, and estimated schedule
for Swift 2.2.

Swift 2.2 is the first official release of Swift after Swift was
released as open source. It will be a mostly source-compatible
release with Swift 2.1, containing a large number of core improvements
(e.g., bug fixes, enhancements to diagnostics, faster generated code)
without many significant visible changes to the language itself. It
is intended to be an intermediate point between Swift 2 and Swift 3,
with Swift 3 containing more disruptive changes to both the language
and Standard Library.

Swift on Linux will be included in this release. However it is still
relatively new and has known caveats. Swift 2.2 will not include the
[Swift Core Libraries](/documentation/core-libraries/) but will include LLDB and the REPL.

The [Swift Package Manager](/documentation/package-manager/) is still early in development and
will not be included in this release.

In addition to its Swift.org release, Swift 2.2 will ship in a future version
of Xcode.

Logistics 
  
 

Impacted Repositories 
  
 

The following repositories will have a `swift-2.2-branch` to track sources as part of
Swift 2.2 release:

 
- [swift](https://github.com/apple/swift)

 
- [swift-llvm](https://github.com/apple/swift-llvm)

 
- [swift-clang](https://github.com/apple/swift-clang)

 
- [swift-lldb](https://github.com/apple/swift-lldb)

 
- [swift-cmark](https://github.com/swiftlang/swift-cmark)

Schedule 
  
 

 
- 
 Swift 2.2 will branch from `master` on **January 13, 2016**. After
January 13 the `master` branch will primarily track Swift 3
development. This specifically applies to the [swift](https://github.com/apple/swift), [swift-lldb](https://github.com/apple/swift-lldb),
and [swift-cmark](https://github.com/swiftlang/swift-cmark) repositories.
 

 
- 
 The `swift-2.2-branch` branches for the [swift-llvm](https://github.com/apple/swift-llvm) and
[swift-clang](https://github.com/apple/swift-clang) repositories were created earlier, from roughly LLVM revision
252576, to provide a longer period of time to stabilize the underlying
LLVM platform. Those repositories will not be re-merging from
`master`.
 

 
- 
 The final release date for Swift 2.2 is TBD. The intent is for
Swift 2.2 to undergo a convergence period and will likely be
released sometime in March to May of 2016.
 

 
- 
 [Binary builds](/download/) of the Swift 2.2 release branch will be
made available alongside snapshots of Swift’s `master` development branch.
 

 
- 
 Swift 3 will follow a similar branching process as Swift 2.2 and
will be announced a future date.
 

Getting Changes into Swift 2.2 
  
 

 
- 
 Before January 13 all changes going into Swift’s `master` branch by
definition will be a part of Swift 2.2.
 

 
- 
 Afterwards, the only language/API changes that will be considered
for Swift 2.2 after the branch will be ones that make it more source
compatible with Swift 2.1 or migration warnings for code that will
be a build error in Swift 3. New features/extensions will not be
accepted unless there is a really good reason to take them.
 

 
- 
 Criteria (as set by the release manager) for accepting changes will
becoming increasingly restrictive over time as the release
converges.
 

 
- 
 Contributions to the `swift-2.2-branch` for those without direct
commit access can be initiated via a [pull request](#pull-requests).
Pull requests should usually be used to pull in changes that are
already in `master` unless those changes are specific to Swift 2.2
and will not be included in Swift 3. For example, a bug fix should
first land in `master` and then pulled into `swift-2.2-branch`.
 

 
- 
 Core contributors with direct commit access will be able to directly
cherry-pick or otherwise apply changes to `swift-2.2-branch` under
the guidance of the respective code owner or release manager until a
point in the schedule where the branch is under strict change
control. An announcement will be made to the relevant component
development mailing list (e.g., [swift-dev](https://lists.swift.org/pipermail/swift-dev/)) once the
`swift-2.2-branch` is under stricter change management. At that
point all changes must go through a nomination process via a
[pull request](#pull-requests).
 

Release Management 
  
 

The overall management of the release will be overseen by the following
individuals, who will announce when stricter control of change
goes into effect for the Swift 2.2 release as the release converges:

 
- 
 [Ted Kremenek](https://github.com/tkremenek) is the overall release
manager for Swift 2.2.
 

 
- 
 [Bob Wilson](https://github.com/bob-wilson) is the release manager
for the LLVM and Clang components of Swift 2.2.
 

 
- 
 [Kate Stone](https://github.com/k8stone) is the
release manager for the LLDB component of Swift 2.2.
 

Please feel free to email [swift-dev](https://lists.swift.org/pipermail/swift-dev/) or Ted directly concerning any
questions about the release management process.

 Note: Swift mailing lists have been shut down, archived, and replaced with
[Swift Forums](https://forums.swift.org). See
[the announcement here](/blog/forums/).

Pull Requests 
  
 

All pull requests nominating changes for inclusion in the
`swift-2.2-branch` should include the following information:

 
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

Prior to the `swift-2.2-branch` going into restrictive change
control (as announced by the release manager) a code owner is allowed
to directly accept a pull request after it has gone through the
aforementioned technical review. Once restrictive change control is
in place, only the release manager is allowed to accept a pull request
into `swift-2.2-branch`.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Ted Kremenek](https://github.com/tkremenek/)
 
 
 
 
 
 
 Ted Kremenek is a member of the Swift Core Team and manages the Languages and Runtimes group at Apple.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Swift 3 API Design Guidelines

 December 3, 2015
 
 The design of commonly-used libraries has a large impact on the
overall feel o..
 
 [ More ](/blog/swift-3-api-design/)
 
 

 
 
- 
 
 ### It's Coming: the Great Swift API Transformation

 January 29, 2016
 
 Cocoa, the Swift standard library, maybe even your own types and
methods—it’s ..
 
 [ More ](/blog/swift-api-transformation/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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