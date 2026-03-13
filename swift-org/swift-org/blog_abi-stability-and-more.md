---
source: https://www.swift.org/blog/abi-stability-and-more/
crawled: 2025-11-15T21:27:34Z
---

# ABI Stability and More

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
 

 
 
 
 

 
 # ABI Stability and More

 
 
 
 
 
 
 
 
 *
 
 
 [Jordan Rose](https://github.com/jrose-apple/)
 
 
 
 
 
 
 

 February 7, 2019
 
 
 It has been a longstanding goal to stabilize Swift’s ABI on macOS, iOS, watchOS, and tvOS. While a stable ABI is an important milestone for the maturity of any language, the ultimate benefit to the Swift ecosystem was to enable binary compatibility for apps and libraries. This post describes what binary compatibility means in Swift 5 and how it will evolve in future releases of Swift.

You may ask: what about other platforms? ABI stability is implemented for each operating system that it compiles and runs on. Swift’s ABI is currently declared stable for Swift 5 on Apple platforms. As development of Swift on Linux, Windows, and other platforms matures, the Swift Core Team will evaluate stabilizing the ABI on those platforms.

Swift 5 provides binary compatibility for apps: a guarantee that going forward, an app built with one version of the Swift compiler will be able to talk to a library built with another version. This applies even when using the compatibility mode with older language versions (`-swift-version 4.2`).

In this example, an app built with Swift 5.0 will run on systems that have a Swift 5 standard library installed, as well as those with a hypothetical Swift 5.1 or Swift 6.

(all version numbers past Swift 5.0 in this post are hypothetical, of course)

ABI stability for Apple OSes means that apps deploying to upcoming releases of those OSes will no longer need to embed the Swift standard library and “overlay” libraries within the app bundle, shrinking their download size; the Swift runtime and standard library will be shipped with the OS, like the Objective-C runtime.

More information on how this affects apps submitted to the App Store is available in the [Xcode 10.2 release notes](https://developer.apple.com/documentation/xcode_release_notes/xcode_10_2_beta_release_notes/swift_5_release_notes_for_xcode_10_2_beta).

Module Stability 
  
 

ABI stability is about mixing versions of Swift at run time. What about compile time? Right now, Swift uses an opaque archive format called “swiftmodule” to describe the interface of a library, such as a framework “MagicKit”, rather than manually-written header files. However, the “swiftmodule” format is also tied to the current version of the compiler, which means an app developer can’t `import MagicKit` if MagicKit was built with a different version of Swift. That is, the app developer and the library author have to be using the same version of the compiler.

To remove this restriction, the library author needs a feature currently being implemented called module stability. This involves augmenting the opaque format with a textual summary of a module, similar to what you see in Xcodeʼs “Generated Interface” view, so that clients can use a module without having to care what compiler it was built with. You can read more about that [on the Swift forums](https://forums.swift.org/t/plan-for-module-stability/14551).

As an example, you could build a framework using Swift 6, and that framework’s interface would be readable by both Swift 6 and a future Swift 7 compiler.

Again, all Swift version numbers here are hypothetical.

Library Evolution 
  
 

Up until now, we’ve been talking about changing the compiler but keeping the Swift code the same. What about changes to libraries that an app is using? Today, when a Swift library changes, any apps using that library have to be recompiled. This has some advantages: because the compiler knows the exact version of the library the app is using, it can make additional assumptions that reduce code size and make the app run faster. But those assumptions might not be true for the next version of the library.

This feature is library evolution support: shipping a new version of a library without having to recompile its clients. This happens when Apple updates the libraries in an OS, but it’s also important when one company’s binary framework depends on another company’s binary framework. In this case, updating the second framework would ideally not require recompiling the first framework.

In this example, the app is built against the original version of the framework, in yellow. With support for library evolution, it will run on systems that have the yellow version available, but also the newer, improved red version.

Swift already has an implementation of support for library evolution, informally termed “resilience”. It’s an opt-in feature for libraries that need it, and it uses not-yet-finalized annotations to strike a balance between performance and future flexibility, which you can see in the source code for the standard library. The first of these to go through the Swift Evolution Process was `@inlinable`, added in Swift 4.2 ([SE-0193](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0193-cross-module-inlining-and-specialization.md)). Look for more proposals about library evolution support in the future.

Summary 
  
 

 
 
 When Swift has…
 …then you can change…
 Status
 
 
 
 
 ABI Stability
 the Swift
standard library
 Swift 5 on macOS, iOS, watchOS, and tvOS
 
 
 Module Stability
(and ABI stability)
 compilers
 Under active development
 
 
 Library Evolution Support
 your library’s APIs
 Largely implemented but needs to go through the Swift Evolution Process
 
 

Questions? 
  
 

Please feel free to post questions about this post on the [associated thread](https://forums.swift.org/t/swift-org-blog-abi-stability-and-more/20250) on the [Swift forums](https://forums.swift.org).

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Jordan Rose](https://github.com/jrose-apple/)
 
 
 
 
 
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Introducing the sourcekitd Stress Tester

 February 6, 2019
 
 Sourcekitd provides the data backing key editor features like code completion,..
 
 [ More ](/blog/sourcekitd-stress-tester/)
 
 

 
 
- 
 
 ### Evolving Swift On Apple Platforms After ABI Stability

 February 11, 2019
 
 With the release of Swift 5.0, Swift is now ABI stable and is delivered as a c..
 
 [ More ](/blog/abi-stability-and-apple/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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