---
source: https://www.swift.org/blog/swift-on-windows/
crawled: 2025-11-15T21:25:21Z
---

# Introducing Swift on Windows

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
 

 
 
 
 

 
 # Introducing Swift on Windows

 
 
 
 
 
 
 
 
 *
 
 
 [Saleem Abdulrasool](https://github.com/compnerd/)
 
 
 
 
 
 
 

 September 22, 2020
 
 
 The Swift project is introducing [new downloadable Swift toolchain images](/download) for Windows! These images contain development components needed to build and run Swift code on Windows.

For over a year now, there has been a significant endeavour to port Swift to Windows in conjunction with the developer community at swift.org. The Windows support is now at a point where early adopters can start using Swift to build real experiences on this platform.

Bringing Swift to Windows 
  
 

Porting Swift to Windows is not about simply porting the compiler, but rather ensuring that the full ecosystem is available on the platform. This includes the compiler, the standard library, and the core libraries (dispatch, Foundation, XCTest). These libraries are part of what enables developers to write powerful applications with ease and without having to worry about many of the details of the underlying system. There are many technical details in the story of bringing Swift to a usable state on Windows, and if you are interested in them, I would recommend checking out my [talk on the topic](https://www.youtube.com/watch?v=Zjlxa1NIfJc) from the LLVM Developer Conference.

With these core libraries and the flexible interoperability of Swift with C, it is possible to develop applications on Windows purely in Swift while taking advantage of the existing corpus of libraries on the Windows platforms.

Example Application 
  
 

This [demo calculator](https://github.com/compnerd/swift-win32/blob/ed4993f7cbb284a83ee77fcecdc2570cf24355e4/Examples/Calculator/Calculator.swift) is written entirely in Swift, with code seamlessly flipping between the application code written in Swift and the system libraries:

This project was built using:

 
- 
 The Swift toolchain on Windows

 

 
- 
 An installation of Visual Studio 2019 which delivers the other needed pieces in the form of CMake, Ninja, and the Windows SDK

 

Although the demo application is built with CMake, Swift Package Manager support on Windows is coming along. It will soon be possible to get the application building using `swift build` without needing CMake or Ninja.

Here you can see stepping through the application using `lldb`:

Cross-Platform Applications 
  
 

Early adopters like [Readdle](https://readdle.com/) are experimenting with cross-platform applications written in Swift, easily bringing many of the existing Swift libraries to Windows to support their applications.

I had been working with Alexander at Readdle about his team’s work, and he sent me this note:

 We at Readdle started experimenting with Swift on Windows more than a year ago, in Q2 of 2019. By that time we already released Spark for Android which uses Swift to share core code with iOS/macOS, and the opportunity to extend to one more platform was really tempting.

 Despite some functionality being unready as of yet, Swift on Windows turned out to be fully satisfying our needs. In fact, some third party C/C++ dependencies gave us more headaches than Swift did itself. All business logic of Spark is located in a separate Core module. A pack of modules, actually, but we refer to them as Core. This allows us to use any UI framework on the target platform: AppKit on macOS, UIKit on iOS, native UI Toolkit on Android. So, basically, we had to port Spark Core on Windows. After all initial concepts were proved, it was mostly routine day-to-day work to bring it alive on Windows.

 What we have now:

 
 
- 9 Swift modules (255 739 SLOC, 2 133 source files)

 
- 3 third party swift modules

 
- 1452 tests (powered by XCTest)

 
- Windows-based CI to keep all tests green

 
- Heterogenous build system (partially CMake, partially custom scripts)

 

 As a good example, pure Swift modules like CryptoSwift and OAuthSwift almost worked right out of the box. We did trivial imports adjustment, excluded a few AppKit/UIKit references and voilà!

 Another challenge was to decide how to implement the user interface. After extensive discourse we ended up with Electron as the front-end part of future Spark for Windows. That meant we not only needed to be able to build Spark Core on Windows but also use it as a loadable addon for Node.js.

 Node.js addon in pure Swift? That appeared to be surprisingly easy. Swift perfectly imports N-API headers. We still need three lines of C code plus one small C header to define addon entry point, but all logic is in Swift. Due to the crossplatform nature of Node.js, we were able to use macOS as a development platform with Xcode as IDE, and then use the agility of CMake to build the same code on Windows.

 Since the first day we started, Swift on Windows did a giant step forward in terms of platform support and stability. I’d say, if you are thinking about extending your existing application codebase to platforms other than macOS/iOS – you absolutely can do it with Swift now, or, at least, soon. If you are maintaining a small Swift library – you could easily add Windows support already!

 — Alexander Smarus; Product Engineering Lead at Spark Team, Readdle Inc

More details are available on [Readdle’s blog](https://sparkmailapp.com/blog/swift-windows).

Adding support for Windows to Swift is the beginning of a journey. The current support sets the first milestone where the language is usable. There is yet another even broader part of the ecosystem like lldb and the Swift Package Manager which still need more work to be as complete in their support for this different platform.

Getting Started and Getting Involved! 
  
 

The [Getting Started](/getting-started/) section has been updated with new information about using Swift on Windows! For the early adopters who are getting started and finding issues, please report them to the [Swift Bug Tracker](https://bugs.swift.org).

There are many opportunities for those interested in helping push Swift on Windows forward. One of the things that makes Swift easy to use is libraries: publishing new libraries and packages for Swift on Windows or porting existing ones is another way to get involved and help make working with Swift an ever greater delight.

For the ones interested in working on core tooling, there is plenty of work to be done to improve the debugger and to improve the Windows support in the Swift Package Manager. We invite you to check out the [Swift Bug Tracker](https://bugs.swift.org) for the current issues and to send patches to the GitHub repositories. There is also a new section on the Swift forums to [discuss the development of Swift on Windows](https://forums.swift.org/c/development/windows/67). There the community can discuss issues or you can introduce yourself and let others know what area of the tooling you are focusing on. This is the perfect opportunity to become involved in the project and help it grow into a strong, vibrant, cross-platform ecosystem. We cannot wait to see what exciting things you build with Swift!

 
 
 

 
 
 ## Authors

 
 
 
 
 
 
 
 
 
 [Saleem Abdulrasool](https://github.com/compnerd/)
 
 
 
 
 
 
 Saleem Abdulrasool is a member of the Swift Core Team and a Software Engineer at The Browser Company, and previously worked at Google Brain, Facebook, and Microsoft, and currently focuses on cross-platform and embedded Swift.
 
 
 
 
 

 

 

 

 ## Continue Reading

 
 
 
 
 
 
- 
 
 ### Swift 5.3 released!

 September 16, 2020
 
 Swift 5.3 is now officially released! 🎉

 
 [ More ](/blog/swift-5.3-released/)
 
 

 
 
- 
 
 ### Swift System is Now Open Source

 September 25, 2020
 
 In June, Apple introduced Swift System, a new library for Apple platforms that..
 
 [ More ](/blog/swift-system/)
 
 

 
 

 
 
 
 
 

 
 
 
 
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