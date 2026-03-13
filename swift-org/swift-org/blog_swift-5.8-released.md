---
source: https://www.swift.org/blog/swift-5.8-released/
crawled: 2025-11-15T21:21:47Z
---

# Swift 5.8 Released!

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
 

 
 
 
 

 
 # Swift 5.8 Released!

 
 
 
 
 
 
 
 
 *
 
 
 [Alexander Sandberg](https://github.com/alexandersandberg/)
 
 
 
 
 
 
 Alexander Sandberg is an iOS and macOS developer and a member of the Swift Website Workgroup.
 
 
 
 
 

 March 30, 2023
 
 
 Swift 5.8 is now officially released! 🎉 This release includes major additions to the [language and standard library](#language-and-standard-library), including `hasFeature` to support piecemeal adoption of upcoming features, an improved [developer experience](#developer-experience), improvements to tools in the Swift ecosystem including [Swift-DocC](#swift-docc), [Swift Package Manager](#swift-package-manager), and [SwiftSyntax](#swiftsyntax), refined [Windows support](#windows-platform), and more.

Thank you to everyone in the Swift community who made this release possible. Your Swift Forums discussions, bug reports, pull requests, educational content, and other contributions are always appreciated!

For a quick dive into some of what’s new in Swift 5.8, check out this [playground](https://github.com/twostraws/whats-new-in-swift-5-8) put together by Paul Hudson.

[The Swift Programming Language](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/) book has been updated for Swift 5.8 and is now published with DocC. This is the official Swift guide and a great entry point for those new to Swift. The Swift community also maintains a number of [translations](/documentation/tspl/#translations).

Language and Standard Library 
  
 

Swift 5.8 enables you to start incrementally preparing your projects for Swift 6 by [using upcoming features](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0362-piecemeal-future-features.md). By default, upcoming features are disabled. To enable a feature, pass the compiler flag `-enable-upcoming-feature` followed by the feature’s identifier.

Feature identifiers can also be [used in source code](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0362-piecemeal-future-features.md#feature-detection-in-source-code) using `#if hasFeature(FeatureIdentifier)` so that code can still compile with older tools where the upcoming feature is not available.

Swift 5.8 includes upcoming features for the following Swift evolution proposals:

 
- SE-0274: [Concise magic file names](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0274-magic-file.md) (`ConciseMagicFile`)

 
- SE-0286: [Forward-scan matching for trailing closures](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0286-forward-scan-trailing-closures.md) (`ForwardTrailingClosures`)

 
- SE-0335: [Introduce existential any](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0335-existential-any.md) (`ExistentialAny`)

 
- SE-0354: [Regex literals](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0354-regex-literals.md) (`BareSlashRegexLiterals`)

For example, building the following file at `/Users/example/Desktop/0274-magic-file.swift` in a module called `MagicFile` with `-enable-experimental-feature ConciseMagicFile` will opt into the concise format for `#file` and `#filePath` described in SE-0274:



```
print(#file)
print(#filePath)
fatalError("Something bad happened!")

```



The above code will produce the following output:



```
MagicFile/0274-magic-file.swift
/Users/example/Desktop/0274-magic-file.swift
Fatal error: Something bad happened!: file MagicFile/0274-magic-file.swift, line 3

```



Swift 5.8 also includes conditional attributes to reduce the maintenance cost of libraries that support multiple Swift tools versions. `#if` checks can now surround attributes on a declaration, and a new `hasAttribute(AttributeName)` conditional directive can be used to check whether the compiler version has support for the attribute with the name `AttributeName` in the current language mode:



```
#if hasAttribute(preconcurrency)
@preconcurrency
#endif
protocol P: Sendable { ... }

```



Swift 5.8 brings other language and standard library enhancements, including [unboxing for `any` arguments to optional parameters](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0375-opening-existential-optional.md), [local wrapped properties in result builders](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0373-vars-without-limits-in-result-builders.md), [improved debug printing for key paths](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0369-add-customdebugdescription-conformance-to-anykeypath.md), and more.

You can find the complete list of Swift Evolution proposals that were implemented in Swift 5.8 in the [Swift Evolution Appendix](#swift-evolution-appendix) below.

Developer Experience 
  
 

Improved Result Builder Implementation 
  
 

The result builder implementation has been reworked in Swift 5.8 to greatly improve compile-time performance, code completion results, and diagnostics. The Swift 5.8 result builder implementation enforces stricter type inference that matches the semantics in [SE-0289: Result Builders](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0289-result-builders.md), which has an impact on some existing code that relied on invalid type inference.

The new implementation takes advantage of the [extended multi-statement closure inference](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0326-extending-multi-statement-closure-inference.md) introduced in Swift 5.7 and applies the result builder transformation exactly as specified by the result builder proposal - a source-level transformation which is type-checked like a multi-statement closure. Doing so enables the compiler to take advantage of all the benefits of the improved closure inference for result builder-transformed code, including optimized type-checking performance (especially in invalid code) and improved error messages.

For more details, please refer to the [Swift Forums post](https://forums.swift.org/t/improved-result-builder-implementation-in-swift-5-8/63192) that outlines the improvements and provides more information about invalid inference scenarios.

Ecosystem 
  
 

Swift-DocC 
  
 

As [announced in February](https://www.swift.org/blog/tspl-on-docc/), The Swift Programming Language book has been converted to Swift-DocC and made [open source](https://github.com/swiftlang/swift-book), and with it came some enhancements to Swift-DocC itself in the form of [option directives](https://www.swift.org/documentation/docc/options) you can use to change the behavior of your generated documentation. Swift-DocC has also added some new directives to create more [dynamic documentation pages](https://www.swift.org/documentation/docc/api-reference-syntax#creating-custom-page-layouts), including [Grid-based layouts](https://www.swift.org/documentation/docc/row) and [tab navigators](https://www.swift.org/documentation/docc/tab).

To take things even further, you can now [customize the appearance of your documentation pages](https://www.swift.org/documentation/docc/customizing-the-appearance-of-your-documentation-pages) with color, font, and icon customizations. Navigation also took a step forward with quick navigation, allowing fuzzy in-project search:

Swift-DocC also now supports documenting extensions to types from other modules. This is an opt-in feature and can be [enabled by adding the `--include-extended-types` flag when using the Swift-DocC plugin](https://apple.github.io/swift-docc-plugin/documentation/swiftdoccplugin/generating-documentation-for-extended-types).

Swift Package Manager 
  
 

Following are some highlights from the changes introduced to the [Swift Package Manager](https://github.com/swiftlang/swift-package-manager) in Swift 5.8:

 
- 
 [SE-0362](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0362-piecemeal-future-features.md): Targets can now specify the upcoming language features they require. `Package.swift` manifest syntax has been expanded with an API to include setting `enableUpcomingFeature` and `enableExperimentalFeature` flags at the target level.

 

 
- 
 [SE-0378](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0378-package-registry-auth.md): Token authentication when interacting with a package registry is now supported. The `swift package-registry` command has two new subcommands `login` and `logout` for adding/removing registry credentials.

 

 
- 
 Exposing an executable product that consists solely of a binary target that is backed by an artifact bundle is now allowed. This allows vending binary executables as their own separate package, independently of the plugins that are using them.

 

 
- 
 In packages using tools version 5.8 or later, Foundation is no longer implicitly imported into package manifests. If Foundation APIs are used, the module needs to be imported explicitly.

 

See the [Swift Package Manager changelog](https://github.com/swiftlang/swift-package-manager/blob/main/CHANGELOG.md#swift-58) for the complete list of changes.

SwiftSyntax 
  
 

With the Swift 5.8-aligned release of [SwiftSyntax](https://github.com/swiftlang/swift-syntax), SwiftSyntax contains a completely re-written parser that is implemented entirely in Swift instead of relying on the C++ parser to produce a SwiftSyntax tree. While the Swift compiler still uses the old parser implemented in C++, the eventual goal is to replace the old parser entirely. The new parser has a number of advantages:

 
- Contributing to or depending on SwiftSyntax is now as easy as any other Swift package. This greatly lowers the barrier of entry for new contributors and adopters.

 
- The new parser has been designed with error recovery as a primary goal. It is more tolerant of parsing errors and produces better error messages.

 
- SwiftSyntaxBuilder allows generating source code in a declarative way using a mixture of result builders and string interpolation. An example can be found [here](https://github.com/swiftlang/swift-syntax/blob/release/5.8/Examples/CodeGenerationUsingSwiftSyntaxBuilder.swift).

Windows Platform 
  
 

Swift 5.8 continues the incremental improvements to the Windows toolchain. Some of the important work that has gone into this release cycle includes:

 
- The Windows toolchain has reduced some of its dependency on environment variables. `DEVELOPER_DIR` was previously needed to locate components and this is no longer required. This cleans up the installation and enables us to get closer to per-user installation.

 
- ICU has been changed to static linking. This reduces the number of files that need to be distributed and reduces the number of dependencies that a shipping product requires. This was made possible by the removal of the ICU dependency in the Swift standard library.

 
- Some of the initial work to support C++ interop on Windows has been merged and is available in the toolchain. This includes the work towards modularising the Microsoft C++ Runtime (msvcprt).

 
- The `vcruntime` module has been renamed to `visualc`. This better reflects the module and paves the road for future enhancements for bridging the Windows platform libraries.

 
- A significant amount of work for improving path handling in the Swift Package Manager has been merged. This should help make Swift Package Manager more robust on Windows and improve interactions with SourceKit-LSP.

 
- SourceKit-LSP has benefited from several robustness improvements. Cross-module references are now more reliable and C/C++ references have been improved thanks to the enhanced path handling in SwiftPM which ensures that files are correctly identified.

Downloads 
  
 

Official binaries are [available for download](https://swift.org/download/) from [Swift.org](http://swift.org/) for Xcode, Windows, and Linux. The Swift 5.8 compiler is also included in [Xcode 14.3](https://apps.apple.com/app/xcode/id497799835).

Swift Evolution Appendix 
  
 

The following language, standard library, and Swift Package Manager proposals were accepted through the [Swift Evolution](https://github.com/swiftlang/swift-evolution) process and [implemented in Swift 5.8](https://www.swift.org/swift-evolution/#?version=5.8).

**Language and Standard Library**

 
- SE-0274: [Concise magic file names](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0274-magic-file.md)

 
- SE-0362: [Piecemeal adoption of upcoming language improvements](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0362-piecemeal-future-features.md)

 
- SE-0365: [Allow implicit self for weak self captures, after self is unwrapped](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0365-implicit-self-weak-capture.md)

 
- SE-0367: [Conditional compilation for attributes](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0367-conditional-attributes.md)

 
- SE-0368: [StaticBigInt](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0368-staticbigint.md)

 
- SE-0369: [Add CustomDebugStringConvertible conformance to AnyKeyPath](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0369-add-customdebugdescription-conformance-to-anykeypath.md)

 
- SE-0370: [Pointer Family Initialization Improvements and Better Buffer Slices](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0370-pointer-family-initialization-improvements.md)

 
- SE-0372: [Document Sorting as Stable](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0372-document-sorting-as-stable.md)

 
- SE-0373: [Lift all limitations on variables in result builders](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0373-vars-without-limits-in-result-builders.md)

 
- SE-0375: [Opening existential arguments to optional parameters](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0375-opening-existential-optional.md)

 
- SE-0376: [Function Back Deployment](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0376-function-back-deployment.md)

**Swift Package Manager**

 
- SE-0362: [Piecemeal adoption of upcoming language improvements](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0362-piecemeal-future-features.md)

 
- SE-0378: [Package Registry Authentication](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0378-package-registry-auth.md)

 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Swift Package Index gains Apple sponsorship

 March 24, 2023
 
 Building a thriving open source ecosystem is important to Swift’s success, and..
 
 [ More ](/blog/swift-package-index-developer-spotlight/)
 
 

 
 
- 
 
 ### Foundation Package Preview Now Available

 April 26, 2023
 
 I’m pleased to announce that a preview of the future of Foundation is now avai..
 
 [ More ](/blog/foundation-preview-now-available/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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