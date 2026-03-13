---
source: https://www.swift.org/blog/xcode-9.1-improves-display-of-fatal-errors/
crawled: 2025-11-15T21:29:07Z
---

# Xcode 9.1 Improves Display of Fatal Errors

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
 

 
 
 
 

 
 # Xcode 9.1 Improves Display of Fatal Errors

 
 
 
 
 
 
 
 
 *
 
 
 [Kuba Mracek](https://github.com/kubamracek/)
 
 
 
 
 
 
 

 October 5, 2017
 
 
 Swift has language constructs that allow you to specify your program’s expectations. If these expectations are not met at runtime, the program will be terminated. For example, indexing into an array implicitly expresses an expectation that the index is in bounds:



```
// Program will terminate if 'index' less than 0 or greater than 'array.count - 1'.
let element = array[index]

```



Another common operation that will terminate the program on failure is a forced unwrap of an optional:



```
// Program will terminate if 'self.navigationController' is nil.
let nc = self.navigationController!

```



Preconditions are yet another example:



```
// Program will terminate if 'index' is less or equal to 0.
precondition(index > 0, "Index must be greater than zero.")

```



When the expectations are incorrect or when there’s a bug in the code, Swift guarantees that the program will trap. Especially during development it’s common that some precondition isn’t met, the program terminates and the debugger will show that. However, prior to Xcode 9.1 (currently available as a beta), the debugger displayed these situations just as any other type of crash — usually as `EXC_BAD_INSTRUCTION` or `EXC_BREAKPOINT` (which are the low-level Mach exceptions types).

This has been a source of confusion for both beginners and seasoned developers. In Xcode 9.1 the display of fatal errors is significantly improved. When running under the debugger, Xcode will now show the failure reason in the editor where the trap occurred:

Many events that trigger a runtime trap are covered, including:

 
- forced unwrapping `nil`

 
- forced-try expressions (`try!`) producing an error

 
- out-of-bounds indexing into arrays

 
- precondition failures

 
- assertion failures

 
- `fatalError` calls

Note that this improved experience is only available when the app’s entry point is written in Swift (i.e. your app delegate with the `@UIApplicationMain`/`@NSApplicationMain` attribute).

Xcode 9.1 can be downloaded from [developer.apple.com](https://developer.apple.com/download/) (currently a pre-release version, an official release will be available later this year).

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Kuba Mracek](https://github.com/kubamracek/)
 
 
 
 
 
 
 Kuba Mracek is a member of Apple's compiler team, focusing on Embedded Swift and using Swift for systems programming.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Dictionary and Set Improvements in Swift 4.0

 October 4, 2017
 
 In the latest release of Swift,
dictionaries and sets gain a number of new met..
 
 [ More ](/blog/dictionary-and-set-improvements/)
 
 

 
 
- 
 
 ### Swift 4.1 Release Process

 October 17, 2017
 
 This post describes the goals, release process, and estimated schedule for Swi..
 
 [ More ](/blog/swift-4.1-release-process/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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