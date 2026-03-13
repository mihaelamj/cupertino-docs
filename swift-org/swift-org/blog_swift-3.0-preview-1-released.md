---
source: https://www.swift.org/blog/swift-3.0-preview-1-released/
crawled: 2025-11-15T21:30:34Z
---

# Swift 3.0 Preview 1 Released!

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
 

 
 
 
 

 
 # Swift 3.0 Preview 1 Released!

 
 
 
 
 
 
 
 
 *
 
 
 [Ted Kremenek](https://github.com/tkremenek/)
 
 
 
 
 
 
 

 June 13, 2016
 
 
 We are very pleased to announce **Developer Preview 1** of Swift 3.0!

As described in the [Swift 3.0 Release Process](/blog/swift-3-0-release-process/), developer previews (i.e., “seeds” or
“betas”) provide qualified builds of Swift 3 that are more stable than just
grabbing the latest snapshot of `master` (i.e., tip-of-trunk development).
Developer previews capture Swift 3 as a work-in-progress and should not
be considered the final version of Swift 3 unless otherwise stated.

Implemented Swift Evolution Proposals 
  
 

The following [Swift Evolution](https://github.com/swiftlang/swift-evolution) proposals are newly implemented
in Swift 3.0 Preview 1:

 
- [SE-0002: Removing currying `func` declaration syntax](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0002-remove-currying.md)

 
- [SE-0003: Removing `var` from Function Parameters](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0003-remove-var-parameters.md)

 
- [SE-0004: Remove the `++` and `--` operators](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0004-remove-pre-post-inc-decrement.md)

 
- [SE-0005: Better Translation of Objective-C APIs Into Swift](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0005-objective-c-name-translation.md)

 
- [SE-0006: Apply API Guidelines to the Standard Library](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0006-apply-api-guidelines-to-the-standard-library.md)

 
- [SE-0007: Remove C-style for-loops with conditions and incrementers](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0007-remove-c-style-for-loops.md)

 
- [SE-0008: Add a Lazy flatMap for Sequences of Optionals](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0008-lazy-flatmap-for-optionals.md)

 
- [SE-0016: Adding initializers to Int and UInt to convert from UnsafePointer and UnsafeMutablePointer](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0016-initializers-for-converting-unsafe-pointers-to-ints.md)

 
- [SE-0017: Change `Unmanaged` to use `UnsafePointer`](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0017-convert-unmanaged-to-use-unsafepointer.md)

 
- [SE-0019: Swift Testing](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0019-package-manager-testing.md)

 
- [SE-0023: API Design Guidelines](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0023-api-guidelines.md)

 
- [SE-0028: Modernizing Swift’s Debugging Identifiers (__FILE__, etc)](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0028-modernizing-debug-identifiers.md)

 
- [SE-0029: Remove implicit tuple splat behavior from function applications](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0029-remove-implicit-tuple-splat.md)

 
- [SE-0031: Adjusting inout Declarations for Type Decoration](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0031-adjusting-inout-declarations.md)

 
- [SE-0032: Add `first(where:)` method to `SequenceType`](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0032-sequencetype-find.md)

 
- [SE-0033: Import Objective-C Constants as Swift Types](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0033-import-objc-constants.md)

 
- [SE-0034: Disambiguating Line Control Statements from Debugging Identifiers](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0034-disambiguating-line.md)

 
- [SE-0037: Clarify interaction between comments & operators](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0037-clarify-comments-and-operators.md)

 
- [SE-0039: Modernizing Playground Literals](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0039-playgroundliterals.md)

 
- [SE-0040: Replacing Equal Signs with Colons For Attribute Arguments](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0040-attributecolons.md)

 
- [SE-0043: Declare variables in ‘case’ labels with multiple patterns](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0043-declare-variables-in-case-labels-with-multiple-patterns.md)

 
- [SE-0044: Import as Member](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0044-import-as-member.md)

 
- [SE-0046: Establish consistent label behavior across all parameters including first labels](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0046-first-label.md)

 
- [SE-0047: Defaulting non-Void functions so they warn on unused results](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0047-nonvoid-warn.md)

 
- [SE-0048: Generic Type Aliases](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0048-generic-typealias.md)

 
- [SE-0049: Move @noescape and @autoclosure to be type attributes](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0049-noescape-autoclosure-type-attrs.md)

 
- [SE-0053: Remove explicit use of `let` from Function Parameters](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0053-remove-let-from-function-parameters.md)

 
- [SE-0054: Abolish `ImplicitlyUnwrappedOptional` type](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0054-abolish-iuo.md)

 
- [SE-0055: Make unsafe pointer nullability explicit using Optional](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0055-optional-unsafe-pointers.md)

 
- [SE-0057: Importing Objective-C Lightweight Generics](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0057-importing-objc-generics.md)

 
- [SE-0059: Update API Naming Guidelines and Rewrite Set APIs Accordingly](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0059-updated-set-apis.md)

 
- [SE-0061: Add Generic Result and Error Handling to autoreleasepool()](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0061-autoreleasepool-signature.md)

 
- [SE-0062: Referencing Objective-C key-paths](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0062-objc-keypaths.md)

 
- [SE-0064: Referencing the Objective-C selector of property getters and setters](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0064-property-selectors.md)

 
- [SE-0065: A New Model For Collections and Indices](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0065-collections-move-indices.md)

 
- [SE-0066: Standardize function type argument syntax to require parentheses](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0066-standardize-function-type-syntax.md)

 
- [SE-0069: Mutability and Foundation Value Types](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0069-swift-mutability-for-foundation.md)

 
- [SE-0070: Make Optional Requirements Objective-C-only](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0070-optional-requirements.md)

 
- [SE-0071: Allow (most) keywords in member references](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0071-member-keywords.md)

 
- [SE-0085: Package Manager Command Names](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0085-package-manager-command-name.md)

 
- [SE-0094: Add sequence(first:next:) and sequence(state:next:) to the stdlib](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0094-sequence-function.md)

Downloads 
  
 

Apple (Xcode) 
  
 

Swift 3.0 Preview 1 is available for free as part of [Xcode 8 beta 1](https://developer.apple.com/xcode/download).

Linux (Ubuntu 14.04 and Ubuntu 15.10) 
  
 

Official binaries for Ubuntu 14.04 and Ubuntu 15.10 are [available for download](/download/) on Swift.org.

Documentation 
  
 

An updated version of [The Swift Programming Language](/documentation/tspl) for Swift 3.0 is now available on Swift.org. It is also available for free on Apple’s [iBooks store](https://itunes.apple.com/us/book/the-swift-programming-language/id1002622538?mt=11).

Foundation and Linux (Core Libraries) 
  
 

Not all of the `NS` prefix removal changes have propagated to the Core Libraries implementation of Foundation APIs.
This should be resolved in a future beta.

Migrating to Swift 3 
  
 

Swift 3 is a source-breaking release over Swift 2.2.1. It contains many syntactic refinements and improvements,
but also a huge number of changes for how Objective-C APIs import into Swift due to [SE-0005](https://github.com/swiftlang/swift-evolution/blob/master/proposals/0005-objective-c-name-translation.md).
Please consult the [migration guide](/migration-guide/) for guidance and tips
for migrating to Swift 3.
 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Ted Kremenek](https://github.com/tkremenek/)
 
 
 
 
 
 
 Ted Kremenek is a member of the Swift Core Team and manages the Languages and Runtimes group at Apple.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Swift 2.3

 June 12, 2016
 
 We are pleased to announce Swift 2.3!

 
 [ More ](/blog/swift-2.3/)
 
 

 
 
- 
 
 ### Xcode Playground Support

 July 7, 2016
 
 We are delighted to introduce Xcode Playground Support
as part of the Swift op..
 
 [ More ](/blog/swift-xcode-playground-support/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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