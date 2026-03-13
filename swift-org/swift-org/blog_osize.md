---
source: https://www.swift.org/blog/osize/
crawled: 2025-11-15T21:28:44Z
---

# Code Size Optimization Mode in Swift 4.1

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
 

 
 
 
 

 
 # Code Size Optimization Mode in Swift 4.1

 
 
 
 
 
 
 
 
 *
 
 
 [Erik Eckstein](https://github.com/eeckstein/)
 
 
 
 
 
 
 

 February 8, 2018
 
 
 In Swift 4.1 the compiler now supports a new optimization mode which enables dedicated optimizations to reduce code size.

The Swift compiler comes with powerful optimizations. When compiling with `-O` the compiler tries to transform the code so that it executes with maximum performance. However, this improvement in runtime performance can sometimes come with a tradeoff of increased code size.
With the new `-Osize` optimization mode the user has the choice to compile for minimal code size rather than for maximum speed.

To enable the size optimization mode on the command line, use `-Osize` instead of `-O`. In Xcode 9.3 there is a new Swift compiler code generation build setting:

Also, the compilation mode — single file or whole-module — can now be selected independently of the optimization mode:

The `-Osize` mode works in whole-module as well as in single-file compilation, whereas whole-module mode gives the best optimization results.

We have seen that using `-Osize` reduces code size from 5% to even 30% for some projects.

But what about performance? This completely depends on the project. For most applications the performance hit with `-Osize` will be negligible, i.e. below 5%. But for performance sensitive code `-O` might still be the better choice.

Impact on Code Optimization 
  
 

Let’s go into the details on what the compiler does differently with `-Osize`.
With `-Osize` the compiler optimizes the code, just like with `-O`.
But in contrast to `-O`, the compiler tries to avoid code duplication. For example, when inlining functions the compiler uses a lower size limit to decide whether a function should be inlined.

Completely disabling function inlining would be a bad idea, because inlining small functions often improve code size. For example consider simple getter functions, like



```
struct X {
    var x: Int { return 27 }
}

```



The call overhead of calling this getter would be much higher than to inline the function. This is an extreme example, but it turns out that inlining is still worth up to a certain size while still improving code size.
In addition, function inlining can trigger other optimizations, which in turn can reduce code size. For example, in the code snippet below by inlining the getter `a.x` we know that `a.x` evaluates to 27 and hence the entire `if` branch can be optimized away:



```
func foo(a: X) {
    if a.x != 27 {
        // Can be optimized away if the getter of a.x is inlined
    }
}

```



Beside inlining, the compiler performs other code size specific optimizations with `-Osize`. For example, some code patterns for handling generic types or for Objective-C bridging are extracted into helper functions and are not generated inline.

Conclusion 
  
 

The new `-Osize` optimization mode is a great way to reduce code size for programs which are not super performance sensitive.

We like to encourage you to try `-Osize` and give us feedback. Share your experiences in the forum, using the [osize](https://forums.swift.org/tags/osize) tag.

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Erik Eckstein](https://github.com/eeckstein/)
 
 
 
 
 
 
 Erik Eckstein is member of the Swift team at Apple. He has worked on various aspects of the Swift compiler's optimization pipeline.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Swift Forums Now Open!

 January 19, 2018
 
 We are delighted to announce that the Swift project has completed the process ..
 
 [ More ](/blog/forums/)
 
 

 
 
- 
 
 ### Swift 4.2 Release Process

 February 28, 2018
 
 This post describes the goals, release process, and estimated schedule for
Swi..
 
 [ More ](/blog/4.2-release-process/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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