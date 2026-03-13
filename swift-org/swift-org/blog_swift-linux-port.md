---
source: https://www.swift.org/blog/swift-linux-port/
crawled: 2025-11-15T21:31:37Z
---

# The Swift Linux Port

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
 

 
 
 
 

 
 # The Swift Linux Port

 
 
 
 

 December 3, 2015
 
 
 With the launch of the open source Swift project, we are also releasing
a port that works with the Linux operating system! You can build it from
the Swift sources or [download pre-built binaries for Ubuntu](/download/). The port
is still a work in progress but we’re happy to say that it is usable
today for experimentation. Currently x86_64 is the only supported
architecture on Linux.

Here are a few highlights of what’s working in the port today:

 
- 
 **Swift without the Objective-C Runtime**: Swift on Linux does not
depend on the Objective-C runtime nor includes it. While Swift was
designed to interoperate closely with Objective-C when it is present,
it was also designed to work in environments where the Objective-C
runtime does not exist.
 

 
- 
 **The core Swift Language and [Standard Library](/documentation/standard-library/)** on Linux shares most of
the same implementation and APIs as on Apple platforms. There are some
slight differences of behavior because of the absence of the
Objective-C runtime on Linux (noted below).
 

 
- 
 **The Glibc Module**: Most of the Linux C standard library is available
through this module similar to the Darwin module on Apple platforms.
Some headers aren’t yet imported into the module, such as tgmath.h.

 To try it out, just `import Glibc`.

 

 
- 
 **Swift Core Libraries**: The [Core Libraries](/documentation/core-libraries/) provide implementations
of core APIs from Foundation and XCTest to be used on Linux without
Objective-C . The intention is that these APIs are available in a
cross-platform manner regardless of whether you are using Swift on
Apple’s platforms or Swift on Linux.
 

 
- 
 **LLDB Swift debugging and the REPL**: You can debug your Swift
binaries and [experiment in the REPL](/getting-started/#using-the-repl) just like you do on macOS.
 

 
- 
 **The Swift Package Manager** is a first class citizen as it is on
Apple’s platforms.
 

Here are some things that aren’t quite working yet or are planned for
the future:

 
- 
 **libdispatch**: Part of the Core Libraries, updated Linux support is
in progress. You can follow development on the libdispatch project on
GitHub.
 

 
- 
 **Some C APIs**: Although this is generally true for all of our
supported platforms, a few constructs in C aren’t imported yet into
Swift. This will cause some APIs to be unavailable, such as those that
contain varargs / `va_list`. However, in recent months Swift’s
interoperability with C has significantly advanced, gaining support
for named and anonymous unions, anonymous structs, and bitfields.
 

 
- 
 **Some `String` APIs**: The Standard Library’s `String` is missing implementations
of several important APIs because they are currently tied to the
implementation of `NSString` on Apple’s platforms.
 

 
- 
 **Runtime Introspection**: When a Swift class on Apple’s platforms is
marked `@objc` or subclasses `NSObject` you can use the Objective-C
runtime to enumerate available methods on an object or call methods
using selectors. Such capabilities are absent because they depend on
the Objective-C runtime.
 

 
- 
 `Array<T> as? Array<S>`: Some mechanisms, such as casting
containers with different associated types, currently do not work as
they relied on bridging mechanisms with Objective-C.
 

We’re really excited to be able to release the open source project with
a great head start for Linux support that you can try right now! There
is still plenty of work to be done, so we hope to see you contribute to
Swift to make the Linux port even more complete.

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### The Swift.org Blog

 December 3, 2015
 
 Welcome to the blog on Swift.org! Today we launched the open source Swift proj..
 
 [ More ](/blog/welcome/)
 
 

 
 
- 
 
 ### Swift 3 API Design Guidelines

 December 3, 2015
 
 The design of commonly-used libraries has a large impact on the
overall feel o..
 
 [ More ](/blog/swift-3-api-design/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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
 
 *
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