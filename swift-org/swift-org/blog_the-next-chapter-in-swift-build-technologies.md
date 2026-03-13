---
source: https://www.swift.org/blog/the-next-chapter-in-swift-build-technologies/
crawled: 2025-11-15T21:19:17Z
---

# The Next Chapter in Swift Build Technologies

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
 

 
 
 
 

 
 # The Next Chapter in Swift Build Technologies

 
 
 
 
 
 
 
 
 *
 
 
 [Owen Voorhees](https://github.com/owenv/)
 
 
 
 
 
 
 Owen Voorhees is a member of the team at Apple working on Swift Build.
 
 
 
 
 

 February 1, 2025
 
 
 Swift continues to grow in popularity as a cross-platform language supporting a wide variety of use cases, with support on a [variety of embedded devices](/blog/embedded-swift-examples/), form factors that encompass [wearables](https://developer.apple.com/documentation/watchos-apps/building_a_watchos_app) to [server](/documentation/server/), and a wide variety of [operating systems](/documentation/articles/static-linux-getting-started.html). As Swift expands, there’s value in investing in matching cross-platform build tools that provide a powerful, consistent, and flexible experience across the ecosystem.

As a foundational step in this new chapter of Swift build technologies, today Apple is open sourcing [Swift Build](https://github.com/swiftlang/swift-build), a powerful and extensible build engine that provides a set of build rules for building Swift projects. Swift Build is the engine used by Xcode, which supports millions of apps in the App Store as well as the internal build process for Apple’s own operating systems. The open source repository also includes support for targeting Linux and Windows.

Introducing Swift Build 
  
 

The primary responsibility of the build system is to transform user-authored inputs (such as a project description and source code) into output artifacts like command line tools, libraries, and applications. Build systems play an important role in providing a great developer experience, enabling higher level features which determine how users architect and work with their projects. Furthermore, the performance and reliability of a build system has a direct impact on developer productivity.

Swift Build is an infrastructural component designed to plan and execute builds requested by a higher-level client like Swift Package Manager or Xcode. It builds on top of the existing [llbuild](https://github.com/swiftlang/swift-llbuild) project to add capabilities including:

 
- Robust integration with the Swift compiler to reliably and efficiently coordinate the build of Swift projects

 
- Support for a wide variety of product types including libraries, command line tools, and GUI applications with advanced build configuration options

 
- Build graph optimizations that maximize parallelism when building Swift and C code

Roadmap for Swift Build 
  
 

Compared to the build engine in Xcode, the build engine in Swift Package Manager is fairly simple. On Apple platforms, having two different ways to build packages has also led to user confusion when the two implementations’ behavior didn’t match. Contributing Xcode’s build engine to the Swift project and developing it in open source alongside the Swift compiler provides the tools necessary to address these problems and deliver a great builds experience to all Swift users. With this release, SwiftPM now has the opportunity to offer a unified build execution engine across all platforms. This change should be transparent to users and maintain full compatibility with all existing packages while delivering a consistent cross-platform experience. At the same time, it lays the foundation to enable new features and improvements across all platforms and tools, and unlocks new performance optimizations and developer-facing features. As a small first step towards this vision, today the team is submitting a pull request to begin the process of integrating support for Swift Build in SwiftPM as an alternate build engine. In the coming months, we’d like to collaborate with the community to complete the work of unifying build system integrations so users can benefit from future tooling improvements across all platforms and project models.

We believe this is an important step in continuing to enable a healthy package ecosystem where developers can rely on a consistent, polished development experience — no matter what IDE they’re using or platform they’re targeting. We’ll be sharing more details about this work on the Swift forums, and we’re looking forward to hearing others’ feedback!

An invitation to participate 
  
 

We look forward to working with the community to continue evolving how we build Swift code. You can find the [`swift-build` repository](https://github.com/swiftlang/swift-build) in the Swift organization on GitHub, including a README and documentation describing how to build and contribute. Contributions via pull requests and issues are welcome, and we’d also love to solicit feedback and ideas for improvements on [the Swift forum](https://forums.swift.org/c/development/swift-build/).

This is an exciting new chapter for Swift’s build system, and we’re looking forward to all the potential it opens for Swift!

 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Announcing Swift 6

 September 17, 2024
 
 We’re delighted to announce the general availability of Swift 6. This is a maj..
 
 [ More ](/blog/announcing-swift-6/)
 
 

 
 
- 
 
 ### Updating the Visual Studio Code extension for Swift

 February 10, 2025
 
 Today, we are excited to announce a new version of the Swift extension for
Vis..
 
 [ More ](/blog/vscode-swift-2/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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