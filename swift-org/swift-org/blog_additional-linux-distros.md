---
source: https://www.swift.org/blog/additional-linux-distros/
crawled: 2025-11-15T21:25:50Z
---

# Additional Linux Distributions

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
 

 
 
 
 

 
 # Additional Linux Distributions

 
 
 
 
 
 
 
 
 *
 
 
 [Tom Doron](https://github.com/tomerd/)
 
 
 
 
 
 
 

 May 5, 2020
 
 
 It is my pleasure to announce a new set of Linux distributions officially supported by the Swift project. [Swift.org](/download/) now offers downloadable toolchain and Docker images for the following new Linux distributions:

 
- Ubuntu 20.04

 
- CentOS 8

 
- Amazon Linux 2

The above are added to the Linux platforms we already supported:

 
- Ubuntu 16.04

 
- Ubuntu 18.04

Porting work for Supporting Fedora-based Distributions 
  
 

The work to support Fedora based distributions such as CentOS and Amazon Linux included subtle changes in various components of the Swift project:

 
- Changing minimum required version of `libcurl` so that FoundationNetworking (in swift-corelibs-foundation) can be built on unmodified older Fedora based systems

 
- Teaching SwiftPM about Fedora based systems

 
- Refining how the Swift platform support was built and documented which led to dropping the dependency on `libatomic` on Linux.

In all, the work included 9 PRs to the Swift project:

 
- [https://github.com/apple/swift/pull/29581](https://github.com/apple/swift/pull/29581)

 
- [https://github.com/apple/swift/pull/30155](https://github.com/apple/swift/pull/30155)

 
- [https://github.com/swiftlang/swift-corelibs-foundation/pull/2716](https://github.com/swiftlang/swift-corelibs-foundation/pull/2716)

 
- [https://github.com/swiftlang/swift-corelibs-foundation/pull/2737](https://github.com/swiftlang/swift-corelibs-foundation/pull/2737)

 
- [https://github.com/swiftlang/swift-package-manager/pull/2642](https://github.com/swiftlang/swift-package-manager/pull/2642)

 
- [https://github.com/swiftlang/swift-package-manager/pull/2647](https://github.com/swiftlang/swift-package-manager/pull/2647)

 
- [https://github.com/swiftlang/swift-tools-support-core/pull/59](https://github.com/swiftlang/swift-tools-support-core/pull/59)

 
- [https://github.com/swiftlang/swift-tools-support-core/pull/60](https://github.com/swiftlang/swift-tools-support-core/pull/60)

 
- [https://github.com/swiftlang/swift-llbuild/pull/644](https://github.com/swiftlang/swift-llbuild/pull/644)

How Downloadable Images are Built 
  
 

Swift CI has moved to use Docker to build and qualify the new Linux distributions. A Dockerfile has been created for each one of the supported distributions, and CI jobs have been created to build, test and create a signed toolchain.

Linux build Dockerfiles are managed in [Swift’s Docker repository](https://github.com/swiftlang/swift-docker) with the goal of evolving them in the open with the community. Our plan is to continue and grow the number of Linux distributions we support, with CentOS 7, Debian and Fedora the most likely candidates to be added next.

It is important to note that the new distributions do not run automatically as part of PR testing - we continue to automatically test PRs on Ubuntu 16.04 - but you can “summon” them using the following commands:

 
- @swift-ci Please test Ubuntu 20.04

 
- @swift-ci Please test CentOS 8

 
- @swift-ci Please test Amazon Linux 2

Getting Involved 
  
 

If you are interested in building Swift on Linux, come and get involved!

The [source is available](https://github.com/swiftlang/swift-docker), and we encourage contributions from the open source community. If you have feedback, questions or would like to discuss the project, please feel free to chat on the [Swift forums](https://forums.swift.org/c/server). If you would like to report bugs, please use [the GitHub issue tracker](https://github.com/swiftlang/swift-docker/issues). We look forward to working with you, and helping move the industry forward to a better, safer programming future.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Tom Doron](https://github.com/tomerd/)
 
 
 
 
 
 
 Tom Doron is a member of the Swift Core Team and the Swift Server Workgroup. He manages a team working on server-side Swift libraries at Apple.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Swift 5.3 Release Process

 March 25, 2020
 
 This post describes the goals, release process, and estimated schedule for Swi..
 
 [ More ](/blog/5.3-release-process/)
 
 

 
 
- 
 
 ### Introducing Swift AWS Lambda Runtime

 May 29, 2020
 
 It is my pleasure to announce a new open source project for the Swift Server e..
 
 [ More ](/blog/AWS-lambda-runtime/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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