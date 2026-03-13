---
source: https://www.swift.org/blog/future-of-foundation/
crawled: 2025-11-15T21:22:10Z
---

# The Future of Foundation

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
 

 
 
 
 

 
 # The Future of Foundation

 
 
 
 
 
 
 
 
 *
 
 
 [Tony Parker](https://github.com/parkera/)
 
 
 
 
 
 
 Tony Parker manages teams at Apple working on Foundation, Swift packages, and the Swift Standard Library.
 
 
 
 
 

 December 9, 2022
 
 
 The Foundation framework is used in nearly all Swift projects. It provides both a base layer of functionality for fundamentals like strings, collections, and dates, as well as setting conventions for writing great Swift code.

Today, we have some exciting announcements for the future of Foundation.

Going Open 
  
 
When Swift [began life as an open source project](/blog/welcome/), we wanted to open not just the language itself, but the ecosystem around it. Foundation has been instrumental in the success of decades of software and has been an integral part of the Swift developer experience from the beginning, and we knew it had to be included in the open source offering.

The [swift-corelibs-foundation](https://github.com/swiftlang/swift-corelibs-foundation) project helped launch the open source Swift version of Foundation in 2016, wrapping a Swift layer around the preexisting, open source C implementation of Foundation.

In the intervening years, Swift has grown both technologically (e.g. ABI stability), as well as socially, attracting a diverse community of participants bound together by their interest in Swift.

With that growth, the time has come to reevaluate the strategy of an open source Foundation.

Going Further 
  
 
Today, we are announcing a new open source Foundation project, written in Swift, for Swift.

This achieves a number of technical goals:

 
- **No more wrapped C code.** With a native Swift implementation of Foundation, the framework no longer pays conversion costs between C and Swift, resulting in faster performance. A Swift implementation, developed as a package, also makes it easier for Swift developers to inspect, understand, and contribute code.

 
- **Provide the option of smaller, more granular packages.** Rewriting Foundation provides an opportunity to match its architecture to evolving use cases. Developers want to keep their binary sizes small, and a new FoundationEssentials package will provide the most important types in Foundation with no system dependencies to help accomplish this. A separate FoundationInternationalization package will be available when you need to work with localized content such as formatted dates and time. Other packages will continue to provide XML support and networking. A new FoundationObjCCompatibility package will contain legacy APIs which are useful for certain applications.

 
- **Unify Foundation implementations.** Multiple implementations of any API risks divergent behavior and ultimately bugs when moving code across platforms. This new Foundation package will serve as the core of a single, canonical implementation of Foundation, regardless of platform.

And this also achieves an important community goal:

 
- **Open contribution process.** Open source projects are at their best when the community of users can participate and become a community of developers. A new, open contribution process will be available to enable all developers to contribute new API to Foundation.

Going Together 
  
 
We’re excited to start discussing these plans with everyone [on the Swift forums](https://forums.swift.org/t/what-s-next-for-foundation/61939). The project itself will launch on GitHub in 2023.

 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Swift Summer of Code 2022 Summary

 December 5, 2022
 
 Google Summer of Code (also known as GSoC) is a long-running mentorship progra..
 
 [ More ](/blog/swift-summer-of-code-2022-summary/)
 
 

 
 
- 
 
 ### “The Swift Programming Language” book now published with DocC

 February 16, 2023
 
 We’re happy to announce that The Swift Programming Language book (TSPL) is now..
 
 [ More ](/blog/tspl-on-docc/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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